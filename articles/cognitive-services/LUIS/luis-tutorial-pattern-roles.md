---
title: LUIS tahminleri - Azure geliştirmek için desen rolleri kullanma Öğreticisi | Microsoft Docs
titleSuffix: Cognitive Services
description: Bu öğreticide, bağlamsal ilgili varlıkları için desen rolleri LUIS Öngörüler geliştirmek için kullanın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 938b868fadfa102174b270a222494710fc156442
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174568"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Öğretici: desen rolleri ile uygulama geliştirin

Bu öğreticide, hedefi ve varlık tahmin artırmak için birleştirilmiş desenler rolleri ile bir varlığın kullanın.  Desenler kullanırken, daha az örnek konuşma amacı için gereklidir.

> [!div class="checklist"]
* Desen rollerini anlama
* Varlığın rolleriyle kullanın 
* Varlığın rolleriyle kullanarak konuşma deseni oluşturma
* Desen tahmin geliştirmeleri doğrulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
İnsan Kaynakları uygulamadan yoksa [deseni](luis-tutorial-pattern.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarılacak uygulamasını bulunan [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json) GitHub deposu.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `roles` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="the-purpose-of-roles"></a>Rolleri amacı
Rolleri amacı bir utterance bağlamsal olarak ilişkili varlıkları ayıklamaktır. Utterance içinde `Move new employee Robert Williams from Sacramento and San Francisco`, kaynak Şehir ve hedef Şehir değerlerini birbiriyle ilgili ve ortak dil her bir konum belirtmek için kullanın. 

Desenleri kullanırken, herhangi bir varlık deseninde algılanması gereken _önce_ utterance desenle eşleşir. 

Bir desen oluşturduğunuzda, ilk adım desenin amacı seçmektir. Desen eşleşirse, amaç seçerek doğru amacı her zaman yüksek puanıyla döndürülür (genellikle 99 %100). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Hiyerarşik varlık varlığın rolleri ile Karşılaştır

İçinde [hiyerarşik öğretici](luis-quickstart-intent-and-hier-entity.md), **MoveEmployee** hedefi varolan bir çalışan bir yapı ve office diğerine taşımak ne zaman algılandı. Örnek konuşma kaynak ve hedef konumların vardı, ancak rol kullanmamıştır. Bunun yerine, alt öğeleri hiyerarşik varlık kaynak ve hedef yoktu. 

Bu öğreticide, İnsan Kaynakları uygulamasında yeni çalışanlar bir city taşıma hakkında konuşma algılar. Bu iki tür konuşma benzer ancak farklı LUIS becerileriyle ile çözüldü.

|Öğretici|Örnek utterance|Kaynak ve hedef konumları|
|--|--|--|
|[Hiyerarşik (Rol)](luis-quickstart-intent-and-hier-entity.md)|MV Jill Jones gelen **a-2349** için **b-1298**|a-2349 b-1298|
|Bu öğreticiyle (roller)|Öğesinden Billy Patterson ve Konuğu taşıma **Yuma** için **Denver**.|Yuma, Denver|

Yalnızca hiyerarşik üst içindeki kullanıldığından deseninde hiyerarşik varlık kullanamazsınız. Kullanım muse kaynak ve hedef adlandırılmış konumlar döndürmek için bir desen.

### <a name="simple-entity-for-new-employee-name"></a>Basit varlık için yeni çalışan adı
Yeni bir çalışan, Billy Patterson ve Konuğu, adını liste varlığı bir parçası değil **çalışan** henüz. Yeni çalışan adı, ilk olarak, şirket kimlik bilgilerini oluşturmak için bir dış sistem adı göndermek için ayıklanır. Çalışan kimlik bilgilerini şirket kimlik bilgilerini oluşturulduktan sonra liste varlığı için eklenen **çalışan**.

**Çalışan** listesi oluşturulduğu [liste Öğreticisi](luis-quickstart-intent-and-list-entity.md).

**Yeniçalışan** rol ile bir varlığın bir varlıktır. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Varlığın rollerle yeniden konumlandırma şehirlere
Aile ve yeni çalışan adlı kurgusal şirketin bulunduğu şehir için geçerli city taşınacak gerekir. Yeni bir çalışan herhangi bir şehre gelebileceğinden konumları bulunmaları gerekir. Yalnızca liste şehirlerin ayıklanan çünkü kümesi listesini bir liste varlığı gibi çalışmaz.

Kaynak ve hedef şehirleri ile ilişkili rol adları tüm varlıklar arasında benzersiz olması gerekir. Rolleri benzersiz olduğundan emin olmak için kolay bir yolu bunları adlandırma stratejisi aracılığıyla içeren varlık için bağlamaktır. **NewEmployeeRelocation** varlıktır iki rol ile basit bir varlık: **NewEmployeeReloOrigin** ve **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Basit varlıkları algılanamayacak kadar yeterli örneği ihtiyacı
Çünkü örnek utterance `Move new employee Robert Williams from Sacramento and San Francisco` yalnızca makine öğrenilen varlıklar, sahip varlıklar algılanan şekilde yeterli örnek konuşma amacı sağlamak önemlidir.  

**Desenler varlıkları değil algılanırsa, daha az örnek konuşma sağlamanıza olanak tanırken desenle eşleşmez.**

Bir şehir gibi bir adı olduğundan varlığın algılama ile sorun yaşıyorsanız, benzer değer ifade listesi eklemeyi göz önünde bulundurun. Bu, şehir adı algılanması sözcük veya tümcecik türü hakkında ek bir sinyal LUIS sağlayarak yardımcı olur. İfade listeleri yalnızca eşleşme gerçekleştirilecek desenin için gerekli olan varlık algılama yardımcı olarak deseni yardımcı olur. 

## <a name="create-new-entities"></a>Yeni varlık oluşturma
1. Seçin **derleme** üst menüdeki.

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

    |Konuşma|Yeniçalışan|NewEmployeeRelocation|
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

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
Yeni amacı ve konuşma eğitim gerektirir. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-without-pattern"></a>Sorgu deseni olmadan bir uç noktası
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

2. Adres çubuğundaki URL'nin sonuna gidip `Move Wayne Berry from Miami to Mount Vernon` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

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

Desenlerini tahmin puanı yardımcı olur, önce utterance desenle eşleşir ancak varlıkları doğru şekilde tahmin gerekir. 

## <a name="add-a-pattern-that-uses-roles"></a>Rolleri kullanan bir desen Ekle
1. Seçin **derleme** üst gezintideki.

2. Seçin **desenleri** sol gezinti bölmesinde.

3. Seçin **NewEmployeeRelocationProcess** gelen **bir amaç seçin** aşağı açılan listesi. 

4. Şu deseni girin: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Eğitim, yayımlama ve uç nokta sorgu desen ile eşleşmedi, varlıkları, bulunmayan, görmek hayal kırıklığına olabilir bu nedenle tahminini geliştirme kaydetmedi. Yeterli sayıda örnek konuşma etiketli varlıklarla bir sonucu budur. Daha fazla örnek eklemek yerine, bu sorunu gidermek için bir ifade listesi ekleyin.

## <a name="create-a-phrase-list-for-cities"></a>Şehir için bir ifade listesi oluşturun
Herhangi bir sözcük ve noktalama karışımını olabilirler, şehirler, kişilerin adları gibi hassas. Ancak öğrenme başlamak için Şehir ifade listesi LUIS gerekir müşteri Şehir Bölge ve dünya bilinmektedir. 

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

    Her şehir dünyanın veya bölgede bile her şehir eklemeyin. LUIS generalize mümkün olması gerekir hangi şehirde listesidir. 

    Tutmaya dikkat **birbirinin yerine bu değerleri** seçili. Bu ayar listesindeki sözcükler üzerinde eş anlamlı sözcükler kabul anlamına gelir. Düzende bunların nasıl değerlendirilip tam olarak budur.

    Unutmayın [son](luis-quickstart-primary-and-secondary-data.md) bir ifade listesini oluşturan öğretici serisinin bir varlığın varlık algılama artırmak için ayrıca oldu.  

3. Eğitim ve uygulama yayımlama.

## <a name="query-endpoint-for-pattern"></a>Desen için sorgu uç noktası
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

2. Adres çubuğundaki URL'nin sonuna gidip `Move wayne berry from miami to mount vernon` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. 

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

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için üç noktayı seçin (***...*** ) sağında bulunan uygulama listesinde uygulama adı, seçin **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS uygulamaları için en iyi uygulamaları öğrenin](luis-concept-best-practices.md)