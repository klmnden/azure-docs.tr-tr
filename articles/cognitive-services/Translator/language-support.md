---
title: Dil desteği - Translator metin çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Doğal Translator metin çevirisi API'si tarafından desteklenen dillerin listesi.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: d98922937781fd169d34881fa67a6b5746d06df7
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882473"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Translator metin API'si, dil ve bölge desteği

Translator metin çevirisi API'si için metin çevirisi için aşağıdaki dilleri desteklemektedir. Sinirsel makine çevirisi (NMT), yüksek kaliteli yapay ZEKA destekli makine çevirileri için yeni bir standart olduğundan ve sinir sistem kullanılabilir duruma gelince, Translator Text API V3 kullanarak varsayılan kullanılabilir.

[Makine çevirisi nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

**V2 Translator API'si**

> [!NOTE]
> V2 30 Nisan 2018'de kullanım dışı bırakıldı ve 30 Nisan 2019 üzerinde durdurulacaktır.

* Yalnızca istatistiksel: Sinir sistemi değildir, bu dil için kullanılabilir.
* Sinir kullanılabilir: Sinir sistem kullanılabilir. İlgili parametreyi kullanın `category=generalnn` sinir sistemine erişebilir.
* Sinir varsayılan: Varsayılan çeviri sinir sistemidir. İlgili parametreyi kullanın `category=smt` Microsoft Translator hub'ı ile kullanmak için istatistiksel sistemine erişebilir.
* Sinir yalnızca: Yalnızca sinirsel çeviri kullanılabilir.

**Translator API v3** V3 Translator API'si, varsayılan olarak sinir ve istatistiksel sistemleri bulunan ve yalnızca sinir sistemi bulunmayan mevcut olduğunda. Özel Translator sinirsel dillerle yalnızca kullanılabilir. [Özel Translator şu anda kullanılabilen dilleri görüntülemek](#customization).

|Dil|  Dil kodu|  V2 API'Sİ| V3 API|
|:-----|:-----:|:-----|:-----|
|Afrikaner dili| `af`    |Yalnızca istatistiksel|  Sinir|
|Arapça|    `ar`    |Sinir kullanılabilir|  Sinir|
|Bangla|    `bn`    |Sinir kullanılabilir|  Sinir|
|Boşnakça (Latin)|   `bs`    |Sinir kullanılabilir|  Sinir|
|Bulgarca| `bg`    |Sinir kullanılabilir|  Sinir|
|Kanton (Geleneksel)|   `yue`   |Yalnızca istatistiksel|  İstatistik|
|Katalanca|   `ca`    |Yalnızca istatistiksel|  İstatistik|
|Çince Basitleştirilmiş|    `zh-Hans`   |Sinir varsayılan |Sinir|
|Geleneksel Çince|   `zh-Hant`   |Sinir varsayılan |Sinir|
|Hırvatça|  `hr`    |Sinir kullanılabilir|  Sinir|
|Çekçe| `cs`    |Sinir kullanılabilir|  Sinir|
|Danca|    `da`    |Sinir kullanılabilir   |Sinir|
|Felemenkçe| `nl`    |Sinir kullanılabilir|  Sinir|
|Türkçe|   `en`    |Sinir kullanılabilir|  Sinir|
|Estonca|  `et`    |Sinir kullanılabilir|  Sinir|
|Fiji Adaları dili|    `fj`    |Yalnızca istatistiksel|  İstatistik|
|Filipin dili|  `fil`   |Yalnızca istatistiksel|  İstatistik|
|Fince|   `fi`    |Sinir kullanılabilir|  Sinir|
|Fransızca |    `fr`    |Sinir kullanılabilir|  Sinir|
|Almanca |    `de`    |Sinir kullanılabilir|  Sinir|
|Yunanca| `el`    |Sinir kullanılabilir|  Sinir|
|Haiti Kreyolu|    `ht`    |Yalnızca istatistiksel   |İstatistik|
|İbranice |`he`   |Sinir kullanılabilir   |Sinir|
|Hintçe| `hi`    |Sinir varsayılan|    Sinir|
|Hmong Daw| `mww`   |Yalnızca istatistiksel|  İstatistik|
|Macarca| `hu`    |Sinir kullanılabilir|  Sinir|
|İzlanda dili| `is`    |Yalnızca sinir|   Sinir|
|Endonezya dili|    `id`    |Yalnızca istatistiksel|  İstatistik|
|İtalyanca|   `it`    |Sinir kullanılabilir|  Sinir|
|Japonca|  `ja`    |Sinir kullanılabilir|  Sinir|
|Svahili dili| `sw`    |Yalnızca istatistiksel|  İstatistik|
|Klingon|   `tlh`   |Yalnızca istatistiksel|  İstatistik|
|Klingon (plqaD)|   `tlh-Qaak`  |Yalnızca istatistiksel|  İstatistik|
|Korece |`ko`   |Sinir kullanılabilir|  Sinir|
|Letonca|   `lv`    |Sinir kullanılabilir|  Sinir|
|Litvanca|    `lt`    |Sinir kullanılabilir|  Sinir|
|Malgaşça|  `mg`    |Yalnızca istatistiksel|  İstatistik|
|Malay dili| `ms`    |Yalnızca istatistiksel   |İstatistik|
|Malta dili|   `mt`    |Yalnızca istatistiksel|  İstatistik|
|Norveççe| `nb`    |Sinir kullanılabilir|  Sinir|
|Farsça|   `fa`    |Yalnızca istatistiksel|  İstatistik|
|Lehçe|    `pl`    |Sinir kullanılabilir|  Sinir|
|Portekizce|    `pt`    |Sinir kullanılabilir|  Sinir|
|Queretaro Otomi|   `otq`   |Yalnızca istatistiksel|  İstatistik|
|Rumence|  `ro`    |Sinir kullanılabilir|  Sinir|
|Rusça|   `ru`    |Sinir kullanılabilir|  Sinir|
|Samoaca|    `sm`    |Yalnızca istatistiksel|  İstatistik|
|Sırpça (Kiril)|    `sr-Cyrl`   |Yalnızca istatistiksel|  İstatistik|
|Sırpça (Latin)|   `sr-Latn`   |Yalnızca istatistiksel   |İstatistik|
|Slovakça|    `sk`    |Sinir kullanılabilir|  Sinir|
|Slovence| `sl`    |Sinir kullanılabilir|  Sinir|
|İspanyolca |   `es`    |Sinir kullanılabilir|  Sinir|
|İsveççe|   `sv`    |Sinir kullanılabilir   |Sinir|
|Tahitian|  `ty`    |Yalnızca istatistiksel|  İstatistik|
|Tamil dili| `ta`    |Yalnızca istatistiksel|  İstatistik|
|Telugu dili|    `te`    |Yalnızca sinir|   Sinir|
|Tay Dili|  `th`    |Sinir kullanılabilir|  Sinir|
|Tonga Dili|    `to`    |Yalnızca istatistiksel|  İstatistik|
|Türkçe|   `tr`    |Sinir kullanılabilir   |Sinir|
|Ukrayna dili| `uk`    |Sinir kullanılabilir|  Sinir|
|Urduca|  `ur`    |Yalnızca istatistiksel|  İstatistik|
|Vietnam dili|    `vi`    |Sinir kullanılabilir|  Sinir|
|Gaelce| `cy`    |Sinir kullanılabilir|  Sinir|
|Yucatec Maya|  `yua`   |Yalnızca istatistiksel|  İstatistik|

## <a name="transliteration"></a>Alfabe çevirisi

Transliterate yöntemi aşağıdaki dilleri desteklemektedir. İçinde "içine/dışına", "<> –" dil ya da ya da listelenen betikleri Alfabesiyle yazılmış olduğunu gösterir. "-->" Dil yalnızca bir komut dosyasından diğerine Alfabesiyle yazılmış olduğunu gösterir.

| Dil    | Dil kodu | Betik | Gelen/giden | Betik|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arapça | `ar` | Arapça `Arab` | <--> | Latin `Latn` |
|Bangla  | `bn` | Bengali `Beng` | <--> | Latin `Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Çince Basitleştirilmiş `Hans`| <--> | Latin `Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Çince Basitleştirilmiş `Hans`| <--> | Geleneksel Çince `Hant`|
| Geleneksel Çince | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Latin `Latn` |
| Geleneksel Çince | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Çince Basitleştirilmiş `Hans` |
| Gucerat dili | `gu`  | Gucerat dili `Gujr` | --> | Latin `Latn` |
| İbranice | `he` | İbranice `Hebr` | <--> | Latin `Latn` |
| Hintçe | `hi` | Devanagari `Deva` | <--> | Latin `Latn` |
| Japonca | `ja` | Japonca `Jpan` | <--> | Latin `Latn` |
| Kannada dili | `kn` | Kannada dili `Knda` | --> | Latin `Latn` |
| Malayalam dili | `ml` | Malayalam dili `Mlym` | --> | Latin `Latn` |
| Marathi dili | `mr` | Devanagari `Deva` | --> | Latin `Latn` |
| Odia | `or` | Odia `Orya` | <--> | Latin `Latn` |
| Pencap dili | `pa` | Gurmuki `Guru`  | <--> | Latin `Latn`  |
| Sırpça (Kiril) | `sr-Cyrl` | Kiril `Cyrl`  | --> | Latin `Latn` |
| Sırpça (Latin) | `sr-Latn` | Latin `Latn` | --> | Kiril `Cyrl`|
| Tamil dili | `ta` | Tamil dili `Taml` | --> | Latin `Latn` |
| Telugu dili | `te` | Telugu dili `Telu` | --> | Latin `Latn` |
| Tay Dili | `th` | Tay Dili `Thai` | <--> | Latin `Latn` |

## <a name="dictionary"></a>Sözlük

Sözlük için veya İngilizce arama ve örnekler yöntemleri kullanarak aşağıdaki dilleri desteklemektedir.

| Dil    | Dil kodu |
|:----------- |:-------------:|
| Afrikaner dili      | `af`          |
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Katalanca      | `ca`          |
| Çince Basitleştirilmiş      | `zh-Hans`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca       | `fr`          |
| Almanca       | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
| İzlanda dili    | `is`  |
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Malay dili      | `ms`          |
| Malta dili      | `mt`          |
| Norveççe      | `nb`          |
| Farsça      | `fa`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca       | `es`          |
| İsveççe      | `sv`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |

## <a name="detect"></a>Detect

Aşağıdaki dilleri Algıla yöntemi tarafından desteklenir. Mayıs algılamak Microsoft Translator çeviremez dillerini belirleyin.

| Dil    |
|:----------- |
| Afrikaner dili |
| Arnavutça |
| Arapça |
| Bask dili |
| Belarusça |
| Bulgarca |
| Katalanca |
| Çince |
| Çince (Basitleştirilmiş) |
| Geleneksel Çince |
| Hırvatça |
| Çekçe |
| Danca |
| Felemenkçe |
| Türkçe |
| Esperanto |
| Estonca |
| Fince |
| Fransızca  |
| Galiçya dili |
| Almanca  |
| Yunanca |
| Haiti Kreyolu |
| İbranice |
| Hintçe |
| Macarca |
| İzlanda dili |
| Endonezya dili |
| İrlanda dili |
| İtalyanca |
| Japonca |
| Korece |
| Kurdish (Arabic) |
| Kürtçe (Latin) |
| Latin |
| Letonca |
| Litvanca |
| Makedonca |
| Malay dili |
| Malta dili |
| Norveççe |
| Norveççe (Nynorsk) |
| Peştuca |
| Farsça |
| Lehçe |
| Portekizce |
| Rumence |
| Rusça |
| Sırpça (Kiril) |
| Sırpça (Latin) |
| Slovakça |
| Slovence |
| Somali |
| İspanyolca  |
| Svahili dili |
| İsveççe |
| Tagalog dili |
| Telugu dili |
| Tay Dili |
| Türkçe |
| Ukrayna dili |
| Urduca |
| Özbekçe (Kiril) |
| Özbekçe (Latin) |
| Vietnam dili |
| Gaelce |
| Yidiş |

## <a name="access-the-translator-text-api-language-list-programmatically"></a>Translator metin çevirisi API'si dil listesi programlamayla erişme

Dilleri yöntemiyle Translator Text API v3.0 desteklenen dillerin listesini alabilirsiniz. İngilizce veya desteklenen herhangi bir dili özelliği, dil kodu yanı sıra, dil adı bazında görüntüleyebilir. Yeni diller sunuldukça bu liste Microsoft Translator hizmeti tarafından otomatik olarak güncelleştirilir.

[Dilleri işlemi başvuru belgelerini görüntüleyin](reference/v3-0-languages.md)

## <a name="customization"></a>Özelleştirme

Özelleştirme için veya İngilizce kullanarak aşağıdaki dillerde kullanılabilir [özel Translator](https://aka.ms/CustomTranslator).

| Dil    | Dil kodu |
|:----------- |:-------------:|
| Arapça       | `ar`          |
| Bangla      | `bn`          |
| Boşnakça (Latin)      | `bs`          |
| Bulgarca      | `bg`          |
| Çince Basitleştirilmiş      | `zh-Hans`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Türkçe    | `en`     |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca       | `fr`          |
| Almanca       | `de`          |
| Yunanca      | `el`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Macarca      | `hu`          |
| İzlanda dili | `is` |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
| Norveççe      | `nb`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca       | `es`          |
| İsveççe      | `sv`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Vietnam dili      | `vi`          |
| Gaelce | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesinde listeye erişme

Dilleri hızlı bir bakış için Microsoft Translator Web sitesi Translator metin ve konuşma API'leri tarafından desteklenen tüm dillerde gösterir. Bu liste, dil kodu gibi geliştirici özgü bilgileri içermez.

[Dilleri listesine bakın](https://www.microsoft.com/translator/languages.aspx)
