---
title: Azure Resource Manager şablon parametre bölümüne | Microsoft Docs
description: Bildirim temelli JSON söz dizimi kullanarak Azure Resource Manager şablon parametreleri bölümünde açıklanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: fd6fcff6ac556abe3b2d34c7e8b1b0290208f5b0
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53722151"
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarının parametreler bölümü
Şablon parametreleri bölümünde kaynakları dağıtırken giriş değerleri belirtin. Bu parametre değerleri (örneğin, geliştirme, test ve üretim) belirli bir ortam için uygun değerleri sağlayarak bir dağıtımı özelleştirmek etkinleştirin. Şablonunuzdaki parametrelerle sağlamanıza gerek yoktur, ancak parametre olmadan, şablonunuzu her zaman aynı adları, konumları ve özellikleri ile aynı kaynakları dağıtmak için kullanacağınız.

Şablonda 256 parametreleri ile sınırlıdır. Bu makalede gösterilen şekilde birden çok özelliği içeren nesneleri kullanarak parametre sayısını azaltabilir.

## <a name="define-and-use-a-parameter"></a>Tanımlamak ve bir parametre kullanın

Aşağıdaki örnek, bir basit parametre tanımı gösterilmektedir. Parametrenin adını tanımlar ve bir dize değeri aldığını belirtir. Parametresi yalnızca kullanım amacı için anlamlı değerleri kabul eder. Dağıtım sırasında hiçbir değer sağlandığında varsayılan bir değer belirtir. Son olarak, parametre kullanımı açıklamasını içerir. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

Şablonda, aşağıdaki söz dizimi ile parametresinin değeri başvurusu:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Kullanılabilir özellikler

Yukarıdaki örnekte parametre bölümünde kullanabileceğiniz özellikleri yalnızca bazıları gösterdi. Kullanılabilir özellikler şunlardır:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Öğe adı | Gerekli | Açıklama |
|:--- |:--- |:--- |
| parameterName |Evet |Parametrenin adı. Geçerli bir JavaScript tanımlayıcı olmalıdır. |
| type |Evet |Parametre değeri türü. İzin verilen türleri ve değerleri **dize**, **securestring**, **int**, **bool**, **nesne**, **secureObject**, ve **dizi**. |
| defaultValue |Hayır |Parametresi, parametre için hiçbir değer sağlanmışsa varsayılan değeri. |
| izin verilen değerler |Hayır |Doğru değeri sağlandığından emin olmak parametresi için izin verilen değerler dizisi. |
| minValue |Hayır |İnt türü parametreleri için en düşük değer, bu değer büyük/küçük harf dahildir. |
| maxValue |Hayır |İnt türü parametreleri için maksimum değeri, bu değeri de dahildir. |
| minLength |Hayır |Dize, güvenli dize ve dizi tür parametreleri için minimum uzunluğu, bu değer büyük/küçük harf dahildir. |
| maxLength |Hayır |Dize, güvenli dize ve dizi tür parametreleri için en fazla uzunluk, bu değer büyük/küçük harf dahildir. |
| açıklama |Hayır |Portal aracılığıyla kullanıcılara görüntülenen parametre açıklaması. |

## <a name="template-functions-with-parameters"></a>Parametrelerle şablon işlevleri

Bir parametre için varsayılan değer belirtirken, çoğu şablon işlevleri kullanabilirsiniz. Başka bir parametre değeri, varsayılan bir değer oluşturmak için kullanabilirsiniz. Aşağıdaki şablonu varsayılan değer işlevlerinde kullanımını gösterir:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Kullanamazsınız `reference` parametreleri bölümünde işlevi. Parametreleri, dağıtım öncesinde değerlendirilir böylece `reference` işlevi, bir kaynak çalışma zamanı durumu alınamıyor. 

## <a name="objects-as-parameters"></a>Parametre olarak nesne

Bunları bir nesne olarak geçirerek ilgili değerlerini düzenlemek daha kolay olabilir. Bu yaklaşım da şablon parametrelerinde sayısını azaltır.

Şablonunuzda parametreyi tanımlayın ve dağıtım sırasında bir JSON nesnesi yerine tek bir değer belirtin. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Daha sonra alt parametresinin nokta işlecini kullanarak başvuru.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>Örnek şablonları

Bu örnek şablon parametrelerini kullanarak bazı senaryolar gösterilmektedir. Farklı senaryolarda parametreleri nasıl işleneceğini test etmek için bunları dağıtın.

|Şablon  |Açıklama  |
|---------|---------|
|[Parametreler için varsayılan değerlere işlevler ile](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Parametrelerinin varsayılan değerleri tanımlarken şablon işlevleri nasıl yapılacağı açıklanır. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |
|[Parametre nesnesi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Bir nesne parametresi için kullanmayı gösterir. Şablon kaynakları dağıtmaz. Bu parametre değerlerini oluşturur ve bu değerleri döndürür. |

## <a name="next-steps"></a>Sonraki adımlar

* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Dağıtım sırasında parametre değerlerini giriş nasıl [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md). 
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
* Bir parametre nesnesi kullanma hakkında daha fazla bilgi için bkz: [bir Azure Resource Manager şablonunda bir parametre olarak bir nesne kullanmasını](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).
