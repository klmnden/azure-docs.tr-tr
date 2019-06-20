---
title: Azure Resource Manager şablonu işlevleri - diziler ve nesneleri | Microsoft Docs
description: Nesneler ve diziler ile çalışmak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
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
ms.date: 11/8/2018
ms.author: tomfitz
ms.openlocfilehash: c80625fb36709f66319b4966e210785864f30d09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66128701"
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Dizi ve nesne işlevleri için Azure Resource Manager şablonları

Resource Manager, nesneleri ve dizileri ile çalışmak için çeşitli işlevler sunar.

* [Dizi](#array)
* [birleşim](#coalesce)
* [concat](#concat)
* [içerir](#contains)
* [createArray](#createarray)
* [boş](#empty)
* [ilk](#first)
* [kesişimi](#intersection)
* [json](#json)
* [Son](#last)
* [Uzunluğu](#length)
* [en fazla](#max)
* [Min](#min)
* [Aralığı](#range)
* [Atla](#skip)
* [sınav zamanı](#take)
* [birleşim](#union)

Bir dizi değere göre ayrılmış dize değerlerini almak için bkz. [bölme](resource-group-template-functions-string.md#split).

<a id="array" />

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="array"></a>array
`array(convertToArray)`

Değeri bir diziye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| convertToArray |Evet |int, string, array veya object |Bir diziye dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/array.json) dizi işlevinin farklı türleri ile nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "efgh"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| intOutput | Dizi | [1] |
| stringOutput | Dizi | ["efgh"] |
| objectOutput | Dizi | [{"a": "b", "c": "d"}] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

<a id="coalesce" />

## <a name="coalesce"></a>birleşim
`coalesce(arg1, arg2, arg3, ...)`

Parametreler null olmayan ilk değeri döndürür. Boş dizeler, boş diziler ve boş nesneler boş değildir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, string, dizi veya nesne |Null için test edilecek ilk değeri. |
| Ek bağımsız değişkenleri |Hayır |int, string, dizi veya nesne |Null test etmek için ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Dize, int, dizi veya nesne olabilir ilk null olmayan parametre değeri. Tüm parametreleri null ise null. 

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/coalesce.json) birleşim farklı kullanımlarını çıktısını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | String | default |
| intOutput | Int | 1 |
| objectOutput | Object | {"first": "varsayılan"} |
| arrayOutput | Dizi | [1] |
| emptyOutput | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

<a id="concat" />

## <a name="concat"></a>concat
`concat(arg1, arg2, arg3, ...)`

Birden çok dizisi birleştirir ve birleştirilmiş bir dizi döndürür veya birden çok dize değerleri birleştirir ve birleştirilmiş dizeyi döndürür. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk dizi veya dize birleştirme için. |
| Ek bağımsız değişkenler |Hayır |dizi veya dize |Ek diziler veya sıralı birleştirme için dizeleri. |

Bu işlev, herhangi bir sayıda bağımsız değişken alabilir ve dizeler veya diziler için parametreleri kabul edebilir.

### <a name="return-value"></a>Dönüş değeri
Bir dize veya birleştirilmiş değerleri dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) iki diziyi birleştirme işlemi gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| döndürülecek | Dizi | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) iki dize değerlerini birleştirmek ve birleştirilmiş bir dize döndürecek gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| concatOutput | String | önek 5yj4yjf5mbg72 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

<a id="contains" />

## <a name="contains"></a>içerir
`contains(container, itemToFind)`

Bir dizinin değer içermesi, bir nesne içeren bir anahtar veya bir alt dizenin bir dize içeren olup olmadığını denetler. Dize karşılaştırma büyük/küçük harf duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa test ederken, karşılaştırma büyük küçük harfe duyarlıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| container |Evet |dizi, nesne veya dize |Bulunacak değer içeren kayıt değeri. |
| itemToFind |Evet |dize veya tamsayı |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

**Doğru** öğe bulunduysa, **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) içeren farklı türleri ile nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | Bool | True |
| stringFalse | Bool | False |
| objectTrue | Bool | True |
| objectFalse | Bool | False |
| arrayTrue | Bool | True |
| arrayFalse | Bool | False |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

Parametrelerden bir dizi oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |Dize, tamsayı, dizi veya nesne |Dizideki ilk değer. |
| Ek bağımsız değişkenler |Hayır |Dize, tamsayı, dizi veya nesne |Dizide ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/createarray.json) createArray farklı türleriyle kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringArray | Dizi | ["a", "b", "c"] |
| int | Dizi | [1, 2, 3] |
| objectArray | Dizi | [{"bir": "a", "iki": "b", "üç": "c"}] |
| arrayArray | Dizi | ["[bir", "iki", "üç"]] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

<a id="empty" />

## <a name="empty"></a>boş

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| itemToTest |Evet |dizi, nesne veya dize |Boş olup olmadığı denetlenecek değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** değer boş; Aksi takdirde ise **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) bir dizi, nesne ve dize boş olup olmadığını denetler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayEmpty | Bool | True |
| objectEmpty | Bool | True |
| stringEmpty | Bool | True |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

<a id="first" />

## <a name="first"></a>ilk
`first(arg1)`

Dizinin ilk öğesi ya da dizenin ilk karakteri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

(String, int, dizi veya nesne) ilk öğe türü bir dizi ya da bir dizenin ilk karakteri.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) ilk işlev bir dizi ve dize ile nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | String | bir |
| stringOutput | String | O |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

<a id="intersection" />

## <a name="intersection"></a>kesişimi
`intersection(arg1, arg2, arg3, ...)`

Parametreler, tek bir dizi veya nesne ortak öğeleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ilk değeri. |
| arg2 |Evet |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ikinci değer. |
| Ek bağımsız değişkenler |Hayır |dizi veya nesne |Ortak öğeleri bulmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya nesne ile ortak öğeler.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/intersection.json) nesneleri ve dizileri ile kesişimidir kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Object | {"bir": "a", "üç": "c"} |
| arrayOutput | Dizi | ["iki", "üç"] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

## <a name="json"></a>json
`json(arg1)`

Bir JSON nesnesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |string |JSON için dönüştürülecek değer. |


### <a name="return-value"></a>Dönüş değeri

JSON nesnesinde belirtilen dize veya boş bir nesneyi zaman **null** belirtilir.

### <a name="remarks"></a>Açıklamalar

JSON nesnesi, bir parametre veya değişken eklemeniz kullanırsanız [concat](resource-group-template-functions-string.md#concat) işlevine geçirebileceğimiz dizesi oluşturmak için işlevi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) json işlev nesneleri ve dizileri ile kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testValue": {
            "type": "string",
            "defaultValue": "demo value"
        }
    },
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        },
        "paramOutput": {
            "type": "object",
            "value": "[json(concat('{\"a\": \"', parameters('testValue'), '\"}'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| jsonOutput | Object | {"a": "b"} |
| nullOutput | Boolean | True |
| paramOutput | Object | {"a": "tanıtım değeri"}

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

<a id="last" />

## <a name="last"></a>Son
`last (arg1)`

Dizinin son öğesi ya da dizenin son karakteri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Son öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

Türünü (dize, int, dizi veya nesne) son öğeden bir dizi ya da bir dizenin son karakter.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) son işlevi bir dizi ve dize ile nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | String | üç |
| stringOutput | String | E |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

<a id="length" />

## <a name="length"></a>length
`length(arg1)`

Bir dizi ya da bir dizedeki karakter öğelerin sayısını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Karakter sayısını almak için kullanılacak öğeler veya dize sayısını almak için kullanılacak dizisi. |

### <a name="return-value"></a>Dönüş değeri

Tamsayı 

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) uzunluğu bir dizi ve dize ile nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayLength | Int | 3 |
| stringLength | Int | 13 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

Kaynakları oluşturulurken yineleme sayısını belirtmek için bir dizi ile bu işlevi kullanabilirsiniz. Aşağıdaki örnekte, parametre **siteNames** için web siteleri oluştururken kullanmak için bir ad dizisini başvurur.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Bu işlev bir dizi ile kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).

<a id="max" />

## <a name="max"></a>en fazla
`max(arg1)`

En yüksek değer bir sayı dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya virgülle ayrılmış tamsayı listesi dizisi |En yüksek değeri elde etmek için koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En büyük değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) en büyük tamsayı listesi ve bir dizi ile kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>dk
`min(arg1)`

En düşük değer bir sayı dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya virgülle ayrılmış tamsayı listesi dizisi |Minimum değeri alma koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En düşük değeri temsil eden bir int.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) en az bir dizi ve bir tamsayı listesi ile nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="range" />

## <a name="range"></a>Aralığı
`range(startingInteger, numberOfElements)`

Bir tamsayı başlatma ve birçok öğe içeren tamsayı dizisi oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| startingInteger |Evet |int |Dizideki ilk tamsayı. |
| numberofElements |Evet |int |Dizideki tamsayıların sayısıdır. |

### <a name="return-value"></a>Dönüş değeri

Tamsayı dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/range.json) aralığı işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| rangeOutput | Dizi | [5, 6, 7] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

<a id="skip" />

## <a name="skip"></a>Atla
`skip(originalValue, numberToSkip)`

Sonra belirtilen sayı dizisindeki tüm öğeleri olan bir dizi döndürür veya belirtilen dizede sonra tüm karakterleri içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya atlama için kullanılacak dize. |
| numberToSkip |Evet |int |Öğeleri veya atlamak için karakter sayısı. Bu değer 0 veya daha az ise, tüm öğeleri veya bulunan değerin karakter döndürülür. Dizi veya dize uzunluğundan daha büyük ise, boş bir dizi veya dize döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) dizideki öğelerin belirtilen sayısını ve belirtilen bir dizedeki karakter sayısını atlar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["üç"] |
| stringOutput | String | iki üç |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

<a id="take" />

## <a name="take"></a>sınav zamanı
`take(originalValue, numberToTake)`

Dizi ya da bir karakter dizesi başından itibaren belirtilen sayıda dize başından itibaren belirtilen sayıda öğeyi içeren bir dizi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya dizedeki öğeleri gerçekleştirilecek. |
| numberToTake |Evet |int |Öğeleri veya gerçekleştirilecek karakter sayısı. Bu değer 0 veya daha az ise, boş bir dizi veya dize döndürülür. Belirli bir dizi veya dize uzunluğundan daha büyük ise, dizi veya dizedeki tüm öğeleri döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) diziden öğelerin belirtilen sayısını alır ve bir dizedeki karakter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["", "iki"] |
| stringOutput | String | açık |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

<a id="union" />

## <a name="union"></a>birleşim
`union(arg1, arg2, arg3, ...)`

Parametreler, tek bir dizi veya nesne ile tüm öğeleri döndürür. Yinelenen değerler veya anahtarlarının, yalnızca bir kez dahil.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Öğeleri katılmak için kullanılacak ilk değeri. |
| arg2 |Evet |dizi veya nesne |Öğeleri alanına katılmak için kullanılacak ikinci değer. |
| Ek bağımsız değişkenler |Hayır |dizi veya nesne |Öğeleri alanına katılmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya nesne.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/union.json) diziler ve nesnelerle birleşim kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c1"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c2", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Object | {"bir": "a", "iki": "b", "üç": "c2", "dört": "d", "5": "e"} |
| arrayOutput | Dizi | ["bir", "iki", "üç", "dört"] |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

