---
title: Önceden oluşturulmuş hedefleri ve varlıkları dil anlama - Azure ortak verileri ayıklamak için ekleme | Microsoft Docs
description: Önceden oluşturulmuş hedefleri ve varlıkları farklı varlık veri türlerini ayıklamak için nasıl kullanılacağını öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 06/11/2018
ms.author: v-geberr
ms.openlocfilehash: 37d67bef7712012a95543041744706b240b16e2d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085506"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Öğretici: 2. Önceden derlenmiş amaçlar ve varlıklar ekleme
Önceden oluşturulmuş hedefleri ve varlıkları hedefi tahmin ve verileri ayıklama hızlı bir şekilde sağlamak için İnsan Kaynakları hızlı başlangıç uygulamasını ekleyin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Önceden oluşturulmuş hedefleri Ekle 
* Önceden oluşturulmuş varlıklar datetimeV2 ve numarasını ekleyin
* Eğitmek ve yayımlama
* HALUK sorgulamak ve tahmin yanıtını alma

## <a name="before-you-begin"></a>Başlamadan önce
Sahip değilse [İnsan Kaynakları](luis-quickstart-intents-only.md) önceki öğreticiyi uygulamadan [alma](create-new-app.md#import-new-app) JSON içinde yeni bir uygulama içinde [HALUK](luis-reference-regions.md#luis-website) Web sitesi, gelen [HALUK örnekleri ](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json) Github depo.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `prebuilts` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="add-prebuilt-intents"></a>Önceden oluşturulmuş hedefleri Ekle
HALUK ortak kullanıcı amaçları ile yardımcı olmak için birkaç önceden oluşturulmuş hedefleri sağlar.  

1. Uygulamanızı olduğundan emin olun **yapı** HALUK bölümü. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

2. Seçin **önceden oluşturulmuş bir etki alanı hedefi eklemek**. 

    [ ![Ekran görüntüsü, hedefleri Sayfa Ekle önceden oluşturulmuş bir etki alanı hedefi düğmesi vurgulanan](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

3. Arama `Utilities`. 

    [ ![Arama kutusuna yardımcı programları ile önceden oluşturulmuş hedefleri iletişim ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Aşağıdaki hedefleri seçip **Bitti**: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.Stop
    * Utilities.StartOver

## <a name="add-prebuilt-entities"></a>Önceden oluşturulmuş varlıkları ekleyin
HALUK birkaç önceden oluşturulmuş varlıklar için genel veri ayıklama sağlar. 

1. Seçin **varlıklar** sol gezinti menüsünde.

    [ ![Sol gezinti bölmesinde vurgulanan varlıklarla ekran görüntüsü, amaçlar listesi](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Seçin **önceden oluşturulmuş varlıkları Yönet** düğmesi.

    [ ![Önceden oluşturulmuş varlıklar vurgulanmış Yönet ekran görüntüsü, varlıklar listesi](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Seçin **numarası** ve **datetimeV2** önceden oluşturulmuş varlıkların listesinden seçip **Bitti**.

    ![Önceden oluşturulmuş varlıklar iletişim numarası seçimi ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Eğitmek ve uygulama yayımlama
1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin. 

    ![Tren düğmesi](./media/luis-quickstart-intents-only/train-button.png)

    Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitilmiş durum çubuğu](./media/luis-quickstart-intents-only/trained.png)

2. Üst, sağ tarafındaki HALUK Web sitesi seçin **Yayımla** düğmesi yayımla sayfasını açın. Üretim yuvasına varsayılan olarak seçilidir. Seçin **Yayımla** üretim yuvası seçim düğmesine tıklayın. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

    Yayımlamadan önce veya uç nokta URL'sini test önce Azure portalında HALUK anahtarı oluşturmak zorunda değildir. Her HALUK uygulama geliştirme için ücretsiz starter anahtar vardır. Size verir sınırsız yazma ve [birkaç uç noktası isabet](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-with-an-utterance"></a>Bir utterance sorgu uç noktası
**Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. Adres çubuğundaki URL'nin sonuna gidip `I want to cancel on March 3` yazın. Son sorgu dizesi parametresi `q`, utterance **sorgu**. 

Sonuç Utilities.Cancel hedefi tahmin ve Mart 3 tarihini ve 3 sayısını ayıklanır. 

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

İstemci uygulaması, hızla ve kolayca önceden oluşturulmuş hedefleri ve varlıkları ekleyerek, konuşma yönetim ekleyin ve ortak veri türleri ayıklayın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamaya bir normal ifade varlık ekleme](luis-quickstart-intents-regex-entity.md)

