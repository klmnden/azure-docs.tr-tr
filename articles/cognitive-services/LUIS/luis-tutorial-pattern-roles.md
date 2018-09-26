---
title: 'Öğretici 4: Desen rolleri bağlamı için ilgili verileri'
titleSuffix: Azure Cognitive Services
description: Bir desen, bir iyi biçimlendirilmiş şablon utterance verileri ayıklamak için kullanın. Şablon utterance konumu kaynak ve hedef konumu gibi ilgili verileri ayıklamak için basit bir varlık ve rollerini kullanır.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 2c3705d28d6496c3d20999231de98572bc26e3be
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47160256"
---
# <a name="tutorial-4-extract-contextually-related-patterns"></a>Öğretici 4: bağlamsal ilgili desenleri ayıklayın

Bu öğreticide, bir desen bir iyi biçimlendirilmiş şablon utterance verileri ayıklamak için kullanın. Şablon utterance konumu kaynak ve hedef konumu gibi ilgili verileri ayıklamak için basit bir varlık ve rollerini kullanır.  Desenler kullanırken, daha az örnek konuşma amacı için gereklidir.

Rolleri amacı bir utterance bağlamsal olarak ilişkili varlıkları ayıklamaktır. Utterance içinde `Move new employee Robert Williams from Sacramento and San Francisco`, kaynak Şehir ve hedef Şehir değerlerini birbiriyle ilgili ve ortak dil her bir konum belirtmek için kullanın. 


Yeni bir çalışan, Billy Patterson ve Konuğu, adını liste varlığı bir parçası değil **çalışan** henüz. Yeni çalışan adı, ilk olarak, şirket kimlik bilgilerini oluşturmak için bir dış sistem adı göndermek için ayıklanır. Çalışan kimlik bilgilerini şirket kimlik bilgilerini oluşturulduktan sonra liste varlığı için eklenen **çalışan**.

Aile ve yeni çalışan adlı kurgusal şirketin bulunduğu şehir için geçerli city taşınacak gerekir. Yeni bir çalışan herhangi bir şehre gelebileceğinden konumları bulunmaları gerekir. Yalnızca liste şehirlerin ayıklanan çünkü kümesi listesini bir liste varlığı gibi çalışmaz.

Kaynak ve hedef şehirleri ile ilişkili rol adları tüm varlıklar arasında benzersiz olması gerekir. Rolleri benzersiz olduğundan emin olmak için kolay bir yolu bunları adlandırma stratejisi aracılığıyla içeren varlık için bağlamaktır. **NewEmployeeRelocation** varlıktır iki rol ile basit bir varlık: **NewEmployeeReloOrigin** ve **NewEmployeeReloDestination**. Relo kısaltması yeniden konumlandırma ' dir.

Çünkü örnek utterance `Move new employee Robert Williams from Sacramento and San Francisco` yalnızca makine öğrenilen varlıklar, sahip varlıklar algılanan şekilde yeterli örnek konuşma amacı sağlamak önemlidir.  

**Desenler varlıkları değil algılanırsa, daha az örnek konuşma sağlamanıza olanak tanırken desenle eşleşmez.**

Bir şehir gibi bir adı olduğundan varlığın algılama ile sorun yaşıyorsanız, benzer değer ifade listesi eklemeyi göz önünde bulundurun. Bu, şehir adı algılanması sözcük veya tümcecik türü hakkında ek bir sinyal LUIS sağlayarak yardımcı olur. İfade listeleri yalnızca eşleşme gerçekleştirilecek desenin için gerekli olan varlık algılama yardımcı olarak deseni yardımcı olur. 

**Bu öğreticide, şunların nasıl yapılır:**

> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Yeni varlık oluşturma
> * Yeni hedefi oluşturma
> * Eğitim
> * Yayımlama
> * Uç noktasından amaç ve varlıkları alma
> * Desen ile rolleri oluşturma
> * Şehir ifade listesi oluşturma
> * Uç noktasından amaç ve varlıkları alma

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Var olan bir uygulama kullanma
Adlı son öğreticisinde oluşturulan uygulama devam **İnsanKaynakları**. 

Önceki öğreticide İnsanKaynakları uygulamadan yoksa aşağıdaki adımları kullanın:

1.  İndirip kaydedin [uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-patterns-HumanResources-v2.json).

2. JSON, yeni bir uygulamaya aktarma.

3. Gelen **Yönet** üzerinde bölümünde **sürümleri** sekmesinde sürüm kopyalayın ve adlandırın `roles`. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rota bir parçası olarak kullanıldığından, adın bir URL geçerli olmayan karakterler içeremez.

## <a name="create-new-entities"></a>Yeni varlık oluşturma

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Seçin **varlıkları** sol gezinti bölmesinden. 

3. **Create new entity** (Yeni varlık oluştur) öğesini seçin.

4. Açılır pencerede girin `NewEmployee` olarak bir **basit** varlık.

5. **Create new entity** (Yeni varlık oluştur) öğesini seçin.

6. Açılır pencerede girin `NewEmployeeRelocation` olarak bir **basit** varlık.

7. Seçin **NewEmployeeRelocation** varlıkların listesinden. 

8. İlk rolü olarak girin `NewEmployeeReloOrigin` seçin girin.

9. İkinci rol olarak girin `NewEmployeeReloDestination` seçin girin.

## <a name="create-new-intent"></a>Yeni hedefi oluşturma
Bu adımları varlıklarda etiketleme geri bu bölümdeki adımları tamamladıktan sonra daha sonra eklenen başlamadan önce önceden oluşturulmuş bir anahtar cümlesi varlık kaldırılırsa daha kolay olabilir. 

1. Seçin **hedefleri** sol gezinti bölmesinden.

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

3. Girin `NewEmployeeRelocationProcess` açılır iletişim kutusunda hedefi adı.

4. Aşağıdaki örnek, yeni varlıklar etiketleme konuşma, girin. Varlık ve rol kalın değerlerdir. Unutmayın **belirteçleri görünümü** metin etiketi daha kolay fark ederseniz. 

    Rol varlığının amaca etiketleme sırasında belirtmeyin. Daha sonra deseni oluştururken bunu. 

    |İfade|Yeniçalışan|NewEmployeeRelocation|
    |--|--|--|
    |Taşıma **Bob Jones** gelen **Seattle** için **Los Colinas**|Bob Jones|Seattle, Los Colinas|
    |Taşıma **Dave c Cooper** gelen **Redmond** için **New York City**|Dave c Cooper|Redmond, New York City|
    |Taşıma **Jim Paul Smith** gelen **Toronto** için **Batı Vancouver**|Jim Paul Smith|Toronto, Batı Vancouver|
    |Taşıma **J. Benson** gelen **Boston** için **Thames bağlı Staines**|J. Benson|Boston, Thames bağlı Staines|
    |Taşıma **Travis "Trav" Hinton** gelen **Castelo Branco** için **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Taşıma **Trevor Nottington III** gelen **Aranda de Duero** için **Boise**|Trevor Nottington III|Aranda de Duero, Boise|
    |Taşıma **Dr. Greg Williams** gelen **Orlando** için **Ellicott Şehir**|Dr. Greg Williams|Orlando, Ellicott Şehir|
    |Taşıma **Robert "Bobby" Gregson** gelen **Kansas Şehir** için **San Juan Capistrano**|Robert "Bobby" Gregson|Kansas City, San Juan Capistrano|
    |Taşıma **Patti Owens** gelen **Bellevue** için **Rockford**|Patti Owens|Bellevue, Rockford|
    |Taşıma **Jale Bartlet** gelen **Tuscan** için **Santa Fe**|Jale Bartlet|Tuscan, Santa Fe|

    Çalışan adı ön eki, sözcük sayısını, sözdizimi ve soneki çeşitli sahiptir. Bu yeni bir çalışan ad çeşitleri anlamak LUIS için önemlidir. Şehir adları çeşitli sözcük sayısını ve söz dizimi de var. Bu bir kullanıcının utterance bu varlıkları nasıl görünebilir LUIS öğretmek önemlidir. 
    
    Her iki varlık aynı sözcük sayısını ve diğer Çeşitleme yok olsaydı, bu varlık yalnızca bu sözcük sayısını ve diğer Çeşitleme yok olduğunu LUIS öğretin. LUIS herhangi gösterilmeyen çünkü çeşitlemeleri kurulmuştur doğru şekilde tahmin etmek mümkün olmaz. 

    Anahtar cümlesi varlık kaldırılırsa, uygulamaya geri şimdi ekleyin.

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktasından amaç ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Adres çubuğundaki URL'nin sonuna gidip `Move Wayne Berry from Miami to Mount Vernon` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

Hedefi tahmin puanı yalnızca yaklaşık % 50 ' dir. İstemci uygulamanızı daha yüksek bir sayı gerektiriyorsa, bu düzeltilmesi gerekir. Varlıkları değil ya da tahmin edilen.

Konumlardan birine ayıklanan ancak diğer konumda değildi. 

Desenlerini tahmin puanı yardımcı olur, önce utterance desenle eşleşir ancak varlıkları doğru şekilde tahmin gerekir. 

## <a name="pattern-with-roles"></a>Rolleri deseni

1. Seçin **derleme** üst gezintideki.

2. Seçin **desenleri** sol gezinti bölmesinde.

3. Seçin **NewEmployeeRelocationProcess** gelen **bir amaç seçin** aşağı açılan listesi. 

4. Şu deseni girin: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Eğitim, yayımlama ve uç nokta sorgu desen ile eşleşmedi, varlıkları, bulunmayan, görmek hayal kırıklığına olabilir bu nedenle tahminini geliştirme kaydetmedi. Yeterli sayıda örnek konuşma etiketli varlıklarla bir sonucu budur. Daha fazla örnek eklemek yerine, bu sorunu gidermek için bir ifade listesi ekleyin.

## <a name="cities-phrase-list"></a>Şehir ifade listesi
Herhangi bir sözcük ve noktalama karışımını olabilirler, şehirler, kişilerin adları gibi hassas. Şehir Bölge ve dünya bilinen sorunlarıdır ve LUIS öğrenme başlamak için Şehir ifade listesi gerekir. 

1. Seçin **tümcecik listesi** gelen **uygulama performansını** soldaki menüden bölümü. 

2. Listenin adı `Cities` ve aşağıdakileri ekleyin `values` listesi için:

    |İfade listesinin değerleri|
    |--|
    |Seattle|
    |San Diego|
    |New York City|
    |Los Angeles|
    |İstanbul|
    |Philadephia|
    |Miami|
    |Dallas|

    Her şehir dünyanın veya bölgede bile her şehir eklemeyin. LUIS generalize mümkün olması gerekir hangi şehirde listesidir. Tutmaya dikkat **birbirinin yerine bu değerleri** seçili. Bu ayar listesindeki sözcükler üzerinde eş anlamlı sözcükler kabul anlamına gelir. 

3. Eğitim ve uygulama yayımlama.

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktasından amaç ve varlıkları alma

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Move wayne berry from miami to mount vernon` yazın. Son sorgu dizesi parametresi ifade **s**orgusu olan `q` öğesidir. 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

Intent puanı artık çok daha yüksektir ve rol adları varlık yanıt bir parçasıdır.

## <a name="hierarchical-entities-versus-roles"></a>Karşı rol hiyerarşik varlıklar

İçinde [hiyerarşik öğretici](luis-quickstart-intent-and-hier-entity.md), **MoveEmployee** hedefi varolan bir çalışan bir yapı ve office diğerine taşımak ne zaman algılandı. Örnek konuşma kaynak ve hedef konumların vardı, ancak rol kullanmamıştır. Bunun yerine, alt öğeleri hiyerarşik varlık kaynak ve hedef yoktu. 

Bu öğreticide, İnsan Kaynakları uygulamasında yeni çalışanlar bir city taşıma hakkında konuşma algılar. Bu iki tür konuşma aynıdır, ancak farklı LUIS becerileriyle ile çözüldü.

|Öğretici|Örnek utterance|Kaynak ve hedef konumları|
|--|--|--|
|[Hiyerarşik (Rol)](luis-quickstart-intent-and-hier-entity.md)|MV Jill Jones gelen **a-2349** için **b-1298**|a-2349 b-1298|
|Bu öğreticiyle (roller)|Öğesinden Billy Patterson ve Konuğu taşıma **Yuma** için **Denver**.|Yuma, Denver|

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, rollerine sahip bir varlık ile bir örnek konuşma hedefle eklendi. Varlık doğru kullanarak ilk uç nokta tahmin hedefi ancak düşük güven puanıyla birlikte tahmin. İki varlık yalnızca biri algılandı. Ardından, öğretici varlık rolleri ve deyim listesi konuşma Şehir adları değerini artırmak için kullanılan bir desen eklendi. İkinci uç nokta tahmin, bir yüksek güven puanı döndürülür ve iki varlık rolü bulunamadı. 

> [!div class="nextstepaction"]
> [LUIS uygulamaları için en iyi uygulamaları öğrenin](luis-concept-best-practices.md)
