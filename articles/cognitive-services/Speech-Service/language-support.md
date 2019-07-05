---
title: Dil desteği - konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Azure konuşma Hizmetleri birlikte konuşma çevirisi, Konuşmayı metne ve metin okuma dönüştürme için çok sayıda dilleri destekler. Bu makalede, hizmet tarafından dil desteği kapsamlı bir listesini sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: c3596f230ace2d19df28d0b43af45c74aaa87836
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561309"
---
# <a name="language-and-region-support-for-the-speech-services"></a>Konuşma Hizmetleri dil ve bölge desteği

Farklı diller için farklı bir konuşma hizmetleri işlevleri desteklenir. Aşağıdaki tablolarda, dil desteği özetler.

## <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Hem Microsoft konuşma tanıma SDK'sı hem de REST API (yerel) aşağıdaki dilleri destekler. Farklı düzeyde özelleştirme, her dil için kullanılabilir.

  Kod | Dil | [Akustik uyarlama](how-to-customize-acoustic-models.md) | [Dil uyarlama](how-to-customize-language-model.md) | [Söyleniş uyarlama](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar EG | Arapça (Mısır) modern standart | Hayır | Evet | Hayır
 CA-ES | Katalanca | Hayır | Hayır | Hayır
 v-DK | Danca (Danimarka) | Hayır | Hayır | Hayır
 de-DE | Almanca (Almanya) | Evet | Evet | Hayır
 tr-AU | İngilizce (Avustralya) | Hayır | Evet | Evet
 CA tr | İngilizce (Kanada) | Hayır | Evet | Evet
 en-GB | İngilizce (Birleşik Krallık) | Hayır | Evet | Evet
 tr-giriş | English (India) | Evet | Evet | Evet
 tr NZ | İngilizce (Yeni Zelanda) | Hayır | Evet | Evet  
 en-US | İngilizce (ABD) | Evet | Evet | Evet
 es-ES | İspanyolca (İspanya) | Evet | Evet | Hayır
 es-MX | İspanyolca (Meksika) | Hayır | Evet | Hayır
 FI-FI | Fince (Finlandiya) | Hayır | Hayır | Hayır
 fr-CA | Fransızca (Kanada) | Hayır | Evet | Hayır
 fr-FR | Fransızca (Fransa) | Evet | Evet | Hayır
 yüksek giriş | Hintçe (Hindistan) | Hayır | Evet | Hayır
 İt-IT | İtalyanca (İtalya) | Evet | Evet | Hayır
 ja-JP | Japonca (Japonya) | Hayır | Evet | Hayır
 ko-KR | Korece (Güney Kore) | Hayır | Evet | Hayır
 NB-yok | Norveççe (Bokmal) (Norveç) | Hayır | Hayır | Hayır
 NL-NL | Hollanda dili (Hollanda) | Hayır | Evet | Hayır
 pl-PL | Lehçe (Polonya) | Hayır | Hayır | Hayır
 pt-BR | Portekizce (Brezilya) | Evet | Evet | Hayır
 pt-PT | Portekizce (Portekiz) | Hayır | Evet | Hayır
 ru-RU | Rusça (Rusya) | Evet | Evet | Hayır
 sv-SE | İsveççe (İsveç) | Hayır | Hayır | Hayır
 zh-CN | Çince (Basitleştirilmiş Mandarin) | Evet | Evet | Hayır
 zh-HK | Çince (Kanton, Geleneksel) | Hayır | Evet | Hayır
 zh-TW | Çince (Tayvan Mandarin) | Hayır | Evet | Hayır
 TH TH | Tayca (Tayland) | Hayır | Hayır | Hayır


## <a name="text-to-speech"></a>Metin okuma

Her biri belirli bir dil ve yerel ayar tarafından tanımlanan diyalekti, destekler, bu seslerle metin okuma REST API destekler.

> [!IMPORTANT]
> Fiyatlandırma için standart, özel ve sinir sesleri değişir. Lütfen [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) ek bilgiler sayfası.

### <a name="neural-voices"></a>Sinir sesleri

Sinir metin okuma, konuşma sentezi derin sinir ağı tarafından desteklenen yeni bir türdür. Sinir sesli Sentezlenen konuşma neredeyse İnsan sonuçlarından ayırt olur.

Sinir sesleri etkileşimleri sohbet robotları ve sanal Yardımcıları ile daha doğal yapmasına ve ilgi çekici, e-kitapları gibi dijital metinleri audiobooks dönüştürmek ve içi navigasyon sistemleri geliştirir kullanılabilir. Kullanıcıların yapay ZEKA sistemlerle etkileşim kurduğunuzda İnsan benzeri doğal prosody ve sözcük Temizle articulation sinir sesleri dinleme yorulma ciddi ölçüde düşürün.

Sinir seslerle ve bölgesel kullanılabilirlik tam listesi için bkz: [bölgeleri](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ses adı
--------|----------|--------|---------|------------
de-DE | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, KatjaNeural)" | "de-DE-KatjaNeural"
en-US | English (US) | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, GuyNeural)" | "en-US-GuyNeural"
en-US | English (US) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaNeural)" | "en-US-JessaNeural"
İt-IT | İtalyanca (İtalya) | Kadın |"Microsoft Server Konuşma metin konuşma ses (it-IT, ElsaNeural)" | "BT BT ElsaNeural"
zh-CN | Çince (ana kara) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, XiaoxiaoNeural)" | "zh-CN-XiaoxiaoNeural"

> [!NOTE]
> Konuşma sentezi isteklerinizi tam hizmet adı eşlemesi ya da kısa ses adı kullanabilirsiniz.

### <a name="standard-voices"></a>Standart sesler

75'den fazla standart sesleri üzerinde 45 diller ve Sentezlenen konuşmaya metin dönüştürmenize olanak sağlayan yerel ayarlar kullanılabilir. Bölgesel kullanılabilirlik hakkında daha fazla bilgi için bkz: [bölgeleri](regions.md#standard-and-neural-voices).

Yerel Ayar | Dil | Cinsiyet | Tam hizmet adı eşleme | Kısa ses adı
-------|----------|---------|----------|----------
ar EG\* | Arapça (Mısır) | Kadın | "Microsoft sunucu konuşma Sesli konuşmayı metne (ar-Örneğin, Hoda)" | "ar-ÖRN-Hoda"
ar-SA | Arapça (Suudi Arabistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (ar-SA, Naayf)" | "ar-SA-Naayf"
BG-BG | Bulgarca | Erkek | "Microsoft Server Konuşma metin okuma ses (bg-BG, çalışan Ivan)" | "bg-BG-çalışan Ivan"
CA-ES | Katalanca (İspanya) | Kadın | "Microsoft Server Konuşma metin okuma ses (ca-ES, HerenaRUS)" | "ca-ES-HerenaRUS"
cs-CZ | Çekçe | Erkek | "Microsoft sunucu konuşma Sesli konuşmayı metne (cs-CZ, Jakub)" | "cs-CZ-Jakub"
v-DK | Danca | Kadın | "Microsoft Server Konuşma metin konuşma ses (v-DK, HelleRUS)" | "da DK HelleRUS"
de-AT | Almanca (Avusturya) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-AT, Michael)" | "de-AT-Michael"
de-CH | Almanca (İsviçre) | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-CH, Karsten)" | "de-CH-Karsten"
de-DE | Almanca (Almanya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, Hedda)" | "de-DE-Hedda"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (de-DE, HeddaRUS)" | "de-DE-HeddaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (de-DE, Stefan, Apollo)" | "de-DE-Stefan-Apollo"
el-GR | Yunanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (el-GR, Stefanos)" | "el-GR-Stefanos"
tr-AU | İngilizce (Avustralya) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, Catherine)" | "en-AU-Catherine"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-AU, HayleyRUS)" | "en-AU-HayleyRUS"
CA tr | İngilizce (Kanada) | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, Gamze)" | "tr-CA-Gamze"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (tr-CA, HeatherRUS)" | "tr-CA-HeatherRUS"
en-GB | English (UK) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, Susan, Apollo)" | "en-GB-Susan-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-GB, HazelRUS)" | "en-GB-HazelRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-GB, George, Apollo)" | "en-GB-George-Apollo"
IE tr | İngilizce (İrlanda) | Erkek | "Microsoft Server Konuşma metin konuşma ses (tr-IE, Sean)" | "tr-IE-Sean"
tr-giriş | English (India) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN Heera, Apollo)" | "en-IN-Heera-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-IN, PriyaRUS)" | "en-IN-PriyaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-IN Ravi, Apollo)" | "en-IN-Ravi-Apollo"
en-US | English (US) | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, ZiraRUS)" | "en-US-ZiraRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, JessaRUS)" | "en-US-JessaRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, BenjaminRUS)" | "en-US-BenjaminRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (en-US, Jessa24kRUS)" | "en-US-Jessa24kRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (en-US, Guy24kRUS)" | "en-US-Guy24kRUS"
es-ES | İspanyolca (İspanya) |Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, Gamze, Apollo)" | "es-ES-Laura-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (es-ES, HelenaRUS)" | "es-ES-HelenaRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-ES, Pablo, Apollo)" | "es-ES-Pablo-Apollo"
es-MX | İspanyolca (Meksika) | Kadın | "Microsoft Server Konuşma metin okuma ses (es-MX, HildaRUS)" | "es-MX-HildaRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (es-MX, Raul Apollo)" | "es-MX-Raul-Apollo"
FI-FI | Fince | Kadın | "Microsoft Server Konuşma metin konuşma ses (fi-FI, HeidiRUS)" | "fi FI HeidiRUS"
fr-CA | Fransızca (Kanada) |Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, Caroline)" | "fr-CA-Caroline"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-CA, HarmonieRUS)" | "fr-CA-HarmonieRUS"
FR-CH | Fransızca (İsviçre)| Erkek | "Microsoft Server Konuşma metin okuma ses (fr-CH, Guillaume)" | "fr-CH-Guillaume"
fr-FR | Fransızca (Fransa)| Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, Julie, Apollo)" | "fr-FR-Julie-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (fr-FR, HortenseRUS)" | "fr-FR-HortenseRUS"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (fr-FR, Paul, Apollo)" | "fr-FR-Paul-Apollo"
He IL| İbranice (İsrail) | Erkek| "Microsoft Server Konuşma metin konuşma ses (he-IL, Asaf)" | "he IL Asaf"
yüksek giriş | Hintçe (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba açma, Kalpana, Apollo)" | "Merhaba-IN-Kalpana-Apollo"
| | |Kadın | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Kalpana)" | "Merhaba-IN-Kalpana"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (Merhaba-IN, Hemant)" | "Merhaba-IN-Hemant"
hr-HR | Hırvatça | Erkek | "Microsoft Server Konuşma metin okuma ses (hr-HR, Matej)" | "hr-HR-Matej"
hu-HU | Macarca | Erkek | "Microsoft Server Konuşma metin konuşma ses (hu-HU, Szabolcs)" | "hu-HU-Szabolcs"
ID | Endonezya dili| Erkek | "Microsoft Server Konuşma metin konuşma ses (-ID, Andika)" | "kimliği kimliği Andika"
İt-IT | İtalyanca | Erkek | "Microsoft Server Konuşma metin konuşma ses (Cosimo, it-IT, Apollo)" | "BT-BT-Cosimo-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (it-IT, LuciaRUS)" | "it-IT-LuciaRUS"
ja-JP | Japonca | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ayumi, Apollo)" | "ja-JP-Ayumi-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ja-JP, Ichiro, Apollo)" | "ja-JP-Ichiro-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ja-JP, HarukaRUS)" | "ja-JP-HarukaRUS"
ko-KR | Korece | Kadın | "Microsoft Server Konuşma metin konuşma ses (ko-KR, HeamiRUS)" | "ko-KR-HeamiRUS"
ms-MY | Malay dili | Erkek | "Microsoft Server Konuşma metin okuma ses (ms MY, Rizwan)" | "ms-MY-Rizwan"
NB-yok | Norveççe | Kadın | "Microsoft Server Konuşma metin okuma ses (nb-yok, HuldaRUS)" | "NO-nb-HuldaRUS"
NL-NL | Felemenkçe | Kadın | "Microsoft Server Konuşma metin okuma ses (nl-NL, HannaRUS)" | "nl-NL-HannaRUS"
pl-PL | Lehçe | Kadın | "Microsoft Server Konuşma metin okuma ses (pl-PL, PaulinaRUS)" | "pl-PL-PaulinaRUS"
pt-BR | Portekizce (Brezilya) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-BR, HeloisaRUS)" | "pt-BR-HeloisaRUS"
| | | Erkek |"Microsoft Server Konuşma metin okuma ses (pt-BR, Daniel, Apollo)" | "pt-BR-Daniel-Apollo"
pt-PT | Portekizce (Portekiz) | Kadın | "Microsoft Server Konuşma metin okuma ses (pt-PT, HeliaRUS)" | "pt-PT-HeliaRUS"
Ro-RO | Rumence | Erkek | "Microsoft Server Konuşma metin konuşma ses (ro-RO, Andrei)" | "ro-RO-Andrei"
ru-RU |Rusça| Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, Irina, Apollo)" | "ru-RU-Irina-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin okuma ses (ru-RU, Pavel, Apollo)" | "ru-RU-Pavel-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin okuma ses (ru-RU, EkaterinaRUS)" | RU-RU-EkaterinaRUS
SK-SK | Slovakça | Erkek | "Microsoft Server Konuşma metin okuma ses (sk-SK, Filip)" | "sk-SK-Filip"
SL SI | Slovence | Erkek | "Microsoft Server Konuşma metin okuma ses (sl-sı, Lado)" | "sl-SI-Lado"
sv-SE | İsveççe | Kadın | "Microsoft Server Konuşma metin konuşma ses (sv-SE, HedvigRUS)" | "sv-SE-HedvigRUS"
Veri-ın | Tamilce (Hindistan) | Erkek | "Microsoft Server Konuşma metin konuşma ses (veri-ın, Valluvar)" | "veri-ın-Valluvar"
metin-giriş | Telugu dili (Hindistan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (metin-IN, Chitra)" | "te-IN-Chitra"
TH TH | Tay Dili | Erkek | "Microsoft Server Konuşma metin okuma ses (th-TH, Pattara)" | "th TH Pattara"
tr-TR | Türkçe | Kadın | "Microsoft Server Konuşma metin okuma ses (tr-TR, SedaRUS)" | "tr-TR-SedaRUS"
VI VN | Vietnam dili | Erkek | "Microsoft Server Konuşma metin okuma ses (vi-VN bir)" | "olduğu gibi vi-VN-bir"
zh-CN | Çince (ana kara) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, HuihuiRUS)" | "zh-CN-HuihuiRUS"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Yaoyao, Apollo)" | "zh-CN-Yaoyao-Apollo"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-CN, Kangkang, Apollo)" | "zh-CN-Kangkang-Apollo"
zh-HK | Çince (Hong Kong) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK Tracy, Apollo)" | "zh-HK-Tracy-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-HK TracyRUS)" | "zh-HK-TracyRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-HK Danny, Apollo)" | "zh-HK-Danny-Apollo"
zh-TW | Çince (Tayvan) | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW Yating, Apollo)" | "zh-TW-Yating-Apollo"
| | | Kadın | "Microsoft Server Konuşma metin konuşma ses (zh-TW, HanHanRUS)" | "zh-TW-HanHanRUS"
| | | Erkek | "Microsoft Server Konuşma metin konuşma ses (zh-TW Zhiwei, Apollo)" | "zh-TW-Zhiwei-Apollo"

\* *ar-ÖRN Modern standart Arapça (MSA) destekler.*

> [!NOTE]
> Konuşma sentezi isteklerinizi tam hizmet adı eşlemesi ya da kısa ses adı kullanabilirsiniz.

### <a name="customization"></a>Özelleştirme

Ses özelleştirme, de-DE, en-GB, en-IN, en-US, es için kullanılabilir-MX, fr-FR, it-IT, pt-BR ve zh-CN. Eğitim verileri, özel sesli modeli eğitmek için sahip eşleşen doğru bir yerel ayar seçin. Örneğin, sahip olduğunuz veri kaydını İngiliz bir Vurgu ile İngilizce konuşulur, en-GB'ı seçin.  

> [!NOTE]
> BI dil modeli eğitimi özel sesli, Çince-İngilizce çift dilli dışında desteklemiyoruz. İngilizce de konuşabilen Çince bir üslup eğitimi istiyorsanız 'Çince İngilizce dilli' seçin. 2000'den fazla konuşma, zh-CN herhangi bir eğitim veri boyutuna sahip başlayabileceğiniz ve en-US dışında bir veri kümesinin tüm bölgelerde üslup eğitimi başlar.

## <a name="speech-translation"></a>Konuşma çevirisi

**Konuşma çevirisi** API, konuşma tanıma ve konuşma tanıma ve konuşma metin çevirisi için farklı dilleri destekler. Kaynak dili her zaman konuşma metin dili tablodan olmalıdır. Hedef diller Çeviri hedef konuşma veya metin olmasına göre değişir. Gelen konuşmaya Çevir birden fazla [60 diller](https://www.microsoft.com/translator/business/languages/). Bu dillerin bir alt kümesi için kullanılabilir [konuşma sentezi](language-support.md#text-languages).

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
| Felemenkçe      | `nl`          |
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
| İspanyolca      | `es`          |
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


## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Tanıma Hizmetleri deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
