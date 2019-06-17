---
title: Azure Resource Manager şablonu işlevleri - karşılaştırma | Microsoft Docs
description: Değerleri karşılaştırmak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
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
ms.openlocfilehash: 6591a75577670f5bb50c1275af7e51e9b8328d50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66128676"
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Karşılaştırma işlevleri için Azure Resource Manager şablonları

Resource Manager şablonlarınızı karşılaştırmaları yapmak için çeşitli işlevler sunar.

* [eşittir](#equals)
* [daha büyük](#greater)
* [greaterOrEquals](#greaterorequals)
* [daha az](#less)
* [lessOrEquals](#lessorequals)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="equals"></a>eşittir
`equals(arg1, arg2)`

İki değerin birbirine eşit olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, string, dizi veya nesne |Eşitlik için denetleyin ilk değer. |
| arg2 |Evet |int, string, dizi veya nesne |Eşitlik için denetleyin ikinci değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** değerler, eşit; Aksi takdirde **False**.

### <a name="remarks"></a>Açıklamalar

Equals işlevi ile sık kullanılan `condition` dağıtılan bir kaynak olup olmadığını sınamak için öğesi.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/equals.json) farklı türde değerler eşitlik için denetler. Tüm varsayılan değerleri, True döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | True |
| checkArrays | Bool | True |
| checkObjects | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/equals.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/equals.json 
```

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) kullanan [değil](resource-group-template-functions-logical.md#not) ile **eşittir**.

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
}
```

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/not-equals.json 
```

## <a name="greater"></a>daha büyük
`greater(arg1, arg2)`

İlk değer ikinci değerden büyük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya dize |Büyük karşılaştırma ilk değeri. |
| arg2 |Evet |tamsayı veya dize |Büyük karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** ilk değer değerinden ikinci değeri; Aksi takdirde ise **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/greater.json) diğerinden daha büyük bir değer olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/greater.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/greater.json 
```

## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

İlk değer ikinci değerine eşit veya daha büyük olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya dize |Karşılaştırma büyük veya eşit ilk değeri. |
| arg2 |Evet |tamsayı veya dize |Büyük veya buna eşit karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** büyüktür veya eşittir ikinci değer; tersi durumda, ilk değer, **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/greaterorequals.json) diğerine eşit veya daha büyük bir değer olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkInts | Bool | False |
| checkStrings | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/greaterorequals.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/greaterorequals.json 
```

## <a name="less"></a>daha az
`less(arg1, arg2)`

İkinci değer ilk değer olup değerinden denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya dize |Daha az karşılaştırma ilk değeri. |
| arg2 |Evet |tamsayı veya dize |Daha az karşılaştırma için ikinci değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** ilk değer küçükse değerinden ikinci değeri; Aksi takdirde **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/less.json) bir değer diğerinden daha az olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/less.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/less.json 
```

## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

İlk değer ya da ikinci değer eşit olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya dize |İlk değeri küçüktür veya eşittir karşılaştırma. |
| arg2 |Evet |tamsayı veya dize |İkinci değer için küçüktür veya eşittir karşılaştırma. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** ilk değer daha veya ikinci değerine eşit; Aksi takdirde, daha az ise **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/lessorequals.json) bir değer küçüktür veya eşittir diğerine olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkInts | Bool | True |
| checkStrings | Bool | False |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/lessorequals.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/lessorequals.json 
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

