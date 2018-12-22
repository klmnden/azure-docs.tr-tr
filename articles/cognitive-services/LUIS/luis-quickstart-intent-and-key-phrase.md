---
title: Anahtar tümcecik ayıklama
titleSuffix: Azure Cognitive Services
description: Konuşmaların ana konusunu ayıklamak için önceden oluşturulmuş keyPhrase varlığını kullanın. Herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez. Varlık otomatik olarak algılanır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 7de18cc0ed2e8284e020a1ae6fb9c876ed3d2197
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53718401"
---
# <a name="tutorial-8-extract-key-phrases-of-utterance"></a>Öğretici 8: Utterance, anahtar tümcecikleri ayıklayın
Bu öğreticide konuşmalardan ana konuyu ayıklamak için önceden oluşturulmuş keyPhrase varlığı kullanılmaktadır. Herhangi bir konuşmayı önceden oluşturulmuş varlıklarla etiketlemeniz gerekmez. Varlık otomatik olarak algılanır.

Aşağıdaki konuşmalarda anahtar ifade örnekleri gösterilmektedir:

|Konuşma|keyPhrase varlığı değerleri|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Önümüzdeki yıl daha düşük kesintili yeni bir sağlık planı sunuluyor mu?)|"lower deductible" (daha düşük kesinti)<br>"new medical plan" (yeni sağlık planı)<br>"year" (yıl)|
|Is vision therapy covered in the high deductible medical plan? (Göz tedavisi daha yüksek kesintili sağlık planına dahil mi?)|"high deductible medical plan" (yüksek kesintili sağlık planı)<br>"vision therapy" (göz tedavisi)|

İstemci uygulamanız konuşmanın bir sonraki adımını belirlemek için bu değerleri diğer ayıklanan varlıklarla birlikte kullanabilir.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * keyPhrase varlığını ekleme 
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-simple-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `keyphrase` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="add-keyphrase-entity"></a>keyPhrase varlığını ekleme 
Konuşmaların konusunu ayıklamak için önceden oluşturulmuş keyPhrase varlığını ekleyin.

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Sol menüden **Entities** (Varlıklar) öğesini seçin.

3. **Add prebuilt entity** (Önceden oluşturulan varlık ekle) öğesini seçin.

4. Açılan iletişim kutusunda **keyPhrase** öğesini ve ardından **Done** (Bitti) öğesini seçin. 

    [ ![Entities (Varlıklar) listesi açılan iletişim kutusu ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

5. Sol menüden **Intents** (Amaçlar) öğesini ve ardından **Utilities.Confirm** amacını seçin. keyPhrase varlığı birkaç konuşmada etiketlenmiştir. 

    [ ![keyPhrases etiketli konuşmalarla Utilities.Confirm amacının ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `does form hrf-123456 cover the new dental benefits and medical plan` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. 
    
    ```json
    {
      "query": "does form hrf-123456 cover the new dental benefits and medical plan",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9300641
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9300641
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0359598845
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0141798034
        },
        {
          "intent": "MoveEmployee",
          "score": 0.0112197418
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00507669244
        },
        {
          "intent": "None",
          "score": 0.00238501839
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00202810857
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00102957746
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0008688423
        },
        {
          "intent": "Utilities.Confirm",
          "score": 3.557994E-05
        }
      ],
      "entities": [
        {
          "entity": "hrf-123456",
          "type": "HRF-number",git 
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "new dental benefits",
          "type": "builtin.keyPhrase",
          "startIndex": 31,
          "endIndex": 49
        },
        {
          "entity": "medical plan",
          "type": "builtin.keyPhrase",
          "startIndex": 55,
          "endIndex": 66
        },
        {
          "entity": "hrf",
          "type": "builtin.keyPhrase",
          "startIndex": 10,
          "endIndex": 12
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        }
      ]
    }
    ```

    Kullanıcı form ararken formu bulmak için gerekli olandan daha fazla bilgi sağladı. Ek bilgiler **builtin.keyPhrase** olarak döndürüldü. İstemci uygulaması bu ek bilgileri kullanarak "Would you like to talk to a Human Resource representative about new dental benefits" (Yeni diş tedavisi kapsamları hakkında bilgi almak için bir İnsan Kaynakları temsilcisiyle konuşmak ister misiniz?") gibi bir ek soru sorabilir veya "More information about new dental benefits or medical plan." (Yeni diş tedavisi avantajları veya sağlık planı hakkında daha fazla bilgi.) gibi ek seçeneklerin yer aldığı bir menü sunabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, konuşmalardaki ana tümcecikleri konuşma etiketlemeye gerek kalmadan hemen sunan önceden oluşturulmuş keyPhrase varlığı eklenmiştir. 

> [!div class="nextstepaction"]
> [Uygulamaya yaklaşım analizi ekleme](luis-quickstart-intent-and-sentiment-analysis.md)
