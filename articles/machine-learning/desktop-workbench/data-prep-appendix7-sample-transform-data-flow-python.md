---
title: Örnek dönüşüm veri akışı dönüşümleri Azure Machine Learning veri hazırlama ile olası | Microsoft Docs
description: Bu belge, bir dizi dönüşüm veri akışı dönüşümleri Azure Machine Learning veri hazırlama ile olası örnekleri sağlar
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: ca780b51973a960caec3b9b7a80c8ba5621b5a0b
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651209"
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Özel veri akışı Dönüşümleri (Python) örneği 
Menü dönüşüm adıdır **dönüşüm veri akışı (betik)**. Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Çerçeve dönüştürme
### <a name="create-a-new-column-dynamically"></a>Dinamik olarak yeni bir sütun oluşturun 
Dinamik olarak bir sütun oluşturur (**şehir2**) ve birden çok farklı sürümleri arasında San Francisco mevcut city sütunundaki bir mutabık kılar.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Yeni toplamalar Ekle
Puan sütun için hesaplanan ilk ve son toplama ile yeni bir çerçeve oluşturur. Bunlar göre gruplandırılır **risk_category** sütun.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize bir sütun 
Bir sütundaki aykırı değerler azaltmak için bir formülü karşılayan verileri yeniden formüllendiren.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Veri akışını dönüştürme
### <a name="fill-down"></a>Aşağı Doldur 

Aşağı Doldur iki dönüşümler gerektirir. Bunu, aşağıdaki tabloda gibi görünen veri varsayılır:

|Durum         |Şehir       |
|--------------|-----------|
|Washington DC    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Kaliforniya    |Los Angeles|
|              |San Diego  |
|              |San Jose   |
|Texas         |Dallas     |
|              |SAN Antonio|
|              |Houston    |

1. Aşağıdaki kodu kullanarak bir "Ekle (betik) sütun" dönüştürme oluşturun:
```python
    row['State'] if len(row['State']) > 0 else None
```

2. Aşağıdaki kodu içeren bir "Dönüşüm veri akışı (betik)" dönüştürme oluşturun:
```python
    df = df.fillna( method='pad')
```

Veriler artık aşağıdaki tabloda gibi görünür:

|Durum         |Durum         |Şehir       |
|--------------|--------------|-----------|
|Washington DC    |Washington DC    |Redmond    |
|              |Washington DC    |Bellevue   |
|              |Washington DC    |Issaquah   |
|              |Washington DC    |Seattle    |
|Kaliforniya    |Kaliforniya    |Los Angeles|
|              |Kaliforniya    |San Diego  |
|              |Kaliforniya    |San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |SAN Antonio|
|              |Texas         |Houston    |


### <a name="min-max-normalization"></a>Min-Maks normalleştirme
```python
    df["NewCol"] = (df["Col1"]-df["Col1"].mean())/df["Col1"].std()
```