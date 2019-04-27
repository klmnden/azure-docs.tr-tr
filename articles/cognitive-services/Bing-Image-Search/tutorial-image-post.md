---
title: "Öğretici: Bing resim arama API'si kullanarak görüntü ayrıntılarını ayıklayın veC#"
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API’sini kullanarak görüntü ayrıntılarını ayıklayan bir C# uygulaması oluşturmak için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: tutorial
ms.date: 9/14/2018
ms.author: aahi
ms.openlocfilehash: 567a220ce9adcb3cca9c70cef6a7058a85789eb7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809283"
---
# <a name="tutorial-extract-image-details-using-the-bing-image-search-api-and-c"></a>Öğretici: Bing resim arama API'si kullanarak görüntü ayrıntılarını ayıklayın veC#

Bing Resim Arama API'si aracılığıyla kullanılabilir olan birden çok [uç nokta](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-endpoint) vardır. `/details` uç noktası bir görüntü içeren POST isteğini kabul eder ve görüntüyle ilgili çeşitli ayrıntılar döndürebilir. Bu C# uygulaması, bu API’yi kullanarak bir görüntü gönderir ve aşağıda örnekleri verilen JSON nesneleri olan, Bing tarafından döndürülen ayrıntıları görüntüler:

![[JSON sonuçları]](media/cognitive-services-bing-images-api/jsonResult.jpg)

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * `POST` isteğinde Resim Arama `/details` uç noktasını kullanma
> * İstek için üst bilgileri belirtme
> * Sonuçları belirtmek için URL parametrelerini kullanma
> * Görüntü verilerini karşıya yükleme ve `POST` isteği gönderme
> * JSON sonuçlarını konsolda yazdırma

Bu örneğin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/BingGetSimilarImages.cs)’da mevcuttur.

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)’nin herhangi bir sürümü.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="construct-an-image-details-search-request"></a>Görüntü ayrıntıları arama isteği oluşturma

Aşağıda, isteğin gövdesinde görüntü verilerinin yer aldığı POST isteklerini kabul eden `/details` uç noktası verilmiştir.
```
https://api.cognitive.microsoft.com/bing/v7.0/images/details
```

Arama isteği URL’si oluşturulurken `modules` parametresi yukarıdaki uç noktayı izler ve sonuçların içereceği ayrıntı türlerini belirtir:

* `modules=All`
* `modules=RecognizedEntities` (görüntüde görünen kişiler veya yerler)

Aşağıdakileri içeren JSON metnini almak için POST isteğinde `modules=All` değerini belirtin:

* `bestRepresentativeQuery` - karşıya yüklenen görüntüye benzer görüntüleri döndüren bir Bing sorgusu
* `detectedObjects` - görüntüde bulunan nesneler
* `image` - görüntünün meta verileri
* `imageInsightsToken` - görüntüden `RecognizedEntities` (görüntüde görünen kişiler veya yerler) alan sonraki GET istekleri için bir belirteç.
* `imageTags` - görüntünün etiketleri
* `pagesIncluding` - görüntüyü içeren web sayfaları
* `relatedSearches` - görüntüdeki ayrıntılara dayalı aramalar.
* `visuallySimilarImages` - web üzerindeki benzer görüntüler.

Yalnızca, görüntüdeki kişileri veya yerleri belirlemek için sonraki GET isteğinde kullanılabilecek `imageInsightsToken` öğesini almak için POST isteğinde `modules=RecognizedEntities` değerini belirtin.

## <a name="create-a-webclient-object-and-set-headers-for-the-api-request"></a>WebClient nesnesi oluşturma ve API isteği için üst bilgileri ayarlama

Bir `WebClient` nesnesi oluşturun ve üst bilgileri ayarlayın. Bing Arama API’sine yönelik tüm istekler için bir `Ocp-Apim-Subscription-Key` gerekir. Görüntü karşıya yüklemeye ilişkin `POST` isteği, `ContentType: multipart/form-data` değerini de belirtmelidir.

```javascript
WebClient client = new WebClient();
client.Headers["Ocp-Apim-Subscription-Key"] = accessKey;
client.Headers["ContentType"] = "multipart/form-data";
```

## <a name="upload-the-image-and-display-the-results"></a>Görüntüyü karşıya yükleme ve sonuçları görüntüleme

`WebClient` sınıfının `UpLoadFile()` yöntemi, `RequestStream` öğesinin biçimlendirilmesi ve `HttpWebRequest` çağrısı da dahil olmak üzere `POST` isteği için verileri biçimlendirir.

`/details` uç noktası ve karşıya yüklenecek görüntü dosyası ile `WebClient.UpLoadFile()` çağrısı yapın. `SearchResult` yapısı örneğini başlatmak ve yanıtı depolamak için JSON yanıtını kullanın.

```javascript        
const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/details";
// The image to upload. Replace with your file and path.
const string imageFile = "your-image.jpg";
byte[] resp = client.UploadFile(uriBase + "?modules=All", imageFile);
var json = System.Text.Encoding.Default.GetString(resp);
// Create result object for return
var searchResult = new SearchResult()
{
    jsonResult = json,
    relevantHeaders = new Dictionary<String, String>()
};
```
Bu JSON yanıtı daha sonra konsola yazdırılabilir.

## <a name="use-an-image-insights-token-in-a-request"></a>İstekte görüntü öngörüleri belirtecini kullanma

`POST` sonuçlarıyla birlikte döndürülen `ImageInsightsToken` öğesini kullanmak için bunu bir `GET` isteğine ekleyebilirsiniz. Örneğin:

```
https://api.cognitive.microsoft.com/bing/v7.0/images/details?InsightsToken="bcid_A2C4BB81AA2C9EF8E049C5933C546449*ccid_osS7gaos*mid_BF7CC4FC4A882A3C3D56E644685BFF7B8BACEAF2
```

Görüntüde tanımlanabilir kişiler veya yerler varsa bu istek bunlarla ilgili bilgileri döndürür.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı bir Web uygulamasında görüntüleri ve arama seçeneklerini görüntüleme ](tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Resim Arama API’si başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
