---
title: "Azure Resource Manager şablonu işlevleri - sayısal | Microsoft Docs"
description: "Sayılarla çalışmak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklanmaktadır."
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
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 2b7ec44b820e510d1e8bd99ef195546a519c365c
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Sayısal işlevler için Azure Resource Manager şablonları

Resource Manager tamsayılar ile çalışmak için aşağıdaki işlevleri sunar:

* [ekleme](#add)
* [Copyındex](#copyindex)
* [div](#div)
* [kayan nokta](#float)
* [int](#int)
* [max](#max)
* [Min](#min)
* [mod](#mod)
* [mul](#mul)
* [Sub](#sub)

<a id="add" />

## <a name="add"></a>Ekleme
`add(operand1, operand2)`

Sağlanan iki tamsayının toplamını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- | 
|operand1 |Evet |Int |Eklenecek ilk sayı. |
|operand2 |Evet |Int |Eklenecek ikinci sayı. |

### <a name="return-value"></a>Dönüş değeri

Parametreleri toplamını içeren bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) iki parametre ekler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| addResult | Int | 8 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json 
```

<a id="copyindex" />

## <a name="copyindex"></a>Copyındex
`copyIndex(loopName, offset)`

Bir yineleme döngüsü dizinini döndürür. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| loopName | Hayır | Dize | Yineleme alma döngü adı. |
| uzaklık |Hayır |Int |Sıfır tabanlı yineleme değer eklemek için numarası. |

### <a name="remarks"></a>Açıklamalar

Bu işlev her zaman ile kullanılan bir **kopyalama** nesnesi. İçin hiçbir değer sağlanmazsa **uzaklık**, geçerli yineleme değeri döndürülür. Yineleme değeri sıfırdan başlatır. Yineleme döngüleri kaynakları veya değişkenleri tanımlarken kullanabilirsiniz.

**LoopName** özelliği Copyındex bir kaynak yineleme ya da özellik yineleme başvuran belirtmenize olanak sağlar. İçin hiçbir değer sağlanmazsa **loopName**, geçerli kaynak türü yineleme kullanılır. İçin bir değer girin **loopName** bir özellik dolaşırken. 
 
Nasıl kullanabileceğinize tam bir açıklaması için **Copyındex**, bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).

Kullanarak bir örnek için **Copyındex** bir değişken tanımlarken bkz [değişkenleri](resource-group-authoring-templates.md#variables).

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir kopya döngü ve adına dahil dizin değeri gösterir. 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a>Dönüş değeri

Mevcut dizin yineleme temsil eden bir tamsayı.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

İki sağlanan tamsayı tamsayı bölümünü döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |Int |Ayrılmış sayı. |
| operand2 |Evet |Int |Bölmek için kullanılan sayı. 0 olamaz. |

### <a name="return-value"></a>Dönüş değeri

Bölme temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) başka bir parametre olarak bir parametre böler.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| divResult | Int | 2 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json 
```

<a id="float" />

## <a name="float"></a>Kayan nokta
`float(arg1)`

Değeri kayan dönüştürür nokta sayısı. Bir mantıksal uygulama gibi bir uygulama için özel parametreler geçirilirken yalnızca bu işlevi kullanın.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya int |Kayan dönüştürülecek değer nokta sayısı. |

### <a name="return-value"></a>Dönüş değeri
Kayan nokta sayısı.

### <a name="example"></a>Örnek

Aşağıdaki örnek float parametreleri için bir mantıksal uygulama geçirmek için nasıl kullanılacağını gösterir:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
            "custom1": {
                "value": "[float('3.0')]"
            },
            "custom2": {
                "value": "[float(3)]"
            },
```

<a id="int" />

## <a name="int"></a>Int
`int(valueToConvert)`

Belirtilen değer bir tamsayıya dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet |dize veya int |Bir tamsayıya dönüştürmek için değer. |

### <a name="return-value"></a>Dönüş değeri

Dönüştürülen değerin tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) kullanıcı tarafından sağlanan parametre değeri tamsayıya dönüştürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| intResult | Int | 4 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

<a id="max" />

## <a name="max"></a>max
`max (arg1)`

En büyük değer dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi tamsayı veya virgülle ayrılmış tamsayı listesi |En yüksek değeri elde koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En büyük değeri koleksiyondan temsil eden bir tamsayı.

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
`min (arg1)`

En düşük değer dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dizi tamsayı veya virgülle ayrılmış tamsayı listesi |En düşük değer almak için koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En düşük değer koleksiyondan temsil eden bir tamsayı.

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

<a id="mod" />

## <a name="mod"></a>mod
`mod(operand1, operand2)`

İki sağlanan tamsayılar kullanılarak tamsayı bölme kalanı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |Int |Ayrılmış sayı. |
| operand2 |Evet |Int |Bölmek için kullanılan numarası 0 olamaz. |

### <a name="return-value"></a>Dönüş değeri
Kalan temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) başka bir parametre olarak bir parametre ayırma kalanı döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| modResult | Int | 1 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

İki sağlanan tamsayı çarpma döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |Int |Çarp ilk sayı. |
| operand2 |Evet |Int |Çarpılacağı ikinci sayı. |

### <a name="return-value"></a>Dönüş değeri

Çarpma temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) başka bir parametre olarak bir parametre çoğaltır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

<a id="sub" />

## <a name="sub"></a>Sub
`sub(operand1, operand2)`

İki sağlanan tamsayı çıkarma döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| operand1 |Evet |Int |Gelen çıkarılır sayı. |
| operand2 |Evet |Int |Çıkarılır sayı. |

### <a name="return-value"></a>Dönüş değeri
Çıkarma temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) başka bir parametre bir parametresinden çıkarır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| subResult | Int | 4 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

