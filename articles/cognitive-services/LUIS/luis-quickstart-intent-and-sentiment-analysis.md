---
title: 'Öğretici: Yaklaşım analizi döndüren bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide LUIS uygulamanıza konuşmalardaki pozitif, negatif ve nötr duyguları çözümleme amacıyla duygu analizi eklemeyi öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d000637312619fc493e2f7bad8e8edf0d8d0d94b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265343"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Yaklaşım ve amaç tahmini döndüren bir uygulama oluşturma
Bu öğreticide konuşmalardaki pozitif, negatif ve nötr yaklaşımları ayıklamayı gösteren bir uygulama oluşturacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * Hiyerarşik varlıkları ve bağlamsal olarak öğrenilen alt öğeleri anlama 
> * Bookflight amacını kullanarak seyahat alanı için yeni bir LUIS uygulaması oluşturma
> * _None_ amacını ve örnek konuşmaları ekleme
> * Çıkış ve varış alt öğelerine sahip konum hiyerarşik varlığını ekleme
> * Uygulamayı eğitme ve yayımlama
> * Hiyerarşik alt öğeleri içeren LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama 

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="sentiment-analysis"></a>Yaklaşım analizi
Yaklaşım analizi, bir kullanıcının konuşmasının pozitif, negatif veya nötr olma durumunu belirleme yeteneğidir. 

Aşağıdaki konuşmalarda yaklaşım örnekleri gösterilmektedir:

|Yaklaşım ve puan|Konuşma|
|:--|--|
|pozitif - 0,89 |The soup and salad combo was great. (Çorba ve salata ikilisi harikaydı.)|
|negatif - 0,07 |I didn't like the appetizer during the dinner service. (Akşam yemeği servisi sırasındaki aperitiften hoşlanmadım.)|

Yaklaşım analizi, tüm konuşmalar için geçerli olan bir uygulama ayarıdır. Konuşmadaki yaklaşımı belirten sözcükleri bulup etiketlemeniz gerekmez. LUIS bunu sizin yerinize yapar.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS][LUIS] web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz [bölgede][LUIS-regions] oturum açtığınızdan emin olun.

2. [LUIS][LUIS] web sitesinde **Create new app** (Yeni uygulama oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png "Uygulama listesi sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png#lightbox)

3. **Create new app** (Yeni uygulama oluştur) iletişim kutusunda uygulamaya `Restaurant Reservations With Sentiment` adını verin ve **Done** (Bitti) öğesine tıklayın. 

    ![Create new app (Yeni uygulama oluştur) iletişim kutusunun görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/create-app-ddl.png)

    Uygulama oluşturma işlemi tamamlandıktan sonra LUIS, None (Yok) amacını içeren amaç listesini gösterir.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png "Intents (Amaçlar) listesi sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png#lightbox)

## <a name="add-a-prebuilt-domain"></a>Önceden oluşturulmuş bir alan ekleme
Amaçları, varlıkları ve etiketlenmiş konuşmaları hızlıca eklemek için önceden oluşturulmuş bir alan ekleyin.

1. Sol menüden **Prebuilt Domains** (Önceden Oluşturulmuş Alanlar) öğesini seçin.

    [ ![Prebuilt Domains (Önceden Oluşturulmuş Alanlar) düğmesinin ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-expanded.png#lightbox)

2. Önceden oluşturulmuş **RestaurantReservation** alanı için **Add domain** (Alan ekle) öğesini seçin. Alan eklenene kadar bekleyin.

    [ ![Prebuilt Domains (Önceden Oluşturulmuş Alanlar) listesinin ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-expanded.png#lightbox)

3. Sol gezinti bölmesinden **Intents** (Amaçlar) öğesini seçin. Bu önceden oluşturulmuş alanın bir amacı vardır.

    [ ![Önceden oluşturulmuş alan listesini ve sol gezinti bölmesinde vurgulanmış Intents (Amaçlar) öğesini içeren ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png#lightbox)

4.  **RestaurantReservation.Reserve** amacını seçin. 

    [ ![RestaurantReservation.Reserve vurgulanmış şekilde Intents (Amaçlar) listesinin ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png#lightbox)

5. Alana özgü varlıklarla etiketlenmiş çok sayıda konuşmayı görmek için **Entities View** (Varlık Görünümü) geçişi yapın.

    [ ![Entities View (Varlık Görünümü), Token View (Belirteç Görünümü) konumuna alınmış şekilde vurgulanmış, RestaurantReservation.Reserve amacının göründüğü ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-expanded.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Vurgulanmış Train (Eğitim) düğmesinin ekran görüntüsü](./media/luis-quickstart-intent-and-sentiment-analysis/train-button-expanded.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı bildirim çubuğunun ekran görüntüsü ](./media/luis-quickstart-intent-and-sentiment-analysis/trained-expanded.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Uygulamayı yaklaşım analizi içerecek şekilde yapılandırma
Yaklaşım analizi, **Publish** (Yayımla) sayfasında etkinleştirilir. 

1. Sağ üst gezinti bölmesinde **Publish** (Yayımla) öğesini seçin.

    ![Publish (Yayımla) düğmesi genişletilmiş Intent (Amaç) sayfasının ekran görüntüsü ](./media/luis-quickstart-intent-and-sentiment-analysis/publish-expanded.png)

2. **Enable Sentiment Analysis** (Yaklaşım Analizini Etkinleştir) öğesini seçin.

    ![Enable Sentiment Analysis (Yaklaşım Analizini Etkinleştir) seçeneği vurgulanmış Publish (Yayımla) sayfasının ekran görüntüsü ](./media/luis-quickstart-intent-and-sentiment-analysis/enable-sentiment-expanded.png)

3. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-inline.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-expanded.png#lightbox)

4. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama

1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    ![Uç nokta URL'si vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü](media/luis-quickstart-intent-and-sentiment-analysis/endpoint-url-inline.png)

2. Adres çubuğundaki URL'nin sonuna gidip `Reserve table for  10 on upper level away from kitchen` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `RestaurantReservation.Reserve` amacını yaklaşım analizi ayıklanmış şekilde döndürmelidir.

```
{
  "query": "Reserve table for 10 on upper level away from kitchen",
  "topScoringIntent": {
    "intent": "RestaurantReservation.Reserve",
    "score": 0.9926384
  },
  "intents": [
    {
      "intent": "RestaurantReservation.Reserve",
      "score": 0.9926384
    },
    {
      "intent": "None",
      "score": 0.00961109251
    }
  ],
  "entities": [],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yaklaşım analizi etkin olan bu uygulama, doğal dil sorgusu amacı tanımladı ve genel yaklaşımı puan olarak içeren ayıklanmış verileri döndürdü. 

Sohbet botunuz artık konuşmadaki bir sonraki adımı belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve konuşmadaki yaklaşım verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [C# ile LUIS uç nokta API'sini çağırma](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
