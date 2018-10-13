---
title: Dil desteği - konuşma tanıma hizmeti API'si
description: Doğal konuşma hizmeti tarafından desteklenen dillerin listesi.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/25/2018
ms.author: erhopf
ms.openlocfilehash: 5bf680d22f990e6a93c622271bbb8ccd4c19437e
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166925"
---
# <a name="language-and-region-support-for-speech-service-api"></a>Konuşma hizmeti API'sini dil ve bölge desteği

Farklı diller için farklı bir konuşma hizmeti işlevleri desteklenir. Aşağıdaki tablolarda, dil desteği özetler.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Microsoft konuşma tanıma API'si aşağıdaki dilleri desteklemektedir. Farklı düzeyde özelleştirme, her dil için kullanılabilir.

  Kod | Dil | [Akustik uyarlama](how-to-customize-acoustic-models.md) | [Dil uyarlama](how-to-customize-language-model.md) | [Söyleniş uyarlama](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar EG | Arapça (Mısır) modern standart | Hayır | Evet | Hayır
 CA-ES | Katalanca (İspanya) | Hayır | Hayır | Hayır
 v-DK | Danca (Danimarka) | Hayır | Hayır | Hayır
 de-DE | Almanca (Almanya) | Evet | Evet | Hayır
 tr-AU | İngilizce (Avustralya) | Hayır | Evet | Evet
 CA tr | İngilizce (Kanada) | Hayır | Evet | Evet
 en-GB | İngilizce (Birleşik Krallık) | Hayır | Evet | Evet
 tr-giriş | English (India) | Evet | Evet | Evet
 tr NZ | İngilizce (Yeni Zelanda) | Hayır | Evet | Evet  
 tr-TR | İngilizce (ABD) | Evet | Evet | Evet
 es-ES | İspanyolca (İspanya) | Hayır | Evet | Hayır
 es-MX | İspanyolca (Meksika) | Hayır | Evet | Hayır
 FI-FI | Fince (Finlandiya) | Hayır | Hayır | Hayır
 fr-CA | Fransızca (Kanada) | Hayır | Evet | Hayır
 fr-FR | Fransızca (Fransa) | Evet | Evet | Hayır
 yüksek giriş | Hintçe (Hindistan) | Hayır | Evet | Hayır
 İt-IT | İtalyanca (İtalya) | Evet | Evet | Hayır
 ja-JP | Japonca (Japonya) | Hayır | Evet | Hayır
 ko-KR | Korece (Kore) | Hayır | Evet | Hayır
 NB-yok | Norveççe (Bokmal) (Norveç) | Hayır | Hayır | Hayır
 NL-NL | Hollanda dili (Hollanda) | Hayır | Evet | Hayır
 pl-PL | Lehçe (Polonya) | Hayır | Hayır | Hayır
 pt-BR | Portekizce (Brezilya) | Hayır | Evet | Hayır
 pt-PT | Portekizce (Portekiz) | Hayır | Evet | Hayır
 ru-RU | Rusça (Rusya) | Evet | Evet | Hayır
 sv-SE | İsveççe (İsveç) | Hayır | Hayır | Hayır
 zh-CN | Çince (Basitleştirilmiş Mandarin) | Evet | Evet | Hayır
 zh-HK | Çince (Mandarin, Geleneksel) | Hayır | Evet | Hayır
 zh-TW | Çince (Tayvan Mandarin) | Hayır | Evet | Hayır
 TH TH | Tay dili (Tayland) | Hayır | Hayır | Hayır


## <a name="text-to-speech"></a>Metin Okuma

Konuşma sentezi API'si her biri belirli bir dil ve yerel ayar tarafından tanımlanan diyalekti, destekleyen aşağıdaki sesi sunar.

Yerel Ayar | Dil | Cinsiyet | Hizmet adı eşleme
-------|----------|---------|--------------------
ar EG\* | Arapça (Mısır) | Kadın | "Microsoft sunucu konuşma Sesli konuşmayı metne (ar-Örneğin, Hoda)"
ar-SA | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (ar-SA, Naayf)"
BG-BG | Bulgarca | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)"
CA-ES | Katalanca (İspanya) | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)"
cs-CZ | Çekçe | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Jakub)"
cs-CZ | Çekçe | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Vit)"
v-DK | Danca | Kadın | "Microsoft Server Konuşma metin konuşma ses (v-DK, HelleRUS)"
de-AT | Almanca (Avusturya) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-AT, Michael)"
de-CH | Almanca (İsviçre) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-CH, Karsten)"
de-DE | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, Hedda)"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, HeddaRUS)"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-DE, Stefan, Apollo)"
el-GR | Yunanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (el-GR, Stefanos)"
tr-AU | İngilizce (Avustralya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, Catherine)"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, HayleyRUS)"
CA tr | İngilizce (Kanada) | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, Gamze)"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, HeatherRUS)"
en-GB | English (UK) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, Susan, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, HazelRUS)"
| | |Erkek | "Microsoft Server Konuşma metin konuşma ses (en-GB, George, Apollo)"
IE tr | İngilizce (İrlanda) |Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Sean)"
IE tr | İngilizce (İrlanda) |Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Shaun)"
tr-giriş | English (India) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN Heera, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN, PriyaRUS)"
| | |Erkek | "Microsoft Server Konuşma metin konuşma ses (en-IN Ravi, Apollo)"
tr-TR | English (US) |Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, ZiraRUS)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaRUS)"
| | |Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, BenjaminRUS)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)"
| | |Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)"
es-ES | İspanyolca (İspanya) |Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)"
| | |Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)"
es-MX | İspanyolca (Meksika) | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul Apollo)"
FI-FI | Fince | Kadın | "Microsoft Server Konuşma metin konuşma ses (fi-FI, HeidiRUS)"
fr-CA | Fransızca (Kanada) |Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)"
FR-CH | Fransızca (İsviçre)|Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)"
fr-FR | Fransızca (Fransa)|Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)"
| | |Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)"
He IL| İbranice (İsrail) | Erkek| "Microsoft Server Konuşma metin konuşma ses (he-IL, Asaf)"
yüksek giriş | Hintçe (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba açma, Kalpana, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Kalpana)"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Hemant)"
hr-HR | Hırvatça | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)"
hu-HU | Macarca | Erkek | "Microsoft Server Konuşma metin konuşma ses (hu-HU, Szabolcs)"
ID | Endonezya dili| Erkek | "Microsoft Server Konuşma metin konuşma ses (-ID, Andika)"
İt-IT | İtalyanca |Erkek | "Microsoft Server Konuşma metin konuşma ses (Cosimo, it-IT, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (it-IT, LuciaRUS)"
ja-JP | Japonca |Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)"
| | |Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)"
ko-KR | Kore dili |Kadın | "Microsoft Server Konuşma metin konuşma ses (ko-KR, HeamiRUS)"
ms-MY | Malay dili | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)"
NB-yok | Norveççe | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-yok, HuldaRUS)"
NL-NL | Hollanda dili | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)"
pl-PL | Lehçe | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)"
pt-BR | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)"
| | |Erkek | "Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)"
pt-PT | Portekizce (Portekiz) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)"
Ro-RO | Rumence | Erkek | "Microsoft Server Konuşma metin konuşma ses (ro-RO, Andrei)"
ru-RU |Rusça| Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)"
| | |Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, EkaterinaRUS)"
SK-SK | Slovakça|Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)"
SL SI | Slovence|Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)"
sv-SE | İsveç dili|Kadın | "Microsoft Server Konuşma metin konuşma ses (sv-SE, HedvigRUS)"
Veri-ın | Tamil dili (Hindistan) |Erkek | "Microsoft Server Konuşma metin konuşma ses (veri-ın, Valluvar)"
metin-giriş | Telugu dili (Hindistan) |Kadın | "Microsoft Server Konuşma metin konuşma ses (metin-IN, Chitra)"
TH TH | Tay Dili|Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)"
tr-TR |Türkçe| Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)"
VI VN | Vietnam dili|Erkek | "Microsoft Server Konuşma metin okuma ses (vi-VN bir)"
zh-CN | Çince (ana kara)|Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, HuihuiRUS)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Yaoyao, Apollo)"
| | |Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Kangkang, Apollo)"
zh-HK | Çince (Hong Kong)|Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK Tracy, Apollo)"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK TracyRUS)"
| || Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-HK Danny, Apollo)"
zh-TW | Çince (Tayvan)|Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW Yating, Apollo)"
| || Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW, HanHanRUS)"
| || Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-TW Zhiwei, Apollo)"

\* *ar-ÖRN Modern standart Arapça (MSA) destekler.*

### <a name="customization"></a>Özelleştirme

Ses özelleştirme ABD İngilizce (en-US), ana kara Çince (zh-CN) ve İtalyanca (it-IT) için kullanılabilir.

> [!NOTE]
> Bir veri kümesi ile 2000'den fazla konuşma İtalyan sesi eğitim başlatır. Çince-İngilizce dilli modelleri da bir ilk konuşma 2000'den fazla veri kümesi ile desteklenir.

## <a name="speech-translation"></a>Konuşma Çevirisi

**Konuşma çevirisi** API, konuşma tanıma ve konuşma tanıma ve konuşma metin çevirisi için farklı dilleri destekler. Kaynak dili her zaman aşağıdaki konuşma dilini tablodan olmalıdır. Hedef diller Çeviri hedef konuşma veya metin olmasına göre değişir.

### <a name="speech-languages"></a>Konuşma dillerini

| Konuşma dili   | Dil kodu |
|:----------- |-|
| Arapça (Modern standart)      | `ar` |
| Çince (Mandarin)      | `zh` |
| Türkçe      | `en` |
| Fransızca      | `fr` |
| Almanca      | `de` |
| İtalyanca      | `it` |
| Japonca      | `jp` |
| Portekizce (Brezilya)     | `pt` |
| Rusça      | `ru` |
| İspanyolca      |  `es` |

### <a name="text-languages"></a>Metin dilleri

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
| Hollanda dili      | `nl`          |
| Türkçe      | `en`          |
| Estonca      | `et`          |
| Fiji Adaları dili      | `fj`          |
| Filipin dili      | `fil`          |
| Fince      | `fi`          |
| Fransızca      | `fr`          |
| Almanca      | `de`          |
| Yunanca      | `el`          |
| Haiti Kreyolu      | `ht`          |
| İbranice      | `he`          |
| Hintçe      | `hi`          |
| Hmong Daw      | `mww`          |
| Macarca      | `hu`          |
| Endonezya dili      | `id`          |
| İtalyanca      | `it`          |
| Japonca      | `ja`          |
| Svahili dili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Kore dili      | `ko`          |
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
| İspanyolca      | `es`          |
| İsveç dili      | `sv`          |
| Tahitian      | `ty`          |
| Tamil dili      | `ta`          |
| Tay Dili      | `th`          |
| Tonga Dili      | `to`          |
| Türkçe      | `tr`          |
| Ukrayna dili      | `uk`          |
| Urduca      | `ur`          |
| Vietnam dili      | `vi`          |
| Galce      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
