---
title: Dil desteği - Bing Web araması API'si
titleSuffix: Azure Cognitive Services
description: Doğal dil, ülke ve Bing haber arama API'si tarafından desteklenen bölgeler listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: e657c4678c76e8ff667c1a3f30409fc157f52d16
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798254"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Bing Web araması API'si için dil ve bölge desteği

Bing Web araması API'si üzerinde üç düzine ülke veya bölgelerde, çoğu birden fazla dili destekler. Bir sorgu ile bir ülke veya bölge belirtilmesi, söz konusu ülke veya bölgelerde ilgi alanlarına göre arama sonuçlarını daraltmak yardımcı olur. Sonuçları Bing bağlantılar içerebilir ve bu bağlantıları Bing kullanıcı deneyimini belirtilen ülke/bölge ve dil göre yerelleştirin.

Bir ülke veya bölge kullanarak belirtebilirsiniz `cc` sorgu parametresi. Ülke veya bölge belirtildiğinde, bir veya daha fazla dil kodlarıyla belirtmelisiniz [ `Accept-Language` üstbilgi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Kullanım [pazarlara tablo](#markets) her pazarında desteklenen dillerin bir listesi için.

Alternatif olarak, Microsoft ile pazara açılarak belirtebilirsiniz `mkt` sorgu parametresi ve bir kod **pazarlara** tablo. Aynı anda bir pazar belirterek, bir ülke veya bölge ve tercih edilen dili belirtir. Dil ile açıkça ayarlayabilirsiniz `setLang` sorgu parametresi.

## <a name="countriesregions"></a>Ülkeler/bölgeler

|Ülke/bölge|Kod|
|-------|----|
|Arjantin|AR|
|Avustralya|Otomatik Olarak Güncelleştir|
|Avusturya|AT|
|Belçika|BE|
|Brezilya|BR|
|Kanada|CA|
|Şili|CL|
|Danimarka|DK|
|Finlandiya|FI|
|Fransa|GS|
|Almanya|DE|
|Hong Kong Çin ÖİB|HK|
|Hindistan|IN|
|Endonezya|Kimlik|
|İtalya|IT|
|Japonya|JP|
|Güney Kore|KR|
|Malezya|MY|
|Meksika|MX|
|Hollanda|NL|
|Yeni Zelanda|NZ|
|Norveç|HAYIR|
|Çin|CN|
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
|Arjantin|İspanyolca |es-AR|
|Avustralya|İngilizce|tr-AU|
|Avusturya|Almanca |de-AT|
|Belçika|Felemenkçe|NL-olabilir|
|Belçika|Fransızca |FR-olabilir|
|Brezilya|Portekizce|pt-BR|
|Kanada|İngilizce|CA tr|
|Kanada|Fransızca |fr-CA|
|Şili|İspanyolca |ES-CL|
|Danimarka|Danca|v-DK|
|Finlandiya|Fince|FI-FI|
|Fransa|Fransızca |fr-FR|
|Almanya|Almanca |de-DE|
|Hong Kong Çin ÖİB|Geleneksel Çince|zh-HK|
|Hindistan|İngilizce|tr-giriş|
|Endonezya|İngilizce|tr kimliği|
|İtalya|İtalyanca |İt-IT|
|Japonya|Japonca|ja-JP|
|Güney Kore|Korece|ko-KR|
|Malezya|İngilizce|MY tr|
|Meksika|İspanyolca |es-MX|
|Hollanda|Felemenkçe|NL-NL|
|Yeni Zelanda|İngilizce|tr NZ|
|Norveç|Norveççe|no-NO|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|İngilizce|tr PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|İngilizce|tr-ZA|
|İspanya|İspanyolca |es-ES|
|İsveç|İsveççe|sv-SE|
|İsviçre|Fransızca |FR-CH|
|İsviçre|Almanca |de-CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|İngilizce|en-GB|
|Amerika Birleşik Devletleri|İngilizce|en-US|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD|

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Resim Arama API’si başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
