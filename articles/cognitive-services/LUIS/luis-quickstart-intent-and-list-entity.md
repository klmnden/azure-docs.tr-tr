---
title: Tam metin eşleşmesi
titleSuffix: Azure Cognitive Services
description: Bir listedeki önceden tanımlanmış öğelerle eşleşen verileri alma. Listedeki her öğenin tam olarak eşleşen eş anlamlıları da olabilir
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 9083227dd81dca219666e07b70f487069413855d
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521280"
---
# <a name="tutorial-get-exact-text-matched-data-from-an-utterance"></a>Öğretici: Bir utterance tam metni eşleştirilen veri alma

Bu öğreticide, önceden tanımlanmış öğelerin listesini eşleşen varlık verilerini alma hakkında bilgi edinin. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Uygulama oluştur
> * Amaç ekleme
> * Liste varlığı ekleme 
> * Eğitim 
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="what-is-a-list-entity"></a>Bir liste varlığı nedir?

Bir liste varlığı, utterance sözcükler için tam metin eşleşmedir. 

Listeden her öğenin bir eşanlamlı sözcükler listesi olabilir. İnsan Kaynakları uygulaması için bir şirket departman birkaç temel bilgiler resmi bir adı, genel anlamda kısaltmalardan ve faturalama departmanı kodları gibi belirlenebilir. 

Bir çalışan için aktarma departmanı belirlemekte İnsan Kaynakları uygulaması gerekir. 

Liste varlığı bu veri türü için iyi bir seçimdir:

* Veri değerleri bilinen bir kümedir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Konuşmadaki metin bir eşanlamlı sözcük veya kurallı ad ile tam olarak eşleşiyor. LUIS, tam metin eşleşme ötesinde listesi kullanmaz. Dallanma, gerçekleştirebilse ve diğer Çeşitlemeler yalnızca bir liste varlığı ile çözümlenmiyor. Değişimleri yönetmek için kullanmayı bir [deseni](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) isteğe bağlı metin sözdizimine sahip. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-an-intent-to-transfer-employees-to-a-different-department"></a>Çalışanlar için farklı bir bölüm aktarmak için bir hedefi oluşturma

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

3. Açılan iletişim kutusuna `TransferEmployeeToDepartment` girip **Done** (Bitti) öğesini seçin. 

    ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |John W. Smith için muhasebe Taşı|
    |R & D Jill Jones alanından aktarma|
    |Bölüm 1234 fatura Bradstreet adlı yeni bir üye içeriyor|
    |John Jackson Mühendisliğine yerleştirin |
    |Gamze Doughtery satış içine taşı|
    |MV Jill Jones için BT|
    |DevOps için Shift Alice Anderson|
    |Carl Chamerlin Finans için|
    |Steve Standish 1234 için|
    |3456 için Tanner Thompson|

    [![Ekran görüntüsü örnek konuşma hedefle](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png "örnek konuşma hedefle ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-transfer-employee-to-department.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="department-list-entity"></a>Bölüm listesi varlığı

Şimdi **TransferEmployeeToDepartment** anlaşılabilmelidir örnek konuşma, LUIS departmanı anlamak gerekir. 

Birincil _kurallı_, her öğe için bölüm adı adı. Eş Anlamlılar her kurallı adı örnekleri şunlardır: 

|Kurallı ad|Eş anlamlılar|
|--|--|
|Muhasebe|Hesap<br>accting<br>3456|
|Geliştirme işlemleri|DevOps<br>4949|
|Mühendislik|eng<br>altyapısındaki<br>4567|
|Finans|Bul<br>2020|
|Bilgi teknolojisi|BT<br>2323|
|İçinde satış|isale<br>insale<br>1414|
|Araştırma ve geliştirme|AR -GE<br>1234|

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

1. **Create new entity** (Yeni varlık oluştur) öğesini seçin.

1. Açılan varlık iletişim kutusunda varlık adı olarak `Department`, varlık türü olarak da **List** (Liste) seçin. **Done** (Bitti) öğesini seçin.  

    [![Yeni varlık açılır iletişim kutusu oluşturma işleminin ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png "yeni varlık açılır iletişim kutusu oluşturma işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/create-new-list-entity-named-department.png#lightbox)

1. Departman varlık sayfasında girin `Accounting` yeni değeri.

    [![Değer girme ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "değeri girme ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

1. Eş Anlamlılar için önceki tablosundan eş anlamlı sözcükler ekleyin.

    [![Eş Anlamlılar girme ekran görüntüsü](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "eş anlamlılar girme ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

1. Kurallı tüm adlarını ve bunların eş anlamlılar eklemeye devam edebilirsiniz. 

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Uygulama hedefi değişiklikleri test edilebilir şekilde eğitme 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Eğitilen modelin uç noktasından sorgulanabilir, bu nedenle, uygulamayı yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Uç noktasından amacı ve varlık öngörü Al

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Adres çubuğundaki URL'nin sonuna gidip `shift Joe Smith to IT` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `TransferEmployeeToDepartment` amacını `Department` ayıklanmış şekilde döndürmelidir.

   ```json
    {
      "query": "shift Joe Smith to IT",
      "topScoringIntent": {
        "intent": "TransferEmployeeToDepartment",
        "score": 0.9775754
      },
      "intents": [
        {
          "intent": "TransferEmployeeToDepartment",
          "score": 0.9775754
        },
        {
          "intent": "None",
          "score": 0.0154493852
        }
      ],
      "entities": [
        {
          "entity": "it",
          "type": "Department",
          "startIndex": 19,
          "endIndex": 20,
          "resolution": {
            "values": [
              "Information Technology"
            ]
          }
        }
      ]
    }
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>İlgili bilgiler

* [Varlık listesinde](luis-concept-entity-types.md#list-entity) kavramsal bilgiler
* [Eğitme](luis-how-to-train.md)
* [Yayımlama nasıl yapılır?](luis-how-to-publish-app.md)
* [LUIS portalında test etme](luis-interactive-test.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu, örnek konuşmalar eklendi, ardından konuşmalardan tam metin eşleşmeleri ayıklamak için bir liste varlığı oluşturuldu. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

Bu uygulama ile devam [bileşik bir varlık ekleme](luis-tutorial-composite-entity.md).

> [!div class="nextstepaction"]
> [Uygulamaya bir role sahip önceden oluşturulmuş bir varlık ekleme](tutorial-entity-roles.md)

