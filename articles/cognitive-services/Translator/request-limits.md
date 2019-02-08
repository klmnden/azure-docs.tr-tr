---
title: İstek sınırları - Translator metin çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Bu makalede için Translator Text API isteği sınırları listelenmektedir. Karakter sayısı, istek başına 5000 karakter sınırı değil isteği sıklığı temel ücretleri uygulanır. Karakter tabanlı, 2 milyon karakter saat başına sınırlı F0 ile abonelik limitlerdir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: erhopf
ms.openlocfilehash: f89e7e674efe3a823b7c969840772565650d8d07
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859479"
---
# <a name="request-limits-for-translator-text"></a>Translator metin çevirisi için istek sınırları

Bu makalede, Translator metin API'si azaltma sınırları sağlar. Çeviri, harf çevirisi, cümle uzunluğu algılama, dil algılama ve diğer çevirileri hizmetleri içerir.

## <a name="character-limits-per-request"></a>İstek başına karakter sınırları

Her isteğin 5000 karakter ile sınırlıdır. Karakter değil istek sayısına göre ücretlendirilirsiniz. Daha kısa istekleri göndermek için ve belirli bir zamanda bekleyen bazı istekleri için önerilir.

Translator metin çevirisi API'si Bekleyen isteklerin sayısına bir sınır yoktur.

## <a name="character-limits-per-hour"></a>Saat başına karakter sınırları

Saatlik, karakter sınırı, Translator metin çevirisi abonelik katmanına göre belirlenir. Ulaşın veya bu sınırları aşan büyük olasılıkla yetersiz kota yanıt alırsınız:

| Katman | Karakter sınırı |
|------|-----------------|
| F0 | saat başına 2 milyon karakter |
| S1 | saat başına 40 milyon karakter |
| S2 | saat başına 40 milyon karakter |
| S3 | saat başına 120 milyon karakter |
| S4 | saat başına 200 milyon karakter |

Bu sınırlar, Microsoft'un genel sistemlerine kısıtlanır. Microsoft Translator hub'ı kullanan özel çevirisi sistemleri saniyede 1.800 karakterle sınırlıdır.

## <a name="latency"></a>Gecikme süresi

Translator metin çevirisi, 13 saniye en fazla bir gecikme vardır. Bu zamana kadar bir sonuç ya da bir zaman aşımı yanıt aldığınız. Genellikle, yanıtları için 300 milisaniye 150 milisaniye olarak döndürülür. Yanıt süreleri boyutu veya istek ve dil çifti göre değişir.

## <a name="sentence-length-limits"></a>Tümce uzunluk sınırları

Kullanırken [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) işlev cümle uzunluğu ise 275 karakter ile sınırlıdır. Bu diller için özel durum vardır:

| Dil | Kod | Karakter sınırı |
|----------|------|-----------------|
| Çince | zh | 132 |
| Almanca  | de | 290 |
| İtalyanca | it | 280 |
| Japonca | ja | 150 |
| Portekizce | PT | 290 |
| İspanyolca  | es | 280 |
| İtalyanca | it | 280 |
| Tay Dili | . | 258 |

> [!NOTE]
> Bu sınırı çevirileri için geçerli değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [Fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Bölgesel kullanılabilirlik](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [V3 Translator metin çevirisi API'si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
