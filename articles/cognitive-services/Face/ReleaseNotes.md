---
title: Yüz API'si hizmeti için sürüm notları | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Yüz API'si hizmeti için sürüm notları çeşitli sürümleri için yayın değişikliklerin geçmişini içerir.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 918b3ea5dbaaa44e4eee1a679354c7becffd4686
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355079"
---
# <a name="face-api-release-notes"></a>Yüz API Sürüm Notları

Bu makalede Microsoft yüz API hizmeti sürüm 1.0 için geçerlidir.

### <a name="release-changes-in-may-2018"></a>Mayıs 2018 sürüm değişiklikleri

* Geliştirilmiş `gender` önemli ölçüde ve ayrıca geliştirilmiş özniteliği `age`, `glasses`, `facialHair`, `hair`, `makeup` öznitelikleri. Bunları aracılığıyla [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi. 

* Itanium tabanlı sistemler için artan giriş görüntü dosya boyutu sınırını 4 MB gelen 6 MB cinsinden için [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup Kişi - Ekle Yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ve [LargePersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Mart 2018 sürüm değişiklikleri

* Eklenen milyon ölçekli kapsayıcı: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) ve [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Daha ayrıntılı olarak [büyük ölçekli özelliğinin nasıl kullanılacağı](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Artan [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parametresinden [1, 5] için [1, 100] ve varsayılan 10.

### <a name="release-changes-in-may-2017"></a>Mayıs 2017 sürüm değişiklikleri

* Eklenen `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, ve `noise` öznitelikleri [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi.

* 10 K kişi bir PersonGroup desteklenen ve [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Sayfa numaralandırma içinde desteklenen [PersonGroup kişi - liste](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) isteğe bağlı parametreler ile: `start` ve `top`.

* Farklı FaceLists ve PersonGroup farklı kişilerin karşı yüzeyleri ekleme/silme desteklenen eşzamanlılık.

### <a name="release-changes-in-march-2017"></a>Mart 2017 sürüm değişiklikleri
* Eklenen `emotion` özniteliğini [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parametresi.

* Sabit yüz değil kaldırılmasına döndürülen dikdörtgen [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) olarak `targetFace` içinde [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ve [PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Kesinlikle 36 x 36 için 4096 x 4096 piksel arasında olduğundan emin olmak için algılanabilir yüz boyutu sabit.

### <a name="release-changes-in-november-2016"></a>Kasım 2016 sürüm değişiklikleri
* Eklenen ek kalıcı yüzeyleri kullanırken depolamak için yüz depolama Standart abonelik [PersonGroup kişi - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) veya [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) kimliği veya benzerlik eşleştirme için. Saklı görüntüleri 0,5 başına 1000 yüzeyleri adresindeki ücretlendirilen ve bu oran günlük olarak eşit olarak bölünür. Ücretsiz katmanı abonelikleri 1.000 toplam kişilere sınırlı devam etmektedir.

### <a name="release-changes-in-october-2016"></a>Ekim 2016 sürüm değişiklikleri
* Hata iletisi 'Vardır birden fazla yüz görüntüde' 'Var. birden fazla yüz görüntüde' den targetFace içinde birden fazla yazıtipinin değiştirilecek [FaceList - eklemek yüz](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) ve [PersonGroup kişi - yüzEkle](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Temmuz 2016 sürüm değişiklikleri
* Kişi nesnesi kimlik doğrulamasına yüz desteklenen [yüz - doğrulayın](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* İsteğe bağlı eklenen `mode` iki çalışma modu seçimine etkinleştirme parametresi: `matchPerson` ve `matchFace` içinde [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve varsayılan `matchPerson`.

* İsteğe bağlı eklenen `confidenceThreshold` parametresi için bir nominal bir kişinin nesnesine ait olup eşiği ayarlamak kullanıcı [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* İsteğe bağlı eklenen `start` ve `top` parametrelerinde [PersonGroup - liste](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) başlangıç noktasını ve PersonGroups sayısı listesine belirtmek kullanıcı etkinleştirmek için.

### <a name="v10-changes-from-v0"></a>V0 V1.0 değişiklikler
* Güncelleştirilmiş hizmet kök uç noktasından ```https://westus.api.cognitive.microsoft.com/face/v0/``` için ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Uygulanan değişiklikleri: [yüz - algılamak](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [yüz - tanımlama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [yüz - bulma benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) ve [yüz - grup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* En az algılanabilir yüz boyutu 36 x 36 piksel olarak güncelleştirildi. Yazıtipleri 36 x 36 pikselden küçük algılanmaz.

* Yüz V0 PersonGroup ve kişi verileri kullanım dışı. Bu verilerin yüz V1.0 hizmete erişilemiyor.

* Yüz API V0 uç noktası, 30 Haziran 2016 tarihinde kullanım dışı.
