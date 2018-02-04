---
title: "Örnek hedefleri/Azure Machine Learning ile veri hazırlama olası çıktıları | Microsoft Docs"
description: "Bu belge sağlayan bir özel veri hedefleri/çıkışları Azure Machine Learning veri hazırlığı ile örnekleri kümesi"
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
ms.date: 02/01/2018
ms.openlocfilehash: 4cad3343461a6c7eda78566b3d2552b1e3591960
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="sample-of-destination-connections-python"></a>Hedef bağlantıların (Python) örneği 
Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Excel için yazma 


Excel yazılırken bir ek kitaplığı gerektirir. Yeni kitaplıkları ekleme genişletilebilirlik genel bakış bölümünde belgelenmiştir. `openpyxl`eklemek istediğiniz kitaplık değil.

Excel'e yazmadan önce başka bir değişiklik gerekli. Bazı veri hazırlık kullanılan veri türlerine bazı hedef biçimlerde desteklenmez. "Error" nesneleri varsa, örneğin, bunların doğru şekilde Excel'e seri hale olmaz. Bu nedenle, Excel'e yazma girişiminde bulunmadan önce tüm sütunlardan hataları kaldırır "Hata değerlerini değiştirmek" dönüşüm gerekir.

Tüm önceki iş ise tam, aşağıdaki satırı veri tablosu bir Excel belgesi tek bir sayfa yazar. Bir yazma veri akışı (komut) dönüştürme ekleyin. Ardından bir ifade bölümünde aşağıdaki kodu girin.


### <a name="on-windows"></a>Windows üzerinde 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>On macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
