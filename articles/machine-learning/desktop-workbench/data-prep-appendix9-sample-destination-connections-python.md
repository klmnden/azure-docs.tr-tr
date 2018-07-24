---
title: Örnek hedefleri/Azure Machine Learning veri hazırlama ile olası çıktıları | Microsoft Docs
description: Bu belgede Azure Machine Learning veri hazırlama sahip özel veri hedefleri/çıkışları örnekleri sunmaktadır
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
ms.openlocfilehash: 2173ff15906940b8628aba3615f31e3f7137e3e2
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216553"
---
# <a name="sample-of-destination-connections-python"></a>(Python) hedef bağlantıları örneği 
Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Excel için yazma 


Ek bir kitaplık Excel yazılmasını gerektirir. Yeni kitaplıkları ekleme genişletilebilirlik genel bakış belgelenmiştir. `openpyxl` eklemenize gerek kitaplığıdır.

Excel'e yazmadan önce diğer bazı değişiklikler gerekiyor olabilir. Bazı hedef biçimlerde bazı veri hazırlık kullanılan veri türleri desteklenmez. "Error" nesneler varsa, örneğin, bunlar doğru Excel'e seri hale gerekmez. Bu nedenle, Excel'e yazma girişiminde bulunmadan önce hataları tüm sütunları kaldırır bir "Hata değerleri Değiştir" dönüştürmesi gerekir.

Aşağıdaki satırı veri tablosu tüm önceki iş ise tam bir Excel belgesi tek bir sayfaya yazar. Dönüşüm veri akışı (betik) dönüşüm ekleyin. Ardından bir ifade bölümünde aşağıdaki kodu girin.


### <a name="on-windows"></a>Windows üzerinde 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>MacOS/OS X üzerinde ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
