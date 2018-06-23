---
title: Bilgi Bankası araştırması hizmeti API'si yönteminde değerlendirmek | Microsoft Docs
description: İçinde bilgi araştırması hizmet (KES) API Bilişsel Hizmetleri'ndeki değerlendir yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: fc3d73b326b565cfe40d1b82cc419357b28a801a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351664"
---
# <a name="evaluate-method"></a>Yöntemini değerlendirin
*Değerlendirmek* yöntemi değerlendirir ve dizin verileri temel alan bir yapılandırılmış sorgu ifadesi çıktısını döndürür.

Genellikle, bir ifade yorumlama yöntemine bir yanıtı alınacaktır.  Ancak, aynı zamanda sorgu ifadeleri kendiniz oluşturabilirsiniz (bkz [yapılandırılmış sorgu ifadesi](Expressions.md)).  

## <a name="request"></a>İstek 
`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

Ad|Değer|Açıklama
----|----|----
ifade       | Metin dizesi | Bir alt dizin varlıkların seçer yapılandırılmış sorgu ifadesi.
Öznitelikleri | Metin dizesi | Yanıta eklenecek öznitelikleri virgülle ayrılmış listesi.
count      | Sayı (varsayılan = 10) | Döndürülecek sonuç sayısı.
uzaklık     | Sayı (varsayılan = 0) | Döndürülecek ilk sonucu dizini.
OrderBy |   Metin dizesi | İsteğe bağlı sıralama düzeni tarafından izlenen sonuçları sıralamak için kullanılan öznitelik adı (varsayılan = asc): "*attrname*[: (asc&#124;desc)]".  Belirtilmezse, sonuçları doğal günlük olasılık azaltarak döndürülür.
timeout  | Sayı (varsayılan = 1000) | Milisaniye cinsinden zaman aşımı. Yalnızca zaman aşımı dolmadan hesaplanan sonuçlar döndürülür.

Kullanarak *sayısı* ve *uzaklık* parametreleri, çok sayıda sonuç elde edilebilir artımlı olarak birden çok istek.
  
## <a name="response-json"></a>Yanıt (JSON)
JSONPath|Açıklama
----|----
$.expr | *Expr* istek parametresi.
$.entities | 0 veya daha fazla nesne varlık yapılandırılmış sorgu ifade ile eşleşen dizisi. 
$.aborted | İstek zaman aşımına uğrarsa true.

Her bir varlık içeren bir *logprob* değeri ve istenen özniteliklerin değerleri.

## <a name="example"></a>Örnek
Akademik yayınlar örnekte aşağıdaki isteği bir yapılandırılmış sorgu ifadesi geçirir (büyük olasılıkla çıktısından bir *yorumlama* isteği) ve varlıkları eşleşen üst 2 için birkaç özniteliklerini alır:

`http://<host>/evaluate?expr=Composite(Author.Name=='jaime teevan')&attributes=Title,Y,Author.Name,Author.Id&count=2`

Yanıt üst 2 içerir ("count = 2") büyük olasılıkla, varlıkları eşleşen.  Her bir varlık için başlık, yılı, yazar adı ve yazar kimlik öznitelikleri döndürülür.  Veri dosyasında belirtilen şekilde nasıl bileşik yapısını öznitelik değerlerini eşleşen unutmayın. 

```json
{
  "expr": "Composite(Author.Name=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -6.645,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1968481722
        },
        {
          "Name": "susan t dumais",
          "Id": 676500258
        },
        {
          "Name": "eric horvitz",
          "Id": 1470530979
        }
      ]
    },
    {
      "logprob": -6.764,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1982462162
        },
        {
          "Name": "christine alvarado",
          "Id": 2163512453
        },
        {
          "Name": "mark s ackerman",
          "Id": 2055132526
        },
        {
          "Name": "david r karger",
          "Id": 2012534293
        }
      ]
    }
  ]
}
```
