---
title: HALUK önceden oluşturulmuş varlıklar datetimeV2 başvuru - Azure | Microsoft Docs
titleSuffix: Azure
description: Bu makalede datetimeV2 sahip önceden oluşturulmuş varlık bilgilerini dil anlama (HALUK).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 261f6f27c39c280efdcd070888d735374a473c85
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321893"
---
# <a name="datetimev2-entity"></a>DatetimeV2 varlık

**DatetimeV2** önceden oluşturulmuş varlık tarih ve saat değerlerini ayıklar. Bu değerleri kullanmak istemci programları için standartlaştırılmış bir biçimde çözümleyin. Bir utterance bir tarih veya tam değil saat olduğunda, HALUK içeren _geçmişteki ve gelecekteki değerleri_ uç nokta yanıt. Bu varlık zaten eğitildi çünkü uygulama amaçları için datetimeV2 içeren örnek utterances eklemek gerekmez. 

## <a name="types-of-datetimev2"></a>DatetimeV2 türleri
DatetimeV2 yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) Github deposunu

## <a name="example-json"></a>Örnek JSON 
Aşağıdaki örnek JSON yanıt sahip bir `datetimeV2` alt türünün varlıkla `datetime`. Diğer datetimeV2 varlık türlerini örnekleri için bkz: [datetimeV2 Subtypes](#subtypes-of-datetimev2)</a>.

```JSON
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 15,
    "endIndex": 30,
    "resolution": {
      "values": [
        {
          "timex": "2017-05-02T08",
          "type": "datetime",
          "value": "2017-05-02 08:00:00"
        }
      ]
    }
  }
]
  ```

## <a name="json-property-descriptions"></a>JSON özellik açıklamaları

|Özellik adı |Özellik türü ve açıklama|
|---|---|
|Varlık|**dize** -tarih, saat, tarih aralığı veya zaman aralığı türüne sahip utterance ayıklanan metin.|
|type|**dize** - bir olarak [datetimeV2 subtypes](#subtypes-of-datetimev2)
|startIndex|**int** -aktarılma varlık başlar utterance dizininde.|
|endIndex|**int** -varlık erdiği utterance dizininde.|
|çözüm|Sahip bir `values` biri, iki veya dört sahip dizi [çözümleme değerlerini](#values-of-resolution).|
|end|Bir saat ya da aynı biçimde tarih aralığı bitiş değerini `value`. Yalnızca, kullanılan `type` olan `daterange`, `timerange`, veya `datetimerange`|

## <a name="subtypes-of-datetimev2"></a>DatetimeV2 subtypes

**DatetimeV2** önceden oluşturulmuş varlığı, aşağıdaki alt türleri içeriyor ve her örnekler, aşağıdaki tabloda verilmiştir:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Çözümleme değerleri
* Tarih veya saat utterance içinde tam olarak belirtilen ve anlaşılır ise dizi bir öğe vardır.
* Dizi iki öğe sahipse datetimeV2 belirsiz bir değerdir. Belirsizliği belirli yıl, saat veya saat aralığı eksikliği içerir. Bkz: [belirsiz tarihleri](#ambiguous-dates) örnekleri için. Zaman zaman A.M. olarak belirsiz veya P.M., her iki değeri de dahil edilir.
* Utterance belirsizlik sahip olan iki öğe varsa dizi dört öğeleri vardır. Bu belirsizlik sahip öğeleri içerir:
  * Bir tarih veya yıl konusunda belirsiz tarih aralığı
  * Bir saat veya A.M. konusunda belirsiz zaman aralığı veya P.M. Örneğin, 3:00 Nisan 3.

Her öğeye `values` dizi aşağıdaki alanları olabilir: 

|Özellik adı|Özellik açıklaması|
|--|--|
|Timex|saat, tarih veya tarih aralığı izleyen TIMEX biçiminde ifade [ISO 8601 standardına](https://en.wikipedia.org/wiki/ISO_8601) ve TimeML dil kullanarak ek açıklama TIMEX3 öznitelikleri. Bu ek açıklama açıklanan [TIMEX yönergeleri](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|type|Alt aşağıdaki öğelerden biri olabilir: datetime, tarih, saat, daterange, timerange, datetimerange, süre, kümesi.|
|değer|**İsteğe bağlı.** (Tarih) (datetime) ss: dd: ss: dd: (saat) yyyy:MM:dd biçimi yyyy:MM:dd datetime nesnesinde. Varsa `type` olan `duration`, değeri saniye (süresi) sayısıdır. <br/> Yalnızca, kullanılan `type` olan `datetime` veya `date`, `time`, veya ' süre.|

## <a name="valid-date-values"></a>Geçerli bir tarih değerleri

**DatetimeV2** tarihleri aşağıdaki aralıklar arasında destekler:

| Min | Maks |
|----------|-------------|
| 1 Ocak 1900   | 31 Aralık 2099 |

## <a name="ambiguous-dates"></a>Belirsiz tarihleri

Tarihi geçmiş veya gelecekteki olabiliyorsa, her iki değerin HALUK sağlar. Ay ve yıl olmadan tarih içeren bir utterance örneğidir.  

Örneğin, "May 2" utterance verilen:
* Bugünün tarihini May 3 2017 ise, HALUK "2017-05-02" ve "2018-05-02" değerler olarak sağlar. 
* Bugünün tarihini May 1 2017 olduğunda HALUK "2016-05-02" ve "2017-05-02" değerler olarak sağlar.

Aşağıdaki örnekte "may 2" varlığının çözünürlüğü gösterir. Bu çözüm, bugünün tarihini May 2 2017 ve May 1 2018 arasında bir tarih olarak varsayar.
İçeren alanlar `X` içinde `timex` alan olan utterance açıkça belirtilen olmayan tarihin bölümlerini.

```JSON
  "entities": [
    {
      "entity": "may 2nd",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2017-05-02"
          },
          {
            "timex": "XXXX-05-02",
            "type": "date",
            "value": "2018-05-02"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-numeric-date"></a>Sayısal tarih için tarih aralığını çözümleme örnekleri

`datetimeV2` Varlık tarih ve saat aralığı ayıklar. `start` Ve `end` alanları başlangıcını ve bitişini bir aralığı belirtin. "May 2 Mayıs 5 için" utterance HALUK sağlar **daterange** geçerli yıl ve bir sonraki yıl değerleri. İçinde `timex` alanı `XXXX` değerleri yılın belirsizlik gösterir. `P3D` üç günden uzun süre olduğunu gösterir.

```JSON
"entities": [
    {
      "entity": "may 2nd to may 5th",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 17,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2017-05-02",
            "end": "2017-05-05"
          },
          {
            "timex": "(XXXX-05-02,XXXX-05-05,P3D)",
            "type": "daterange",
            "start": "2018-05-02",
            "end": "2018-05-05"
          }
        ]
      }
    }
  ]
```

## <a name="date-range-resolution-examples-for-day-of-week"></a>Haftanın günü için tarih aralığını çözümleme örnekleri

Aşağıdaki örnek, HALUK nasıl kullandığını gösterir **datetimeV2** "Salı Perşembe için" utterance gidermek için. Bu örnekte, geçerli Haziran 19 tarihtir. HALUK içeren **daterange** değerlerini koyun ve geçerli tarih izleyin aralıklarını her ikisi de.

```JSON
  "entities": [
    {
      "entity": "tuesday to thursday",
      "type": "builtin.datetimeV2.daterange",
      "startIndex": 0,
      "endIndex": 19,
      "resolution": {
        "values": [
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-13",
            "end": "2017-06-15"
          },
          {
            "timex": "(XXXX-WXX-2,XXXX-WXX-4,P2D)",
            "type": "daterange",
            "start": "2017-06-20",
            "end": "2017-06-22"
          }
        ]
      }
    }
  ]
```
## <a name="ambiguous-time"></a>Belirsiz saat
Saat veya saat aralığı belirsizse iki saat öğe sahip değerleri dizisi. Belirsiz saat olduğunda, her iki A.M. değerlere sahip ve PM kez.

## <a name="time-range-resolution-example"></a>Zaman aralığı çözümleme örneği

Aşağıdaki örnek, HALUK nasıl kullandığını gösterir **datetimeV2** bir zaman aralığı olan utterance gidermek için.

```
  "entities": [
    {
      "entity": "6pm to 7pm",
      "type": "builtin.datetimeV2.timerange",
      "startIndex": 0,
      "endIndex": 9,
      "resolution": {
        "values": [
          {
            "timex": "(T18,T19,PT1H)",
            "type": "timerange",
            "start": "18:00:00",
            "end": "19:00:00"
          }
        ]
      }
    }
  ]
```

## <a name="deprecated-prebuilt-datetime"></a>Kullanım dışı önceden oluşturulmuş datetime

`datetime` Önceden oluşturulmuş varlık kullanım ve değiştirilmiştir [ `datetimeV2` ](#builtindatetimev2). 

Değiştirmek için `datetime` ile `datetimeV2` HALUK uygulamanızda aşağıdaki adımları tamamlayın:

1. Açık **varlıklar** bölmesinde HALUK web arabirimi. 
2. Silme **datetime** önceden oluşturulmuş varlık.
3. Tıklatın **önceden oluşturulmuş varlık ekleme**
4. Seçin **datetimeV2** tıklatıp **kaydetmek**.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [boyut](luis-reference-prebuilt-dimension.md), [e-posta](luis-reference-prebuilt-email.md) varlıkları ve [numarası](luis-reference-prebuilt-number.md). 

