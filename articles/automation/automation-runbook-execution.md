---
title: Azure automation'da Runbook yürütme
description: Azure automation'da bir runbook nasıl işleneceğini ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3dfe16cc09f0453aef8adf8bf87a00aebd2054bc
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214644"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure automation'da Runbook yürütme

Azure Automation'da bir runbook başlattığınızda bir iş oluşturulur. Bir iş, bir runbook'un tek bir yürütme örneğidir. Her bir iş çalıştırmak için bir Azure Otomasyonu çalışanı atanır. Çalışanları tarafından birden çok Azure hesabında paylaşılır, ancak başka Otomasyon hesaplarından işleri birbirinden yalıtılmış durumdadır. Değil sahip denetim işiniz için isteği hangi çalışan hizmetleri. Tek bir runbook, aynı anda çalışan birden çok iş bulunabilir. Yürütme Ortamı aynı Otomasyon hesabına ait işler için yeniden kullanılabilir. Runbook'ların listesini Azure portalında görüntülediğinizde, her runbook için başlatılan tüm işlerin durumunu listeler. Her durumunu izlemek için her runbook için iş listesini görüntüleyebilirsiniz. Farklı iş durumları açıklamasını [iş durumları](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

Bir runbook işi için yaşam döngüsü Aşağıdaki diyagramda gösterilmiştir [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) ve [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks).

![İş durumları - PowerShell iş akışı](./media/automation-runbook-execution/job-statuses.png)

Bir runbook işi için yaşam döngüsü Aşağıdaki diyagramda gösterilmiştir [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks).

![İş durumları - PowerShell Betiği](./media/automation-runbook-execution/job-statuses-script.png)

Azure aboneliğinizde bir bağlantı yaparak Azure kaynaklarınıza erişimi işlerinizi var. Bu kaynaklara genel buluttan erişilemeyen ise yalnızca kaynaklarına erişimi veri Merkezinize sahiptirler.

## <a name="job-statuses"></a>İş durumları

Aşağıdaki tabloda, bir iş için olası farklı durumlarını tanımlar.

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |İçin [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md), runbook derlenemedi. İçin [PowerShell Betiği runbook'ları](automation-runbook-types.md), runbook başlatılamadı ya da iş bir özel durumla karşılaştı. |
| Kaynaklar Bekleniyor başarısız oldu |İş başarısız oldu, ulaştığınız için [adil paylaşımı](#fair-share) sınırlamak üç kez ve her zaman aynı kontrol noktasından veya runbook'un başından başlatıldı. |
| Sıraya Alındı |İşi yeniden başlatılmadan, başlatılabilmek için Otomasyon çalışanındaki kaynakları bekliyor. |
| Başlatılıyor |İş bir çalışana atandı ve sistem bunu başlatma işleminde olur. |
| Sürdürülüyor |Askıya alındıktan sonra işi sürdürme işleminde sistemidir. |
| Çalışıyor |İş çalışıyor. |
| Çalışan, kaynaklar bekleniyor |Bunu ulaştığınız için iş kaldırıldı [adil paylaşımı](#fair-share) sınırı. Bu kısa bir süre sonra son denetim noktasından sürdürülür. |
| Durduruldu |Tamamlanmadan iş kullanıcı tarafından durduruldu. |
| Durduruluyor |İşi durdurma işleminde sistemidir. |
| Askıya Alındı |İş, kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Askıya alınmış bir iş yeniden başlatılabilir ve kontrol noktası yoksa en son denetim noktasından veya runbook'un başından devam ettirebilirsiniz. Runbook yalnızca askıya sistem tarafından bir özel durum oluştuğunda. Varsayılan olarak, ErrorActionPreference kümesine **devam**, yani iş hata üzerinde çalışmaya devam eder. Bu tercih değişkeni ayarlanmışsa **Durdur**, sonra da bir hatada işini askıya alır. Uygulandığı [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |
| Askıya alınıyor |Sistem, kullanıcının isteği üzerine işi askıya almak çalışıyor. Runbook, askıya alınmadan önce sonraki denetim noktasına erişmelidir. Son denetim noktasını zaten geçmiş, askıya alınmadan önce tamamlar. Uygulandığı [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Azure portalında iş durumunu görüntüleme

Azure portalında veya runbook iş durumunu ve iş akışları iletmek için Log Analytics çalışma alanınız ile entegrasyonu yapılandırma ile belirli bir runbook işinin ayrıntılarını incelemek ya da özetlenen tüm runbook işlerinin durumunu görüntüleyin. Log Analytics ile tümleştirme hakkında daha fazla bilgi için bkz. [Otomasyon iş durumunu ve iş akışları Log Analytics'e iletme](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Otomasyon runbook işleri özeti

Seçili Otomasyon hesabınızın sağ tarafta, tüm seçili bir Otomasyon hesabı altında için runbook işleri özetini görebilirsiniz **iş istatistikleri** Döşe.

![İş istatistikleri kutucuğu](./media/automation-runbook-execution/automation-account-job-status-summary.png).

Bu kutucuk, sayı ve grafik temsilini yürütülen tüm işler için iş durumunu görüntüler.

Kutucuğa tıklandığında sunan **işleri** yürütülen tüm işler, durumu, iş yürütme ve başlangıç ve tamamlanma zamanlarını bir Özet listesini içeren dikey pencere.

![Otomasyon hesabı işleri dikey penceresi](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

İşlerin listesini seçerek filtreleyebilirsiniz **filtre işleri** ve iş durumu, belirli bir runbook'u veya aşağı açılan listeden, içinde arama yapmak için tarih aralığı Filtresi.

![İş durumu Filtrele](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, belirli bir runbook için iş Özet ayrıntıları bu runbook'tan seçerek görüntüleyebileceğiniz **runbook'ları** dikey penceresinde, Otomasyon hesabı ve ardından **işleri** Döşe. Bu **işleri** dikey penceresinde ve buradan ayrıntılarını ve çıktısını görüntülemek için iş kaydı tıklayabilirsiniz.

![Otomasyon hesabı işleri dikey penceresi](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>İş Özeti

Belirli bir runbook ve en son durumlarını için oluşturulmuş işlerin bir listesini görüntüleyebilirsiniz. Bu listeyi işe ve işe son değişikliğin tarih aralığını filtreleyebilirsiniz. Çıkış ve ayrıntılı bilgileri görüntülemek için bir işin adına tıklayın. İşin ayrıntılı görünümünü bu iş için sağlanan runbook parametrelerinin değerlerini içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanabilirsiniz.

1. Azure portalında **Otomasyon** ve ardından bir Otomasyon hesabının adını seçin.
2. Hub'ından seçin **runbook'ları** ve ardından **runbook'ları** dikey penceresinde, listeden bir runbook seçin.
3. Seçili runbook dikey penceresinde tıklayın **işleri** Döşe.
4. Listedeki işlerden biri tıklatın ve runbook işi ayrıntıları dikey penceresinde ayrıntılarını ve çıktısını görüntüleyebilirsiniz.

## <a name="retrieving-job-status-using-windows-powershell"></a>Windows PowerShell kullanarak iş durumunu alma

Kullanabileceğiniz [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) işi oluşturulan bir runbook'u ve belirli bir işin ayrıntılarını alabilirsiniz. Windows PowerShell ile bir runbook başlatırsanız [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), sonuçtaki işi verir. Kullanım [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)bir işi almak için çıkış çıkış.

Aşağıdaki örnek komutlar örnek bir runbook'un son işini alıp, runbook parametreleri ve iş çıktısını için sağlanan değerler durumunu görüntüleyin.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir işin çıktı alır ve her bir kayıt döndürür. Case kayıtlardan biri için bir özel durum oluştu özel durum değeri yerine yazılır. Özel durumlar, çıkış sırasında normal şekilde oturum açmamış ek bilgi sağlayabilecek şekilde, bu yararlıdır.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Etkinlik günlüğü'nden ayrıntılarını Al

Kişi veya runbook'u başlatan hesabı gibi diğer ayrıntıları Otomasyon hesabı için etkinlik günlüğü alınabilir. Aşağıdaki PowerShell örneği, söz konusu runbook çalıştırmak için son kullanıcı sağlar:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>Orta paylaşımı

Üç saat çalıştırıldıktan sonra buluttaki tüm runbook'lar arasında kaynaklarını paylaşmak için Azure Otomasyonu geçici olarak herhangi bir işi boşaltacak. Bu süre boyunca işler [PowerShell tabanlı runbook'ları](automation-runbook-types.md#powershell-runbooks) durdurulur ve değil olması yeniden başlatılır. İş durumu gösterir **durduruldu**. Kontrol noktaları desteği olmadığından bu tür bir runbook her zaman en baştan başlatılır.

[PowerShell iş akışı tabanlı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) kendi son sürdürüldüğünden [denetim noktası](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints). Üç saat çalıştırdıktan sonra runbook işi hizmet ve durumunu tarafından askıya alındı gösterir **çalıştırma, kaynaklar bekleniyor**. Bir korumalı alan kullanılabilir duruma geldiğinde, runbook Otomasyon hizmeti ve son denetim noktasından modundan tarafından otomatik olarak yeniden başlatılır. Normal PowerShell iş akışı askıya Al/yeniden başlatma davranışı budur. Runbook, yeniden çalışma zamanı üç saat aşarsa, işlem, en fazla üç kez yineler. Runbook hala üç saat içinde tamamlanmadı sonra runbook işi başarısız oldu ve iş durumu gösterir, üçüncü yeniden başlatma işleminden sonra **kaynaklar bekleniyor başarısız oldu,**. Bu durumda, şu özel durumla hata alırsınız.

*İş, tekrar tekrar aynı denetim noktasından çıkarıldığından çalıştırmaya devam edemiyor. Runbook'unuzda uzun işlemlerin durumunu kalıcı hale getirilmesine gerçekleştirmez emin olun.*

Bunlar yeniden bulgudaki olmadan sonraki kontrol noktasına yapmak mümkün olmadığından hizmet tamamlamadan, süresiz olarak çalışan runbook'ları korumak için budur.

Ardından runbook hiç kontrol noktası varsa veya iş ilk kontrol noktası kaldırılıyor önce tamamladı değil baştan başlatılır.

Uzun çalıştırmak için görevler önerilir kullanmak için bir [karma Runbook çalışanı](automation-hrw-run-runbooks.md#job-behavior). Karma Runbook çalışanları tarafından adil paylaşımı sınırlı değildir ve bir sınırlama yoktur üzerinde ne kadar bir runbook çalıştırabilirsiniz.

Azure üzerinde PowerShell iş akışı runbook kullanıyorsanız, bir runbook oluştururken iki denetim noktaları arasından herhangi bir etkinlik çalışma süresi üç saat aşmadığından emin olun. Runbook'unuzda bu üç saatlik sınırına ulaşmadığınız veya desteklemez uzun süre sonu emin emin olmak için kontrol noktaları eklemek gerekebilir süreli işlemler. Örneğin, runbook'unuz bir reindex büyük bir SQL veritabanı'nda gerçekleştirebilir. Bu tek işlem adil paylaşım sınırı içinde tamamlanmazsa, ardından iş kaldırıldı ve baştan yeniden. Bu durumda bir kerede bir tablo ölçeklemek gibi birden çok adımlarını reindex işlemi bölmeniz ve işi tamamlamak için son işlemi sonra devam edilemiyor, böylece her işlemden sonra bir denetim noktası ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Automation'da bir runbook başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md)
