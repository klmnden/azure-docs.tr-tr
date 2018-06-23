---
title: Desteklenen ülke ve dilleri için Bing Visual arama API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Hangi ülke ve dilleri Bing Visual arama API'si tarafından desteklenen bulun.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 4723d028cc22caf8be3eb294b52506ec112cbab5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354142"
---
# <a name="bing-visual-search-countries-and-languages"></a>Bing Visual arama ülke ve dilleri

Bing Visual arama API birden fazla üç düzine ülkelerde, birçok birden fazla dili destekler. Her istek, kullanıcının ülke ve tercih ettiğiniz dili eklemeniz gerekir. Kullanıcının Pazar bilerek uygun sonuçları döndüren Bing yardımcı olur. Ülke ve dil belirtmezseniz, Bing kullanıcının ülke ve dil belirlemek için bir en iyi çaba yapar. Sonuçları Bing bağlantılar içerebileceğinden kullanıcı Bing bağlantılar tıklarsa ülke ve dil bilerek tercih edilen bir yerelleştirilmiş Bing kullanıcı deneyimi sağlayabilir.

Ülke ve dil belirtmek için ayarlayın `mkt` (Pazar) sorgu parametresine bir kodundan **pazarda** aşağıdaki tablo. Pazar bir ülke ve dili belirtir. Görmek kullanıcının tercih ediyorsa metni farklı bir dilde görüntülemek, Ayarla `setLang` sorgu parametresi için uygun dil kodu.

Alternatif olarak, ülke kullanarak belirtebilirsiniz `cc` sorgu parametresi. Bir ülke belirtirseniz kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` HTTP üstbilgisi. Desteklenen diller ülkeye göre değişiklik gösterir; Her ülkenin pazarda tablosundaki verilir.



> [!NOTE]
> Aşağıdaki Pazar kısıtlamalar geçerlidir:
> 
> - Görüntü tanıma ek açıklamalar yalnızca İngilizce olarak kullanılabilir. 
> - Yalnızca en-US pazarında tarif, alışveriş ve sayfalar dahil Öngörüler kullanılabilir. 


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
