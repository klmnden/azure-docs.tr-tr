---
title: Destek Azure'da HALUK uygulamaları kullanarak yerelleştirme | Microsoft Docs
description: HALUK destekleyen dilleri hakkında bilgi edinin.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2017
ms.author: cahann
ms.openlocfilehash: 1eabc01ee07f8791680738a156471e3efe2c44ff
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356114"
---
# <a name="culture-specific-understanding-in-luis-apps"></a>Kültüre özgü HALUK uygulamalarında anlama

HALUK uygulama kültüre özgü ve ayarlandıktan sonra değiştirilemez. 

## <a name="multi-language-luis-apps"></a>Çok dilli HALUK uygulamalar
Çok dilli HALUK istemci uygulaması gibi bir chatbot gerekiyorsa, birkaç seçeneğiniz vardır. HALUK tüm diller destekliyorsa, her dil için HALUK uygulaması geliştirin. Her HALUK uygulamanın benzersiz uygulama kimliği ve uç nokta günlüğü vardır. Dil anlama HALUK desteklemiyor, bir dil için kullanabileceğiniz sağlamak ihtiyacınız varsa [Microsoft Çeviricisi API](../Translator/translator-info-overview.md) utterance desteklenen dile çevirmenizi HALUK uç utterance gönderme ve alma Sonuçta elde edilen puanlar.

## <a name="languages-supported"></a>Desteklenen diller
HALUK utterances aşağıdaki dillerde anlar:


| Dil |Yerel ayar  |  Önceden oluşturulmuş bir etki alanı | Önceden oluşturulmuş varlık | Tümcecik önerileri | **[Metin analizi](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) | 
|--|--|:--:|:--:|:--:|:--:|
| Amerikan İngilizcesi |`en-US` | ✔ | ✔  |✔|✔|
| Kanada Fransızca |`fr-CA` |-|   -   |-|✔|
| *[Çince](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Felemenkçe |`nl-NL` |-|  -   |-|✔|
| Fransızca (Fransa) |`fr-FR` |-| ✔ |✔ |✔|
| Almanca  |`de-DE` |-| ✔ |✔ |✔|
| İtalyanca |`it-IT` |-| ✔ |✔|✔|
| *[Japonca](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Yalnızca anahtar deyimi|
| Kore dili |`ko-KR` |-|   -   |-|Yalnızca anahtar deyimi|
| Portekizce (Brezilya) |`pt-BR` |-| ✔ |✔ |tüm alt kültürler|
| İspanyolca (İspanya) |`es-ES` |-| ✔ |✔|✔|
| İspanyolca (Meksika)|`es-MX` |-|  -   |✔|✔|


Dil desteği değişir [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md) ve [önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md). 

### <a name="chinese-support-notes"></a>* Çince desteği notları

 - İçinde `zh-cn` kültür HALUK yerine geleneksel karakter kümesi Basitleştirilmiş Çince karakter bekliyor.
 - Hedefleri, varlıkları, özellikleri ve normal ifadeler adlarını Çince veya Latin karakterler olabilir.
 - Bkz: [önceden oluşturulmuş etki alanları başvuru ](luis-reference-prebuilt-domains.md) üzerinde önceden oluşturulmuş etki alanları desteklenmektedir içinde bilgi `zh-cn` kültür.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>* Japonca notları desteği

 - HALUK söz dizimi analiz sağlamaz ve Keigo ve resmi olmayan Japonca arasındaki farkı anlamak değil çünkü formality eğitim örnekler, uygulamalarınız için farklı düzeylerde birleştirmek gerekir. 
     - でございます です ile aynı değil. 
     - です だ ile aynı değil. 

### <a name="text-analytics-support-notes"></a>** Metin analizi, notları destekler.
Yalnızca Portekizce subcultures için desteklenir: `pt-PT` ve `pt-BR`. Diğer tüm kültürler birincil kültür düzeyinde desteklenir. Metin analizi hakkında daha fazla bilgi [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). 

### <a name="speech-api-supported-languages"></a>Konuşma API desteklenen diller
Konuşma bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) konuşma dikte modu diller için.

### <a name="bing-spell-check-supported-languages"></a>Desteklenen Bing yazım denetimi dilleri
Bing yazım denetimi bkz [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) desteklenen diller ve durum listesi.

## <a name="rare-or-foreign-words-in-an-application"></a>Bir uygulamada nadir veya yabancı sözcükler
İçinde `en-us` kültür HALUK öğrenir argo dahil olmak üzere en Türkçe sözcükler ayırt etmek. İçinde `zh-cn` kültür HALUK öğrenir çoğu Çince karakterler ayırt etmek. Nadir bir sözcük kullanırsanız `en-us` veya içinde karakter `zh-cn`, ve HALUK bu sözcük veya karakter ayıramadığından görünüyorsa, bu word ekleyebilir veya için karakter gördüğünüz bir [tümcecik liste özelliğini](luis-how-to-add-features.md). Örneğin, sözcükler uygulama--yabancı sözcükler--kültürünü dışında bir tümcecik listesi özelliği eklenmelidir. Bu tümcecik listesi olmayan-birbirinin yerine, HALUK tanımak için öğrenmelisiniz bir sınıf nadir sözcükler kümesini oluşturur, ancak bunlar eş anlamlıları değildir belirtmek için veya birbirleri ile karşılıklı olarak işaretlenmelidir.

### <a name="hybrid-languages"></a>Karma dilleri
Karma dilleri İngilizce ve Çince gibi iki kültürler sözcükleri birleştirin. Bir uygulama üzerinde tek bir kültür bağlı olduğundan bu diller HALUK içinde desteklenmez.

## <a name="tokenization"></a>Simgeleştirme
Machine learning gerçekleştirmek için HALUK bir utterance keser [belirteçleri](luis-glossary.md#token) kültür üzerinde temel. 

|Dil|  Her bir alan veya özel karakter | karakter düzeyi|Bileşik sözcüklerin|[döndürülen parçalanmış varlık](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Çince||✔||✔|
|Felemenkçe|||✔|✔|
|İngilizce (en-us)|✔ ||||
|Fransızca (fr-FR)|✔||||
|Fransızca (fr-CA)|✔||||
|Almanca |||✔|✔|
|İtalyanca|✔||||
|Japonca||||✔|
|Kore dili||✔||✔|
|Portekizce (Brezilya)|✔||||
|İspanyolca (es-ES)|✔||||
|İspanyolca (es-MX)|✔||||

 