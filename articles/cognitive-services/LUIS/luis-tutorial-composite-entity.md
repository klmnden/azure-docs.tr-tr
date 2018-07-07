---
title: Karmaşık verileri - Azure ayıklamak için bileşik bir varlık oluşturma | Microsoft Docs
description: LUIS uygulamanızı farklı türde varlık verilerini ayıklamak için bileşik bir varlık oluşturmayı öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: 375b52f9206f55e620d5e664844b8fa1d7249a07
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888754"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Karmaşık verileri ayıklamak için bileşik bir varlık kullanın
Bu basit uygulamaya iki içeren [hedefleri](luis-concept-intent.md) ve birkaç varlık. Amacı, '1 bilet seattle'dan Cairo Cuma' gibi uçuşlar kitap ve ayırma verileri tek bir parçası olarak tüm ayrıntılarını döndürür sağlamaktır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
* Önceden oluşturulmuş varlıklarla datetimeV2 ve numarası ekleyin
* Bileşik bir varlık oluşturma
* Sorgu LUIS ve Bileşik varlık verilerini alma

## <a name="before-you-begin"></a>Başlamadan önce
* LUIS uygulamanızı  **[hiyerarşik hızlı](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Zaten bir aboneliğiniz yoksa, kaydolabilirsiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Bileşik varlık mantıksal bir gruplandırmasıdır 
Varlığın amacı, konuşma içindeki metnin bölümlerini bulmak ve kategorilere ayırmaktır. A [bileşik](luis-concept-entity-types.md) varlık bağlamından öğrenilen diğer varlık türleri oluşur. Uçuş ayırmaları alan bu seyahat uygulaması için birkaç bölümü ele alınmakta tarihleri, konumları ve bilgisayar lisansı sayısı gibi bilgiler vardır. 

Bir bileşik oluşturulmadan önce bilgi ayrı varlıklar olarak bulunmaktadır. Mantıksal olarak ayrı varlıklar gruplandırılmasını ve bu mantıksal gruplandırma sohbet botu veya başka LUIS kullanan bir uygulama için yararlıdır, bileşik bir varlık oluşturun. 

Kullanıcıların konuşmalarına basit örnekler şunlardır:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
Bileşik varlık lisans sayısı, kaynak konumu, hedef konum ve tarih eşleşir. 

## <a name="what-luis-does"></a>LUIS tarafından gerçekleştirilen işlemler
Konuşmadaki amaç ve varlıklar tanımlandıktan, [ayıklandıktan](luis-concept-data-extraction.md#list-entity-data) ve [uç noktadan](https://aka.ms/luis-endpoint-apis) JSON biçiminde döndürüldükten sonra LUIS görevini tamamlamış olur. Arama uygulaması veya sohbet botu bu JSON yanıtını alıp tasarlanma amacına uygun olarak isteği gerçekleştirir. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Önceden oluşturulmuş varlıklarla sayısı ve datetimeV2 Ekle
1. Seçin `MyTravelApp` uygulamalar listesinden uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi.

2. Uygulama açıldığında seçin **varlıkları** sol gezinti bağlantısı.

    ![Varlıkları düğmeyi seçin](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. **Manage prebuilt entities** (Önceden oluşturulan varlıkları yönet) öğesini seçin.

    ![Varlıkları düğmeyi seçin](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Açılır kutusunda **numarası** ve **datetimeV2**.

    ![Varlıkları düğmeyi seçin](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Ayıklanacak yeni varlıklar için sırayla seçin **eğitme** üst gezinti çubuğunda.

    ![Train (Eğitim) düğmesini seçin](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Bileşik bir varlık oluşturmak için mevcut hedefi kullanma
1. Seçin **hedefleri** sol gezinti bölmesinden. 

    ![Intents sayfayı seçin](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Seçin `BookFlight` gelen **hedefleri** listesi.  

    ![Listeden BookFlight hedefi seçin](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    Sayısı ve datetimeV2 konuşma üzerinde önceden oluşturulmuş varlıklar olarak etiketlenmiş.

3. Utterance için `book 2 flights from seattle to cairo next monday`, mavi seçin `number` varlık, ardından **kaydırma bileşik varlıkta** listeden. Bileşik bir varlığı gösteren sağa hareket ettikçe sözcüklerin altında yeşil bir çizgi imleç izler. Ardından son önceden oluşturulmuş varlık seçilecek Sağa Taşı `datetimeV2`, enter `FlightReservation` açılır pencerede metin kutusuna seçip **oluştur yeni birleşik**. 

    ![Intents sayfasında bileşik bir varlık oluşturun](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Bileşik varlık alt doğrulamak bir açılır iletişim kutusu belirir. **Done** (Bitti) öğesini seçin.

    ![Intents sayfasında bileşik bir varlık oluşturun](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Varlıklar bileşik bir varlıkta Kaydır
Bileşik varlık oluşturulduktan sonra kalan konuşma bileşik varlıktaki etiketleyin. Tümcecik bileşik bir varlık olarak kaydırmak için en soldaki sözcüğünü seçin, ardından seçmek gereken **kaydırma bileşik varlıkta** görünen listeden sonra en sağdaki word seçip adlandırılmış Bileşik varlık `FlightReservation`. Bu hızlı ve kesintisiz bir adım seçimleri, aşağıdaki adımları bölünmüştür.

1. Utterance içinde `schedule 4 seats from paris to london for april 1`, 4 sayı önceden oluşturulmuş varlığı seçin.

    ![En soldaki sözcük Seç](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Seçin **kaydırma bileşik varlıkta** görünen listeden.

    ![Listeden seçim Kaydır](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. En sağdaki word seçin. Bileşik bir varlığı gösteren ifadesinin altında yeşil bir çizgi görünür.

    ![En sağdaki sözcük Seç](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Select bileşik adı `FlightReservation` görünen listeden.

    ![Adlandırılmış Bileşik varlık seçin](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Son utterance için kaydırma `London` ve `tomorrow` bileşik varlıkta, aynı yönergeleri kullanarak. 

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Uygulamayı eğitme](./media/luis-tutorial-composite-entity/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı oldu](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin.

    ![Uygulama yayımlama](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    ![Uç nokta URL'si seçin](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Adres çubuğundaki URL'nin sonuna gidip `reserve 3 seats from London to Cairo on Sunday` yazın. Son sorgu dizesi parametresi `q`, utterance sorgu. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `BookFlight` amacını hiyerarşik varlık ayıklanmış şekilde döndürmelidir.

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

Bir bileşik varlıkları dizisi dahil olmak üzere bu utterance döndürür **flightreservation** ayıklanan verilerin nesne.  

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Bu uygulama, yalnızca iki amacı ve birleşik bir varlık ile doğal dil sorgu engellemekse tanımlanır ve Ayıklanan veriler döndürdü. 

Sohbet Robotu, artık birincil eylem belirlemek için yeterli bilgiye sahip `BookFlight`, ayırma bilgilerini utterance içinde bulunamadı. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="next-steps"></a>Sonraki adımlar

[Varlıklar hakkında daha fazla bilgi](luis-concept-entity-types.md). 
