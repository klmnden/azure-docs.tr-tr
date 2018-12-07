---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: 3252a6454bf3f70250d2d792ca1f36a819ab22bf
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53020186"
---
### <a name="cacheskuname"></a>cacheSKUName
Yeni Azure Azure önbelleği için Redis fiyatlandırma katmanı.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Azure Cache for Redis."
      }
    },

Şablon (temel veya standart) Bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (Temel) atar. Temel kadarını kullanılabilir birden çok boyut ile tek bir düğüm 53 GB'ye sunar.
Standart kadarını kullanılabilir birden çok boyut ile iki düğümlü birincil/çoğaltma için 53 GB ve % 99,9 SLA sağlar.

### <a name="cacheskufamily"></a>cacheSKUFamily
SKU ailesi.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
Redis örneği için yeni Azure Azure önbellek boyutu. 

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
        "description": "The size of the new Azure Azure Cache for Redis instance. "
      }
    }


Şablon (0, 1, 2, 3, 4, 5 veya 6) Bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (0) atar. Bu sayılar aşağıdaki önbellek boyutlarına karşılık gelir: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

