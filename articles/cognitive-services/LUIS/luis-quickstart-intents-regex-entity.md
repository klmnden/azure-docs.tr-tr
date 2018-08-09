---
title: 'Öğretici: Normal ifadeyle eşleşen verileri almak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide veri ayıklama amacıyla amaçları ve bir normal ifade varlığı kullanan basit bir LUIS uygulaması oluşturmayı öğreneceksiniz.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 994bd6f2a041e25d15c7e0b4a216952cec4101fa
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492832"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>Öğretici: 3. Normal ifade varlığı ekleme
Bu öğreticide **Regular Expression** varlığını kullanarak bir konuşmadaki tutarlı bir şekilde biçimlendirilmiş verileri ayıklamayı gösteren bir uygulama oluşturacaksınız.


<!-- green checkmark -->
> [!div class="checklist"]
> * Normal ifade varlıklarını anlama 
> * FindForm amacı ile İnsan Kaynakları (İK) alanı için bir LUIS uygulaması kullanma
> * Konuşmadan Form numarasını ayıklamak için normal ifade varlığı ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce
[Önceden oluşturulmuş varlıklar](luis-tutorial-prebuilt-intents-entities.md) öğreticisinde oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json) Github deposundaki JSON verilerini [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulamaya [aktarın](luis-how-to-start-new-app.md#import-new-app).

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `regex` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 


## <a name="purpose-of-the-regular-expression-entity"></a>Normal ifade varlığının amacı
Varlığın amacı, konuşmada bulunan önemli verileri almaktır. Uygulama, normal ifade varlığını bir konuşmadaki biçimlendirilmiş İnsan Kaynakları (İK) Form numaralarını çekmektir. Makine öğrenmesi verisi değildir. 

Basit konuşma örnekleri şunlardır:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

Konuşmaların kısaltılmış veya argo sürümleri şunlardır:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Form numarasıyla eşleştirilecek normal ifade varlığı: `hrf-[0-9]{6}`. Bu normal ifade, `hrf -` değişmez karakterleriyle eşleşir ancak büyük/küçük harf ve kültür farklarını yoksayar. Tam olarak 6 basamak için 0-9 basamaklarını eşleştirir.

HRF, insan kaynakları formunu ifade eder.

### <a name="tokenization-with-hyphens"></a>Kısa çizgilerle belirteçlere ayırma
LUIS, amaca eklenmiş olan konuşmayı belirteçlere ayırır. Bu konuşmalar için belirteçlere ayırma işlemi, kısa çizginin önüne ve arkasına boşluk ekler, `Where is HRF - 123456?` Normal ifade, konuşmanın belirteçlere ayrılmamış ham biçimine uygulanır. Normal ifade _ham_ biçimine uygulandığından sözcük sınırlarıyla ilgilenmesi gerekmez. 


## <a name="add-findform-intent"></a>FindForm amacı ekleme

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

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

## <a name="create-an-hrf-number-regular-expression-entity"></a>HRF-numara normal ifade varlığı oluşturma 
Aşağıdaki adımları izleyerek LUIS uygulamasına HRF-numara biçimini bildiren bir normal ifade varlığı oluşturun:

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

2. Entities (Varlıklar) sayfasında **Create new entity** (Yeni varlık oluştur) düğmesini seçin. 

3. Açılan iletişim kutusuna yeni varlık adı olarak `HRF-number` girin, varlık türü olarak **RegEx** (Normal ifade) seçin, normal ifade olarak `hrf-[0-9]{6}` girin ve ardından **Done** (Bitti) öğesini seçin.

    ![Yeni varlık özelliklerinin ayarlandığı açılan iletişim kutusunun ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. **Intents** (Amaçlar) ve ardından **FindForm** varlığını seçerek konuşmalarda etiketlenmiş olan normal ifadeleri görebilirsiniz. 

    [![Konuşmayı var olan varlıkla ve normal ifade düzeniyle etiketleme işleminin ekran görüntüsü](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Varlık, makine öğrenmesi varlığı olmadığından etiket oluşturulduktan hemen sonra konuşmalara uygulanır ve LUIS web sitesinde görüntülenir.

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Adres çubuğundaki URL'nin sonuna gidip `When were HRF-123456 and hrf-234567 published in the last year?` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `FindForm` amacını `HRF-123456` ile `hrf-234567` olmak üzere iki form numarasını döndürmelidir.

    ```
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

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Bu uygulama, amacı tanımladı ve ayıklanan verileri döndürdü. 

Sohbet botunuz artık `FindForm` birincil eylemini ve aramada yer alan form numaralarını belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve form numaralarını alarak üçüncü taraf bir API ile arama gerçekleştirebilir. LUIS bu görevi gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler ve bu amaçla ilgili verileri ayıklar. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Liste varlığı hakkında bilgi edinin](luis-quickstart-intent-and-list-entity.md)

