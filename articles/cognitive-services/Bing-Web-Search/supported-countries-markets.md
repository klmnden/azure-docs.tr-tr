---
title: Desteklenen ülke ve dilleri Bing Web arama API'si Azure üzerinde | Microsoft Docs
description: Hangi ülke ve dilleri Bing Web arama API'si tarafından desteklenen bulun.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 7b62c4a4feb7144662a8fe4d692f11f1efe5db1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351808"
---
# <a name="bing-web-search-countries-and-languages"></a>Bing Web araması ülke ve dilleri

Bing Web arama API birden fazla üç düzine ülkelerde, birçok birden fazla dili destekler. Öncelikle söz konusu ülkenin ilgi göre arama sonuçlarını daraltmak için bir ülke ile bir sorgu belirtmeyi işlevi görür. Ayrıca, sonuçları Bing bağlantılar içerebilir ve bu bağlantıları Bing kullanıcı deneyimi belirlenen ülke veya dil göre yerelleştirme.

Kullanarak bir ülke belirtebilirsiniz `cc` sorgu parametresi. Bir ülke belirtirseniz kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller ülkeye göre değişiklik gösterir; Her ülkenin pazarda tablosundaki verilir.

Alternatif olarak, Pazar kullanarak belirtebilir `mkt` sorgu parametresi ve bir kodundan **pazarda** tablo. Bir pazar eşzamanlı olarak belirterek, bir ülke ve tercih edilen dili belirtir. `setLang` Sorgu parametresi ayarlanmış olabilir bir dil kodu bu durumda; genellikle bu tarafından belirtilen aynı dildir `mkt` Bing başka bir dilde görmek kullanıcının tercih sürece.

## <a name="countries"></a>Ülke

|Ülke|Kod|
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


## <a name="markets"></a>Pazarda

|Ülke|Dil|Pazar kodu|
|-------|--------|-----------|
|Arjantin|İspanyolca |ES AR|
|Avustralya|Türkçe|tr AU|
|Avusturya|Almanca |de AT|
|Belçika|Felemenkçe|NL-olabilir|
|Belçika|Fransızca |FR-olabilir|
|Brezilya|Portekizce|pt-BR|
|Kanada|Türkçe|tr-CA|
|Kanada|Fransızca |fr-CA|
|Şili|İspanyolca |ES-CL|
|Danimarka|Danca|da DK|
|Finlandiya|Fince|Fi-FI|
|Fransa|Fransızca |fr-FR|
|Almanya|Almanca |de-DE|
|Hong Kong|Geleneksel Çince|zh-HK|
|Hindistan|Türkçe|tr-IN|
|Endonezya|Türkçe|tr kimliği|
|İtalya|İtalyanca|it-IT|
|Japonya|Japonca|ja-JP|
|Kore|Kore dili|ko-KR|
|Malezya|Türkçe|tr MY|
|Meksika|İspanyolca |es-MX|
|Hollanda|Felemenkçe|NL-NL|
|Yeni Zelanda|Türkçe|tr NZ|
|Norveç|Norveççe|no-NO|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|Türkçe|en FAZ|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA'sı|
|Güney Afrika|Türkçe|tr-ZA|
|İspanya|İspanyolca |es-ES|
|İsveç|İsveç dili|sv-SE|
|İsviçre|Fransızca |FR CH|
|İsviçre|Almanca |de CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|Türkçe|tr GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca |ES-ABD|
