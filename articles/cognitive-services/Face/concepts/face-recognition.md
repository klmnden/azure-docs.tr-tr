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
ms.openlocfilehash: fa38c492530cb8938e49bc15e13fdd39ed5b6f1c
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65890889"
---
# <a name="face-recognition-concepts"></a>Yüz tanıma kavramları

Bu makalede doğrulayın, benzer bulun, Grup ve tanımla yüz tanıma işlemleri ve temel alınan veri yapılarını kavramlarını açıklar. Genel anlamıyla tanıma benzer veya aynı kişiye ait olmadığını belirlemek için iki farklı yüz karşılaştırma işlemlerini açıklar.

## <a name="recognition-related-data-structures"></a>Tanıma ile ilgili veri yapıları

Tanıma işlemleri genellikle aşağıdaki veri yapılarını kullanın. Bu nesneler, bulutta depolanır ve kendi kimlik dizeleri tarafından başvurulabilir. Kimliği her zaman bir abonelikte benzersiz dizelerdir. Ad alanlarını çoğaltılabilir.

|Ad|Açıklama|
|:--|:--|
|DetectedFace| Bu tek yüz gösterim tarafından alınan [yüz algılama](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) işlemi. Kendi Kimliğini 24 saat oluşturulduktan sonra süresi dolar.|
|PersistedFace| DetectedFace nesneler FaceList veya kişi gibi bir gruba eklendiğinde PersistedFace nesneler haline gelir. Olabilirler [alınan](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) herhangi saat ve dolmasın.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) veya [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Bu veri yapısını PersistedFace nesnelerin çeşitli bir listedir. Bir FaceList benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi var.|
|[Kişi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Aynı kişiye ait PersistedFace nesnelerin bir listesini bu veri yapısıdır. Bu, benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi sahiptir.|
|[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) veya [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Bu veri yapısını, kişi nesnelerini çeşitli listesidir. Bu, benzersiz bir kimliği, adı dizesi ve isteğe bağlı olarak bir kullanıcı veri dizesi sahiptir. Bir PersonGroup olmalıdır [eğitilen](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) tanıma işlemlerinde kullanılmadan önce.|

## <a name="recognition-operations"></a>Tanıma işlemleri

Bu bölümde, dört tanıma işlemleri daha önce açıklanan veri yapılarını kullanma açıklanmaktadır. Her tanıma işleminin geniş bir açıklaması için bkz [genel bakış](../Overview.md).

### <a name="verify"></a>Doğrula

[Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) işlemi face ID DetectedFace veya PersistedFace ve başka bir face ID veya kişi nesnesini alır ve aynı kişiye ait olup olmadığını belirler. Bir kişi nesnesi içinde geçirin, performansı artırmak için bu kişiye ait olduğu bir PersonGroup isteğe bağlı olarak geçirebilirsiniz.

### <a name="find-similar"></a>Benzer bulun

[Bul benzer](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) işlemi face ID DetectedFace veya PersistedFace ve bir FaceList veya diğer yüz kimlikleri dizisi alır. Bir FaceList ile verilen yüze benzeyen yüzleri, daha küçük bir FaceList döndürür. Yüz tanıma kimlikleri dizisi ile daha küçük bir dizi benzer şekilde döndürür.

### <a name="group"></a>Grup

[Grubu](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) işlemi DetectedFace veya PersistedFace çeşitli yüz kimlikleri dizisi alır ve aynı kimlikleri gruplandırılmış birkaç küçük diziye döndürür. Her "grupları" dizi yüz benzer görünür kimliklerini içerir. Yüz tanıma kimlikleri tek "messyGroup" dizi içeren bulunduğu hiçbir benzerlikler için.

### <a name="identify"></a>Belirleme

[Tanımla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) işlemi bir veya birkaç yüz kimlikleri DetectedFace veya PersistedFace ve bir PersonGroup alır ve her yüz ait olabilir kişi nesnelerin bir listesini döndürür. Nesneleri tahmin olasılık değeri aday nesneler olarak sarmalanır ve kişi döndürdü.

## <a name="input-data"></a>Giriş verileri

Giriş görüntülerinizi en doğru tanıma sonuçları verdiğiniz emin olmak için aşağıdaki ipuçlarını kullanın:

* Desteklenen giriş görüntü biçimleri, JPEG, PNG, GIF (ilk çerçeve), BMP olacaktır.
* Resim dosyasının boyutu 4 MB'tan büyük olmamalıdır.
* Kişi nesneler oluşturduğunuzda, farklı türde açıları ve Aydınlatma özelliği fotoğraflar'ı kullanın.
* Bazı yüzleri teknik güçlükler nedeniyle gibi tanınmayabilir:
  * Fon ışığı aşırı aydınlatma, örneğin, ciddi görüntüler.
  * Birini veya ikisini gözler block engelleri.
  * Saç türü veya yanı sıra yüz artı farklılıkları.
  * Değişiklikler geçerlilik süresi nedeniyle yüz görünümü.
  * Extreme yüz ifadelerini.

## <a name="next-steps"></a>Sonraki adımlar

Yüz tanıma kavramlarına alışık olduğunuz, eğitilen PersonGroup karşı yüzleri tanımlayan bir komut dosyası yazmayı öğrenin.

* [Resimlerdeki yüzleri belirleyin](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)