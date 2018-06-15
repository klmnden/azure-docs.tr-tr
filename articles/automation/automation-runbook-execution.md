---
title: Azure Otomasyonu Runbook yürütme
description: Azure automation'da bir runbook nasıl işleneceğini ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ff58e22f8b9b837ec272cd2cd6193da80a7b718e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195429"
---
# <a name="runbook-execution-in-azure-automation"></a>Azure Otomasyonu Runbook yürütme

Azure Automation'da bir runbook başlattığınızda bir iş oluşturulur. Bir iş bir runbook tek yürütme örneğidir. Bir Azure Otomasyonu çalışan her bir iş çalıştırmak için atanır. Çalışan birden çok Azure paylaştığı olsa da, farklı Automation hesapları işlerden birbirinden yalıtılır. Değil sahip denetim işinizi isteği hangi çalışan hizmetleri. Tek bir runbook'ta aynı anda çalışan birden çok iş bulunabilir. Aynı Otomasyon hesabı işlerden yürütme ortamı yeniden kullanılabilir. Azure portalında listesini görüntülediğinizde, her runbook için başlatılan tüm işlerin durumunu listeler. Her durumunu izlemek için her runbook için iş listesini görüntüleyebilirsiniz. Farklı iş durumları açıklaması [iş durumları](#job-statuses).

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../includes/gdpr-dsr-and-stp-note.md)]

Aşağıdaki diyagramda bir runbook işi için yaşam döngüsü gösterilmektedir [grafik runbook'lar](automation-runbook-types.md#graphical-runbooks) ve [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks).

![İş durumları - PowerShell iş akışı](./media/automation-runbook-execution/job-statuses.png)

Aşağıdaki diyagramda bir runbook işi için yaşam döngüsü gösterilmektedir [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks).

![İş durumları - PowerShell Betiği](./media/automation-runbook-execution/job-statuses-script.png)

İşleriniz Azure kaynaklarınızı Azure aboneliğinize bağlantı yaparak erişimi. Bu kaynakları genel buluttan erişilebilir olması durumunda yalnızca kaynakları veri merkezinizde erişimi.

## <a name="job-statuses"></a>İş durumları

Aşağıdaki tabloda, bir iş için olası farklı durumlarını tanımlar.

| Durum | Açıklama |
|:--- |:--- |
| Tamamlandı |İş başarıyla tamamlandı. |
| Başarısız |İçin [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md), runbook derlenemedi. İçin [PowerShell Betiği runbook'ları](automation-runbook-types.md), runbook başlatılamadı veya iş bir özel durumla karşılaştı. |
| Başarısız oldu, kaynaklar bekleniyor |İşi başarısız oldu, ulaştığından [Orta paylaşımı](#fair-share) sınırlamak üç kez ve her zaman aynı denetim noktasından veya runbook'un başından başlatıldı. |
| Sıraya Alındı |İş başlatılabilir başlatılabilmek için Otomasyon çalışanındaki kaynakları bekliyor. |
| Başlatılıyor |İş bir çalışana atandı ve bunu başlatma işleminde sistemidir. |
| Sürdürülüyor |Askıya alındıktan sonra işi sürdürme işleminde sistemidir. |
| Çalışıyor |İşi çalışıyor. |
| Çalıştıran, kaynaklar bekleniyor |Bunu ulaştığından iş kaldırıldı [Orta paylaşımı](#fair-share) sınırı. Kısa bir süre sonra son denetim noktasından sürdürür. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |İşi durdurma işleminde sistemidir. |
| Askıya Alındı |İş, kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Askıya alınmış bir iş yeniden başlatılabilir ve kontrol noktası yoksa en son denetim noktasından veya runbook'un başından devam. Runbook yalnızca askıya sistem tarafından bir özel durum oluştuğunda. Varsayılan olarak, ErrorActionPreference ayarlamak **devam**, yani bir hatayla işi tutar. Bu tercih değişkeni ayarlanmışsa **durdurmak**, sonra da bir hata işini askıya alır. Uygulandığı öğe [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |
| Askıya alınıyor |Sistem, kullanıcının isteği üzerine işi askıya almaya çalışıyor. Runbook, askıya alınmadan önce sonraki denetim noktasına erişmelidir. Askıya alınmadan önce tamamlandıktan sonra zaten en son denetim aktarılırsa. Uygulandığı öğe [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Azure portalından işi durumunu görüntüleme

Tüm runbook işleri özetlenen durumunu görüntüleyin ya da belirli bir runbook işi Azure portalında veya runbook iş durumu ve iş akışları iletmek için günlük analizi çalışma alanınız ile tümleştirme yapılandırarak ayrıntılarını incelemek. Günlük analizi ile tümleştirme hakkında daha fazla bilgi için bkz: [Otomasyon iş durumu ve iş akışları için günlük analizi iletmek](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Otomasyon runbook işleri özeti

Seçili Otomasyon hesabınızı sağ tarafta, tüm seçili Otomasyon hesabında runbook işlerini özetini görebilirsiniz **Proje istatistikleri** döşeme.

![Proje istatistikleri döşeme](./media/automation-runbook-execution/automation-account-job-status-summary.png):

Bu kutucuğun count ve iş durumu yürütülen tüm işler için grafik gösterimi görüntüler.

Kutucuğa tıklandığında sunar **işleri** yürütülen tüm işler, durum, iş yürütme ve başlangıç ve tamamlanma sürelerini özetlenen bir listesini içeren dikey penceresi.

![Otomasyon hesabı işler dikey](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Seçerek işlerin listesini filtreleyebilirsiniz **filtre işleri** ve filtre iş durumu, belirli bir runbook'u veya aşağı açılan listeden, içinde arama yapmak için tarih aralığı.

![Filtre iş durumu](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, belirli bir runbook için iş Özet ayrıntıları bu runbook'tan seçerek görüntüleyebileceğiniz **Runbook'lar** dikey penceresinde, Automation hesabı ve ardından **işleri** döşeme. Bu **işleri** dikey penceresinde ve buradan ayrıntılarını ve çıktısını görüntülemek için iş kaydı tıklatabilirsiniz.

![Otomasyon hesabı işler dikey](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>İş Özeti

Belirli bir runbook ve en son durumlarını için oluşturulan işleri tümünün listesini görüntüleyebilirsiniz. Bu listeyi işe ve işe son değişikliğin tarih aralığına göre filtreleyebilirsiniz. Ayrıntılı bilgi ve çıktısını görüntülemek için bir işin adına tıklayın. İşin ayrıntılı görünümü bu iş için sağlanan runbook parametrelerinin değerlerini içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanın.

1. Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabının adını seçin.
2. Hub'ından seçin **Runbook'lar** ve daha sonra **Runbook'lar** dikey penceresinde, listeden bir runbook seçin.
3. Seçilen runbook için dikey penceresinde **işleri** döşeme.
4. İşlerini listesinde birini tıklatın ve runbook iş ayrıntıları dikey penceresinde ayrıntılarını ve çıktısını görüntüleyebilirsiniz.

## <a name="retrieving-job-status-using-windows-powershell"></a>Windows PowerShell kullanarak iş durumunu alma

Kullanabileceğiniz [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) bir runbook ve belirli bir işin ayrıntılarını için oluşturulan işleri alınamadı. Windows PowerShell ile bir runbook başlatırsanız [başlangıç AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), sonuçtaki işi verir. Kullanım [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)animasyonun bir işi almak için çıktı çıktı.

Aşağıdaki örnek komutlar bir örnek runbook için en son iş alıp runbook parametreleri ve iş çıktısını için sağlanan değerler durumunu görüntüleyin.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

Aşağıdaki örnek, belirli bir iş için çıktıyı alır ve her bir kayıt döndürür. Durumda kayıtları biri için bir özel durum oluştu özel durum değeri yerine yazılır. Özel durumlar çıkış sırasında normal şekilde oturum açmadığı ek bilgiler sağlayabilir gibi bu yararlı olur.

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

## <a name="get-details-from-activity-log"></a>Etkinlik günlüğü'nden Al Ayrıntıları

Kişi veya runbook'u başlatan hesabı gibi diğer ayrıntıları Otomasyon hesabının etkinlik günlüğü penceresinden alınabilir. Aşağıdaki PowerShell örneğinde, son kullanıcının söz konusu runbook'a sağlar:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>Dengeli dağıtım

Üç saat boyunca çalıştıktan sonra kaynakları bulutta tüm runbook'lar arasında paylaşmak için Azure Automation geçici olarak herhangi bir işi kaldıracak. Bu süre boyunca işler [PowerShell tabanlı runbook'ları](automation-runbook-types.md#powershell-runbooks) durdurulur ve değil olması yeniden başlatılır. İş durumu gösterir **durduruldu**. Kontrol noktaları desteklemeyen beri runbook bu tür her zaman en baştan yeniden başlatılır.

[PowerShell iş akışı tabanlı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) kendi sonuncudan sürdürüldü [denetim noktası](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints). Üç saat çalıştırdıktan sonra runbook işi hizmeti ve durumunu tarafından askıya alınacak gösterir **çalıştıran, kaynaklar bekleniyor**. Bir korumalı alan kullanılabilir duruma geldiğinde runbook Otomasyon hizmeti ve en son kontrol modundan tarafından otomatik olarak yeniden başlatılır. Askıya alma/yeniden başlatma için normal PowerShell iş akışı davranış budur. Runbook yeniden çalışma zamanı, üç saatlik aşarsa işlemi, en fazla üç kez yineler. Runbook hala üç saat içinde tamamlanmadı sonra runbook işi başarısız oldu ve iş durumu gösterir üçüncü yeniden başlatma işleminden sonra **kaynaklar bekleniyor başarısız**. Bu durumda, şu özel durumla hatası alırsınız.

*İş sürekli olarak aynı denetim noktasından çıkarıldı çünkü çalışmaya devam edemiyor. Runbook'unuzda durumuna kalıcı olmadan uzun işlemleri gerçekleştirmez emin olun.*

Yeniden kaldırılıyor olmadan sonraki denetim sağlamak mümkün olmayan gibi hizmet tamamlamadan, süresiz olarak çalışan runbook'lar korumak için budur.

Ardından runbook herhangi bir denetim noktası varsa veya iş ilk denetim noktası kaldırıldığında önce ulaşmıştı değil, baştan başlatılır.

Bir runbook oluştururken etkinlikler arasında iki kontrol noktaları çalıştırmak için zaman üç saat aşmadığından emin olmalısınız. Runbook'unuzda bu değil Bu üç saatlik sınırına ulaştığında veya uzun süre sonu emin olmak için denetim noktaları eklemek gerekebilir işlemleri çalıştırma. Örneğin, runbook'unuz bir arat büyük bir SQL veritabanında gerçekleştirebilir. Bu tek bir işlem Orta paylaşımı sınırı içinde tamamlanmazsa, sonra iş kaldırıldı ve baştan yeniden. Bu durumda, bir kerede bir tablo yeniden dizin oluşturmaya gibi birden çok adımlara arat işlemi bölmeniz ve son işlemi sonra işi devam ettirin böylece her işlemden sonra bir denetim noktası ekleme gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Otomasyonu runbook'u başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Otomasyonu runbook başlatma](automation-starting-a-runbook.md)
