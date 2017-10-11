
### <a name="cacheskuname"></a>cacheSKUName
Yeni Azure Redis önbelleği fiyatlandırma katmanı.

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

Şablon (temel veya standart) Bu parametre için izin verilen değerleri tanımlar ve herhangi bir değer belirtilmezse varsayılan değer (Temel) atar. Basic yukarı kullanılabilir birden çok boyutu ile tek bir düğüm 53 GB'a sağlar.
Standart iki düğümlü birincil/çoğaltma yukarı kullanılabilir birden çok boyutu, 53 GB'a ve % 99,9 SLA sağlar.

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
Yeni Azure Redis önbelleği örneği boyutu. 

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


Şablon bu parametre (0, 1, 2, 3, 4, 5 veya 6) için izin verilen değerleri tanımlar ve herhangi bir değer belirtilmezse varsayılan değer (1) atar. Bu sayı aşağıdaki önbellek boyutlarına karşılık gelen: 0 = 250 MB, 1 = 1 GB, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB'a

