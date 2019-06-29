---
title: -Yüz tanıma API'si olan algılama modelini belirleme
titleSuffix: Azure Cognitive Services
description: Bu makale, Azure yüz tanıma API'sini uygulama ile kullanmak için hangi yüz algılama model seçmek nasıl gösterir.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: yluiu
ms.openlocfilehash: 037a059c95150314b6f85ea3eacdec0f6bb3d6c0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449022"
---
# <a name="specify-a-face-detection-model"></a>Yüz algılama modeli belirtme

Bu kılavuz Azure yüz tanıma API'si için bir yüz algılama modelini belirleme gösterir.

Yüz tanıma API'si, görüntülerdeki İnsan yüzlerini işlemleri için makine öğrenimi modelleri kullanır. Müşteri geri bildirimi ve araştırma gelişmeler göre bizim modelleri doğruluğunu artırmak devam ediyoruz ve biz Bu geliştirmeler model güncelleştirmelerini olarak sunun. Geliştiriciler, yüz algılama modelin hangi sürümü, kullanmak istediğiniz belirtmek için seçeneğiniz vardır; Bunlar, kullanım örnekleri için en uygun model seçebilirsiniz.

İçin okumaya devam belirli yüz işlemlerinde yüz algılama modelini belirleme konusunda bilgi edinin. Diğer bazı veri forma bir görüntü yüz dönüştürür her yüz tanıma API'si, yüz algılama kullanır.

En son modele kullanmanız gereken emin değilseniz, atlamak [farklı modellerin değerlendirmesi](#evaluate-different-models) yeni modeli değerlendirin ve geçerli veri kümenizi kullanarak sonuçları karşılaştırmak için bölüm.

## <a name="prerequisites"></a>Önkoşullar

Yapay ZEKA yüz algılama kavramı biliyor olmanız gerekir. Yüz algılama kavramsal kılavuz veya nasıl yapılır kılavuzunda değilseniz, bkz:

* [Yüz algılama kavramları](../concepts/face-detection.md)
* [Nasıl bir görüntüde yüz algılama](HowtoDetectFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Belirtilen model ile yüzleri algılayın

Yüz algılama, İnsan yüzlerini sınırlayıcı kutunun konumunu bulur ve onların visual yer işareti tanımlar. Yüz Tanıma'nın özellikleri ayıklar ve bunları daha sonra kullanmak için depolar [tanıma](../concepts/face-recognition.md) operations.

Kullanırken [Yüz tanıma - algılayın] API, model sürümüyle atayabilirsiniz `detectionModel` parametresi. Kullanılabilir değerler şunlardır:

* `detection_01`
* `detection_02`

İstek URL'sini [Yüz tanıma - algılayın] REST API şöyle görünür:

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel][&detectionModel]
&subscription-key=<Subscription key>`

İstemci kitaplığı kullanıyorsanız, değeri atayabilirsiniz `detectionModel` geçirerek uygun bir dize. API, atanmamış değiştirmeden bırakırsanız, varsayılan model sürümü kullanır (`detection_01`). Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, false, false, recognitionModel: "recognition_02", detectionModel: "detection_02");
```

## <a name="add-face-to-person-with-specified-model"></a>Yüz tanıma ile belirtilen model için Kişi Ekle

Yüz tanıma API'si, yüz verileri bir görüntüden ayıklamak ve ilişkilendirin bir **kişi** nesnesi aracılığıyla [PersonGroup kişi - yüz tanıma ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API. Bu API çağrısı içinde aynı şekilde olarak algılama modelini belirtebilirsiniz [Yüz tanıma - algılayın].

Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
// Create a PersonGroup and add a person with face detected by "detection_02" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_02");

string personId = (await faceClient.PersonGroupPerson.CreateAsync(personGroupId, "My Person Name")).PersonId;

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.PersonGroupPerson.AddFaceFromUrlAsync(personGroupId, personId, imageUrl, detectionModel: "detection_02");
```

Bu kod oluşturur bir **PersonGroup** kimlikli `mypersongroupid` ve ekler bir **kişi** ona. Bir yüz için bunu ekler sonra **kişi** kullanarak `detection_02` modeli. Belirtmezseniz *detectionModel* parametresi, API varsayılan model kullanacağı `detection_01`.

> [!NOTE]
> Tüm yüzlerini için aynı olan algılama modelini kullanmak zorunda kalmazsınız bir **kişi** nesne, gerekmez yeni yüzler tespit edilirken bir karşılaştırmak için aynı algılama modelini kullanmak bir **kişi** nesne ( içinde[Yüz - Belirleme] API, örneğin).

## <a name="add-face-to-facelist-with-specified-model"></a>Yüz tanıma ile belirtilen model için FaceList Ekle

Varolan bir yüz eklediğinizde algılama modeli de belirtebilirsiniz **FaceList** nesne. Aşağıdaki kod örneği için .NET istemci kitaplığı bakın.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_02");

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.FaceList.AddFaceFromUrlAsync(faceListId, imageUrl, detectionModel: "detection_02");
```

Bu kod oluşturur bir **FaceList** adlı `My face collection` ve bunu bir yüz ekler `detection_02` modeli. Belirtmezseniz *detectionModel* parametresi, API varsayılan model kullanacağı `detection_01`.

> [!NOTE]
> Tüm yüzlerini için aynı olan algılama modelini kullanmak zorunda kalmazsınız bir **FaceList** nesne, gerekmez yeni yüzler tespit edilirken bir karşılaştırmak için aynı algılama modelini kullanmak bir **FaceList** nesne.

## <a name="evaluate-different-models"></a>Farklı modellerin değerlendirmesi

Farklı görevler için farklı yüz algılama modeller iyileştirilir. Aşağıdaki tabloda farklar genel bir bakış için bkz.

|**detection_01**  |**detection_02**  |
|---------|---------|
|Tüm yüz algılama işlemleri için varsayılan seçimdir. | Mayıs 2019 ve isteğe bağlı olarak tüm yüz algılama işlemleri kullanılabilir yayımladı.
|Küçük, yan görünümü veya bulanık yüzler için optimize edilmemiş.  | Geliştirilmiş doğruluğuna küçük, yan görünümü ve bulanık yüzleri. |
|Algılama çağrısında belirtilen döndürür öznitelikleri (baş poz, yaş, duygu tanıma vb.) karşı karşıyadır. |  Yüz öznitelikleri döndürmez.     |
|Algılama çağrısında belirtilen döndürür, yer işareti karşı karşıyadır.   | Yüz tanıma yer işareti döndürmez.  |

En iyi yolu, performanslarını Karşılaştırılacak `detection_01` ve `detection_02` modeller, bir örnek veri kümesi üzerinde kullanılacak. Arama öneririz [Yüz tanıma - algılayın] görüntüleri, çeşitli API özellikle görüntüleri birden çok yüzü veya görmek zor, yüzleri her algılama modelini kullanma. Her model döndüren yüzlerin sayısı dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, farklı yüz tanıma API'leri ile kullanılacak algılama modelini belirtin öğrendiniz. Ardından, yüz algılama ile çalışmaya başlamak için bir Hızlı Başlangıç'ı izleyin.

* [(.NET SDK) bir görüntüde yüz algılama](../quickstarts/csharp-detect-sdk.md)

[Yüz tanıma - algılayın]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Yüz - Belirleme]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[FaceList - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc