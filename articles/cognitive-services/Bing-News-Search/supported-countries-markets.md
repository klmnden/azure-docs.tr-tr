---
title: Desteklenen ülke ve dilleri Bing Haberler arama API'si Azure üzerinde | Microsoft Docs
description: Hangi ülke ve dilleri Bing görüntü arama API'si tarafından desteklenen bulun.
services: cognitive-services
author: MikeDodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 12/20/2017
ms.author: v-gedod
ms.openlocfilehash: 80326d66e509b64efd5d386fe793bc9942b29ae3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351899"
---
# <a name="bing-news-search-countries-and-languages"></a>Bing Haberler arama ülke ve dilleri

Bing Haberler arama API çok sayıda ülkelerde, birçok birden fazla dili destekler. Öncelikle söz konusu ülkenin ilgi göre arama sonuçlarını daraltmak için bir ülke ile bir sorgu belirtmeyi işlevi görür. Ayrıca, sonuçları Bing bağlantılar içerebilir ve bu bağlantıları Bing kullanıcı deneyimi belirlenen ülke veya dil göre yerelleştirme.

Kullanarak bir ülke belirtebilirsiniz `cc` sorgu parametresi. Bir ülke belirtirseniz kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller ülkeye göre değişiklik gösterir; Her ülkenin pazarda tablosundaki verilir.

Alternatif olarak, Pazar kullanarak belirtebilir `mkt` sorgu parametresi ve bir kodundan **pazarda** tablo. Bir pazar eşzamanlı olarak belirterek, bir ülke ve tercih edilen dili belirtir. `setLang` Sorgu parametresi ayarlanmış olabilir bir dil kodu bu durumda; genellikle bu tarafından belirtilen aynı dildir `mkt` Bing başka bir dilde görmek kullanıcının tercih sürece.

## <a name="supported-markets-for-news-search-endpoint"></a>Pazarda haber arama uç noktası için desteklenen

İçin `/news/search` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanabilir Pazar kodu değerleri `mkt` sorgu parametresi. Bing içerik için yalnızca bu pazarda döndürür. Değiştirilebilir listesidir.  
  
Ülke listesini kodları için belirttiğiniz `cc` sorgu parametresi için bkz: [ülke kodlarına](#countrycodes).  
  
|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------| 
|Danimarka|Danca|da DK|
|Avusturya|Almanca |de AT| 
|İsviçre|Almanca |de CH|
|Almanya|Almanca |de-DE|
|Avustralya|Türkçe|tr AU|
|Kanada|Türkçe|tr-CA|
|Birleşik Krallık|Türkçe|tr GB|
|Endonezya|Türkçe|tr kimliği|
|İrlanda|Türkçe|tr-IE|
|Hindistan|Türkçe|tr-IN|
|Malezya|Türkçe|tr MY|
|Yeni Zelanda|Türkçe|tr NZ|
|Filipinler Cumhuriyeti|Türkçe|en FAZ|
|Singapur|Türkçe|tr SG|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|Genel|tr WW|
|Türkçe|Genel|tr XA|
|Güney Afrika|Türkçe|tr-ZA|
|Arjantin|İspanyolca |ES AR|
|Şili|İspanyolca |ES-CL|
|İspanya|İspanyolca |es-ES|
|Meksika|İspanyolca |es-MX|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD| 
|İspanyolca |Genel|ES-XL|
|Finlandiya|Fince|Fi-FI|  
|Fransa|Fransızca |FR-olabilir|
|Kanada|Fransızca |fr-CA| 
|Belçika|Felemenkçe|NL-olabilir|
|İsviçre|Fransızca |FR CH|
|Fransa|Fransızca |fr-FR|  
|İtalya|İtalyanca|it-IT| 
|Hong Kong Çin ÖİB|Geleneksel Çince|zh-HK|  
|Tayvan|Geleneksel Çince|zh-TW|
|Japonya|Japonca|ja-JP|  
|Kore|Kore dili|ko-KR|  
|Hollanda|Felemenkçe|NL-NL|  
|Çin Halk Cumhuriyeti|Çince|zh-CN|  
|Brezilya|Portekizce|pt-BR|
|Rusya|Rusça|ru-RU|  
|İsveç|İsveç dili|sv-SE|  
|Türkiye|Türkçe|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Haber uç noktası için desteklenen pazarda
İçin `/news` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanabilir Pazar kodu değerleri `mkt` sorgu parametresi. Bing içerik için yalnızca bu pazarda döndürür. Değiştirilebilir listesidir.  
  
Ülke listesini kodları için belirttiğiniz `cc` sorgu parametresi için bkz: [ülke kodlarına](#countrycodes).  
  
|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------| 
|Danimarka|Danca|da DK|
|Almanya|Almanca |de-DE|
|Avustralya|Türkçe|tr AU|
|Birleşik Krallık|Türkçe|tr GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|Genel|tr WW|
|Şili|İspanyolca |ES-CL|
|Meksika|İspanyolca |es-MX|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD| 
|Finlandiya|Fince|Fi-FI|  
|Kanada|Fransızca |fr-CA|
|Fransa|Fransızca |fr-FR|  
|İtalya|İtalyanca|it-IT| 
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Pazarda haber oluşturan eğilim uç noktası için desteklenen
İçin `/news/trendingtopics` uç noktası, aşağıdaki tabloda listelenmektedir belirtmek için kullanabilir Pazar kodu değerleri `mkt` sorgu parametresi. Bing içerik için yalnızca bu pazarda döndürür. Değiştirilebilir listesidir.  
  
Ülke listesini kodları için belirttiğiniz `cc` sorgu parametresi için bkz: [ülke kodlarına](#countrycodes).  
  
|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------| 
|Almanya|Almanca |de-DE|
|Avustralya|Türkçe|tr AU|
|Birleşik Krallık|Türkçe|tr GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Kanada|Türkçe|tr-CA|
|Hindistan|Türkçe|tr-IN|
|Fransa|Fransızca |fr-FR|
|Kanada|Fransızca |fr-CA|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Ülke kodları  

Belirttiğiniz ülke kodlarına şunlardır `cc` sorgu parametresi. Değiştirilebilir listesidir.  
  
|Ülke/bölge|Ülke kodu|  
|---------------------|------------------|  
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
|Hong Kong Çin ÖİB|HK|  
|Hindistan|IN|  
|Endonezya|Kimlik|  
|İtalya|BT|  
|Japonya|JP|  
|Kore|KR|  
|Malezya|UYGULAMAM|  
|Meksika|MX|  
|Hollanda|NL|  
|Yeni Zelanda|NZ|  
|Norveç|HAYIR|  
|Çin Halk Cumhuriyeti|CN =|  
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
Bing Haberler arama uç noktaları hakkında daha fazla bilgi için bkz: [haber arama API v7 başvurusu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).