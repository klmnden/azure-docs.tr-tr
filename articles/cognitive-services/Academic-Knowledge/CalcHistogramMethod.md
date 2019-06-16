---
title: CalcHistogram yöntemi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Dağıtım bir dizi kağıt varlık ait öznitelik değerleri hesaplamak için CalcHistogram yöntemini kullanın.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: a228c5b90e47c9c24c5da70484a1a28f9a3054b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60498844"
---
# <a name="calchistogram-method"></a>CalcHistogram yöntemi

**Calchistogram** REST API, bir dizi kağıt varlık ait öznitelik değerleri dağıtım hesaplamak için kullanılır.          


**REST uç noktası:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>

## <a name="request-parameters"></a>İstek Parametreleri

Ad  |Değer | Gerekli mi?  |Açıklama
-----------|----------|--------|----------
**ifade**    |Metin dizesi | Evet  |Varlıkları histogramlar hesaplanacağı belirten bir sorgu ifadesi.
**Model** |Metin dizesi | Hayır |Sorgulamak istediğiniz model adını seçin.  Değer şu anda, varsayılan olarak *son*.
**Öznitelikleri** | Metin dizesi | Hayır<br>Varsayılan: | Yanıta dahil öznitelik değerleri belirten bir virgülle ayrılmış listesi. Öznitelik adları büyük/küçük harfe duyarlıdır.
**count** |Sayı | Hayır<br>Varsayılan: 10 |Döndürülecek sonuç sayısı.
**uzaklık**  |Sayı | Hayır<br>Varsayılan: 0 |Döndürülecek ilk sonuç dizini.
**zaman aşımı**  |Sayı | Hayır<br>Varsayılan: 1000 |Milisaniye cinsinden zaman aşımı. Zaman aşımı dolmadan bulunan ınterpretations döndürülür.

## <a name="response-json"></a>Yanıt (JSON)

Ad | Açıklama
--------|---------
**ifade**  |İfade parametre istek.
**num_entities** | Eşleşen varlıkların toplam sayısı.
**histogramlar** |  Bir istekte belirtilen her bir öznitelik histogramları dizisi.
**[x] histogramlar .attribute** | Histogram üzerine Hesaplandı özniteliğinin adı.
**[x] histogramlar .distinct_values** | Bu öznitelik için varlıklar eşleşen arasında farklı değerleri sayısı.
**[x] histogramlar .total_count** | Değer örnekleri arasında bu öznitelik için varlıklar eşleşen toplam sayısı.
**[x] histogramlar .histogram** | Bu öznitelik için histogram verileri.
**[x] .histogram [y] .value histogramlar** |  Öznitelik için bir değer.
**[x] [y] .histogram .logprob histogramlar**  |Varlıkları bu öznitelik değeri ile eşleşen toplam doğal logaritmayı olasılık.
**[x] [y] .histogram .count histogramlar**  |Bu öznitelik değeri ile eşleşen varlıkların sayısı.
**İptal edildi** | İstek zaman aşımına uğrarsa true.


#### <a name="example"></a>Örnek:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Bu örnekte, bir çubuk grafik, yayın sayısı yıla göre belirli bir yazar için 2010'dan itibaren oluşturmak için biz ilk sorgu ifadesi kullanarak oluşturabilirsiniz **yorumlama** sorgu dizesi API'SİYLE: *tarafından incelemeler jaime teevan 2012 sonra*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>İfade yorumlama API döndürülen ilk yorumu içinde *ve (bileşik (AA. AuN == 'jaime teevan'), Y > 2012)* .
<br>Bu ifade değeri içinde geçirilerek **calchistogram** API. *Attributes=Y,F.FN* parametre kağıt sayıları dağıtımlarını yıl ve çalışma alanı tarafından örneğin olması gerektiğini belirtir:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Bu istek için yanıt, ilk sorgu ifadesiyle eşleşen 37 incelemeler olduğunu gösterir.  İçin *yıl* öznitelik, bir sorgu belirtilen 2012 (yani 2013, 2014 ve 2015) sonra her yıl için 3 farklı değerleri vardır.  3 farklı değerleri üzerinde toplam kağıt 37 sayısıdır.  Her *yıl*, değer, toplam doğal logaritmayı olasılık ve eşleşen varlık sayısı histogram gösterir.     

Histogram için *, alanı çalışma* 34 farklı çalışma alanları gösterir. Bir inceleme birden çok çalışma alanlarıyla ilişkili olarak toplam sayısı (53) eşleşen varlıkları sayısından büyük olamaz.  Yanıt 34 farklı değerleri olsa da, yalnızca üst 4 nedeniyle içeriyor. *sayısı = 4* parametresi.

```JSON
{
  "expr": "And(Composite(AA.AuN=='jaime teevan'),Y>2012)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -15.753,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -15.805,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -16.035,
          "count": 10
        }
      ]
    },
    {
      "attribute": "F.FN",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -15.258,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -16.002,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -16.226,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -17.228,
          "count": 2
        }
      ]
    }
  ]
}
```
