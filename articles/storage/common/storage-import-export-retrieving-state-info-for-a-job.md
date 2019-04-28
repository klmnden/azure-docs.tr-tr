---
title: Azure içeri/dışarı aktarma işi için durum bilgilerini alma | Microsoft Docs
description: Microsoft Azure içeri/dışarı aktarma hizmeti işleri için durum bilgilerini almak öğrenin.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 1a878b5a9f0502ff9acd411359895d7431fb76f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478683"
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>İçeri/dışarı aktarma işi için durum bilgilerini alma
Çağırabilirsiniz [alma işi](/rest/api/storageimportexport/jobs) hem hakkında bilgi almak için işlemi içeri ve dışarı aktarma işleri. Döndürülen bilgileri içerir:

-   İşin geçerli durumu.

-   Yaklaşık yüzde her işlemi tamamlanmıştır.

-   Her sürücü geçerli durumu.

-   Hata günlüklerini ve ayrıntılı günlük kaydı bilgilerini (etkinse) içeren bloblar için URI.

Tarafından döndürülen bilgileri aşağıdaki bölümlerde açıklanmaktadır `Get Job` işlemi.

## <a name="job-states"></a>İş durumları
Tablo ve durumu aşağıdaki diyagramda yaşam döngüsü sırasında geçişler aracılığıyla bir iş durumları açıklayın. İşin geçerli durumu çağırarak belirlenebilir `Get Job` işlemi.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

Aşağıdaki tabloda, bir işi geçişine her bir durumu açıklar.

|İş Durumu|Açıklama|
|---------------|-----------------|
|`Creating`|Sonra iş Put işlemi çağrısı, bir iş oluşturulur ve durumuna ayarlanır `Creating`. İş çalışırken `Creating` durumunda, içeri/dışarı aktarma hizmeti sürücüleri sevk veri merkezine varsayar. Bir iş olarak kalabilir `Creating` iki sonra bunu otomatik olarak silinmez hizmet tarafından haftaya kadar durum.<br /><br /> İş çalışırken işi özellikleri güncelleştirme işlemi çağrısı yaparsanız `Creating` durumunda, işi kaldığı `Creating` durumu ve zaman aşımı aralığı için iki hafta sıfırlanır.|
|`Shipping`|Paketiniz sevk sonra güncelleştirme işi özellikleri işlem güncelleştirmesi için iş durumunu çağırmalıdır `Shipping`. Sevkiyat durumu yalnızca aşağıdaki durumlarda ayarlanabilir `DeliveryPackage` (posta taşıyıcı ve izleme numarası) ve `ReturnAddress` özellikleri, iş için ayarlandı.<br /><br /> İş için iki haftalık gönderim halde kalır. İki hafta geçmiştir ve sürücüleri alınmamış, içeri/dışarı aktarma hizmeti işleçleri bildirilir.|
|`Received`|Tüm sürücüler veri merkezinde ulaştıktan sonra iş durumu alındı durumuna ayarlanır.|
|`Transferring`|Sürücüleri, veri merkezinde alındı ve en az bir sürücü, işleme başladı sonra iş durumu ayarlanacak `Transferring` durumu. Bkz: `Drive States` bölümünde ayrıntılı bilgi için.|
|`Packaging`|Tüm sürücüleri işlemeyi tamamladıktan sonra iş yerleştirileceği `Packaging` sürücüleri müşteriye geri sevk kadar belirtin.|
|`Completed`|Projenin hatasız tamamlanırsa müşteri için tüm sürücüleri sevk edilmiş sonra ardından iş ayarlanacak `Completed` durumu. İş otomatik olarak 90 gün sonra silinir `Completed` durumu.|
|`Closed`|İşin işleme sırasında hataları yapıldı, müşteri için tüm sürücüleri sevk edilmiş sonra ardından iş ayarlanacak `Closed` durumu. İş otomatik olarak 90 gün sonra silinir `Closed` durumu.|

Bir işi yalnızca belirli durumlar iptal edebilirsiniz. İptal edilen bir işi veri kopyalama adımı atlar, ancak iş iptal olarak aynı durum geçişlerini takip eden Aksi takdirde.

Aşağıdaki tabloda, bir hata oluştuğunda iş üzerindeki etkisini yanı sıra, her iş durumu için oluşabilecek hatalar açıklanmaktadır.

|İş Durumu|Olay|Çözüm / sonraki adımlar|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Bir iş için bir veya daha fazla sürücü gelen, ancak iş kullanımda olmayan `Shipping` durumu veya hizmetteki iş kaydı yok.|İçeri/dışarı aktarma hizmeti operasyon ekibinin, oluşturmak veya iş iş ilerlemek için gerekli olan bilgileri güncelleştirmek için bir müşteri sizinle iletişime geçmeyi dener.<br /><br /> İşlemler ekibinin müşteri iki hafta içinde iletişim kuramazsa, operasyon ekibinin sürücüleri dönüş dener.<br /><br /> Sürücüleri döndürülemez ve müşteri ulaşılamaması halinde durumunda, sürücüleri 90 gün içinde güvenli bir şekilde yok edilir.<br /><br /> Bir işin durumuna şekilde güncelleştirilene kadar işlenemiyor Not `Shipping`.|
|`Shipping`|Bir işi için paketi iki hafta boyunca edinildi değil.|İşlemler ekibinin, müşterinin eksik paket bildirir. Müşterinin yanıtta bağlı olarak, operasyon ekibinin paketini gelmesi için beklenecek genişletmek veya işi iptal.<br /><br /> Müşteri temas kurulamıyor ya da 30 gün içinde yanıt vermezse, olay, operasyon ekibinin işten taşımak için eylem başlatacak `Shipping` doğrudan durum `Closed` durumu.|
|`Completed/Closed`|Sürücüleri hiçbir zaman dönüş adresi ulaştınız veya bu (yalnızca bir dışarı aktarma işi için geçerlidir) sevkiyat zarar görmüş.|Sürücüleri dönüş adresi erişmez, müşteri alma işi işlem çağırma ilk veya sürücüleri yüklediğinizden emin olmak için portalında iş durumunu denetleyin. Sürücüleri sevk edilmişse, müşteri sürücüleri bulmak için sevkiyat sağlayıcısı başvurmanız gerekir.<br /><br /> Sevkiyat sırasında sürücüleri bozuksa, müşterinin başka bir dışarı aktarma işi isteği ya da eksik blobları indirmek isteyebilirsiniz.|
|`Transferring/Packaging`|İş veya dönüş adresi eksik, yanlış bir sahiptir.|Operasyon ekibinin doğru adresini almak için ilgili kişiye işin ulaşır.<br /><br /> Müşteri ulaşılamaması halinde durumunda, sürücüleri 90 gün içinde güvenli bir şekilde yok edilir.|
|`Creating / Shipping/ Transferring`|İçeri aktarılacak sürücülerin listede görünmeyen bir sürücü sevkiyat paketine dahildir.|Ek sürücüler işlenmeyecek ve iş tamamlandığında müşteri için döndürülür.|

## <a name="drive-states"></a>Sürücü durumları
Bir içeri veya dışarı aktarma işi ile geçiş olarak tablo ve aşağıdaki diyagramda ayrı ayrı bir sürücü yaşam döngüsünü açıklar. Çağırarak geçerli sürücü durumu alabilirsiniz `Get Job` işlemi ve İnceleme `State` öğesinin `DriveList` özelliği.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

Aşağıdaki tabloda, bir sürücü geçişine her bir durumu açıklar.

|Sürücü durumu|Açıklama|
|-----------------|-----------------|
|`Specified`|Bir içeri aktarma işi için iş Put işlemiyle iş oluşturulduğunda ilk sürücü için durumudur `Specified` durumu. Sürücü yok iş oluşturulduğunda, belirtilen bir dışarı aktarma işi için ilk sürücü durumu olduğu `Received` durumu.|
|`Received`|Sürücü geçiş `Received` durum içeri aktarma işi için kargo şirketi öğesinden alınan sürücüler içeri/dışarı aktarma hizmeti işleci zaman işledi. Bir dışarı aktarma işi için ilk sürücü durumudur `Received` durumu.|
|`NeverReceived`|Sürücü taşınacağı `NeverReceived` durum paket için bir iş ulaşan ancak paket sürücü içermiyor. Bir sürücü bu duruma, iki haftalık gönderim bilgilerinizi hizmet aldı, ancak paket henüz veri merkezinde edinildi değil oluşturmanızın da taşıyabilirsiniz.|
|`Transferring`|Bir sürücü taşınacağı `Transferring` durumu hizmeti Windows Azure depolama alanına sürücüsünden veri aktarımı başladığı.|
|`Completed`|Bir sürücü taşınacağı `Completed` durum zaman hizmeti herhangi bir hata ile tüm verileri başarıyla aktarıldı.|
|`CompletedMoreInfo`|Bir sürücü taşınacağı `CompletedMoreInfo` durum hizmet bazı sorunlar ya da sürücüye veri kopyalarken zaman karşılaştı. Bilgiler, hataları, uyarıları veya BLOB'ları üzerine hakkında bilgilendirme iletileri ekleyebilirsiniz.|
|`ShippedBack`|Sürücü taşınacağı `ShippedBack` dönüş adresi için veri merkezi yeniden gönderildi, durumu.|

Aşağıdaki tabloda, sürücü hata durumları ve her durum için gerçekleştirilen eylemleri açıklar.

|Sürücü durumu|Olay|Çözüm / sonraki adım|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|Olarak işaretlenmiş bir sürücü `NeverReceived` (işin sevkiyat bir parçası olarak alınmadı çünkü) başka bir sevkiyat ulaşır.|Operasyon ekibinin sürücüye taşınır `Received` durumu.|
|`N/A`|Başka bir iş parçası olarak veri merkezindeki herhangi bir iş parçası olmayan bir sürücüye ulaşır.|Sürücü, ek bir sürücü olarak işaretlenir ve özgün paket ile ilişkili iş tamamlandığında müşteri için döndürülecek.|

## <a name="faulted-states"></a>Hatalı durumları
Normalde, beklenen yaşam döngüsü boyunca ilerleme bir proje veya sürücü başarısız olduğunda, iş veya sürücü ne taşınacak bir `Faulted` durumu. Bu noktada, operasyon ekibinin, müşteri tarafından e-posta veya telefon bağlantı kurar. Sorun çözüldükten sonra hatalı bir iş veya sürücü tanesi gerçekleştirilecek `Faulted` durum ve uygun duruma taşındı.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'sini kullanma](storage-import-export-using-the-rest-api.md)
