---
title: Örnek filtre ifadeleri Azure Machine Learning ile veri hazırlama olası | Microsoft Docs
description: Bu belge sağlayan bir Azure Machine Learning ile veri hazırlama olası filtre ifadeleri örnekleri kümesi
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 434da595762a077b94ff034325d5bdb156585884
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830369"
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
