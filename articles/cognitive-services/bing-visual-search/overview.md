---
title: Bing Görsel Arama nedir?
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama bir resimle ilgili olarak benzer resimler veya alışveriş kaynakları gibi ayrıntılar veya içgörüler sağlar.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 61a851b0efbcc4fdb55308e47447d218014ef9e0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154363"
---
# <a name="what-is-the-bing-visual-search-api"></a>Bing Görsel Arama API’si nedir?

Bing Görsel Arama API'si, Bing.com/images sayfasında gösterilenlere benzer resim ayrıntıları sağlar. Bir görüntü yüklemek veya bir URL sağlamak, bu API hakkındaki ayrıntıları görsel açıdan benzer resimler dahil olmak üzere çeşitli kaynakları, görüntü ve daha fazlasını içeren Web sayfalarını alışveriş tanımlayabilirsiniz. Kullanırsanız [Bing resim arama API'si](../bing-image-search/overview.md), bir görüntü yüklemek yerine API'nin arama sonuçlarına iliştirilmiş Insight belirteçleri kullanabilirsiniz.

## <a name="insights"></a>Insights

Görsel arama bulmanıza olanak tanır ınsights şunlardır:

| İçgörü                              | Açıklama |
|--------------------------------------|-------------|
| Görsel olarak benzer resimler              | Giriş görüntünün görsel olarak benzer resimler listesi. |
| Görsel olarak benzer ürünler            | Gösterilen ürün için görsel olarak benzer ürünleri.            |
| Alışveriş kaynakları                     | Yerleri burada giriş görüntüde verilen öğe satın alabilirsiniz.            |
| İlgili aramalar                     | İlgili aramalar diğerlerinden veya, tarafından yapılan, resmin içeriğini temel temel alır.            |
| Görüntü içeren Web sayfaları     | Girdi görüntüsünün dahil Web sayfaları.            |
| Tarifleri                              | Giriş görüntüde verilen tabağın için tarif içeren Web sayfaları            |

Bu içgörülere ek olarak, Görsel Arama giriş resminden türetilen çeşitli terimler (etiketler) de döndürür. Bu etiketler kullanıcıların resimde bulunan kavramları incelemesine olanak tanır. Örneğin, giriş resmi ünlü bir sporcuya aitse, etiketlerden biri sporcunun adı ve bir diğeri de Spor olabilir. Öte yandan giriş resminde elmalı tart gösteriliyorsa, etiketler Elmalı Tart, Tart, Tatlılar olabilir ve bu sayede kullanıcılar ilgili kavramları inceleyebilir.

Görsel Arama sonuçları, resimdeki ilgi çekici bölgeler için sınırlayıcı kutular da içerir. Örneğin resimde birkaç ünlü görünüyorsa, sonuçlar resimde tanınan ünlülerden her biri için sınırlayıcı kutular içerebilir. Öte yandan Bing resimde bir ürün veya giysi bulunduğunu tanırsa, sonuç tanınan ürünün veya giysi parçası için sınırlayıcı kutu içerebilir.

> [!IMPORTANT]
> Bing resim arama API'si kullanarak resim öngörüleri alırsanız, Bing görsel arama daha kapsamlı içgörüler sağlayan API'sine, değiştirmeyi göz önünde bulundurun.

## <a name="workflow"></a>İş akışı

Bing görsel arama API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir. REST API veya SDK'sını kullanarak hizmetini kullanabilirsiniz.

1. Bing arama API'leri erişimi olan bir Bilişsel hizmetler API hesabı oluşturma. Azure aboneliğiniz yoksa, ücretsiz bir hesap oluşturabilirsiniz.
2. Geçerli bir arama sorgusuyla API'sine bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.


## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, Bing Resim Arama API'sinin [etkileşimli tanıtımını](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/) deneyin.
Bu tanırım arama sorgusunu hızlı bir şekilde özelleştirmeyi ve web'de resim aramayı göstermektedir.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

İlk isteğinizi hızlıca başlamak için hızlı başlangıçları bakın: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).


## <a name="see-also"></a>Ayrıca bkz.

* [Bing görsel Arama başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) belge tanımları ve uç noktaları, üst bilgileri, API yanıtları ve görüntü tabanlı arama sonuçları istemek için kullanabileceğiniz sorgu parametreleri hakkında bilgi içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-and-display-requirements.md) konusunda belirtilmektedir.
