---
title: Dil desteği - Translator metin çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Doğal Translator metin çevirisi API'si tarafından desteklenen dillerin listesi.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 06/04/2019
ms.author: v-pawal
ms.openlocfilehash: 924324b11f49a50bfb5f00e117b33c0cc572e3bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66514993"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Translator metin API'si, dil ve bölge desteği

Translator metin çevirisi API'si için metin çevirisi için aşağıdaki dilleri desteklemektedir. Sinirsel makine çevirisi (NMT), yüksek kaliteli yapay ZEKA destekli makine çevirileri için yeni bir standart olduğundan ve sinir sistem kullanılabilir duruma gelince, Translator Text API V3 kullanarak varsayılan kullanılabilir.

[Makine çevirisi nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

## <a name="translation"></a>Çeviri

**V2 Translator API'si**

> [!NOTE]
> V2 30 Nisan 2018'de kullanım dışı bırakıldı. Lütfen yalnızca V3 sürümünde kullanılabilir olan yeni işlevsellikten yararlanmak için V3, uygulamalarınızı geçirin.

* Yalnızca istatistiksel: Sinir sistemi değildir, bu dil için kullanılabilir.
* Sinir kullanılabilir: Sinir sistem kullanılabilir. İlgili parametreyi kullanın `category=generalnn` sinir sistemine erişebilir.
* Sinir varsayılan: Varsayılan çeviri sinir sistemidir. İlgili parametreyi kullanın `category=smt` Microsoft Translator hub'ı ile kullanmak için istatistiksel sistemine erişebilir.
* Sinir yalnızca: Yalnızca sinirsel çeviri kullanılabilir.

**Translator API v3** V3 Translator API'si, varsayılan olarak sinir ve istatistiksel sistemleri bulunan ve yalnızca sinir sistemi bulunmayan mevcut olduğunda.

> [!NOTE]
> Şu anda özel Translator sinirsel dillerin bir alt kullanılabilir ve daha yavaş yavaş ekliyoruz. [Özel Translator şu anda kullanılabilen dilleri görüntülemek](#customization).

|Dil|  Dil kodu|  V2 API'Sİ| V3 API|
|:-----|:-----:|:-----|:-----|
|Afrikaner dili| `af`    |Yalnızca istatistiksel|  Nöral|
|Arapça|    `ar`    |Sinir kullanılabilir|  Nöral|
|Bangla|    `bn`    |Sinir kullanılabilir|  Nöral|
|Boşnakça (Latin)|   `bs`    |Sinir kullanılabilir|  Nöral|
|Bulgarca| `bg`    |Sinir kullanılabilir|  Nöral|
|Kanton (Geleneksel)|   `yue`   |Yalnızca istatistiksel|  İstatistik|
|Katalanca|   `ca`    |Yalnızca istatistiksel|  İstatistik|
|Basitleştirilmiş Çince|    `zh-Hans`   |Sinir varsayılan |Nöral|
|Geleneksel Çince|   `zh-Hant`   |Sinir varsayılan |Nöral|
|Hırvatça|  `hr`    |Sinir kullanılabilir|  Nöral|
|Çekçe| `cs`    |Sinir kullanılabilir|  Nöral|
|Danca|    `da`    |Sinir kullanılabilir   |Nöral|
|Felemenkçe| `nl`    |Sinir kullanılabilir|  Nöral|
|Türkçe|   `en`    |Sinir kullanılabilir|  Nöral|
|Estonca|  `et`    |Sinir kullanılabilir|  Nöral|
|Fiji Adaları dili|    `fj`    |Yalnızca istatistiksel|  İstatistik|
|Filipin dili|  `fil`   |Yalnızca istatistiksel|  İstatistik|
|Fince|   `fi`    |Sinir kullanılabilir|  Nöral|
|Fransızca|    `fr`    |Sinir kullanılabilir|  Nöral|
|Almanca|    `de`    |Sinir kullanılabilir|  Nöral|
|Yunanca| `el`    |Sinir kullanılabilir|  Nöral|
|Haiti Kreyolu|    `ht`    |Yalnızca istatistiksel   |İstatistik|
|İbranice |`he`   |Sinir kullanılabilir   |Nöral|
|Hintçe| `hi`    |Sinir varsayılan|    Nöral|
|Hmong Daw| `mww`   |Yalnızca istatistiksel|  İstatistik|
|Macarca| `hu`    |Sinir kullanılabilir|  Nöral|
|İzlanda dili| `is`    |Yalnızca sinir|   Nöral|
|Endonezya dili|    `id`    |Yalnızca istatistiksel|  İstatistik|
|İtalyanca|   `it`    |Sinir kullanılabilir|  Nöral|
|Japonca|  `ja`    |Sinir kullanılabilir|  Nöral|
|Svahili dili| `sw`    |Yalnızca istatistiksel|  İstatistik|
|Klingon|   `tlh`   |Yalnızca istatistiksel|  İstatistik|
|Klingon (plqaD)|   `tlh-Qaak`  |Yalnızca istatistiksel|  İstatistik|
|Korece |`ko`   |Sinir kullanılabilir|  Nöral|
|Letonca|   `lv`    |Sinir kullanılabilir|  Nöral|
|Litvanca|    `lt`    |Sinir kullanılabilir|  Nöral|
|Malgaşça|  `mg`    |Yalnızca istatistiksel|  İstatistik|
|Malay dili| `ms`    |Yalnızca istatistiksel   |İstatistik|
|Malta dili|   `mt`    |Yalnızca istatistiksel|  İstatistik|
|Norveççe| `nb`    |Sinir kullanılabilir|  Nöral|
|Farsça|   `fa`    |Yalnızca istatistiksel|  İstatistik|
|Lehçe|    `pl`    |Sinir kullanılabilir|  Nöral|
|Portekizce|    `pt`    |Sinir kullanılabilir|  Nöral|
|Queretaro Otomi|   `otq`   |Yalnızca istatistiksel|  İstatistik|
|Rumence|  `ro`    |Sinir kullanılabilir|  Nöral|
|Rusça|   `ru`    |Sinir kullanılabilir|  Nöral|
|Samoaca|    `sm`    |Yalnızca istatistiksel|  İstatistik|
|Sırpça (Kiril)|    `sr-Cyrl`   |Yalnızca istatistiksel|  İstatistik|
|Sırpça (Latin)|   `sr-Latn`   |Yalnızca istatistiksel   |İstatistik|
|Slovakça|    `sk`    |Sinir kullanılabilir|  Nöral|
|Slovence| `sl`    |Sinir kullanılabilir|  Nöral|
|İspanyolca|   `es`    |Sinir kullanılabilir|  Nöral|
|İsveççe|   `sv`    |Sinir kullanılabilir   |Nöral|
|Tahitian|  `ty`    |Yalnızca istatistiksel|  İstatistik|
|Tamil dili| `ta`    |Yalnızca istatistiksel|  İstatistik|
|Telugu dili|    `te`    |Yalnızca sinir|   Nöral|
|Tay Dili|  `th`    |Sinir kullanılabilir|  Nöral|
|Tonga Dili|    `to`    |Yalnızca istatistiksel|  İstatistik|
|Türkçe|   `tr`    |Sinir kullanılabilir   |Nöral|
|Ukrayna dili| `uk`    |Sinir kullanılabilir|  Nöral|
|Urduca|  `ur`    |Yalnızca istatistiksel|  İstatistik|
|Vietnam dili|    `vi`    |Sinir kullanılabilir|  Nöral|
|Gaelce| `cy`    |Sinir kullanılabilir|  Nöral|
|Yucatec Maya|  `yua`   |Yalnızca istatistiksel|  İstatistik|

## <a name="transliteration"></a>Alfabe çevirisi

Transliterate yöntemi aşağıdaki dilleri desteklemektedir. İçinde "içine/dışına", "<> –" dil ya da ya da listelenen betikleri Alfabesiyle yazılmış olduğunu gösterir. "-->" Dil yalnızca bir komut dosyasından diğerine Alfabesiyle yazılmış olduğunu gösterir.

| Dil    | Dil kodu | Komut Dosyası | Gelen/giden | Komut Dosyası|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arapça | `ar` | Arapça `Arab` | <--> | Latin `Latn` |
|Bangla  | `bn` | Bengali `Beng` | <--> | Latin `Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Basitleştirilmiş Çince `Hans`| <--> | Latin `Latn` |
| Çince (Basitleştirilmiş) | `zh-Hans` | Basitleştirilmiş Çince `Hans`| <--> | Geleneksel Çince `Hant`|
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Latin `Latn` |
| seçenekleri yerine | `zh-Hant` | Geleneksel Çince `Hant`| <--> | Basitleştirilmiş Çince `Hans` |
| Gucerat dili | `gu`  | Gujarati `Gujr` | --> | Latin `Latn` |
| İbranice | `he` | İbranice `Hebr` | <--> | Latin `Latn` |
| Hintçe | `hi` | Devanagari `Deva` | <--> | Latin `Latn` |
| Japonca | `ja` | Japonca `Jpan` | <--> | Latin `Latn` |
| Kannada dili | `kn` | Kannada `Knda` | --> | Latin `Latn` |
| Malayalam dili | `ml` | Malayalam `Mlym` | --> | Latin `Latn` |
| Marathi | `mr` | Devanagari `Deva` | --> | Latin `Latn` |
| Oriya | `or` | Odia `Orya` | <--> | Latin `Latn` |
| Pencap dili | `pa` | Gurmuki `Guru`  | <--> | Latin `Latn`  |
| Sırpça (Kiril) | `sr-Cyrl` | Kiril `Cyrl`  | --> | Latin `Latn` |
| Sırpça (Latin) | `sr-Latn` | Latin `Latn` | --> | Kiril `Cyrl`|
| Tamil dili | `ta` | Tamil dili `Taml` | --> | Latin `Latn` |
| Telugu dili | `te` | Telugu dili `Telu` | --> | Latin `Latn` |
| Tay Dili | `th` | Tay dili `Thai` | <--> | Latin `Latn` |

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
| Basitleştirilmiş Çince      | `zh-Hans`          |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
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
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Gaelce      | `cy`          |

## <a name="detect"></a>Detect

Translator metin çevirisi API'si, çeviri ve harf çevirisi için kullanılabilen tüm dilleri algılar.


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
| Basitleştirilmiş Çince      | `zh-Hans`          |
|Geleneksel Çince|   `zh-Hant`   |
| Hırvatça      | `hr`          |
| Çekçe      | `cs`          |
| Danca      | `da`          |
| Felemenkçe      | `nl`          |
| Türkçe    | `en`     |
| Estonca      | `et`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Macarca      | `hu`          |
| İzlanda dili | `is` |
| Endonezya dili|   `id`    |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
|Svahili dili| `sw`    |
| Korece      | `ko`          |
| Letonca      | `lv`          |
| Litvanca      | `lt`          |
|Malgaşça|  `mg`    |
| Norveççe      | `nb`          |
| Lehçe      | `pl`          |
| Portekizce      | `pt`          |
| Rumence      | `ro`          |
| Rusça      | `ru`          |
|Samoaca|    `sm`    |
| Sırpça (Latin)      | `sr-Latn`          |
| Slovakça     | `sk`          |
| Slovence      | `sl`          |
| İspanyolca      | `es`          |
| İsveççe      | `sv`          |
| Tay Dili      | `th`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Vietnam dili      | `vi`          |
| Gaelce | `cy` |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesinde listeye erişme

Dilleri hızlı bir bakış için Microsoft Translator Web sitesi Translator metin ve konuşma API'leri tarafından desteklenen tüm dillerde gösterir. Bu liste, dil kodu gibi geliştirici özgü bilgileri içermez.

[Dilleri listesine bakın](https://www.microsoft.com/translator/languages.aspx)
