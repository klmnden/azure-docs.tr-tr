---
title: Azure Otomasyonu iş verilerini Log Analytics’e iletme
description: Bu makalede, iş durumunu ve runbook ek bilgiler ve yönetim sağlamak üzere Azure günlük analizi için iş akışlarını göndermek gösterilmiştir.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 562b1f1371133a1da8d24ebbb9c588f0597dda7f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34194409"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics"></a>İş durumu ve iş akışları için günlük analizi Otomasyon iletme
Otomasyon runbook iş durumu ve iş akışları için günlük analizi çalışma alanınız gönderebilirsiniz. İş günlüğe kaydeder ve tek tek işler ve bu verir için basit araştırmalar gerçekleştirmek iş akışlarını Azure portalında veya PowerShell ile görünür. Şimdi günlük analizi ile şunları yapabilirsiniz:

* Otomasyon işleriniz hakkında bilgi edinme.
* Bir e-posta veya uyarı (örneğin, başarısız veya askıya alınmış), runbook işi durumlarına dayalı tetikleyici.
* Gelişmiş sorgular, iş akışları yazma.
* İşlerini Automation hesaplarında ilişkilendirmek.
* İş Geçmişi zaman içinde görselleştirin.

## <a name="prerequisites-and-deployment-considerations"></a>Önkoşulları ve dağıtım hakkında önemli noktalar
Günlük analizi için Otomasyon günlüklerinizi göndermeye başla, gerekir:

* Kasım 2016 veya sonraki sürümünün [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
* Günlük analizi çalışma alanı. Daha fazla bilgi için bkz: [günlük Analytics ile çalışmaya başlama](../log-analytics/log-analytics-get-started.md). 
* Azure Automation hesabınız için ResourceId.


Azure Automation hesabınız için ResourceId bulmak için:

```powershell-interactive
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

ResourceId için günlük analizi çalışma alanınız bulmak için aşağıdaki PowerShell çalıştırın:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Birden fazla Automation hesapları varsa veya çalışma alanları, önceki komutların çıktısı Bul *adı* değerini kopyalayın ve yapılandırmanız gerekir *ResourceId*.

Bulmanız gerekiyorsa *adı* Otomasyon hesabınızı Azure portalında penceresinden Otomasyon hesabınızı seçin. **Otomasyon hesabı** dikey penceresinde ve select **tüm ayarları** . **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br> ![Otomasyon hesabı özellikleri](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Günlük analizi ile tümleştirmeyi ayarlamak

1. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran.
2. Aşağıdaki PowerShell çalıştırın ve değeri Düzenle `[your resource id]` ve `[resource id of the log analytics workspace]` önceki adımı değerlerle.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Bu komut dosyasını çalıştırdıktan sonra 10 dakika içinde yeni JobLogs veya JobStreams yazılan günlük analizi kayıtlarında görürsünüz.

Günlükleri görmek için günlük analizi günlük aramada aşağıdaki sorguyu çalıştırın: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Automation hesabınız için günlük analizi çalışma alanınız günlükleri göndermek onaylamak için tanılama doğru Otomasyon hesabında aşağıdaki PowerShell kullanılarak yapılandırılıp yapılandırılmadığını denetleyin:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda emin olun:
+ Altında *günlükleri*, değeri *etkin* olan *doğru*.
+ Değeri *Workspaceıd* günlük analizi çalışma alanınız ResourceId için ayarlanır.

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Azure Otomasyonu tanılama günlük analizi kayıtları iki tür oluşturur ve olarak etiketlenir **AzureDiagnostics**. Aşağıdaki sorguları için günlük analizi yükseltilmiş sorgu dilini kullanır. Eski sorgu dili ve yeni Azure günlük analizi sorgu dili arasındaki ortak sorguları hakkında bilgi için ziyaret [yeni Azure günlük analizi sorgu dili kopya sayfası eski](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>İş günlükleri
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| ResultType |Runbook işinin durumudur. Olası değerler şunlardır:<br>-Yeni<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>-Tamamlandı |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için iş bir değerdir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliğini belirtir. |
| Abonelik kimliği | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| Kaynak grubu | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>İş akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Arayanlar için Kiracı tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| ResultType |Runbook işinin durumudur. Olası değerler şunlardır:<br>-Devam eden |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için iş bir değerdir. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Günlük analizi nasıl veriler toplanır. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook’un çıktı akışını içerir. |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliğini belirtir. |
| Abonelik kimliği | Otomasyon hesabının Azure abonelik kimliği (GUID). |
| Kaynak grubu | Otomasyon hesabının kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Günlük analizi günlüklerini Otomasyon görüntüleme
Günlük analizi için Otomasyon iş günlüklerinizi gönderilmeye başlandı, günlük analizi içinde bu günlükleri ile neler yapabileceğinizi görelim.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Bir runbook işi başarısız olduğunda veya askıya alındığında bir e-posta Gönder
Üst müşteri birini soran bir e-posta veya bir metin bir şeyler ile bir runbook işi yanlış gittiğinde gönderme olanağı içindir.   

Bir uyarı kuralı oluşturmak için uyarı çağıracağı runbook iş kaydı için bir günlük arama oluşturarak başlayın. Tıklatın **uyarı** oluşturmak ve uyarı kuralı yapılandırmak için düğmesi.

1. Günlük analizi genel bakış sayfasında **günlük arama**.
2. Sorgu alanına aşağıdaki arama yazarak, uyarı için bir günlük arama sorgusu oluşturun: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` kullanarak RunbookName göre de gruplandırabilirsiniz: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Günlüklerini birden fazla Otomasyon hesabı veya abonelik alanınıza ayarlarsanız, uyarılarınızı aboneliği ve Automation hesabı göre gruplandırabilirsiniz. Otomasyon hesabı adı JobLogs arama kaynak alanında bulunabilir.
1. Açmak için **uyarı kuralı Ekle** ekranında **uyarı** sayfanın üst kısmındaki. Uyarı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz: [günlük analizi uyarılarını](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlandı tüm işleri bulun
Hatalarında uyarı ek olarak, bir runbook işi Sonlandırıcı olmayan bir hata olduğunda bulabilirsiniz. Bu durumlarda, bir hata akışı PowerShell üretir ancak Sonlandırıcı olmayan hataları, işini askıya alma veya başarısız şekilde neden yoktur.    

1. Günlük analizi çalışma alanınızda tıklatın **günlük arama**.
2. Sorgu alanına yazın `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` ve ardından **arama** düğmesi.

### <a name="view-job-streams-for-a-job"></a>Bir iş için Görünüm iş akışları
Bir işi hatalarını ayıkladığınız, iş akışları aramak isteyebilirsiniz. Aşağıdaki sorgu GUID 2ebd22ea-e05e-4eb9 - 9 d 76-d73cbd4356e0 tek bir işin tüm akışlarını gösterir:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş işi durumunu görüntüleme
Son olarak, zaman içinde iş geçmişi görselleştirmek isteyebilirsiniz. Zaman içinde işlerin durumunu aramak için bu sorguyu kullanın.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Günlük analizi geçmiş iş durumu grafiği](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Özet
Günlük analizi için Otomasyon iş durumu ve akış veri göndererek Otomasyon işlerinizin tarafından durumunu daha iyi bir anlayış alabilirsiniz:
+ Bir sorun olduğunda sizi bilgilendirmek üzere uyarılarını ayarlama.
+ Runbook sonuçlarınızı görselleştirmek için özel görünümleri ve arama sorguları kullanarak, runbook iş durumu ve diğer anahtar göstergeleri veya ölçümleri ilgili.  

Günlük analizi Otomasyon işleriniz için daha fazla işlem görünürlük sağlar ve adres olaylar daha hızlı yardımcı olabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* Farklı arama sorguları oluşturmak ve günlük analizi ile Otomasyon iş günlükleri gözden geçirmek hakkında daha fazla bilgi için bkz: [günlük analizi aramaları oturum](../log-analytics/log-analytics-log-searches.md).
* Oluşturmak ve runbook'lardan çıkış ve hata iletileri almak nasıl anlamak için bkz: [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md).
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
* Günlük analizi ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [toplama Azure storage veri günlük analizi genel bakış](../log-analytics/log-analytics-azure-storage.md).
