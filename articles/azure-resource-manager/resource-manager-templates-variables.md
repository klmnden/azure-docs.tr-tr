---
title: Azure Resource Manager şablonu değişkenleri | Microsoft Docs
description: Bildirim temelli JSON sözdizimini kullanarak, Azure Resource Manager şablonları değişkenleri tanımlayın açıklar.
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
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: 08728a3c0b4d4578939004e2d1b1ee2d30a682ab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34359297"
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Azure Resource Manager şablonları değişkenler bölümü
Değişkenler bölümünde şablonunuzu kullanılabilir değerleri oluşturun. Değişkenleri tanımlayın gerekmez, ancak bunlar karmaşık ifadeler azaltarak genellikle şablonunuzu basitleştirin.

## <a name="define-and-use-a-variable"></a>Tanımlama ve değişken kullanma

Aşağıdaki örnekte, değişken tanımını gösterir. Depolama hesabı adı için bir dize değeri oluşturur. Bir parametre değeri almak ve benzersiz bir dize birleştirmek için birkaç şablon işlevleri kullanır.

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

## <a name="available-definitions"></a>Kullanılabilir tanımları

Önceki örnekte bir değişkeni tanımlamak için bir yol gösterilmiştir. Aşağıdaki tanımları birini kullanabilirsiniz:

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

Parametreleri kullanmak için hangi yapılandırma değerlerini gösteren bir değeri oluşturun.

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

Geçerli ayarlarla Al:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>Copy öğesi değişken tanımında kullanın

Kullanabileceğiniz **kopyalama** birden çok öğe bir dizi bir değişken oluşturmak için sözdizimi. Öğe sayısı için bir sayım sağlar. Her öğe içinde özellikler içerir **giriş** nesnesi. Kopya bir değişken içinde veya bir değişken oluşturmak için kullanabilirsiniz. Ne zaman bir değişken tanımlama ve kullanma **kopya** bu değişkenin içinde bir dizi özelliği olan bir nesneyi oluşturun. Kullandığınızda **kopyalama** en üst düzeyinde ve bir tane tanımlayacaksınız veya daha fazla değişken içindeki bir veya daha fazla diziler oluşturma. Her iki yaklaşımın aşağıdaki örnekte gösterilmiştir:

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

Değişkeni **disk dizisi üzerinde nesnesi** adlı bir dizi ile aşağıdaki nesnesini içeren **diskleri**:

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

Değişkeni **üst düzey dizi diskleri** aşağıdaki dizisi içerir:

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

Birden fazla nesne kopyalama değişkenleri oluşturmak üzere kullanılırken de belirtebilirsiniz. Aşağıdaki örnekte iki dizi değişkenleri olarak tanımlar. Bir adlandırılan **üst düzey dizi diskleri** ve beş öğesine sahip. Diğer adlı **a farklı-dizi** ve üç öğeye sahiptir.

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

İyi parametre değerlerini almak ve bir şablon değeri doğru biçimde olduklarından emin olmak gerektiğinde bu yaklaşım çalışır. Aşağıdaki örnekte güvenlik kurallarını tanımlama kullanılacak parametre değerlerini biçimlendirir:

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

## <a name="recommendations"></a>Öneriler
Değişkenleri ile çalışırken aşağıdaki bilgiler yararlı olabilir:

* Birden çok kez bir şablon kullanmak için gereken değerleri için değişkenlerini kullanın. Bir değer yalnızca bir kez kullandıysanız, sabit kodlanmış bir değer şablonunuzu okumak kolaylaştırır.
* Kullanamazsınız [başvuru](resource-group-template-functions-resource.md#reference) işlevi **değişkenleri** şablon bölümünü. **Başvuru** işlevi kaynağın çalışma zamanı durumunu değerinden türetilir. Ancak, ilk şablonunu Ayrıştırma sırasında çözümlenen değişkenlerdir. Yapı değerleri gereken **başvuru** doğrudan işlev **kaynakları** veya **çıkarır** şablon bölümünü.
* Benzersiz kaynak adları için değişkenleri içerir.

## <a name="example-templates"></a>Örnek şablonları

Bu örnek şablonları değişkenleri kullanmak için bazı senaryolar gösterilmektedir. Değişkenleri farklı senaryolarda nasıl işleneceğini test etmek için bunları dağıtın. 

|Şablon  |Açıklama  |
|---------|---------|
| [değişken tanımları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Değişkenleri farklı uygulama türleri gösterilmektedir. Şablonu herhangi bir kaynağa dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [yapılandırma değişkeni](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Yapılandırma değerleri tanımlayan bir değişken kullanımını göstermektedir. Şablonu herhangi bir kaynağa dağıtmaz. Değişken değerleri oluşturur ve bu değerleri döndürür. |
| [Ağ güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) ve [parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Bir ağ güvenlik grubu güvenlik kuralları atamak için doğru biçimde bir dizi oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar
* Farklı türlerde çözümler için tam şablonları görüntülemek üzere bkz. [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/documentation/templates/).
* Kullanabileceğiniz gelen bir şablonda işlevleri hakkında daha fazla ayrıntı için bkz: [Azure Resource Manager şablonu işlevleri](resource-group-template-functions.md).
* Birden fazla şablon dağıtımı sırasında birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Farklı bir kaynak grubu içinde mevcut kaynakları kullanmanız gerekebilir. Bu senaryo, depolama hesapları veya birden çok kaynak grupları arasında paylaşılan sanal ağlar ile çalışırken yaygındır. Daha fazla bilgi için bkz: [ResourceId işlevi](resource-group-template-functions-resource.md#resourceid).