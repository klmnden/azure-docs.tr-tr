---
title: "OMS günlük analizi için Azure Otomasyonu işi veri iletmek | Microsoft Docs"
description: "Bu makalede iş durumu ve runbook iş akışları için ek bilgiler sunmak için Microsoft Operations Management Suite günlük analizi ve Yönetimi nasıl gönderileceğini gösterir."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: b3b9457e6c8ce501a7295859923838460e7ab6cc
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>İş durumu ve iş akışları Otomasyon günlük analizi (OMS) iletme
Otomasyon runbook iş durumu ve iş akışları için Microsoft Operations Management Suite (OMS) günlük analizi çalışma alanınız gönderebilirsiniz.  İş günlüğe kaydeder ve tek tek işler ve bu verir için basit araştırmalar gerçekleştirmek iş akışlarını Azure portalında veya PowerShell ile görünür. Şimdi günlük analizi ile şunları yapabilirsiniz:

* Otomasyon işleriniz hakkında bilgi edinme
* Bir e-posta veya uyarı (örneğin, başarısız veya askıya alınmış), runbook işi durumlarına dayalı tetikleyici
* Gelişmiş sorgular, iş akışları yazma
* İşlerini Automation hesaplarında ilişkilendirmek
* İş Geçmişi zaman içinde görselleştirin     

## <a name="prerequisites-and-deployment-considerations"></a>Önkoşulları ve dağıtım hakkında önemli noktalar
Günlük analizi için Otomasyon günlüklerinizi göndermeye başla, gerekir:

1. Kasım 2016 veya sonraki sürümünün [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
2. Günlük analizi çalışma alanı. Daha fazla bilgi için bkz: [günlük Analytics ile çalışmaya başlama](../log-analytics/log-analytics-get-started.md). 
3. Azure Automation hesabınız için ResourceId

Azure Otomasyonu hesabı ve günlük analizi çalışma alanı için ResourceId bulmak için aşağıdaki PowerShell çalıştırın:

```powershell
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"

# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Birden çok Automation hesapları varsa veya çalışma alanları, önceki komutların çıktısı Bul *adı* değerini kopyalayın ve yapılandırmanız gerekir *ResourceId*.

Bulmanız gerekiyorsa *adı* Otomasyon hesabınızı Azure portalında penceresinden Otomasyon hesabınızı seçin. **Otomasyon hesabı** dikey penceresinde ve select **tüm ayarları** .  **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br> ![Otomasyon hesabı özellikleri](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Günlük analizi ile tümleştirmeyi ayarlamak
1. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran.  
2. Kopyalama ve aşağıdaki PowerShell yapıştırın ve değeri Düzenle `$workspaceId` ve `$automationAccountId`.  İçin `-Environment` parametre, geçerli değerler *AzureCloud* veya *AzureUSGovernment* içinde çalıştığınız bulut ortamınıza bağlı olarak.     

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }

# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true

```

Bu komut dosyasını çalıştırdıktan sonra 10 dakika içinde yeni JobLogs veya JobStreams yazılan günlük analizi kayıtlarında görürsünüz.

Günlükleri görmek için günlük analizi günlük aramada aşağıdaki sorguyu çalıştırın:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Automation hesabınız için günlük analizi çalışma alanınız günlükleri göndermek onaylamak için aşağıdaki PowerShell kullanarak Automation hesabını tanılama doğru ayarlandığından emin denetleyin:

```powershell
[cmdletBinding()]
    Param
    (
        [Parameter(Mandatory=$True)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$Environment="AzureCloud"
    )

#Check to see which cloud environment to sign into.
Switch ($Environment)
   {
       "AzureCloud" {Login-AzureRmAccount}
       "AzureUSGovernment" {Login-AzureRmAccount -EnvironmentName AzureUSGovernment} 
   }
# if you have one Log Analytics workspace you can use the following command to get the resource id of the workspace
$workspaceId = (Get-AzureRmOperationalInsightsWorkspace).ResourceId

$automationAccountId = "/SUBSCRIPTIONS/ec11ca67-1234-421e-5678-c25/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.AUTOMATION/ACCOUNTS/DEMO" 

Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda emin olun:
+ Altında *günlükleri*, değeri *etkin* olan *True*
+ Değeri *Workspaceıd* günlük analizi çalışma alanınız ResourceId için ayarlama


## <a name="log-analytics-records"></a>Log Analytics kayıtları
Azure Otomasyonu tanılama günlük analizi kayıtları iki tür oluşturur ve olarak etiketlenir **türü AzureDiagnostics =**.

### <a name="job-logs"></a>İş günlükleri
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır.  Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| ResultType |Runbook işinin durumudur.  Olası değerler şunlardır:<br>-Yeni<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>-Tamamlandı |
| Kategori | Veri türü sınıflandırması.  Otomasyon için değer JobLogs olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir.  Otomasyon için iş bir değerdir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook iş sonucu durumunu açıklar.  Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliğini belirtir. |
| SubscriptionId | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>İş akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır.  Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| ResultType |Runbook işinin durumudur.  Olası değerler şunlardır:<br>-Devam eden |
| Kategori | Veri türü sınıflandırması.  Otomasyon için değer JobStreams olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir.  Otomasyon için iş bir değerdir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook’un çıktı akışını içerir. |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliğini belirtir. |
| SubscriptionId | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| ResourceGroup | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Günlük analizi günlüklerini Otomasyon görüntüleme
Günlük analizi için Otomasyon iş günlüklerinizi gönderme başlattığınız, günlük analizi içinde bu günlükleri ile neler yapabileceğinizi görelim.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Bir runbook işi başarısız olduğunda veya askıya alındığında bir e-posta Gönder
Üst müşterimiz birini soran bir e-posta veya bir metin bir şeyler ile bir runbook işi yanlış gittiğinde gönderme olanağı içindir.   

Bir uyarı kuralı oluşturmak için uyarı çağıracağı runbook iş kaydı için bir günlük arama oluşturarak başlayın.  Tıklatın **uyarı** oluşturmak ve uyarı kuralı yapılandırmak için düğmesi.

1. Günlük analizi genel bakış sayfasında **günlük arama**.
2. Sorgu alanına aşağıdaki arama yazarak, uyarı için bir günlük arama sorgusu oluşturun: `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended)` kullanarak RunbookName göre de gruplandırabilirsiniz:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs (ResultType=Failed OR ResultType=Suspended) | measure Count() by RunbookName_s`   

   Günlüklerini birden fazla Otomasyon hesabı veya abonelik alanınıza ayarladıysanız, uyarılarınızı aboneliği ve Automation hesabı göre gruplandırabilirsiniz.  Otomasyon hesabı adı JobLogs arama kaynak alanından elde edilebilir.  
3. Açmak için **uyarı kuralı Ekle** ekranında **uyarı** sayfanın üst kısmındaki. Uyarı yapılandırmak için seçenekleri hakkında daha fazla ayrıntı için bkz: [günlük analizi uyarılarını](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlandı tüm işleri bulun
Hatalarında uyarı ek olarak, bir runbook işi Sonlandırıcı olmayan bir hata olduğunda bulabilirsiniz. Bu gibi durumlarda, bir hata akışı PowerShell oluşturur, ancak Sonlandırıcı olmayan hataları, işini askıya alma veya başarısız şekilde neden olmaz.    

1. Günlük analizi çalışma alanınızda tıklatın **günlük arama**.
2. Sorgu alanına yazın `Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams StreamType_s=Error | measure count() by JobId_g` ve ardından **arama**.

### <a name="view-job-streams-for-a-job"></a>Bir iş için Görünüm iş akışları
Bir işi ayıklarken iş akışlara aramak isteyebilirsiniz.  Aşağıdaki sorgu GUID 2ebd22ea-e05e-4eb9 - 9 d 76-d73cbd4356e0 tek bir işin tüm akışlarını gösterir:   

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobStreams JobId_g="2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort TimeGenerated | select ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş işi durumunu görüntüleme
Son olarak, zaman içinde iş geçmişi görselleştirmek isteyebilirsiniz.  Zaman içinde işlerin durumunu aramak için bu sorguyu kullanın.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=JobLogs NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  
<br> ![OMS geçmiş iş durumu grafiği](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Özet
Günlük analizi için Otomasyon iş durumu ve akış veri göndererek Otomasyon işlerinizin tarafından durumunu daha iyi bir anlayış alabilirsiniz:
+ Bir sorun olduğunda bildirimde bulunacak uyarılar ayarlama
+ Runbook sonuçlarınızı görselleştirmek için özel görünümleri ve arama sorguları kullanarak, runbook iş durumu ve diğer anahtar göstergeleri veya ölçümleri ilgili.  

Günlük analizi Otomasyon işleriniz için daha fazla işlem görünürlük sağlar ve adres olaylar daha hızlı yardımcı olabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* Farklı arama sorguları oluşturma ve Log Analytics ile Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te günlük aramaları](../log-analytics/log-analytics-log-searches.md)
* Oluşturmak ve runbook'lardan çıkış ve hata iletileri almak nasıl anlamak için bkz: [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md)
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md)
* OMS Log Analytics ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için bkz. [Log Analytics’te Azure depolama verileri toplamaya genel bakış](../log-analytics/log-analytics-azure-storage.md)
