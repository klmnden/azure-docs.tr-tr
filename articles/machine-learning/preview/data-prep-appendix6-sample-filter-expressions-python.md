---
title: "Örnek filtre ifadeleri Azure Machine Learning ile veri hazırlama olası | Microsoft Docs"
description: "Bu belge sağlayan bir Azure Machine Learning ile veri hazırlama olası filtre ifadeleri örnekleri kümesi"
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
ms.openlocfilehash: ae70f5981c7ca533740968e0eea1b0b83df414fa
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="sample-of-filter-expressions-python"></a>Filtre ifadeleri (Python) örneği 
Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Filtre ile eşdeğer testi
Yalnızca (sayısal) Col2 değeri 4'ten büyük olduğu satırları filtreleyin. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Birden çok filtre 
Filtre sadece, Col1 içerdiği değeri olan satırları **iyi** ve Col2 1 (sayısal) değeri içerir. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Null karşı test filtresi
Yalnızca Col1 null bir değere sahip olduğu bu satırları filtreleyin. 
```python
    pd.isnull(row["Col1"])
```
