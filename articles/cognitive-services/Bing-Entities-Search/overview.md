---
title: Bing varlık arama API'si nedir?
titlesuffix: Azure Cognitive Services
description: Bing varlık arama API'si ayıklayın ve varlıkları ve yerde arama kullanmasını arama sorgularından.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: overview
ms.date: 02/01/2019
ms.author: scottwhi
ms.openlocfilehash: 9190c87b7afff66162e25fb3cd08bfeac76aff74
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757492"
---
# <a name="what-is-bing-entity-search-api"></a>Bing varlık arama API'si nedir?

Bing Varlık Arama API'si, Bing'e bir arama sorgusu gönderip varlıkları ve yerleri içeren sonuçlar alır. Yer sonuçları restoranlar, oteller veya diğer yerel işletmeleri kapsar. Sorguda yerel işletmenin adı belirtildiğinde veya bir işletme türü istendiğinde (yakınımdaki restoranlar gibi) Bing, yerleri döndürür. Sorgu tanınmış kişiler, yerler (turistik noktalar, şehirler, ülkeler vb.) veya nesneler olduğunda Bing, varlıkları döndürür.

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
* [Bing varlık arama API'si v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) başvuru bölümü.
* Bing Arama API'leri ile edinilen içeriğin ve bilgilerin kabul edilebilir kullanımları [Bing Kullanımı ve Görüntü Gereksinimleri](./use-display-requirements.md) konusunda belirtilmektedir.
