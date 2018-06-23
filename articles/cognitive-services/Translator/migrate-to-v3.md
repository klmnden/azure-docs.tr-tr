---
title: Microsoft Çeviricisi metin API - V3 geçirme | Microsoft Docs
description: V2 Çeviricisi metin API V3 geçirmek öğrenin.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: translator
ms.topic: article
ms.date: 03/27/2018
ms.author: v-jansko
ms.openlocfilehash: 16fec351af5b5b3875657ee244c18f305311d965
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354983"
---
# <a name="microsoft-translator-text-api-v2-to-v3-migration"></a>Microsoft Çeviricisi metin API V2 V3 geçiş

Microsoft Translator takım metin çeviri API sürüm 3 (V3) yayımladı. Bu sürüm, yeni özellikler, kullanım dışı yöntemler ve gönderme ve Microsoft Çeviricisi hizmetinden veri almak için yeni bir biçimi içerir. Bu belge V3 kullanan uygulamaları değiştirmek için bilgi sağlar. V2 30 Nisan 2018 kullanım ve 30 Nisan 2019 sona erecek.

Bu belgenin sonuna, daha fazla bilgi edinmek yararlı bağlantılar içerir.

## <a name="summary-of-features"></a>Özelliklerin özeti

* Azure Portalı'ndaki tüm fiyatlandırma katmanlarına hiçbir izleme - V3 No-izleme uygular. Bu V3 API'sine gönderilen metin kaydedilecek, Microsoft tarafından anlamına gelir.
* JSON - XML, JSON değiştirilir. Hizmete gönderilen ve hizmetinden alınan tüm veriler JSON biçiminde değil.
* Tek bir istekte - Çevir yöntemi birden fazla hedef dilde 'to' birden çok dil çeviri tek bir istek için kabul eder. Örneğin, tek bir istek 'from' İngilizce ve 'to' Almanca, İspanyolca ve Japonca veya başka bir dil grubu olabilir.
* İki dilli sözlük - iki dilli sözlük yöntemi API için eklendi. Bu yöntem 'araması' ve 'örnekler' içerir.
* Transliterate - transliterate yöntemi API için eklendi. Bu yöntem sözcükler ve bir komut dosyası (örneğin cümlelerde dönüştürür Arapça) başka bir komut dosyası (örneğin içine Latin).
* Diller - yeni bir 'dilleri' yöntemi 'Çevir', 'Sözlüğü' ve 'transliterate' yöntemleri ile kullanmak için JSON biçiminde dil bilgileri sunar.
* Çeviri - yeni ayrı yöntemleri olarak V2 API'si olan özelliklerden bazıları desteklemek için 'Çevir' yöntemi için yeni özellikler eklenmiştir. TranslateArray örneğidir.
* Yöntem seslendir - metin okuma işlevselliği artık Microsoft Çeviricisi API desteklenmiyor. Microsoft Azure Bilişsel hizmetler Bing konuşma API metin okuma işlevselliği kullanılabilir.

Aşağıdaki listede V2 ve V3 yöntemlerinin V2 ile gelen işlevselliği sağlayacak API'leri ve V3 yöntemleri tanımlar.

| V2 API yöntemi   | V3 API Uyumluluk |
|:----------- |:-------------|
| Çevir     | Çevir          |
| TranslateArray      | Çevir          |
| GetLanguageNames      | Diller          |
| GetLanguagesForTranslate     | Diller        |
| GetLanguagesForSpeak      | Bilişsel hizmetler konuşma API         |
| Seslendir     | Bilişsel hizmetler konuşma API          |
| Algılama     | Algılama         |
| DetectArray     | Algılama         |
| AddTranslation     | Microsoft Çeviricisi HUB API         |
| AddTranslationArray    | Microsoft Çeviricisi HUB API          |
| BreakSentences      | BreakSentence         |
| GetTranslations      | Özelliği artık desteklenmiyor         |
| GetTranslationsArray      | Özelliği artık desteklenmiyor         |

## <a name="move-to-json-format"></a>JSON biçimine taşıma

Microsoft Çeviricisi metin çeviri V2 kabul edilir ve XML biçiminde veri döndürdü. V3 gönderilen ve alınan API kullanarak tüm verileri JSON biçiminde değil. XML artık kabul veya kaldırılacak V3 döndürdü. 

Bu değişiklik V2 metin çeviri API'si için yazılan uygulamanın çeşitli yönlerini etkiler. Örnek: dilleri API metin çevirisi, harf çevirisi ve iki sözlük yöntemleri için dil bilgilerini döndürür. Tüm dil bilgileri bir çağrısında tüm yöntemleri için istek veya bunları tek tek isteyin.

Dilleri yöntemi kimlik doğrulaması gerektirmeyen; Aşağıdaki bağlantıyı tıklatarak JSON V3 için tüm dil bilgileri görebilirsiniz:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, sözlük, harf çevirisi](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Kimlik Doğrulaması Anahtarı

V2 için kullandığınız kimlik doğrulama anahtarı V3 için kabul edilir. Yeni bir abonelik edinmeniz gerekmez. V3-JSON hala V2 XML'den geçirirken yeni sürümlerini yayınlama kolaylaştırma V2 ve V3 uygulamalarınızda yearlong geçiş süresi boyunca karıştırmak kuramaz.

## <a name="pricing-model"></a>Fiyatlandırma Modeli

Microsoft Çeviricisi V3 V2 fiyatlandırılır aynı şekilde fiyatlandırılır; karakter boşluklar dahil olmak üzere. V3'deki yeni özelliklerin hangi karakterlere için fatura sayılan bazı değişiklikler yapın.

| V3 yöntemi   | İçin fatura sayılan karakterler |
|:----------- |:-------------|
| Diller     | Gönderilen hiçbir karakter yok sayılan, herhangi bir ücret alınmaz.          |
| Çevir     | Count kaç karakterin gönderilen temel çeviri ve kaç tane diller için karakterler uygulamasına dönüştürülür. 50 karakter gönderildi ve istenen 5 dilleri 50 x 5 olacaktır.           |
| Transliterate     | Harf çevirisi için gönderilen karakter sayısı sayılır.         |
| Sözlük Arama & örneği     | Sözlük Arama ve örnekler için gönderilen karakter sayısı sayılır.         |
| BreakSentence     | Ücret ödemeden.       |
| Algılama     | Ücret ödemeden.      |

## <a name="v3-end-points"></a>V3 uç noktaları

Genel

* api.cognitive.microsofttranslator.com


## <a name="v3-api-text-translations-methods"></a>V3 API metin çevirileri yöntemleri

[Diller](reference/v3-0-languages.md)

[Çevir](reference/v3-0-translate.md)

[Transliterate](reference/v3-0-transliterate.md)

[BreakSentence](reference/v3-0-break-sentence.md)

[Algıla](reference/v3-0-detect.md)

[Sözlük/Arama](reference/v3-0-dictionary-lookup.md)

[Sözlük/örnek](reference/v3-0-dictionary-examples.md)

## <a name="customization"></a>Özelleştirme

Microsoft Çeviricisi V3 sinir makine çevirisi varsayılan olarak kullanır. Bu nedenle, Microsoft Çeviricisi yalnızca eski istatistiksel makine çevirisi destekleyen Hub ile kullanılamaz. Özelleştirme sinir çeviri için özel Çeviricisi kullanılarak kullanıma sunulmuştur. [Sinir makine çevirisi özelleştirme hakkında daha fazla bilgi edinin](customization.md)

API V3 metinle sinir çeviri standart kategorileri (smt, konuşma, metin, generalnn) kullanımını desteklemez.


## <a name="links"></a>Bağlantılar

* [Microsoft gizlilik ilkesi](https://privacy.microsoft.com/privacystatement)
* [Yasal bilgiler Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Çevrimiçi Hizmet Koşulları'nı](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görünüm V3.0 belgeleri](reference/v3-0-reference.md)
