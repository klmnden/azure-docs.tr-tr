---
title: "Öğretici 2: Önceden oluşturulmuş amaçlar ve varlıklar - önceden oluşturulmuş yaygın konuşmaları kullanma - LUIS'de ortak veri ayıklama"
titleSuffix: Azure Cognitive Services
description: Hızlıca amaç tahmini ve veri ayıklaması gerçekleştirmek için İnsan Kaynakları öğretici uygulamasına önceden oluşturulmuş amaçlar ve varlıklar ekleyin. Herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez. Varlık otomatik olarak algılanır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 3bad68d1a388a5bc8780df633313206afaadcef9
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422431"
---
# <a name="tutorial-2-identify-common-intents-and-entities"></a>Öğretici 2: Ortak amaçları ve varlıkları tanımlama
Bu öğreticide İnsan Kaynakları uygulamasında değiştireceksiniz. Hızlıca amaç tahmini ve veri ayıklaması gerçekleştirmek için İnsan Kaynakları öğretici uygulamasına önceden oluşturulmuş amaçlar ve varlıklar ekleyin. Varlıklar otomatik algılandığından herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez.

Genel konu etki alanlarının ve veri türlerinin önceden oluşturulmuş modelleri, modelinizi hızla oluşturmanıza yardımcı olmanın yanı sıra modellerin yapısını gösteren bir örnek de sağlar. 

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Önceden oluşturulmuş amaçları ekleme 
> * Önceden oluşturulmuş varlıkları ekleme 
> * Eğitim 
> * Yayımlama 
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-intent-only-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `prebuilts` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez. 

## <a name="add-prebuilt-intents"></a>Önceden oluşturulmuş amaçları ekleme
LUIS, ortak kullanıcı amaçları konusunda yardımcı olmak için önceden oluşturulmuş birçok amaca sahiptir.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Önceden oluşturulmuş amacı ekle**'yi seçin. 

3. `Utilities` arayın. 

    [ ![Arama kutusunda Utilities (Yardımcı Programlar) yazan önceden oluşturulmuş amaçlar iletişim kutusunun ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Aşağıdaki amaçları ve **Done** (Bitti) öğesini seçin: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Stop


## <a name="add-prebuilt-entities"></a>Önceden oluşturulmuş varlıkları ekleme
LUIS, ortak veri ayıklama işlemi için önceden oluşturulmuş birkaç varlık sunar. 

1. Sol gezinti menüsünden **Entities** (Varlıklar) öğesini seçin.

2. **Önceden oluşturulmuş varlıkları yönet** düğmesini seçin.

3. Önceden oluşturulmuş varlık listesinden **number** ve **datetimeV2** girişlerini ve ardından **Done** (Bitti) öğesini seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Tarayıcı adres çubuğundaki URL'nin sonuna gidip `I want to cancel on March 3` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. 

    ```JSON
    {
      "query": "I want to cancel on March 3",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.7818295
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.7818295
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0237864349
        },
        {
          "intent": "GetJobInformation",
          "score": 0.017576348
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0130122062
        },
        {
          "intent": "Utilities.Help",
          "score": 0.006731322
        },
        {
          "intent": "None",
          "score": 0.00524190161
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.004912514
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00092950504
        }
      ],
      "entities": [
        {
          "entity": "march 3",
          "type": "builtin.datetimeV2.date",
          "startIndex": 20,
          "endIndex": 26,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2018-03-03"
              },
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2019-03-03"
              }
            ]
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 26,
          "endIndex": 26,
          "resolution": {
            "value": "3"
          }
        }
      ]
    }
    ```

    Sonuç Utilities.Cancel amacını tahmin etmiş ve 3 Mart tarihi ile 3 rakamını ayıklamıştır. 

    Konuşmada 3 Mart tarihinin geçmişte mi yoksa gelecekte mi olduğu belirtilmediğinden 3 Mart için iki değer vardır. Gerekirse varsayımda bulunma veya netleştirmek üzere soru sorma kararı istemci uygulamasına aittir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulama, önceden oluşturulmuş varlıklar ekleyerek yaygın kullanıcı amaçlarını belirleyebilir ve ortak veri türlerini ayıklayabilir. 

> [!div class="nextstepaction"]
> [Uygulamaya normal ifade varlığı ekleme](luis-quickstart-intents-regex-entity.md)

