---
title: Yüz tanıma kavramları
titleSuffix: Azure Cognitive Services
description: Yüz tanıma kavramlarını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: 8c0bf001c2bdbedaa041dbd10b5c7edb08eec4c6
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "64416006"
---
# <a name="face-recognition-concepts"></a>Yüz tanıma kavramları

Bu makalede, çeşitli yüz tanıma işlemlerini (doğrulama, bulun gruplama kimliği benzer) ve temel alınan veri yapılarını kavramlarını açıklar. Genel anlamıyla tanıma benzer olmaları durumunda belirlemek için iki farklı yüz karşılaştırılması işi tanımlar veya aynı kişiye ait.

## <a name="recognition-related-data-structures"></a>Tanıma ile ilgili veri yapıları

Tanıma işlemleri genellikle aşağıdaki veri yapılarını kullanın. Bu nesneler, bulutta depolanır ve kendi kimlik dizeleri tarafından başvurulabilir. Ad alanlarını çoğaltılabilir ancak kimliği bu nedenle her zaman bir abonelikte benzersiz dizelerdir.

|Ad|Açıklama|
|:--|:--|
|**DetectedFace**| Tarafından alınan bir tek yüz gösterimi budur [yüz algılama](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) işlemi. Kendi Kimliğini 24 saat oluşturulduktan sonra süresi dolar.|
|**PersistedFace**| Zaman **DetectedFace** nesneleri, bir gruba eklenir (gibi **FaceList** veya **kişi**), olduklarında **PersistedFace** olabilir nesneleri olması [alınan](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) herhangi saat ve süresinin sona ermediğinden.|
|**[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b)**/**[LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)**| Bu, çeşitli bir listesini **PersistedFace** nesneleri. A **FaceList** benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi.|
|**[Kişi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)**| Bu bir listesidir **PersistedFace** aynı kişiye ait nesneler. Bu, benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi sahiptir.|
|**[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)**/**[LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)**| Bu, çeşitli bir listesini **kişi** nesneleri. Bu, benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi sahiptir. A **PersonGroup** olmalıdır [eğitilen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) tanıma işlemlerinde kullanılmadan önce.|

## <a name="recognition-operations"></a>Tanıma işlemleri

Bu bölümde, dört tanıma işlemleri Yukarıdaki veri yapılarını kullanma açıklanmaktadır. Bkz: [genel bakış](../Overview.md) her tanıma işleminin ayrıntılı bir açıklaması için.

### <a name="verification"></a>Doğrulama

[Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) işlemi face ID alır (**DetectedFace** veya **PersistedFace**) ve başka bir ya da face ID veya **kişi** nesne ve aynı kişiye ait olup olmadığını belirler. İçinde başarılı olursa bir **kişi**, isteğe bağlı olarak, geçirebilirsiniz bir **PersonGroup** hangi, **kişi** performansını geliştirmek için ait.

### <a name="find-similar"></a>Benzer bulun

[Benzer Bul](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemi face ID alır (**DetectedFace** veya **PersistedFace**) ve ya da bir **FaceList** veya bir dizi diğer yüz tanıma Kimlikleri. İle bir **FaceList**, daha küçük döndürür **FaceList** verilen yüze benzeyen yüzleri biri. Yüz tanıma kimlikleri dizisi ile daha küçük bir dizi benzer şekilde döndürür.

### <a name="grouping"></a>Gruplama

[Grubu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) işlemi çeşitli yüz kimlikleri dizisi alır (**DetectedFace** veya **PersistedFace**) ve aynı kimlikleri gruplandırılmış birkaç küçük diziye döndürür. Her "grupları" dizi yüz benzer görünür kimlikleri ve yüz kimlikleri tek "messyGroup" dizi içerir, hiçbir benzerlikler bulunamadı için.

### <a name="identification"></a>Tanımlama

[Tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) işlemi bir veya birkaç yüz kimliği alır (**DetectedFace** veya **PersistedFace**) ve bir **PersonGroup** ve listesini döndürür ' ın **kişi** her yüz ait olabilir nesneleri. Döndürülen **kişi** nesneleri olarak sarılır **aday** tahmin olasılık değeri olan nesneler.

## <a name="input-data"></a>Giriş verileri

Giriş görüntülerinizi en doğru tanıma sonuçları verdiğiniz emin olmak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş görüntü biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP olacaktır.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır.
* Oluştururken **kişi** nesneler, çeşitli açıları ve Aydınlatma özelliği fotoğraf kullanmalıdır.
* Teknik güçlükler nedeniyle bazı yüzleri tanınmayabilir:
  * (Örneğin, ciddi Arka Aydınlatma) aşırı aydınlatma ile görüntüler.
  * Engelleri birini veya ikisini gözler engelleme.
  * Saç stil veya yanı sıra yüz artı farklılıkları.
  * Yüz tanıma görünüm yaş nedeniyle değişiklikler.
  * Extreme yüz ifadelerini.

## <a name="next-steps"></a>Sonraki adımlar

Yüz tanıma kavramlarına alışık olduğunuz, eğitilen bir karşı yüzleri tanımlayan basit bir komut dosyası yazmayı öğrenin **PersonGroup**.

* [Resimlerde yüz tanımlama](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)