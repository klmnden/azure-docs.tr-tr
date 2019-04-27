---
title: Önceden oluşturulmuş hedefleri ve varlıkları
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, önceden oluşturulmuş hedefleri ve varlıkları hızlıca hedefi tahmin ve veri ayıklama elde etmek için bir uygulama ekleyin. Herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez. Varlık otomatik olarak algılanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 87e006cc5d56e0c7eb5455147c5ce9eb40afc162
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597450"
---
# <a name="tutorial-identify-common-intents-and-entities"></a>Öğretici: Ortak hedefleri ve varlıkları tanımlama

Bu öğreticide, önceden oluşturulmuş hedefleri ve varlıkları hızlıca hedefi tahmin ve veri ayıklama elde etmek için bir insan kaynakları öğretici uygulamaya ekleyin. Varlık otomatik olarak algıladığından, önceden oluşturulmuş varlıklarla herhangi bir konuşma işaretlemek gerekmez.

Önceden oluşturulmuş modelleri (etki alanları, amaçlarını ve varlıklar) modelinizi hızlı bir şekilde oluşturmanıza yardımcı olur.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Önceden oluşturulmuş amaçları ekleme 
> * Önceden oluşturulmuş varlıkları ekleme 
> * Eğitim 
> * Yayımlama 
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]


## <a name="add-prebuilt-intents-to-help-with-common-user-intentions"></a>Ortak kullanıcı amaçları ile yardımcı olmak için önceden oluşturulmuş hedef ekleme

LUIS, ortak kullanıcı amaçları konusunda yardımcı olmak için önceden oluşturulmuş birçok amaca sahiptir.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Add prebuilt domain intent** (Önceden oluşturulmuş etki alanı amacı ekle) öğesini seçin. 

1. `Utilities` arayın. 

    [![Arama kutusuna yardımcı programları ile önceden oluşturulmuş hedefleri iletişim kutusunun ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

1. Aşağıdaki amaçları ve **Done** (Bitti) öğesini seçin: 

   * Utilities.Cancel
   * Utilities.Confirm
   * Utilities.Help
   * Utilities.StartOver
   * Utilities.Stop

     Bu ıntents kullanıcı, konuşma, nerede ve ne bunlar yapmak kaydolmasını belirlemek yararlıdır. 


## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Ortak veri türü ayıklama yardımcı olmak için önceden oluşturulmuş varlıklar ekleme

LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar. 

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

1. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) düğmesini seçin.

1. Aşağıdaki varlıkların önceden oluşturulmuş varlıklarla listesinden ardından seçin **Bitti**:

   * **[PersonName](luis-reference-prebuilt-person.md)** 
   * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

     ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

     Bu varlıkların adı ve yer tanıma istemci uygulamanıza eklemenize yardımcı olur.

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktasından amacı ve varlık öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Tarayıcı adres çubuğundaki URL'nin sonuna gidip `I want to cancel my trip to Seattle to see Bob Smith` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. 

    ```json
    {
      "query": "I want to cancel my trip to Seattle to see Bob Smith",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.807676256
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.807676256
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0487322025
        },
        {
          "intent": "Utilities.Help",
          "score": 0.0208660364
        },
        {
          "intent": "None",
          "score": 0.008789532
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.006929268
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00136293867
        }
      ],
      "entities": [
        {
          "entity": "seattle",
          "type": "builtin.geographyV2.city",
          "startIndex": 28,
          "endIndex": 34
        },
        {
          "entity": "bob smith",
          "type": "builtin.personName",
          "startIndex": 43,
          "endIndex": 51
        }
      ]
    }
    ```

    Sonucu % 80'e güvenle Utilities.Cancel amacını tahmin ve şehir ve kişi adını veri ayıklanır. 


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

Önceden oluşturulmuş modelleri hakkında daha fazla bilgi edinin:

* [Önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md): Bunlar genel LUIS uygulaması yazma azaltmak ortak etki alanları
* Önceden oluşturulmuş hedefleri: ortak etki alanlarının bireysel hedefleri şunlardır. Tüm etki alanı eklemek yerine tek tek amacı ekleyebilirsiniz.
* [Önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md): ortak veri türleri için çoğu LUIS uygulaması yararlı şunlardır.

LUIS uygulamanızı ile çalışma hakkında daha fazla bilgi edinin:

* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulama, önceden oluşturulmuş varlıklar ekleyerek yaygın kullanıcı amaçlarını belirleyebilir ve ortak veri türlerini ayıklayabilir.  

> [!div class="nextstepaction"]
> [Uygulamaya normal ifade varlığı ekleme](luis-quickstart-intents-regex-entity.md)

