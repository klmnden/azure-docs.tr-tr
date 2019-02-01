---
title: Bing arama API'leri nelerdir?
titleSuffix: Azure Cognitive Services
description: Bing arama API'leri ve, uygulamalarınız ve hizmetlerinizdeki bilişsel alanlarınıza nasıl olanak sağlayabileceğiniz hakkında bilgi edinmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 01/31/2019
ms.author: aahi
ms.openlocfilehash: b2277f540b076307fe74c57068ff288860f82796
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513120"
---
# <a name="what-are-the-bing-search-apis"></a>Bing arama API'leri nelerdir?

Bing arama API'leri, web bağlantılı uygulamalar ve Web sayfaları, görüntüler, haber, konumları ve tanıtımları olmadan daha fazla bilgi bulmak services oluşturmanıza olanak sağlar. Bing arama REST API veya SDK'ları kullanarak arama istekleri gönderdiğinizde, web aramaları için ilgili bilgiler ve içerik alabilirsiniz. Çeşitli Bing arama API'leri ve uygulamalarınızı ve hizmetlerinizi bilişsel arama nasıl tümleştirebilirsiniz hakkında bilgi edinmek için bu makaleyi kullanın. Fiyatlandırma ve oran sınırlarını API'leri arasında değişiklik gösterebilir.

## <a name="the-bing-web-search-api"></a>Bing Web araması API'si

[Bing Web araması API'si](../Bing-Web-Search/index.yml) Web sayfaları, görüntüler, video, haber ve daha fazla döndürür. Bu API'ye gönderilen arama sorguları, dahil etmek veya belirli içerik türlerini dışarıda bırakmak için filtrelenebilir.

Bing Web araması API'si tüm ilgili web içeriği türleri için arama gereken uygulamalar kullanmayı düşünün. Uygulamanızı belirli bir çevrimiçi içerik türü için arama, arama API'leri aşağıdaki birini göz önünde bulundurun: 

## <a name="content-specific-bing-search-apis"></a>İçerik özgü Bing arama API'leri

Aşağıdaki Bing arama API'leri, resimler, Haberler, yerel işletmeler ve videolar gibi Web belirli içerik döndürür.

| Bing API'si | Açıklama |
| -- | -- | 
| [Varlık arama](../Bing-Entities-Search/index.yml) | Bing varlık arama API'si, varlıklar, kişiler, yerler veya noktalar içeren arama sonuçlarını döndürür. Sorguya bağlı olarak önemli kişiler, yerel işletmeler, yer işareti, hedefler ve daha fazlasını içerebilir arama sorgusu için uygun bir veya daha fazla varlık API döndürür. |
| [Resim arama](../Bing-Image-Search/index.yml) | Bing resim arama API'si arama ve bulma yüksek kaliteli statik ve animasyonlu görüntüler benzer şekilde sağlayan [Bing.com/images](https://www.Bing.com/images). Eklemek veya görüntü boyutu, renk, lisans ve güncellik gibi özniteliği tarafından dışlamak için arama iyileştirebilirsiniz. Popüler resimler için arama yapın, bunlar hakkında Öngörüler elde etmek için görüntüleri karşıya yükleme ve küçük resim önizlemeleri görüntüler. |
| [Haber arama](../Bing-News-Search/index.yml) | Bing haber arama API'si haberleri benzer şekilde bulmanızı sağlayan [Bing.com/news](https://www.Bing.com/news). API, birden çok kaynakları veya özel etki alanları haber makalelerini döndürür. Popüler makaleler, Haberler ve başlıkları almak için kategoriler arasında arama yapabilirsiniz.
| [Video arama](../Bing-Video-Search/index.yml) | Bing Video arama API'si, Web'de video bulun sağlar. Popüler videolar ve ilgili içerik küçük resim önizlemeleri alın. |
| [Görsel arama](../Bing-visual-search/index.yml) | Bir görüntüyü karşıya yükleme veya görsel olarak benzer ürünler, görüntüler ve ilgili aramalar gibi hakkında ayrıntılı bilgi almak için bir URL kullanın. |
 [Yerel işletme arama](../bing-local-business-search/index.yml) | Bing Yerel İşletme Arama API'si, uygulamalarınızın arama sorguları temelinde yerel işletmeler hakkında ilgili kişi ve konum bilgilerini bulmasına olanak tanır. |

## <a name="the-bing-custom-search-api"></a>Bing özel arama API'si

Bir özel arama örneği ile [Bing özel arama](../Bing-Custom-Search/index.yml) API yalnızca içerik ve, önem verdiğiniz konulara odaklanan bir arama deneyimi oluşturmanıza olanak sağlar. Örneğin, etki alanları, Web siteleri ve Bing arama yapacağı belirli Web sayfalarını belirttikten sonra sonuçları belirli bir içeriğe kurulmasını. Bing özel otomatik öneri, görüntü, birleştirmek ve Video arama API'leri, aramanızı daha fazla özelleştirmek için deneyimi.  

## <a name="additional-bing-search-apis"></a>Ek Bing arama API'leri

Aşağıdaki Bing arama API'leri, diğer Bing arama API'leri ile birleştirerek, arama deneyimini geliştirmek etkinleştirin.

| API | Açıklama |
| -- | -- | 
| [Bing Otomatik Öneri](../Bing-Autosuggest/index.yml) | Gerçek zamanlı olarak önerilen aramalar döndürerek Bing otomatik öneri API'si, uygulamanızın arama deneyimini geliştirin.  |
| [Bing istatistikleri](bing-web-stats.md) | Bing Statistics, Bing arama API'leri uygulamanızın kullandığı için analiz sağlar. Kullanılabilir analytics bazıları şunlardır: çağrı hacmi, en iyi sorgu dizeleri ve coğrafi dağıtım. |

## <a name="next-steps"></a>Sonraki adımlar

* Bing arama API'si [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
