---
title: İyi bir örnek konuşma
titleSuffix: Language Understanding - Azure Cognitive Services
description: Konuşma yorumlamak için uygulamanız gereken kullanıcıdan giriş. Kullanıcıların girer düşündüğünüz tümcecikleri toplayın. Aynı anlama gelir, ancak oluşturulan konuşma farklı sözcük uzunluğu ve sözcük yerleştirme dahil.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 2fd3416824189007bfdbe55d30907d9cb56f87ca
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895131"
---
# <a name="understand-what-good-utterances-are-for-your-luis-app"></a>LUIS uygulamanızı iyi konuşma neler olduğunu anlama

**Konuşma** yorumlamak için uygulamanız gereken kullanıcıdan giriş. Amaç ve varlıkları bunları ayıklanacak LUIS eğitmek için her amaç için farklı bir örnek konuşma çeşitli yakalamak önemlidir. Etkin öğrenme veya işlemi yeni konuşma üzerinde eğitmek doğurduğu LUIS sağlayan makine öğrenilen zekasından gereklidir.

Kullanıcıların girer düşündüğünüz konuşma toplayın. Aynı anlama gelir ancak çeşitli yöntemlerle oluşturulur, konuşma şunlardır:

* Utterance uzunluk - kısa, Orta ve uzun, istemci uygulamanızın
* Sözcük ve tümcecik uzunluğu 
* Word yerleştirme - varlık başlangıç, Orta ve utterance sonu
* Dil bilgisi 
* Çoğullaştırmayı
* Dallanma
* İsim ve eylem seçimi
* Noktalama işaretleri - kullanarak doğru yanlış iyi çeşitli ve hiçbir dil bilgisi

## <a name="how-to-choose-varied-utterances"></a>Çeşitli konuşma seçme

Ne zaman önce başlamanıza tarafından [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md) LUIS modelinize göz önünde bulundurmanız bazı ilkeler aşağıda verilmiştir.

### <a name="utterances-arent-always-well-formed"></a>Konuşma her zaman iyi biçimlendirilmiş değil

"Bir bilet Paris için benim için kitap" veya "Kayıt" gibi bir cümle bir parçasını gibi bir cümle olabilir ya da "Paris uçuş."  Kullanıcılar, yazım hatalarını genellikle yapın. Uygulamanızı planlarken, kullandığınız olup olmadığını göz önünde bulundurun [Bing yazım denetimi](luis-tutorial-bing-spellcheck.md) LUIS için iletmeden önce kullanıcı girişi düzeltmek için. 

Onay kullanıcı konuşma yazım değil, yazım hatalarını ve yazım hatalarını içeren konuşma üzerinde LUIS eğitme.

### <a name="use-the-representative-language-of-the-user"></a>Kullanıcı Temsilcisi dili kullanın

Konuşma seçerken, bir ortak terimini veya tümceciğini olduğunu düşünüyorsanız, istemci uygulamanızın normal kullanıcı için doğru olmayabileceğini unutmayın. Etki alanı deneyimi olmayabilir. Koşulları ya da tümcelere konusunda uzman olan bir kullanıcı yalnızca diyor kullanırken dikkatli olun.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Yapılar yanı sıra çeşitli terimler seçin

Değiştirilen cümle desenleri oluşturma işlemleri yaparsanız bile bazı sözlük hala yinelenir bulabilirsiniz.

Bu örnek konuşma uygulayın:

|Örnek konuşmalar|
|--|
|bir bilgisayara nasıl alabilirim?|
|Bir bilgisayar nereden bulabilirim?|
|Bir bilgisayara nasıl adadım gitmeliyim almak istiyorsunuz?|
|Bir bilgisayar zaman olabilir mi?| 

Burada, çekirdek terimi değil "bilgisayar" değişken. Masaüstü bilgisayar, iş istasyonu, dizüstü bilgisayar ya da bile yalnızca makine gibi alternatifleri kullanın. LUIS akıllıca eş anlamlılar bağlamdan çıkarır, ancak eğitim konuşma oluşturduğunuzda, bunları farklı yine de daha iyi olur.

## <a name="example-utterances-in-each-intent"></a>Her amacı, örnek konuşma

Her hedefi en az 15 örnek konuşma olmalıdır. Tüm örnek konuşma sahip olmayan bir hedefi varsa LUIS eğitmek mümkün olmayacaktır. Bir veya daha çok az sayıda örnek Konuşma ile bir hedefi varsa LUIS doğru şekilde amacını tahmin değil. 

## <a name="add-small-groups-of-15-utterances-for-each-authoring-iteration"></a>Küçük geliştirme her yineleme için 15 konuşma grupları Ekle

Modelin her yinelemede konuşma büyük bir miktarını eklemeyin. Konuşma 15 miktarlar ekleme. [Train](luis-how-to-train.md), [yayımlama](luis-how-to-publish-app.md), ve [test](luis-interactive-test.md) yeniden.  

LUIS, etkin LUIS model yazarı tarafından dikkatli bir şekilde seçili olan konuşma modelleriyle oluşturur. Karışıklık getirdiği için çok fazla Konuşma ekleme önemli değildir.  

Ardından birkaç Konuşma ile başlatmak iyidir [konuşma uç noktası gözden](luis-how-to-review-endpoint-utterances.md) doğru hedefi tahmin ve varlık ayıklama için.

## <a name="punctuation-marks"></a>Noktalama işaretleri

Bazı istemci uygulamalar üzerinde bu işaretler anlam yerleştirebilirsiniz çünkü LUIS varsayılan olarak, noktalama işaretleri yoksaymaz. Örnek konuşma noktalama hem hiçbir noktalama için her iki stil sırayla aynı göreli puanları döndürülecek kullandığınızdan emin olun. Noktalama, istemci uygulamasında özel bir anlamı varsa, göz önünde bulundurun [noktalama yoksayılıyor](#ignoring-words-and-punctuation) düzenleri kullanarak. 

## <a name="ignoring-words-and-punctuation"></a>Sözcükleri ve noktalama işaretleri yoksayılıyor

Belirli sözcükleri ya da örnek utterance, noktalama işareti yok saymak istiyorsanız, kullanan bir [deseni](luis-concept-patterns.md#pattern-syntax) ile _Yoksay_ söz dizimi. 

## <a name="training-utterances"></a>Eğitim konuşma

Eğitim genellikle belirleyici: utterance tahmin sürümleri veya uygulamalar arasında biraz farklı. Güncelleştirerek belirleyici eğitim kaldırabilirsiniz [sürüm ayarlarını](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) API'SİYLE `UseAllTrainingData` ad/değer çifti tüm eğitim verilerini kullanmak için.

## <a name="testing-utterances"></a>Konuşma test etme 

Geliştiriciler, konuşma göndererek gerçek trafiği ile kendi LUIS uygulama testi başlamalıdır [tahmin uç nokta](luis-how-to-azure-subscription.md) URL'si. Bu konuşma ile varlıkları ve hedefleri performansını artırmak için kullanılan [gözden geçirin, konuşma](luis-how-to-review-endpoint-utterances.md). Testler bölmesinde test LUIS Web sitesi ile gönderilen uç noktası aracılığıyla gönderilmez ve bu nedenle etkin olarak öğrenmeye katkıda bulunmuyor. 

## <a name="review-utterances"></a>Konuşma gözden geçirin

Modelinizi eğitilen, yayımlanmış ve alıcı sonra [uç nokta](luis-glossary.md#endpoint) sorgular [konuşma gözden](luis-how-to-review-endpoint-utterances.md) LUIS tarafından önerilen. LUIS hedefi veya varlık için düşük puanlar olan konuşma uç noktası seçer. 

## <a name="best-practices"></a>En iyi uygulamalar

Gözden geçirme [en iyi uygulamalar](luis-concept-best-practices.md) ve normal geliştirme döngünüzün bir parçası olarak uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md) kullanıcı konuşma anlamak için bir LUIS uygulaması eğitim hakkında bilgi.

