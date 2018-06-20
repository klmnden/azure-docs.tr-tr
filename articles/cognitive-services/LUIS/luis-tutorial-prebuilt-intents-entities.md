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
ms.openlocfilehash: 20950ced66497fb0dc96365975b37f244f677ce3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266388"
---
# <a name="use-prebuilt-intents-and-entities-to-handle-common-intents-and-data"></a>Ortak amaçları ve verileri işlemek için önceden oluşturulmuş hedefleri ve varlıkları kullanmak
Önceden oluşturulmuş hedefleri ve varlıkları hedefi tahmin ve verileri ayıklama hızlı bir şekilde sağlamak için İnsan Kaynakları hızlı başlangıç uygulamasını ekleyin. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Önceden oluşturulmuş hedefleri Ekle 
* Önceden oluşturulmuş varlıklar datetimeV2 ve numarasını ekleyin
* Eğitmek ve yayımlama
* HALUK sorgulamak ve tahmin yanıtını alma

## <a name="before-you-begin"></a>Başlamadan önce
İnsan Kaynakları uygulamadan yoksa [özel etki alanı](luis-quickstart-intents-only.md) hızlı başlangıç, [alma](create-new-app.md#import-new-app) JSON içinde yeni bir uygulama içinde [HALUK] [ LUIS] Web sitesi , gelen [HALUK-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json) Github depo.

Özgün İnsan Kaynakları uygulaması tutmak istiyorsanız, sürüm üzerinde kopyalama [ayarları](luis-how-to-manage-versions.md#clone-a-version) sayfasında ve adlandırın `prebuilts`. Kopyalama, özgün sürümü etkilemeden çeşitli HALUK özelliklerle yürütmek için harika bir yoludur. 

## <a name="add-prebuilt-intents"></a>Önceden oluşturulmuş hedefleri Ekle
HALUK ortak kullanıcı amaçları ile yardımcı olmak için birkaç önceden oluşturulmuş hedefleri sağlar.  

1. Uygulamanızı olduğundan emin olun **yapı** HALUK bölümü. Bu bölümde seçerek değiştirebilirsiniz **yapı** menü çubuğu üst, sağ. 

    [ ![Yapı hightlighted üst, sağ gezinti çubuğunda uygulamayla HALUK, ekran görüntüsü](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

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
1. Üst sağ tarafında HALUK Web sitesinin, seçin **tren** düğmesi. 

    ![Tren düğmesi](./media/luis-quickstart-intents-only/train-button.png)

    Yeşil durum çubuğu başarı onaylama Web sitesinin üst gördüğünüzde eğitim tamamlanır.

    ![Eğitilmiş durum çubuğu](./media/luis-quickstart-intents-only/trained.png)

2. Üst, sağ tarafındaki HALUK Web sitesi seçin **Yayımla** düğmesi yayımla sayfasını açın. Üretim yuvasına varsayılan olarak seçilidir. Seçin **Yayımla** üretim yuvası seçim düğmesine tıklayın. Yeşil durum çubuğu başarı onaylama Web sitesinin üst gördüğünüzde yayımlama işlemi tamamlanmış olur.

    Yayımlamadan önce veya uç nokta URL'sini test önce Azure portalında HALUK anahtarı oluşturmak zorunda değildir. Her HALUK uygulama geliştirme için ücretsiz starter anahtar vardır. Size verir sınırsız yazma ve [birkaç uç noktası isabet](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-with-an-utterance"></a>Bir utterance sorgu uç noktası
Üzerinde **Yayımla** sayfasında, **endpoint** sayfanın sonundaki bağlantı. Bu eylem, adres çubuğundaki uç nokta URL'si ile başka bir tarayıcı penceresi açar. URL adresi sonuna gidin ve girin `I want to cancel on March 3`. Son sorgu dizesi parametresi `q`, utterance **sorgu**. 

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

[Varlıklar hakkında daha fazla bilgi](luis-concept-entity-types.md). 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
