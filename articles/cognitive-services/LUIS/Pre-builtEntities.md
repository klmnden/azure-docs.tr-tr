---
title: Önceden oluşturulmuş varlıklarda HALUK | Microsoft Docs
description: Bu makale, dil anlama akıllı Hizmetleri (HALUK içinde) dahil edilen önceden oluşturulmuş varlıklar listesi içerir.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: 4858de8b8517016ca385e9105ca8948b66aa683b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322054"
---
# <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

HALUK bir dizi ortak tarihler, saatler, sayılar, ölçümleri ve para birimi gibi bilgi türlerini tanıma için önceden oluşturulmuş varlıkları içerir. Önceden oluşturulmuş varlık destek HALUK uygulamanızı kültür göre değişir. HALUK destekler, kültür tarafından desteği dahil olmak üzere önceden oluşturulmuş varlıklar tam listesi için bkz: [önceden oluşturulmuş varlık başvurusu](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** kullanım dışı bırakılmıştır. İle değiştirilir [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), tarih ve saat aralığı tanınmasını yanı sıra, belirsiz tarihler ve saatler geliştirilmiş tanınmasını sağlar.

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

1. Uygulamanızı adını tıklayarak açın **My uygulamaları** sayfasında ve ardından **varlıklar** sol tarafındaki. 
2. Üzerinde **varlıklar** sayfasında, **önceden oluşturulmuş varlıkları Yönet**.

    ![Varlıkları sayfası - önceden oluşturulmuş varlıkları Yönet](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. İçinde **önceden oluşturulmuş varlıkları ekleyin** iletişim kutusunda, (örneğin, "datetimeV2") eklemek istediğiniz önceden oluşturulmuş varlığı tıklatın. Daha sonra **Kaydet**'e tıklayın.

    ![Önceden oluşturulmuş varlık Ekle iletişim kutusu](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Önceden oluşturulmuş bir numara varlık kullanın
Önceden oluşturulmuş bir varlık uygulamanızda dahil edildiğinde, tahminleri yayımlanan uygulamanıza dahil edilir. Önceden oluşturulmuş varlıkların önceden eğitilmiş ve **olamaz** değiştirilmesi. Önceden oluşturulmuş bir varlık nasıl çalıştığını görmek için şu adımları izleyin:

1. Ekleme bir **numarası** varlık uygulamanıza, ardından [Train](interactive-test.md) ve [yayımlama](PublishApp.md) uygulama.
2. Uç nokta URL'sini tıklayın **uygulama yayımlama** sayfa HALUK uç noktası bir web tarayıcısında açın. 
3. Bir utterance sayısal ifade içeren URL'sini ekleyin. Örneğin, yazabilirsiniz `buy two plane ticktets`ve HALUK tanımlayan `two` olarak bir `builtin.number` varlık ve tanımlar `2` kendi değer olarak `resolution` alan. `resolution` Alanı kullanmak, istemci uygulamanız için kolay bir kurallı sayıları ve tarihleri çözümlemeye yardımcı olur. 

    ![sayı bir varlık içeren tarayıcıda utterance](./media/luis-use-prebuilt-entity/browser-query.png)

HALUK akıllıca standart olmayan formunda olmayan sayılar tanıyabilirsiniz. Sayısal ifadeler, utterances farklı denemek ve ne HALUK döndürür bakın.

Aşağıdaki örnek, "iki düzine" utterance değeri 24, çözümü içeren HALUK, JSON yanıttan gösterir.

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
**DatetimeV2** önceden oluşturulmuş varlık tarihler, saatler, tarih aralıkları ve süreler tanır. Bu adımları görmek için nasıl `datetimeV2` önceden oluşturulmuş varlık çalışır:

1. Ekleme bir **datetimeV2** uygulamanız, sonra varlığa [Train](interactive-test.md) ve [yayımlama](PublishApp.md) uygulama.
2. Uç nokta URL'sini tıklayın **uygulama yayımlama** sayfa HALUK uç noktası bir web tarayıcısında açın. 
3. Bir utterance bir tarih aralığı içeren URL'sini ekleyin. Örneğin, yazabilirsiniz `book a flight tomorrow`ve HALUK tanımlayan `tomorrow` olarak bir `builtin.datetimeV2.date` varlık ve tarihine kendi değer olarak tanımlayan `resolution` alan. 

Aşağıdaki örnekte, bugünün tarihini 31 Ekim 2017 olsaydı HALUK JSON yanıtı nasıl görünebileceği gösterilmektedir.

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
> [Önceden oluşturulmuş varlık başvurusu](./luis-reference-prebuilt-entities.md)
