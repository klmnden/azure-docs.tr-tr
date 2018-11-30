---
title: 'Öğreticinin 6: LUIS bileşik varlıkla bileşik veri ayıklamak'
titleSuffix: Azure Cognitive Services
description: Çeşitli türlerde ayıklanan verileri içeren tek bir varlığa paket için bileşik bir varlık ekleyin. İstemci uygulama, verileri paketleme tarafından farklı veri türlerinde ilgili verileri kolayca ayıklayabilirsiniz.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 8f7edecf1abd1f01a2f40f1420a6a85224271239
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423510"
---
# <a name="tutorial-6-group-and-extract-related-data"></a>Öğreticinin 6: Grup ve ilgili verileri ayıklayın
Bu öğreticide, bileşik bir varlık içeren tek bir varlığa çeşitli türlerde ayıklanan veri paketi ekleyin. İstemci uygulama, verileri paketleme tarafından farklı veri türlerinde ilgili verileri kolayca ayıklayabilirsiniz.

Bileşik varlık amacı bir üst kategori varlığa ilgili varlıkları grup. Bir bileşik oluşturulmadan önce bilgi ayrı varlıklar olarak bulunmaktadır. Bu, hiyerarşik varlığa benzer ancak değişik tür varlıklar içerebilir. 

Bileşik varlık olduğundan bu veri türü için uygun olan veri:

* Birbiriyle ilgilidir. 
* Varlık türleri çeşitli kullanın.
* İstemci uygulama tarafından bir bilgi birimi olarak gruplanmaları ve işlenmeleri gerekir.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Bileşik varlık ekleme 
> * Eğitim
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-hier-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `composite` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.


## <a name="composite-entity"></a>Bileşik varlık
Mantıksal olarak ayrı varlıklar gruplandırılmasını ve bu mantıksal gruplandırma istemci uygulamasına yardımcı olduğunda bileşik bir varlık oluşturun. 

Bu uygulamada çalışan adı tanımlanan **çalışan** varlık listesinde ve adı, e-posta adresi, şirketin dahili telefon, cep telefonu numarası ve ABD eş anlamlı sözcükler içerir Federal Vergi kimliği 

**MoveEmployee** çalışan taşınabilir bir bina ve office diğerine istemek için örnek konuşma anlaşılabilmelidir. Yapı adları alfabetik: "A", "B" vb. ofisleri sayısal olmasına rağmen: "1234", "13245". 

Örnek konuşma içinde **MoveEmployee** hedefini içerir:

|Örnek konuşmalar|
|--|
|John W taşıyın. Smith a-2345 için|
|shift x12345 to h-1234 tomorrow (yarın x12345 ile h-1234'ü değiştirin)|
 
Taşıma isteği, (herhangi bir eş anlamlı kullanarak) çalışan ve son bina ve ofis konumu içermelidir. İstek, bir tarih taşıma olacağını yanı sıra kaynak office de içerebilir. 

Ayıklanan uç noktasına ait verilerin ve bu bilgileri içeren içinde döndürün `RequestEmployeeMove` Bileşik varlık:

```JSON
"compositeEntities": [
  {
    "parentType": "RequestEmployeeMove",
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
]
```

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Üzerinde **hedefleri** sayfasında **MoveEmployee** hedefi. 

3. Araç çubuğundaki konuşma listeyi filtrelemek için büyüteç simgesini seçin. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Büyüteç düğmesi vurgulanan 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Girin `tomorrow` utterance bulmak için filtre metin kutusuna `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Vurgulanan 'yarının' filtresi 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Varlık seçerek datetimeV2 göre filtre uygulamak için başka bir yöntemdir **varlık filtreleri** seçip **datetimeV2** listeden. 

5. İlk varlığı seçin `Employee`, ardından **kaydırma bileşik varlıkta** açılır menüsü listesinde. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "İlk varlık içinde bileşik vurgulanmış seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Son varlık hemen ardından `datetimeV2` utterance içinde. Bileşik bir varlığı gösteren sözcüklerin altında yeşil bir çubuk çizilir. Açılır menüde, bileşik adını `RequestEmployeeMove` seçin ENTER. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Vurgulanan son varlık bileşik ve oluşturma varlıkta seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?**, gerekli neredeyse tüm alanlar listesinde bağlıdır. Yalnızca kaynak konumunda eksik. Seçin **alt varlık ekleme**seçin **Locations::Origin** var olan varlıkları listesinden seçip **Bitti**. 

    Dikkat edin önceden oluşturulmuş bir varlık, sayı, bileşik varlığa eklendi. Başlangıç ve bitiş belirteçleri bileşik bir varlığın arasında görünür önceden oluşturulmuş bir varlık olabilir, önceden oluşturulmuş bu varlıkların bileşik varlığı içermelidir. Önceden oluşturulmuş varlıklar dahil edilmez, bileşik bir varlık değil doğru şekilde tahmin edildiğinde ancak her tek öğedir.

    ![Açılır pencerede başka bir varlık ekleme 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Filtreyi kaldırmak için araç çubuğundaki Büyüteç'i seçin. 

9. Word kaldırmak `tomorrow` filtresinden tüm örnek konuşma yeniden görürsünüz. 

## <a name="label-example-utterances-with-composite-entity"></a>Bileşik varlıkla etiket örnek konuşma


1. Her örnek utterance içinde bileşik içinde olması gereken en soldaki varlığı seçin. Ardından **kaydırma bileşik varlıkta**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "İlk varlık içinde bileşik vurgulanmış seçerek 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Bileşik varlıkta son sözcüğü eşleyebilir ve ardından seçin **RequestEmployeeMove** açılır menüden. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Son varlık seçme içinde bileşik vurgulanmış 'MoveEmployee' hedefi üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Tüm konuşma amacı, bileşik bir varlık ile etiketlenmiş doğrulayın. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Etiketli tüm konuşma ile 'MoveEmployee' üzerinde LUIS ekran görüntüsü")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

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
          "type": "RequestEmployeeMove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "RequestEmployeeMove",
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
      ]
    }
    ```

  Bu utterance bir bileşik varlıkları dizisi döndürür. Her varlık türü ve değeri verilir. Her bir alt varlık için daha fazla duyarlık bulmak için karşılık gelen öğe varlıkları dizide bulmak için bileşik bir dizi öğesi türü ve bir kombinasyonu kullanın.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, var olan varlıkları kapsüllemek için bileşik bir varlık oluşturuldu. Bu konuşmaya devam etmek için farklı veri türleri ilgili bir veri grubu bulmak istemci uygulaması sağlar. Bu İnsan Kaynakları uygulamasında, bir istemci uygulamanın hangi gün ve saat taşıma başlamalı ve bitmelidir gerekiyor sorabilirsiniz. Fiziksel bir telefon olarak movesuch başka bir Lojistik hakkında da sorabilirsiniz. 

> [!div class="nextstepaction"] 
> [Bir ifade listesi tek bir varlığın eklemeyi öğrenin](luis-quickstart-primary-and-secondary-data.md)  
