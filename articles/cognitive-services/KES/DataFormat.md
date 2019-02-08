---
title: Veri biçimi - bilgi keşfetme hizmeti API'si
titlesuffix: Azure Cognitive Services
description: Veri biçimi, bilgi keşfetme hizmeti (KES) API'si hakkında bilgi edinin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: f17bc3c0588476fdc4f8414fb020b16718bf1d90
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854804"
---
# <a name="data-format"></a>Veri biçimi

Veri dosyası dizini oluşturmak için nesne listesini açıklar.
Dosya her satırda bir nesnenin öznitelik değerleri belirtir [JSON biçimine](http://json.org/) UTF-8 kodlamalı.
Tanımlanan öznitelikleri yanı sıra [şema](SchemaFormat.md), her nesnenin nesneleri arasındaki göreli günlük olasılık belirten isteğe bağlı "logprob" özniteliği vardır.
Hizmet olasılık azalan sırada nesneleri geri döndüğünde, biz "logprob" eşleşen nesneler dönüş sırasını belirtmek için kullanabilirsiniz.
Verilen bir olasılık *p* karşılık gelen günlük olasılık 0 ile 1 arasında günlük hesaplanabilir (*p*), doğal logaritmayı işlevi log() olduğu.
Logprob için değer belirtilmezse, varsayılan değer 0 kullanılır.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

Dize, GUID ve Blob öznitelikleri için değer tırnak işaretli bir JSON dizesi temsil edilir.  (Int32, Int64, çift) sayısal öznitelikleri için değer JSON sayı olarak temsil edilir.  Bileşik öznitelikleri için alt öznitelik değerleri belirten bir JSON nesnesi bir değerdir.  Daha hızlı dizin derlemeler, günlük olasılık azaltarak nesneleri presort.

Genel olarak, bir öznitelik 0 veya daha fazla değer olabilir.  Bir öznitelik değeri yoksa, biz yalnızca JSON'dan bırakın.  Bir öznitelik 2 veya daha fazla değer varsa, biz JSON nesnesinde öznitelik değer çifti yineleyebilirsiniz.  Alternatif olarak, biz öznitelik için birden çok değer içeren bir JSON dizisi atayabilirsiniz.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
