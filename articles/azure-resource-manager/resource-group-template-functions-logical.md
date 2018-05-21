---
title: Azure Resource Manager şablonu işlevleri - mantıksal | Microsoft Docs
description: Mantıksal değerleri belirlemek için bir Azure Resource Manager şablonu kullanmak üzere işlevleri açıklanmaktadır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2017
ms.author: tomfitz
ms.openlocfilehash: d8a7ae412fc80dff7bd91c1cdc5d4fcd985e07f4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için mantıksal işlevleri

Resource Manager şablonlarınızı içinde karşılaştırmaları yapmak için çeşitli işlevleri sağlar.

* [Ve](#and)
* [bool](#bool)
* [Eğer](#if)
* [değil](#not)
* [Veya](#or)

## <a name="and"></a>ve
`and(arg1, arg2)`

Her iki parametre değeri doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Denetlenecek ilk değer olup olmadığını doğrudur. |
| arg2 |Evet |boole |Denetlenecek ikinci değer olup olmadığını doğrudur. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** her iki değeri varsa true, aksi takdirde **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerinin nasıl kullanılacağı gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Önceki örnekte çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="bool"></a>bool
`bool(arg1)`

Parametresi bir boolean değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya int |Bir boolean değerine dönüştürmek için değer. |

### <a name="return-value"></a>Dönüş değeri
Dönüştürülen değerin bir Boole değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) bool bir string veya tamsayı ile nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| trueString | bool | True |
| falseString | bool | False |
| trueInt | bool | True |
| falseInt | bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/bool.json
```

## <a name="if"></a>Eğer
`if(condition, trueValue, falseValue)`

Bir değer olup olmadığına göre göre döndürür bir koşul true veya false.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| koşul |Evet |boole |Doğru olup olmadığını denetlemek için değer. |
| trueValue |Evet | dize, int, nesne veya dizi |Koşul doğru olduğunda döndürülecek değer. |
| false değerini |Evet | dize, int, nesne veya dizi |Koşul yanlış ise döndürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

İkinci parametre ilk parametre olduğunda döndürür **True**; Aksi halde, üçüncü parametre döndürür.

### <a name="remarks"></a>Açıklamalar

Koşullu bir kaynak özelliği ayarlamak için bu işlevi kullanabilirsiniz. Aşağıdaki örnek, tam bir şablon değildir, ancak koşullu kullanılabilirlik kümesi ayarlamak için ilgili bölümleri gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) nasıl kullanılacağını gösterir `if` işlevi.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

Önceki örnekte çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| yesOutput | Dize | evet |
| noOutput | Dize | hayır |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/if.json
```

## <a name="not"></a>değil
`not(arg1)`

Boole değeri ters değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** parametresi olduğunda **False**. Döndürür **False** parametresi olduğunda **doğru**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerinin nasıl kullanılacağı gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Önceki örnekte çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) kullanan **değil** ile [eşittir](resource-group-template-functions-comparison.md#equals).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

Önceki örnekte çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkNotEquals | bool | True |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

## <a name="or"></a>or
`or(arg1, arg2)`

Her iki parametre değeri doğru olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Denetlenecek ilk değer olup olmadığını doğrudur. |
| arg2 |Evet |boole |Denetlenecek ikinci değer olup olmadığını doğrudur. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** ya da değeri geçerliyse true, aksi takdirde **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal işlevlerinin nasıl kullanılacağı gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Önceki örnekte çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | bool | False |
| orExampleOutput | bool | True |
| notExampleOutput | bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/andornot.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

