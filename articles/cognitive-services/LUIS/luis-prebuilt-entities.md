---
title: LUIS uygulamasında önceden oluşturulmuş varlıklar | Microsoft Docs
description: Bu makale, Language Understanding Intelligent Services (LUIS içinde) dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: f7abf6d8a9f0fe18017fe5c54801ac0d3b6c379e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170792"
---
# <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık destek LUIS uygulamanızı kültüre göre değişir. LUIS destekler, kültür tarafından desteği dahil olmak üzere önceden oluşturulmuş varlıkların tam listesi için bkz. [önceden oluşturulmuş bir varlık başvurusu](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** kullanım dışı bırakılmıştır. İle değiştirilir [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), tarih ve saat aralığı tanıma yanı sıra, belirsiz tarihler ve saatler geliştirilmiş tanınmasını sağlar.

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **varlıkları** sol tarafındaki. 
2. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş varlıklarla yönetme**.

    ![Varlıkları sayfası - önceden oluşturulmuş varlıkları yönetme](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. İçinde **önceden oluşturulmuş varlıklar ekleme** iletişim kutusunda, önceden oluşturulmuş (örneğin, "datetimeV2") eklemek istediğiniz varlığa tıklayın. Daha sonra **Kaydet**'e tıklayın.

    ![Önceden oluşturulmuş varlık Ekle iletişim kutusu](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Önceden oluşturulmuş bir numara varlık kullanın
Önceden oluşturulmuş bir varlık, uygulamanızda eklendiğinde, Öngörüler, yayımlanan uygulamayı dahil edilir. Önceden oluşturulmuş varlıkların önceden eğitilmiş ve **olamaz** değiştirilemiyor. Önceden oluşturulmuş bir varlık nasıl çalıştığını görmek için aşağıdaki adımları izleyin:

1. Ekleme bir **numarası** uygulamanızı, ardından bir varlığa [eğitme](luis-interactive-test.md) ve [yayımlama](luis-how-to-publish-app.md) uygulama.
2. Uç nokta URL'sini tıklayın **uygulama yayımlama** LUIS uç noktası bir web tarayıcısında açmak için sayfa. 
3. Bir utterance sayısal ifade içeren URL'sini ekleyin. Örneğin, yazabilirsiniz `buy two plane ticktets`ve LUIS tanımlayan `two` olarak bir `builtin.number` varlığı ve tanımlar `2` değeriyle `resolution` alan. `resolution` Alanı sayıları ve tarihleri için kullanılacak istemci uygulamanız için daha kolay bir kurallı biçimi çözmenize yardımcı olur. 

    ![sayı bir varlık içeren tarayıcıda utterance](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS, standart olmayan biçiminde olmayan sayılar akıllı bir şekilde tanınmasını sağlayabilir. Farklı sayısal ifadeler, konuşma denemek ve LUIS döndürür ne bakın.

Aşağıdaki örnek, çözüm için "iki düzine" utterance 24, değeri içeren bir JSON yanıtı, luıs'den gösterir.

```json
{
  "query": "order two dozen tickets for group travel",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.905443209
  },
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```
## <a name="use-a-prebuilt-datetimev2-entity"></a>Önceden oluşturulmuş datetimeV2 varlık kullanın
**DatetimeV2** önceden oluşturulmuş bir varlık, tarihler, saatler, tarih aralıkları ve süreler tanır. Görmek için bu adımları nasıl `datetimeV2` önceden oluşturulmuş varlık çalışır:

1. Ekleme bir **datetimeV2** uygulamanızı, ardından bir varlığa [eğitme](luis-interactive-test.md) ve [yayımlama](luis-how-to-publish-app.md) uygulama.
2. Uç nokta URL'sini tıklayın **uygulama yayımlama** LUIS uç noktası bir web tarayıcısında açmak için sayfa. 
3. Bir utterance bir tarih aralığı içeren URL'sini ekleyin. Örneğin, yazabilirsiniz `book a flight tomorrow`ve LUIS tanımlayan `tomorrow` olarak bir `builtin.datetimeV2.date` varlığı ve yarının tarihini değeriyle tanımlar `resolution` alan. 

Aşağıdaki örnekte, bugünün tarihini 31 Ekim 2017 olsaydı JSON yanıtı luıs'den görünebileceği gösterilmektedir.

```json
{
  "query": "book a flight tomorrow",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9063408
  },
  "entities": [
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 14,
      "endIndex": 21,
      "resolution": {
        "values": [
          {
            "timex": "2017-11-01",
            "type": "date",
            "value": "2017-11-01"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Önceden oluşturulmuş bir varlık başvurusu](./luis-reference-prebuilt-entities.md)
