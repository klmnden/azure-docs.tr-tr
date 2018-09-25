---
title: Azure Machine Learning veri hazırlama, yeni bir sütun türetmek için Python örnek | Microsoft Docs
description: Bu belge, Azure Machine Learning veri hazırlama yeni sütun oluşturmak için Python kod örnekleri sağlar.
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
ms.openlocfilehash: 43a66efde2623a848065bbde2be4b56fd6c7fc8b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969317"
---
# <a name="sample-of-custom-column-transforms-python"></a>Özel sütun Dönüşümleri (Python) örneği 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Bu dönüşüm menüde adını **Sütun Ekle (betik)**.

Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Denklik test edin ve değerleri Değiştir 
Sütun1 değeri 4'ten az ise, yeni bir sütun 1 değerine sahip olmalıdır. Sütun1 değeri 4'den fazla ise, yeni bir sütun 2 değeri vardır. 

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
## <a name="evaluate-for-nullness"></a>Nullness değerlendir 
Sütun1 null değeri içeriyorsa, ardından yeni bir sütun olarak işaretlemek **hatalı**. Aksi takdirde, olarak işaretlemek **iyi.** 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Yeni bir hesaplanmış sütun 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Dönem hesaplama 
UNIX dönem (zaten olan bir tarih varsayılıyor Col1) geçen saniye sayısının: 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Yeni bir sütun karma bir sütun değeri
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




