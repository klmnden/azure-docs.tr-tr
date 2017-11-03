---
title: "Azure Resource Manager şablonu işlevleri - diziler ve nesneleri | Microsoft Docs"
description: "Diziler ve nesneleri ile çalışmak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklanmaktadır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2017
ms.author: tomfitz
ms.openlocfilehash: 7d040fe55cb46665c97668a76ccbc66adc002f89
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Dizi ve nesne işlevleri için Azure Resource Manager şablonları 

Kaynak Yöneticisi, nesneler ve diziler ile çalışmak için birkaç işlevleri sağlar.

* [dizi](#array)
* [birleşim](#coalesce)
* [concat](#concat)
* [içerir](#contains)
* [createArray](#createarray)
* [boş](#empty)
* [ilk](#first)
* [kesişim](#intersection)
* [JSON](#json)
* [Son](#last)
* [uzunluğu](#length)
* [max](#max)
* [Min](#min)
* [Aralık](#range)
* [Atla](#skip)
* [Al](#take)
* [birleşim](#union)

Değere göre ayrılmış dize değerleri dizisi almak için bkz: [bölme](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>Dizi
`array(convertToArray)`

Değer bir diziye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| convertToArray |Evet |int, string, dizi veya nesne |Bir dizi dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/array.json) dizi işlevinin farklı türleri ile nasıl kullanılacağını gösterir.

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
            "defaultValue": "a"
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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| intOutput | Dizi | [1] |
| stringOutput | Dizi | ["a"] |
| objectOutput | Dizi | [{"a": "b", "c": "d"}] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/array.json
```

<a id="coalesce" />

## <a name="coalesce"></a>birleşim
`coalesce(arg1, arg2, arg3, ...)`

İlk değeri null olmayan parametreler döndürür. Boş dizeler, boş diziler ve boş nesneleri null değil.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |int, string, dizi veya nesne |Null test etmek için ilk değer. |
| Ek bağımsız değişken |Hayır |int, string, dizi veya nesne |Null test için ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Dize, int, dizi veya nesne olabilir ilk null olmayan parametre değeri. Tüm parametreleri null ise null. 

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/coalesce.json) birleşim farklı kullanımlarını çıktısını gösterir.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | Varsayılan |
| intOutput | Int | 1 |
| objectOutput | Nesne | {"ilk": "varsayılan"} |
| arrayOutput | Dizi | [1] |
| emptyOutput | bool | True |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/coalesce.json
```

<a id="concat" />

## <a name="concat"></a>concat
`concat(arg1, arg2, arg3, ...)`

Birden çok birleştirir ve birleştirilmiş bir dizi döndürür veya birden çok dize değerlerini birleştirir ve birleştirilmiş dizeyi döndürür. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk dizi veya dize birleştirme için. |
| Ek bağımsız değişkenler |Hayır |dizi veya dize |Ek diziler veya birleştirme için sıralı bir düzende dizeleri. |

Bu işlev bağımsız değişkenleri herhangi bir sayıda alabilir ve dizeler veya diziler parametreleri için kabul edebilir.

### <a name="return-value"></a>Dönüş değeri
Bir dize veya birleştirilmiş değerleri dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-array.json) nasıl dizilerinin birleştirileceğini gösterir.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| Döndür | Dizi | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-array.json
```

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/concat-string.json) iki dize değerleri birleştirmek ve birleştirilmiş dizeyi döndürür gösterilmektedir.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| concatOutput | Dize | önek 5yj4yjf5mbg72 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/concat-string.json
```

<a id="contains" />

## <a name="contains"></a>içerir
`contains(container, itemToFind)`

Bir değer dizisini içerir, bir nesne bir anahtar veya bir dize bir alt dizeyi içeren olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| kapsayıcı |Evet |dizi, nesne veya dize |Bulunacak değer içeren değeri. |
| itemToFind |Evet |dize veya int |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

**Doğru** öğe bulunduysa, **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/contains.json) nasıl kullanılacağını içeren farklı türleriyle gösterir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringTrue | bool | True |
| stringFalse | bool | False |
| objectTrue | bool | True |
| objectFalse | bool | False |
| arrayTrue | bool | True |
| arrayFalse | bool | False |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/contains.json
```

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

Bir dizi parametrelerinden oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |Dize, tamsayı, dizi veya nesne |Dizinin ilk değer. |
| Ek bağımsız değişkenler |Hayır |Dize, tamsayı, dizi veya nesne |Dizideki ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/createarray.json) farklı türleriyle createArray kullanmayı gösterir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringArray | Dizi | ["a", "b", "c"] |
| int | Dizi | [1, 2, 3] |
| objectArray | Dizi | [{"bir": "a", "iki": "b", "üç": "c"}] |
| arrayArray | Dizi | [["bir", "iki", "üç"]] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/createarray.json
```

<a id="empty" />

## <a name="empty"></a>boş

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| itemToTest |Evet |dizi, nesne veya dize |Değerin boş olup olmadığını denetlemek için. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** değeri geçerliyse boş, aksi takdirde **False**.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/empty.json) bir dizi, nesne ve dize boş olup olmadığını denetler.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayEmpty | bool | True |
| objectEmpty | bool | True |
| stringEmpty | bool | True |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/empty.json
```

<a id="first" />

## <a name="first"></a>ilk
`first(arg1)`

Dizinin ilk öğesi veya ilk karakter dizesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

(String, int, dizi veya nesne) ilk öğe türü bir dizi ya da ilk karakteri bir dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/first.json) ilk işlevi bir dizi ve dize ile nasıl kullanılacağını gösterir.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dize | bir |
| stringOutput | Dize | O |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/first.json
```

<a id="intersection" />

## <a name="intersection"></a>kesişim
`intersection(arg1, arg2, arg3, ...)`

Tek bir dizi veya nesne ortak öğeler ile parametrelerinden döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Ortak öğeler bulmak için kullanılacak ilk değer. |
| arg2 |Evet |dizi veya nesne |Ortak öğeler bulmak için kullanılacak ikinci değer. |
| Ek bağımsız değişkenler |Hayır |dizi veya nesne |Ortak öğeler bulmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya nesne ortak öğeler ile.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/intersection.json) kesişim dizileri ve nesneleri ile nasıl kullanılacağı gösterilmektedir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"bir": "a", "üç": "c"} |
| arrayOutput | Dizi | ["iki", "üç"] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/intersection.json
```

## <a name="json"></a>JSON
`json(arg1)`

Bir JSON nesnesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |Dize |JSON olarak dönüştürülecek değer. |


### <a name="return-value"></a>Dönüş değeri

Belirtilen dize JSON nesnesinden ya da boş bir nesneyi zaman **null** belirtilir.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/json.json) json işlevi dizileri ve nesneleri ile nasıl kullanılacağı gösterilmektedir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
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
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| jsonOutput | Nesne | {"a": "b"} |
| nullOutput | Boole değeri | True |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/json.json
```

<a id="last" />

## <a name="last"></a>Son
`last (arg1)`

Son dizi öğesinden veya son karakter dizesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Son öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

(String, int, dizi veya nesne) son öğe türü bir dizi ya da son karakter bir dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/last.json) son işlevi bir dizi ve dize ile nasıl kullanılacağını gösterir.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dize | üç |
| stringOutput | Dize | E |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/last.json
```

<a id="length" />

## <a name="length"></a>uzunluğu
`length(arg1)`

Bir dizi ya da bir dizedeki karakter öğe sayısını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Karakter sayısını almak için kullanılacak öğeler veya dize sayısını almak için kullanılacak dizisi. |

### <a name="return-value"></a>Dönüş değeri

İnt 

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/length.json) bir dizi ve dize uzunluğu kullanmayı gösterir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayLength | Int | 3 |
| stringLength | Int | 13 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/length.json
```

Kaynakları oluştururken, yineleme sayısını belirtmek için bir dizi bu işlevi kullanabilirsiniz. Aşağıdaki örnekte, parametre **siteNames** bir dizi web siteleri oluştururken kullanılacak adını başvurur.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Bu işlev bir dizi ile kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).

<a id="max" />

## <a name="max"></a>max
`max(arg1)`

En büyük değer dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi tamsayı veya virgülle ayrılmış tamsayı listesi |En yüksek değeri elde koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En büyük değeri temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/max.json) en büyük dizi ve tamsayı listesi ile nasıl kullanılacağı gösterilmektedir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Int | 5 |
| intOutput | Int | 5 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>dk
`min(arg1)`

En düşük değer dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi tamsayı veya virgülle ayrılmış tamsayı listesi |En düşük değer almak için koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En düşük değer temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/min.json) en az bir dizi ve tamsayı listesi ile nasıl kullanılacağı gösterilmektedir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Int | 0 |
| intOutput | Int | 0 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="range" />

## <a name="range"></a>Aralık
`range(startingInteger, numberOfElements)`

Dizisi bir tamsayı başlatma ve bir öğe sayısı içeren oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| startingInteger |Evet |Int |Dizideki ilk tamsayı. |
| numberofElements |Evet |Int |Dizideki tamsayılar sayısı. |

### <a name="return-value"></a>Dönüş değeri

Dizisi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/range.json) aralığı işlevinin nasıl kullanılacağını gösterir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| rangeOutput | Dizi | [5, 6, 7] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/range.json
```

<a id="skip" />

## <a name="skip"></a>Atla
`skip(originalValue, numberToSkip)`

Belirtilen geçtikten sonra dizinin tüm öğeleri bir dizi döndürür veya belirtilen geçtikten sonra dizesindeki tüm karakterler içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya atlama kullanılacak dize. |
| numberToSkip |Evet |Int |Öğeleri veya atlamak için karakter sayısı. Bu değer 0 veya daha az ise, tüm öğeleri veya karakter değeri döndürülür. Dizi veya dize uzunluğundan büyük olursa, boş dize veya dize döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/skip.json) belirtilen sayıda öğeyi dizisinde bulunan ve belirtilen bir dizedeki karakter sayısını atlar.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["üç"] |
| stringOutput | Dize | iki üç |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/skip.json
```

<a id="take" />

## <a name="take"></a>Al
`take(originalValue, numberToTake)`

Dize veya dizi karakter dizesinin başından başlayarak belirtilen sayıda ile başından bir dizi belirtilen sayıda öğeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya dize öğeleri gerçekleştirilecek. |
| numberToTake |Evet |Int |Öğeleri veya yapılacak karakter sayısı. Bu değer 0 veya daha az ise, boş dize veya dize döndürülür. Verilen dizi veya dize uzunluğundan büyük olursa, dizi veya dize tüm öğeler döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/take.json) belirtilen sayıda öğeyi dizisinden alır ve bir dizeden karakterleri.

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| arrayOutput | Dizi | ["", "iki"] |
| stringOutput | Dize | üzerinde |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/take.json
```

<a id="union" />

## <a name="union"></a>birleşim
`union(arg1, arg2, arg3, ...)`

Tek bir dizi veya nesne tüm öğelerle parametrelerinden döndürür. Yinelenen değerler veya anahtarlar, yalnızca bir kez dahil.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya nesne |Öğeleri katılmak için kullanılacak ilk değer. |
| arg2 |Evet |dizi veya nesne |Öğeleri katılmak için kullanılacak ikinci değer. |
| Ek bağımsız değişkenler |Hayır |dizi veya nesne |Öğeleri katılmak için kullanılacak ek değerler. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya nesne.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/union.json) UNION dizileri ve nesneleri ile nasıl kullanılacağı gösterilmektedir:

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

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | Nesne | {"bir": "a", "iki": "b", "üç": "c2", "dört": "d", "beş": "e"} |
| arrayOutput | Dizi | ["bir", "iki", "üç", "dört"] |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/union.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

