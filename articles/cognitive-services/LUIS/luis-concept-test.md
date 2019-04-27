---
title: LUIS uygulamanızı test edin
titleSuffix: Language Understanding - Azure Cognitive Services
description: Test LUIS için örnek konuşma sağlayan ve LUIS tanınan hedefleri ve varlıkların bir yanıt alma işlemidir.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: c9f1cf80cd3a781e878daca2048f7c5dc9095a7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710627"
---
# <a name="testing-example-utterances-in-luis"></a>Örnek konuşma LUIS test etme

Test LUIS için örnek konuşma sağlayan ve LUIS tanınan hedefleri ve varlıkların bir yanıt alma işlemidir. 

Yapabilecekleriniz [test](luis-interactive-test.md) LUIS etkileşimli olarak, her seferinde bir utterance veya sağlayan bir [batch](luis-concept-batch-test.md) konuşma. Test ile geçerli karşılaştırma [etkin](luis-concept-version.md#active-version) yayımlanan model için model. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>

## <a name="what-is-a-score-in-testing"></a>Test içinde bir puan nedir?
Bkz: [tahmin puanı](luis-concept-prediction-score.md) tahmin puanları hakkında daha fazla bilgi için kavramlar.

## <a name="interactive-testing"></a>Etkileşimli test
Etkileşimli test gelen yapılır **Test** Web sitesinin paneli. Nasıl amaç ve varlıkları tanımlanan ve puanlanmış görmek için bir utterance girebilirsiniz. Bir utterance test bölmesinde beklediğiniz gibi LUIS amaç ve varlıkları tahmin etmeye yönelik değildir, kopyalayın **hedefi** sayfası yeni utterance olarak. Ardından bu utterance bölümlerini etiket ve LUIS eğitin. 

## <a name="batch-testing"></a>Toplu işe testi
Bkz: [toplu test](luis-concept-batch-test.md) aynı anda birden fazla utterance sınıyorsanız.

## <a name="endpoint-testing"></a>Uç nokta test etme
Kullanarak test [uç nokta](luis-glossary.md#endpoint) en fazla iki uygulama sürümü. Sürümünüzle ana veya dinamik olarak ayarlamak uygulama **üretim** uç noktası, ikinci bir sürüme eklemek **hazırlama** uç noktası. Bu yaklaşım bir utterance üç sürümlerini sağlar: Test bölmesi Geçerli modelde [LUIS](luis-reference-regions.md) Web sitesi ve iki farklı uç noktalar iki sürümlerin. 

Tüm uç nokta test sayılarını, kullanım kotası doğru. 

## <a name="do-not-log-tests"></a>Testleri günlüğe kaydetme
Bir uç noktasına karşı test etmek ve günlüğe utterance istemediğiniz kullanmayı unutmayın `logging=false` sorgu dizesi yapılandırma.

## <a name="where-to-find-utterances"></a>Konuşma nerede bulacağını
LUIS, ücretsiz olarak kullanılabilir sorgu günlüğü günlüğe kaydedilen tüm konuşma depolar [LUIS](luis-reference-regions.md) Web sitesi **uygulamaları** LUIS yanı sıra, liste sayfası [yazma API'leri](https://aka.ms/luis-authoring-apis). 

LUIS emin değilseniz, herhangi bir konuşma listelenir **[gözden geçirin, konuşma uç noktası](luis-how-to-review-endpoint-utterances.md)** sayfasının [LUIS](luis-reference-regions.md) Web sitesi. 

![Uç nokta ifadelerini gözden geçirme](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Eğitim unutmayın
Unutmayın [eğitme](luis-how-to-train.md) LUIS modeline değişiklikler yaptıktan sonra. LUIS uygulaması değişiklikleri uygulama eğitildi kadar test görülmez. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [test](luis-interactive-test.md) , konuşma.
