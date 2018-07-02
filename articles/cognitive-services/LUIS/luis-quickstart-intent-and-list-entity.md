---
title: 'Öğretici: Listelenen verilerle tam eşleşen metinleri almak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide veri ayıklama amacıyla amaçları ve liste varlıklarını kullanan basit bir LUIS uygulaması oluşturmayı öğrenerek hızlı bir başlangıç yapacaksınız.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: v-geberr
ms.openlocfilehash: 68c241833aab756bfc5e71c03da5d4175401910d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335831"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Öğretici: Liste varlığı kullanarak uygulama oluşturma
Bu öğreticide önceden tanımlanmış bir listeyle eşleşen verileri nasıl alacağınızı gösteren bir uygulama oluşturacaksınız. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Liste varlıklarını anlama 
> * MoveEmployee amacıyla İnsan Kaynakları (İK) alanında yeni bir LUIS uygulaması oluşturma
> * Konuşmadan Çalışanı ayıklamak için liste varlığı ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md#luis-website) hesabına ihtiyacınız olacak.

## <a name="before-you-begin"></a>Başlamadan önce
[Özel etki alanı](luis-quickstart-intents-regex-entity.md) öğreticisindeki normal ifade varlıklarından oluşturulan İnsan Kaynakları uygulamasına sahip değilseniz JSON verilerini [içe aktararak](create-new-app.md#import-new-app) [LUIS](luis-reference-regions.md#luis-website) web sitesinde yeni bir uygulama oluşturun. İçeri aktarmanız gereken uygulama [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-regex-HumanResources.json) Github deposunda bulunmaktadır.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `list` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="purpose-of-the-list-entity"></a>Liste varlığının amacı
Bu uygulama, bir çalışanı bir binadan farklı bir binaya taşıma hakkındaki konuşmaları tahmin eder. Bu uygulama, bir çalışanı ayıklamak için liste varlığı kullanır. Çalışandan ad, telefon numarası, e-posta adresi veya ABD sosyal güvenlik numarasıyla söz edilebilir. 

Liste varlığı birçok öğe ve her birinin eş anlamlısını barındırabilir. Küçük-orta büyüklükte bir şirkette çalışan bilgilerini ayıklamak için liste varlığı kullanılır. 

Her öğenin kurallı adı, çalışan numarasıdır. Bu etki alanı için eş anlamlı örnekleri şunlardır: 

|Eş anlamlı amacı|Eş anlamlı değeri|
|--|--|
|Adı|John W. Smith|
|E-posta adresi|john.w.smith@mycompany.com|
|Dahili telefon|x12345|
|Kişisel cep telefonu numarası|425-555-1212|
|ABD sosyal güvenlik numarası|123-45-6789|

Liste varlığı bu veri türü için iyi bir seçimdir:

* Veri değerleri bilinen bir kümedir.
* Küme, bu varlık türü için maksimum LUIS [sınırlarını](luis-boundaries.md) aşmaz.
* Konuşmadaki metin, bir eş anlamlı ile tam eşleşmedir. 

LUIS çalışanı istemci uygulamasıyla standart bir çalışanı taşıma emri oluşturacak şekilde ayıklar.
<!--
## Example utterances
Simple example utterances for a `MoveEmployee` inent:

```
move John W. Smith from B-1234 to H-4452
mv john.w.smith@mycompany from office b-1234 to office h-4452

```
-->

## <a name="add-moveemployee-intent"></a>MoveEmployee amacını ekleme

1. İnsan Kaynakları uygulamanızın LUIS sisteminin **Build** (Derleme) bölümünde olduğundan emin olun. Sağ taraftaki menü çubuğunun en üstünde bulunan **Build** (Derleme) ifadesini seçerek bu bölüme geçebilirsiniz. 

    [ ![Sağ taraftaki menü çubuğunun en üstünde bulunan Build (Derleme) ifadesi vurgulanmış LUIS uygulaması ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png)](./media/luis-quickstart-intent-and-list-entity/hr-first-image.png#lightbox)

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

    [ ![Create new intent (Yeni amaç oluştur) düğmesi vurgulanmış Intents (Amaçlar) sayfasının ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-button.png#lightbox)

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

    Bu uygulamaya önceki öğreticide eklenmiş numaralar vardır ve bu nedenle her numara etiketlenmiştir. Bu bilgiler istemci uygulamanız için yeterli olabilir ancak numara, tür ile etiketlenmiş olmayacaktır. Uygun bir adla yeni bir varlık oluşturulması, istemci uygulamasının LUIS tarafından döndürülen varlığı işlemesini sağlar.

## <a name="create-an-employee-list-entity"></a>Çalışan listesi varlığı oluşturma
**MoveEmployee** amacına konuşmalar eklendiğine göre LUIS uygulamasının çalışanın ne olduğunu anlaması gerekir. 

1. Sol panelde **Entities** (Varlıklar) öğesini seçin.

    [ ![Sol gezinti bölmesinde vurgulanmış Entities (Varlıklar) düğmesine sahip Intent (Amaç) sayfasının ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-select-entity-button.png#lightbox)

2. **Create new entity** (Yeni varlık oluştur) öğesini seçin.

    [![Create new entity (Yeni varlık oluştur) öğesi vurgulanmış Entities (Varlıklar) sayfasının ekran görüntüsü](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png) ](./media/luis-quickstart-intent-and-list-entity/hr-create-new-entity-button.png#lightbox)

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

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Uygulamayı eğitme](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı oldu](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Publish (Yayımla) düğmesinin seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Publish to production slot (Üretim yuvasına yayımla) düğmesinin seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Publish (Yayımla) sayfasındaki uç nokta URL'sinin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

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

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Doğal dil işleme, List (Liste) varlığının hangi bölümünde gerçekleştirilir? 
Liste varlığı tam metin eşleşmesi olduğundan dil işleme (veya makine öğrenmesi) kullanmaz. LUIS, doğru en yüksek puanlı amacı seçmek için doğal dil işleme (veya makine öğrenmesi) süreçlerini kullanır. Ayrıca bir konuşma birden fazla varlığın karışımı veya birden fazla varlık türü olabilir. Her konuşma doğal dil işleme (veya makine öğrenmesi) dahil olmak üzere uygulamadaki tüm varlıklar için işlenir.

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Bir liste varlığına sahip olan bu uygulama, doğru çalışanı ayıkladı. 

Sohbet botunuz artık `MoveEmployee` birincil eylemini ve taşınacak çalışanı belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hiyerarşik varlık eklemeyi öğrenin](luis-quickstart-intent-and-hier-entity.md)

