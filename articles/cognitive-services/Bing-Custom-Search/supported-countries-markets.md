---
title: Desteklenen ülke ve dilleri Bing özel arama API'si Azure üzerinde | Microsoft Docs
description: Hangi ülke ve dilleri Bing özel arama API'si tarafından desteklenen bulun.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/19/2017
ms.author: v-gedod
ms.openlocfilehash: 7ff309f9b789662c4ebd791dffaa2bc2e440763e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352889"
---
# <a name="bing-custom-search-countries-and-languages"></a>Bing özel arama ülke ve dilleri

Bing özel arama API birden fazla üç düzine ülkelerde, birçok birden fazla dili destekler. 

İsteğe bağlı olsa da, istek belirtmelisiniz [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) alınması için sonuçları istediğiniz Pazar tanımlayan sorgu parametresi. İsteğe bağlı sorgu parametrelerin listesi için bkz: [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)

Kullanarak bir ülke belirtebilirsiniz `cc` sorgu parametresi. Bir ülke belirtirseniz kullanarak bir veya daha fazla dil kodunu da belirtmeniz gerekir `Accept-Language` üstbilgi. Desteklenen diller ülkeye göre değişiklik gösterir; Her ülkenin verilen **pazarda** tablo.

`Accept-Language` Üstbilgi ve `setLang` sorgu parametresi karşılıklı olarak birbirini dışlar — her ikisini birden belirtmeyin. Ayrıntılar için bkz [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

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
