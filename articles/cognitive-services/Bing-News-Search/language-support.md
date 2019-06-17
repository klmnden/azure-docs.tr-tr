---
title: Dil desteği - Bing haber arama API'si
titleSuffix: Azure Cognitive Services
description: Doğal dil, ülke ve Bing haber arama API'si tarafından desteklenen bölgeler listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: d15058126f43fff328acfc563ffd081164a69a90
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384977"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Bing haber arama API'si için dil ve bölge desteği

Bing haber arama API'si, çok sayıda ülkeler/bölgeler, çoğu birden fazla dili destekler. Öncelikle bu ülke/bölge ilgi göre arama sonuçlarını daraltmak için bir sorgu ile bir ülke/bölge belirtme işlevi görür. Ayrıca, sonuçları Bing bağlantılarını içerebilir ve bu bağlantıları Bing kullanıcı deneyimini belirtilen ülke/bölge ve dil göre yerelleştirin.

Kullanarak bir ülke/bölge belirtin `cc` sorgu parametresi. Bir ülke/bölge belirtirseniz, kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller countr/bölgeye göre değişir; pazarlara tablodaki her ülke/bölge için verilir.

Alternatif olarak, Pazar kullanarak belirtebilir `mkt` sorgu parametresi ve bir kod **pazarlara** tablo. Aynı anda bir pazar belirterek, bir ülke/bölge ve tercih edilen dili belirtir. `setLang` Sorgu parametresi ayarlanabilir bir dil kodu bu durumda; genellikle bu tarafından belirtilen aynı dilde `mkt` başka bir dilde Bing görmek kullanıcının tercih ettiği sürece.

## <a name="supported-markets-for-news-search-endpoint"></a>Pazarlara haber arama uç noktası için desteklenen

İçin `/news/search` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanın Pazar kod değerleri `mkt` sorgu parametresi. Bing içerik yalnızca bu pazarlar için döndürür. Değiştirilebilir listesidir.  

Ülke/bölge listesi, kodları için belirttiğiniz `cc` sorgu parametresi, bkz: [ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kod|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|v-DK|
|Avusturya|Almanca|de-AT|
|İsviçre|Almanca|de-CH|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Kanada|Türkçe|CA tr|
|Birleşik Krallık|Türkçe|en-GB|
|Endonezya|Türkçe|tr kimliği|
|İrlanda|Türkçe|IE tr|
|Hindistan|Türkçe|tr-giriş|
|Malezya|Türkçe|MY tr|
|Yeni Zelanda|Türkçe|tr NZ|
|Filipinler Cumhuriyeti|Türkçe|tr PH|
|Singapur|Türkçe|tr SG|
|Amerika Birleşik Devletleri|Türkçe|en-US|
|Türkçe|Genel|tr WW|
|Türkçe|Genel|XA tr|
|Güney Afrika|Türkçe|tr-ZA|
|Arjantin|İspanyolca|es-AR|
|Şili|İspanyolca|ES-CL|
|İspanya|İspanyolca|es-ES|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|ES-ABD|
|İspanyolca|Genel|ES XL|
|Finlandiya|Fince|FI-FI|  
|Fransa|Fransızca|FR-olabilir|
|Kanada|Fransızca|fr-CA|
|Belçika|Felemenkçe|NL-olabilir|
|İsviçre|Fransızca|FR-CH|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|İt-IT|
|Hong Kong SAR|Geleneksel Çince|zh-HK|  
|Tayvan|Geleneksel Çince|zh-TW|
|Japonya|Japonca|ja-JP|  
|Güney Kore|Korece|ko-KR|  
|Hollanda|Felemenkçe|NL-NL|  
|Çin Halk Cumhuriyeti|Çince|zh-CN|  
|Brezilya|Portekizce|pt-BR|
|Rusya|Rusça|ru-RU|  
|İsveç|İsveççe|sv-SE|  
|Türkiye|Türkçe|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Haber uç noktası için desteklenen pazarlar
İçin `/news` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanın Pazar kod değerleri `mkt` sorgu parametresi. Bing içerik yalnızca bu pazarlar için döndürür. Değiştirilebilir listesidir.  

Ülke/bölge listesi, kodları için belirttiğiniz `cc` sorgu parametresi, bkz: [ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kod|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|v-DK|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|en-US|
|Türkçe|Genel|tr WW|
|Şili|İspanyolca|ES-CL|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|ES-ABD|
|Finlandiya|Fince|FI-FI|  
|Kanada|Fransızca|fr-CA|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|İt-IT|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Pazarlara haber popüler uç noktası için desteklenen
İçin `/news/trendingtopics` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanın Pazar kod değerleri `mkt` sorgu parametresi. Bing içerik yalnızca bu pazarlar için döndürür. Değiştirilebilir listesidir.  

Ülke/bölge listesi, kodları için belirttiğiniz `cc` sorgu parametresi, bkz: [ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kod|  
|---------------------|--------------|-----------------|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|en-US|
|Kanada|Türkçe|CA tr|
|Hindistan|Türkçe|tr-giriş|
|Fransa|Fransızca|fr-FR|
|Kanada|Fransızca|fr-CA|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Ülke Kodları  

Belirttiğiniz ülke/bölge kodları şunlardır `cc` sorgu parametresi. Değiştirilebilir listesidir.  

|Ülke/bölge|Ülke kodu|  
|---------------------|------------------|  
|Arjantin|AR|  
|Avustralya|AU|  
|Avusturya|AT|  
|Belçika|BE|  
|Brezilya|BR|  
|Kanada|CA|  
|Şili|CL|  
|Danimarka|DK|  
|Finlandiya|FI|  
|Fransa|GS|  
|Almanya|DE|  
|Hong Kong SAR|HK|  
|Hindistan|IN|  
|Endonezya|Kimlik|  
|İtalya|BT|  
|Japonya|JP|  
|Güney Kore|KR|  
|Malezya|MY|  
|Meksika|MX|  
|Hollanda|NL|  
|Yeni Zelanda|NZ|  
|Norveç|NO|  
|Çin Halk Cumhuriyeti|CN|  
|Polonya|PL|  
|Portekiz|PT|  
|Filipinler Cumhuriyeti|PH|  
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

## <a name="next-steps"></a>Sonraki adımlar
Bing haber arama uç noktaları hakkında daha fazla bilgi için bkz. [haber arama API'si v7 başvuru](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference).
