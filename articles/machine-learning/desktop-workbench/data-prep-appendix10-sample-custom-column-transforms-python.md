---
title: Azure Machine Learning veri hazırlık yeni sütunlar türetme için Python örnek | Microsoft Docs
description: Bu belge, Azure Machine Learning veri hazırlık yeni sütun oluşturmak için Python kod örnekleri sağlar.
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
ms.openlocfilehash: babe7b8dd1ea459fd3478d20dd7d69138c9ca163
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830104"
---
# <a name="sample-of-custom-column-transforms-python"></a>Özel sütun Dönüşümler (Python) örneği 
Bu dönüştürme menüde adıdır **Sütun Ekle (komut)**.

Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Eşdeğer test ve değerleri 
Col1 değer 4'ten az ise, yeni bir sütun değeri 1 olmalıdır. Col1 değer 4'ten fazla ise, yeni bir sütun değeri 2 ' var. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Geçerli tarih ve saat 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Typecasting 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Nullness için değerlendir 
Col1 null içeriyorsa, yeni bir sütun olarak işaretlemek **hatalı**. Aksi takdirde, olarak işaretlemek **iyi.** 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Yeni bir hesaplanmış sütun 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Dönem hesaplama 
UNIX (zaten olan bir tarih olduğunu varsayarak Col1) dönem beri geçen saniye sayısı: 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Yeni bir sütun halinde karma bir sütun değeri
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




