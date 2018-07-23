---
title: Azure'da LUIS uygulamalarında konuşma | Microsoft Docs
description: Konuşma, Language Understanding Intelligent Service (LUIS) uygulamaları ekleme.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: v-geberr
ms.openlocfilehash: acf328b706a992df03de837ba8837c5810593ae5
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173750"
---
# <a name="utterances-in-luis"></a>LUIS, konuşma

**Konuşma** yorumlamak için uygulamanız gereken kullanıcıdan giriş. Amaç ve varlıkları bunları ayıklanacak LUIS eğitmek için birçok farklı giriş her amaç için yakalamak önemlidir. Etkin öğrenme veya işlemi yeni konuşma üzerinde eğitmek doğurduğu LUIS sağlayan makine öğrenilen zekasından gereklidir.

Kullanıcıların girer düşündüğünüz tümcecikleri toplayın. Aynı anlama gelir, ancak oluşturulan konuşma farklı sözcük uzunluğu ve sözcük yerleştirme dahil. 

## <a name="how-to-choose-varied-utterances"></a>Çeşitli konuşma seçme
Ne zaman önce başlamanıza tarafından [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md) LUIS modelinize göz önünde bulundurmanız bazı ilkeler aşağıda verilmiştir.

### <a name="utterances-arent-always-well-formed"></a>Konuşma her zaman iyi biçimlendirilmiş değil
"Bana Paris bilet kitap" veya "Kayıt" gibi bir cümle bir parçasını gibi bir cümle olabilir ya da "Paris uçuş."  Kullanıcılar, yazım hatalarını genellikle yapın. Uygulamanızı planlarken, kullanıcı girişi için LUIS iletmeden önce yazım olup olmadığını göz önünde bulundurun. [Bing yazım denetimi API'si] [ BingSpellCheck] LUIS ile tümleşir. Uygulamayı yayımladığınızda, Bing yazım denetimi API'si için bir dış anahtarla LUIS uygulamanızı ilişkilendirebilirsiniz. Onay kullanıcı konuşma yazım değil, yazım hatalarını ve yazım hatalarını içeren konuşma üzerinde LUIS eğitme.

### <a name="use-the-representative-language-of-the-user"></a>Kullanıcı Temsilcisi dili kullanın
Konuşma seçerken, bir ortak terimini veya tümceciğini olduğunu düşündüğünüz, istemci uygulamasının tipik bir kullanıcı olmayabileceğini unutmayın. Etki alanı deneyimi olmayabilir. Bu nedenle koşulları ya da tümcelere konusunda uzman olan bir kullanıcı yalnızca diyor kullanırken dikkatli olun.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Yapılar yanı sıra çeşitli terimler seçin
Değiştirilen cümle desenleri oluşturma işlemleri yaparsanız bile bazı sözlük hala yinelenir bulabilirsiniz.

Bu örnek konuşma uygulayın:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
Burada, çekirdek terimi "bilgisayar" değil değiştirilen. Bunlar, masaüstü bilgisayar, dizüstü bilgisayar, iş istasyonu veya hatta yalnızca makine diyebilirsiniz. LUIS akıllıca eş anlamlılar bağlamdan çıkarır, ancak eğitim konuşma oluşturduğunuzda, bunları farklı yine de daha iyi olur.

## <a name="example-utterances-in-each-intent"></a>Her amacı, örnek konuşma
Her amacı örnek konuşma, en az 10 ila 15 olmalıdır. Tüm örnek konuşma sahip olmayan bir hedefi varsa LUIS eğitmek mümkün olmayacaktır. Bir veya daha çok az sayıda örnek Konuşma ile bir hedefi varsa LUIS doğru şekilde amacını tahmin değil. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>10-15 konuşma yazma her yineleme için küçük grupları Ekle
Modelin her yinelemede konuşma büyük bir miktarını eklemeyin. Konuşma onlarca miktarları içinde ekleyin. [Train](luis-how-to-train.md), [yayımlama](luis-how-to-publish-app.md), ve [test](luis-interactive-test.md) yeniden.  

LUIS, etkin dikkatli bir şekilde seçili olan konuşma modelleriyle oluşturur. Çok fazla konuşma eklemek, karışıklığı getirdiği için yararlı değildir.  

Ardından birkaç Konuşma ile başlatmak iyidir [konuşma uç noktası gözden](luis-how-to-review-endoint-utt.md) doğru hedefi tahmin ve varlık ayıklama için.

## <a name="ignoring-words-and-punctuation"></a>Sözcükleri ve noktalama işaretleri yoksayılıyor
Belirli sözcükleri ya da örnek utterance, noktalama işareti yok saymak istiyorsanız, kullanan bir [deseni](luis-concept-patterns.md#pattern-syntax) ile _Yoksay_ söz dizimi. 

## <a name="training-utterances"></a>Eğitim konuşma
Eğitim belirleyici: utterance tahmin sürümleri veya uygulamalar arasında biraz farklı.

## <a name="testing-utterances"></a>Konuşma test etme 

Geliştiriciler, konuşma uç noktaya göndererek gerçek trafiği ile kendi LUIS uygulama testi başlamanız gerekir. Bu konuşma ile varlıkları ve hedefleri performansını artırmak için kullanılan [gözden geçirin, konuşma](luis-how-to-review-endoint-utt.md). Testler bölmesinde test LUIS Web sitesi ile gönderilen uç noktası aracılığıyla gönderilmez ve bu nedenle etkin olarak öğrenmeye katkıda bulunmuyor. 

## <a name="review-utterances"></a>Konuşma gözden geçirin
Modelinizi eğitilen, yayımlanmış ve alıcı sonra [uç nokta](luis-glossary.md#endpoint) sorgular [konuşma gözden](luis-how-to-review-endoint-utt.md) LUIS tarafından önerilen. LUIS hedefi veya varlık için düşük puanlar olan konuşma uç noktası seçer. 

## <a name="best-practices"></a>En iyi uygulamalar
Gözden geçirme [en iyi uygulamalar](luis-concept-best-practices.md) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md) kullanıcı konuşma anlamak için bir LUIS uygulaması eğitim hakkında bilgi.

[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text