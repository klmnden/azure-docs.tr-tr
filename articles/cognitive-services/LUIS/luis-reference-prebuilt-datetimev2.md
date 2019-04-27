---
title: DatetimeV2 önceden oluşturulmuş varlıklar
titleSuffix: Azure
description: Bu makalede datetimeV2 sahip önceden oluşturulmuş varlık bilgilerini Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 6b4c3f7445d18ab1548fd63b1f4d12c5901cf949
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712840"
---
# <a name="datetimev2-prebuilt-entity-for-a-luis-app"></a>DatetimeV2 LUIS uygulaması için önceden oluşturulmuş varlık

**DatetimeV2** önceden oluşturulmuş varlık tarih ve saat değerlerini ayıklar. Bu değerler kullanılacağı istemci programları için standartlaştırılmış bir biçimde çözümleyin. Bir tarih veya saat tam olmayan bir utterance sahip olduğunda LUIS içerir _hem geçmiş hem de gelecekte değerlerini_ uç nokta yanıt. Bu varlık zaten eğitildi çünkü uygulama hedefleri için datetimeV2 içeren örnek Konuşma ekleme gerekmez. 

## <a name="types-of-datetimev2"></a>DatetimeV2 türleri
DatetimeV2 yönetilen [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-DateTime.yaml) GitHub deposu

## <a name="example-json"></a>Örnek JSON 
Aşağıdaki örnek JSON yanıtı sahip bir `datetimeV2` bir alt varlık `datetime`. Diğer datetimeV2 varlık türlerini örnekleri için bkz: [datetimeV2 öbeklerinin](#subtypes-of-datetimev2)</a>.

```json
"entities": [
  {
    "entity": "8am on may 2nd 2017",
    "type": "builtin.datetimeV2.datetime",
    "startIndex": 0,
    "endIndex": 18,
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

|Özellik adı |Özellik türü ve açıklaması|
|---|---|
|Varlık|**dize** -tarih, saat, tarih aralığı veya zaman aralığı türüne sahip utterance ayıklanan metin.|
|type|**dize** - bir, [datetimeV2 öbeklerinin](#subtypes-of-datetimev2)
|startIndex|**int** -varlık başlar utterance dizin.|
|endIndex|**int** -varlık erdiği utterance dizin.|
|çözüm|Sahip bir `values` bir, iki veya dört sahip dizi [çözümleme değerlerini](#values-of-resolution).|
|end|Bir saat veya aynı biçimdeki tarih aralığı bitiş değerini `value`. Yalnızca `type` olduğu `daterange`, `timerange`, veya `datetimerange`|

## <a name="subtypes-of-datetimev2"></a>DatetimeV2 öbeklerinin

**DatetimeV2** önceden oluşturulmuş varlığın aşağıdaki alt türleri vardır ve her örnekler, aşağıdaki tabloda verilmiştir:
* `date`
* `time`
* `daterange`
* `timerange`
* `datetimerange`
* `duration`
* `set`

## <a name="values-of-resolution"></a>Çözüm değerleri
* Tarih veya saat utterance içinde tam olarak belirtilen ve anlaşılır ise sahip bir öğe dizisi.
* DatetimeV2 değer belirsiz ise sahip iki öğe dizisi. Belirsizlik eksikliği belirli bir yıl, saat veya zaman aralığı içerir. Bkz: [belirsiz tarihleri](#ambiguous-dates) örnekler. Zaman zaman da için belirsiz veya'da, her iki değeri de dahil edilir.
* Utterance belirsizlik sahip iki öğe varsa, dizi dört öğelere sahiptir. Bu belirsizlik işlemler sırasında sahip öğeleri içerir:
  * Bir tarih veya yıl için belirsiz tarih aralığı
  * Bir saat veya seçeceğine da belirsiz bir zaman aralığı veya Pasifik Örneğin, 3:00 Nisan 3.

Her öğeyi `values` dizisi, aşağıdaki alanları olabilir: 

|Özellik adı|Özellik açıklaması|
|--|--|
|Timex|saat, tarih veya tarih aralığını izleyen TIMEX biçiminde ifade [ISO 8601 standardına](https://en.wikipedia.org/wiki/ISO_8601) ve TIMEX3 özniteliklerini TimeML dilini kullanarak ek açıklaması için. Bu ek açıklama açıklanan [TIMEX yönergeleri](http://www.timeml.org/tempeval2/tempeval2-trial/guidelines/timex3guidelines-072009.pdf).|
|type|Aşağıdaki öğelerden herhangi birini alt tür: tarih/saat, tarih, saat, daterange, timerange, datetimerange, süre, kümesi.|
|değer|**İsteğe bağlı.** Bir datetime nesnesini (tarih) (TarihSaat) ss: dd: ss (saat) yyyy:MM:dd biçimi yyyy:MM:dd içinde. Varsa `type` olduğu `duration`, değeri (süre) saniye sayısıdır. <br/> Yalnızca `type` olduğu `datetime` veya `date`, `time`, veya ' süresi.|

## <a name="valid-date-values"></a>Geçerli bir tarih değerleri

**DatetimeV2** tarihleri arasında aşağıdaki aralıklarını destekler:

| Min | Maks |
|----------|-------------|
| 1 Ocak 1900   | 31 Aralık 2099 |

## <a name="ambiguous-dates"></a>Belirsiz tarihleri

LUIS, tarihi geçmiş veya gelecekteki olabilir, her iki değer sağlar. Ay ve yıl olmadan tarih içeren bir utterance buna bir örnektir.  

Örneğin, "2 Mayıs" utterance verilen:
* 3 Mayıs 2017 bugünün tarihini ise LUIS, hem "2017-05-02" ve "2018-05-02" değerleri sağlar. 
* Bugünün tarihi 1 Mayıs 2017 olduğunda LUIS, hem "2016-05-02" ve "2017-05-02" değerleri sağlar.

Aşağıdaki örnek, "2 Mayıs" varlık çözümleme gösterir. Bu çözüm, bugünün tarihini 2 Mayıs 2017 ve 1 Mayıs 2018'e genel bakış arasında bir tarih olduğunu varsayar.
İçeren alanlar `X` içinde `timex` alan olan utterance içinde açıkça belirtilmeyen tarih kısımlarını.

```json
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

## <a name="date-range-resolution-examples-for-numeric-date"></a>Sayısal tarih için tarih aralığı çözümleme örnekleri

`datetimeV2` Varlık tarih ve saat aralığı ayıklar. `start` Ve `end` alanları, başlangıç ve bitiş aralığı belirtin. LUIS "2 Mayıs Mayıs 5 için" utterance sağlar **daterange** hem geçerli yıl hem de sonraki yıl için değerler. İçinde `timex` alan `XXXX` belirsizlik yılın değerleri gösterir. `P3D` üç gün uzun zamandır gösterir.

```json
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

## <a name="date-range-resolution-examples-for-day-of-week"></a>Tarih aralığı çözümleme örnekler için haftanın günü

Aşağıdaki örnek, LUIS nasıl kullandığını gösterir. **datetimeV2** "Perşembe için Salı" utterance çözümlenecek. Bu örnekte, geçerli 19 Haziran tarihtir. LUIS içerir **daterange** değerleri hem tarih aralıklarını koyun ve geçerli tarih izleyin.

```json
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
Saat veya zaman aralığı belirsiz ise iki saat öğe değerleri dizisi vardır. Belirsiz bir saat olduğunda, her iki saat değerlerine sahip ve Pasifik saatler.

## <a name="time-range-resolution-example"></a>Zaman aralığı çözümleme örneği

Aşağıdaki örnek, LUIS nasıl kullandığını gösterir. **datetimeV2** bir zaman aralığı olan utterance çözümlenecek.

```json
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

## <a name="deprecated-prebuilt-datetime"></a>Önceden oluşturulmuş kullanım dışı tarih/saat

`datetime` Önceden oluşturulmuş varlık kullanım dışıdır ve yerine **datetimeV2**. 

Değiştirilecek `datetime` ile `datetimeV2` LUIS uygulamanızı aşağıdaki adımları tamamlayın:

1. Açık **varlıkları** bölmesinde LUIS web arabirimi. 
2. Silme **datetime** önceden oluşturulmuş varlık.
3. Tıklayın **önceden oluşturulmuş bir varlık ekleme**
4. Seçin **datetimeV2** tıklatıp **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [boyut](luis-reference-prebuilt-dimension.md), [e-posta](luis-reference-prebuilt-email.md) varlıkları ve [numarası](luis-reference-prebuilt-number.md). 

