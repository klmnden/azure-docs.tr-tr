---
title: Bing Görsel Arama API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama bir resimle ilgili olarak benzer resimler veya alışveriş kaynakları gibi ayrıntılar veya içgörüler sağlar.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 03/27/2019
ms.author: scottwhi
ms.openlocfilehash: 8bcb0372ebb60ac3a46cf06bf85322b288e153ba
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630216"
---
# <a name="what-is-the-bing-visual-search-api"></a>Bing Görsel Arama API’si nedir?

Bing görsel arama API'si, bir görüntü için ınsights döndürür. Bir görüntüyü karşıya yükleme veya bir URL sağlayın. Insights olan görsel açıdan benzer resimler, alışveriş kaynakları, görüntü ve daha fazlasını içeren Web sayfaları. Bing görsel arama API'si tarafından döndürülen ınsights olanları Bing.com/images üzerinde gösterilen benzerdir.

Kullanırsanız [Bing resim arama API'si](../bing-image-search/overview.md), bir görüntü yüklemek yerine, Bing görsel arama için API'nin Arama sonuçlarından Insight belirteçleri kullanabilirsiniz.

> [!IMPORTANT]
> Bing resim arama API'si kullanarak resim öngörüleri alırsanız, Bing görsel arama daha kapsamlı içgörüler sağlayan API'sine, değiştirmeyi göz önünde bulundurun.

## <a name="insights"></a>Öngörüler

Bing görsel arama'yı kullanarak aşağıdaki Öngörüler bulabilir:

| İçgörü                              | Açıklama |
|--------------------------------------|-------------|
| Görsel olarak benzer resimler              | Giriş görüntünün görsel olarak benzer resimler listesi. |
| Görsel olarak benzer ürünler            | Gösterilen ürün için görsel olarak benzer ürünleri.            |
| Alışveriş kaynakları                     | Yerleri burada giriş görüntüde verilen öğe satın alabilirsiniz.            |
| İlgili aramalar                     | İlgili aramalar diğerlerinden veya, tarafından yapılan, resmin içeriğini temel temel alır.            |
| Görüntü içeren Web sayfaları     | Girdi görüntüsünün dahil Web sayfaları.            |
| Tarifleri                              | Giriş görüntüde verilen tabağın için tarif içeren Web sayfaları.            |

Insights yanı sıra, Bing görsel arama terimleri (diğer bir deyişle, etiketler) giriş görüntüden türetilmiş çeşitli döndürür. Etiketleri görüntüde bulunan kavramlarını keşfedin olanak tanıyın. Örneğin, bir ünlü athlete girdi görüntüsünün ise etiketlerinden birini athlete adını olabilir, spor başka bir etiket olabilir. Veya, girdi görüntüsünün bir apple pasta ise, etiketler Apple Pasta ve pasta Desserts olabilir.

Bing görsel arama sonuçları, sınırlayıcı kutular bölgeler için görüntüde faiz de içerir. Örneğin, görüntüyü birkaç ünlüleri içeriyorsa, sonuçları her tanınan ünlüleri için sınırlayıcı kutular içerebilir. Veya bir ürün veya görüntüde giysi Bing tanır, sonuç tanınmış öğe için bir sınırlayıcı kutu içerebilir.

## <a name="workflow"></a>İş akışı

Bing görsel arama API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir. REST API veya SDK hizmeti için kullanabilirsiniz.

1. Oluşturma bir [Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) Bing arama API'lerine erişmek için. Azure aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/). Abonelik anahtarınızı alabilirsiniz [Azure portalında](https://docs.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account#access-your-resource) hesabınızı oluşturduktan sonra veya [Azure Web sitesi](https://azure.microsoft.com/try/cognitive-services/my-apis) sonra ücretsiz deneme sürümü etkinleştiriliyor.
2. Geçerli bir arama sorgusuyla API'sine bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing görsel arama API'sini deneyin [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/).
Tanıtımın nasıl hızlı bir şekilde bir arama sorgusu özelleştirebilir ve Web'in gösterir.

İlk isteğinizi hızlıca başlamak için hızlı başlangıçları bakın: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Görüntüleri - görsel arama](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) istek üst, yanıtları, başvuru tanımları ve uç noktaları hakkında bilgi açıklar ve istek görüntü tabanlı kullanabileceğiniz sorgu parametreleri arama sonuçları.

* [Bing arama API'si kullanın ve gereksinimlerini görüntülemek](../bing-web-search/use-display-requirements.md) içerik ve Bing arama API'leri elde edilen bilgilerden kabul edilebilir kullanımlarını belirtin.
