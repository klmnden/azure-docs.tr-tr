---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 04/02/2019
ms.author: wesmc
ms.openlocfilehash: 498a7ee28b9404d0733e4615f4df635a8c904b51
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188180"
---
### <a name="cacheskuname"></a>cacheSKUName

Yeni Azure Redis önbelleğine fiyatlandırma katmanı.

```json
    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard",
        "Premium"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
      }
    },
```

Şablon (temel, standart veya Premium) Bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (Temel) atar. Temel kadarını kullanılabilir birden çok boyut ile tek bir düğüm 53 GB'ye sunar. Standart kadarını kullanılabilir birden çok boyut ile iki düğümlü birincil/çoğaltma için 53 GB ve % 99,9 SLA sağlar.

### <a name="cacheskufamily"></a>cacheSKUFamily

SKU ailesi.

```json
    "cacheSKUFamily": {
      "type": "string",
      "allowedValue/s": [
        "C",
        "P"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },
```

### <a name="cacheskucapacity"></a>cacheSKUCapacity

Redis örneği için yeni Azure önbellek boyutu.

Temel ve standart aileleri için:

```json
    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }
```

Premium değeri önbelleği kapasitesi tanımlanan aynı, izin verilen değerler dışında çalıştırmayı 1 ile 5 yerine 0'dan 6.

Şablon (temel ve standart aileleri için 0 ile 6; ila 5 Premium ailesi için 1) Bu parametre için izin verilen tamsayı değerleri tanımlar. Hiçbir değer belirtilmemişse, şablonu temel ve standart, Premium 1 0 varsayılan değeri atar.

Değerleri aşağıdaki önbellek boyutlarına karşılık gelir:

| Değer | Temel ve standart<br>Önbellek boyutu | Premium<br>Önbellek boyutu |
| :---: | :------------------------------: | :-------------------: |
| 0     | 250 MB (varsayılan)                 | yok                   |
| 1     | 1 GB                             | 6 GB (varsayılan)        |
| 2     | 2,5 GB                           | 13 GB                 |
| 3     | 6 GB                             | 26 GB                 |
| 4     | 13 GB                            | 53 GB                 |
| 5     | 26 GB                            | 120 GB                |
| 6     | 53 GB                            | yok                   |
