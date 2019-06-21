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
ms.date: 06/04/2019
ms.author: erhopf
ms.openlocfilehash: d04677362e0ba3ace59d55ede9bd6241f17130e9
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67269233"
---
# <a name="request-limits-for-translator-text"></a>Translator metin çevirisi için istek sınırları

Bu makalede, Translator metin API'si azaltma sınırları sağlar. Çeviri, harf çevirisi, cümle uzunluğu algılama, dil algılama ve diğer çevirileri hizmetleri içerir.

## <a name="character-and-array-limits-per-request"></a>İstek başına karakter ve dizi sınırları

Her çeviri isteği 5000 karakter ile sınırlıdır. Karakter değil istek sayısına göre ücretlendirilirsiniz. Daha kısa istekleri göndermek için önerilir.

Aşağıdaki tablo listeleri dizi öğesi ve karakter sınırları Translator metin çevirisi API'si, her işlem için.

| İşlem | Dizi öğesi en büyük boyutu |   Dizi öğelerinin üst limiti |  En fazla istek boyutu (karakter) |
|:----|:----|:----|:----|
| Translate | 5,000 | 100   | 5,000 |
| Transliterate | 5,000 | 10    | 5,000 |
| Detect | 10,000 | 100 |   50,000 |
| BreakSentence | 10,000    | 100 | 5,0000 |
| Sözlük Arama| 100 |  10  | 1000 |
| Sözlük Örnekleri | 100 metin ve 100 çeviri (toplam 200)| 10|   2,000 |

## <a name="character-limits-per-hour"></a>Saat başına karakter sınırları

Saatlik, karakter sınırı, Translator metin çevirisi abonelik katmanına göre belirlenir. Saatlik kota saat eşit olarak kullanılması. Ulaşın veya bu sınırları aşan ya da kısa bir süre içinde kotasının bir bölümü çok büyük göndermek, büyük olasılıkla yetersiz kota yanıt alırsınız. Üzerinde eşzamanlı istek sınırı yoktur.

| Katman | Karakter sınırı |
|------|-----------------|
| F0 | saat başına 2 milyon karakter |
| S1 | saat başına 40 milyon karakter |
| S2 / C2 | saat başına 40 milyon karakter |
| S3 / C3 | saat başına 120 milyon karakter |
| S4 / C4 | saat başına 200 milyon karakter |

İçin sınırlar [hizmetli abonelikleri](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) S1 katmanı ile aynıdır.

Bu sınırlar, Microsoft'un standart çeviri modellerini için kısıtlanır. Özel Translator kullanan özel çeviri modellerini saniyede 1.800 karakterle sınırlıdır.

## <a name="latency"></a>Gecikme süresi

Translator metin çevirisi API'si standart modelleri kullanarak 15 saniyede en fazla bir gecikme vardır. Özel modelleri kullanarak çeviri 25 saniye en fazla bir gecikme vardır. Bu zamana kadar bir sonuç ya da bir zaman aşımı yanıt aldığınız. Genellikle, yanıtları için 300 milisaniye 150 milisaniye olarak döndürülür. Yanıt süreleri, istek ve dil çiftin boyutuna bağlı olarak değişir. Bir çeviri almazsanız veya bir [hata yanıtı](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) bu süre içinde size ağ bağlantınızı denetleyin ve yeniden deneyin.

## <a name="sentence-length-limits"></a>Tümce uzunluk sınırları

Kullanırken [BreakSentence](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) işlev cümle uzunluğu ise 275 karakter ile sınırlıdır. Bu diller için özel durum vardır:

| Dil | Kod | Karakter sınırı |
|----------|------|-----------------|
| Çince | zh | 132 |
| Almanca | de | 290 |
| İtalyanca | it | 280 |
| Japonca | ja | 150 |
| Portekizce | PT | 290 |
| İspanyolca | es | 280 |
| İtalyanca | it | 280 |
| Tay Dili | TH | 258 |

> [!NOTE]
> Bu sınırı çevirileri için geçerli değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [Fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Bölgesel kullanılabilirlik](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [V3 Translator metin çevirisi API'si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
