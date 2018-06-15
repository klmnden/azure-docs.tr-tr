---
title: HALUK tahminleri - Azure geliştirmek için bir ifade listesini kullanarak Öğreticisi | Microsoft Docs
description: Bu öğreticide bir HALUK uygulamasına tümcecik listeye ekleyin ve puanı geliştirme bakın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 4ced7bcec87a9edde2e3ded8c8c61abe96003572
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356085"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Öğretici: tahminleri artırmak için tümcecik listeye ekleyin
Bu öğreticide hedefi puanları doğruluğunu artırmak ve varlıklar için bir birbirinin yerine ekleyerek (anlamlıları) ile aynı anlamı olan sözcükler tanımlamak [tümcecik liste özelliğini](./luis-concept-feature.md).

> [!div class="checklist"]
* Yeni bir uygulama içeri aktarın  
* Bilinen utterance sorgu uç noktası 
* Sorgu noktayla _bilinmeyen_ utterance
* Bilinmeyen utterance puan artırmak için tümcecik listeye ekleyin
* Varlık tümcecik listesi kullanırken doğrulayın

Bu makalede, ücretsiz bir gereksinim duyduğunuz [HALUK] [ LUIS] HALUK uygulamanızı yazmak için hesap.

## <a name="import-a-new-app"></a>Yeni bir uygulama içeri aktarın
1. Karşıdan [örnek HALUK uygulama] [ LuisSampleApp] Bu öğretici için tasarlanmıştır. Sonraki adımda kullanır. 

2. Bölümünde açıklandığı gibi [bir uygulama oluşturmak](Create-new-app.md#import-new-app), içine indirilen dosyayı içeri [HALUK] [ LUIS] yeni bir uygulama olarak Web sitesi. Uygulama adı "Benim tümcecik listesi." öğreticidir Hedefleri, varlıkları ve utterances sahiptir. 

3. [Tren](luis-how-to-train.md) uygulamanızı. Denenip ayarlanana kadar yapamazsınız [etkileşimli olarak test](Train-Test.md#interactive-testing) içinde [HALUK] [ LUIS] Web sitesi. 

4. Üzerinde [Yayımla](PublishApp.md) sayfasında, **dahil tüm tahmin hedefi puanları** onay kutusu. Onay kutusu seçili olduğunda, tüm hedefleri döndürülür. Onay kutusu temizlendiğinde, yalnızca üst hedefi döndürülür. 

5. [Yayımlama](PublishApp.md) uygulama. Uygulama yayımlama HTTPS uç noktasını kullanarak test etmenizi sağlar. 

## <a name="test-a-trained-utterance"></a>Eğitilmiş utterance test
Uygulama zaten bilir bir utterance sorgulamak için yayımlanan uç noktasını kullanın. HALUK zaten utterance bildiği için puan yüksektir ve varlık algıladı.

1. Üzerinde [dil anlama (HALUK)] [ LUIS] Web sitesi, **Yayımla** sayfasında yeni uygulama için uç nokta URL'sini seçin **kaynakları ve anahtarları**bölümü. 

    ![Uç nokta URL'sini yayımlama](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. Tarayıcıda, URL'nin sonunda sonra aşağıdaki sorguyu eklemek `q=`.

    `I want a computer replacement`

    Uç nokta aşağıdaki JSON ile yanıt verir:
    
    ```JSON
    {
      "query": "I want a computer replacement",
      "topScoringIntent": {
        "intent": "GetHardware",
        "score": 0.9735458
      },
      "intents": [
        {
          "intent": "GetHardware",
          "score": 0.9735458
        },
        {
          "intent": "None",
          "score": 0.07053033
        },
        {
          "intent": "Whois",
          "score": 0.03760778
        },
        {
          "intent": "CheckCalendar",
          "score": 0.02285902
        },
        {
          "intent": "CheckInventory",
          "score": 0.0110072717
        }
      ],
      "entities": [
        {
          "entity": "computer",
          "type": "Hardware",
          "startIndex": 9,
          "endIndex": 16,
          "score": 0.8465915
        }
      ]
    }
    ```

    Uygulama ile bu utterance eğitilmiş çünkü 0.973 hedefi puanı ve 0.846 varlık algılama puanı yüksektir. Hedefi sayfasında HALUK uygulama utterance bulunduğu **GetHardware**. Utterance's metin `computer`, olarak etiketli **donanım** varlık. 
    
    |Durum|Word| Hedefi puanı | Varlık puanı |
    |--|--|--|--|
    |Eğitilen| istediğiniz | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Eğitimsiz utterance test
Tarayıcıda, uygulama zaten bilmiyor bir utterance ile aynı yayımlanan uç nokta sorgu için kullanın:

`I require a computer replacement`

Bu utterance önceki utterance eşanlamlısı kullanır:

| Eğitilmiş word | Eğitimsiz eş anlamlı |
|--|--|
| istediğiniz | Gerektirir |

Uç nokta yanıt şöyledir:

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.840912163
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.840912163
    },
    {
      "intent": "None",
      "score": 0.0972757638
    },
    {
      "intent": "Whois",
      "score": 0.0448251367
    },
    {
      "intent": "CheckCalendar",
      "score": 0.0291390456
    },
    {
      "intent": "CheckInventory",
      "score": 0.0137837809
    }
  ],
  "entities": []
}
```

| Durum | Word | Hedefi puanı | Varlık puanı |
|--|--|--|--|
| Eğitilen| istediğiniz | 0.973 | 0.846 |
| Eğitimsiz| Gerektirir | 0.840 | - |

Eğitimsiz utterance hedefi puan etiketli utterance düşük olduğundan HALUK tümce dilbilgisi açısından aynı olduğunu bilir. Ancak HALUK utterances aynı anlama sahip bilmiyor. Ayrıca, tümcecik listesi olmadan **donanım** varlık bulunamadı.

HALUK öğretmek gerekir *istediğiniz* ve *gerektiren* bir sözcük birden fazla anlama sahip olabilir çünkü bu uygulama etki alanında aynı şeyi anlamına gelir. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Tümcecik listesiyle eğitimsiz utterance puanı geliştirmek 
1. Ekleme bir [tümcecik listesi](luis-how-to-add-features.md) adlı özellik **istediğiniz** değeriyle `want`ve ardından **Enter**.

    > [!TIP]
    > Her sözcük veya tümcecik sonra seçin **Enter** anahtarı. Sözcük veya tümcecik eklenen **tümceyi liste değerleri** imleci kalarak kutusunda **değeri** kutusu. Bu özellik ile hızlı bir şekilde birden fazla değer girebilirsiniz.

2. HALUK önerir sözcükler görüntülemek için seçin **önerilir**. 

    ![Değerler önerilir](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Tüm sözcükleri ekleyin. Varsa `require` olan isteğe bağlı olarak önerilen listede olmayan, gerekli bir değer ekleyin. 

4. Bu sözcükleri eş anlamlıları olduğundan tutmak *birbirinin yerine* ayarlama ve ardından **kaydetmek**.

    ![Tümcecik liste değerleri](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Üst gezinti çubuğunda seçin **eğitmek** uygulama Eğitilecek ancak onu yayımlama. Şimdi iki modelleri vardır. İki model değerleri karşılaştırabilirsiniz.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Yayımlanan modeli tümcecik listesi modeline Karşılaştır
Bu uygulamada, yayımlanan modeli ile anlamlıları eğitildi değil. Yalnızca şu anda düzenlenen modeli eş anlamlıları tümcecik listesini içerir. Modeli karşılaştırmak için kullanmak [etkileşimli sınama](Train-Test.md#interactive-testing). 

1. Açık **Test** bölmesinde, aşağıdaki utterance girin:

    `I require a computer replacement`

2. İnceleme panelini açmak için seçin **incele**. 

    ![Select inceleyin.](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Yeni tümcecik listesi modeline yayımlanan modeli karşılaştırmak için seçin **ile karşılaştırın yayımlanan**.

    ![İnceleme karşı geçerli yayımlanan](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Tümcecik listesi, utterance artan doğruluğunu ekledikten sonra ve **donanım** varlık bulundu. 

|Durum | Tümcecik listesi| Hedefi puanı | Varlık puanı |
|--|--|--|--|
| Yayımlanma | - | 0,84 | - |
| Şu anda düzenleme |✔| 0.92 | Tanımlanan donanım varlık |

> [!TIP]
> * Kullanarak [etkileşimli test](Train-Test.md#interactive-testing), yayımlanan, yayımladıktan sonra yapılan değişiklikleri eğitilen modele karşılaştırabilirsiniz. 
> * Kullanarak [Endpoint test](PublishApp.md#test-your-published-endpoint-in-a-browser), tam HALUK yanıt JSON görüntüleyebilirsiniz. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Uç nokta test ile varlık puan Al
Varlık puan görüntülemek için [modele yayımlama](PublishApp.md) ve uç nokta sorgulayabilirsiniz. 

`I require a computer replacement`

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.916503668
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.916503668
    },
    {
      "intent": "None",
      "score": 0.136505231
    },
    {
      "intent": "Whois",
      "score": 0.02778677
    },
    {
      "intent": "CheckInventory",
      "score": 0.0144592477
    },
    {
      "intent": "CheckCalendar",
      "score": 0.01401332
    }
  ],
  "entities": [
    {
      "entity": "computer",
      "type": "Hardware",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.5959917
    }
  ]
}
```

**Donanım** varlık 0.595 tümcecik listesiyle puanı gösterir. Tümcecik listesi var önce varlık algılanmadı. 

|Durum | Tümcecik listesi| Hedefi puanı | Varlık puanı |
|--|--|--|--|
| Yayımlanma | - | 0,84 | - |
| Şu anda düzenleme |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerektiğinde HALUK uygulamayı silin. Bunu yapmak için uygulama adının sağ tarafındaki üç nokta menüsüne (...) uygulama listesinde seçin **silmek**. Açılan iletişim kutusunda **Delete uygulama?** seçin **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uç nokta sorgusuyla utterance tahmin al](luis-get-started-cs-get-intent.md)

[LUIS]: luis-reference-regions.md

  [LUIS]:luis-reference-regions.md
  [LuisFeatures]: luis-concept-feature.md
  [LuisSampleApp]:https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
