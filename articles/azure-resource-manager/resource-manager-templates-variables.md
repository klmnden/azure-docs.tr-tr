---
title: Azure Resource Manager şablon değişkenleri | Microsoft Docs
description: Bildirim temelli JSON söz dizimini kullanarak bir Azure Resource Manager şablonlarında değişkenleri tanımlayın açıklar.
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
ms.date: 02/14/2019
ms.author: tomfitz
ms.openlocfilehash: 50feca90d375d6afd3b04afe019ad9f9025f19dc
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56308590"
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Değişkenler bölümünde Azure Resource Manager şablonları
Değişkenler bölümünde kullanılabilir değerler, şablonun tamamında oluşturun. Değişkenleri tanımlamanız gerekmez, ancak bunlar karmaşık ifadeleri azaltarak genellikle şablonunuzu basitleştirin.

## <a name="define-and-use-a-variable"></a>Tanımlama ve değişken kullanma

Aşağıdaki örnek, bir değişken tanımı gösterilmektedir. Bir depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri alın ve benzersiz bir dizeye bitiştirmek için birkaç şablon işlevleri kullanır.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Kaynak tanımlarken değişkeni kullanın.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

## <a name="available-definitions"></a>Kullanılabilen tanımlar

Yukarıdaki örnekte, bir değişken tanımlamak için bir yol gösterilmiştir. Şu tanımlamalardan herhangi biri kullanabilirsiniz:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    },
    "<variable-object-name>": {
        "copy": [
            {
                "name": "<name-of-array-property>",
                "count": <number-of-iterations>,
                "input": <object-or-value-to-repeat>
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": <object-or-value-to-repeat>
        }
    ]
}
```

## <a name="configuration-variables"></a>Yapılandırma değişkenleri

Karmaşık JSON türleri, bir ortam için ilgili değerleri tanımlamak için kullanabilirsiniz. 

```json
"variables": {
    "environmentSettings": {
        "test": {
            "instanceSize": "Small",
            "instanceCount": 1
        },
        "prod": {
            "instanceSize": "Large",
            "instanceCount": 4
        }
    }
},
```

Parametreleri kullanmak için yapılandırma değerlerini gösteren bir değeri oluşturun.

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
```

Geçerli ayarlarla aldığınız:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>Copy öğesinde değişken tanımında kullanın

Bir değişken birden çok örneğini oluşturmak için kullanın `copy` değişkenler bölümünde özelliği. Bir dizi değeri oluşturulan öğeleri oluşturma `input` özelliği. Kullanabileceğiniz `copy` özelliği içinde bir değişken veya en üst düzeyinde değişkenler bölümü. Kullanırken `copyIndex` içinde değişken bir yineleme, yinelemede adı sağlamanız gerekir.

Aşağıdaki örnek, kopyalama işlemi gösterilir:

```json
"variables": {
  "disk-array-on-object": {
    "copy": [
      {
        "name": "disks",
        "count": 3,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks')]"
        }
      }
    ]
  },
  "copy": [
    {
      "name": "disks-top-level-array",
      "count": 3,
      "input": {
        "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
        "diskSizeGB": "1",
        "diskIndex": "[copyIndex('disks-top-level-array')]"
      }
    },
    {
      "name": "top-level-string-array",
      "count": 5,
      "input": "[concat('myDataDisk', copyIndex('top-level-string-array', 1))]"
    }
  ]
},
```

Kopyalama ifade değerlendirildikten sonra değişkeni **nesne üzerindeki disk dizisi** şu nesne adlı bir diziyi içeren **diskleri**:

```json
{
  "disks": [
    {
      "name": "myDataDisk1",
      "diskSizeGB": "1",
      "diskIndex": 0
    },
    {
      "name": "myDataDisk2",
      "diskSizeGB": "1",
      "diskIndex": 1
    },
    {
      "name": "myDataDisk3",
      "diskSizeGB": "1",
      "diskIndex": 2
    }
  ]
}
```

Değişken **üst düzey dizi diskleri** aşağıdaki dizi içeriyor:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  }
]
```

Değişken **top-düzey-dize-dizisi** aşağıdaki dizi içeriyor:

```json
[
  "myDataDisk1",
  "myDataDisk2",
  "myDataDisk3",
  "myDataDisk4",
  "myDataDisk5"
]
```

Kopyalama kullanarak da parametre değerlerini alıp bunları kaynak değerlerine eşlemek, ihtiyacınız olduğunda çalışır. Aşağıdaki örnek, güvenlik kuralları tanımlama kullanılacak parametre değerlerini biçimlendirir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "securityRules": {
      "type": "array"
    }
  },
  "variables": {
    "copy": [
      {
        "name": "securityRules",
        "count": "[length(parameters('securityRules'))]",
        "input": {
          "name": "[parameters('securityRules')[copyIndex('securityRules')].name]",
          "properties": {
            "description": "[parameters('securityRules')[copyIndex('securityRules')].description]",
            "priority": "[parameters('securityRules')[copyIndex('securityRules')].priority]",
            "protocol": "[parameters('securityRules')[copyIndex('securityRules')].protocol]",
            "sourcePortRange": "[parameters('securityRules')[copyIndex('securityRules')].sourcePortRange]",
            "destinationPortRange": "[parameters('securityRules')[copyIndex('securityRules')].destinationPortRange]",
            "sourceAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].sourceAddressPrefix]",
            "destinationAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].destinationAddressPrefix]",
            "access": "[parameters('securityRules')[copyIndex('securityRules')].access]",
            "direction": "[parameters('securityRules')[copyIndex('securityRules')].direction]"
          }
        }
      }
    ]
  },
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "NSG1",
      "location": "[resourceGroup().location]",
      "properties": {
        "securityRules": "[variables('securityRules')]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="example-templates"></a>Örnek şablonları

Bu örnek şablon değişkenleri kullanmakla bazı senaryolar gösterilmektedir. Değişkenleri farklı senaryolarda nasıl işleneceğini test etmek için bunları dağıtın. 

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Farklı türde değişkenleri gösterir. Şablon kaynakları dağıtmaz. Bu değişken değerlerini oluşturur ve bu değerleri döndürür. |
| [yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerlerini tanımlayan bir değişken kullanımını gösterir. Şablon kaynakları dağıtmaz. Bu değişken değerlerini oluşturur ve bu değerleri döndürür. |
| [Ağ güvenlik kuralından](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubu güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen içinde şablon işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Şablonları oluşturma hakkında daha fazla öneri için bkz. [Azure Resource Manager şablonu iyi](template-best-practices.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu depolama hesaplarını veya birden fazla kaynak grubu arasında paylaşılan sanal ağlar ile çalışırken yaygın senaryodur. Daha fazla bilgi için [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).