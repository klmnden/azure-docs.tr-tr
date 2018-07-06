---
title: LUIS tahminleri - Azure geliştirmek için bir ifade listesini kullanarak Öğreticisi | Microsoft Docs
description: Bu öğreticide, bir LUIS uygulaması için bir ifade listesi ekleyin ve score geliştirme konusuna bakın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 2fd53225a455a34d03772487a10f62a94ac86735
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868982"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Öğretici: Öngörüler geliştirmek için ifade listesi ekleme
Bu öğreticide, hedefi puanları doğruluğunu artırmak ve varlıkları bir birbirinin yerine ekleyerek (eş anlamlılar) aynı anlama sahip sözcükleri tanımlama [tümcecik liste özelliğini](./luis-concept-feature.md).

> [!div class="checklist"]
* Yeni bir uygulamayı içeri aktarma  
* Bilinen utterance sorgu uç noktası 
* Uç nokta ile sorgu _bilinmeyen_ utterance
* Bilinmeyen utterance puanını artırmak için ifade listesi ekleme
* Varlık ifade listesi kullanırken bulunamazsa doğrulayın

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="import-a-new-app"></a>Yeni bir uygulamayı içeri aktarma
1. İndirme [örnek LUIS uygulaması] [ LuisSampleApp] Bu öğretici için tasarlanmıştır. Sonraki adımda kullanır. 

2. Bölümünde anlatıldığı gibi [uygulama oluşturma](Create-new-app.md#import-new-app), içine indirdiğiniz dosyayı içeri [LUIS] [ LUIS] yeni bir uygulama olarak Web sitesi. Uygulama adı "My tümcecik listesi." öğreticidir Bunun amacı, varlıkları ve konuşma sahiptir. 

3. [Train](luis-how-to-train.md) uygulamanızı. Bunu eğitildi kadar yapamazsınız [etkileşimli olarak test](interactive-test.md#interactive-testing) içinde [LUIS] [ LUIS] Web sitesi. 

4. Üzerinde [Yayımla](luis-how-to-publish-app.md) sayfasında **INCLUDE tüm hedefi puanları tahmin** onay kutusu. Onay kutusu işaretli olduğunda tüm hedefleri döndürülür. Onay kutusunun işareti kaldırıldığında, yalnızca üst hedefi döndürülür. 

5. [Yayımlama](luis-how-to-publish-app.md) uygulama. Uygulama yayımlama HTTPS uç noktasını kullanarak test etmenizi sağlar. 

## <a name="test-a-trained-utterance"></a>Eğitilen utterance test
Uygulama zaten bildiği bir utterance sorgulamak için yayımlanan uç noktayı kullanın. LUIS utterance bildiğinden, puan yüksektir ve varlık algılandı.

1. Üzerinde [Language Understanding (LUIS)] [ LUIS] Web sitesi, **Yayımla** sayfasında yeni bir uygulama için uç nokta URL'sini **kaynakları ve anahtarları**bölümü. 

    ![Uç nokta URL'sini yayımlama](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. Tarayıcıda, URL'nin sonunda sonra aşağıdaki sorgu Ekle `q=`.

    `I want a computer replacement`

    Uç nokta, aşağıdaki JSON ile yanıt verir:
    
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

    Uygulama ile bu utterance ayarlandığından 0.973 hedefi puanı ve 0.846 varlık algılama puanı yüksektir. LUIS uygulaması hedefi sayfasında utterance bulunduğu **GetHardware**. Utterance'nın metin `computer`, olarak etiketlenmiş **donanım** varlık. 
    
    |Durum|Word| Intent puanı | Varlık puanı |
    |--|--|--|--|
    |Eğitilen| istediğiniz | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Bir deneyimsiz utterance test
Tarayıcıda, uygulama zaten bilmez bir utterance ile sorgu aynı yayımlanan uç noktaya kullanın:

`I require a computer replacement`

Bu utterance eşanlamlısı önceki utterance birini kullanır:

| Eğitilen bir sözcük | Deneyimsiz eş anlamlı |
|--|--|
| istediğiniz | gerektirir |

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

| Durum | Word | Intent puanı | Varlık puanı |
|--|--|--|--|
| Eğitilen| istediğiniz | 0.973 | 0.846 |
| Deneyimsiz| gerektirir | 0.840 | - |

LUIS cümlenin bakımından aynı olduğunu bildiğinden deneyimsiz utterance hedefi puanı, etiketlenmiş utterance düşüktür. Ancak LUIS konuşma aynı anlamlara sahip olduğunu bilmez. Ayrıca, ifade listesi olmadan **donanım** varlık bulunamadı.

LUIS öğretin gerekir *istediğiniz* ve *gerektiren* bir sözcüğün birden fazla anlamı olabileceği için bu uygulama etki alanında aynı şeyi anlamına gelir. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Tümcecik listesiyle deneyimsiz utterance puanı geliştirin 
1. Ekleme bir [tümcecik listesi](luis-how-to-add-features.md) adlı özellik **istediğiniz** değeriyle `want`ve ardından **Enter**.

    > [!TIP]
    > Her sözcüğün veya tümceciğin sonra seçin **Enter** anahtarı. Bir sözcük veya tümcecik eklenir **tümcecik liste değerleri** imleç kalarak kutusunda **değer** kutusu. Bu özellik ile hızlı bir şekilde birçok değer girebilirsiniz.

2. LUIS önerir sözcük görüntülemek için seçin **önerilir**. 

    ![Değerler önerilir](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Tüm sözcükler ekleyin. Varsa `require` olan isteğe bağlı olarak önerilen listede olmayan, gerekli bir değer olarak ekleyin. 

4. Bu sözcükler eş anlamlılar olduğundan, tutmak *birbirinin yerine* ayarlama ve ardından **Kaydet**.

    ![Tümcecik liste değerleri](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Üst gezinti çubuğunda **eğitme** uygulama geliştirmek için ancak bu yayımlama. Artık iki modeli vardır. İki modelleri değerlerde karşılaştırabilirsiniz.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Yayımlanan model ifade listesi modele karşılaştırın
Bu uygulamada, yayımlanan modeli ile eşanlamlı eğitim almamış. Yalnızca şu anda düzenlenen modeli eş anlamlıları ifade listesi içerir. Modeli karşılaştırmak için kullanın [etkileşimli test](interactive-test.md#interactive-testing). 

1. Açık **Test** bölmesinde aşağıdaki utterance girin:

    `I require a computer replacement`

2. İnceleme panelini açmak için seçin **inceleyin**. 

    ![Select inceleyin](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Yeni ifade listesi modeline yayımlanan model Karşılaştırılacak seçin **Karşılaştır yayımlanan**.

    ![İnceleme geçerli yayımlanan](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Utterance artan doğruluğunu tümcecik listesi ekledikten sonra ve **donanım** varlık bulundu. 

|Durum | İfade listesi| Intent puanı | Varlık puanı |
|--|--|--|--|
| Yayımlanma | - | 0,84 | - |
| Şu anda düzenleme |✔| 0.92 | Tanımlanmış donanım varlık |

> [!TIP]
> * Kullanarak [etkileşimli test](interactive-test.md#interactive-testing), yayımlanan yayımladıktan sonra yapılan değişiklikleri eğitilen modele karşılaştırabilirsiniz. 
> * Kullanarak [uç noktasını sınama](luis-how-to-publish-app.md#test-your-published-endpoint-in-a-browser), LUIS tepki JSON görüntüleyebilirsiniz. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Uç nokta sınamaya varlık puan Al
Varlık puanı görüntülemek için [modeli yayımlama](luis-how-to-publish-app.md) ve uç noktasını sorgulayın. 

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

**Donanım** varlığı ifade listesi ile 0.595 puanı gösterir. İfade listesi var önce varlık algılanmadı. 

|Durum | İfade listesi| Intent puanı | Varlık puanı |
|--|--|--|--|
| Yayımlanma | - | 0,84 | - |
| Şu anda düzenleme |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için üç noktayı seçin (***...*** ) sağında bulunan uygulama listesinde uygulama adı, seçin **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uç nokta sorgusuyla utterance öngörü Al](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
