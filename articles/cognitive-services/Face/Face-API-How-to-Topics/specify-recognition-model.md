---
title: Tanıma modeli - yüz tanıma API'si belirtme
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure yüz tanıma API'sini uygulama ile kullanmak için hangi tanıma model seçmek nasıl gösterir.
services: cognitive-services
author: longl
manager: nitinme
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: longl
ms.openlocfilehash: 88b0ac853c64e1e32a2d1c429bdf8655158f030d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65411459"
---
# <a name="specify-a-face-recognition-model"></a>Yüz tanıma modeli belirtme

Bu kılavuz bir yüz tanıma modeli yüz algılama, tanımlama ve Azure yüz tanıma API'sini kullanarak benzerlik arama belirtmek nasıl gösterir.

Yüz tanıma API'si, görüntülerdeki İnsan yüzlerini işlemleri için makine öğrenimi modelleri kullanır. Müşteri geri bildirimi ve araştırma gelişmeler göre bizim modelleri doğruluğunu artırmak devam ediyoruz ve biz Bu geliştirmeler model güncelleştirmelerini olarak sunun. Geliştiriciler, yüz tanıma modelin hangi sürümü, kullanmak istediğiniz belirtmek için seçeneğiniz vardır; Bunlar, kullanım örnekleri için en uygun model seçebilirsiniz.

Yeni bir kullanıcıysanız, en son modele kullanmanızı öneririz. İçin okumaya devam modeli çakışmaları kaçınarak farklı yüz işlemlerinde belirtin öğrenin. İleri düzey bir kullanıcıysanız ve en son modele geçiş yapmamalıdır emin değilseniz, atlamak [farklı modellerin değerlendirmesi](#evaluate-different-models) yeni modeli değerlendirin ve geçerli veri kümenizi kullanarak sonuçları karşılaştırmak için bölüm.

## <a name="prerequisites"></a>Önkoşullar

Yapay ZEKA yüz algılama ve kimlik kavramlarla ilgili bilgi sahibi olması gerekir. Değilseniz, bu nasıl yapılır kılavuzlarından ilk bakın:

* [Nasıl bir görüntüde yüz algılama](HowtoDetectFacesinImage.md)
* [Görüntüdeki yüzleri belirleme](HowtoIdentifyFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Belirtilen model ile yüzleri algılayın

Yüz algılama, İnsan yüzlerini, görsel işaretleri tanımlar ve sınırlayıcı kutu konumlarına bulur. Ayrıca, yüzünün özellikleri ayıklar ve bunları tanımlama bilgisinde saklar. Tüm bu bilgileri bir yüz temsilini oluşturur.

Yüz tanıma özellikleri ayıklandığında tanıma modeli Algıla işlemi gerçekleştirirken bir model sürümü belirtmek için kullanılır.

Kullanırken [Yüz tanıma - algılayın] API, model sürümüyle atama `recognitionModel` parametresi. Kullanılabilir değerler şunlardır:

* `recognition_01`
* `recognition_02`

İsteğe bağlı olarak belirtebilirsiniz _returnRecognitionModel_ parametre (varsayılan **false**) göstermek için olup olmadığını _recognitionModel_ yanıtta döndürülmesi. Bunu, bir istek URL'sini [Yüz tanıma - algılayın] REST API şöyle görünür:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel]
&subscription-key=<Subscription key>`

İstemci kitaplığı kullanıyorsanız, değeri atayabilirsiniz `recognitionModel` sürümünü temsil eden bir dize geçirerek.
Bunu atanmamış, varsayılan model sürüm bırakırsanız (_recognition_01_) kullanılacak. Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.Face.DetectWithUrlAsync(imageUrl, true, true, recognitionModel: "recognition_02", returnRecognitionModel: true);
```

## <a name="identify-faces-with-specified-model"></a>Belirtilen model ile yüzleri belirleyin

Yüz tanıma API'si, yüz verileri bir görüntüden ayıklamak ve ilişkilendirin bir **kişi** nesne (aracılığıyla [yüz ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API çağrısı, örneğin) ve birden çok **kişi** nesneler olabilir. birlikte depolanan bir **PersonGroup**. Ardından, yeni bir yüz karşı karşılaştırılabilir bir **PersonGroup** (ile [Yüz - Belirleme] çağrısı), ve o gruptaki eşleşen kişi tanımlanabilir.

A **PersonGroup** bir benzersiz tanıma modeli, tüm için olmalıdır **kişi**s ve belirtebileceğiniz bu kullanarak `recognitionModel` grubu oluşturduğunuzda parametre ([PersonGroup - Oluşturma] veya [LargePersonGroup - oluşturma]). Bu parametre, özgün belirtmezseniz `recognition_01` modeli kullanılır. Bir grup her zaman oluşturulduğu tanıma modeli kullanır ve bunlar için eklenen yeni yüzler bu modeli ile ilişkili olur; Bu, bir grup oluşturulduktan sonra değiştirilemez. Hangi modelini görmek için bir **PersonGroup** yapılandırılmış birlikte kullanmak [PersonGroup - Get] API'SİYLE _returnRecognitionModel_ parametre olarak **true**.

Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
// Create an empty PersonGroup with "recognition_02" model
string personGroupId = "mypersongroupid";
await faceServiceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");
```

Bu kodda, bir **PersonGroup** kimlikli `mypersongroupid` oluşturulur, ve bunu kullanmak için ayarlanan _recognition_02_ yüz özelliklerini ayıklamak için model.

Yüzleri algılama buna karşı karşılaştırmak için kullanılacak hangi modelle belirtmenize gerek gelenlere, **PersonGroup** (aracılığıyla [Yüz tanıma - algılayın] API). Modeli kullandığınız her zaman ile tutarlı olmalıdır **PersonGroup**'s yapılandırma; Aksi takdirde işlemi uyumsuz modeller nedeniyle başarısız olur.

Hiçbir değişiklik [Yüz - Belirleme] API; yalnızca model sürümü algılama belirtmeniz gerekir.

## <a name="find-similar-faces-with-specified-model"></a>Belirtilen model ile benzer yüzleri bulun

Benzerlik arama tanıma modeli de belirtebilirsiniz. Model sürümüyle atayabilirsiniz `recognitionModel` yüz listesiyle oluştururken [FaceList - oluşturma] API veya [LargeFaceList - oluşturma]. Bu parametre, özgün belirtmezseniz `recognition_01` modeli kullanılır. Yüz tanıma listesini her zaman oluşturulduğu tanıma modeli kullanır ve için eklenen yeni yüzler bu modeli ile ilişkili olur; Bu, oluşturulduktan sonra değiştirilemez. Hangi modeli ile yapılandırılmış yüz listesini görmek için [FaceList - Get] API'SİYLE _returnRecognitionModel_ parametre olarak **true**.

Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
await faceServiceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");
```

Bu kod olarak adlandırılan bir yüz listesi oluşturur ve `My face collection`kullanarak _recognition_02_ özelliği ayıklama için model. Bu yüz listesi için yeni bir algılanan yüz benzer yüzleri için arama yaparken, yüz algılandı gerekir ([Yüz tanıma - algılayın]) kullanarak _recognition_02_ modeli. Önceki bölümde gösterildiği gibi modelin tutarlı olması gerekir.

Hiçbir değişiklik [Yüz tanıma - benzer Bul] API; yalnızca algılama modeli sürümü belirtin.

## <a name="verify-faces-with-specified-model"></a>Belirtilen model ile yüzleri doğrulayın

[Yüz tanıma - doğrulama] API, iki yüzün aynı insana ait olup olmadığını denetler. Tanıma modelleri onaylamaz doğrulayın API'sindeki herhangi bir değişiklik olmaz ancak yalnızca aynı modeliyle algılanan yüzeylere karşılaştırabilirsiniz. Bu nedenle, bu iki yüz her ikisini de kullanarak algılanan gerekecektir `recognition_01` veya `recognition_02`.

## <a name="evaluate-different-models"></a>Farklı modellerin değerlendirmesi

Performanslarını, karşılaştırmak istiyorsanız _recognition_01_ ve _recognition_02_ modellerini verilerinize gerekir:

1. İki **PersonGroup**s ile _recognition_01_ ve _recognition_02_ sırasıyla.
1. Yüz algılama ve onlara kaydetmek için görüntü verilerinizi kullanın **kişi**s için bu iki **PersonGroup**s ve eğitim tetikleyici işlem ile [PersonGroup - Eğitme] API.
1. Test [Yüz - Belirleme] hem de **PersonGroup**s ve sonuçlarını karşılaştırın.

Normalde bir olasılık eşiği (sıfır ve modelin bir yüz belirlemek için nasıl emin olmalıdır belirleyen bir arasında bir değer) belirtirseniz, farklı modelleri için farklı eşikler kullanmak gerekebilir. Bir model için bir eşik diğerine paylaşılmak üzere tasarlanmamıştır ve mutlaka aynı sonuçları oluşturmaz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı yüz hizmet API'leri ile kullanılacak tanıma modelini belirtin öğrendiniz. Ardından, yüz algılama ile çalışmaya başlamak için bir Hızlı Başlangıç'ı izleyin.

* [Görüntüdeki yüzleri algılayın](../quickstarts/csharp-detect-sdk.md)

[Yüz tanıma - algılayın]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Yüz tanıma - benzer Bul]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Yüz - Belirleme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Yüz tanıma - doğrulama]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Eğitme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[LargeFaceList - oluşturma]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc
