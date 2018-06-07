---
title: Azure içeri/dışarı aktarma işlerin durumunu görüntülemek | Microsoft Docs
description: İçeri/dışarı aktarma işleri ve kullanılan sürücüleri durumunu görüntülemek öğrenin.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 05/17/2018
ms.author: alkohli
ms.openlocfilehash: 176cbf190b6442682a222eb4f24b6583fb87a46b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661018"
---
# <a name="view-the-status-of-azure-importexport-jobs"></a>Azure içeri/dışarı aktarma işleri durumunu görüntüleme

Bu makalede, Azure içeri/dışarı aktarma işleri sürücü ve iş durumunu görüntülemek nasıl hakkında bilgiler sağlar. Azure içeri/dışarı aktarma hizmeti, Azure BLOB'ları ve Azure dosyaları güvenli bir şekilde büyük miktarlarda veri aktarmak için kullanılır. Hizmeti, verileri Azure Blob depolama alanından vermek için de kullanılır.  

## <a name="view-job-and-drive-status"></a>İş ve sürücü durumunu görüntüle
Alma durumunu izlemek veya Azure portalından işleri verebilirsiniz. Tıklatın **içeri/dışarı aktarma** sekmesi. İşleriniz Listesi sayfasında görüntülenir.

![Görünüm iş durumu](./media/storage-import-export-service/jobstate.png)


## <a name="view-job-status"></a>İş durumunu görüntüleme

Sürücünüzü işlemde olduğu bağlı olarak aşağıdaki iş durumlardan birine bakın.

| İş Durumu | Açıklama |
|:--- |:--- |
| Oluşturma | Bir işi oluşturulduktan sonra durumu kümesine **oluşturma**. İş olarak kullanılırken **oluşturma** durumunda, içeri/dışarı aktarma hizmeti varsayar sürücüleri sevk veri merkezi. Bir işi haftaya kadar iki daha sonra otomatik olarak hizmet tarafından silinir, bu durumda kalabilir. |
| Sevkiyat | Paketinizi sevk sonra Azure portalında izleme bilgilerini güncelleştirmeniz gerekir.  Bu işi kapatır **sevkiyat** durumu. Kalan iş **sevkiyat** iki haftaya durum. 
| Alındı | Tüm sürücüleri veri merkezinde alındıktan sonra iş durumu kümesine **alınan**. |
| Aktarılıyor | En az bir sürücü işleme başladıktan sonra iş durumu kümesine **aktarma**. Daha fazla bilgi için Git [sürücü durumları](#view-drive-status). |
| Paketleme | Tüm sürücüler işlemeyi tamamladıktan sonra iş yerleştirilir **paketleme** sürücüleri size geri gönderilir kadar belirtin. |
| Tamamlandı | İş hatasız tamamladıysa tüm sürücüler size geri gönderilir sonra iş sonra ayarlanmış **tamamlandı**. İş 90 gün sonra otomatik olarak silinir **tamamlandı** durumu. |
| Kapatıldı | İş işleme sırasında hataları durumunda tüm sürücüler size geri gönderilir sonra iş kümesine **kapalı**. İş 90 gün sonra otomatik olarak silinir **kapalı** durumu. |

## <a name="view-drive-status"></a>Sürücü durumunu görüntüle

Bir içe veya dışa aktarma işi ile geçiş yapıldığı gibi ayrı ayrı bir sürücü yaşam döngüsünü aşağıdaki tabloda açıklanmıştır. Bir işi her sürücüde geçerli durumu, Azure portalında görülür.

Aşağıdaki tabloda her bir iş sürücüde geçirir her durumu açıklar.

| Sürücü durumu | Açıklama |
|:--- |:--- |
| Belirtilen | Bir içeri aktarma işi için iş Azure portalından oluşturulduğunda ilk bir sürücü için bir durumda **belirtilen**. İş oluşturulduğunda, hiçbir sürücü belirtilen bir dışa aktarma işi için ilk sürücü durumu olduğu **alınan**. |
| Alındı | Sürücü geçişleri için **alınan** durum içeri/dışarı aktarma hizmeti bir içeri aktarma işi için sevkiyat şirketten alınan sürücüleri zaman işlediğinden. Bir dışarı aktarma işi için ilk sürücü durumda **alınan** durumu. |
| NeverReceived | Sürücü taşır **NeverReceived** işi için paket ulaşır ancak sürücü içermiyor. paket durumu. İki hafta teslimat hizmeti aldı, ancak paket henüz datacenter geldi değil bu yana olması durumunda bir sürücü Ayrıca, bu duruma taşır. |
| Aktarılıyor | Bir sürücü taşır **aktarma** durumu ne zaman sürücüden Azure depolama alanına veri aktarmak hizmet başlar. |
| Tamamlandı | Bir sürücü taşır **tamamlandı** durum hizmeti başarıyla aktarılan hatasız tüm verilerin ne zaman.
| CompletedMoreInfo | Bir sürücü taşır **CompletedMoreInfo** durum ya da sürücüye veri kopyalanırken hizmet bazı sorunlar olduğunda karşılaştı. Hataları, uyarı veya bilgilendirme iletileri BLOB'lar üzerine hakkında bilgiler içerebilir.
| ShippedBack | Bir sürücü taşır **ShippedBack** , veri merkezinden geri dönüş adresi sevk edilmiş zaman durumu. |

Bu görüntü Azure portalından bir örnek işi sürücü durumunu görüntüler:

![Sürücü durumunu görüntüle](./media/storage-import-export-service/drivestate.png)

Aşağıdaki tabloda, sürücü hata durumları ve her durum için gerçekleştirilen eylemleri açıklar.

| Sürücü durumu | Olay | Çözümleme / sonraki adım |
|:--- |:--- |:--- |
| NeverReceived | Olarak işaretlenmiş bir sürücü **NeverReceived** (işin sevkiyat bir parçası olarak alınmadı çünkü) başka bir sevkiyat ulaşır. | İşlemler ekibinin sürücüye taşır **alınan**. |
| Yok | Başka bir iş parçası olarak veri merkezindeki herhangi bir işi parçası olmayan bir sürücü ulaşır. | Sürücü fazladan bir sürücü olarak işaretlenir ve özgün paket ile ilişkili iş tamamlandığında, döndürülür. |

## <a name="time-to-process-job"></a>İşlem işi zaman
İçeri/dışarı aktarma işi işlemek için gereken süreyi gibi bir dizi etkene göre farklılık gösterir:

-  Sevkiyat Zamanı
-  Datacenter yükleme
-  İş türü ve kopyalanan verilerin boyutu
-  Bir İşte disk sayısı. 

İçeri/dışarı aktarma hizmeti bir SLA yok ancak kopyalama 7-10 diskleri alındıktan sonra gün içinde tamamlamak için hizmet çalışır. Azure Portal'da gönderilen durum ek olarak, REST API'leri iş ilerleme durumunu izlemek için kullanılabilir. Tamamlanma parametresinde [listesi işleri]() işlem API çağrısı yüzdesi kopyalama ilerleme sağlar.


## <a name="next-steps"></a>Sonraki adımlar

* [WAImportExport aracı ayarlamak](storage-import-export-tool-how-to.md)
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](storage-use-azcopy.md)
* [Azure alma verme REST API örnek](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

