---
title: Benzerlik yöntemi - akademik bilgi API'si
titlesuffix: Azure Cognitive Services
description: İki dizenin akademik benzerlik hesaplamak için benzerlik yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 76e86eb78a06d98e3d5c6c54b244add3c0c245d2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900470"
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
**S1**        |Dize   |Evet  |Karşılaştırılacak dize *
**S2**        |Dize   |Evet  |Karşılaştırılacak dize *
<sub> * Karşılaştırılacak dizeler 1 MB en uzunluğuna sahip. </sub>
<br>
## <a name="response"></a>Yanıt
Ad | Açıklama
--------|---------
**SimilarityScore**        |Bir kayan nokta Kosinüs benzerliğini s1 ve s2, 1.0 anlamı daha benzer yakın değerler ve daha az anlamı -1.0 yakın değerler gösteren değer
<br>

## <a name="successerror-conditions"></a>Başarı/hata koşulları
HTTP durumu | Neden | Yanıt
-----------|----------|--------
**200**         |Başarılı | Kayan noktalı sayı
**400**         | Hatalı istek veya istek geçersiz | Hata iletisi      
**500**         |İç sunucu hatası | Hata iletisi
**Zaman aşımına uğradı**     | İstek zaman aşımına uğradı.  | Hata iletisi
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Örnek: iki kısmi özetleri benzerliğini hesaplayın
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