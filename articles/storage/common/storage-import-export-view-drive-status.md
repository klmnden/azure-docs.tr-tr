---
title: Azure içeri/dışarı aktarma işi durumunu görüntüleme | Microsoft Docs
description: İçeri/dışarı aktarma işleri ve kullanılan sürücüleri durumunu görüntüleme hakkında bilgi edinin.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 225164fe00f70839446f8b74155cd3959f745a49
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478054"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri durumunu görüntüleme

Bu makalede, Azure içeri/dışarı aktarma işleri için sürücü ve iş durumunun nasıl görüntüleneceği hakkında bilgi sağlar. Azure içeri/dışarı aktarma hizmeti, büyük miktarda veriyi Azure BLOB ve Azure dosyaları için güvenli bir şekilde aktarmak için kullanılır. Hizmet Ayrıca, Azure Blob depolama alanından verileri dışarı aktarmak için kullanılır.  

## <a name="view-job-and-drive-status"></a>İşimin ve Sürücümün durumunu görüntüle
İçeri aktarma işleminizin durumunu izlemek veya işleri, Azure portalından dışarı aktarın. Tıklayın **içeri/dışarı aktarma** sekmesi. İşlerinizi Listesi sayfasında görüntülenir.

![Görünüm işi durumu](./media/storage-import-export-service/jobstate.png)

## <a name="view-job-status"></a>İş durumunu görüntüleme

Sürücünüzün işlemde olduğu bağlı olarak aşağıdaki iş durumları birini görürsünüz.

| İş durumu | Açıklama |
|:--- |:--- |
| Oluşturma | Bir işi oluşturulduktan sonra durumuna ayarlanır **oluşturma**. İş çalışırken **oluşturma** durumunda, içeri/dışarı aktarma hizmeti sürücüleri sevk veri merkezine varsayar. Bir iş, iki sonra otomatik olarak hizmet tarafından silinir haftaya kadar bu durumda kalabilir. |
| Sevkiyat | Paketiniz sevk sonra Azure portalında izleme bilgilerini güncelleştirmeniz gerekir.  Bu işi kapatır **sevkiyat** durumu. Kalan iş **sevkiyat** iki haftaya kadar durum. 
| Alındı | Tüm sürücüleri, veri merkezinde alındıktan sonra iş durumu kümesine **alınan**. |
| Aktarma | En az bir sürücüsü işleme başladıktan sonra iş durumu kümesine **aktarma**. Daha fazla bilgi için Git [sürücü durumları](#view-drive-status). |
| Paketleme | Tüm sürücüleri işlemeyi tamamladıktan sonra iş yerleştirildi **paketleme** sürücüler size geri gönderilir ve kadar belirtin. |
| Tamamlandı | Projenin hatasız tamamlanırsa tüm sürücüler size geri gönderilir ve sonra iş sonra ayarlanmış **tamamlandı**. İş otomatik olarak 90 gün sonra silinir **tamamlandı** durumu. |
| Kapalı | İş işlenmesi sırasında herhangi bir hata varsa tüm sürücüler size geri gönderilir ve sonra iş kümesine **kapalı**. İş otomatik olarak 90 gün sonra silinir **kapalı** durumu. |

## <a name="view-drive-status"></a>Sürücü durumunu görüntüleme

Aşağıdaki tabloda bir içeri veya dışarı aktarma işi ile geçiş ayrı ayrı bir sürücü yaşam döngüsünü açıklanmaktadır. Azure portalında bir işteki her bir sürücü geçerli durumunu görebilirsiniz.

Aşağıdaki tabloda, her bir iş sürücüsünde geçişine her durumunu açıklar.

| Sürücü durumu | Açıklama |
|:--- |:--- |
| Belirtilen | İçeri aktarma işi için Azure portalından iş oluşturulduğunda ilk sürücü için durumudur **belirtilen**. Sürücü yok iş oluşturulduğunda, belirtilen bir dışarı aktarma işi için ilk sürücü durumu olduğu **alınan**. |
| Alındı | Sürücü geçiş **alınan** durum içeri aktarma işi için kargo şirketi öğesinden alınan sürücüleri içeri/dışarı aktarma hizmetini ne zaman işlediği. Bir dışarı aktarma işi için ilk sürücü durumudur **alınan** durumu. |
| NeverReceived | Sürücü taşır **NeverReceived** durum paket için bir iş ulaşan ancak paket sürücü içermiyor. İki haftalık gönderim bilgilerinizi hizmet aldı, ancak paket henüz veri merkezinde edinildi değil beri çağrıldıysa, bir sürücü, ayrıca bu duruma taşır. |
| Aktarma | Bir sürücü taşır **aktarma** durum verileri Azure Depolama'ya sürücüden aktarmak hizmet başladığı. |
| Tamamlandı | Bir sürücü taşır **tamamlandı** durum zaman hizmeti herhangi bir hata ile tüm verileri başarıyla aktarıldı.
| CompletedMoreInfo | Bir sürücü taşır **CompletedMoreInfo** durum hizmet bazı sorunlar ya da sürücüye veri kopyalarken zaman karşılaştı. Bilgiler, hataları, uyarıları veya BLOB'ları üzerine hakkında bilgilendirme iletileri ekleyebilirsiniz.
| ShippedBack | Bir sürücü taşır **ShippedBack** geri dönüş adresi için veri merkezinden gönderildi, durumu. |

Bu görüntü Azure portalından bir örnek iş sürücüsü durumunu gösterir:

![Sürücü durumunu görüntüle](./media/storage-import-export-service/drivestate.png)

Aşağıdaki tabloda, sürücü hata durumları ve her durum için gerçekleştirilen eylemleri açıklar.

| Sürücü durumu | Olay | Çözüm / sonraki adım |
|:--- |:--- |:--- |
| NeverReceived | Olarak işaretlenmiş bir sürücü **NeverReceived** (işin sevkiyat bir parçası olarak alınmadı çünkü) başka bir sevkiyat ulaşır. | Operasyon ekibinin sürücüye taşır **alınan**. |
| Yok | Başka bir iş parçası olarak veri merkezinde herhangi bir iş parçası olmayan bir sürücüye ulaşır. | Sürücü, ek bir sürücü olarak işaretlenir ve özgün paket ile ilişkili iş tamamlandığında size geri döndürülür. |

## <a name="time-to-process-job"></a>İşleme işi süresi
İçeri/dışarı aktarma işi işlemek için geçen süreyi gibi bir dizi etkene göre farklılık gösterir:

-  Gönderim saati
-  Veri merkezinde yükleme
-  İş türü ve kopyalanan verilerin boyutu
-  Bir İşte disk sayısı. 

İçeri/dışarı aktarma hizmeti, bir SLA yoktur, ancak hizmet kopyalama diskleri alındıktan sonra 7 ila 10 gün içinde tamamlanması üstlenmeye çalışır. Azure portalında gönderilen durum testlerine ek olarak, REST API'leri, iş ilerleme durumunu izlemek için kullanılabilir. Tamamlanma parametresinde [Listeleyemeyeceksiniz](/previous-versions/azure/dn529083(v=azure.100)) API işlem çağrısı yüzdesi kopyalama işinin ilerleme durumunu sağlar.


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport Aracı'nı ayarlama](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programıyla veri aktarma](storage-use-azcopy.md)
* [Azure içeri dışarı aktarma REST API örneği](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
