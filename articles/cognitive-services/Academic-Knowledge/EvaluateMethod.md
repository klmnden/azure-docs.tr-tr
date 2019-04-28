---
title: Yöntemi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: Akademik varlıklara bir sorgu ifadeye göre bir dizi döndürmek için değerlendir yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: d2e628fb7fc502ef9ba81d20680d66f24fd7d138
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61339105"
---
# <a name="evaluate-method"></a>yöntemi

**Değerlendirmek** REST API, akademik varlıklara bir sorgu ifadeye göre bir dizi döndürmek için kullanılır.
<br>

**REST uç noktası:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>

## <a name="request-parameters"></a>İstek Parametreleri  

Ad     | Değer | Gerekli mi?  | Açıklama
-----------|-----------|---------|--------
**ifade**       | Metin dizesi | Evet | Sorgu ifadesinin hangi varlıkları döndürülmesi gerektiğini belirtir.
**Model**      | Metin dizesi | Hayır  | Sorgulamak istediğiniz modelin adı.  Değer şu anda, varsayılan olarak *son*.        
**Öznitelikleri** | Metin dizesi | Hayır<br>Varsayılan: Kimlik | Yanıta dahil öznitelik değerleri belirten bir virgülle ayrılmış listesi. Öznitelik adları büyük/küçük harfe duyarlıdır.
**count**        | Sayı | Hayır<br>Varsayılan: 10 | Döndürülecek sonuç sayısı.
**uzaklık**     | Sayı |   Hayır<br>Varsayılan: 0    | Döndürülecek ilk sonuç dizini.
**OrderBy** |   Metin dizesi | Hayır<br>Varsayılan: olasılık azaltarak | Varlıkları sıralama için kullanılan bir öznitelik adı. İsteğe bağlı olarak, artan/azalan belirtilebilir. Biçim: *adı: asc* veya *adı: desc*.
  
 <br>

## <a name="response-json"></a>Yanıt (JSON)

Ad | Açıklama
-------|-----   
**ifade** |  *Expr* istek parametresi.
**Varlıklar** |  0 veya sorgu ifadesi ile eşleşen daha fazla varlık dizisi. Her varlık, bir doğal logaritmayı olasılık değerini ve diğer istenen özniteliklerinin değerlerini içerir.
**İptal edildi** | İstek zaman aşımına uğrarsa true.

<br>

#### <a name="example"></a>Örnek:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Genellikle, bir ifade gelen bir yanıt elde edileceği **yorumlama** yöntemi.  Ancak, aynı zamanda sorgu ifadeleri kendiniz oluşturabilirsiniz (bkz [sorgu ifadesi söz dizimi](QueryExpressionSyntax.md)).  
  
Kullanarak *sayısı* ve *uzaklığı* parametre sonuçları çok sayıda elde edilemiyor, sonuçları bir çok büyük (ve büyük olasılıkla yavaş) yanıt tek bir istek göndermeden.  Bu örnekte, ilk yorumu ifade istek kullanılan **yorumlama** API yanıt olarak *expr* değeri. *Sayısı = 2* parametresi 2 varlık sonuçları istenen belirtir. Ve *öznitelikleri Za, Y, bilgi, AA =. AuN, AA. AuId* parametresi gösterir başlık, yıl, alıntı sayısı, yazarın adı ve Yazar Kimliği için her sonuç istenir.  Bkz: [varlık öznitelikleri](EntityAttributes.md) özniteliklerin bir listesi için.
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
