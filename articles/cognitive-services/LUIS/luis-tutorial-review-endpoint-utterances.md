---
title: Language Understanding'de (LUIS) uç nokta konuşmalarını gözden geçirme öğreticisi - Azure | Microsoft Docs
description: Bu öğreticide, LUIS'de İnsan Kaynakları etki alanındaki uç nokta konuşmalarını gözden geçirmeyi öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: db44bfad5ece59ed3373699c10d6134201bf1879
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160090"
---
# <a name="tutorial-review-endpoint-utterances"></a>Öğretici: Uç nokta konuşmalarını gözden geçirme
Bu öğreticide, LUIS HTTP uç noktası üzerinden alınan konuşmaları doğrulayarak veya düzelterek uygulama tahminlerini geliştirin. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Uç nokta konuşmalarını gözden geçirmeyi anlama 
> * İnsan Kaynakları (İK) etki alanı için LUIS uygulaması kullanma 
> * Uç nokta konuşmasını gözden geçirme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce
[Yaklaşım](luis-quickstart-intent-and-sentiment-analysis.md) öğreticisinden İnsan Kaynakları uygulamanız yoksa, uygulamayı [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-sentiment-HumanResources.json) Github deposundan indirin. Bu öğreticiyi yeni, içeri aktarılan bir uygulama olarak kullanıyorsanız, ayrıca bunu eğitmeniz, yayımlamanız ve bir [betikle](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) veya tarayıcıda uç noktadan konuşmaları eklemeniz gerekir. Eklenecek konuşmalar:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `review` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

Öğretici serisi aracılığıyla uygulamanın tüm sürümlerine sahipseniz, **Uç nokta konuşmalarını gözden geçir** listesinin sürüme göre değişmediğini görmek sizi şaşırtabilir. Etkin olarak hangi sürümün konuşmasını düzenlediğinizden veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak, gözden geçirilecek tek bir konuşma havuzu vardır. 

## <a name="purpose-of-reviewing-endpoint-utterances"></a>Uç nokta konuşmalarını gözden geçirmenin amacı
Bu gözden geçirme işlemi, LUIS için uygulama etki alanınızı öğrenmenin bir diğer yoludur. LUIS gözden geçirme listesinde konuşmaları seçer. Bu liste:

* Uygulamaya özeldir.
* Uygulamanın tahmin doğruluğunu geliştirmek için hazırlanmıştır. 
* Düzenli aralıklarla gözden geçirilmelidir. 

Uç nokta konuşmalarını gözden geçirerek, konuşmanın tahmin edilen amacını doğrular veya düzeltirsiniz. Ayrıca, tahmin edilmemiş olan özel varlıkları da etiketlersiniz. 

## <a name="review-endpoint-utterances"></a>Uç nokta konuşmasını gözden geçirme

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

2. Sol gezintiden **Uç nokta konuşmalarını gözden geçir**'i seçin. Bu liste **ApplyForJob** amacı için filtrelenmiştir. 

    [ ![Sol gezintideki uç nokta konuşmalarını gözden geçir düğmesinin ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png)](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png#lightbox)

3. Etiketlenmiş varlıkları görmek için **Varlıklar görünümüne** geçin. 
    
    [ ![Varlıklar görünümü iki durumlu düğmesini vurgulandığı Uç nokta konuşmalarını gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png#lightbox)

    |Konuşma|Doğru amaç|Eksik varlıklar|
    |:--|:--|:--|
    |Doğal Dil İşleme ile ilgili bir iş arıyorum|GetJobInfo|İş - "Doğal Dil İşleme"|

    Bu konuşma doğru amaçta değildir ve puanı %50'den düşüktür. **ApplyForJob** amacının 21 konuşması varken, **GetJobInformation** amacının yedi konuşması vardır. Uç nokta konuşmasını düzgün hizalamanın yanı sıra, **GetJobInformation** amacına daha fazla konuşma da eklenmelidir. Bu, kendi başınıza tamamlamanız için bir alıştırma olarak bırakılmıştır. **Hiçbiri** amacı dışındaki her amacın kabaca aynı sayıda örnek konuşması olmalıdır. **Hiçbiri** amacındaki konuşma sayısının, uygulamadaki konuşmaların %10'u kadar olması gerekir. 

    **Belirteçler Görünümünde**, konuşmada mavi metinlerin üzerine gelerek tahmin edilen varlık adını görebilirsiniz. 

4. `I'm looking for a job with Natual Language Processing` amacı olarak, **Hizalı amaç** sütununda doğru amaç olan **GetJobInformation**'ı seçin. 

    [ ![Konuşmayı amaca hizalayan Uç nokta konuşmaları gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

5. Aynı konuşmada `Natural Language Processing` varlığı keyPhrase'dir. Bunun yerine bu bir **Job** varlığı olmalıdır. `Natural Language Processing` öğesini ve ardından listede **Job** varlığını seçin.

    [ ![Konuşmada varlığın etiketlendiği Uç nokta konuşmalarını gözden geçir ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

6. Aynı satırda, **Hizalanmış amaca ekle** sütununda yuvarlak için alınmış onay işaretini seçin. 

    [ ![Amaçta konuşma hizalamasını son haline getirme ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Bu eylem konuşmayı **Uç nokta konuşmalarını gözden geçir** ekranından **GetJobInformation** amacına taşır. Uç nokta konuşması şimdi söz konusu amaç için örnek bir konuşma olmuştur. 

7. Bu amaçtaki diğer konuşmaları gözden geçirin, konuşmaları etiketleyin ve **Hizalanmış amaç** yanlışsa düzeltin.

8. Tüm konuşmalar doğru olduğunda, her satırdaki onay kutusunu seçin ve ardından **Seçileni ekle**'yi seçerek konuşmaları doğru hizalayın. 

    [ ![Hizalanmış amacın kalan konuşmalarına son haline getirme ekran görüntüsü](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

9. Artık listede bu konuşmalar yer almamalıdır. Başka konuşmalar da gösteriliyorsa, listede çalışmaya devam edin ve liste boşalana kadar amaçları düzeltin ve eksik varlıkları etiketleyin. Filtre listesinde bir sonraki amacı seçin, sonra konuşmaları düzeltmeye ve varlıkları etiketlemeye devam edin. Her amacın son adımının konuşma satırında **Hizalanmış amaca ekle**'yi seçmek veya her amacın yanındaki kutuyu işaretleyip yukarıdaki tabloda **Seçileni ekle**'yi seçmek olduğunu unutmayın. 

    Bu çok küçük bir uygulamadır. Gözden geçirme işlemi yalnızca birkaç dakika sürer.

## <a name="add-new-job-name-to-phrase-list"></a>Tümcecik listesine yeni iş adı ekleme
Tüm yeni eklenen iş adlarıyla tümcecik listesini güncel tutun. 

1. Sol gezintiden **Tümcecik listeleri**'ni seçin.

2. **Jobs** tümcecik listesini seçin.

3. `Natural Language Processing` öğesini bir değer olarak ekleyin ve ardından **Kaydet**'i seçin. 

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

LUIS eğitilene kadar bu değişiklikten haberdar olmaz. 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

Bu uygulamayı içeri aktardıysanız, **Yaklaşım analizi**'ni seçmelisiniz.

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama

Düzeltilmiş konuşmaya yakın bir konuşma deneyin. 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Are there any natural language processing jobs in my department right now?` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

  ```JSON
  {
    "query": "are there any natural language processing jobs in my department right now?",
    "topScoringIntent": {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    "intents": [
      {
        "intent": "GetJobInformation",
        "score": 0.9247605
      },
      {
        "intent": "ApplyForJob",
        "score": 0.129989788
      },
      {
        "intent": "FindForm",
        "score": 0.006438211
      },
      {
        "intent": "EmployeeFeedback",
        "score": 0.00408575451
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00194211153
      },
      {
        "intent": "None",
        "score": 0.00166400627
      },
      {
        "intent": "Utilities.Help",
        "score": 0.00118593348
      },
      {
        "intent": "MoveEmployee",
        "score": 0.0007885918
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.0006373631
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0005980781
      },
      {
        "intent": "Utilities.Confirm",
        "score": 3.719905E-05
      }
    ],
    "entities": [
      {
        "entity": "right now",
        "type": "builtin.datetimeV2.datetime",
        "startIndex": 64,
        "endIndex": 72,
        "resolution": {
          "values": [
            {
              "timex": "PRESENT_REF",
              "type": "datetime",
              "value": "2018-07-05 15:23:18"
            }
          ]
        }
      },
      {
        "entity": "natural language processing",
        "type": "Job",
        "startIndex": 14,
        "endIndex": 40,
        "score": 0.9869922
      },
      {
        "entity": "natural language processing jobs",
        "type": "builtin.keyPhrase",
        "startIndex": 14,
        "endIndex": 45
      },
      {
        "entity": "department",
        "type": "builtin.keyPhrase",
        "startIndex": 53,
        "endIndex": 62
      }
    ],
    "sentimentAnalysis": {
      "label": "positive",
      "score": 0.8251864
    }
  }
  }
  ```

  Doğru amaç yüksek puanla tahmin edilmiş ve **Job** varlığı `natural language processing` olarak algılanmıştır. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>Gözden geçirme işleminin yerini daha fazla konuşma ekleme alabilir mi? 
Neden daha fazla örnek konuşma eklenmediğini merak ediyor olabilirsiniz. Uç nokta konuşmalarını gözden geçirmenin amacı nedir? Gerçek dünyadaki bir LUIS uygulamasında, uç nokta konuşmaları henüz alışmadığınız sözcük seçimi ve yerleşimi olan kullanıcılara aittir. Aynı sözcük seçimini ve yerleşimini daha önce kullandıysanız, özgün tahminin yüzdesi yüksek olabilir. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>En üst amaç neden konuşma listesinde yer alıyor? 
Bazı uç nokta konuşmalarının gözden geçirme listesinde yüksek bir yüzdesi olacaktır. Bu konuşmaları yine de gözden geçirmeniz ve doğrulamanız gerekir. Bunların listede olmasının nedeni, bir sonraki yüksek amacın puanının en üst amaç puanına yakın olmasıdır. 

## <a name="what-has-this-tutorial-accomplished"></a>Bu öğretici hangi işlemleri gerçekleştirdi?
Bu uygulamanın tahmin doğruluğu, uç noktadan konuşmalar gözden geçirilerek artırıldı. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Desenleri kullanmayı öğrenin](luis-tutorial-pattern.md)
