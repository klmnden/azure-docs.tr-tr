---
title: LUIS uygulamanızı eğitin
titleSuffix: Azure Cognitive Services
description: Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulamanızı eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 6ed76e35ce07f2848c67ef007ad7d3f062f465f7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036771"
---
# <a name="train-your-luis-app"></a>LUIS uygulamanızı eğitin

Eğitim, doğal dil anlama geliştirmek için Language Understanding (LUIS) uygulamanızı eğitiminde işlemidir. Ekleme, düzenleme, etiketleme veya varlıkları, amacı veya konuşma silme gibi bir Modeli'ne güncelleştirmelerinden sonra LUIS uygulamanızı eğitin. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Eğitim ve [test](luis-concept-test.md) uygulama yinelemeli bir işlemdir. LUIS uygulamanızı eğitme sonra varlıkları ve hedefleri doğru olarak tanınır olmadığını görmek için örnek Konuşma ile test edin. Değilseniz, güncelleştirmeleri LUIS uygulaması, eğitin ve test için yeniden yapın. 

## <a name="train-your-app"></a>Uygulamanızı eğitme
Yinelemeli işlemini başlatmak için önce en az bir kez LUIS uygulamanızı geliştirmek gerekir. Eğitim önce en az bir utterance her hedefi olduğundan emin olun.

1. Adını seçerek uygulamanıza erişmek **uygulamalarım** sayfası. 

2. Uygulamanızda seçin **eğitme** üst panelinde. 

3. Alıştırma tamamlandıktan sonra yeşil bildirim çubuğu tarayıcı üst kısmında görünür.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Örnek konuşma içermeyen bir veya daha fazla ıntents uygulamanızda varsa, uygulamanızı eğitme olamaz. Tüm amaçlar için konuşma ekleyin. Daha fazla bilgi için [örnek Konuşma ekleme](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Sonraki adımlar

* [LUIS ile önerilen konuşma etiket](luis-how-to-review-endoint-utt.md) 
* [LUIS uygulamanızın performansını artırmak için özellikleri kullanın](luis-how-to-add-features.md) 