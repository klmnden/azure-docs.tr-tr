---
title: Azure Resource Manager şablonu işlevleri - kaynakları | Microsoft Docs
description: Kaynaklarla ilgili değerleri almak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklanmaktadır.
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
ms.date: 06/06/2018
ms.author: tomfitz
ms.openlocfilehash: f1271a6afba91cf75820f2e4b973b7cd42782449
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34824345"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için kaynak işlevleri

Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [listKeys](#listkeys)
* [listSecrets](#list)
* [Liste *](#list)
* [sağlayıcıları](#providers)
* [Başvuru](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [aboneliği](#subscription)

Parametreler, değişkenleri veya geçerli dağıtım değerlerini almak için bkz: [dağıtım değer işlevleri](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-listsecrets-and-list"></a>listKeys, listSecrets ve liste *
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`listSecrets(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Liste işlemi destekleyen herhangi bir kaynak türü için değerleri döndürür. En yaygın kullanımları şunlardır `listKeys` ve `listSecrets`. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya resourceIdentifier |Evet |dize |Kaynak için benzersiz tanımlayıcı. |
| apiVersion |Evet |dize |Kaynak çalışma zamanı durumunu API sürümü. Genellikle, biçiminde **yyyy-aa-gg**. |

### <a name="return-value"></a>Dönüş değeri

ListKeys döndürülen nesnesinden aşağıdaki biçime sahiptir:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Diğer liste işlevler farklı dönüş biçimlerine sahip. Bir işlev biçimi görmek için örnek şablonda gösterildiği gibi çıkışları bölümünde içerir. 

### <a name="remarks"></a>Açıklamalar

İle başlayan herhangi bir işlem **listesi** şablonunuzda bir işlevi olarak kullanılabilir. Yalnızca listKeys kullanılabilir işlemleri içerir, ancak ayrıca operations ister `list`, `listAdminKeys`, ve `listStatus`. Ancak, kullanamazsınız **listesi** istek gövdesindeki değerleri gerektiren işlemler. Örneğin, [listesi hesap SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) işlem gerektirir istek gövde parametreleri ister *signedExpiry*, dolayısıyla bir şablonda kullanamaz.

Hangi kaynak türlerinin bir listesini işlemi bulunduğunu belirlemek için aşağıdaki seçenekleriniz vardır:

* Görünüm [REST API işlemleri](/rest/api/) için bir kaynak sağlayıcısı ve listeleme işlemleri arayın. Örneğin, depolama hesaplarına sahip [listKeys işlemi](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Kullanım [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet'i. Aşağıdaki örnek, depolama hesapları için tüm listeleme işlemleri alır:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Yalnızca listesi işlemleri filtre uygulamak için aşağıdaki Azure CLI komutu kullanın:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Kaynak adı kullanarak kaynak belirtin veya [ResourceId işlevi](#resourceid). Bu işlev başvurulan kaynak dağıtır şablonda kullanırken, kaynak adı kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) nasıl birincil ve ikincil anahtarları depolama hesabındaki çıkışları bölümünde döndürüleceğini gösterir.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[listKeys(parameters('storageAccountName'), '2016-12-01')]"
      }
    }
}
``` 

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storageAccountName=<your-storage-account>
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storageAccountName <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>sağlayıcıları
`providers(providerNamespace, [resourceType])`

Bir kaynak sağlayıcısı ve desteklenen kaynak türleri hakkında bilgi döndürür. Bir kaynak türü belirtmezseniz, işlevi desteklenen tüm türler için kaynak sağlayıcısı döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| providerNamespace |Evet |dize |Namespace sağlayıcısı |
| Kaynak türü |Hayır |dize |Belirtilen ad alanı içindeki kaynak türü. |

### <a name="return-value"></a>Dönüş değeri

Desteklenen her türü aşağıdaki biçimde döndürülür: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Dizi döndürülen değerlerini sıralama garanti edilmez.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) sağlayıcı işlevinin nasıl kullanılacağını gösterir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "providerNamespace": {
            "type": "string"
        },
        "resourceType": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers(parameters('providerNamespace'), parameters('resourceType'))]",
            "type" : "object"
        }
    }
}
```

İçin **Microsoft.Web** kaynak sağlayıcısı ve **siteleri** kaynak türü, önceki örnekte döndürür nesneyi şu biçimde:

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json -providerNamespace Microsoft.Web -resourceType sites
```

<a id="reference" />

## <a name="reference"></a>Başvuru
`reference(resourceName or resourceIdentifier, [apiVersion], ['Full'])`

Bir kaynağın çalışma zamanı durumunu temsil eden bir nesne döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya resourceIdentifier |Evet |dize |Adı veya bir kaynak benzersiz tanıtıcısı. |
| apiVersion |Hayır |dize |Belirtilen kaynak API sürümü. Kaynak aynı şablonu içinde değil sağlandığında bu parametreyi dahil edin. Genellikle, biçiminde **yyyy-aa-gg**. |
| 'Tam' |Hayır |dize |Tam kaynak nesnesi döndürülmeyeceğini belirten değer. Belirtmezseniz, `'Full'`, yalnızca kaynak özellikleri nesnesinin döndürülür. Tam nesne konumu ve kaynak kimliği gibi değerler içerir. |

### <a name="return-value"></a>Dönüş değeri

Her kaynak türü başvurusu işlevi için farklı özellikleri döndürür. İşlev tek, önceden tanımlanmış biçimi döndürmez. Ayrıca, döndürülen değer tam nesne belirtmiş olmasına göre farklılık gösterir. Bir kaynak türü özelliklerini görmek için örnekte gösterildiği gibi çıkışları bölümünde nesnesi döndürür.

### <a name="remarks"></a>Açıklamalar

Başvuru işlevi bir çalışma zamanı durumu değerinden türeten ve bu nedenle değişkenler bölümünde kullanılamaz. Şablon çıktıları bölümünde kullanılabilir veya [bağlantılı şablon](resource-group-linked-templates.md#link-or-nest-a-template). Çıkış bölümünde kullanılamaz bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Bir iç içe geçmiş şablonunda dağıtılmış bir kaynak için değer döndürmek için iç içe geçmiş şablonunuzu bağlantılı şablona dönüştürebilirsiniz. 

Başvuru işlevi kullanarak, dolaylı olarak başvurulan kaynak içinde aynı şablonu sağlanır ve adıyla (kaynak kimliği değil) kaynağa başvuran bir kaynak üzerinde başka bir kaynak bağlıdır bildirin. Ayrıca dependsOn özelliğinin kullanılması gerekmez. Başvurulan kaynak dağıtımı tamamlanana kadar işlevi değerlendirilmez.

Özellik adları ve değerleri bir kaynak türü için görmek için çıkış bölümünde nesnesi döndüren bir şablon oluşturun. Bu tür mevcut bir kaynağı varsa, şablonunuzun herhangi yeni kaynaklar dağıtmadan nesnesini döndürür. 

Genellikle, kullandığınız **başvuru** blob uç noktası URI veya tam etki alanı adı gibi bir nesne belirli bir değeri döndürmek için işlevi.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

Kullanım `'Full'` özellikleri şema parçası olmayan kaynak değerlerini gerektiğinde. Örneğin, anahtar kasası erişim ilkelerini ayarlamak için bir sanal makine için kimlik özelliklerini al.

```json
{
  "type": "Microsoft.KeyVault/vaults",
  "properties": {
    "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
    "accessPolicies": [
      {
        "tenantId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.tenantId]",
        "objectId": "[reference(concat('Microsoft.Compute/virtualMachines/', variables('vmName')), '2017-03-30', 'Full').identity.principalId]",
        "permissions": {
          "keys": [
            "all"
          ],
          "secrets": [
            "all"
          ]
        }
      }
    ],
    ...
```

Yukarıdaki şablonu tam örnek için bkz: [anahtar kasası için Windows](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Benzer bir örnek için kullanılabilir [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) kaynak dağıtır ve bu kaynağa başvuruyor.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      },
      "fullReferenceOutput": {
        "type": "object",
        "value": "[reference(parameters('storageAccountName'), '2016-12-01', 'Full')]"
      }
    }
}
``` 

Önceki örnekte iki nesne döndürür. Özellikleri nesnesi şu biçimdedir:

```json
{
   "creationTime": "2017-10-09T18:55:40.5863736Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

Tam nesne aşağıdaki biçimdedir:

```json
{
  "apiVersion":"2016-12-01",
  "location":"southcentralus",
  "sku": {
    "name":"Standard_LRS",
    "tier":"Standard"
  },
  "tags":{},
  "kind":"Storage",
  "properties": {
    "creationTime":"2017-10-09T18:55:40.5863736Z",
    "primaryEndpoints": {
      "blob":"https://examplestorage.blob.core.windows.net/",
      "file":"https://examplestorage.file.core.windows.net/",
      "queue":"https://examplestorage.queue.core.windows.net/",
      "table":"https://examplestorage.table.core.windows.net/"
    },
    "primaryLocation":"southcentralus",
    "provisioningState":"Succeeded",
    "statusOfPrimary":"available",
    "supportsHttpsTrafficOnly":false
  },
  "subscriptionId":"<subscription-id>",
  "resourceGroupName":"functionexamplegroup",
  "resourceId":"Microsoft.Storage/storageAccounts/examplestorage",
  "referenceApiVersion":"2016-12-01",
  "condition":true,
  "isConditionTrue":true,
  "isTemplateResource":false,
  "isAction":false,
  "provisioningOperation":"Read"
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) bu şablonda dağıtılmamış bir depolama hesabı başvuruyor. Depolama hesabı, aynı kaynak grubunda zaten mevcut.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageAccountName=<your-storage-account>
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>kaynak grubu
`resourceGroup()`

Geçerli kaynak grubunda temsil eden bir nesne döndürür. 

### <a name="return-value"></a>Dönüş değeri

Döndürülen nesne aşağıdaki biçimdedir:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a>Açıklamalar

Bir ortak resourceGroup işlevinin kaynak grubu ile aynı konumda kaynakları oluşturmak için kullanılır. Aşağıdaki örnek, bir web sitesi için konum atamak için kaynak grubu konumu kullanır.

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) kaynak grubunun özelliklerini döndürür.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceGroupOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Önceki örnekte aşağıdaki biçimde bir nesne döndürür:

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Bir kaynak benzersiz tanımlayıcısını döndürür. Kaynak adı belirsiz ya da aynı şablonu içinde sağlanan olduğunda bu işlevi kullanın. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |dize (içinde GUID biçimi) |Geçerli aboneliğe varsayılan değerdir. Bir kaynağı başka bir abonelik almak gerektiğinde bu değeri belirtin. |
| resourceGroupName |Hayır |dize |Geçerli kaynak grubunda varsayılan değerdir. Bir kaynağı başka bir kaynak grubunda almanız gerektiğinde, bu değeri belirtin. |
| Kaynak türü |Evet |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Kaynak iç içe yerleştirilmiş ise sonraki kaynak adı kesimi. |

### <a name="return-value"></a>Dönüş değeri

Tanımlayıcı aşağıdaki biçimde verilir:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Belirttiğiniz parametre değerlerini kaynağın geçerli dağıtım aynı abonelik ve kaynak grubunda olmasına göre değişir.

Bir depolama hesabı aynı abonelikte ve kaynak grubu için kaynak kimliği almak için kullanın:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Bir depolama hesabı aynı abonelikte ancak farklı bir kaynak grubu için kaynak kimliği almak için kullanın:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Bir depolama hesabında farklı bir abonelik ve kaynak grubu için kaynak kimliği almak için kullanın:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Bir veritabanında farklı bir kaynak grubu için kaynak kimliği almak için kullanın:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Genellikle, bu işlev bir depolama hesabı veya sanal ağ bir alternatif kaynak grubunda kullanırken gerekir. Aşağıdaki örnek, bir dış kaynak grubundan bir kaynak kolayca nasıl kullanılabileceğini gösterir:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) kaynak grubunda bir depolama hesabı için kaynak kimliği döndürür:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('11111111-1111-1111-1111-111111111111', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

Varsayılan değerlerle önceki örnekten çıktısı şöyledir:

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| sameRGOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Dize | /Subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName |

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>aboneliği
`subscription()`

Geçerli dağıtım için abonelik ayrıntılarını döndürür. 

### <a name="return-value"></a>Dönüş değeri

İşlev aşağıdaki biçimde döndürür:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) abonelik işlevini çağırdı çıkışları bölümünde gösterir. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

Bu örnek şablonu Azure CLI ile dağıtmak için kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

Bu örnek şablon PowerShell ile dağıtmak için kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtmak](resource-group-template-deploy.md).

