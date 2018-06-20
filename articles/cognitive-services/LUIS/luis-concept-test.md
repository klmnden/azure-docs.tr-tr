---
title: HALUK uygulamanızı - Azure test | Microsoft Docs
description: Sürekli olarak geliştirebilirsiniz ve kendi dil anlama geliştirmek için uygulamanızın üzerinde çalışmak için dil anlama (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8c702d2adbadd2736eed05c7580e8aabf69affbf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266337"
---
# <a name="testing-in-luis"></a>İçinde HALUK test etme

Test etme için HALUK örnek utterances sağlayarak ve HALUK tanınan hedefleri ve varlıkları yanıtın alma işlemidir. 

Yapabilecekleriniz [test](interactive-test.md) HALUK etkileşimli olarak, her seferinde bir utterance veya sağlayan bir [toplu](luis-concept-batch-test.md) utterances. Testi ile geçerli karşılaştırmak [etkin](luis-concept-version.md#active-version) yayımlanan model için model. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>
## <a name="what-is-a-score-in-testing"></a>Sınama sırasında bir puan nedir?
Bkz: [tahmin puan](luis-concept-prediction-score.md) tahmin puanları hakkında daha fazla bilgi edinmek için kavramlar.

## <a name="interactive-testing"></a>Etkileşimli test etme
Etkileşimli sınama gelen yapılır **Test** Masası Web sitesinin. Nasıl hedefleri ve varlıkları tanımlanan ve puanlanmış görmek için bir utterance girebilirsiniz. Sınama bölmesinde bir utterance üzerinde beklediğiniz gibi HALUK hedefleri ve varlıkları tahmin etmeye değil, kopyalayın **hedefi** sayfa yeni utterance olarak. Ardından bu utterance bölümlerini etiket ve HALUK eğitmek. 

## <a name="batch-testing"></a>Toplu test etme
Bkz: [toplu sınama](luis-concept-batch-test.md) aynı anda birden fazla utterance sınıyorsanız.

## <a name="endpoint-testing"></a>Uç nokta test etme
Kullanarak test [endpoint](luis-glossary.md#endpoint) en fazla iki sürümü, uygulamanızın. Sürümünüzle ana veya dinamik olarak ayarlayın, uygulamanızın **üretim** uç noktası, ikinci bir sürüme eklemek **hazırlama** uç noktası. Bu yaklaşım, bir utterance üç sürümlerini sağlar: Test bölmesi Geçerli modelde [HALUK] [ LUIS] Web sitesi ve iki farklı uç noktalar iki sürümlerin. 

Tüm uç nokta test sayımları kullanım kota bulunun. 

## <a name="do-not-log-tests"></a>Testleri oturum açılamıyor
Bir uç nokta karşı test etmek ve oturum utterance istemiyorsanız, kullanmayı unutmayın `logging=false` sorgu dizesi yapılandırma.

## <a name="where-to-find-utterances"></a>Utterances nerede bulacağını
HALUK indirmek için kullanılabilir sorgu günlüğü günlüğe kaydedilen tüm utterances depolar [HALUK] [ LUIS] Web sitesi **uygulamaları** HALUK yanı sıra, liste sayfası [API'lerini geliştirme ](https://aka.ms/luis-authoring-apis). 

HALUK emin değilseniz, utterances içinde listelenen **[gözden geçirin, uç nokta utterances](label-suggested-utterances.md)** sayfasında [HALUK] [ LUIS] Web sitesi. 

![Uç nokta utterances gözden geçirin](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Eğitmek unutmayın
Unutmayın [eğitmek](luis-how-to-train.md) modele değişiklikleri yaptıktan sonra HALUK. Değişiklikleri HALUK uygulama için uygulama eğitildi kadar testinde görülmez. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [test](interactive-test.md) , utterances.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions