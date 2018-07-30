---
title: Language Understanding'de ortak verileri ayıklamak için önceden oluşturulmuş amaçları ve varlıkları ekleme - Azure | Microsoft Docs
description: Önceden oluşturulmuş amaçları ve varlıkları, farklı varlık verisi türlerini ayıklamak için kullanmayı öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: diberry
ms.openlocfilehash: 3fc2040e66f6fc649448d3241b01678b7bb7f214
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239044"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Öğretici: 2. Önceden derlenmiş amaçlar ve varlıklar ekleme
Hızlıca amaç tahmini ve veri ayıklaması gerçekleştirmek için İnsan Kaynakları öğretici uygulamasına önceden oluşturulmuş amaçlar ve varlıklar ekleyin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Önceden oluşturulmuş amaçları ekleme 
* Önceden oluşturulmuş datetimeV2 ve number varlıklarını ekleme
* Eğitme ve yayımlama
* LUIS uygulamasını sorgulama ve tahmin yanıtını alma

## <a name="before-you-begin"></a>Başlamadan önce
Bir önceki öğreticide oluşturulan [İnsan Kaynakları](luis-quickstart-intents-only.md) uygulamasına sahip değilseniz [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json) Github deposundaki JSON verilerini [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulamaya [aktarın](luis-how-to-start-new-app.md#import-new-app).

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `prebuilts` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="add-prebuilt-intents"></a>Önceden oluşturulmuş amaçları ekleme
LUIS, ortak kullanıcı amaçları konusunda yardımcı olmak için önceden oluşturulmuş birçok amaca sahiptir.  

1. Uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

2. **Add prebuilt domain intent** (Önceden oluşturulmuş etki alanı amacı ekle) öğesini seçin. 

    [ ![Add prebuilt domain intent (Önceden oluşturulmuş etki alanı amacı ekle) düğmesi vurgulanmış Intents (Amaçlar) sayfasının ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

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

    [ ![Sol gezinti bölmesinde vurgulanmış Entities (Varlıklar) düğmesine sahip Intents (Amaçlar) listesinin ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. **Manage prebuilt entities** (Önceden oluşturulan varlıkları yönet) düğmesini seçin.

    [ ![Manage prebuilt entities (Önceden oluşturulan varlıkları yönet) düğmesi vurgulanmış Entities (Varlıklar) listesinin ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Önceden oluşturulmuş varlık listesinden **number** ve **datetimeV2** girişlerini ve ardından **Done** (Bitti) öğesini seçin.

    ![Önceden oluşturulmuş varlıklar iletişim kutusunda sayının seçildiğini gösteren ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama
1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin. 

    ![Train (Eğitim) düğmesi](./media/luis-quickstart-intents-only/train-button.png)

    Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim durumu çubuğu](./media/luis-quickstart-intents-only/trained.png)

2. Publish (Yayımla) sayfasını açmak için LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

3. Üretim yuvası varsayılan olarak seçilir. Üretim yuvası seçeneğinin yanındaki **Publish** (Yayımla) düğmesini seçin. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

    Yayımlama veya uç nokta URL'sini test etme öncesinde Azure portalda bir LUIS uç noktası anahtarı oluşturmanıza gerek yoktur. Her LUIS uygulaması, yazma için ücretsiz bir başlangıç anahtarına sahiptir. Bu anahtar size sınırsız yazma ve [birkaç uç noktası isabeti](luis-boundaries.md#key-limits) sunar. 

## <a name="query-endpoint-with-an-utterance"></a>Uç noktayı bir konuşmayla sorgulama
**Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. Adres çubuğundaki URL'nin sonuna gidip `I want to cancel on March 3` yazın. Son sorgu dizesi parametresi konuşma **sorgusu** olan `q` öğesidir. 

Sonuç Utilities.Cancel amacını tahmin etmiş ve 3 Mart tarihi ile 3 rakamını ayıklamıştır. 

    ```
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

Konuşmada 3 Mart tarihinin geçmişte mi yoksa gelecekte mi olduğu belirtilmediğinden 3 Mart için iki değer vardır. Gerekirse varsayımda bulunma veya netleştirmek üzere soru sorma kararı LUIS çağrı uygulamasına aittir. 

Önceden oluşturulmuş amaçları ve varlıkları kolayca ve hızla ekleyerek istemci uygulaması konuşma yönetimi ekleyebilir ve ortak veri türlerini ayıklayabilir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunun için sol üstteki menüden **My apps** (Uygulamalarım) öğesini seçin. Uygulama listesinde uygulama adının yanındaki üç noktayı (***...***) ve sonra da **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamaya normal ifade varlığı ekleme](luis-quickstart-intents-regex-entity.md)

