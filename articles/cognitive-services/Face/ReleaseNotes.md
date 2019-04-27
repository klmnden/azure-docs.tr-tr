---
title: Sürüm Notları - yüz tanıma API'si hizmeti
titleSuffix: Azure Cognitive Services
description: Yüz tanıma API'si hizmeti için sürüm notları, çeşitli sürümleri için yayın değişikliklerinin geçmişini içerir.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: yluiu
ms.openlocfilehash: 8502d643986392ba846d509819c6d8f63f931c07
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60815212"
---
# <a name="face-api-release-notes"></a>Yüz tanıma API'si sürüm notları

Bu makalede, yüz tanıma API'si hizmeti sürüm 1.0 için geçerlidir.

### <a name="release-changes-in-march-2019"></a>Mart 2019 yayın değişiklikleri

* Yeni bir yüz tanıma modeli ile geliştirilmiş doğruluğuna eklendi. Aracılığıyla kullanmak [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [LargeFaceList - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [PersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) ve [ LargePersonGroup - oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d) yeni yüz tanıma model adını belirterek `recognition_02` içinde `recognitionModel` parametresi. Daha ayrıntılı bilgi [tanıma modeli belirleme](Face-API-How-to-Topics/specify-recognition-model.md).

### <a name="release-changes-in-january-2019"></a>Ocak 2019 yayın değişiklikleri

* Abonelikler arasında veri geçişini desteklemek için eklenen anlık görüntü özelliği: [Anlık Görüntü](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/snapshot-get). Daha ayrıntılı bilgi [yüz tanıma verileriniz farklı bir yüz aboneliğe geçirme](Face-API-How-to-Topics/how-to-migrate-face-data.md).

### <a name="release-changes-in-october-2018"></a>Ekim 2018'de yayın değişiklikleri

* İyileştirilmektedir için açıklama `status`, `createdDateTime`, `lastActionDateTime`, ve `lastSuccessfulTrainingDateTime` içinde [PersonGroup - eğitim durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247), [LargePersonGroup - eğitim durumunu Al](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae32c6ac60f11b48b5aa5)ve [ LargeFaceList - Get eğitim durumu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a1582f8d2de3616c086f2cf).

### <a name="release-changes-in-may-2018"></a>Mayıs 2018'de yayın değişiklikleri

* Geliştirilmiş `gender` önemli ölçüde ve ayrıca Gelişmiş öznitelik `age`, `glasses`, `facialHair`, `hair`, `makeup` öznitelikleri. Bunları aracılığıyla [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi. 

* Itanium tabanlı sistemler için artan giriş resim dosya boyutu sınırı 4 MB'den 6 MB [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup kişi - ekleyin Yüz tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ve [LargePersonGroup kişi - yüz tanıma eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Mart 2018'de yayın değişiklikleri

* Milyon ölçeğinde kapsayıcı eklendi: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) ve [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Daha ayrıntılı bilgi [büyük ölçekli özelliğinin nasıl kullanılacağı](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Artırılmış [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parametresi [1, 5] için [1, 100] ve varsayılan 10.

### <a name="release-changes-in-may-2017"></a>Mayıs 2017'de yayın değişiklikleri

* Eklenen `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, ve `noise` öznitelikleri [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi.

* 10 bin kişiler bir PersonGroup desteklenen ve [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Sayfalandırma, desteklenen [PersonGroup kişi - listesinde](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) isteğe bağlı parametrelerle: `start` ve `top`.

* Ekleme/silme karşı farklı belirlenmiştir ve PersonGroup farklı kişilerin yüzlerini desteklenen eşzamanlılık.

### <a name="release-changes-in-march-2017"></a>Mart 2017'de yayın değişiklikleri
* Eklenen `emotion` özniteliğini [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi.

* Sabit yüzü değil kaldırılmasına döndürüldüğü dikdörtgen [yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) olarak `targetFace` içinde [FaceList - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ve [PersonGroup kişi - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Kesinlikle 36 x 36 için 4096 x 4096 piksel arasında olduğundan emin olmak için algılanabilir yüz boyutu sabit.

### <a name="release-changes-in-november-2016"></a>Kasım 2016 yayın değişiklikleri
* Eklenen kullanırken daha fazla kalıcı yüz depolamak için yüz depolama Standart abonelik [PersonGroup kişi - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) veya [FaceList - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) tanımlama veya benzerlik eşleşen. Depolanan görüntüler, 1000 yüz başına 0,5 $ üzerinden ücretlendirilir ve bu fiyat, günlere eşit olarak dağıtılır. Ücretsiz katman abonelik 1.000 toplam kişilere sınırlandırılacak şekilde devam edin.

### <a name="release-changes-in-october-2016"></a>Yayın değişiklikleri Ekim 2016
* Hata iletisini 'Vardır birden fazla yüz görüntüde' 'Var. birden fazla yüz görüntüde' den targetFace birden çok yüzü sürümünde değiştirilmiştir [FaceList - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ve [PersonGroup kişi - yüz tanımaEkle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Temmuz 2016 yayın değişiklikleri
* Kişi nesnesinin kimlik doğrulaması için yüz desteklenen [yüz tanıma - doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* İsteğe bağlı eklenen `mode` seçimi iki çalışma modunu etkinleştirme parametresi: `matchPerson` ve `matchFace` içinde [yüz tanıma - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve varsayılan `matchPerson`.

* İsteğe bağlı eklenen `confidenceThreshold` parametresi için bir kişi nesnesine ait bir yüz olup eşiği ayarlamak kullanıcı [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* İsteğe bağlı eklenen `start` ve `top` parametrelerinde [PersonGroup - liste](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) başlangıç noktası ve kişi sayısı listesine belirtmek kullanıcı etkinleştirmek için.

### <a name="v10-changes-from-v0"></a>V0 V1.0 değişiklikleri
* Güncelleştirilmiş hizmet kök uç noktasından ```https://westus.api.cognitive.microsoft.com/face/v0/``` için ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Uygulanan değişiklikler: [Yüz tanıma - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [yüz tanıma - tanımlamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz tanıma - benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [yüz tanıma - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* En az algılanabilir yüz boyutu 36 x 36 piksel olarak güncelleştirildi. Yüzleri 36 x 36 pikselden küçük algılanmaz.

* Yüz tanıma V0 PersonGroup ve kişi verileri kullanım dışı. Bu veri yüz tanıma V1.0 hizmetine erişilemiyor.

* Yüz tanıma API'si V0 uç noktası, 30 Haziran 2016 tarihinde kullanım dışı.
