---
title: Bing Otomatik Öneri nedir?
titlesuffix: Azure Cognitive Services
description: Bing Otomatik Öneri API’sini kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: 24f35d795b34e7d9c214a23c040791841b4a711b
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66382566"
---
# <a name="what-is-bing-autosuggest"></a>Bing Otomatik Öneri nedir?

Uygulamanızın tüm Bing arama API'leri sorguları gönderirse, Bing otomatik öneri API'si kullanıcılarınızın arama deneyimini geliştirmek için kullanabilirsiniz. Bing otomatik öneri API'si, bir arama kutusu kısmi sorgu dizesine göre önerilen sorgular listesi döndürür. Arama kutusuna girilen karakterler gibi aşağı açılan listede önerileri görüntüleyebilirsiniz.

## <a name="bing-autosuggest-api-features"></a>Bing otomatik öneri API'si özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/get-suggestions.md) | Bunlar yazıldığı gibi önerilen arama terimlerini görüntülemek için otomatik öneri API'si kullanarak uygulama deneyiminizi geliştirin. |

## <a name="workflow"></a>İş akışı

Bing otomatik öneri API'si bir RESTful web HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağırmak kolay hizmettir. 

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Bir istek, bir kullanıcı, uygulamanızın arama kutusuna yeni karakter türleri her zaman bu API'ye gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

Genellikle, her seferinde kullanıcı, uygulamanızın arama kutusuna yeni karakter türleri bu API çağrısıyla sonlandırmalısınız. Daha fazla karakter girdiniz gibi API'nin daha ilgili önerilen arama sorguları döndürür. Öneriler API'si için tek bir örnek döndürebilir `s` için daha az ilgili olasılığı `sail`.

Aşağıdaki örnek, Bing otomatik öneri API'si bir açılan arama kutusuna önerilen sorgu terimleri gösterir.

![Otomatik öneri açılır arama kutusu listesi](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Bir kullanıcının açılan listeden bir öneri seçtiğinde, Bing arama API'leri biriyle aramaya başlanacak kullanın veya doğrudan Bing arama sonuçları sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizi hızlı bir şekilde başlatmak için bkz. [İlk Sorgunuzu Yapma](quickstarts/csharp.md).

[Bing Otomatik Öneri API’si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference) başvurusunu inceleyin. Başvuruda, önerilen sorgu terimlerini istemek için kullanacağınız uç noktaların, üst bilgilerin ve sorgu parametrelerinin listesinin yanı sıra yanıt nesnelerinin tanımları yer alır.

[Bing Web Araması API](../bing-web-search/search-the-web.md)’sini kullanarak web’de arama yapmayı öğrenin.

Arama sonuçlarını kullanma kurallarına uygun hareket ettiğinizden emin olmak için [Bing Kullanım ve Görüntüleme Gereksinimleri](./useanddisplayrequirements.md)'ni okumayı unutmayın.
