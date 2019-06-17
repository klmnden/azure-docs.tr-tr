---
title: Yöntemi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Bilgi keşfetme hizmeti (KES içinde) API değerlendir yöntemi kullanmayı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: dcfa9bb7931cf3b682bacf722b67acd6d4a370c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60814098"
---
# <a name="evaluate-method"></a>yöntemi

*Değerlendirmek* yöntemi değerlendirir ve çıkış dizini verileri temel alan bir yapılandırılmış sorgu ifadesi döndürür.

Genellikle, bir ifade yorumlama yöntemi için bir yanıt alınır.  Ancak, aynı zamanda sorgu ifadeleri kendiniz oluşturabilirsiniz (bkz [yapılandırılmış sorgu ifadesi](Expressions.md)).  

## <a name="request"></a>İstek 

`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

Ad|Değer|Açıklama
----|----|----
ifade       | Metin dizesi | Bir alt dizin varlıkların seçer yapılandırılmış sorgu ifadesi.
Öznitelikleri | Metin dizesi | Yanıta eklenecek özniteliklerin virgülle ayrılmış listesi.
count      | Sayı (varsayılan = 10) | Döndürülecek sonuç sayısı.
offset     | Sayı (varsayılan = 0) | Döndürülecek ilk sonuç dizini.
OrderBy |   Metin dizesi | İsteğe bağlı bir sıralama düzeni tarafından izlenen sonuçları sıralamak için kullanılan özniteliğin adı (varsayılan = asc): "*attrname*[: (asc&#124;desc)]".  Belirtilmezse, doğal logaritmayı olasılık azaltarak sonuçlar döndürülür.
timeout  | Sayı (varsayılan = 1000) | Milisaniye cinsinden zaman aşımı. Zaman aşımı dolmadan hesaplanan sonuçlar döndürülür.

Kullanarak *sayısı* ve *uzaklığı* parametre sonuçları çok sayıda elde edilebilir artımlı olarak birden çok istek.
  
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

Yanıt üst 2 içerir ("sayısı 2 =") büyük olasılıkla, varlıkları eşleşen.  Her varlık için başlık, yıl, yazarın adı ve Yazar Kimliği öznitelikleri döndürülür.  Veri dosyasında belirtilen şekilde nasıl birleşik yapısını öznitelik değerlerini eşleşen dikkat edin. 

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
