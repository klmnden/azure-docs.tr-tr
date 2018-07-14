---
title: Desteklenen ülkeler/bölgeler ve diller için azure'da Bing resim arama API'si | Microsoft Docs
description: Hangi ülkeler/bölgeler ve diller Bing resim arama API'si tarafından desteklendiğini öğrenin.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 97e2bed509146172c10aa9ac2658b99ed7610fcc
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004447"
---
# <a name="bing-image-search-countriesregions-and-languages"></a>Bing resim arama ülkeler/bölgeler ve diller

Bing resim arama API'si, birden fazla üç düzine ülkeler/bölgeler, çoğu birden fazla dili destekler. Öncelikle bu ülke/bölge ilgi göre arama sonuçlarını daraltmak için bir sorgu ile bir ülke/bölge belirtme işlevi görür. Ayrıca, sonuçları Bing bağlantılarını içerebilir ve bu bağlantıları Bing kullanıcı deneyimini belirtilen ülke/bölge ve dil göre yerelleştirin.

Ülke/Bölge ve dil belirtmek için ayarlayın `mkt` (Pazar) sorgu parametresine bir koddan **pazarlara** aşağıdaki tabloda. Market, bir ülke/bölge ve dil belirtir. Kullanıcının tercih ettiği görmek, metni farklı bir dilde görüntülemek, ayarlamak `setLang` sorgu parametresi için uygun dil kodu.

Alternatif olarak, ülke/bölge kullanarak belirtebilirsiniz `cc` sorgu parametresi. Bir ülke/bölge belirtirseniz, kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller, ülke/bölgeye göre değişiklik gösterir; pazarlara tablodaki her ülke/bölge için verilir.

> [!NOTE]
> Popüler resimler API'si şu anda aşağıdaki borsa destekler:
> - en-US (İngilizce, Amerika Birleşik Devletleri) 
> - tr-CA (İngilizce, Kanada) 
> - tr-AU (İngilizce, Avustralya) 
> - zh-CN (Çince, Çin)

## <a name="countries"></a>Ülkeler

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
|Arjantin|İspanyolca |ES AR|
|Avustralya|Türkçe|tr-AU|
|Avusturya|Almanca |de-AT|
|Belçika|Felemenkçe|NL-olabilir|
|Belçika|Fransızca |FR-olabilir|
|Brezilya|Portekizce|pt-BR|
|Kanada|Türkçe|CA tr|
|Kanada|Fransızca |fr-CA|
|Şili|İspanyolca |ES-CL|
|Danimarka|Danca|v-DK|
|Finlandiya|Fince|FI-FI|
|Fransa|Fransızca |fr-FR|
|Almanya|Almanca |de-DE|
|Hong Kong|Geleneksel Çince|zh-HK|
|Hindistan|Türkçe|tr-giriş|
|Endonezya|Türkçe|tr kimliği|
|İtalya|İtalyanca|it-IT|
|Japonya|Japonca|ja-JP|
|Kore|Kore dili|ko-KR|
|Malezya|Türkçe|MY tr|
|Meksika|İspanyolca |es-MX|
|Hollanda|Felemenkçe|NL-NL|
|Yeni Zelanda|Türkçe|tr NZ|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|Türkçe|tr PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|Türkçe|tr-ZA|
|İspanya|İspanyolca |es-ES|
|İsveç|İsveç dili|sv-SE|
|İsviçre|Fransızca |FR-CH|
|İsviçre|Almanca |de-CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD|

## <a name="next-steps"></a>Sonraki adımlar
Bing haber arama uç noktaları hakkında daha fazla bilgi için bkz. [haber resim arama API'si v7 başvuru](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
