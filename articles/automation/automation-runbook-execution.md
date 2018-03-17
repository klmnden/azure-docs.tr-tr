---
title: "Azure Otomasyonu Runbook yürütme"
description: "Azure automation'da bir runbook nasıl işleneceğini ayrıntılarını açıklar."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: edfd317e7d3f7595f656c6c24ad65f3d87fea14c
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="runbook-execution-in-azure-automation"></a>Azure Otomasyonu Runbook yürütme
Azure Automation'da bir runbook başlattığınızda bir iş oluşturulur. Bir iş bir runbook tek yürütme örneğidir. Bir Azure Otomasyonu çalışan her bir iş çalıştırmak için atanır. Çalışan birden çok Azure paylaştığı olsa da, farklı Automation hesapları işlerden birbirinden yalıtılır. Değil sahip denetim işinizi isteği hangi çalışan hizmetleri. Tek bir runbook'ta aynı anda çalışan birden çok iş bulunabilir.  Aynı Otomasyon hesabı işlerden yürütme ortamı yeniden kullanılabilir. Azure portalında listesini görüntülediğinizde, her runbook için başlatılan tüm işlerin durumunu listeler. Her durumunu izlemek için her runbook için iş listesini görüntüleyebilirsiniz. Farklı iş durumları açıklaması [iş durumları](#job-statuses).

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
| Başarısız |İçin [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md), runbook derlenemedi.  İçin [PowerShell Betiği runbook'ları](automation-runbook-types.md), runbook başlatılamadı veya iş bir özel durumla karşılaştı. |
| Başarısız oldu, kaynaklar bekleniyor |İşi başarısız oldu, ulaştığından [Orta paylaşımı](#fair-share) sınırlamak üç kez ve her zaman aynı denetim noktasından veya runbook'un başından başlatıldı. |
| Kuyruğa alındı |İş başlatılabilir başlatılabilmek için Otomasyon çalışanındaki kaynakları bekliyor. |
| Başlatılıyor |İş bir çalışana atandı ve bunu başlatma işleminde sistemidir. |
| Sürdürülüyor |Askıya alındıktan sonra işi sürdürme işleminde sistemidir. |
| Çalışıyor |İşi çalışıyor. |
| Çalıştıran, kaynaklar bekleniyor |Bunu ulaştığından iş kaldırıldı [Orta paylaşımı](#fair-share) sınırı. Kısa bir süre sonra son denetim noktasından sürdürür. |
| Durduruldu |İş tamamlanmadan kullanıcı tarafından durduruldu. |
| Durduruluyor |İşi durdurma işleminde sistemidir. |
| Askıya Alındı |İş, kullanıcı, sistem veya runbook'taki bir komut tarafından askıya alındı. Askıya alınmış bir iş yeniden başlatılabilir ve kontrol noktası yoksa en son denetim noktasından veya runbook'un başından devam. Özel durum oluştuğunda runbook yalnızca sistem tarafından askıya alınır. Varsayılan olarak, ErrorActionPreference ayarlamak **devam**, yani bir hatayla işi tutar. Bu tercih değişkeni ayarlanmışsa **durdurmak**, sonra da bir hata işini askıya alır.  Uygulandığı öğe [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |
| Askıya alınıyor |Sistem, kullanıcının isteği üzerine işi askıya almaya çalışıyor. Runbook, askıya alınmadan önce sonraki denetim noktasına erişmelidir. Askıya alınmadan önce tamamlandıktan sonra zaten en son denetim aktarılırsa.  Uygulandığı öğe [grafik ve PowerShell iş akışı runbook'ları](automation-runbook-types.md) yalnızca. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Azure portalından işi durumunu görüntüleme
Tüm runbook işleri özetlenen durumunu görüntüleyin ya da belirli bir runbook işi Azure portalında veya runbook iş durumu ve iş iletmek için Microsoft Operations Management Suite (OMS) günlük analizi çalışma alanınız ile tümleştirme yapılandırarak ayrıntılarını incelemek Akışlar.  OMS günlük analizi ile tümleştirme hakkında daha fazla bilgi için bkz: [Otomasyon iş durumu ve iş akışları günlük analizi (OMS) iletmek](automation-manage-send-joblogs-log-analytics.md).  

### <a name="automation-runbook-jobs-summary"></a>Otomasyon runbook işleri özeti
Seçili Otomasyon hesabınızı sağ tarafta, tüm seçili Otomasyon hesabında runbook işlerini özetini görebilirsiniz **Proje istatistikleri** döşeme.<br><br> ![Proje istatistikleri döşeme](./media/automation-runbook-execution/automation-account-job-status-summary.png).<br> Bu kutucuğun count ve iş durumu yürütülen tüm işler için grafik gösterimi görüntüler.  

Kutucuğa tıklandığında sunar **işleri** yürütülen tüm işler, durum, iş yürütme ve başlangıç ve tamamlanma sürelerini özetlenen bir listesini içeren dikey penceresi.<br><br> ![Otomasyon hesabı işler dikey](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)<br><br>  Seçerek işlerin listesini filtreleyebilirsiniz **filtre işleri** ve filtre iş durumu, belirli bir runbook'u veya aşağı açılan listeden, içinde arama yapmak için tarih aralığı.<br><br> ![Filtre iş durumu](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Alternatif olarak, belirli bir runbook için iş Özet ayrıntıları bu runbook'tan seçerek görüntüleyebileceğiniz **Runbook'lar** dikey penceresinde, Automation hesabı ve ardından **işleri** döşeme.  Bu **işleri** dikey penceresinde ve buradan ayrıntılarını ve çıktısını görüntülemek için iş kaydı tıklatabilirsiniz.<br><br> ![Otomasyon hesabı işler dikey](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)<br> 

### <a name="job-summary"></a>İş Özeti
Belirli bir runbook ve en son durumlarını için oluşturulan işleri tümünün listesini görüntüleyebilirsiniz. Bu listeyi işe ve işe son değişikliğin tarih aralığına göre filtreleyebilirsiniz. Ayrıntılı bilgi ve çıktısını görüntülemek için bir işin adına tıklayın. İşin ayrıntılı görünümü bu iş için sağlanan runbook parametrelerinin değerlerini içerir.

Bir runbook işlerini görüntülemek için aşağıdaki adımları kullanın.

1. Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabının adını seçin.
2. Hub'ından seçin **Runbook'lar** ve daha sonra **Runbook'lar** dikey penceresinde, listeden bir runbook seçin.
3. Seçilen runbook için dikey penceresinde **işleri** döşeme.
4. İşlerini listesinde birini tıklatın ve runbook iş ayrıntıları dikey penceresinde ayrıntılarını ve çıktısını görüntüleyebilirsiniz.

## <a name="retrieving-job-status-using-windows-powershell"></a>Windows PowerShell kullanarak iş durumunu alma
Kullanabileceğiniz [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) bir runbook ve belirli bir işin ayrıntılarını için oluşturulan işleri alınamadı. Windows PowerShell ile bir runbook başlatırsanız [başlangıç AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), sonuçtaki işi verir. Kullanım [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)animasyonun bir işi almak için çıktı çıktı.

Aşağıdaki örnek komutlar, örnek bir runbook'un son işini almak ve runbook parametreleri ve iş çıktısını için sağlanan değerler durumunu görüntüler.

    $job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
    –RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
    $job.Status
    $job.JobParameters
    Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output

## <a name="fair-share"></a>Dengeli dağıtım
Üç saat boyunca çalıştıktan sonra kaynakları bulutta tüm runbook'lar arasında paylaşmak için Azure Automation geçici olarak herhangi bir işi kaldıracak.  Bu süre boyunca işler [PowerShell tabanlı runbook'ları](automation-runbook-types.md#powershell-runbooks) durdurulur ve yeniden.  İş durumu gösterir **durduruldu**.  Kontrol noktaları desteklemeyen beri runbook bu tür her zaman en baştan yeniden başlatılır.  

[PowerShell iş akışı tabanlı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) kendi sonuncudan sürdürülecek [denetim noktası](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints).  Üç saat çalıştırdıktan sonra runbook işi hizmeti ve durumunu tarafından askıya alınacak gösterir **çalıştıran, kaynaklar bekleniyor**.  Bir korumalı alan kullanılabilir duruma geldiğinde runbook Otomasyon hizmeti ve en son kontrol modundan tarafından otomatik olarak başlatılır.  Askıya alma/yeniden başlatma için normal PowerShell iş akışı davranış budur.  Runbook yeniden çalışma zamanı, üç saatlik aşarsa işlemi, en fazla üç kez yineler.  Runbook hala üç saat içinde tamamlanmadı sonra runbook işi başarısız oldu ve iş durumu gösterir üçüncü yeniden başlatma işleminden sonra **kaynaklar bekleniyor başarısız**.  Bu durumda, şu özel durumla hatası alırsınız.

*İş sürekli olarak aynı denetim noktasından çıkarıldı çünkü çalışmaya devam edemiyor. Runbook'unuzda durumuna kalıcı olmadan uzun işlemleri gerçekleştirmez emin olun.*

Yeniden kaldırılıyor olmadan sonraki denetim sağlamak mümkün olmayan gibi hizmet tamamlamadan, süresiz olarak çalışan runbook'lar korumak için budur.

Ardından runbook herhangi bir denetim noktası varsa veya iş ilk denetim noktası kaldırıldığında önce ulaşmıştı değil, baştan başlatılır.  

Bir runbook oluştururken etkinlikler arasında iki kontrol noktaları çalıştırmak için zaman üç saat aşmadığından emin olmalısınız. Runbook'unuzda bu değil Bu üç saat sınırına ulaştığında veya uzun süre sonu emin olmak için denetim noktaları eklemek gerekebilir işlemleri çalıştırma. Örneğin, runbook'unuz bir arat büyük bir SQL veritabanında gerçekleştirebilir. Bu tek bir işlem Orta paylaşımı sınırı içinde tamamlanmazsa, sonra iş kaldırıldı ve baştan yeniden. Bu durumda, bir kerede bir tablo yeniden dizin oluşturmaya gibi birden çok adımlara arat işlemi bölmeniz ve son işlemi sonra işi devam ettirin böylece her işlemden sonra bir denetim noktası ekleme gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure Otomasyonu runbook'u başlatmak için kullanılan farklı yöntemleri hakkında daha fazla bilgi için bkz: [Azure Otomasyonu runbook başlatma](automation-starting-a-runbook.md)

