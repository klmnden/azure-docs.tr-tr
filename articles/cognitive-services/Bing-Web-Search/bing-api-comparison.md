---
title: Bing arama API'leri nelerdir?
titleSuffix: Azure Cognitive Services
description: Bing arama API'leri ve, uygulamalarınız ve hizmetlerinizdeki bilişsel alanlarınıza nasıl olanak sağlayabileceğiniz hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 03/12/2019
ms.author: aahi
ms.openlocfilehash: 5a883fcb3533374afbbf946281b6a4a1e9a2912e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61431370"
---
# <a name="what-are-the-bing-search-apis"></a>Bing arama API'leri nelerdir?

Bing arama API'leri, web bağlantılı uygulamalar ve Web sayfaları, görüntüler, haber, konumları ve tanıtımları olmadan daha fazla bilgi bulmak services oluşturmanıza olanak tanır. Bing arama REST API veya SDK'ları kullanarak arama istekleri gönderdiğinizde, web aramaları için ilgili bilgiler ve içerik alabilirsiniz. Farklı Bing arama API'leri ve uygulamalarınızı ve hizmetlerinizi bilişsel arama nasıl tümleştirebilirsiniz hakkında bilgi edinmek için bu makaleyi kullanın. Fiyatlandırma ve oran sınırlarını API'leri arasında değişiklik gösterebilir.

## <a name="the-bing-web-search-api"></a>Bing Web araması API'si

[Bing Web araması API'si](../Bing-Web-Search/index.yml) Web sayfaları, görüntüler, video, haber ve daha fazla döndürür. Dahil etmek veya belirli içerik türlerini dışarıda bırakmak için bu API'ye gönderilen arama sorguları filtreleyebilirsiniz.

Bing Web araması API'si tüm ilgili web içeriği türleri için arama gereken uygulamalar kullanmayı düşünün. Uygulamanızı belirli bir çevrimiçi içerik türü için arama, arama API'leri aşağıdaki birini göz önünde bulundurun:

## <a name="content-specific-bing-search-apis"></a>İçerik özgü Bing arama API'leri

Aşağıdaki Bing arama API'leri, resimler, Haberler, yerel işletmeler ve videolar gibi Web belirli içerik döndürür.

| Bing API'si | Açıklama |
| -- | -- |
| [Varlık arama](../Bing-Entities-Search/index.yml) | Bing varlık arama API'si, varlıklar, kişiler, yerler veya noktalar içeren arama sonuçlarını döndürür. Sorguya bağlı olarak, arama sorgusu için uygun bir veya daha fazla varlık API döndürür. Arama sorgusu, kayda değer kişiler, yerel işletmeler, yer işareti, hedefler ve daha fazlasını içerebilir. |
| [Resim arama](../Bing-Image-Search/index.yml) | Bing resim arama API'si arama ve bulma yüksek kaliteli statik ve animasyonlu görüntüleri benzer sağlar [Bing.com/images](https://www.Bing.com/images). Eklemek veya görüntü boyutu, renk, lisans ve güncellik gibi özniteliği tarafından dışlamak için arama iyileştirebilirsiniz. Popüler resimler için arama yapın, bunlar hakkında Öngörüler elde etmek için görüntüleri karşıya yükleme ve küçük resim önizlemeleri görüntüler. |
| [Haber arama](../Bing-News-Search/index.yml) | Bing haber arama API'si benzer haberleri bulmanıza olanak tanır [Bing.com/news](https://www.Bing.com/news). API, birden çok kaynakları veya özel etki alanları haber makalelerini döndürür. Popüler makaleler, Haberler ve başlıkları almak için kategoriler arasında arama yapabilirsiniz. |
| [Video arama](../Bing-Video-Search/index.yml) | Bing Video arama API'si, Web'de video bulun sağlar. Popüler videolar ve ilgili içerik küçük resim önizlemeleri alın. |
| [Görsel arama](../Bing-visual-search/index.yml) | Bir görüntüyü karşıya yükleme veya görsel olarak benzer ürünler, görüntüler ve ilgili aramalar gibi hakkında ayrıntılı bilgi almak için bir URL kullanın. |
 [Yerel işletme arama](../bing-local-business-search/index.yml) | Bing yerel iş arama API uygulamalarınızı başvurun ve konum arama sorgularına dayalı yerel işletmeler hakkında bilgi sağlar. |

## <a name="the-bing-custom-search-api"></a>Bing özel arama API'si

Bir özel arama örneği ile [Bing özel arama](../Bing-Custom-Search/index.yml) API'si, yalnızca içerik ve, önem verdiğiniz konulara odaklanan bir arama deneyimi oluşturmanıza olanak tanır. Örneğin, etki alanı, Web siteleri ve Bing arama yapacağı belirli Web sayfalarını belirttikten sonra Bing özel arama sonuçları, belirli bir içeriğe uyarlayın. Bing özel otomatik öneri, görüntü, birleştirmek ve Video arama API'leri, aramanızı daha fazla özelleştirmek için deneyimi.

## <a name="additional-bing-search-apis"></a>Ek Bing arama API'leri

Aşağıdaki Bing arama API'leri diğer Bing arama API'leri ile birleştirerek, arama deneyimini geliştirmenize olanak tanır.

| API | Açıklama |
| -- | -- |
| [Bing Otomatik Öneri](../Bing-Autosuggest/index.yml) | Gerçek zamanlı olarak önerilen aramalar döndürerek Bing otomatik öneri API'si, uygulamanızın arama deneyimini geliştirin.  |
| [Bing istatistikleri](bing-web-stats.md) | Bing Statistics, Bing arama API'leri uygulamanızın kullandığı için analiz sağlar. Kullanılabilir analytics bazıları şunlardır: çağrı hacmi, en iyi sorgu dizeleri ve coğrafi dağıtım. |

## <a name="next-steps"></a>Sonraki adımlar

* Bing arama API'si [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
