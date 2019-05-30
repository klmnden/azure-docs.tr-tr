---
title: Bing Video Arama API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bing Video arama API'si kullanarak web üzerinden videolar için arama hakkında bilgi edinin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: 2f6be01a3aee4e156b7af184fa2537455fc73f3f
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66386528"
---
# <a name="what-is-the-bing-video-search-api"></a>Bing Video Arama API'si nedir?

Bing Video arama API'si, hizmetler ve uygulamalar, video arama özellikleri eklemek kolaylaştırır. API ile kullanıcı arama sorguları gönderdiğinizde, alma ve benzer şekilde ilgili ve yüksek kaliteli videolar görüntüleme [Bing Video](https://www.bing.com/video). Bu API yalnızca videolar içeren arama sonuçları almak için kullanın. [Bing Web araması API'si](../bing-web-search/search-the-web.md) web içerik, Web sayfaları, video, haber ve görüntüleri dahil olmak üzere diğer türleri döndürebilir.

## <a name="bing-video-search-api-features"></a>Bing Video arama API'si özellikleri

| Özellik                                                                                                                                                                                 | Açıklama                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Gerçek zamanlı arama terimleri önerme](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Yazılmaya başladıkları anda önerilen arama terimleri görüntülemek için [Bing Otomatik Öneri API](../bing-autosuggest/get-suggested-search-terms.md)'sini kullanarak uygulama deneyimini iyileştirin. |
| [Filtre ve video sonuçları kısıtlama](concepts/get-videos.md#filtering-videos)                      | Sorgu parametreleri düzenleyerek döndürülen videoları filtreleyin.                                                                                                       |
| [Küçük resimleri kırpma, yeniden boyutlandırma ve görüntüleme](resize-and-crop-thumbnails.md)                                                | Düzenle ve küçük resim önizlemeleri için Bing Video arama API'si tarafından döndürülen videoları görüntüleyin.                                                                                      |
| [Popüler videolar Al](trending-videos.md) | Dünyanın dört bir yanındaki popüler videoları arayın.                                                                                                          |
| [Video Öngörüler elde edin](video-insights.md) | Dünyanın dört bir yanındaki popüler videolar için arama özelleştirin.                                                                                                          |

## <a name="workflow"></a>İş akışı

Bing Video arama API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir. Hizmeti [REST API](csharp.md) veya [SDK](video-search-sdk-quickstart.md) kullanarak kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Geçerli bir arama sorgusuyla API'sine bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bing Video arama API'si [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) nasıl bir arama sorgusu özelleştirebilir ve videolar için Web'de arama gösterir.

API'yi çağırmaya hazır olduğunuzda, bir [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).

Kullanım [hızlı](csharp.md) ilk API isteğinizle birlikte hızlıca başlamak için.

## <a name="see-also"></a>Ayrıca bkz.

* [Bing Video arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) başvuru sayfası uç noktaları, üst bilgiler ve arama sonuçlarını istemek için kullanılan sorgu parametreleri listesini içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.