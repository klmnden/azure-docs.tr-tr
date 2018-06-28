---
title: 'Öğretici: Anahtar ifadeler döndüren bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide, konuşmalarda temel konu çözümlemesi gerçekleştirme amacıyla LUIS uygulamanıza keyPhrase varlığı eklemeyi ve bunu döndürmeyi öğreneceksiniz.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12c306b5199da5862302c28d1690b81c6e1edb0e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264624"
---
# <a name="tutorial-create-app-that-returns-keyphrases-entity-data-found-in-utterances"></a>Öğretici: Konuşmalarda bulunan keyPhrases varlığı verilerini döndüren bir uygulama oluşturma
Bu öğreticide konuşmalardaki temel konuları ayıklamayı gösteren bir uygulama oluşturacaksınız.

<!-- green checkmark -->
> [!div class="checklist"]
> * keyPhrase varlıklarını anlama 
> * İnsan kaynakları alanında yeni bir LUIS uygulaması oluşturma
> * _None_ amacını ve örnek konuşmaları ekleme
> * Konuşmadan içerik ayıklamak için keyPhrase varlığını ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabını kullanabilirsiniz.

## <a name="keyphrase-entity-extraction"></a>keyPhrase varlığı ayıklama
Temel konu, önceden oluşturulmuş **keyPhrase** varlığı tarafından sağlanır. Bu varlık, konuşmadaki temel konuyu döndürür

Aşağıdaki konuşmalarda anahtar ifade örnekleri gösterilmektedir:

|Konuşma|keyPhrase varlığı değerleri|
|--|--|
|Is there a new medical plan with a lower deductible offered next year? (Önümüzdeki yıl daha düşük kesintili yeni bir sağlık planı sunuluyor mu?)|"lower deductible" (daha düşük kesinti)<br>"new medical plan" (yeni sağlık planı)<br>"year" (yıl)|
|Is vision therapy covered in the high deductible medical plan? (Göz tedavisi daha yüksek kesintili sağlık planına dahil mi?)|"high deductible medical plan" (yüksek kesintili sağlık planı)<br>"vision therapy" (göz tedavisi)|

Sohbet botunuz konuşmadaki bir sonraki adımı belirlerken ayıklanan diğer tüm varlıklara ek olarak bu değerleri de kullanabilir.

## <a name="download-sample-app"></a>Örnek uygulamayı indirin
[İnsan Kaynakları](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) uygulamasını indirin ve *.json uzantılı bir dosyaya kaydedin. Bu örnek uygulama çalışan yan hakları, kuruluş şemaları ve fiziksel varlıklarla ilgili konuşmaları tanır.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS][LUIS] web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz [bölgede][LUIS-regions] oturum açtığınızdan emin olun.

2. [LUIS][LUIS] web sitesinde **Import new app** (Yeni uygulama içeri aktar) öğesini seçerek bir önceki bölümde indirdiğiniz İnsan Kaynakları uygulamasını içeri aktarın. 

    [![](media/luis-quickstart-intent-and-key-phrase/app-list.png "Uygulama listesi sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-key-phrase/app-list.png#lightbox)

3. **Import new app** (Yeni uygulama içeri aktar) iletişim kutusunda uygulamaya `Human Resources with Key Phrase entity` adını verin. 

    ![Create new app (Yeni uygulama oluştur) iletişim kutusunun görüntüsü](./media/luis-quickstart-intent-and-key-phrase/import-new-app-inline.png)

    Uygulama oluşturma işlemi tamamlandıktan sonra LUIS amaç listesini gösterir.

    [![](media/luis-quickstart-intent-and-key-phrase/intents-list.png "Intents (Amaçlar) listesi sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-key-phrase/intents-list.png#lightbox)

## <a name="add-keyphrase-entity"></a>keyPhrase varlığını ekleme 
Konuşmaların konusunu ayıklamak için önceden oluşturulmuş keyPhrase varlığını ekleyin.

1. Sol menüden **Entities** (Varlıklar) öğesini seçin.

    [ ![Build (Derleme) bölümünün sol tarafındaki gezinti bölmesinde vurgulanmış Entities (Varlıklar) öğesinin ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/select-entities.png)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png#lightbox)

2. **Manage prebuilt entities** (Önceden oluşturulan varlıkları yönet) öğesini seçin.

    [ ![Entities (Varlıklar) listesi açılan iletişim kutusu ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png#lightbox)

3. Açılan iletişim kutusunda **keyPhrase** öğesini ve ardından **Done** (Bitti) öğesini seçin. 

    [ ![Entities (Varlıklar) listesi açılan iletişim kutusu ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS eğitilene kadar bu değişiklikten haberdar olmaz. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Vurgulanmış Train (Eğitim) düğmesinin ekran görüntüsü](./media/luis-quickstart-intent-and-key-phrase/train-button-expanded.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı bildirim çubuğunun ekran görüntüsü ](./media/luis-quickstart-intent-and-key-phrase/trained-inline.png)

## <a name="publish-app-to-endpoint"></a>Uygulamayı uç noktasına yayımlama

1. Sağ üst gezinti bölmesinde **Publish** (Yayımla) öğesini seçin.

    ![Publish (Yayımla) düğmesi genişletilmiş Entity (Varlık) sayfasının ekran görüntüsü ](./media/luis-quickstart-intent-and-key-phrase/publish-expanded.png)

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    [![](media/luis-quickstart-intent-and-key-phrase/publish-to-production-inline.png "Publish to production slot (Üretim yuvasında yayımla) düğmesi vurgulanmış Yayımlama sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-key-phrase/publish-to-production-expanded.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama

1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    ![Uç nokta URL'si vurgulanmış Publish (Yayımla) sayfası ekran görüntüsü](media/luis-quickstart-intent-and-key-phrase/endpoint-url-inline.png )

2. Adres çubuğundaki URL'nin sonuna gidip `Is there a new medical plan with a lower deductible offered next year?` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

```
{
  "query": "Is there a new medical plan with a lower deductible offered next year?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.216838628
  },
  "entities": [
    {
      "entity": "lower deductible",
      "type": "builtin.keyPhrase",
      "startIndex": 35,
      "endIndex": 50
    },
    {
      "entity": "new medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 26
    },
    {
      "entity": "year",
      "type": "builtin.keyPhrase",
      "startIndex": 65,
      "endIndex": 68
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
keyPhrase varlığı algılama özelliğine sahip bu uygulama, doğal dil sorgusu amacı tanımladı ve temel konuyu içeren ayıklanmış verileri döndürdü. 

Sohbet botunuz artık konuşmadaki bir sonraki adımı belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve konuşmadaki keyPhrase verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yaklaşım ve amaç tahmini döndüren bir uygulama oluşturma](luis-quickstart-intent-and-sentiment-analysis.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
