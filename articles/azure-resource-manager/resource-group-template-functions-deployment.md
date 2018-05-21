---
title: Azure Resource Manager şablonu işlevleri - dağıtım | Microsoft Docs
description: Dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklanmaktadır.
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
ms.openlocfilehash: 725bc41f96359d4bf0d9d570f73f91dba5da2cab
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için dağıtım işlevleri 

Resource Manager şablonu ve dağıtımıyla ilgili değerleri bölümlerden değerleri almak için aşağıdaki işlevleri sunar:

* [Dağıtım](#deployment)
* [parametreler](#parameters)
* [değişkenleri](#variables)

Kaynakları, kaynak grupları ya da abonelik değerlerini almak için bkz: [kaynak işlevlerini](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>dağıtım
`deployment()`

Geçerli dağıtım işlemiyle ilgili bilgi döndürür.

### <a name="return-value"></a>Dönüş değeri

Bu işlev, dağıtımı sırasında geçirilen nesneyi döndürür. Döndürülen nesne özelliklerinde dağıtım nesnesi bir bağlantı veya bir satır içi nesnesi olarak geçirilir göre farklılık gösterir. Ne zaman dağıtım nesnesi geçirilir satır içi, gibi kullanırken **- TemplateFile** yerel bir dosyaya işaret edecek şekilde Azure PowerShell parametresi, döndürülen nesne aşağıdaki biçim sahiptir:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Ne zaman nesne geçirilen bir bağlantı olarak gibi kullanırken **- TemplateUri** parametresini kullanarak uzak bir nesneye işaret nesnesi şu biçimde döndürülür: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a>Açıklamalar

Deployment() üst şablon URI'sini bağlı başka bir şablonu bağlamak için kullanabilirsiniz.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) dağıtım nesnesi döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

Önceki örnekte aşağıdaki nesneyi döndürür:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

<a id="parameters" />

## <a name="parameters"></a>parametreler
`parameters(parameterName)`

Bir parametre değeri döndürür. Belirtilen parametre adı Şablon Parametreler bölümünde tanımlanmış olması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| parameterName |Evet |dize |Döndürülecek parametresinin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen parametre değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle, parametreleri, kaynak değerlerini ayarlamak için kullanın. Aşağıdaki örnek, dağıtımı sırasında geçirilen parametre değeri için web sitesinin adını ayarlar.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) parametreleri işlevi Basitleştirilmiş kullanımını göstermektedir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | Dize | seçenek 1 |
| intOutput | Int | 1 |
| objectOutput | Nesne | {"bir": "a", "iki": "b"} |
| arrayOutput | Dizi | [1, 2, 3] |
| crossOutput | Dize | seçenek 1 |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

<a id="variables" />

## <a name="variables"></a>değişkenleri
`variables(variableName)`

Değişkenin değerini döndürür. Belirtilen değişken adı şablon değişkenleri bölümünde tanımlanmış olması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| variableName |Evet |Dize |Döndürülecek değişkeninin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen değişken değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle, karmaşık değerler yalnızca bir kez oluşturarak şablonunuzu basitleştirmek için değişkenleri kullanın. Aşağıdaki örnek, bir depolama hesabı için benzersiz bir ad oluşturur.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) farklı değişken değerleri döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| exampleOutput1 | Dize | myVariable |
| exampleOutput2 | Dizi | [1, 2, 3, 4] |
| exampleOutput3 | Dize | myVariable |
| exampleOutput4 |  Nesne | {"property1": "value1", "property2": "value2"} |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

