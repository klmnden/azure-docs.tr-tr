---
title: Bing varlık arama API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si ayıklayın ve varlıkları ve yerde arama kullanmasını arama sorgularından.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 02/01/2019
ms.author: scottwhi
ms.openlocfilehash: 679a3d9efbeeb75e0aa8e3986fa85b7ecf0d77bd
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388491"
---
# <a name="what-is-bing-entity-search-api"></a>Bing varlık arama API'si nedir?

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Sorguda yerel işletmenin adı belirtildiğinde veya bir işletme türü istendiğinde (yakınımdaki restoranlar gibi) Bing, yerleri döndürür. Sorgu iyi bilinen kişiler, yerler (turistik yerleri, durumları, ülkeler/bölgeler, vb.) veya şeyler belirtiyorsa Bing varlıkları döndürür.

|Özellik  |Açıklama  |
|---------|---------|
|[Gerçek zamanlı arama önerileri](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Bir açılan listedeki kullanıcılar türünüz olarak olarak görüntülenen arama önerileri sağlar.       | 
| [Varlık Kesinleştirme](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Birden çok olası anlamı içeren sorgular için birden çok varlık alın. |
| [Yerler bulun](concepts/search-for-entities.md#find-places) | Arayın ve yerel işletmeler ve varlıklar hakkında bilgi döndürür  |

## <a name="workflow"></a>İş akışı

Bing varlık arama API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir. REST API veya SDK'sını kullanarak hizmetini kullanabilirsiniz.

1. Bing Arama API'lerine erişimi olan bir [Bilişsel Hizmetler API'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) oluşturun. Azure aboneliğiniz yoksa ücretsiz olarak [hesap oluşturabilirsiniz](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api).
2. Geçerli bir arama sorgusuyla API'sine bir istek gönderin.
3. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Deneyin [etkileşimli tanıtım](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) Bing varlık arama API'si için. 
* İlk isteğinizi hızlıca başlamak için deneyin bir [hızlı](quickstarts/csharp.md).
* [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) başvuru bölümü.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
