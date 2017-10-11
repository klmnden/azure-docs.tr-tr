---
title: "Bir Azure içeri/dışarı aktarma işi için durum bilgilerini alma | Microsoft Docs"
description: "Microsoft Azure içeri/dışarı aktarma hizmeti işlerinin durumu bilgilerini elde öğrenin."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: 13169716c47cf9389c8f2651393ac744441bdd6f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>İçeri/dışarı aktarma işi için durum bilgilerini alma
Çağırabilirsiniz [alma işi](/rest/api/storageimportexport/jobs#Jobs_Get) her ikisi de hakkında bilgi almak için işlemi almak ve işleri dışarı aktarma. Döndürülen bilgileri içerir:

-   İşin geçerli durumu.

-   Her işi tamamlandı yaklaşık yüzde.

-   Her bir sürücü geçerli durumu.

-   Hata günlüklerini ve ayrıntılı günlük kaydı bilgileri (etkinse) içeren BLOB'ları URI.

Aşağıdaki bölümlerde açıklanmıştır tarafından döndürülen bilgi `Get Job` işlemi.

## <a name="job-states"></a>İş durumları
Tablo ve aşağıdaki durumu diyagramı yaşam döngüsü sırasında aracılığıyla bir iş geçiş durumları açıklayın. İşin geçerli durumu çağırarak belirlenebilir `Get Job` işlemi.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

Aşağıdaki tabloda bir işi geçirir her durumu açıklar.

|İş durumu|Açıklama|
|---------------|-----------------|
|`Creating`|Sonra iş Put işlemini çağırın, bir iş oluşturulur ve durumu kümesine `Creating`. İş olarak kullanılırken `Creating` durumunda, içeri/dışarı aktarma hizmeti varsayar sürücüleri sevk veri merkezi. Bir iş devam edebilir `Creating` haftaya kadar iki daha sonra onu otomatik olarak silinir hizmeti tarafından durum.<br /><br /> İş olarak kullanılırken güncelleştirme işi özellikleri işlem çağrısı yaparsanız `Creating` durumunda, iş kaldığı `Creating` durumu ve zaman aşımı aralığı için iki hafta sıfırlanır.|
|`Shipping`|Paketinizi sevk sonra işin durumunu güncelleştirme işi özellikleri işlemi güncelleştirme çağırmalıdır `Shipping`. Sevkiyat durumu yalnızca ayarlanabilir `DeliveryPackage` (posta taşıyıcı ve izleme numarası) ve `ReturnAddress` özellikleri iş için ayarlanmış.<br /><br /> İş için iki haftalık sevkiyat durumunda kalır. İki hafta geçtiğini ve sürücüleri alınmamış içeri/dışarı aktarma hizmeti işleçleri bildirilir.|
|`Received`|Tüm sürücüleri veri merkezinde alınmış olan sonra iş durumu alınan durumuna ayarlanır.|
|`Transferring`|Sürücüleri veri merkezinde alınmış olan ve en az bir sürücü işleme başladı sonra iş durumu ayarlanacak `Transferring` durumu. Bkz: `Drive States` ayrıntılı bilgi için bölüm aşağıda.|
|`Packaging`|Tüm sürücüler işlemeyi tamamladıktan sonra iş yerleştirilecek `Packaging` sürücüleri müşteriye geri gönderilen kadar belirtin.|
|`Completed`|İş bir hata olmadan tamamlandı, müşteri için tüm sürücülerin sevk edilmiş sonra sonra iş ayarlanacak `Completed` durumu. İş 90 gün sonra otomatik olarak silinir `Completed` durumu.|
|`Closed`|İş işleme sırasında hataları olmuştur, müşteri için tüm sürücülerin sevk edilmiş sonra sonra iş ayarlanacak `Closed` durumu. İş 90 gün sonra otomatik olarak silinir `Closed` durumu.|

Bir işi yalnızca belirli durumlar iptal edebilirsiniz. Veri kopyalama adımı işi iptal edildi atlar ancak Aksi durumda iptal bir iş olarak aynı durumu geçişleri izler.

Aşağıdaki tabloda, bir hata oluştuğunda, iş etkisi yanı sıra, her iş durumu için oluşabilecek hatalar açıklanmaktadır.

|İş durumu|Olay|Çözümleme / sonraki adımlar|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Bir iş için bir veya daha fazla sürücüler ulaşmadığını, ancak iş olmayan `Shipping` durumu veya hizmet iş kaydı yok.|İçeri/dışarı aktarma hizmeti işletim ekibi müşteri oluşturmak veya iş iş ilerlemek için gerekli olan bilgileri güncelleştirmek için iletişim dener.<br /><br /> İşlemler ekibinin iki hafta içinde müşteri bağlanamadığından ise, işletim ekibi sürücüleri dönüş dener.<br /><br /> Sürücüleri, güvenli bir şekilde sürücüleri döndürülemiyor ve müşteri ulaşılamıyor gelmesi durumunda, 90 gün içinde yok.<br /><br /> Bir iş durumu olarak güncelleştirilene kadar işlenemiyor Not `Shipping`.|
|`Shipping`|Paket için bir iş üzerinde iki hafta boyunca gelen değil.|İşlemler ekibinin eksik paket müşteri size bildirir. Müşteri'nin yanıtta bağlı olarak, işletim ekibi ulaşması paket için beklenecek süreyi genişletmek, veya işi iptal.<br /><br /> Müşteri temas kurulamıyor veya 30 gün içinde yanıt vermezse, durumunda, işletim ekibi işten taşımak için eylem başlatacaktır `Shipping` doğrudan durum `Closed` durumu.|
|`Completed/Closed`|Sürücüler hiçbir zaman dönüş adresi sınırına ya da (yalnızca bir dışa aktarma işi için geçerlidir) sevkiyat zarar görmüş.|Sürücüleri dönüş adresi değil ulaştıysanız, müşteri alma işi işlem çağırma ilk veya sürücüleri sevk edilmiş emin olmak için portalda iş durumunu denetleyin. Sürücüleri sevk, müşteri sürücüleri bulmak için sevkiyat sağlayıcısı başvurmanız gerekir.<br /><br /> Sürücüleri sevkiyat sırasında bozuksa, müşterinin başka bir dışarı aktarma işini isteyebilir veya eksik blobları yüklemek isteyebilirsiniz.|
|`Transferring/Packaging`|İş, yanlış bir ya da dönüş adresi eksik sahiptir.|İşlemler ekibinin doğru adresi elde etmek için ilgili kişiye iş için ulaşın.<br /><br /> Sürücüleri, güvenli bir şekilde müşteri ulaşılamıyor gelmesi durumunda, 90 gün içinde yok.|
|`Creating / Shipping/ Transferring`|İçeri aktarılacak sürücüler listesinde görünmeyen bir sürücü sevkiyat pakete dahil edilir.|Ek sürücüler işlenmeyecek ve iş tamamlandığında, müşteriye döndürülür.|

## <a name="drive-states"></a>Sürücü durumları
Bir içe veya dışa aktarma işi ile geçiş yapıldığı gibi tablo ve aşağıdaki diyagramda ayrı ayrı bir sürücü yaşam döngüsünü açıklanmaktadır. Geçerli sürücü durumu çağırarak alabilir `Get Job` işlemi ve İnceleme `State` öğesinin `DriveList` özelliği.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

Aşağıdaki tabloda bir sürücü üzerinden iletebilir her durumu açıklar.

|Sürücü durumu|Açıklama|
|-----------------|-----------------|
|`Specified`|Bir içeri aktarma işi için iş iş Put işlemiyle oluşturulduğunda ilk bir sürücü için bir durumda `Specified` durumu. İş oluşturulduğunda, hiçbir sürücü belirtilen bir dışa aktarma işi için ilk sürücü durumu olduğu `Received` durumu.|
|`Received`|Sürücü geçişleri için `Received` durum içeri/dışarı aktarma hizmeti işleci bir içeri aktarma işi için sevkiyat şirketten alınan sürücüleri zaman işledi. Bir dışarı aktarma işi için ilk sürücü durumda `Received` durumu.|
|`NeverReceived`|Sürücü taşır `NeverReceived` işi için paket ulaşır ancak sürücü içermiyor. paket durumu. İki hafta teslimat hizmeti aldı, ancak paket henüz veri merkezinde geldi değil bu yana olması durumunda bir sürücü de bu duruma taşıyabilirsiniz.|
|`Transferring`|Bir sürücü taşır `Transferring` durumu ne zaman sürücüden Windows Azure depolama alanına veri aktarmak hizmet başlar.|
|`Completed`|Bir sürücü taşır `Completed` durum hizmeti başarıyla aktarılan hatasız tüm verilerin ne zaman.|
|`CompletedMoreInfo`|Bir sürücü taşır `CompletedMoreInfo` durum ya da sürücüye veri kopyalanırken hizmet bazı sorunlar olduğunda karşılaştı. Hataları, uyarı veya bilgilendirme iletileri BLOB'lar üzerine hakkında bilgiler içerebilir.|
|`ShippedBack`|Sürücü taşır `ShippedBack` , veri merkezi arkadan dönüş adresi sevk edilmiş zaman durumu.|

Aşağıdaki tabloda, sürücü hata durumları ve her durum için gerçekleştirilen eylemleri açıklar.

|Sürücü durumu|Olay|Çözümleme / sonraki adım|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|Olarak işaretlenmiş bir sürücü `NeverReceived` (işin sevkiyat bir parçası olarak alınmadı çünkü) başka bir sevkiyat ulaşır.|İşlemler ekibinin sürücüye taşınır `Received` durumu.|
|`N/A`|Başka bir iş parçası olarak veri merkezindeki herhangi bir işi parçası olmayan bir sürücü ulaşır.|Sürücü fazladan bir sürücü olarak işaretlenir ve özgün paket ile ilişkili iş tamamlandığında müşteriye döndürülür.|

## <a name="faulted-states"></a>Hatalı durumları
Normalde, beklenen yaşam döngüsü boyunca ilerleme bir iş veya sürücü başarısız olduğunda, iş veya sürücü ne taşınacak bir `Faulted` durumu. Bu noktada, işletim ekibi müşteri e-posta veya telefon tarafından sizinle iletişim kuracaktır. Sorun çözüldükten sonra hatalı iş veya sürücü dışı gerçekleştirilecek `Faulted` durum ve uygun durumda içine taşındı.

## <a name="next-steps"></a>Sonraki adımlar

* [İçeri/dışarı aktarma hizmeti REST API'si kullanma](storage-import-export-using-the-rest-api.md)
