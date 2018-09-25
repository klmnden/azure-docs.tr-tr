---
title: Azure Machine Learning veri hazırlama ile olası filtre ifadeleri örneği | Microsoft Docs
description: Bu belgede Azure Machine Learning veri hazırlama ile olası filtre ifadeleri örnekleri sunmaktadır
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
ROBOTS: NOINDEX
ms.openlocfilehash: a389007dea344de461b23b96f2942686005aa119
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982401"
---
# <a name="sample-of-filter-expressions-python"></a>Filtre ifadeleri (Python) örneği 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="filter-with-equivalence-test"></a>Denklik ile test filtre
(Sayısal) Sütun2 değerini 4'ten büyük olduğu satırlardaki filtreleyin. 

```python
    row["Col2"] > 4
```

## <a name="filter-with-multiple-columns"></a>Birden çok sütun içeren Filtrele 
Filtre yalnızca burada Col1 değeri içeriyorsa bu satırları **iyi** ve Sütun2 (sayısal) 1 değerini içerir. 
```python
    row["Col1"] == 'Good' and row["Col2"] == 1
```

## <a name="test-filter-against-null"></a>Test filtre null
Sadece Col1 null değere sahip olduğu olan satırları filtreleme. 
```python
    pd.isnull(row["Col1"])
```
