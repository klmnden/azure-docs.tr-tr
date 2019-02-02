---
title: Nesne algılama - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak nesne algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 93ce86a438fca47100a34da2524515b46bcad574
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55567171"
---
# <a name="object-detection"></a>Nesne algılama

Nesne algılama benzer [etiketleme](concept-tagging-images.md), ancak API, bulunan her nesne için bir sınırlama kutusu koordinatları (piksel cinsinden) döndürür. Örneğin, bir görüntü köpek, cat ve kişi içeriyorsa, Algıla işlemi görüntüde onların koordinatları ile birlikte bu nesneleri listeler. Bir görüntü içindeki nesneler arasındaki ilişkileri işlemek için bu işlevi kullanabilirsiniz. Ayrıca birden çok örneğini görüntüdeki aynı etiketi olup olmadığını belirlemenizi sağlar.

Algılama API'si nesneleri veya living şeyler görüntüde tanımlanmış temel alan etiketler uygulanır. Bu noktada unutmayın, Sınıflandırma, etiketleme ve nesne algılama için kullanılan sınıflandırma arasında biçimsel bir ilişki yoktur. Etiket API sınırlayıcı kutular yerelleştirilemez bağlamsal kullanım koşullarını "İç" gibi de içerebilir ancak kavramsal bir düzeyde algılama API'si yalnızca nesneleri ve living öğeleri bulur.

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

Önlemek veya hatalı negatif (eksik nesneler) ve sınırlı ayrıntı etkilerini azaltmak için nesne algılama özelliği sınırlamaları unutmayın.
* (Daha azını %5 görüntünün) çok küçük olmaları durumunda nesneler genellikle algılanmaz.
* Nesneler genellikle algılanmayan birbirine çok yakın yerleştirilmişse (örneğin kalıplar yığını).
* Nesneleri markaya göre ayırt edilen değil veya ürün adları (örneğin bir mağaza raf sodas farklı türde). Bir görüntüden kullanarak marka bilgilerini ancak alabilirsiniz [marka algılama](concept-brand-detection.md) özelliği.

## <a name="use-the-api"></a>API kullanın
Nesne algılama özelliği parçasıdır [analiz görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. Bu API'nin yerel SDK veya REST çağrılarını aracılığıyla çağırabilirsiniz. Yalnızca tam JSON yanıt aldığınızda içeriğini dizesi ayrıştırılamıyor. `"objects"` bölümü.

* [Hızlı Başlangıç: (.NET SDK) bir resmi çözümleme](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Hızlı Başlangıç: Bir resmi (REST API'si) çözümleme](./quickstarts/csharp-analyze.md)