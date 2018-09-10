---
title: 'Öğretici: Anahtar ifadeler döndüren bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide, konuşmalarda temel konu çözümlemesi gerçekleştirme amacıyla LUIS uygulamanıza keyPhrase varlığı eklemeyi ve bunu döndürmeyi öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: ef7a1c81f453a8d4ff9526a4844518782e152c4f
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159495"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Öğretici: 8. keyPhrase varlığını ekleme 
Bu öğreticide konuşmalardaki temel konuları ayıklamayı gösteren bir uygulama kullanacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * keyPhrase varlıklarını anlama 
> * LUIS uygulamasını İnsan Kaynakları (İK) alanında kullanma 
> * Konuşmadan içerik ayıklamak için keyPhrase varlığını ekleme
> * Uygulamayı eğitme ve yayımlama
> * Anahtar ifadeler içeren LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce
[Basit varlık](luis-quickstart-primary-and-secondary-data.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](luis-how-to-start-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `keyphrase` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="keyphrase-entity-extraction"></a>keyPhrase varlığı ayıklama
Temel konu, önceden oluşturulmuş **keyPhrase** varlığı tarafından sağlanır. Bu varlık, konuşmadaki temel konuyu döndürür.

Aşağıdaki konuşmalarda anahtar ifade örnekleri gösterilmektedir:

|Konuşma|keyPhrase varlığı değerleri|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Önümüzdeki yıl daha düşük kesintili yeni bir sağlık planı sunuluyor mu?)|"lower deductible" (daha düşük kesinti)<br>"new medical plan" (yeni sağlık planı)<br>"year" (yıl)|
|Is vision therapy covered in the high deductible medical plan? (Göz tedavisi daha yüksek kesintili sağlık planına dahil mi?)|"high deductible medical plan" (yüksek kesintili sağlık planı)<br>"vision therapy" (göz tedavisi)|

İstemci uygulamanız konuşmanın bir sonraki adımını belirlemek için bu değerleri diğer ayıklanan varlıklarla birlikte kullanabilir.

## <a name="add-keyphrase-entity"></a>keyPhrase varlığını ekleme 
Konuşmaların konusunu ayıklamak için önceden oluşturulmuş keyPhrase varlığını ekleyin.

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

2. Sol menüden **Entities** (Varlıklar) öğesini seçin.

    [ ![Build (Derleme) bölümünün sol tarafındaki gezinti bölmesinde vurgulanmış Entities (Varlıklar) öğesinin ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. **Manage prebuilt entities** (Önceden oluşturulan varlıkları yönet) öğesini seçin.

    [ ![Entities (Varlıklar) listesi açılan iletişim kutusu ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. Açılan iletişim kutusunda **keyPhrase** öğesini ve ardından **Done** (Bitti) öğesini seçin. 

    [ ![Entities (Varlıklar) listesi açılan iletişim kutusu ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. Sol menüden **Intents** (Amaçlar) öğesini ve ardından **Utilities.Confirm** amacını seçin. keyPhrase varlığı birkaç konuşmada etiketlenmiştir. 

    [ ![keyPhrases etiketli konuşmalarla Utilities.Confirm amacının ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Uygulamayı uç noktasına yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]


## <a name="query-the-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `does form hrf-123456 cover the new dental benefits and medical plan` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

```
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

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
keyPhrase varlığı algılama özelliğine sahip bu uygulama, doğal dil sorgusu amacı tanımladı ve temel konuyu içeren ayıklanmış verileri döndürdü. 

Sohbet botunuz artık konuşmadaki bir sonraki adımı belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve konuşmadaki keyPhrase verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamaya yaklaşım analizi ekleme](luis-quickstart-intent-and-sentiment-analysis.md)