---
title: 'Öğretici 3: Normal ifade ile eşleşen veri - düzgün biçimlendirilmiş veri ayıklama'
titleSuffix: Azure Cognitive Services
description: Normal İfade varlığını kullanarak bir konuşmadaki tutarlı olarak biçimlendirilmiş verileri ayıklayın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 06e212ef756fda9224b38b41c69c7c4eccfb9796
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159865"
---
# <a name="tutorial-3-extract-well-formatted-data"></a>Öğretici 3: Düzgün biçimlendirilmiş verileri ayıklama
Bu öğreticide, **Normal İfade** varlığını kullanarak bir konuşmadan tutarlı olarak biçimlendirilmiş veriler ayıklamak için İnsan Kaynakları uygulamasını değiştirme anlatılmaktadır.

Varlığın amacı, konuşmada bulunan önemli verileri almaktır. Bu uygulama, normal ifade varlığını kullanarak bir konuşmadaki biçimlendirilmiş İnsan Kaynakları (İK) Form numaralarını çekmektedir. Konuşmanın amacı her zaman makine öğrenimi ile belirlenirse de bu özel varlık türü makine öğrenimli değildir. 

**Basit konuşma örnekleri:**

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Normal bir ifade, şu durumlarda bu tür veri için iyi bir seçimdir:

* veriler düzgün biçimlendirilmiş olduğunda.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * FindForm amacı ekleme
> * Normal ifade varlığı ekleme 
> * Eğitim
> * Yayımlama
> * Uç noktasındaki amaçları ve varlıkları alma

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Mevcut uygulamayı kullanma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Önceki öğreticinin HumanResources uygulaması elinizde yoksa, aşağıdaki adımları izleyin:

1. [Uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-prebuilts-HumanResources.json) indirin ve kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde, **Sürümler** sekmesinde, sürümü kopyalayın ve `regex` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan hiçbir karakter içeremez. 

## <a name="findform-intent"></a>FindForm amacı

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

3. Açılan iletişim kutusuna `FindForm` girip **Done** (Bitti) öğesini seçin. 

    ![Arama kutusunda Utilities (Yardımcı Programlar) yazan yeni amaç oluşturma iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Amaca örnek konuşmalar ekleyin.

    |Örnek konuşmalar|
    |--|
    |What is the URL for hrf-123456? (hrf-123456 numaralı formun URL'si nedir?)|
    |Where is hrf-345678? (hrf-345678 nerede?)|
    |When was hrf-456098 updated? (hrf-456098 ne zaman güncelleştirildi?)|
    |Did John Smith update hrf-234639 last week? (John Smith, hrf-234639 numaralı formu geçen hafta güncelleştirdi mi?)|
    |How many version of hrf-345123 are there? (hrf-345123 numaralı formun kaç farklı sürümü var?)|
    |Who needs to authorize form hrf-123456? (hrf-123456 numaralı formun yetkilisi kim?)|
    |How many people need to sign off on hrf-345678? (hrf-345678 numaralı formu kaç kişinin onaylaması gerekiyor?)|
    |hrf-234123 date? (hrf-234123 numaralı formun tarihi nedir?)|
    |author of hrf-546234? (hrf-546234 numaralı formun yazarı kim?)|
    |title of hrf-456234? (hrf-456234 numaralı formun başlığı nedir?)|

    [ ![Yeni konuşmaların vurgulandığı Intent (Amaç) sayfasının ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    Uygulamaya önceki öğreticide eklenmiş numaralar vardır ve bu nedenle her form etiketlenmiştir. Bu kadarı istemci uygulamanız için yeterli olabilir ancak numara, numara türü etiketi ile etiketlenmiş olmayacaktır. Uygun bir adla yeni bir varlık oluşturulması, istemci uygulamasının LUIS tarafından döndürülen varlığı uygun şekilde işlemesini sağlar.

    [!include[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="regular-expression-entity"></a>Normal ifade varlığı 
Form numarasıyla eşleştirilecek normal ifade varlığı: `hrf-[0-9]{6}`. Bu normal ifade, `hrf-` değişmez karakterleriyle eşleşir ancak büyük/küçük harf ve kültür farklarını yoksayar. Tam olarak 6 basamak için 0-9 basamaklarını eşleştirir.

HRF `human resources form` anlamına gelir.

LUIS, bir amaca eklendiğinde konuşmayı belirteçlerine ayırır. Bu konuşmalar için belirteçlere ayırma işlemi, kısa çizginin önüne ve arkasına boşluk ekler, `Where is HRF - 123456?` Normal ifade, konuşmanın belirteçlere ayrılmamış ham biçimine uygulanır. Normal ifade _ham_ biçimine uygulandığından sözcük sınırlarıyla ilgilenmesi gerekmez. 

Aşağıdaki adımları izleyerek LUIS uygulamasına HRF-numara biçimini bildiren bir normal ifade varlığı oluşturun:

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

2. Entities (Varlıklar) sayfasında **Create new entity** (Yeni varlık oluştur) düğmesini seçin. 

3. Açılan iletişim kutusuna yeni varlık adı olarak `HRF-number` girin, varlık türü olarak **RegEx**'i seçin, **Normal İfade** değeri olarak `hrf-[0-9]{6}` girin ve ardından **Bitti** düğmesini seçin.

    ![Yeni varlık özelliklerinin ayarlandığı açılan iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Sol menüden **Amaçlar**'ı, ardından **FindForm** amacını seçerek konuşmalarda etiketlenmiş olan normal ifadeleri görebilirsiniz. 

    [![Konuşmayı var olan varlıkla ve normal ifade düzeniyle etiketleme işleminin ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Varlık, makine öğrenmesi varlığı olmadığından etiket oluşturulduktan hemen sonra konuşmalara uygulanır ve LUIS web sitesinde görüntülenir.

## <a name="train"></a>Eğitim

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Yayımlama

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Uç noktasındaki amacı ve varlıkları alma

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `When were HRF-123456 and hrf-234567 published in the last year?` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `FindForm` amacını `HRF-123456` ile `hrf-234567` olmak üzere iki form numarasını döndürmelidir.

    ```JSON
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Konuşmadaki numaralar bir kez yeni varlık (`hrf-number`), bir kez de önceden oluşturulmuş varlık (`number`) olmak üzere iki kez döndürülür. Bu örnekte gördüğünüz gibi bir konuşmada birden fazla varlık ve aynı varlık türünden birden fazla bulunabilir. LUIS, normal ifade varlığı kullanarak adlandırılmış verileri ayıklar ve bu durum, JSON yanıtını alan istemci yazılımı için program açısından daha faydalıdır.


## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide yeni bir amaç oluşturuldu, örnek konuşmalar eklendi ve ardından konuşmalardan düzgün biçimlendirilmiş veriler ayıklamak için normal ifade varlığı eklendi. Uygulama eğitildikten ve yayımlandıktan sonra uç noktaya gönderilen bir sorgu amacı tanımladı ve ayıklanan verileri döndürdü.

> [!div class="nextstepaction"]
> [Liste varlığı hakkında bilgi edinin](luis-quickstart-intent-and-list-entity.md)

