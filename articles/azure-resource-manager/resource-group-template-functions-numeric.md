---
title: Azure Resource Manager şablonu işlevleri - sayısal | Microsoft Docs
description: Sayı ile iş için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
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
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: 5ed3a0a57dad61a5fe783790eba4cb89ce19c660
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66128656"
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Sayısal işlevler için Azure Resource Manager şablonları

Resource Manager, tamsayı ile çalışmak için aşağıdaki işlevleri sunar:

* [Ekleme](#add)
* [Copyındex](#copyindex)
* [div](#div)
* [kayan nokta](#float)
* [int](#int)
* [en fazla](#max)
* [Min](#min)
* [mod](#mod)
* [mul](#mul)
* [alt](#sub)

<a id="add" />

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add"></a>ekle
`add(operand1, operand2)`

İki sağlanan tam sayının toplamını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- | 
|İşlenen1 |Evet |int |Eklenecek ilk sayı. |
|İşlenen2 |Evet |int |Eklenecek ikinci sayı. |

### <a name="return-value"></a>Dönüş değeri

Parametreleri toplamını içeren bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/add.json) iki parametre ekler.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| addResult | Int | 8 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/add.json 
```

<a id="copyindex" />

## <a name="copyindex"></a>Copyındex
`copyIndex(loopName, offset)`

Bir yineleme döngüsü dizinini döndürür. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| loopName | Hayır | string | Yineleme almak için döngünün adı. |
| uzaklık |Hayır |int |Sıfır tabanlı yineleme değerine eklenecek sayı. |

### <a name="remarks"></a>Açıklamalar

Bu işlev her zaman ile kullanılan bir **kopyalama** nesne. İçin hiçbir değer sağlanmışsa **uzaklığı**, geçerli yineleme değeri döndürülür. Yineleme değeri sıfırdan başlatır. Yineleme döngüleri kaynakları veya değişkenleri tanımlarken kullanabilirsiniz.

**LoopName** özelliği Copyındex bir kaynak yineleme veya yinelemeye özelliği başvuru belirtmenize imkan tanır. İçin hiçbir değer sağlanmışsa **loopName**, geçerli kaynak türü yineleme kullanılır. İçin bir değer girin **loopName** bir özellikte dolaşırken. 
 
Nasıl kullanacağınız eksiksiz bir açıklaması için **Copyındex**, bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).

Kullanma örneği için **Copyındex** bir değişken tanımlama, bkz. [değişkenleri](resource-group-authoring-templates.md#variables).

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir kopyalama döngüsü ve adına eklenmiş dizin değeri gösterir. 

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

Geçerli yineleme dizinini temsil eden bir tamsayı.

<a id="div" />

## <a name="div"></a>div
`div(operand1, operand2)`

Tamsayı bölme iki sağlanan tamsayı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| İşlenen1 |Evet |int |Ayrılmış sayı. |
| İşlenen2 |Evet |int |Bölmek için kullanılan bir sayı. 0 olamaz. |

### <a name="return-value"></a>Dönüş değeri

Bölme işlemi temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/div.json) başka bir parametre olarak bir parametre böler.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| divResult | Int | 2 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/div.json 
```

<a id="float" />

## <a name="float"></a>float
`float(arg1)`

Kayan değer dönüştürür nokta sayısı. Mantıksal uygulama gibi bir uygulamaya özel parametreler geçerken yalnızca bu işlevi kullanın.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya tamsayı |Kayan dönüştürülecek değer nokta sayısı. |

### <a name="return-value"></a>Dönüş değeri
Kayan nokta sayısı.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kayan bir mantıksal uygulama için parametreleri geçirmek için nasıl kullanılacağını gösterir:

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

## <a name="int"></a>int
`int(valueToConvert)`

Belirtilen değeri bir tamsayıya dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| valueToConvert |Evet |dize veya tamsayı |Tamsayıya dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Tamsayıya dönüştürülen değer.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/int.json) kullanıcı tarafından sağlanan parametre değeri tamsayıya dönüştürür.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| intResult | Int | 4 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/int.json
```

<a id="max" />

## <a name="max"></a>en çok
`max (arg1)`

En yüksek değer bir sayı dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya virgülle ayrılmış tamsayı listesi dizisi |En yüksek değeri elde etmek için koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En büyük değeri koleksiyondan temsil eden bir tamsayı.

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

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/max.json
```

<a id="min" />

## <a name="min"></a>dk
`min (arg1)`

En düşük değer bir sayı dizisi veya virgülle ayrılmış tamsayı listesi döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |tamsayı veya virgülle ayrılmış tamsayı listesi dizisi |Minimum değeri alma koleksiyonu. |

### <a name="return-value"></a>Dönüş değeri

En düşük değer koleksiyondan temsil eden bir tamsayı.

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

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/min.json
```

<a id="mod" />

## <a name="mod"></a>mod
`mod(operand1, operand2)`

Sağlanan iki tamsayı kullanarak tamsayı bölme kalanını döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| İşlenen1 |Evet |int |Ayrılmış sayı. |
| İşlenen2 |Evet |int |Bölmek için kullanılan numarası 0 olamaz. |

### <a name="return-value"></a>Dönüş değeri
Kalan temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mod.json) başka bir parametre olarak bir parametre bölme kalanı döndürür.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| modResult | Int | 1 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mod.json
```

<a id="mul" />

## <a name="mul"></a>mul
`mul(operand1, operand2)`

Çarpma işlemi iki sağlanan tamsayı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| İşlenen1 |Evet |int |Çarpılacak ilk sayı. |
| İşlenen2 |Evet |int |Çarpılacak ikinci sayı. |

### <a name="return-value"></a>Dönüş değeri

Çarpma işlemi temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/mul.json) başka bir parametre olarak bir parametre çarpar.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| mulResult | Int | 15 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/mul.json
```

<a id="sub" />

## <a name="sub"></a>alt
`sub(operand1, operand2)`

Çıkarma iki sağlanan tamsayı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| İşlenen1 |Evet |int |Den çıkarılır numara. |
| İşlenen2 |Evet |int |Numara çıkarılır. |

### <a name="return-value"></a>Dönüş değeri
Çıkarma temsil eden bir tamsayı.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/sub.json) başka bir parametre bir parametreyi çıkartır.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| subResult | Int | 4 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/sub.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

