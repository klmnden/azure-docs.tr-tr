---
title: Desteklenen diller - Translator konuşma tanıma API'si
titlesuffix: Azure Cognitive Services
description: Translator konuşma tanıma API'si tarafından desteklenen diller görüntüleyin.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: conceptual
ms.date: 3/5/2018
ms.author: v-jansko
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 63592a0afc7e5da0a37c25c226b92b587aa5f886
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60827535"
---
# <a name="languages-supported-by-the-translator-speech-api"></a>Translator konuşma tanıma API'si tarafından desteklenen diller

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Aşağıdaki dilleri konuşma çevirisi için desteklenir. Konuşma çevirisi, konuşma için okuma veya Konuşmayı metne dönüştürme için desteklenen iki dilde kullanılabilir. Hedef Dil, konuşma çevirisi için desteklenmiyor, yalnızca konuşma metin çevirisi için kullanılabilir.

| Konuşma dili    |
|:----------- |
| Arapça (Modern standart)      |
| Çince (Mandarin)      |
| Türkçe      |
| Fransızca       |
| Almanca       |
| İtalyanca      |
| Japonca      |
| Portekizce (Brezilya)     |
| Rusça      |
| İspanyolca       |

Translator konuşma tanıma API'si, metin çevirisi konuşma için bir hedef dil olarak aşağıdaki dilleri desteklemektedir.

| SMS dili    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner dili      | `af`          |
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Kanton (Geleneksel)      | `yue`          |
| Katalanca      | `ca`          |
| Basitleştirilmiş Çince      | `zh-Hans`          |
| Geleneksel Çince      | `zh-Hant`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Türkçe      | `en`          |
| Estonca      | `et`          |
| Fiji Adaları dili      | `fj`          |
| Filipin dili      | `fil`          |
| Fince      | `fi`          |
| Fransızca       | `fr`          |
| Almanca       | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
|İzlanda dili|`is`          |
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malgaşça      | `mg`          |
| Malay dili      | `ms`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
| Queretaro Otomi      | `otq`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Samoaca      | `sm`          |
| Sırpça (Kiril)      | `sr-Cyrl`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca       | `es`          |
| İsveççe      | `sv`          |
| Tahitian      | `ty`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Tonga Dili      | `to`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |
| Yucatec Maya      | `yua`          |

## <a name="access-the-list-programmatically"></a>Listenin programlamayla erişme

Dilleri kaynak program aracılığıyla kullanarak desteklenen dillerin listesini erişebilirsiniz. Liste, desteklenen herhangi bir dili veya İngilizce dil adı yanı sıra dil kodu sağlar. Yeni dil kullanılabilir duruma geldiğinde bu liste Translator konuşma çevirisi hizmeti tarafından otomatik olarak güncelleştirilir.

Dil kaynağı, konuşma tanıma, metin ve metin okuma için desteklenen dillerin listesini döndürür. Dil kaynağı, kimlik doğrulaması gerektirmez.

[API Başvurusu, dil yöntemini deneyin için ziyaret edin](languages-reference.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesinde listeye erişme

Dilleri hızlı bir bakış için Microsoft Translator Web sitesi Translator metin ve konuşma API'leri tarafından desteklenen tüm dillerde gösterir. Bu liste, dil kodu gibi geliştirici özgü bilgileri içermez.

[Dilleri listesine bakın](https://www.microsoft.com/translator/languages.aspx)
