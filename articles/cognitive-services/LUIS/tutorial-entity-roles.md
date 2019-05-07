---
title: Rolleri - Language Understanding ile bağlamsal veriler
titleSuffix: Azure Cognitive Services
description: İlgili veri bağlama göre bulun. Örneğin, bir bina ya da ofisten başka bir bina ya da ofise fiziksel olarak taşınmada çıkış ve varış konumları.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a0ab928ef3b8551e3e20ff3c4b16533c80ee4b7d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149290"
---
# <a name="tutorial-extract-contextually-related-data-from-an-utterance"></a>Öğretici: Bir utterance bağlamsal ilgili verileri ayıklayın

Bu öğreticide bağlama göre ilgili veri parçalarını bulacaksınız. Örneğin, bir kaynak ve hedef konumların bir şehir için başka bir aktarımı için. Her iki veri parçasını gerekli olabilir ve birbiriyle ilgilidir.  

Bir rol ile herhangi bir önceden oluşturulmuş veya özel varlık türü kullanılan ve örnek konuşma ve desenler kullanılır. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Yeni uygulama oluşturma
> * Amaç ekleme 
> * Rolleri kullanarak kaynak ve hedef bilgilerini alma
> * Eğitim
> * Yayımlama
> * Hedefleri ve varlık rolleri uç noktasından Al

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="related-data"></a>İlgili veriler

Bu uygulama, bir çalışan hedef şehri kaynak city taşınacak olduğu belirler. Bir GeographyV2 kullanan Şehir adları ve belirlemek için önceden oluşturulmuş varlık rolleri içinde utterance (kaynak ve hedef) konumu türlerini belirlemek için kullanır.

Bir rolü olmalıdır yüklendiğinde varlık verilerini ayıklamak için:

* Utterance bağlamında birbiriyle ilgili.
* Her rol göstermek için belirli bir sözcük seçeneği kullanır. Bu sözcüklere örnekler şunlardır: from/to, leaving/headed to, away from/toward (çıkış/varış, ayrılıyor/gidiyor, kaynaktan/hedefe doğru)
* Her iki rolde sık sık bu bağlamsal kullanımdan öğrenmek LUIS izin vererek aynı utterance arasındadır.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-move-employees-between-cities"></a>Çalışanların şehirler arasında taşımak için bir hedefi oluşturma

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

1. Açılan iletişim kutusuna `MoveEmployeeToCity` girip **Done** (Bitti) öğesini seçin. 

    ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/tutorial-entity-roles/create-new-intent-move-employee-to-city.png)

1. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Seattle bırakarak Smith için Orlando vizyonumuzdan Taşı|
    |Jill Jones Seattle'dan Cairo için Aktarım|
    |Atlanta için gelen bir yerde John Jackson Tampa, işlerini hayatınızdan çıkarın |
    |Gamze Doughtery için Tulsa Chicago'dan taşıyın.|
    |MV Tampa için Jill Cairo bırakarak Jones vizyonumuzdan|
    |Redmond'dan Oakland için Shift Alice Anderson|
    |San Francisco gelen Carl Chamerlin Redmond için|
    |San Diego Bellevue doğru Steve Standish aktarımı |
    |Kansas Şehir ve shift Etikan Thompson Şikago'ya yükseltme|

    [![MoveEmployee amacı, yeni Konuşma ile LUIS ekran görüntüsü](./media/tutorial-entity-roles/hr-enter-utterances.png)](./media/tutorial-entity-roles/hr-enter-utterances.png#lightbox)

## <a name="add-prebuilt-entity-geographyv2"></a>Önceden oluşturulmuş varlık geographyV2 Ekle

Önceden oluşturulmuş varlığı geographyV2, şehir adları da dahil olmak üzere Konum bilgisini ayıklar. Konuşma bağlamında, birbiriyle ilgili iki Şehir adları olduğundan, içeriği ayıklamak için rolleri kullanın.

1. Seçin **varlıkları** sol taraftaki gezinti bölmesinden.

1. Seçin **önceden oluşturulmuş bir varlık ekleme**, ardından `geo` önceden oluşturulmuş varlıklarla filtrelemek için arama çubuğuna. 

    ![GeographyV2 ekleme uygulamasına önceden oluşturulmuş varlık](media/tutorial-entity-roles/add-geographyV2-prebuilt-entity.png)
1. Onay kutusunu seçip **Bitti**.
1. İçinde **varlıkları** listesinden **geographyV2** yeni varlığı açın. 
1. İki rol ekleme `Origin`, ve `Destination`. 

    ![Roller için önceden oluşturulmuş varlık ekleme](media/tutorial-entity-roles/add-roles-to-prebuilt-entity.png)
1. Seçin **hedefleri** seçip sol taraftaki gezinti bölmesinden **MoveEmployeeToCity** hedefi. Şehir adları ile önceden oluşturulmuş varlık etiketli fark **geogrpahyV2**.
1. İçinde ilk utterance listenin kaynağı konumu seçin. Bir açılan menü görünür. Seçin **geographyV2** listesinde menüsünün yatay seçmek için daha sonra izleyin **kaynak**.
1. Konuşma konumlarda tüm rolleri işaretlemek için önceki adımdan gelen yöntemi kullanın. 


## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktasından amacı ve varlık öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]


1. Adres çubuğundaki URL'nin sonuna gidip `Please move Carl Chamerlin from Tampa to Portland` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. İyi bir testtir ve döndürmelidir bu utterance etiketli konuşma hiçbirini aynı olmadığından `MoveEmployee` ayıklanan varlıkla hedefi.

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
          "entity": "geographyV2",
          "role": "Destination",
          "startIndex": 41,
          "endIndex": 48,
          "score": 0.6044041
        },
        {
          "entity": "geographyV2",
          "role": "Origin",
          "startIndex": 32,
          "endIndex": 36,
          "score": 0.739491045
        }
      ]
    }
    ```
    
    Doğru amacını tahmin edilen ve karşılık gelen kaynak ve hedef rolleri sahip varlıklar dizisi **varlıkları** özelliği.
    
## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlıkları kavramları](luis-concept-entity-types.md)
* [Rol kavramları](luis-concept-roles.md)
* [Önceden oluşturulmuş varlıklar listesi](luis-reference-prebuilt-entities.md)
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)
* [Roller](luis-concept-roles.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, oluşturulan yeni bir hedefi ve kaynak ve hedef konumların öğrenilen bağlamsal veriler için örnek konuşma eklenir. Uygulama eğitilip yayımlandıktan sonra bir istemci uygulama bu bilgileri ilgili bilgilerle bir taşınma bileti oluşturmak için kullanabilir.

> [!div class="nextstepaction"] 
> [Bileşik varlık eklemeyi öğrenin](luis-tutorial-composite-entity.md) 
