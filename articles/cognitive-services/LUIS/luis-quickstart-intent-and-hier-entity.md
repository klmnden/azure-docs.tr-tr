---
title: Hiyerarşik varlık
titleSuffix: Azure Cognitive Services
description: Bağlama göre ilgili veri parçaları bulabilirsiniz. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: diberry
ms.openlocfilehash: c17a74c81d9c9d2ac3f585ab17f0b7d2acc628f6
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873933"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Öğretici: Bir utterance bağlamsal ilgili verileri ayıklayın

Bu öğreticide bağlama göre ilgili veri parçalarını bulacaksınız. Örneğin, bir kaynak ve hedef konumların bir şehir için başka bir aktarımı için. Her iki veri parçasını gerekli olabilir ve birbiriyle ilgilidir.  

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Amaç ekleme 
> * Çıkış ve varış alt öğelerine sahip konum hiyerarşik varlığını ekleme
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="hierarchical-data"></a>Hiyerarşik veriler

Bu uygulama, bir çalışan hedef şehri kaynak city taşınacak olduğu belirler. Konuşma içindeki konumları belirlemek için hiyerarşik varlığı kullanır. 

Hiyerarşik varlık olduğundan bu veri türü için uygun olan iki parça verilerin alt konumları:

* Basit varlıklardır.
* Konuşma bağlamında birbiriyle ilişkilidir.
* Her varlık belirtmek için belirli bir sözcük seçimi kullanın. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Her iki alt sık aynı utterance var. 
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Çalışanların şehirler arasında taşımak için bir hedefi oluşturma

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

1. Açılan iletişim kutusuna `MoveEmployeeToCity` girip **Done** (Bitti) öğesini seçin. 

    ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/create-new-intent-move-employee-to-city.png)

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Seattle bırakarak Smith için Dallas vizyonumuzdan Taşı|
    |Jill Jones Seattle'dan Cairo için Aktarım|
    |Atlanta için gelen bir yerde John Jackson Tampa, işlerini hayatınızdan çıkarın |
    |Gamze Doughtery Tulsa için Dallas taşıyın.|
    |MV Tampa için Jill Cairo bırakarak Jones vizyonumuzdan|
    |Redmond'dan Oakland için Shift Alice Anderson|
    |San Francisco gelen Carl Chamerlin Redmond için|
    |San Diego Bellevue doğru Steve Standish aktarımı |
    |Kansas Şehir ve shift Etikan Thompson Şikago'ya yükseltme|

    [![MoveEmployee amacı, yeni Konuşma ile LUIS ekran görüntüsü](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png)](./media/luis-quickstart-intent-and-hier-entity/hr-enter-utterances.png#lightbox)

## <a name="create-a-location-entity"></a>Konum varlığı oluşturma
LUIS uygulamasının konuşmalardaki kaynak ve hedef konumları etiketleyerek konumun ne olduğunu anlaması gerekir. Konuşmayı belirteç (ham) görünümünde görmek isterseniz konuşmaların üzerinde çubukta yer alan **Entities View** (Varlık Görünümü) denetimini seçin. Anahtarı açık duruma getirdikten sonra denetim **Tokens View** (Belirteç Görünümü) olarak etiketlenir.

Aşağıdaki konuşmaya bir göz atın:

```json
move John W. Smith leaving Seattle headed to Dallas
```

Konuşmada `Seattle` ve `Dallas` olmak üzere iki konum belirtilmiştir. Hem alt öğeleri hiyerarşik bir varlık olarak gruplandırılır `Location`, çünkü istemci uygulamasındaki bir isteği tamamlamaya utterance ayıklanacak hem veri parçasını gerekir ve birbiriyle ilişkili. 
 
Hiyerarşik varlığın alt öğelerinden yalnızca bir tanesi (çıkış veya varış) mevcut olduğunda da ayıklama işlemi gerçekleştirilir. Bir veya birden fazla öğenin ayıklanması için tüm alt öğelerin bulunması gerekmez. 

1. `move John W. Smith leaving Seattle headed to Dallas` konuşmasının içinde `Seattle` sözcüğünü seçin. En üstte bir metin kutusu bulunan açılan menü görüntülenir. `Location` metin kutusuna varlık adını girip açılan menüden **Create new entity** (Yeni varlık oluştur) öğesini seçin. 

    [![Hedefi sayfasında yeni varlık oluşturma işleminin ekran görüntüsü](media/luis-quickstart-intent-and-hier-entity/tutorial-hierarichical-entity-labeling-1.png "hedefi sayfasında yeni varlık oluşturma işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/tutorial-hierarichical-entity-labeling-1.png#lightbox)

1. Açılır pencerede **Hierarchical** (Hiyerarşik) varlık türünü ve `Origin` ile `Destination` alt varlıkları seçin. **Done** (Bitti) öğesini seçin.

    ![Varlık için yeni konum varlık oluşturma açılır iletişim kutusunun ekran görüntüsü](media/luis-quickstart-intent-and-hier-entity/hr-create-new-entity-2.png "varlık için yeni konum varlık oluşturma açılır iletişim kutusunun ekran görüntüsü")

1. LUIS, terimin çıkış mı, varış mı yoksa ikisi dışında bir değer mi olduğunu bilmediğinden `Seattle` etiketi `Location` olarak işaretlenir. Seçin `Seattle`, ardından **konumu**, sonra sağ menü izleyin ve seçin `Origin`.

    [![Konumları varlık alt değiştirmek için ekran açılır iletişim kutusu etiketleme varlığın](media/luis-quickstart-intent-and-hier-entity/tutorial-hierarichical-entity-labeling-2.png "açılır iletişim kutusu etiketleme varlık konumları varlık alt değiştirmek için ekran görüntüsü")](media/luis-quickstart-intent-and-hier-entity/tutorial-hierarichical-entity-labeling-2.png#lightbox)

1. Diğer tüm sesleri başka konumlarda etiketleyin. Tüm Konumlar işaretlendiğinde, bir desen gibi görünmesini konuşma başlayın. 

    [![Konuşma etiketlenmiş ekran görüntüsü, konumları varlık](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png "konuşma etiketlenmiş ekran görüntüsü, konumları varlık")](media/luis-quickstart-intent-and-hier-entity/all-intents-marked-with-origin-and-destination-location.png#lightbox)

    Kırmızı alt çizgi LUIS varlık hakkında başarılara olduğunu gösterir. Eğitim Bu çözümler. 

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktasından amacı ve varlık öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Adres çubuğundaki URL'nin sonuna gidip `Please move Carl Chamerlin from Tampa to Portland` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `MoveEmployee` amacını hiyerarşik varlık ayıklanmış şekilde döndürmelidir.

    ```json
    {
      "query": "Please move Carl Chamerlin from Tampa to Portland",
      "topScoringIntent": {
        "intent": "MoveEmployeeToCity",
        "score": 0.979823351
      },
      "intents": [
        {
          "intent": "MoveEmployeeToCity",
          "score": 0.979823351
        },
        {
          "intent": "None",
          "score": 0.0156363435
        }
      ],
      "entities": [
        {
          "entity": "portland",
          "type": "Location::Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "tampa",
          "type": "Location::Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    Doğru amacını tahmin edilen ve karşılık gelen kaynak ve hedef değer varlıkları dizi **varlıkları** özelliği.
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Hiyerarşik varlık](luis-concept-entity-types.md) kavramsal bilgiler
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)
* [Hiyerarşik varlıkları ve rolleri](luis-concept-roles.md#roles-versus-hierarchical-entities)
* [Desenler ile Öngörüler geliştirin](luis-concept-patterns.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, oluşturulan yeni bir hedefi ve kaynak ve hedef konumların öğrenilen bağlamsal veriler için örnek konuşma eklenir. Uygulama eğitilip yayımlandıktan sonra bir istemci uygulama bu bilgileri ilgili bilgilerle bir taşınma bileti oluşturmak için kullanabilir.

> [!div class="nextstepaction"] 
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md) 
