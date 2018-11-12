---
title: 'Öğretici 4: Tam metin eşleşmesi - LUIS liste varlığı'
titleSuffix: Azure Cognitive Services
description: Bir listedeki önceden tanımlanmış öğelerle eşleşen verileri alma. Listedeki her öğenin tam olarak eşleşen eş anlamlıları da olabilir
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 2ce6e7c796faf0c7377a33dabe1e8c05e81fde2f
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51280723"
---
# <a name="tutorial-4-extract-exact-text-matches"></a>Öğretici 4: Tam metin eşleşmelerini ayıklama
Bu öğreticide bir listedeki önceden tanımlı öğelerle eşleşen verileri almayı öğreneceksiniz. Listeden her öğenin bir eşanlamlı sözcükler listesi olabilir. İnsan kaynakları uygulamasında, bir çalışan ad, e-posta, telefon numarası ve ABD federal vergi numarası gibi birkaç başlıca bilgi ile tanımlanabilir. 

İnsan Kaynakları uygulamasının hangi çalışanın bir binadan başka bir binaya taşındığını belirlemesi gerekiyor. LUIS, çalışan taşıma ile ilgili bir konuşmadan amacı belirliyor ve istemci uygulama tarafından çalışanı taşımak üzere standart bir emir oluşturulabilmesi için çalışanı çıkarıyor.

Bu uygulama çalışanı çıkarmak için bir varlık listesi kullanıyor. Çalışana ad, şirket telefonu, cep telefonu numarası, e-posta veya ABD federal sosyal güvenlik numarası kullanılarak değinilebilir. 

Liste varlığı bu veri türü için iyi bir seçimdir:

* Veri değerleri bilinen bir kümedir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Konuşmadaki metin bir eşanlamlı sözcük veya kurallı ad ile tam olarak eşleşiyor. 

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * MoveEmployee amacını ekleme
> * Liste varlığı ekleme 
> * Eğitim 
> * Yayımlama
> * Uç noktadan amaçları ve varlıkları alma

[!INCLUDE[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa aşağıdaki adımları izleyin:

1.  [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-regex-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `list` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez. 


## <a name="moveemployee-intent"></a>MoveEmployee amacı

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

3. Açılan iletişim kutusuna `MoveEmployee` girip **Done** (Bitti) öğesini seçin. 

    ![Create new intent (Yeni amaç oluştur) iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |move John W. Smith from B-1234 to H-4452 (John W. Smith'i B-1234'ten H-4452'e taşıyın)|
    |mv john.w.smith@mycompany.com from office b-1234 to office h-4452 (kişiyi b-1234 numaralı ofisten h-4452 numaralı ofise taşıyın)|
    |shift x12345 to h-1234 tomorrow (yarın x12345 ile h-1234'ü değiştirin)|
    |place 425-555-1212 in HH-2345 (425-555-1212'yi HH-2345'e alın)|
    |move 123-45-6789 from A-4321 to J-23456 (123-45-6789'yi A-4321'den J-23456'ya taşıyın)|
    |mv Jill Jones from D-2345 to J-23456 (D-2345'teki Jill Jones'u J-23456'ya taşıyın)|
    |shift jill-jones@mycompany.com to M-12345 (kişiyi M-12345'e alın)|
    |x23456 to M-12345|
    |425-555-0000 to h-4452|
    |234-56-7891 to hh-2345|

    [ ![Yeni konuşmaların vurgulandığı Intent (Amaç) sayfasının ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

    Buradaki number ve datetimeV2 varlıklarının önceki bir öğreticide eklendiğini ve herhangi bir örnek konuşmada bulunduğunda otomatik olarak etiketleneceğini unutmayın.

    [!INCLUDE[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="employee-list-entity"></a>Çalışan listesi varlığı
**MoveEmployee** amacına örnek konuşmalar eklendiğine göre artık LUIS uygulamasının çalışanın ne olduğunu anlaması gerekir. 

Her öğenin birincil adı olan _kurallı ad_ çalışanın numarasıdır. Bu etki alanı için her kurallı adın eş anlamlı sözcüklerinin örnekleri şunlardır: 

|Eş anlamlı amacı|Eş anlamlı değeri|
|--|--|
|Adı|John W. Smith|
|E-posta adresi|john.w.smith@mycompany.com|
|Dahili telefon|x12345|
|Kişisel cep telefonu numarası|425-555-1212|
|ABD sosyal güvenlik numarası|123-45-6789|


1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

2. **Create new entity** (Yeni varlık oluştur) öğesini seçin.

3. Açılan varlık iletişim kutusunda varlık adı olarak `Employee`, varlık türü olarak da **List** (Liste) seçin. **Done** (Bitti) öğesini seçin.  

    [![](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "Yeni varlık oluşturma açılan iletişim kutusunun ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Employee (Çalışan) varlık sayfasında yeni değer olarak `Employee-24612` girin.

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "Değer girme işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Eş anlamlılar için aşağıdaki değerleri ekleyin:

    |Eş anlamlı amacı|Eş anlamlı değeri|
    |--|--|
    |Adı|John W. Smith|
    |E-posta adresi|john.w.smith@mycompany.com|
    |Dahili telefon|x12345|
    |Kişisel cep telefonu numarası|425-555-1212|
    |ABD sosyal güvenlik numarası|123-45-6789|

    [![](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "Eş anlamlıları girme sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Yeni değer olarak `Employee-45612` girin.

7. Eş anlamlılar için aşağıdaki değerleri ekleyin:

    |Eş anlamlı amacı|Eş anlamlı değeri|
    |--|--|
    |Adı|Jill Jones|
    |E-posta adresi|jill-jones@mycompany.com|
    |Dahili telefon|x23456|
    |Kişisel cep telefonu numarası|425-555-0000|
    |ABD sosyal güvenlik numarası|234-56-7891|

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktadan amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Adres çubuğundaki URL'nin sonuna gidip `shift 123-45-6789 from Z-1242 to T-54672` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `MoveEmployee` amacını `Employee` ayıklanmış şekilde döndürmelidir.

  ```JSON
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  Çalışan bulundu, `Employee` türü ve `Employee-24612` çözünürlük değeriyle döndürüldü.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu, örnek konuşmalar eklendi, ardından konuşmalardan tam metin eşleşmeleri ayıklamak için bir liste varlığı oluşturuldu. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Uygulamaya hiyerarşik varlık ekleme](luis-quickstart-intent-and-hier-entity.md)

