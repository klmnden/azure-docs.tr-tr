---
title: Nesne algılama - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak nesne algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 7e3311aab74f58e3936039393eab76dee12b83ea
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58496947"
---
# <a name="object-detection"></a>Nesne algılama

Nesne algılama benzer [etiketleme](concept-tagging-images.md), ancak API, bulunan her nesne için bir sınırlama kutusu koordinatları (piksel cinsinden) döndürür. Örneğin, bir görüntü köpek, cat ve kişi içeriyorsa, Algıla işlemi görüntüde onların koordinatları ile birlikte bu nesneleri listeler. Bir görüntü içindeki nesneler arasındaki ilişkileri işlemek için bu işlevi kullanabilirsiniz. Ayrıca birden çok örneğini görüntüdeki aynı etiketi olup olmadığını belirlemenizi sağlar.

Algılama API'si nesneleri veya living şeyler görüntüde tanımlanmış temel alan etiketler uygulanır. Bu noktada, nesne algılama sınıflandırma ile etiketleme sınıflandırma arasındaki biçimsel bir ilişki yoktur. Etiket API sınırlayıcı kutular yerelleştirilemez bağlamsal kullanım koşullarını "İç" gibi de içerebilir ancak kavramsal bir düzeyde algılama API'si yalnızca nesneleri ve living öğeleri bulur.

## <a name="object-detection-example"></a>Nesnesi algılama örneği

Görüntü işleme örnek görüntüde nesneleri tespit edilirken döndürür aşağıdaki JSON yanıtı gösterilir.

![Microsoft Surface cihazı bir mutfakta kullanan bir kadın](./Images/windows-kitchen.jpg)

```json
{
   "objects":[
      {
         "rectangle":{
            "x":730,
            "y":66,
            "w":135,
            "h":85
         },
         "object":"kitchen appliance",
         "confidence":0.501
      },
      {
         "rectangle":{
            "x":523,
            "y":377,
            "w":185,
            "h":46
         },
         "object":"computer keyboard",
         "confidence":0.51
      },
      {
         "rectangle":{
            "x":471,
            "y":218,
            "w":289,
            "h":226
         },
         "object":"Laptop",
         "confidence":0.85,
         "parent":{
            "object":"computer",
            "confidence":0.851
         }
      },
      {
         "rectangle":{
            "x":654,
            "y":0,
            "w":584,
            "h":473
         },
         "object":"person",
         "confidence":0.855
      }
   ],
   "requestId":"a7fde8fd-cc18-4f5f-99d3-897dcd07b308",
   "metadata":{
      "width":1260,
      "height":473,
      "format":"Jpeg"
   }
}
```

## <a name="limitations"></a>Sınırlamalar

Önlemek veya hatalı negatif (eksik nesneler) ve sınırlı ayrıntı etkilerini azaltmak için nesne algılama sınırlamaları unutmayın.

* (Daha azını %5 görüntünün) küçük olursa nesneler genellikle algılanmaz.
* Nesneler genellikle algılanmayan birbirine yakın dizilmişlerdir varsa (örneğin kalıplar yığını).
* Nesneleri markaya göre ayırt edilen değil veya ürün adları (örneğin bir mağaza raf sodas farklı türde). Bir görüntüden kullanarak marka bilgilerini ancak alabilirsiniz [marka algılama](concept-brand-detection.md) özelliği.

## <a name="use-the-api"></a>API kullanın

Nesne algılama özelliği parçasıdır [analiz görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. Bu API'nin yerel SDK veya REST çağrılarını aracılığıyla çağırabilirsiniz. Dahil `Objects` içinde **visualFeatures** sorgu parametresi. Daha sonra tam JSON yanıt aldığınızda, sadece içerikleri için dizeyi ayrıştırmak `"objects"` bölümü.

* [Hızlı Başlangıç: (.NET SDK) bir resmi çözümleme](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Hızlı Başlangıç: Bir resmi (REST API'si) çözümleme](./quickstarts/csharp-analyze.md)