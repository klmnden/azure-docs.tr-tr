---
title: HALUK uygulamanızı - Azure eğitmek | Microsoft Docs
description: Modelinizi eğitmek için dil anlama (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 4593954e9e0a60beaa5ee86df848f908b23c6b20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355432"
---
# <a name="train-your-luis-app"></a>HALUK uygulamanızı eğitme

Eğitim doğal dil anlama artırmak için dil anlama (HALUK) uygulamanızı eğitme işlemidir. Ekleme, düzenleme, etiketleme veya varlıklar, hedefleri veya utterances silme gibi model güncellemelerini sonra HALUK uygulamanızı eğitmek. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Eğitim ve [sınama](luis-concept-test.md) bir uygulama yinelemeli bir işlemdir. HALUK uygulamanızı eğitmek sonra hedefleri ve varlıkları doğru tanınan olmadığını görmek için örnek utterances ile test edin. Değilseniz, güncelleştirmelerinin HALUK uygulaması, eğitme ve test için yeniden olması. 

## <a name="train-your-app"></a>Uygulamanızı eğitme
Yinelemeli işlemini başlatmak için önce HALUK uygulamanızı en az bir kez eğitmek gerekir. Eğitim önce en az bir utterance her hedefi olduğundan emin olun.

1. Şirket adını seçerek uygulamanızı erişim **My uygulamaları** sayfası. 

2. Uygulamanızda seçin **tren** üst panelinde. 

    ![Tren düğmesi](./media/luis-how-to-train/train-button.png)

3. Eğitim tamamlandığında yeşil bildirim çubuğu tarayıcısının üstünde görünür.

    ![Tren & Test uygulama sayfası](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Örnek utterances içermeyen bir veya daha fazla amacı, uygulamanızda varsa, uygulamanızı eğitmek olamaz. Tüm amaçlar için utterances ekleyin. Daha fazla bilgi için bkz: [örnek utterances eklemek](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Önerilen utterances HALUK ile etiket](Label-Suggested-Utterances.md) 
* [HALUK uygulamanızın performansını artırmak için özelliklerini kullanma](luis-how-to-add-features.md) 