---
title: Dil desteği - Translator metin çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Doğal Translator metin çevirisi API'si tarafından desteklenen dillerin listesi.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 09/25/2018
ms.author: v-jansko
ms.openlocfilehash: 0b1187083c14fc7c536f6a32f3a41957f53f299b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679724"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Translator metin API'si, dil ve bölge desteği

Translator metin çevirisi API'si için metin çevirisi için aşağıdaki dilleri desteklemektedir. Sinirsel makine çevirisi (NMT), yüksek kaliteli yapay ZEKA destekli makine çevirileri için yeni bir standart olduğundan ve sinir sistem kullanılabilir duruma gelince, Translator Text API V3 kullanarak varsayılan kullanılabilir. 

[Makine çevirisi nasıl çalıştığı hakkında daha fazla bilgi edinin](https://www.microsoft.com/translator/mt.aspx)

**V2 Translator API'si**

> [!NOTE]
> V2 30 Nisan 2018'de kullanım dışı bırakıldı ve 30 Nisan 2019 üzerinde durdurulacaktır.

* Yalnızca istatistiksel: hiçbir sinir sistemi bu dil için kullanılabilir.
* Sinir kullanılabilir: sinir sistem kullanılabilir. İlgili parametreyi kullanın `category=generalnn` sinir sistemine erişebilir.
* Sinir varsayılan: sinir, varsayılan çeviri sistemidir. İlgili parametreyi kullanın `category=smt` Microsoft Translator hub'ı ile kullanmak için istatistiksel sistemine erişebilir.
* Yalnızca sinir: yalnızca sinirsel çeviri kullanılabilir.

**Translator API v3** V3 Translator API'si, varsayılan olarak sinir ve istatistiksel sistemleri bulunan ve yalnızca sinir sistemi bulunmayan mevcut olduğunda. Özel Translator sinirsel dillerle yalnızca kullanılabilir. 

|Dil|  Dil kodu|  V2 API'Sİ| V3 API|
|:-----|:-----:|:-----|:-----|
|Afrikaner dili| `af`    |Yalnızca istatistiksel|  Sinir|
|Arapça|    `ar`    |Sinir kullanılabilir|  Sinir|
|Arapça, Levantine| `apc`   |Sinir kullanılabilir|  Sinir|
|Bangla|    `bn`    |Sinir kullanılabilir|  Sinir|
|Boşnakça (Latin)|   `bs`    |Yalnızca istatistiksel|  İstatistik|
|Bulgarca| `bg`    |Sinir kullanılabilir|  Sinir|
|Kanton (Geleneksel)|   `yue`   |Yalnızca istatistiksel|  İstatistik|
|Katalanca|   `ca`    |Yalnızca istatistiksel|  İstatistik|
|Basitleştirilmiş Çince|    `zh-Hans`   |Sinir varsayılan |Sinir|
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
| Arapça | ar | Arapça | <--> | Latin |
|Bangla  | Bn | Bengali | <--> | Latin |
| Çince (Basitleştirilmiş) | zh-Hans | Basitleştirilmiş Çince | <--> | Latin |
| Çince (Basitleştirilmiş) | zh-Hans | Basitleştirilmiş Çince | <--> | Geleneksel Çince |
| Çince (Geleneksel) | zh-Hant | Geleneksel Çince | <--> | Latin |
| Çince (Geleneksel) | zh-Hant | Geleneksel Çince | <--> | Basitleştirilmiş Çince |
| Gucerat dili | gu  | Gucerat dili | --> | Latin |
| İbranice | He | İbranice | <--> | Latin |
| Hintçe | Merhaba | Devanagari | <--> | Latin |
| Japonca | ja | Japonca | <--> | Latin |
| Kannada dili | KN | Kannada dili | --> | Latin |
| Malayalam dili | ML | Malayalam dili | --> | Latin |
| Marathi dili | MR | Devanagari | --> | Latin |
| Odia | or | Odia | <--> | Latin |
| Pencap dili | Pa | Gurmuki | <--> | Latin  |
| Sırpça (Kiril) | SR-Cyrl | Kiril  | --> | Latin |
| Sırpça (Latin) | SR-Latn | Latin | --> | Kiril |
| Tamil dili | veri | Tamil dili | --> | Latin |
| Telugu dili | metin | Telugu dili | --> | Latin |
| Tay Dili | . | Tay Dili | <--> | Latin |

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

## <a name="languages-detected-by-the-detect-method"></a>Algılama yöntemi tarafından algılanan dil

Aşağıdaki dilleri Algıla yöntemi tarafından algılanabilir. Mayıs algılamak Microsoft Translator çeviremez dilleri algılayın.

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
| Çince (Geleneksel) |
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
| Kürtçe (Arapça) |
| Kürtçe (Latin) |
| Latin |
| Letonca |
| Litvanca |
| Makedonca (EYC) |
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

## <a name="access-the-list-programmatically"></a>Listenin programlamayla erişme

Program aracılığıyla dilleri işlemi V3.0 metin API'si kullanarak desteklenen dillerin listesini erişebilirsiniz. İngilizce veya desteklenen herhangi bir dili özelliği, dil kodu yanı sıra, dil adı bazında görüntüleyebilir. Yeni dil kullanılabilir duruma geldiğinde bu liste Microsoft Translator hizmeti tarafından otomatik olarak güncelleştirilir.

[Dilleri işlemi başvuru belgelerini görüntüleyin](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Microsoft Translator Web sitesinde listeye erişme

Dilleri hızlı bir bakış için Microsoft Translator Web sitesi Translator metin ve konuşma API'leri tarafından desteklenen tüm dillerde gösterir. Bu liste, dil kodu gibi geliştirici özgü bilgileri içermez.

[Dilleri listesine bakın](https://www.microsoft.com/translator/languages.aspx)
