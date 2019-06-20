---
title: Bing Haber Arama API’si nedir?
titlesuffix: Azure Cognitive Services
description: Bing haber arama API'si, popüler konular ve başlıkları dahil olmak üzere kategoriler arasında geçerli başlıklar Web'i aramak için kullanmayı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: overview
ms.date: 06/19/2019
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: d4d8c35869fbfc13220aba037a97aadd3cea01c2
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272724"
---
# <a name="what-is-the-bing-news-search-api"></a>Bing Haber Arama API’si nedir?

Bing haber arama API'si, Bing'ın bilişsel haber arama özellikleri uygulamalarınızla tümleştirin kolaylaştırır. Benzer bir deneyim için API'nin sağladığı [Bing Haberler](https://www.bing.com/news), süreniz arama sorguları göndermek ve ilgili haber makalelerini alırsınız.

Bing haber arama API'si, haber arama sonuçları yalnızca sağladığını unutmayın. Kullanım [Bing Web araması API'si](../bing-web-search/search-the-web.md), [Video arama API'si](../bing-video-search/search-the-web.md) ve [resim arama API'si](../bing-image-search/overview.md) diğer web içeriği türleri için.

## <a name="bing-news-search-api-features"></a>Bing haber arama API'si özellikleri

Bing haber arama API'si, öncelikli olarak bulur ve ilgili haber makalelerini döndürür, ancak Web Akıllı ve odaklanmış haberleri almak için çeşitli özellikler sağlar.

|Özellik  |Açıklama  |
|---------|---------|
|[Önermek ve arama terimlerini kullanma](concepts/search-for-news.md#suggest-and-use-search-terms)     | Kullanarak arama deneyiminizi geliştirmeye [Bing otomatik öneri API'si](../bing-autosuggest/get-suggested-search-terms.md) yazıldığı gibi önerilen arama terimlerini görüntülenecek.         |
|[Genel haberleri alın](concepts/search-for-news.md#get-general-news)     | Bing haber arama API'si için bir arama sorgusu gönderme ve ilgili haber makalelerini listesini geri alamazsınız haber bulun.           |
|[Günümüzün en güncel](concepts/search-for-news.md#get-todays-top-news)      | Üst haberleri gün için tüm kategorilerde geride alın.       |
|[Haberleri kategoriye göre](concepts/search-for-news.md)     | Belirli kategorileri haberler için arama yapın.        | 
|[Başlık Haberleri](concepts/search-for-news.md)     | Üst başlıkları için tüm kategorilerde geride arayın.         |

## <a name="workflow"></a>İş akışı

Bing haber arama API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir. REST API veya SDK'sını kullanarak hizmetini kullanabilirsiniz.

1. Bing arama API'leri erişimi olan bir Bilişsel hizmetler API hesabı oluşturma. Azure aboneliğiniz yoksa, şunları yapabilirsiniz [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-news-api).

2. Geçerli bir arama sorgusuyla API'sine bir istek gönderin.

3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

İlk olarak, deneyin [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) Bing haber arama API'si için. Bu Tanıtım, nasıl hızlı bir şekilde bir arama sorgusu özelleştirebilir ve Web'de haber bulma gösterir.

İlk API isteğinizle birlikte hızlıca başlamak için hızlı başlangıç için deneyin [REST API](quickstart.md) veya biri [SDK'ları](sdk.md).

## <a name="see-also"></a>Ayrıca bkz.

* [Bing haber arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference) başvuru bölümünde, tanımları ve uç noktaları, üst bilgileri, API yanıtları ve görüntü tabanlı arama sonuçları istemek için kullanabileceğiniz sorgu parametreleri hakkında bilgi içerir.

* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./useanddisplayrequirements.md) konusunda belirtilmektedir.
