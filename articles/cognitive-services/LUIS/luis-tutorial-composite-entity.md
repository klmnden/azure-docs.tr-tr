---
title: Karmaşık veri - Azure ayıklamak için bileşik bir varlık oluşturun | Microsoft Docs
description: Varlık veri farklı türlerde ayıklamak için HALUK uygulamanızda Bileşik varlık oluşturmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: 5e4d5a03db6210c159227530b94fbde6873bc211
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356117"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Karmaşık veri ayıklamak için Bileşik varlık kullanın
Bu basit uygulama iki sahip [hedefleri](luis-concept-intent.md) ve birkaç varlıklar. Amacı uçuşlar '1 biletindeki Seattle Kahire Cuma günü için' gibi kitap ve veri tek bir parçası olarak ayırma tüm özellikleri geri dönmek için ' dir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Önceden oluşturulmuş varlıklar datetimeV2 ve numarasını ekleyin
* Bileşik varlık oluştur
* HALUK sorgular ve Bileşik varlık verilerini alır

## <a name="before-you-begin"></a>Başlamadan önce
* HALUK uygulamanızdan  **[hiyerarşik Hızlı Başlangıç](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Bir abonelik zaten yoksa için kaydedebilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Bileşik varlık mantıksal bir gruplandırmasıdır 
Bul ve utterance metinde bölümlerini kategorilere ayırmak için varlık amacı budur. A [bileşik](luis-concept-entity-types.md) varlık bağlamından öğrenilen diğer varlık türlerinin oluşur. Uçuş Rezervasyonları alır bu seyahat uygulaması için çeşitli tarih, konumları ve bilgisayar lisansı sayısına gibi bilgileri parçalarını vardır. 

Bileşik oluşturulmadan önce bilgileri ayrı varlıklar olarak bulunmaktadır. Ayrı varlıklar mantıksal olarak gruplandırılabilir ve bu mantıksal gruplandırma chatbot veya diğer HALUK tüketen uygulamaya yararlıdır bileşik bir varlık oluşturun. 

Basit bir örnek utterances kullanıcıların şunları içerir:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
Bileşik varlık lisans sayısı, kaynak konumunu, hedef konumu ve tarih eşleşir. 

## <a name="what-luis-does"></a>HALUK yaptığı
Amacı ve utterance varlıklarının tanımlandığında, [ayıklanan](luis-concept-data-extraction.md#list-entity-data)ve JSON öğesinden döndürülen [endpoint](https://aka.ms/luis-endpoint-apis), HALUK yapılır. Arama uygulama veya chatbot bu JSON yanıtı alır ve isteği yerine getiren--hangi yolla yapmak için tasarlanmıştır uygulama veya chatbot. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Önceden oluşturulmuş varlıkların sayısı ve datetimeV2 ekleyin
1. Seçin `MyTravelApp` uygulamaları listesinden uygulama [HALUK] [ LUIS] Web sitesi.

2. Uygulama açıldığında seçin **varlıklar** sol gezinti bağlantısı.

    ![Varlıkları düğmesini seçin](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Seçin **önceden oluşturulmuş varlıkları Yönet**.

    ![Varlıkları düğmesini seçin](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Açılır kutusunda **numarası** ve **datetimeV2**.

    ![Varlıkları düğmesini seçin](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Ayıklanacak yeni varlıklar için sırayla seçin **tren** üst gezinti çubuğunda.

    ![Select tren düğmesi](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Bileşik varlık oluşturmak için varolan hedefi kullanın
1. Seçin **hedefleri** sol gezinti gelen. 

    ![Hedefleri sayfa seçin](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Seçin `BookFlight` gelen **hedefleri** listesi.  

    ![Listeden BookFlight hedefi seçin](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Sayısı ve datetimeV2 önceden oluşturulmuş varlıklar üzerinde utterances etiketli.

3. Utterance için `book 2 flights from seattle to cairo next monday`, mavi seçin `number` varlık, ardından **sarmalamak Bileşik varlık** listeden. Bileşik bir varlığı gösteren sağa hareket ederken sözcüklerin altında yeşil bir çizgi imleci izler. Son önceden oluşturulmuş varlık seçmek için sağa taşıyın `datetimeV2`, enter `FlightReservation` açılır pencere metin kutusuna seçip **oluştur yeni bileşik**. 

    ![Hedefleri sayfasında Bileşik varlık oluştur](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Bileşik varlık alt doğrulamak bir açılır iletişim kutusu belirir. Seçin **Bitti**.

    ![Hedefleri sayfasında Bileşik varlık oluştur](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Bileşik varlıkta varlıkları sarmalama
Bileşik varlık oluşturulduktan sonra kalan utterances bileşik varlıktaki etiketleyin. Tümcecik bileşik bir varlık olarak kaydırmak için en solundaki word seçin, ardından seçmek gereken **sarmalamak Bileşik varlık** görünen listeden sonra en sağdaki word seçin, sonra adlandırılmış Bileşik varlık seçin`FlightReservation`. Aşağıdaki adımların içine ayrıntılarıyla seçimleri, hızlı ve sorunsuz bir adımında şudur:

1. Utterance içinde `schedule 4 seats from paris to london for april 1`, 4 numara önceden oluşturulmuş bir varlık olarak seçin.

    ![En soldaki word seçin](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Seçin **sarmalamak Bileşik varlık** listeden görüntülenir.

    ![Listeden seçme kaydırma](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. En sağdaki sözcüğü seçin. Yeşil bir çizgi bileşik bir varlığı gösteren tümcecik altında görüntülenir.

    ![En sağdaki word seçin](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Select bileşik adı `FlightReservation` listeden görüntülenir.

    ![Adlandırılmış bileşik varlığı seçin](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Son utterance için Kaydır `London` ve `tomorrow` bileşik varlıkta aynı yönergeleri kullanarak. 

## <a name="train-the-luis-app"></a>Tren HALUK uygulama
Hedefleri ve varlıkları (modeli), HALUK değişiklikler hakkında bilmeniz değil, denenip ayarlanana kadar. 

1. Üst sağ tarafında HALUK Web sitesinin, seçin **tren** düğmesi.

    ![Tren uygulama](./media/luis-tutorial-composite-entity/train-button.png)

2. Yeşil durum çubuğu başarı onaylama Web sitesinin üst gördüğünüzde eğitim tamamlanır.

    ![Eğitim başarılı oldu](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Bir chatbot veya başka bir uygulama HALUK tahmin alabilmek için uygulamayı yayımlamak için gerekir. 

1. Üst sağ tarafında HALUK Web sitesinin, seçin **Yayımla** düğmesi. 

2. Üretim yuvasına seçin ve **Yayımla** düğmesi.

    ![Uygulama yayımlama](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. Yeşil durum çubuğu başarı onaylama Web sitesinin üst gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Sorgu farklı utterance noktayla
1. Üzerinde **Yayımla** sayfasında, **endpoint** sayfanın sonundaki bağlantı. Bu eylem, adres çubuğundaki uç nokta URL'si ile başka bir tarayıcı penceresi açar. 

    ![Uç nokta URL'si seçin](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. URL adresi sonuna gidin ve girin `reserve 3 seats from London to Cairo on Sunday`. Son sorgu dizesi parametresi `q`, utterance sorgu. İyi bir sınama olduğundan ve döndürmelidir bu utterance etiketli utterances hiçbirini aynı olmadığından `BookFlight` ayıklanan hiyerarşik varlık ile hedefi.

```
{
  "query": "reserve 3 seats from London to Cairo on Sunday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.227036044
    }
  ],
  "entities": [
    {
      "entity": "sunday",
      "type": "builtin.datetimeV2.date",
      "startIndex": 40,
      "endIndex": 45,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-03-25"
          },
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-04-01"
          }
        ]
      }
    },
    {
      "entity": "3 seats from london to cairo on sunday",
      "type": "flightreservation",
      "startIndex": 8,
      "endIndex": 45,
      "score": 0.6892485
    },
    {
      "entity": "cairo",
      "type": "Location::Destination",
      "startIndex": 31,
      "endIndex": 35,
      "score": 0.557570755
    },
    {
      "entity": "london",
      "type": "Location::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.8933808
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "3"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "flightreservation",
      "value": "3 seats from london to cairo on sunday",
      "children": [
        {
          "type": "builtin.datetimeV2.date",
          "value": "sunday"
        },
        {
          "type": "Location::Destination",
          "value": "cairo"
        },
        {
          "type": "builtin.number",
          "value": "3"
        },
        {
          "type": "Location::Origin",
          "value": "london"
        }
      ]
    }
  ]
}
```

Bu utterance bileşik varlıklar dahil dizisi döndürür **flightreservation** ayıklanan verilerin nesnesiyle.  

## <a name="what-has-this-luis-app-accomplished"></a>Ne bu HALUK uygulama gerçekleştirdi?
Bu uygulama yalnızca iki amaçları ve bir Bileşik varlık ile doğal dil sorgusu amacınıza tanımlanır ve ayıklanan veri döndürdü. 

Chatbot artık birincil eylemi belirlemek için yeterli bilgiye sahip `BookFlight`ve utterance bulunan ayırma bilgileri. 

## <a name="where-is-this-luis-data-used"></a>Bu HALUK verileri nerede kullanılır? 
Bu istekle HALUK yapılır. Bir chatbot gibi çağrı yapan uygulamanın topScoringIntent sonucu ve sonraki adıma yapılacak varlığın verileri alabilir. HALUK bot veya çağıran uygulama programlama çalışan yapmaz. HALUK yalnızca kullanıcının amacınıza nedir belirler. 

## <a name="next-steps"></a>Sonraki adımlar

[Varlıklar hakkında daha fazla bilgi](luis-concept-entity-types.md). 

<!--References-->
[LUIS]:luis-reference-regions.md#luis-website
[LUIS-regions]:luis-reference-regions.md#publishing-regions
