---
title: Azure Resource Manager şablonu işlevleri - dağıtım | Microsoft Docs
description: Dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2019
ms.author: tomfitz
ms.openlocfilehash: c5bd40741ec0fe047f98b4b4431819d90e188385
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66128672"
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Dağıtım işlevleri için Azure Resource Manager şablonları 

Resource Manager şablonu ve dağıtımıyla ilgili değerleri bölümlerden değerleri almak için aşağıdaki işlevleri sunar:

* [Dağıtım](#deployment)
* [parametreler](#parameters)
* [Değişkenleri](#variables)

Kaynakları, kaynak gruplarını veya abonelikleri değerleri almak için bkz: [kaynak işlevleri](resource-group-template-functions-resource.md).

<a id="deployment" />

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment"></a>deployment
`deployment()`

Geçerli dağıtım işlemiyle ilgili bilgi döndürür.

### <a name="return-value"></a>Dönüş değeri

Bu işlevin dağıtımı sırasında geçirilen nesneyi döndürür. Döndürülen nesne özellikleri dağıtım nesnesi bir bağlantı veya bir satır içi nesnesi olarak geçirilir göre farklılık gösterir. Ne zaman dağıtım nesnesi geçirilir satır içi, gibi kullanırken **- TemplateFile** parametresi yerel bir dosyaya işaret edecek şekilde, Azure PowerShell'de döndürülen nesne aşağıdaki biçim vardır:

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

Ne zaman nesne geçirilir, bir bağlantı gibi kullanırken **- TemplateUri** parametresini kullanarak uzak bir nesneye işaret nesnesi şu biçimde döndürülür: 

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

Olduğunda, [bir Azure aboneliğine dağıtma](deploy-to-subscription.md), bir kaynak grubu yerine dönüş nesneyi içeren bir `location` özelliği. Location özelliği, yerel bir şablon veya bir dış şablonu dağıtırken dahil edilir.

### <a name="remarks"></a>Açıklamalar

Deployment() ana şablon URİ'SİNDE tabanlı başka bir şablona bağlamak için kullanabilirsiniz.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

Portaldaki dağıtım geçmişinden bir şablonu yeniden dağıtın, şablonu yerel dosya olarak dağıtılır. `templateLink` Özellik dağıtım işlevinde döndürülen değil. Şablonunuzu dayanıyorsa `templateLink` başka bir şablon için bir bağlantı oluşturmak için portalı yeniden dağıtmak için kullanmayın. Bunun yerine, başlangıçta şablonu dağıtmak için kullandığınız komutları kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deployment.json) dağıtım nesnesi döndürür:

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

Yukarıdaki örnekte, şu nesne döndürür:

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/deployment.json
```

Dağıtım işlevi kullanan bir abonelik düzeyinde şablonu için bkz: [abonelik dağıtım işlevi](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/deploymentsubscription.json). İle birlikte dağıtılır `az deployment create` veya `New-AzDeployment` komutları.

<a id="parameters" />

## <a name="parameters"></a>parametreler
`parameters(parameterName)`

Bir parametre değeri döndürür. Belirtilen parametre adı şablon parametreleri bölümünde tanımlanmış olması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| parameterName |Evet |string |Döndürülecek parametrenin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen parametre değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle, kaynak değerlerini ayarlamak için parametreleri kullanın. Aşağıdaki örnek, web sitesinin adı dağıtımı sırasında geçirilen parametre değerine ayarlar.

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

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/parameters.json) parametreleri işlevi basitleştirilmiş bir kullanımını göstermektedir.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| stringOutput | String | Seçenek 1 |
| intOutput | Int | 1 |
| objectOutput | Object | {"bir": "a", "iki": "b"} |
| arrayOutput | Array | [1, 2, 3] |
| crossOutput | String | Seçenek 1 |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/parameters.json
```

<a id="variables" />

## <a name="variables"></a>Değişkenleri
`variables(variableName)`

Değişkenin değerini döndürür. Belirtilen değişken adı şablon değişkenleri bölümünde tanımlanmış olması gerekir.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| variableName |Evet |String |Döndürmek için değişkenin adı. |

### <a name="return-value"></a>Dönüş değeri

Belirtilen değişken değeri.

### <a name="remarks"></a>Açıklamalar

Genellikle karmaşık değerleri yalnızca bir kez oluşturarak, şablonunuzu basitleştirmek için değişkenleri kullanır. Aşağıdaki örnek, bir depolama hesabı için benzersiz bir ad oluşturur.

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

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/variables.json) farklı değişken değerleri döndürür.

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| exampleOutput1 | String | myVariable |
| exampleOutput2 | Array | [1, 2, 3, 4] |
| exampleOutput3 | String | myVariable |
| exampleOutput4 |  Object | {"property1": "value1", "Özellik2": "value2"} |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/variables.json
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Çeşitli şablonlar birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

