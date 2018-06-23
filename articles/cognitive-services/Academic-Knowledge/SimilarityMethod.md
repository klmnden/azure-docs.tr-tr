---
title: Akademik bilgi API'si benzerlik yönteminde | Microsoft Docs
description: Microsoft Bilişsel hizmetler iki dizeleri akademik benzerlik hesaplamak için benzerlik yöntemi kullanın.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 472498d6bfe06ae4477a30f892d44e79c901acf5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351394"
---
# <a name="similarity-method"></a>Benzerlik yöntemi

**Benzerlik** REST API iki dizeyi arasında akademik benzerlik hesaplamak için kullanılır. 
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
<sub> * Karşılaştırmak için dizeleri 1 MB parolalarınızdan uzunluğuna sahip. </sub>
<br>
## <a name="response"></a>Yanıt
Ad | Açıklama
--------|---------
**SimilarityScore**        |Kayan nokta s1 ve 1.0 anlamı daha benzer yakın değerler ve daha az anlamı -1.0 yakın değerler ile s2 kosinüsünü benzerlik gösteren değeri
<br>

## <a name="successerror-conditions"></a>Başarı/hata koşulları
HTTP durumu | Neden | Yanıt
-----------|----------|--------
**200**         |Başarılı | Kayan noktalı sayı
**400**         | Hatalı istek veya isteği geçersiz | Hata iletisi      
**500**         |İç sunucu hatası | Hata iletisi
**Zaman aşımına uğradı**     | İstek zaman aşımına uğradı.  | Hata iletisi
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Örnek: iki kısmi özetleri benzerlik Hesapla
#### <a name="request"></a>İsteği:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
Bu örnekte, biz kullanarak iki kısmi özetleri arasında benzerlik puan oluşturmak **benzerlik** API.
#### <a name="response"></a>Yanıtı:
```
0.520
```
#### <a name="remarks"></a>Notlar:
Benzerlik puan word katıştırma aracılığıyla akademik kavramları değerlendiriliyor tarafından belirlenir. Bu örnekte, iki kısmi özetleri biraz benzer 0.52 anlamına gelir.
<br>