---
title: Akademik bilgi API'si yönteminde değerlendirmek | Microsoft Docs
description: Microsoft Bilişsel hizmetler sorgu ifadesinde göre akademik varlık kümesi döndürmek için değerlendir yöntemini kullanın.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 3005ae1f6df042a49db086de4982d8206f6938a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351383"
---
# <a name="evaluate-method"></a>Yöntemini değerlendirin

**Değerlendirmek** REST API'si, bir sorgu ifadesi temelinde akademik varlık kümesi döndürmek için kullanılır.
<br>

**REST uç noktası:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>
## <a name="request-parameters"></a>İstek Parametreleri  
Ad     | Değer | Gerekli mi?  | Açıklama
-----------|-----------|---------|--------
**ifade**       | Metin dizesi | Evet | Hangi varlıkların belirten bir sorgu ifadesi döndürülmelidir.
**modeli**      | Metin dizesi | Hayır  | Sorgu istediğiniz modelin adı.  Değer şu anda varsayılan olarak *son*.        
**Öznitelikleri** | Metin dizesi | Hayır<br>Varsayılan: kimliği | Yanıta dahil öznitelik değerleri belirten bir virgülle ayrılmış listesi. Öznitelik adları büyük/küçük harfe duyarlıdır.
**sayısı**        | Sayı | Hayır<br>Varsayılan: 10 | Döndürülecek sonuç sayısı.
**uzaklık**     | Sayı |   Hayır<br>Varsayılan: 0    | Döndürülecek ilk sonucu dizini.
**OrderBy** |   Metin dizesi | Hayır<br>Varsayılan: olasılık azaltarak | Varlıkları sıralama için kullanılan bir özniteliğin adı. İsteğe bağlı olarak, artan/azalan belirtilebilir. Biçim: *name: asc* veya *name: desc*.
  
 <br>
## <a name="response-json"></a>Yanıt (JSON)
Ad | Açıklama
-------|-----   
**ifade** |  *Expr* istek parametresi.
**varlıklar** |  0 veya sorgu ifadesi eşleşen daha fazla varlık dizisi. Her varlığın doğal günlük olasılık değerini ve diğer istenen özniteliklerinin değerlerini içerir.
**iptal edildi** | İstek zaman aşımına uğrarsa true.

<br>
#### <a name="example"></a>Örnek:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Genellikle, bir ifade yanıt alanından elde edileceği **yorumlama** yöntemi.  Ancak, aynı zamanda sorgu ifadeleri kendiniz oluşturabilirsiniz (bkz [sorgu ifade sözdizimi](QueryExpressionSyntax.md)).  
  
Kullanarak *sayısı* ve *uzaklık* parametreleri, çok sayıda sonuç elde edilebilir, sonuçları bir çok büyük (ve büyük olasılıkla yavaş) yanıt tek bir istek göndermeden.  Bu örnekte, ilk yorumlama ifade istek kullanılan **yorumlama** API yanıt olarak *expr* değeri. *Sayısı = 2* parametresi, 2 varlık sonuçları istenen belirtir. Ve *öznitelikleri tı, Y, CC, AA =. AuN, AA. AuId* parametresi gösterir başlık, yılı, alıntı sayısını, yazar adı ve Yazar Kimliği için her bir sonucu istenir.  Bkz: [varlık öznitelikleri](EntityAttributes.md) öznitelikler listesi.
  
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
 
