---
title: Azure Resource Manager şablonu işlevleri - dize | Microsoft Docs
description: Dizelerle çalışma için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/08/2019
ms.author: tomfitz
ms.openlocfilehash: 82b9403a3d5a5b6938f5b95bbfce888d1e70e451
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431220"
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Dize işlevleri için Azure Resource Manager şablonları

Resource Manager, dizeleri ile çalışmak için aşağıdaki işlevleri sunar:

* [Base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [concat](#concat)
* [içerir](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [boş](#empty)
* [endsWith](#endswith)
* [ilk](#first)
* [Biçim](#format)
* [GUID](#guid)
* [indexOf](#indexof)
* [Son](#last)
* [lastIndexOf](#lastindexof)
* [Uzunluğu](#length)
* [newGuid](#newguid)
* [padLeft](#padleft)
* [Değiştir](#replace)
* [Atla](#skip)
* [split](#split)
* [startsWith](#startswith)
* [dize](#string)
* [alt dize](#substring)
* [sınav zamanı](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [Kırpma](#trim)
* [uniqueString](#uniquestring)
* [URI](#uri)
* [uriComponent](#uricomponent)
* [uriComponentToString](#uricomponenttostring)
* [utcNow](#utcnow)

## <a name="base64"></a>Base64

`base64(inputString)`

Giriş dizesinin base64 gösterimini döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| inputString |Evet |string |Bir base64 gösterimine döndürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Base64 gösterimine içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64 işlevinin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Bir iki üç |
| toJsonOutput | Object | {"bir": "a", "iki": "b"} |

## <a name="base64tojson"></a>base64ToJson

`base64tojson`

Bir JSON nesnesi için bir base64 gösterimine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base64Value |Evet |string |Bir JSON nesnesine dönüştürmek için base64 gösterimi. |

### <a name="return-value"></a>Dönüş değeri

Bir JSON nesnesi.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64ToJson işlevi bir base64 değeri dönüştürmek için kullanır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Bir iki üç |
| toJsonOutput | Object | {"bir": "a", "iki": "b"} |

## <a name="base64tostring"></a>base64ToString

`base64ToString(base64Value)`

Bir base64 gösterimi bir dizeye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| base64Value |Evet |string |Bir dizeye dönüştürmek için base64 gösterimi. |

### <a name="return-value"></a>Dönüş değeri

Dönüştürülen bir base64 değeri bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/base64.json) base64ToString işlevi bir base64 değeri dönüştürmek için kullanır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| base64Output | String | b25lLCB0d28sIHRocmVl |
| toStringOutput | String | Bir iki üç |
| toJsonOutput | Object | {"bir": "a", "iki": "b"} |

## <a name="concat"></a>concat

`concat (arg1, arg2, arg3, ...)`

Birden çok dize değerleri birleştirir ve birleştirilmiş dizeyi döndürür veya birden çok dizisi birleştirir ve birleştirilmiş bir dizi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya dizi |Birleştirme için ilk değer. |
| Ek bağımsız değişkenler |Hayır |string |Sıralı birleştirme için ek değerler. |

### <a name="return-value"></a>Dönüş değeri
Bir dize veya birleştirilmiş değerleri dizisi.

### <a name="examples"></a>Örnekler

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

## <a name="contains"></a>içerir

`contains (container, itemToFind)`

Bir dizinin değer içermesi, bir nesne içeren bir anahtar veya bir alt dizenin bir dize içeren olup olmadığını denetler. Dize karşılaştırma büyük/küçük harf duyarlıdır. Ancak, bir nesne bir anahtar içeriyorsa test ederken, karşılaştırma büyük küçük harfe duyarlıdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| container |Evet |dizi, nesne veya dize |Bulunacak değer içeren kayıt değeri. |
| itemToFind |Evet |dize veya tamsayı |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

**Doğru** öğe bulunduysa, **False**.

### <a name="examples"></a>Örnekler

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

## <a name="datauri"></a>dataUri

`dataUri(stringToConvert)`

Bir veri URI'SİNİN bir değere dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToConvert |Evet |string |Bir veri URI'sine dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Bir veri URI'si biçimlendirilmiş bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) değeri bir veri URI'SİNİN dönüştürür ve bir veri URI'SİNİN bir dizeye dönüştürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Merhaba Dünya! |

## <a name="datauritostring"></a>dataUriToString

`dataUriToString(dataUriToConvert)`

Bir veri URI'SİNİN dize değerine biçimlendirilmiş.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Evet |string |Verileri dönüştürmek için URI değeri. |

### <a name="return-value"></a>Dönüş değeri

Dönüştürülen değer içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/datauri.json) değeri bir veri URI'SİNİN dönüştürür ve bir veri URI'SİNİN bir dizeye dönüştürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| dataUriOutput | String | data:text/plain;charset=utf8;base64,SGVsbG8= |
| toStringOutput | String | Merhaba Dünya! |

## <a name="empty"></a>boş

`empty(itemToTest)`

Bir dizi, nesne veya dize boş olup olmadığını belirler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| itemToTest |Evet |dizi, nesne veya dize |Boş olup olmadığı denetlenecek değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** değer boş; Aksi takdirde ise **False**.

### <a name="examples"></a>Örnekler

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

## <a name="endswith"></a>endsWith

`endsWith(stringToSearch, stringToFind)`

Bir dize değeri ile bitip bitmediğini belirler. Karşılaştırma büyük/küçük harfe duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğe içeren bir değer. |
| stringToFind |Evet |string |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

**Doğru** değeri; son karakteri veya bir dizenin karakterlerini eşleştirmek, aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) startsWith ve endsWith işlevleri kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="first"></a>ilk

`first(arg1)`

Dize veya dizinin ilk öğesinin ilk karakteri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |İlk öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

Bir dizenin ilk karakteri veya bir dizideki ilk öğe türünü (dize, int, dizi veya nesne).

### <a name="examples"></a>Örnekler

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

## <a name="format"></a>format

`format(formatString, arg1, arg2, ...)`

Giriş değerlerini biçimlendirilmiş bir dize oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| formatString | Evet | string | Bileşik biçimlendirme dizesi. |
| arg1 | Evet | dize, tamsayı veya Boole değeri | Biçimlendirilen dize içinde değer. |
| Ek bağımsız değişkenler | Hayır | dize, tamsayı veya Boole değeri | Biçimlendirilen dize içinde için ek değerler. |

### <a name="remarks"></a>Açıklamalar

Şablonunuzda bir dizeyi biçimlendirmek için bu işlevi kullanın. Aynı biçimlendirme seçenekleri kullanan [System.String.Format](/dotnet/api/system.string.format) .NET yöntemi.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablonu biçimi işlevinin nasıl kullanılacağını gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "greeting": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "name": {
            "type": "string",
            "defaultValue": "User"
        },
        "numberToFormat": {
            "type": "int",
            "defaultValue": 8175133
        }
    },
    "resources": [
    ],
    "outputs": {
        "formatTest": {
            "type": "string",
            "value": "[format('{0}, {1}. Formatted number: {2:N0}', parameters('greeting'), parameters('name'), parameters('numberToFormat'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| formatTest | String | Merhaba, kullanıcı. Biçimlendirilen sayı: 8,175,133 |

## <a name="guid"></a>GUID

`guid(baseString, ...)`

Parametre olarak sağlanan değerlere göre genel olarak benzersiz bir tanımlayıcı biçiminde bir değer oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| baseString |Evet |string |Karma işlev GUID oluşturmak için kullanılan değer. |
| gerektiği gibi ek parametreler |Hayır |string |Benzersizlik düzeyini belirten değer oluşturmak için gereken sayıda dizeleri ekleyebilirsiniz. |

### <a name="remarks"></a>Açıklamalar

Bu işlev, genel olarak benzersiz bir tanımlayıcı biçiminde bir değer oluşturmak ihtiyacınız olduğunda yararlıdır. Sonuç için benzersizlik kapsamını sınırlayan parametre değerlerini sağlayın. Abonelik, kaynak grubu veya dağıtım aşağı benzersiz adı olup olmadığını belirtebilirsiniz.

Döndürülen değer, rastgele bir dize, ancak bunun yerine bir karma işlev parametrelerine sonucu değil. 36 karakterden döndürülen değerdir. Genel olarak benzersiz değil. Parametrelerden biri bu karma değere göre değil, yeni bir GUID oluşturmak için kullanın [newGuid](#newguid) işlevi.

Aşağıdaki örnekler, yaygın olarak kullanılan düzeyleri için benzersiz bir değer oluşturmak için GUID kullanmayı gösterir.

Benzersiz abonelik kapsamı

```json
"[guid(subscription().subscriptionId)]"
```

Kaynak grubu kapsamında benzersiz

```json
"[guid(resourceGroup().id)]"
```

Benzersiz bir kaynak grubu için dağıtım kapsamına

```json
"[guid(resourceGroup().id, deployment().name)]"
```

### <a name="return-value"></a>Dönüş değeri

Genel olarak benzersiz bir tanımlayıcı biçimi 36 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/guid.json) GUID'ini sonuçlarını döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [],
    "outputs": {
        "guidPerSubscription": {
            "value": "[guid(subscription().subscriptionId)]",
            "type": "string"
        },
        "guidPerResourceGroup": {
            "value": "[guid(resourceGroup().id)]",
            "type": "string"
        },
        "guidPerDeployment": {
            "value": "[guid(resourceGroup().id, deployment().name)]",
            "type": "string"
        }
    }
}
```

## <a name="indexof"></a>indexOf

`indexOf(stringToSearch, stringToFind)`

Bir dize içindeki bir değerin ilk konumunu döndürür. Karşılaştırma büyük/küçük harfe duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğe içeren bir değer. |
| stringToFind |Evet |string |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

Bulunacak öğenin konumunu temsil eden bir tamsayı. Değer sıfır tabanlıdır. Öğe bulunmazsa -1 döndürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) indexOf ve lastIndexOf işlevleri kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| Bulunamadı | Int | -1 |

## <a name="last"></a>Son

`last (arg1)`

Döndürür en son karakter dizesi veya dizinin son öğesi.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Son öğe veya karakter almak için değer. |

### <a name="return-value"></a>Dönüş değeri

Bir dizenin son karakter veya bir dizi içerisindeki son öğe türünü (dize, int, dizi veya nesne).

### <a name="examples"></a>Örnekler

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

## <a name="lastindexof"></a>lastIndexOf

`lastIndexOf(stringToSearch, stringToFind)`

Bir dize içindeki bir değerin son konumunu döndürür. Karşılaştırma büyük/küçük harfe duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğe içeren bir değer. |
| stringToFind |Evet |string |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

Bulunacak öğenin son konumunu temsil eden bir tamsayı. Değer sıfır tabanlıdır. Öğe bulunmazsa -1 döndürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/indexof.json) indexOf ve lastIndexOf işlevleri kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| firstT | Int | 0 |
| lastT | Int | 3 |
| firstString | Int | 2 |
| lastString | Int | 0 |
| Bulunamadı | Int | -1 |

## <a name="length"></a>length

`length(string)`

Bir dize veya bir dizideki öğelerin karakter sayısını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi veya dize |Karakter sayısını almak için kullanılacak öğeler veya dize sayısını almak için kullanılacak dizisi. |

### <a name="return-value"></a>Dönüş değeri

Tamsayı 

### <a name="examples"></a>Örnekler

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

## <a name="newguid"></a>newGuid

`newGuid()`

Genel olarak benzersiz bir tanımlayıcı biçiminde bir değer döndürür. **Bu işlev, parametre için varsayılan değeri yalnızca kullanılabilir.**

### <a name="remarks"></a>Açıklamalar

Bu gibi durumlarda, bu işlev içindeki bir ifade yalnızca bir parametrenin varsayılan değeri için kullanabilirsiniz. Bu işlev bir şablonda başka bir yerde kullanarak bir hata döndürür. Her çağrıldığında farklı bir değer döndürdüğünden işlev şablonunun diğer bölümlerinde izin verilmiyor. Aynı parametrelere sahip aynı şablon dağıtımı, güvenilir bir şekilde aynı sonucu verir mıydı.

NewGuid işlevi farklıdır [GUID](#guid) hiçbir parametre almaz çünkü işlev. GUID ile aynı parametre çağırdığınızda, her zaman aynı tanımlayıcıyı döndürür. GUID, belirli bir ortam için aynı GUID güvenilir bir şekilde oluşturmak ihtiyacınız olduğunda kullanın. Kaynakları bir test ortamına dağıtmak gibi her seferinde farklı bir tanımlayıcı gerektiğinde newGuid kullanın.

Kullanırsanız [seçeneğini bir önceki başarılı dağıtımı yeniden](resource-group-template-deploy-rest.md#redeploy-when-deployment-fails)ve önceki dağıtım newGuid kullanan bir parametre içeriyorsa, parametreyi yeniden değerlendirimiş değil. Bunun yerine, önceki dağıtımla parametre değeri geri alma dağıtımda otomatik olarak yeniden kullanılır.

Bir test ortamında art arda kısa bir süre için yalnızca canlı kaynakları dağıtmanız gerekebilir. Benzersiz adlar oluşturmak yerine newGuid ile kullanabileceğiniz [uniqueString](#uniquestring) benzersiz adlar oluşturmak için.

NewGuid işlevi için bir varsayılan değer dayanan bir şablonu yeniden dağıtırken dikkatli olun. Yeniden parametresi için bir değer sağlamıyorsa, işlev değerlendirilir. Mevcut bir kaynağı güncelleştirmek yerine yeni bir tane oluşturmak isterseniz, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Dönüş değeri

Genel olarak benzersiz bir tanımlayıcı biçimi 36 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablonu, yeni bir tanımlayıcıya sahip bir parametre gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "guidOutput": {
            "type": "string",
            "value": "[parameters('guidValue')]"
        }
    }
}
```

Önceki örnekteki çıktı, her dağıtım için farklılık gösterir ancak şuna benzer olacaktır:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| guidOutput | string | b76a51fc-bd72-4a77-b9a2-3c29e7d2e551 |

Aşağıdaki örnek, bir depolama hesabı için benzersiz bir ad oluşturmak için newGuid işlevi kullanır. Bu şablon, burada depolama hesabı için kısa bir süre var ve imzalanmasını değil test ortamı için işe yarayabilir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "guidValue": {
            "type": "string",
            "defaultValue": "[newGuid()]"
        }
    },
    "variables": {
        "storageName": "[concat('storage', uniqueString(parameters('guidValue')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "West US",
            "apiVersion": "2018-07-01",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {}
        }
    ],
    "outputs": {
        "nameOutput": {
            "type": "string",
            "value": "[variables('storageName')]"
        }
    }
}
```

Önceki örnekteki çıktı, her dağıtım için farklılık gösterir ancak şuna benzer olacaktır:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| nameOutput | string | storagenziwvyru7uxie |


## <a name="padleft"></a>padLeft

`padLeft(valueToPad, totalLength, paddingCharacter)`

Toplam belirtilen uzunlukta ulaşana kadar sola karakter ekleyerek sağa hizalanmış bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToPad |Evet |dize veya tamsayı |Sağa hizalama değeri. |
| totalLength |Evet |int |Döndürülen dize karakter sayısı. |
| paddingCharacter |Hayır |tek karakter |Sol-toplam uzunluğu ulaşılana kadar doldurma için kullanılacak karakter. Varsayılan değer bir alandır. |

Özgün dizeye karakter yazma sayısından daha uzun ise, hiçbir karakteri eklenir.

### <a name="return-value"></a>Dönüş değeri

Bir dize ile en az belirtilen karakter sayısı.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/padleft.json) kullanıcı tarafından sağlanan parametre değeri sıfır karakter toplam karakter sayısı ulaşana kadar ekleyerek yazma işlemi gösterilmektedir. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | String | 0000000123 |

## <a name="replace"></a>Değiştir

`replace(originalString, oldString, newString)`

Başka bir dizeyle değiştirildiği bir dizenin tüm örnekleri ile yeni bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalString |Evet |string |Sahip tüm örneklerini başka bir dizeyle değiştirildiği bir dize değeri. |
| Eskidize |Evet |string |Orijinal dizeden kaldırılacak dize. |
| Yenidize |Evet |string |Kaldırılan dize yerine eklenecek dize. |

### <a name="return-value"></a>Dönüş değeri

Değiştirilen karakterleri içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/replace.json) kullanıcı tarafından sağlanan dizedeki tüm tire kaldırma ve dizenin bir bölümünü başka bir dizeyle değiştirmeniz gösterilmektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secondOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| firstOutput | String | 1231231234 |
| secondOutput | String | 123-123-xxxx |

## <a name="skip"></a>Atla

`skip(originalValue, numberToSkip)`

Sonra belirtilen sayıda öğeyi belirtilen sayıda karakter veya tüm öğeleri olan bir dizi sonra tüm karakterleri içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya atlama için kullanılacak dize. |
| numberToSkip |Evet |int |Öğeleri veya atlamak için karakter sayısı. Bu değer 0 veya daha az ise, tüm öğeleri veya bulunan değerin karakter döndürülür. Dizi veya dize uzunluğundan daha büyük ise, boş bir dizi veya dize döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="examples"></a>Örnekler

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

## <a name="split"></a>split

`split(inputString, delimiter)`

Tarafından belirtilen sınırlayıcılardan ayrılmış alt dizelere Giriş dizesinin içeren dize dizisi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| inputString |Evet |string |Ayrılacak dize. |
| Sınırlayıcı |Evet |dize veya dize dizisi |Dizeyi bölerken kullanılacak ayırıcı. |

### <a name="return-value"></a>Dönüş değeri

Dize dizisi.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/split.json) giriş virgül ve virgül veya noktalı virgül ile dizeyi böler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| firstOutput | Dizi | ["bir", "iki", "üç"] |
| secondOutput | Dizi | ["bir", "iki", "üç"] |

## <a name="startswith"></a>startsWith

`startsWith(stringToSearch, stringToFind)`

Bir dizeyi bir değerle başlayıp başlamadığını belirler. Karşılaştırma büyük/küçük harfe duyarsızdır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToSearch |Evet |string |Bulunacak öğe içeren bir değer. |
| stringToFind |Evet |string |Bulunacak değer. |

### <a name="return-value"></a>Dönüş değeri

**Doğru** ilk karakteri veya karakterleri dize değeri; eşleşiyorsa Aksi takdirde, **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/startsendswith.json) startsWith ve endsWith işlevleri kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| startsTrue | Bool | True |
| startsCapTrue | Bool | True |
| startsFalse | Bool | False |
| endsTrue | Bool | True |
| endsCapTrue | Bool | True |
| endsFalse | Bool | False |

## <a name="string"></a>string

`string(valueToConvert)`

Belirtilen değerin bir dizeye dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet | Tüm |Dizeye dönüştürülecek değer. Nesneler ve diziler de dahil olmak üzere herhangi bir türde değer dönüştürülebilir. |

### <a name="return-value"></a>Dönüş değeri

Dönüştürülmüş değeri bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/string.json) farklı değer türlerini dizelere dönüştürme gösterilmektedir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| objectOutput | String | {"Değera": 10, "Değerb": "Örnek Text"} |
| arrayOutput | String | ["a", "b", "c"] |
| intOutput | String | 5 |

## <a name="substring"></a>alt dize

`substring(stringToParse, startIndex, length)`

Belirtilen karakter konumunda başlar ve belirtilen sayıda karakteri içeren bir alt dizeyi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToParse |Evet |string |Alt dizenin ayıklanacağı özgün dize. |
| startIndex |Hayır |int |Sıfır tabanlı başlangıç karakteri konumu alt dize. |
| length |Hayır |int |Alt dizenin karakter sayısı. Dize içindeki bir konuma başvurmalıdır. Sıfır olmalıdır veya büyük. |

### <a name="return-value"></a>Dönüş değeri

Alt dize. Veya uzunluğu sıfır ise boş bir dize.

### <a name="remarks"></a>Açıklamalar

İşlevi, alt dizeyi dizenin sonunu aşan veya uzunluğu küçük olduğunda sıfır başarısız olur. Aşağıdaki örnek, "dizin ve uzunluk parametreleri dize içindeki bir konuma başvurmalıdır. şu hatayla başarısız oluyor Dizin parametresi: '0', uzunluk parametresi: '11', dize parametresinin uzunluğu: '10'.".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/substring.json) bir parametresi bir alt dizeyi ayıklar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| substringOutput | String | iki |

## <a name="take"></a>sınav zamanı

`take(originalValue, numberToTake)`

Dize veya bir dizi öğeleri dizisi başından itibaren belirtilen sayıda başından itibaren belirtilen sayıda karakteri içeren bir dize döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| originalValue |Evet |dizi veya dize |Dizi veya dizedeki öğeleri gerçekleştirilecek. |
| numberToTake |Evet |int |Öğeleri veya gerçekleştirilecek karakter sayısı. Bu değer 0 veya daha az ise, boş bir dizi veya dize döndürülür. Belirli bir dizi veya dize uzunluğundan daha büyük ise, dizi veya dizedeki tüm öğeleri döndürülür. |

### <a name="return-value"></a>Dönüş değeri

Bir dizi veya dize.

### <a name="examples"></a>Örnekler

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

## <a name="tolower"></a>toLower

`toLower(stringToChange)`

Belirtilen dizeyi küçük harfe dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToChange |Evet |string |Küçük harflere dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Dizeyi küçük harfe dönüştürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) bir parametre değeri için büyük harf ve küçük harfe dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| toLowerOutput | String | Bir iki üç |
| toUpperOutput | String | BİR İKİ ÜÇ |

## <a name="toupper"></a>toUpper

`toUpper(stringToChange)`

Belirtilen dizeyi büyük harfe dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToChange |Evet |string |Büyük harfe dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Dizeyi büyük harfe dönüştürülür.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/tolower.json) bir parametre değeri için büyük harf ve küçük harfe dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| toLowerOutput | String | Bir iki üç |
| toUpperOutput | String | BİR İKİ ÜÇ |

## <a name="trim"></a>Kırpma

`trim (stringToTrim)`

Belirtilen dizeden tüm öndeki ve sondaki boşluk karakterlerini kaldırır.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToTrim |Evet |string |Trim değer. |

### <a name="return-value"></a>Dönüş değeri

Baştaki ve sondaki boşluk karakterlerini içermeyen bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/trim.json) parametre beyaz boşluk karakterleri kırpar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| döndürülecek | String | Bir iki üç |

## <a name="uniquestring"></a>uniqueString

`uniqueString (baseString, ...)`

Parametre olarak sağlanan değerlere göre belirleyici karma dize oluşturur. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| baseString |Evet |string |Karma işlev benzersiz bir dize oluşturmak için kullanılan değer. |
| gerektiği gibi ek parametreler |Hayır |string |Benzersizlik düzeyini belirten değer oluşturmak için gereken sayıda dizeleri ekleyebilirsiniz. |

### <a name="remarks"></a>Açıklamalar

Bu işlev, bir kaynak için benzersiz bir ad oluşturmak ihtiyacınız olduğunda yararlıdır. Sonuç için benzersizlik kapsamını sınırlayan parametre değerlerini sağlayın. Abonelik, kaynak grubu veya dağıtım aşağı benzersiz adı olup olmadığını belirtebilirsiniz. 

Döndürülen değer, rastgele bir dize, ancak bunun yerine bir karma işlev sonucu değil. 13 karakter döndürülen değerdir. Genel olarak benzersiz değil. Değer bir adlandırma kuralınızın anlamlı bir ad oluşturmak için önekten birleştirmek isteyebilirsiniz. Aşağıdaki örnek, döndürülen değerin biçimi gösterir. Gerçek değeri tarafından sağlanan parametreler değişir.

    tcvhiyu5h2o5o

Aşağıdaki örnekler uniqueString yaygın olarak kullanılan düzeyleri için benzersiz bir değer oluşturmak için nasıl kullanılacağını gösterir.

Benzersiz abonelik kapsamı

```json
"[uniqueString(subscription().subscriptionId)]"
```

Kaynak grubu kapsamında benzersiz

```json
"[uniqueString(resourceGroup().id)]"
```

Benzersiz bir kaynak grubu için dağıtım kapsamına

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Aşağıdaki örnek, kaynak grubunuzun tabanlı bir depolama hesabı için benzersiz bir ad oluşturma işlemi gösterilmektedir. Kaynak grubu içinde benzersiz şekilde oluşturulmuş olup bir adı değil.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

Her zaman benzersiz yeni bir ad oluşturmak ihtiyacınız varsa şablon dağıtma ve kaynak güncelleştirme düşünmüyorsanız, kullanabilirsiniz [utcNow](#utcnow) uniqueString işleviyle. Bir test ortamında bu yaklaşımı kullanabilirsiniz. Bir örnek için bkz. [utcNow](#utcnow).

### <a name="return-value"></a>Dönüş değeri

13 karakter içeren bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uniquestring.json) uniquestring sonuçları döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

## <a name="uri"></a>URI

`uri (baseUri, relativeUri)`

Bir mutlak URI baseUri ve relativeUri dize birleştirerek oluşturur.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| baseUri |Evet |string |Temel uri dizesi. |
| relativeUri |Evet |string |Taban URI dizeye eklenecek göreli uri dizesi. |

Değeri **baseUri** parametresi, belirli bir dosya dahil edebilirsiniz, ancak yalnızca temel yol URI diziden oluşturulurken kullanılır. Örneğin, geçirme `http://contoso.com/resources/azuredeploy.json` baseUri parametre sonuçları bir temel URI'sini olarak `http://contoso.com/resources/`.

### <a name="return-value"></a>Dönüş değeri

Temel ve göreli değerlerin mutlak URI'sini temsil eden bir dize.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek, ana şablon değerine göre bir iç içe geçmiş şablon için bir bağlantı oluşturmak gösterilmektedir.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) URI uriComponent ve uriComponentToString kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="uricomponent"></a>uriComponent

`uricomponent(stringToEncode)`

Bir URI kodlar.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| stringToEncode |Evet |string |Kodlanacak değer. |

### <a name="return-value"></a>Dönüş değeri

URI'ın bir dize değeri kodlanmış.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) URI uriComponent ve uriComponentToString kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="uricomponenttostring"></a>uriComponentToString

`uriComponentToString(uriEncodedString)`

Kodlamalı bir dize URI değeri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Evet |string |URI bir dizeye dönüştürülecek değer kodlanmış. |

### <a name="return-value"></a>Dönüş değeri

Kodu çözülen dize URI değeri kodlanmış.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/uri.json) URI uriComponent ve uriComponentToString kullanma işlemini gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| uriOutput | String | http://contoso.com/resources/nested/azuredeploy.json |
| componentOutput | String | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | String | http://contoso.com/resources/nested/azuredeploy.json |

## <a name="utcnow"></a>utcNow

`utcNow(format)`

(UTC) datetime değeri belirtilen biçimde döndürür. Biçim sağlanmazsa ISO 8601 (yyyyMMddTHHmmssZ) biçimi kullanılır. **Bu işlev, parametre için varsayılan değeri yalnızca kullanılabilir.**

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| format |Hayır |string |URI bir dizeye dönüştürülecek değer kodlanmış. Hangisini [standart biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) veya [özel biçim dizeleri](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). |

### <a name="remarks"></a>Açıklamalar

Bu gibi durumlarda, bu işlev içindeki bir ifade yalnızca bir parametrenin varsayılan değeri için kullanabilirsiniz. Bu işlev bir şablonda başka bir yerde kullanarak bir hata döndürür. Her çağrıldığında farklı bir değer döndürdüğünden işlev şablonunun diğer bölümlerinde izin verilmiyor. Aynı parametrelere sahip aynı şablon dağıtımı, güvenilir bir şekilde aynı sonucu verir mıydı.

Kullanırsanız [seçeneğini bir önceki başarılı dağıtımı yeniden](resource-group-template-deploy-rest.md#redeploy-when-deployment-fails)ve önceki dağıtım utcNow kullanan bir parametre içeriyorsa, parametreyi yeniden değerlendirimiş değil. Bunun yerine, önceki dağıtımla parametre değeri geri alma dağıtımda otomatik olarak yeniden kullanılır.

UtcNow işlevi için bir varsayılan değer dayanan bir şablonu yeniden dağıtırken dikkatli olun. Yeniden parametresi için bir değer sağlamıyorsa, işlev değerlendirilir. Mevcut bir kaynağı güncelleştirmek yerine yeni bir tane oluşturmak isterseniz, önceki dağıtımdan parametre değerini geçirin.

### <a name="return-value"></a>Dönüş değeri

Geçerli UTC tarih saat değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki örnek şablonu, tarih saat değeri için farklı biçimler gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcValue": {
            "type": "string",
            "defaultValue": "[utcNow()]"
        },
        "utcShortValue": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "utcCustomValue": {
            "type": "string",
            "defaultValue": "[utcNow('M d')]"
        }
    },
    "resources": [
    ],
    "outputs": {
        "utcOutput": {
            "type": "string",
            "value": "[parameters('utcValue')]"
        },
        "utcShortOutput": {
            "type": "string",
            "value": "[parameters('utcShortValue')]"
        },
        "utcCustomOutput": {
            "type": "string",
            "value": "[parameters('utcCustomValue')]"
        }
    }
}
```

Önceki örnekteki çıktı, her dağıtım için farklılık gösterir ancak şuna benzer olacaktır:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| utcOutput | string | 20190305T175318Z |
| utcShortOutput | string | 03/05/2019 |
| utcCustomOutput | string | 3 5 |

Sonraki örnek, bir etiket değeri ayarlanırken işlevden bir değer kullanmayı gösterir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "utcShort": {
            "type": "string",
            "defaultValue": "[utcNow('d')]"
        },
        "rgName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "name": "[parameters('rgName')]",
            "location": "westeurope",
            "tags":{
                "createdDate": "[parameters('utcShort')]"
            },
            "properties":{}
        }
    ],
    "outputs": {
        "utcShort": {
            "type": "string",
            "value": "[parameters('utcShort')]"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

