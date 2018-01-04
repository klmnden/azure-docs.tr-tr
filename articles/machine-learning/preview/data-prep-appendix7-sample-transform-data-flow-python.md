---
title: "Örnek dönüştürme veri akışını dönüşümleri Azure Machine Learning veri hazırlığı ile olası | Microsoft Docs"
description: "Bu belge dönüştürme veri akışı dönüşümler Azure Machine Learning ile veri hazırlama olası örnekleri kümesi sağlar"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 9139866b0dffd102f9b7c34835443d6337e7d39a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Özel veri akışı Dönüşümler (Python) örneği 
Menüde dönüştürme adıdır **dönüştürme veri akışı (komut)**. Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Çerçeve dönüştürme
### <a name="create-a-new-column-dynamically"></a>Yeni bir sütun dinamik olarak oluşturun 
Dinamik olarak bir sütun oluşturur (**city2**) ve mevcut Şehir sütunundan bir tane San Francisco birden çok farklı sürümlerini uzlaştırır.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Yeni toplamalar ekleme
Yeni bir çerçeve puan sütunu için hesaplanan ilk ve son toplamalar oluşturur. Bunlar göre gruplandırılır **risk_category** sütun.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize bir sütun 
Bir sütundaki aykırı değerlerini azaltmak için bir formül karşılamak için verileri yeniden formüllendiren.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Veri akışı dönüştürme
### <a name="fill-down"></a>Aşağı Doldur 
Aşağı Doldur iki dönüşümler gerektirir. Aşağıdakine benzer veri varsayılır:


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

İlk olarak, aşağıdaki kodu içeren bir sütun Ekle (komut) dönüştürme oluşturun:
```python
    row['State'] if len(row['State']) > 0 else None
```
Şimdi aşağıdaki kodu içeren bir dönüştürme veri akışı (komut) dönüştürme oluşturun:
```python
    df = df.fillna( method='pad')
```

Verileri aşağıdaki gibi görünür:

|Durum         |newState         |Şehir       |
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

