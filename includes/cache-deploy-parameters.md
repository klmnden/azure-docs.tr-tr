---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: 1ddb81de479317a098f9de8aa5756cbaae59cb72
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331575"
---
### <a name="cacheskuname"></a>cacheSKUName
Yeni Azure Redis Cache fiyatlandırma katmanı.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Redis Cache."
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
Yeni Azure Redis Cache örneği boyutu. 

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
        "description": "The size of the new Azure Redis Cache instance. "
      }
    }


Şablon (0, 1, 2, 3, 4, 5 veya 6) Bu parametre için izin verilen değerleri tanımlar ve hiçbir değer belirtilmemişse, varsayılan değer (0) atar. Bu sayılar aşağıdaki önbellek boyutlarına karşılık gelir: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

