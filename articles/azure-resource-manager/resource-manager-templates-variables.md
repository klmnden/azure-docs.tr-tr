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
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: f6c629182fdcce83c566869860480d9c70488797
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53712755"
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
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": {
                <properties-to-repeat>
            }
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

Kullanabileceğiniz **kopyalama** çeşitli öğelerin bir dizisi bir değişken oluşturmak için söz dizimi. Öğe sayısı için sayısını sağlar. Her öğe içinde özellikleri içeren **giriş** nesne. İçinde bir değişken veya değişken oluşturmak için kopyalama kullanabilirsiniz. Ne zaman bir değişken tanımlayın ve kullanın **kopyalama** bu değişkenin içinde oluşturduğunuz bir dizi özelliği olan bir nesne. Kullanırken **kopyalama** en üst düzeyde ve tanımlayan bir veya daha fazla değişkenleri içindeki bir veya daha fazla dizi oluşturma. Her iki yaklaşım, aşağıdaki örnekte gösterilmiştir:

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
        }
    ]
},
```

Değişken **nesne üzerindeki disk dizisi** şu nesne adlı bir diziyi içeren **diskleri**:

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

Birden fazla nesne, kopya değişkenleri oluşturmak üzere kullanırken de belirtebilirsiniz. Aşağıdaki örnek, iki dizi değişkenleri olarak tanımlar. Bir adlandırılmış **üst düzey dizi diskleri** ve beş öğeye sahiptir. Diğer adlı **a farklı-dizi** ve üç öğe vardır.

```json
"variables": {
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 5,
            "input": {
                "name": "[concat('oneDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        },
        {
            "name": "a-different-array",
            "count": 3,
            "input": {
                "name": "[concat('twoDataDisk', copyIndex('a-different-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('a-different-array')]"
            }
        }
    ]
},
```

Bu yaklaşım da parametre değerleri alır ve bunlar için bir şablon değeri doğru biçimde olduğundan emin olun, ihtiyacınız olduğunda çalışır. Aşağıdaki örnek, güvenlik kuralları tanımlama kullanılacak parametre değerlerini biçimlendirir:

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