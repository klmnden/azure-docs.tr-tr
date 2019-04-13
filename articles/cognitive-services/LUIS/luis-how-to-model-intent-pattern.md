---
title: Doğruluk düzenleri ekleyin
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS) uygulamalarında tahmin doğruluğunu artırmak için desen şablonları ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 202b9632b7a7faaf955874a0300edbe5134b7fa1
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521263"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Nasıl tahmin doğruluğunu artırmak için düzenleri ekleyin
Bir LUIS uygulaması konuşma uç noktası aldıktan sonra kullanmak bir [deseni](luis-concept-patterns.md) sözcük sırasını ve sözcük seçim içindeki bir desenle açığa konuşma için tahmin doğruluğunu artırmak için. Belirli desenleri kullanın [söz dizimi](luis-concept-patterns.md#pattern-syntax) konumunu belirtmek için: [varlıkları](luis-concept-entity-types.md), varlık [rolleri](luis-concept-roles.md)ve isteğe bağlı metin.

## <a name="add-template-utterance-to-create-pattern"></a>Desen oluşturmak için şablon utterance Ekle
1. Adını seçerek uygulamanızı açın **uygulamalarım** sayfasında ve ardından **desenleri** sol bölmede altında **uygulama performansını**.

    ![Desen listesinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Desen için doğru hedefini seçin. 

    ![Hedefi seçin](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Şablon metin şablonu utterance yazın ve Enter'ı seçin. Varlık adı girmek istediğiniz zaman doğru deseni varlık sözdizimini kullanın. Varlık sözdizimi ile başlayan `{`. Varlıklar görüntüler listesi. Doğru varlığı seçin ve ardından Enter'ı seçin. 

    ![Varlık deseni için ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlık içeriyorsa bir [rol](luis-concept-roles.md), tek bir iki nokta rolüyle belirtmek `:`, varlık adı sonra gibi `{Location:Origin}`. Rolleri varlıkların listesini bir liste görüntüler. Rolü seçin ve ardından Enter'ı seçin. 

    ![Rolü içeren varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlık seçtikten sonra deseni girdikten ve ardından Enter'ı seçin. Girme desenleri, işiniz bittiğinde [eğitme](luis-how-to-train.md) uygulamanızı.

    ![Girilen deseninin her iki türdeki varlık ile ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Model desenleri ile değiştirdikten sonra uygulamanızı eğitin
Sonra ekleme, düzenleme, kaldırmak veya bir desen yeniden atama [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

<a name="search-patterns"></a>
<a name="edit-a-pattern"></a>
<a name="reassign-individual-pattern-to-different-intent"></a>
<a name="reassign-several-patterns-to-different-intent"></a>
<a name="delete-a-single-pattern"></a>
<a name="delete-several-patterns"></a>
<a name="filter-pattern-list-by-entity"></a>
<a name="filter-pattern-list-by-intent"></a>
<a name="remove-entity-or-intent-filter"></a>
<a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>

## <a name="use-contextual-toolbar"></a>Bağlamsal araç çubuğu kullan

Desenler listenin üstündeki bağlamsal araç sağlar:

* Desenler arayın
* Bir desen Düzenle
* Farklı bir amaç için ayrı ayrı desen yeniden atama
* Farklı bir hedefi için çeşitli desenlerden yeniden atama
* Delete a tek deseni
* Çeşitli desenlerden Sil
* Varlık tarafından filtre deseni listesi
* Filtre-deseni-liste-tarafından-hedefi
* Varlık veya hedefi Filtreyi Kaldır
* Desen hedefi veya varlık sayfasında mevcut utterance ekleyin

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [desen yapı](luis-tutorial-pattern.md) bir pattern.any ve bir öğretici rolleriyle.
* Bilgi nasıl [eğitme](luis-how-to-train.md) uygulamanızı.
