---
title: Bilgi Bankası araştırması hizmeti API'si CalcHistogram yönteminde | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'ndeki CalcHistogram yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 6ed694b0cc9cf41b815cc54b9f6d12adb2b7cd64
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351623"
---
# <a name="calchistogram-method"></a>calchistogram yöntemi
*Calchistogram* yöntemi bir yapılandırılmış sorgu ifade ile eşleşen nesneleri hesaplar ve öznitelik değerlerine dağıtımını hesaplar.

## <a name="request"></a>İstek
`http://<host>/calchistogram?expr=<expr>[&options]` 

Ad|Değer|Açıklama
----|-----|-----------
ifade | Metin dizesi | Histogram hesaplanacağı üzerinden dizin varlıkları belirtir yapılandırılmış sorgu ifadesi.
Öznitelikleri | Metin dizesini (varsayılan = "") | Yanıta dahil için öznitelik virgülle ayrılmış listesi.
count   | Sayı (varsayılan = 10) | Döndürülecek sonuç sayısı.
uzaklık  | Sayı (varsayılan = 0) | Döndürülecek ilk sonucu dizini.

## <a name="response-json"></a>Yanıt (JSON)
JSONPath | Açıklama
----|----
$.expr | *Expr* istek parametresi.
$.num_entities | Eşleşen varlıkları toplam sayısı.
$.histograms |  Histogram, istenen her öznitelik için bir tane dizisi.
$.histograms [\*] .attribute | Üzerinde histogram hesaplanmıştır özniteliğin adı.
$.histograms [\*] .distinct_values | Bu öznitelik için varlıklar eşleşen arasında farklı değerleri sayısı.
$.histograms [\*] .total_count | Bu öznitelik için varlıklar eşleşen arasında değer örnekleri toplam sayısı.
$.histograms [\*] .histogram | Bu öznitelik için histogram verileri.
$.histograms [\*] .histogram [\*] .value | Öznitelik değeri.
$.histograms [\*] .histogram [\*] .logprob  | Varlıkları bu öznitelik değeri ile eşleşen toplam doğal günlük olasılık.
$.histograms [\*] .histogram [\*] .count    | Bu öznitelik değeri ile eşleşen varlıkların sayısı.
$.aborted | İstek zaman aşımına uğrarsa true.

### <a name="example"></a>Örnek
Akademik yayınlar örnekte aşağıdaki histogram yayın sayıları yıl ve anahtar sözcüğü ile için belirli bir yazar bu yana 2013 hesaplar:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

Yanıt sorgu ifade ile eşleşen 37 yazıları olduğunu gösterir.  İçin *yıl* öznitelik, bir 2013 itibaren her yıl 3 farklı değerleri vardır.  Toplam kağıt 3 farklı değerleri üzerinden 37 sayısıdır.  Her *yıl*, değer, toplam doğal günlük olasılık ve varlıkları eşleşen sayısı histogram gösterir.     

İçin histogram *anahtar sözcüğü* 34 DISTINCT anahtar sözcükleri gösterir. Tez birden çok anahtar sözcük ile ilişkili olarak toplam sayısı (53) eşleşen varlıkları sayısından büyük olamaz.  Yanıt 34 farklı değerleri olsa da, yalnızca üst 4 nedeniyle içeriyor. "count = 4" parametresi.

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
