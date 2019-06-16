---
title: CalcHistogram yöntemi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Bilgi keşfetme hizmeti (KES içinde) API CalcHistogram yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: aaa5b3a85c08f11d821557257de451b8ffc8a3fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60814176"
---
# <a name="calchistogram-method"></a>calchistogram yöntemi
*Calchistogram* yöntemi bir yapılandırılmış sorgu ifadesi ile eşleşen nesneleri hesaplar ve dağıtım, öznitelik değerleri hesaplar.

## <a name="request"></a>İstek
`http://<host>/calchistogram?expr=<expr>[&options]` 

Ad|Değer|Açıklama
----|-----|-----------
ifade | Metin dizesi | Dizin varlıkları histogramlar hesaplanacağı belirten yapılandırılmış sorgu ifade.
Öznitelikleri | Metin dizesi (varsayılan = "") | Yanıta dahil edilmesi için öznitelik virgülle ayrılmış listesi.
count   | Sayı (varsayılan = 10) | Döndürülecek sonuç sayısı.
offset  | Sayı (varsayılan = 0) | Döndürülecek ilk sonuç dizini.

## <a name="response-json"></a>Yanıt (JSON)
JSONPath | Açıklama
----|----
$.expr | *Expr* istek parametresi.
$.num_entities | Eşleşen varlıkların toplam sayısı.
$.histograms |  Histogramlar, istenen her öznitelik için bir dizi.
$.histograms [\*] .attribute | Histogram üzerine Hesaplandı özniteliğinin adı.
$.histograms[\*].distinct_values | Bu öznitelik için varlıklar eşleşen arasında farklı değerleri sayısı.
$.histograms [\*] .total_count | Değer örnekleri arasında bu öznitelik için varlıklar eşleşen toplam sayısı.
$.histograms[\*].histogram | Bu öznitelik için histogram verileri.
$.histograms[\*].histogram[\*].value | Öznitelik değeri.
$.histograms[\*].histogram[\*].logprob  | Varlıkları bu öznitelik değeri ile eşleşen toplam doğal logaritmayı olasılık.
$.histograms[\*].histogram[\*].count    | Bu öznitelik değeri ile eşleşen varlıkların sayısı.
$.aborted | İstek zaman aşımına uğrarsa true.

### <a name="example"></a>Örnek
Akademik yayınlar örnekte aşağıdaki anahtar sözcüğü ve year tarafından yayın sayısı Histogramı belirli bir yazar için 2013'ten beri hesaplar:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

Yanıt, sorgu ifadesi ile eşleşen 37 incelemeler olduğunu gösterir.  İçin *yıl* öznitelik, bir 2013 itibaren her yıl için 3 farklı değerleri vardır.  3 farklı değerleri üzerinde toplam kağıt 37 sayısıdır.  Her *yıl*, değer, toplam doğal logaritmayı olasılık ve eşleşen varlık sayısı histogram gösterir.     

Histogram için *anahtar sözcüğü* 34 farklı anahtar sözcükler olduğunu gösterir. Bir inceleme birden çok anahtar sözcükleri ile ilişkili olarak toplam sayısı (53) eşleşen varlıkları sayısından büyük olamaz.  Yanıt 34 farklı değerleri olsa da, yalnızca üst 4 nedeniyle içeriyor. "sayısı = 4" parametresi.

```json
{
  "expr": "And(Composite(Author.Name=='jaime teevan'),Y>=2013)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -6.894,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -6.927,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -7.082,
          "count": 10
        }
      ]
    },
    {
      "attribute": "Keyword",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -7.142,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -7.389,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -7.623,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -7.674,
          "count": 2
        }
      ]
    }
  ]
}
``` 
