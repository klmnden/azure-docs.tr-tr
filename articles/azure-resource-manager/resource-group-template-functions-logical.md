---
title: Azure Resource Manager şablonu işlevleri - mantıksal | Microsoft Docs
description: Mantıksal değerleri belirlemek için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/15/2019
ms.author: tomfitz
ms.openlocfilehash: 2ccdd337d5c01a0ac0253fe1d1e131fa4e6d51a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60782999"
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için mantıksal işlevler

Resource Manager şablonlarınızı karşılaştırmaları yapmak için çeşitli işlevler sunar.

* [ve](#and)
* [bool](#bool)
* [Eğer](#if)
* [değil](#not)
* [veya](#or)

## <a name="and"></a>ve

`and(arg1, arg2, ...)`

Tüm parametre değerlerini true olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Denetlenecek ilk değer olup olmadığını geçerlidir. |
| arg2 |Evet |boole |Kontrol edilecek ikinci değerle olmadığını geçerlidir. |
| Ek bağımsız değişkenler |Hayır |boole |Denetlemek için ek bağımsız değişkenler olmadığını doğrudur. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** tüm değerleri ise true; Aksi takdirde **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal İşlevler'i kullanmayı gösterir.

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

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="bool"></a>bool

`bool(arg1)`

Parametresi bir boolean değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |dize veya tamsayı |Bir boolean değerine dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri
Dönüştürülen değerin bir Boole değeri.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) bir dize veya tamsayı ile bool kullanmayı gösterir.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Bağlı değeri döndürür, bir koşul true veya false.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| koşul |Evet |boole |True veya false olup olmadığını denetlemek için değer. |
| trueValue |Evet | dize, int, nesne veya dizi |Koşul true olduğunda döndürülecek değer. |
| false değerini |Evet | dize, int, nesne veya dizi |Koşul false ise döndürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

İkinci parametresi ilk parametre olduğunda döndürür **True**; Aksi takdirde, üçüncü parametresinin döndürür.

### <a name="remarks"></a>Açıklamalar

Koşul olduğunda **True**, yalnızca true değeri değerlendirilir. Koşul olduğunda **False**, yalnızca false değeri değerlendirilir. İle **varsa** işlevi, yalnızca geçerli koşullu ifadeleri içerebilir. Örneğin, bir koşul altında ancak başka bir koşul altında var olan bir kaynağa başvuruda bulunabilir. Aşağıdaki bölümde, koşullu ifadeleri değerlendirme örneği gösterilir.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) nasıl kullanılacağını gösterir `if` işlevi.

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
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| yesOutput | String | evet |
| noOutput | String | hayır |
| objectOutput | Object | {"test": "value1"} |

Aşağıdaki [örnek şablonu](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) bu işlevi yalnızca koşullu olarak geçerli olan ifadeleri ile kullanma işlemi gösterilmektedir.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>değil

`not(arg1)`

Boolean değerini ters değerine dönüştürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Dönüştürülecek değer. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** parametre olduğunda **False**. Döndürür **False** parametre olduğunda **True**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal İşlevler'i kullanmayı gösterir.

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

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) kullanan **değil** ile [eşittir](resource-group-template-functions-comparison.md#equals).

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

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

## <a name="or"></a>or

`or(arg1, arg2, ...)`

Herhangi bir parametre değer true olup olmadığını denetler.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| arg1 |Evet |boole |Denetlenecek ilk değer olup olmadığını geçerlidir. |
| arg2 |Evet |boole |Kontrol edilecek ikinci değerle olmadığını geçerlidir. |
| Ek bağımsız değişkenler |Hayır |boole |Denetlemek için ek bağımsız değişkenler olmadığını doğrudur. |

### <a name="return-value"></a>Dönüş değeri

Döndürür **True** herhangi bir değeri varsa true; Aksi takdirde **False**.

### <a name="examples"></a>Örnekler

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) mantıksal İşlevler'i kullanmayı gösterir.

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

Önceki örnekte çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

