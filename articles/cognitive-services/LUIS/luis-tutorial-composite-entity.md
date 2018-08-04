---
title: Karmaşık verileri - Azure ayıklamak için bileşik bir varlık oluşturma Öğreticisi | Microsoft Docs
description: LUIS uygulamanızı farklı türde varlık verilerini ayıklamak için bileşik bir varlık oluşturmayı öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 5f11409ff49830be97d9a13a0ab7f033d9cc1041
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494474"
---
# <a name="tutorial-6-add-composite-entity"></a>Öğretici: 6. Bileşik varlık ekleme 
Bu öğreticide, bileşik bir varlık içeren bir varlığa ayıklanan veri paketi ekleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

<!-- green checkmark -->
> [!div class="checklist"]
> * Bileşik varlıkları anlama 
> * Verileri ayıklamak için bileşik bir varlık ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce
[Hiyerarşik varlık](luis-quickstart-intent-and-hier-entity.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](luis-how-to-start-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-hier-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `composite` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar.  

## <a name="composite-entity-is-a-logical-grouping"></a>Bileşik varlık mantıksal bir gruplandırmasıdır 
Bileşik varlık amacı bir üst kategori varlığa ilgili varlıkları grup. Bir bileşik oluşturulmadan önce bilgi ayrı varlıklar olarak bulunmaktadır. Hiyerarşik bir varlığa benzer, ancak daha fazla varlık türlerini içerebilir. 

 Mantıksal olarak ayrı varlıklar gruplandırılmasını ve bu mantıksal gruplandırma istemci uygulamasına yardımcı olduğunda bileşik bir varlık oluşturun. 

Bu uygulamada çalışan adı tanımlanan **çalışan** varlık listesinde ve adı, e-posta adresi, şirketin dahili telefon, cep telefonu numarası ve ABD eş anlamlı sözcükler içerir Federal Vergi kimliği 

**MoveEmployee** çalışan taşınabilir bir bina ve office diğerine istemek için örnek konuşma anlaşılabilmelidir. Yapı adları alfabetik: "A", "B" vb. ofisleri sayısal olmasına rağmen: "1234", "13245". 

Örnek konuşma içinde **MoveEmployee** hedefini içerir:

|Örnek konuşmalar|
|--|
|John W taşıyın. Smith a-2345 için|
|shift x12345 to h-1234 tomorrow (yarın x12345 ile h-1234'ü değiştirin)|
 
Taşıma isteği, en az (herhangi bir eş anlamlı kullanarak) çalışan ve son bina ve ofis konumu içermelidir. İstek, bir tarih taşıma olacağını yanı sıra kaynak office de içerebilir. 

Ayıklanan uç noktasına ait verilerin bu bilgileri içeren ve bunu, iade bir `RequestEmployeeMove` Bileşik varlık. 

## <a name="create-composite-entity"></a>Bileşik varlık oluşturma
1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

2. Üzerinde **hedefleri** sayfasında **MoveEmployee** hedefi. 

3. Araç çubuğundaki konuşma listeyi filtrelemek için büyüteç simgesini seçin. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Büyüteç düğmesi vurgulanan 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Girin `tomorrow` utterance bulmak için filtre metin kutusuna `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Vurgulanan 'yarının' filtresi 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Varlık seçerek datetimeV2 göre filtre uygulamak için başka bir yöntemdir **varlık filtreleri** seçip **datetimeV2** listeden. 

5. İlk varlığı seçin `Employee`, ardından **kaydırma bileşik varlıkta** açılır menüsü listesinde. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "İlk varlık içinde bileşik vurgulanmış seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Son varlık hemen ardından `datetimeV2` utterance içinde. Bileşik bir varlığı gösteren sözcüklerin altında yeşil bir çubuk çizilir. Açılır menüde, bileşik bir ad girin `RequestEmployeeMove` seçip **oluştur yeni birleşik** üzerinde açılır menüde. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Vurgulanan son varlık bileşik ve oluşturma varlıkta seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?**, gerekli neredeyse tüm alanlar listesinde bağlıdır. Yalnızca kaynak konumunda eksik. Seçin **alt varlık ekleme**seçin **Locations::Origin** var olan varlıkları listesinden seçip **Bitti**. 

    ![Açılır pencerede başka bir varlık ekleme 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Filtreyi kaldırmak için araç çubuğundaki Büyüteç'i seçin. 

## <a name="label-example-utterances-with-composite-entity"></a>Bileşik varlıkla etiket örnek konuşma
1. Her örnek utterance içinde bileşik içinde olması gereken en soldaki varlığı seçin. Ardından **kaydırma bileşik varlıkta**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "İlk varlık içinde bileşik vurgulanmış seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Bileşik varlıkta son sözcüğü eşleyebilir ve ardından seçin **RequestEmployeeMove** açılır menüden. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Son varlık seçme içinde bileşik vurgulanmış 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Tüm konuşma amacı, bileşik bir varlık ile etiketlenmiş doğrulayın. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Etiketli tüm konuşma ile 'MoveEmployee' üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint"></a>Sorgu bitiş noktası 

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.` yazın. Son sorgu dizesi parametresi `q`, utterance sorgu. 

    Bileşik doğru bir şekilde ayıklandıktan doğrulamak için bu test olduğundan, bir test ya da mevcut bir örnek utterance veya yeni bir utterance içerebilir. Bileşik varlıktaki tüm alt varlıklar eklemek iyi bir testtir.

    ```JSON
    {
      "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
      "topScoringIntent": {
        "intent": "MoveEmployee",
        "score": 0.9959525
      },
      "intents": [
        {
          "intent": "MoveEmployee",
          "score": 0.9959525
        },
        {
          "intent": "GetJobInformation",
          "score": 0.009858314
        },
        {
          "intent": "ApplyForJob",
          "score": 0.00728598563
        },
        {
          "intent": "FindForm",
          "score": 0.0058053555
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.005371796
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00266987388
        },
        {
          "intent": "None",
          "score": 0.00123299169
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00116407464
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00102653319
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.0006628214
        }
      ],
      "entities": [
        {
          "entity": "march 3 2 p.m",
          "type": "builtin.datetimeV2.datetime",
          "startIndex": 41,
          "endIndex": 54,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2018-03-03 14:00:00"
              },
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2019-03-03 14:00:00"
              }
            ]
          }
        },
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 5,
          "endIndex": 14,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "z - 2345",
          "type": "Locations::Destination",
          "startIndex": 31,
          "endIndex": 36,
          "score": 0.9690751
        },
        {
          "entity": "a - 1234",
          "type": "Locations::Origin",
          "startIndex": 21,
          "endIndex": 26,
          "score": 0.9713137
        },
        {
          "entity": "-1234",
          "type": "builtin.number",
          "startIndex": 22,
          "endIndex": 26,
          "resolution": {
            "value": "-1234"
          }
        },
        {
          "entity": "-2345",
          "type": "builtin.number",
          "startIndex": 32,
          "endIndex": 36,
          "resolution": {
            "value": "-2345"
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 47,
          "endIndex": 47,
          "resolution": {
            "value": "3"
          }
        },
        {
          "entity": "2",
          "type": "builtin.number",
          "startIndex": 50,
          "endIndex": 50,
          "resolution": {
            "value": "2"
          }
        },
        {
          "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "type": "requestemployeemove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "requestemployeemove",
          "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "children": [
            {
              "type": "builtin.datetimeV2.datetime",
              "value": "march 3 2 p.m"
            },
            {
              "type": "Locations::Destination",
              "value": "z - 2345"
            },
            {
              "type": "Employee",
              "value": "jill jones"
            },
            {
              "type": "Locations::Origin",
              "value": "a - 1234"
            }
          ]
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
    }
    ```

  Bu utterance bir bileşik varlıkları dizisi döndürür. Her varlık türü ve değeri verilir. Her bir alt varlık için daha fazla duyarlık bulmak için karşılık gelen öğe varlıkları dizide bulmak için bileşik bir dizi öğesi türü ve bir kombinasyonu kullanın.  

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Bu uygulama, doğal dil sorgu engellemekse tanımlanır ve ayıklanan verilerin adlandırılmış bir grup olarak döndürdü. 

Sohbet Robotu, artık birincil eylem ve ilgili ayrıntıları utterance olduğunu belirlemede yeterli bilgi vardır. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"] 
> [Bir ifade listesi tek bir varlığın eklemeyi öğrenin](luis-quickstart-primary-and-secondary-data.md)  