---
title: Akademik bilgi API'si CalcHistogram yönteminde | Microsoft Docs
description: Microsoft Bilişsel hizmetler kağıt varlıklarda bir dizi öznitelik değerlerini dağıtımını hesaplamak için CalcHistogram yöntemini kullanın.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: e0b773fb9791ee638c8cfdbbc9dca40543e50ec0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351425"
---
# <a name="calchistogram-method"></a>CalcHistogram yöntemi

**Calchistogram** REST API, öznitelik değerleri kağıt varlık kümesi için dağıtım hesaplamak için kullanılır.          


**REST uç noktası:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>
  
## <a name="request-parameters"></a>İstek Parametreleri

Ad  |Değer | Gerekli mi?  |Açıklama
-----------|----------|--------|----------
**ifade**    |Metin dizesi | Evet  |Histogram hesaplanacağı üzerinden varlıklar belirten bir sorgu ifadesi.
**modeli** |Metin dizesi | Hayır |Sorgu istediğiniz model adını seçin.  Değer şu anda varsayılan olarak *son*.
**Öznitelikleri** | Metin dizesi | Hayır<br>Varsayılan: | Yanıta dahil öznitelik değerleri belirten bir virgülle ayrılmış listesi. Öznitelik adları büyük/küçük harfe duyarlıdır.
**sayısı** |Sayı | Hayır<br>Varsayılan: 10 |Döndürülecek sonuç sayısı.
**uzaklık**  |Sayı | Hayır<br>Varsayılan: 0 |Döndürülecek ilk sonucu dizini.
<br>
## <a name="response-json"></a>Yanıt (JSON)
Ad | Açıklama
--------|---------
**ifade**  |İfade parametresi istek.
**num_entities** | Eşleşen varlıkları toplam sayısı.
**çubuk grafikler** |  Histogram, bir istekte belirtilen her bir öznitelik için bir dizi.
**Histogram [x] .attribute** | Üzerinde histogram hesaplanmıştır özniteliğin adı.
**Histogram [x] .distinct_values** | Bu öznitelik için varlıklar eşleşen arasında farklı değerleri sayısı.
**Histogram [x] .total_count** | Bu öznitelik için varlıklar eşleşen arasında değer örnekleri toplam sayısı.
**Histogram [x] .histogram** | Bu öznitelik için histogram verileri.
**[x] .histogram [y] .value çubuk grafikler** |  Özniteliği için bir değer.
**[x] .histogram [y] .logprob çubuk grafikler**  |Varlıkları bu öznitelik değeri ile eşleşen toplam doğal günlük olasılık.
**[x] .histogram [y] .count çubuk grafikler**  |Bu öznitelik değeri ile eşleşen varlıkların sayısı.
**iptal edildi** | İstek zaman aşımına uğrarsa true.

 <br>
#### <a name="example"></a>Örnek:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Bu örnekte, histogram yayınlar sayısı tarafından yıl için belirli bir yazarın 2010'dan sonra oluşturmak için size ilk sorgu ifadesi kullanarak oluşturabilirsiniz **yorumlama** API'si sorgu dizesi ile: *tarafından yazıları 2012 sonra jaime teevan*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>Yorumlama API döndürülen ilk yorumlama ifade *ve (bileşik (AA. AuN 'jaime teevan' ==), Y > 2012)*.
<br>Bu ifade değeri sonra için geçirilen **calchistogram** API. *Attributes=Y,F.FN* parametresi kağıt sayıları dağıtımları yıl ve çalışma alanı tarafından örneğin olması gerektiğini gösterir:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Bu istek için yanıt ilk sorgu ifadesi eşleşen 37 yazıları olduğunu gösterir.  İçin *yıl* öznitelik, bir sorgu belirtildiği gibi 2012 (yani 2013, 2014 ve 2015) sonra her yıl için 3 farklı değerleri vardır.  Toplam kağıt 3 farklı değerleri üzerinden 37 sayısıdır.  Her *yıl*, değer, toplam doğal günlük olasılık ve varlıkları eşleşen sayısı histogram gösterir.     

İçin histogram *, alan araştırmak* öğrenim 34 farklı alanları gösterir. Tez birden çok çalışma alanı ile ilişkili olarak toplam sayısı (53) eşleşen varlıkları sayısından büyük olamaz.  Yanıt 34 farklı değerleri olsa da, yalnızca üst 4 nedeniyle içeriyor. *sayısı = 4* parametresi.

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
