---
title: Dil desteği - Bing Web araması API'si
titleSuffix: Azure Cognitive Services
description: Doğal dil, ülke ve Bing haber arama API'si tarafından desteklenen bölgeler listesi.
services: cognitive-services
author: v-jerkin
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 09/25/2018
ms.author: erhopf
ms.openlocfilehash: c15e1ddd35e625a713ff569f26e9312d9dcd0bc8
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435610"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Bing Web araması API'si için dil ve bölge desteği

Bing Web araması API'si üzerinde üç düzine ülke veya bölgelerde, çoğu birden fazla dili destekler. Bir sorgu ile bir ülke veya bölge belirtilmesi, söz konusu ülke veya bölgelerde ilgi alanlarına göre arama sonuçlarını daraltmak yardımcı olur. Sonuçları Bing bağlantılar içerebilir ve bu bağlantıları Bing kullanıcı deneyimini belirtilen ülke/bölge ve dil göre yerelleştirin.

Bir ülke veya bölge kullanarak belirtebilirsiniz `cc` sorgu parametresi. Ülke veya bölge belirtildiğinde, bir veya daha fazla dil kodlarıyla belirtmelisiniz [ `Accept-Language` üstbilgi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Kullanım [pazarlara tablo](#Markets) her pazarında desteklenen dillerin bir listesi için.

Alternatif olarak, Microsoft ile pazara açılarak belirtebilirsiniz `mkt` sorgu parametresi ve bir kod **pazarlara** tablo. Aynı anda bir pazar belirterek, bir ülke veya bölge ve tercih edilen dili belirtir. Dil ile açıkça ayarlayabilirsiniz `setLang` sorgu parametresi.

## <a name="countriesregions"></a>Ülkeler/bölgeler

|Ülke/bölge|Kod|
|-------|----|
|Arjantin|AR|
|Avustralya|Otomatik Olarak Güncelleştir|
|Avusturya|AT|
|Belçika|OLABİLİR|
|Brezilya|BR|
|Kanada|CA|
|Şili|CL|
|Danimarka|DK|
|Finlandiya|FI|
|Fransa|GS|
|Almanya|GİZLE|
|Hong Kong|HK|
|Hindistan|GİRİŞ|
|Endonezya|Kimlik|
|İtalya|BT|
|Japonya|JP|
|Kore|KR|
|Malezya|BİLGİSAYARIM|
|Meksika|MX|
|Hollanda|NL|
|Yeni Zelanda|NZ|
|Norveç|HAYIR|
|Çin|CN =|
|Polonya|PL|
|Portekiz|PT|
|Filipinler|PH|
|Rusya|RU|
|Suudi Arabistan|SA|
|Güney Afrika|ZA|
|İspanya|ES|
|İsveç|SE|
|İsviçre|CH|
|Tayvan|TW|
|Türkiye|TR|
|Birleşik Krallık|GB|
|Amerika Birleşik Devletleri|ABD|

## <a name="markets"></a>Pazarlar

|Ülke/bölge|Dil|Pazar kod|
|-------|--------|-----------|
|Arjantin|İspanyolca|ES AR|
|Avustralya|Türkçe|tr-AU|
|Avusturya|Almanca|de-AT|
|Belçika|Hollanda dili|NL-olabilir|
|Belçika|Fransızca|FR-olabilir|
|Brezilya|Portekizce|pt-BR|
|Kanada|Türkçe|CA tr|
|Kanada|Fransızca|fr-CA|
|Şili|İspanyolca|ES-CL|
|Danimarka|Danca|v-DK|
|Finlandiya|Fince|FI-FI|
|Fransa|Fransızca|fr-FR|
|Almanya|Almanca|de-DE|
|Hong Kong|Geleneksel Çince|zh-HK|
|Hindistan|Türkçe|tr-giriş|
|Endonezya|Türkçe|tr kimliği|
|İtalya|İtalyanca|İt-IT|
|Japonya|Japonca|ja-JP|
|Kore|Kore dili|ko-KR|
|Malezya|Türkçe|MY tr|
|Meksika|İspanyolca|es-MX|
|Hollanda|Hollanda dili|NL-NL|
|Yeni Zelanda|Türkçe|tr NZ|
|Norveç|Norveççe|no-NO|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|Türkçe|tr PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|Türkçe|tr-ZA|
|İspanya|İspanyolca|es-ES|
|İsveç|İsveç dili|sv-SE|
|İsviçre|Fransızca|FR-CH|
|İsviçre|Almanca|de-CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca|ES-ABD|
