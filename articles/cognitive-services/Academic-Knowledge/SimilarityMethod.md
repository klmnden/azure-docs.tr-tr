---
title: Benzerlik yöntemi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: İki dizenin akademik benzerlik hesaplamak için benzerlik yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 7f692c08f8af322bf7e6ab576e2e6f516594a6c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61336526"
---
# <a name="similarity-method"></a>Benzerlik yöntemi

**Benzerlik** REST API, iki dizeyi arasındaki akademik benzerlik hesaplamak için kullanılır. 
<br>

**REST uç noktası:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>İstek Parametreleri

Parametre        |Veri Türü      |Gerekli | Açıklama
----------|----------|----------|------------
**s1**        |String   |Evet  |Karşılaştırılacak dize *
**s2**        |String   |Evet  |Karşılaştırılacak dize *

<sub> * En çok 1 MB Karşılaştırılacak dizeler sahip. </sub>
<br>

## <a name="response"></a>Yanıt

Ad | Açıklama
--------|---------
**SimilarityScore**        |Bir kayan nokta Kosinüs benzerliğini s1 ve s2, 1.0 anlamı daha benzer yakın değerler ve daha az anlamı -1.0 yakın değerler gösteren değer

<br>

## <a name="successerror-conditions"></a>Başarı/hata koşulları

HTTP durumu | Reason | Yanıt
-----------|----------|--------
**200**         |Başarılı | Kayan noktalı sayı
**400**         | Hatalı istek veya istek geçersiz | Hata iletisi      
**500**         |İç sunucu hatası | Hata iletisi
**Zaman aşımına uğradı**     | İstek zaman aşımına uğradı.  | Hata iletisi

<br>

## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Örnek: İki kısmi özetleri benzerliğini hesaplar
#### <a name="request"></a>İstek:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
Bu örnekte, oluşturduğumuz kullanarak iki kısmi özetleri arasında benzerlik puanı **benzerlik** API.
#### <a name="response"></a>Yanıt:
```
0.520
```
#### <a name="remarks"></a>Notlar:
Word katıştırma aracılığıyla akademik kavramları inceleyerek benzerlik puanı belirlenir. Bu örnekte, iki kısmi özetleri biraz benzerdir 0.52 anlamına gelir.
<br>
