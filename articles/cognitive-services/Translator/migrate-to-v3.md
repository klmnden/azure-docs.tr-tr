---
title: V3 - Translator metin çevirisi API'si geçirme
titlesuffix: Azure Cognitive Services
description: V2'den Translator Text API V3 geçirmeyi öğrenin.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: v-jansko
ms.openlocfilehash: eaf65bef28110d73378c213ae4781a409b86e1bd
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128188"
---
# <a name="translator-text-api-v2-to-v3-migration"></a>Translator Text API V2 V3 geçiş

Microsoft Translator takım Translator metin çevirisi API'si, sürüm 3 (V3) kullanıma sundu. Bu sürüm, yeni özellikler, kullanım dışı yöntemler ve gönderme ve Microsoft Translator hizmetten veri almak için yeni bir biçim içerir. Bu belge V3 kullanan uygulamaları değiştirmek için bilgi sağlar. V2 30 Nisan 2018'de kullanım dışı kalacaktır ve 30 Nisan 2019 üzerinde durdurulacaktır.

Bu belgenin sonuna, daha fazla bilgi edinmek için yararlı bağlantılar içerir.

## <a name="summary-of-features"></a>Özelliklerin özeti

* İzleme yok - V3 No-izleme, Azure portalında tüm fiyatlandırma katmanları için geçerlidir. Bu özellik V3 API'sine gönderilen metin kaydedilir, Microsoft tarafından anlamına gelir.
* JSON - XML, JSON değiştirilir. Hizmete gönderilen ve hizmetten alınan tüm veriler JSON biçiminde olan.
* Tek bir istek - Çevir yöntemi birden çok hedef dilde 'to' birden çok dil için çeviri tek bir istek kabul eder. Örneğin, tek bir istek 'from' İngilizce ve 'to' Almanca, İspanyolca ve Japonca veya diğer bir dil grubunu olabilir.
* İki dilli sözlük - API için iki dilli sözlük yöntemi eklendi. Bu yöntem 'Ara' ve 'örnekler' içerir.
* Alfabeye - transliterate yöntemi API için eklendi. Bu yöntem, sözcük ve bir betik (örneğin cümlelerde dönüştürülecektir. Arapça) (örneğin başka bir komut dosyası içine Latin).
* Dil - yeni bir 'diller' yöntemi 'Çevir', 'Sözlüğü' ve 'alfabeye' yöntemleri ile kullanmak için JSON biçiminde dil bilgileri sunar.
* Çeviri - yeni ayrı yöntemler olarak V2 API'si olan özelliklerden bazıları desteklemek için 'Çevir' yöntemi için yeni özellikler eklenmiştir. TranslateArray buna bir örnektir.
* Yöntem konuşun - metin okuma işlevselliği artık Microsoft Translator API'SİNDE desteklenmiyor. Azure Bilişsel hizmetler Bing konuşma API'si metin okuma işlevselliği kullanılabilir.

Aşağıdaki listede yer alan V2 ve V3 yöntemleri V2 ile gelen işlevselliği sağlayan API'ler ve V3 yöntemleri tanımlar.

| V2 API yöntemi   | V3 API uyumluluğu |
|:----------- |:-------------|
| Çevirme     | Çevirme          |
| TranslateArray      | Çevirme          |
| GetLanguageNames      | Diller          |
| GetLanguagesForTranslate     | Diller        |
| GetLanguagesForSpeak      | Bilişsel hizmetler konuşma tanıma API'si         |
| Söyleyin     | Bilişsel hizmetler konuşma tanıma API'si          |
| Algılama     | Algılama         |
| DetectArray     | Algılama         |
| AddTranslation     | Microsoft Translator HUB API         |
| AddTranslationArray    | Microsoft Translator HUB API          |
| BreakSentences      | BreakSentence         |
| GetTranslations      | Özelliği artık desteklenmiyor         |
| GetTranslationsArray      | Özelliği artık desteklenmiyor         |

## <a name="move-to-json-format"></a>JSON biçimine Taşı

Microsoft Translator metin çevirisi V2 kabul edilir ve XML biçiminde veriler döndürdü. V3 sürümünde API'si kullanılarak alınan tüm veriler JSON biçiminde olan. XML bundan böyle kabul edilecek veya V3 sürümünde döndürdü. 

Bu değişiklik V2 metin çevirisi API'si için yazılan bir uygulamanın çeşitli yönlerini etkiler. Örnek: diller API metin çevirisi, harf çevirisi ve iki sözlük yöntemleri için dil bilgilerini döndürür. Tüm dil bilgisinin bir çağrıda tüm yöntemleri için istek veya bunları ayrı ayrı isteyin.

Dilleri yöntemi kimlik doğrulaması gerektirmez; Aşağıdaki bağlantıyı tıklatarak json'da V3 için tüm dil bilgileri görebilirsiniz:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, sözlük, harf çevirisi](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Kimlik Doğrulaması Anahtarı

V2 için kullandığınız kimlik doğrulama anahtarı V3 için kabul edilir. Yeni bir abonelik edinmeniz gerekir değil. V2 XML'den V3 JSON-yine de geçiş sırasında yeni sürümlerini yayınlama kolaylaştırmaya V2 ve V3 yearlong geçiş dönemi boyunca, uygulamalarınızda karıştırmak mümkün olmayacak.

## <a name="pricing-model"></a>Fiyatlandırma Modeli

Microsoft Translator V3 V2 fiyatlandırılır aynı şekilde fiyatlandırılır; karakter boşluklar dahil. V3'daki yeni özelliklerin hangi karakter faturalandırması sayılan birkaç değişiklik yapalım.

| V3 yöntemi   | Faturalandırma için sayılan karakterler |
|:----------- |:-------------|
| Diller     | Gönderilen herhangi bir karakter yok sayılan, ücret alınmaz.          |
| Çevirme     | Sayısı temel karakterlerinin kaçının tutulacağını gönderilen çeviri ve kaç diller için karakter çevrilir. 50 karakteri gönderildi ve istenen 5 dilleri 50 x 5 olacaktır.           |
| Karakter Dönüştürme     | Harf çevirisi için gönderilen karakter sayısına göre sayılır.         |
| Sözlük Arama ve örnek     | Sözlük Arama ve örnekler için gönderilen karakter sayısına göre sayılır.         |
| BreakSentence     | Ücret alınmaz.       |
| Algılama     | Ücret alınmaz.      |

## <a name="v3-end-points"></a>V3 uç noktaları

Genel

* api.cognitive.microsofttranslator.com


## <a name="v3-api-text-translations-methods"></a>V3 API metin çevirileri yöntemleri

[Diller](reference/v3-0-languages.md)

[Çevir](reference/v3-0-translate.md)

[Alfabeye](reference/v3-0-transliterate.md)

[BreakSentence](reference/v3-0-break-sentence.md)

[Algılama](reference/v3-0-detect.md)

[Sözlük/Arama](reference/v3-0-dictionary-lookup.md)

[Sözlük/örnek](reference/v3-0-dictionary-examples.md)

## <a name="customization"></a>Özelleştirme

Microsoft Translator V3 sinirsel makine çevirisi, varsayılan olarak kullanır. Bu nedenle, Microsoft Translator hub'ı ile kullanılamaz. Translator hub'ı yalnızca istatistiksel makine çevirisi eski destekler. Özelleştirme sinirsel çeviri için özel Translator'ı kullanarak kullanıma sunuldu. [Sinirsel makine çevirisi özelleştirme hakkında daha fazla bilgi edinin](customization.md)

Sinirsel çeviri V3 metin tanıma API'si ile standart kategorileri (SMT, konuşma tanıma, metin, generalnn) kullanımını desteklemiyor.


## <a name="links"></a>Bağlantılar

* [Microsoft gizlilik ilkesi](https://privacy.microsoft.com/privacystatement)
* [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal)
* [Çevrimiçi hizmet koşulları](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [V3.0 belgelerini görüntüleyin](reference/v3-0-reference.md)
