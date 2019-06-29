---
title: V3 - Translator metin çevirisi API'si geçirme
titlesuffix: Azure Cognitive Services
description: V2'den Translator Text API V3 geçirmeyi öğrenin.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: swmachan
ms.openlocfilehash: 8a2530a4eb4365a022ab11279b344a5d2852430b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448292"
---
# <a name="translator-text-api-v2-to-v3-migration"></a>Translator Text API V2 V3 geçiş

> [!NOTE]
> V2 30 Nisan 2018'de kullanım dışı bırakıldı. Lütfen yalnızca V3 sürümünde kullanılabilir olan yeni işlevsellikten yararlanmak için V3, uygulamalarınızı geçirin.
> 
> 17 Mayıs 2019 üzerinde Microsoft Translator hub'ı kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihleri görüntüleme](https://www.microsoft.com/translator/business/hub/).  

Microsoft Translator takım Translator metin çevirisi API'si, sürüm 3 (V3) kullanıma sundu. Bu sürüm, yeni özellikler, kullanım dışı yöntemler ve gönderme ve Microsoft Translator hizmetten veri almak için yeni bir biçim içerir. Bu belge V3 kullanan uygulamaları değiştirmek için bilgi sağlar. 

Bu belgenin sonuna, daha fazla bilgi edinmek için yararlı bağlantılar içerir.

## <a name="summary-of-features"></a>Özelliklerin özeti

* İzleme yok - V3 No-izleme, Azure portalında tüm fiyatlandırma katmanları için geçerlidir. Bu özellik V3 API'sine gönderilen metin kaydedilir, Microsoft tarafından anlamına gelir.
* JSON - XML, JSON değiştirilir. Hizmete gönderilen ve hizmetten alınan tüm veriler JSON biçiminde olan.
* Tek bir istek - Çevir yöntemi birden çok hedef dilde 'to' birden çok dil için çeviri tek bir istek kabul eder. Örneğin, tek bir istek 'from' İngilizce ve 'to' Almanca, İspanyolca ve Japonca veya diğer bir dil grubunu olabilir.
* İki dilli sözlük - API için iki dilli sözlük yöntemi eklendi. Bu yöntem 'Ara' ve 'örnekler' içerir.
* Alfabeye - transliterate yöntemi API için eklendi. Bu yöntem, sözcük ve bir betik (örneğin cümlelerde dönüştürülecektir. Arapça) (örneğin başka bir komut dosyası içine Latin).
* Dil - yeni bir 'diller' yöntemi 'Çevir', 'Sözlüğü' ve 'alfabeye' yöntemleri ile kullanmak için JSON biçiminde dil bilgileri sunar.
* Çeviri - yeni ayrı yöntemler olarak V2 API'si olan özelliklerden bazıları desteklemek için 'Çevir' yöntemi için yeni özellikler eklenmiştir. TranslateArray buna bir örnektir.
* Yöntem konuşun - metin okuma işlevselliği artık Microsoft Translator API'SİNDE desteklenmiyor. Metin okuma işlevi [Microsoft konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech).

Aşağıdaki listede yer alan V2 ve V3 yöntemleri V2 ile gelen işlevselliği sağlayan API'ler ve V3 yöntemleri tanımlar.

| V2 API yöntemi   | V3 API uyumluluğu |
|:----------- |:-------------|
| `Translate`     | [Çevir](reference/v3-0-translate.md)          |
| `TranslateArray`      | [Çevir](reference/v3-0-translate.md)        |
| `GetLanguageNames`      | [Diller](reference/v3-0-languages.md)         |
| `GetLanguagesForTranslate`     | [Diller](reference/v3-0-languages.md)       |
| `GetLanguagesForSpeak`      | [Microsoft konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech)         |
| `Speak`     | [Microsoft konuşma hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech)          |
| `Detect`     | [Algılama](reference/v3-0-detect.md)         |
| `DetectArray`     | [Algılama](reference/v3-0-detect.md)         |
| `AddTranslation`     | Özelliği artık desteklenmiyor       |
| `AddTranslationArray`    | Özelliği artık desteklenmiyor          |
| `BreakSentences`      | [BreakSentence](reference/v3-0-break-sentence.md)       |
| `GetTranslations`      | Özelliği artık desteklenmiyor         |
| `GetTranslationsArray`      | Özelliği artık desteklenmiyor         |

## <a name="move-to-json-format"></a>JSON biçimine Taşı

Microsoft Translator metin çevirisi V2 kabul edilir ve XML biçiminde veriler döndürdü. V3 sürümünde API'si kullanılarak alınan tüm veriler JSON biçiminde olan. XML bundan böyle kabul edilecek veya V3 sürümünde döndürdü.

Bu değişiklik V2 metin çevirisi API'si için yazılan bir uygulamanın çeşitli yönlerini etkiler. Örneğin: Dilleri API, metin çevirisi, harf çevirisi ve iki sözlük yöntemleri için dil bilgilerini döndürür. Tüm dil bilgisinin bir çağrıda tüm yöntemleri için istek veya bunları ayrı ayrı isteyin.

Dilleri yöntemi kimlik doğrulaması gerektirmez; Aşağıdaki bağlantıyı tıklatarak json'da V3 için tüm dil bilgileri görebilirsiniz:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, sözlük, harf çevirisi](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Kimlik Doğrulaması Anahtarı

V2 için kullandığınız kimlik doğrulama anahtarı V3 için kabul edilir. Yeni bir abonelik edinmeniz gerekir değil. V2 XML'den V3 JSON-yine de geçiş sırasında yeni sürümlerini yayınlama kolaylaştırmaya V2 ve V3 yearlong geçiş dönemi boyunca, uygulamalarınızda karıştırmak mümkün olmayacak.

## <a name="pricing-model"></a>Fiyatlandırma Modeli

Microsoft Translator V3 V2 fiyatlandırılır aynı şekilde fiyatlandırılır; karakter boşluklar dahil. V3'daki yeni özelliklerin hangi karakter faturalandırması sayılan birkaç değişiklik yapalım.

| V3 yöntemi   | Faturalandırma için sayılan karakterler |
|:----------- |:-------------|
| `Languages`     | Gönderilen herhangi bir karakter yok sayılan, ücret alınmaz.          |
| `Translate`     | Sayısı temel karakterlerinin kaçının tutulacağını gönderilen çeviri ve kaç diller için karakter çevrilir. 50 karakteri gönderildi ve istenen 5 dilleri 50 x 5 olacaktır.           |
| `Transliterate`     | Harf çevirisi için gönderilen karakter sayısına göre sayılır.         |
| `Dictionary lookup & example`     | Sözlük Arama ve örnekler için gönderilen karakter sayısına göre sayılır.         |
| `BreakSentence`     | Ücret alınmaz.       |
| `Detect`     | Ücret alınmaz.      |

## <a name="v3-end-points"></a>V3 uç noktaları

Genel

* api.cognitive.microsofttranslator.com

## <a name="v3-api-text-translations-methods"></a>V3 API metin çevirileri yöntemleri

[`Languages`](reference/v3-0-languages.md)

[`Translate`](reference/v3-0-translate.md)

[`Transliterate`](reference/v3-0-transliterate.md)

[`BreakSentence`](reference/v3-0-break-sentence.md)

[`Detect`](reference/v3-0-detect.md)

[`Dictionary/lookup`](reference/v3-0-dictionary-lookup.md)

[`Dictionary/example`](reference/v3-0-dictionary-examples.md)

## <a name="compatibility-and-customization"></a>Uyumluluk ve özelleştirme

> [!NOTE]
> 
> 17 Mayıs 2019 üzerinde Microsoft Translator hub'ı kullanımdan kaldırılacaktır. [Önemli geçiş bilgilerini ve tarihleri görüntüleme](https://www.microsoft.com/translator/business/hub/).   

Microsoft Translator V3 sinirsel makine çevirisi, varsayılan olarak kullanır. Bu nedenle, Microsoft Translator hub'ı ile kullanılamaz. Translator hub'ı yalnızca istatistiksel makine çevirisi eski destekler. Özelleştirme sinirsel çeviri için özel Translator'ı kullanarak kullanıma sunuldu. [Sinirsel makine çevirisi özelleştirme hakkında daha fazla bilgi edinin](custom-translator/overview.md)

Sinirsel çeviri V3 metin tanıma API'si ile standart kategorileri (SMT, konuşma, teknoloji, generalnn) kullanımını desteklemiyor.

| |Uç Nokta|    GDPR işlemci uyumluluğu|  Translator hub'ı kullanın| Özel Translator (Önizleme) kullanma|
|:-----|:-----|:-----|:-----|:-----|
|Translator Text API sürüm 2| api.microsofttranslator.com|    Hayır  |Evet    |Hayır|
|Translator Text API sürüm 3| api.cognitive.microsofttranslator.com|  Evet|    Hayır| Evet|

**Translator Text API sürüm 3**
* Genel olarak kullanılabilir ve tam olarak desteklenen ' dir.
* GDPR işlemci olarak uyumlu olduğundan ve tüm ISO 20001 ve SOC 3 sertifika yanı sıra 20018 gereksinimlerini karşılar. 
* Özel Translator ile (Önizleme), yeni Translator NMT özelleştirme özelliği özelleştirdiğiniz sinir ağı çevirisi sistemleri çağrılacak sağlar. 
* Microsoft Translator hub'ı kullanarak oluşturduğunuz özel çevirisi sistemleri için erişim sağlamaz.

Translator metin çevirisi API'si, sürüm 3 api.cognitive.microsofttranslator.com uç noktası kullanıyorsanız, kullanmakta olduğunuz.

**Translator Text API sürüm 2**
* Tüm 20001,20018 ISO ve SOC 3 sertifika gereksinimlerini karşılamıyor. 
* Translator özelleştirme özelliği ile özelleştirdiğiniz sinir ağı çevirisi sistemleri çağırmak izin vermez.
* Microsoft Translator hub'ı kullanarak oluşturduğunuz özel çevirisi sistemleri erişim sağlar.
* Translator metin çevirisi API'si, sürüm 2, api.microsofttranslator.com uç noktası kullanıyorsanız kullanıyor.

Translator API sürümü yok çevirilerinizi bir kaydını oluşturur. Hiçbir zaman çevirilerinizi kimseyle paylaşılmaz. Daha fazla bilgi [Translator yapmama](https://www.aka.ms/NoTrace) Web sayfası.

## <a name="links"></a>Bağlantılar

* [Microsoft gizlilik ilkesi](https://privacy.microsoft.com/privacystatement)
* [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal)
* [Çevrimiçi hizmet koşulları](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [V3.0 belgelerini görüntüleyin](reference/v3-0-reference.md)
