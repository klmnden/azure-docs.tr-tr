---
title: Azure Otomasyonu iş verilerini Log Analytics’e iletme
description: Bu makalede, iş durumunu ve runbook iş akışları hakkındaki ek bilgiler ve yönetim sağlamak üzere Azure Log Analytics'e göndermek nasıl gösterir.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6170b69d213470b1f5b7e75c9b102e5e07c09209
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682903"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics"></a>İş durumunu ve iş akışları Automation'ı Log Analytics'e iletme

Log Analytics çalışma alanınıza Automation runbook iş durumunu ve iş akışları yeniden gönderebilirsiniz. Bu işlem, çalışma alanı bağlama içermeyen ve tamamen bağımsızdır. İş günlükleri ve iş akışları tek tek işler ve bu sağlayan için basit araştırmalar gerçekleştirmek Azure portalında veya PowerShell ile görünür. Artık Log Analytics ile şunları yapabilirsiniz:

* Otomasyon işlerinizi ilgili Öngörüler edinin.
* Bir e-posta veya uyarı (örneğin, başarısız olan veya askıya alınmış), runbook iş durumu temelinde tetikleyicisi.
* Gelişmiş sorgular, iş akışları arasında yazın.
* İşleri, Otomasyon hesabında ilişkilendirin.
* İş geçmişinizi zaman içinde görselleştirin.

## <a name="prerequisites-and-deployment-considerations"></a>Önkoşulları ve dağıtım konuları

Otomasyon günlüklerinizi Log Analytics'e gönderme başlamak için ihtiyacınız vardır:

* Kasım 2016 veya sonraki sürümünün [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
* Log Analytics çalışma alanı. Daha fazla bilgi için [Log Analytics ile çalışmaya başlama](../log-analytics/log-analytics-get-started.md). 
* Azure Automation hesabınız için ResourceId.

Azure Automation hesabınız için ResourceId bulmak için:

```powershell-interactive
# Find the ResourceId for the Automation Account
Get-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Log Analytics çalışma alanınız için ResourceId bulmak için aşağıdaki PowerShell komutunu çalıştırın:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Birden fazla Otomasyon hesapları varsa veya çalışma alanlarını önceki komut çıkışında bulun *adı* değerini kopyalayın ve yapılandırmak için ihtiyaç duyduğunuz *ResourceId*.

Bulmanız gerekiyorsa *adı* Otomasyon hesabınızda Azure portalında Otomasyon hesabınızı seçin. **Otomasyon hesabı** dikey penceresinde ve select **tüm ayarlar** . **Tüm ayarlar** dikey penceresindeki **Hesap Ayarları** altında **Özellikler**’i seçin.  **Özellikler** dikey penceresinde bu değerleri fark edebilirsiniz.<br> ![Otomasyon hesabı özellikleri](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Log Analytics ile tümleştirmesini ayarlama

1. Bilgisayarınızda Başlat **Windows PowerShell** gelen **Başlat** ekran.
2. Aşağıdaki PowerShell komutunu çalıştırın ve değerini Düzenle `[your resource id]` ve `[resource id of the log analytics workspace]` önceki adımdan gelen değerlerle.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Bu betiği çalıştırdıktan sonra Log Analytics kayıtları yeni JobLogs veya JobStreams yazılmakta olan 10 dakika içinde görürsünüz.

Günlükleri görmek için Log Analytics günlük araması ' aşağıdaki sorguyu çalıştırın: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Yapılandırmayı doğrulama
Otomasyon hesabınızı, Log Analytics çalışma alanına günlükleri gönderme onaylamak için tanılama doğru Otomasyon hesabında aşağıdaki PowerShell kullanılarak yapılandırılıp yapılandırılmadığını denetleyin:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Çıktıda emin olun:
+ Altında *günlükleri*, değeri *etkin* olduğu *True*.
+ Değerini *Workspaceıd* Log Analytics çalışma alanınızın ResourceId için ayarlanır.

## <a name="log-analytics-records"></a>Log Analytics kayıtları

Azure Otomasyonu tanılamadan Log Analytics'te iki tür kayıt oluşturur ve olarak etiketlenir **AzureDiagnostics**. Aşağıdaki sorgularda Log analytics'e yükseltilen sorgu dili kullanın. Eski sorgu dili ve yeni Azure Log Analytics sorgu diline arasında sık kullanılan sorguları hakkında bilgi için ziyaret [yeni Azure Log Analytics sorgu dilini hızlı başvuru sayfası için eski](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>İş günlükleri
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| Tenant_g | Kiracı için çağıranın tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| resulttype'ı |Runbook işinin durumudur. Olası değerler şunlardır:<br>-Yeni<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>-Tamamlandı |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Log Analytics toplanan verileri nasıl. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliği belirtir. |
| SubscriptionId | Otomasyon hesabı için Azure aboneliği kimliğini (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>İş akışları
| Özellik | Açıklama |
| --- | --- |
| TimeGenerated |Runbook işinin yürütüldüğü tarih ve saat. |
| RunbookName_s |Runbook’un adı. |
| Caller_s |İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir. |
| StreamType_s |İş akışı türü. Olası değerler şunlardır:<br>- İlerleme durumu<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı |
| Tenant_g | Kiracı için çağıranın tanımlayan GUID. |
| JobId_g |Runbook işinin Kimliği olan GUID. |
| resulttype'ı |Runbook işinin durumudur. Olası değerler şunlardır:<br>İlerleme- |
| Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır. |
| OperationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer iş olacaktır. |
| Kaynak | Otomasyon hesabının adı |
| SourceSystem | Log Analytics toplanan verileri nasıl. Her zaman *Azure* Azure tanılama için. |
| ResultDescription |Runbook’un çıktı akışını içerir. |
| CorrelationId |Runbook işinin Bağıntı Kimliği olan GUID. |
| ResourceId |Runbook'un Azure Otomasyon hesabı kaynak kimliği belirtir. |
| SubscriptionId | Otomasyon hesabı için Azure aboneliği kimliğini (GUID). |
| ResourceGroup | Otomasyon hesabı için kaynak grubunun adı. |
| ResourceProvider | MICROSOFT. OTOMASYON |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Otomasyon görüntüleme Log Analytics'te günlüğe kaydeder
Şimdi, Otomasyon iş günlüklerini Log Analytics'e gönderme başlatıldı, bu günlükleri Log Analytics içinde ile neler yapabileceğinize göz atın.

Günlükleri görmek için aşağıdaki sorguyu çalıştırın: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Bir runbook işi başarısız olursa veya askıya alır, bir e-posta Gönder
Sık karşılaşılan müşteri birini ister bir şeyler ile bir runbook işi yanlış gittiğinde e-posta veya metin gönderme olanağı içindir.   

Bir uyarı kuralı oluşturmak için bir günlük araması uyarı çağırması gereken runbook işi kayıtlar için oluşturarak başlayın. Tıklayın **uyarı** düğmesine oluşturmak ve uyarı kuralını yapılandırın.

1. Log Analytics'e genel bakış sayfasında **günlük araması**.
2. Aşağıdaki arama sorgu alanına yazarak günlük arama sorgusu için uyarı oluştur: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` kullanarak RunbookName göre de gruplandırabilirsiniz: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Günlüklerini birden fazla Otomasyon hesabını veya aboneliğini çalışma alanınıza ayarlarsanız, uyarılarınızı aboneliği ve Automation hesabı göre gruplandırabilirsiniz. Otomasyon hesabı adı, kaynak alanına JobLogs arama bulunabilir.
1. Açmak için **oluşturma kuralı** ekranında **+ yeni uyarı kuralı** sayfanın üstünde. Uyarı yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [günlük uyarıları Azure'da](../monitoring-and-diagnostics/monitor-alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Hatalarla tamamlanan tüm işleri bulun
Hatalarında uyarı ek olarak, bir runbook işi Sonlandırıcı olmayan bir hata olduğunda bulabilirsiniz. Bu gibi durumlarda, bir hata akışı PowerShell üretir, ancak Sonlandırıcı olmayan hatalara, işi askıya alma veya başarısız olmasına neden olmaz.    

1. Log Analytics çalışma alanınızda tıklayın **günlük araması**.
2. Sorgu alanına `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` ve ardından **arama** düğmesi.

### <a name="view-job-streams-for-a-job"></a>Bir iş için iş akışları görüntüleme
Bir işi hata ayıklama işlemi yaparken, iş akışları bak isteyebilirsiniz. Aşağıdaki sorgu tüm GUID 2ebd22ea-e05e-4eb9 - 9 d 76-d73cbd4356e0 ile tek bir iş akışlarını gösterir:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Geçmiş iş durumunu görüntüleme
Son olarak, zaman içinde iş geçmişini görselleştirmek isteyebilirsiniz. Bu sorgu, zaman içinde işinizin durumunu aramak için kullanabilirsiniz.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Log Analytics geçmiş iş durumu grafiği](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Tanılama ayarları Kaldır

Otomasyon hesabından tanılama ayarını kaldırmak için aşağıdaki komutları çalıştırın:

```powershell-interactive
$automationAccountId = "[resource id of your automation account]"

Remove-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="summary"></a>Özet

Otomasyon iş durumu ve akış verileri Log Analytics'e göndererek Otomasyon işlerinizi durumunu daha iyi bir anlayış alabilirsiniz:
+ Bir sorun olduğunda bildirimde bulunacak uyarılar ayarlanması.
+ Runbook sonuçlarınızı görselleştirmek için özel görünümlerinizi ve arama sorgularını kullanarak runbook iş durumu ve diğer önemli göstergeleri veya ölçümleri ilgili.  

Log Analytics, Otomasyon işleri için daha fazla operasyonel görünürlük sağlar ve olaylara daha hızlı yardımcı olabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* Farklı arama sorguları oluşturma ve Log Analytics ile Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi için bkz. [Log Analytics'te günlük aramaları](../log-analytics/log-analytics-log-searches.md).
* Oluşturma ve runbook'lardan çıktı ve hata iletileri alma anlamak için bkz: [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md).
* Runbook yürütme, runbook işlerini izleme ve diğer teknik ayrıntılar hakkında daha fazla bilgi edinmek için bkz. [Runbook işi izleme](automation-runbook-execution.md).
* Log Analytics ve veri toplama kaynakları hakkında daha fazla bilgi için bkz: [Azure depolama verileri toplamaya Log Analytics'e genel bakış](../azure-monitor/platform/collect-azure-metrics-logs.md).
