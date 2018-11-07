---
title: Azure Resource Manager şablonu işlevleri - kaynakları | Microsoft Docs
description: Kaynaklarla ilgili değerleri almak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
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
ms.openlocfilehash: 6da2f7792df564ea3a41df37ab9b00574a205e5b
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219554"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için kaynak işlevleri

Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [listAccountSas](#list)
* [Listkeys'i](#listkeys)
* [listSecrets](#list)
* [Liste *](#list)
* [sağlayıcıları](#providers)
* [Başvuru](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [aboneliği](#subscription)

Parametreleri, değişkenleri veya geçerli dağıtım değerlerini almak için bkz. [dağıtım değeri işlevlerini](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listaccountsas-listkeys-listsecrets-and-list"></a>listAccountSas Listkeys'i, listSecrets ve liste *
`listAccountSas(resourceName or resourceIdentifier, apiVersion, functionValues)`

`listKeys(resourceName or resourceIdentifier, apiVersion)`

`listSecrets(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Listeleme işlemi destekleyen herhangi bir kaynak türü için değerleri döndürür. En yaygın kullanımları, `listKeys` ve `listSecrets`. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya Resourceıdentifier |Evet |dize |Kaynağın benzersiz tanımlayıcısı. |
| apiVersion |Evet |dize |Kaynak çalışma zamanı durumu API sürümü. Genellikle, biçiminde **yyyy-aa-gg**. |
| functionValues |Hayır |object | İşlev için değerleri içeren nesne. Yalnızca bu nesne için parametre değerlerini içeren bir nesne gibi almayı destekleyen işlevleri sağlar **listAccountSas** bir depolama hesabı üzerinde. | 

### <a name="return-value"></a>Dönüş değeri

Döndürülen nesneyi Listkeys'i aşağıdaki biçime sahiptir:

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

Diğer liste işlevleri farklı dönüş biçimlerine sahip. Bir işlev biçimini görmek için örnek şablonda görüldüğü gibi çıktıların bölüme ekleyin. 

### <a name="remarks"></a>Açıklamalar

İle başlayan herhangi bir işlem **listesi** şablonunuzda bir işlev olarak kullanılabilir. Yalnızca Listkeys'i kullanılabilir işlemleri içerir, ancak ayrıca operations ister `list`, `listAdminKeys`, ve `listStatus`. [Listesi hesap SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) işlem gerektiriyor istek gövde parametreleri ister *signedExpiry*. Bir şablonda bu işlevi kullanmak için bir nesne birlikte gövdede parametre değerlerini sağlayın.

Hangi kaynak türlerinin bir listesini işlemi olduğunu belirlemek için aşağıdaki seçenekleriniz vardır:

* Görünüm [REST API işlemleri](/rest/api/) bir kaynak sağlayıcısı ve liste işlemleri arayın. Örneğin, depolama hesaplarının [Listkeys'i işlemi](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).
* Kullanım [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet'i. Aşağıdaki örnek, depolama hesapları için tüm listeleme işlemleri alır:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Listeleme işlemleri filtrelemek için aşağıdaki Azure CLI komutunu kullanın:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

Kaynak adı kullanarak bir kaynak belirtin veya [ResourceId işlevi](#resourceid). Bu işlev, başvurulan kaynak dağıtan aynı şablonda kullanırken, kaynak adı kullanın.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) nasıl çıktılar bölümünü bir depolama hesabı birincil ve ikincil anahtarları döndürüleceğini gösterir. Ayrıca, depolama hesabı için bir SAS belirtecini döndürür. Bu belirteci almak için bir nesne listAccountSas işleve geçirir. Bu örnekte liste işlevlerini nasıl kullanacağınız göstermek için tasarlanmıştır. Genellikle, bir kaynak değerinde bir SAS belirteci kullanabilir yerine, bir çıkış değeri döndürür. Çıkış değerleri dağıtım geçmişini depolanır ve güvenli değildir. Bir süre sonu gelecekteki dağıtımın başarılı olması belirtmeniz gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagename": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus"
        },
        "accountSasProperties": {
            "type": "object",
            "defaultValue": {
                "signedServices": "b",
                "signedPermission": "r",
                "signedExpiry": "2018-08-20T11:00:00Z",
                "signedResourceTypes": "s"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-02-01",
            "name": "[parameters('storagename')]",
            "location": "[parameters('location')]",
            "type": "Microsoft.Storage/storageAccounts",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "StorageV2",
            "properties": {
                "supportsHttpsTrafficOnly": false,
                "accessTier": "Hot",
                "encryption": {
                    "services": {
                        "blob": {
                            "enabled": true
                        },
                        "file": {
                            "enabled": true
                        }
                    },
                    "keySource": "Microsoft.Storage"
                }
            },
            "dependsOn": []
        }
    ],
    "outputs": {
        "keys": {
            "type": "object",
            "value": "[listKeys(parameters('storagename'), '2018-02-01')]"
        },
        "accountSAS": {
            "type": "object",
            "value": "[listAccountSas(parameters('storagename'), '2018-02-01', parameters('accountSasProperties'))]"
        }
    }
}
``` 

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json --parameters storagename=<your-storage-account>
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/listkeys.json -storagename <your-storage-account>
```

<a id="providers" />

## <a name="providers"></a>sağlayıcıları
`providers(providerNamespace, [resourceType])`

Bir kaynak sağlayıcısı ve desteklenen kaynak türleri hakkında bilgi döndürür. Bir kaynak türü sağlamazsanız, işlev kaynak sağlayıcısı için desteklenen tüm türleri döndürür.

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| providerNamespace |Evet |dize |Namespace sağlayıcısı |
| Kaynak türü |Hayır |dize |Belirtilen ad alanı içinde kaynak türü. |

### <a name="return-value"></a>Dönüş değeri

Desteklenen her türü, şu biçimde döndürülür: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Döndürülen değerleri dizi sıralama garantisi yoktur.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/providers.json) sağlayıcısı işlevinin nasıl kullanılacağını gösterir:

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

İçin **Microsoft.Web** kaynak sağlayıcısı ve **siteleri** kaynak türü, önceki örnekte nesne döndürür şu biçimde:

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/providers.json --parameters providerNamespace=Microsoft.Web resourceType=sites
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

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
| resourceName veya Resourceıdentifier |Evet |dize |Adı veya bir kaynağın benzersiz tanımlayıcısı. |
| apiVersion |Hayır |dize |Belirtilen kaynak API sürümü. Kaynak, aynı şablonu içinde sağlanan değil, bu parametreyi dahil edin. Genellikle, biçiminde **yyyy-aa-gg**. |
| 'Full' |Hayır |dize |Tam kaynak nesnesini döndürülüp döndürülmeyeceğini belirleyen değer. Belirtmezseniz `'Full'`, yalnızca kaynak özellikleri nesne döndürülür. Tam nesne, konum ve kaynak kimliği gibi değerlerini içerir. |

### <a name="return-value"></a>Dönüş değeri

Her kaynak türü için başvuru işlevi farklı özellikleri döndürür. İşlevi, önceden tanımlanmış tek bir biçim döndürmüyor. Ayrıca, döndürülen değer tam nesne belirtilip göre farklılık gösterir. Bir kaynak türü için özellikleri görmek için örnekte gösterildiği gibi çıkış bölümü nesneyi döndürür.

### <a name="remarks"></a>Açıklamalar

Başvuru işlev çalışma zamanı durumu değerinden türetilir ve değişkenler bölümünde kullanılamaz. Şablon çıktıları bölümünde kullanılabilir veya [bağlı şablon](resource-group-linked-templates.md#link-or-nest-a-template). Çıkış bölümünde kullanılamaz bir [iç içe geçmiş şablon](resource-group-linked-templates.md#link-or-nest-a-template). Dönüş değerleri dağıtılan kaynağın içinde iç içe geçmiş bir şablon için iç içe geçmiş şablon bağlantılı şablona dönüştürebilirsiniz. 

Başvuru işlevini kullanarak, aynı şablonu içinde başvurulan kaynak sağlandıktan ve kaynağa adıyla (kaynak kimliği değil) başvurun bir kaynak başka bir kaynaktaki bağlıdır örtük olarak bildirdiğiniz. Ayrıca dependsOn özelliği kullanmanız gerekmez. Başvurulan kaynak dağıtımı tamamlanana kadar işlevi değerlendirilmez.

Özellik adlarını ve değerlerini bir kaynak türü için görmek için çıkışları bölümünde nesnesi döndüren bir şablon oluşturun. Mevcut bir kaynak türü varsa, tüm yeni kaynaklar dağıtmadan şablonunuzu nesneyi döndürür. 

Genellikle, kullandığınız **başvuru** blob uç noktası URI'si veya tam etki alanı adı gibi bir nesne belirli bir değer döndürüleceğini işlevi.

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

Kullanım `'Full'` özellikleri şemanın parçası olmayan kaynak değerleri gerektiğinde. Örneğin, anahtar kasası erişim ilkeleri ayarlamak için bir sanal makine için kimlik özelliklerini alır.

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

Yukarıdaki şablonu tam örnek için bkz: [Windows için Key Vault](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo08.msiWindowsToKeyvault.json). Benzer bir örnek için kullanılabilir [Linux](https://github.com/rjmax/AzureSaturday/blob/master/Demo02.ManagedServiceIdentity/demo07.msiLinuxToArm.json).

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/referencewithstorage.json) kaynak dağıtır ve o kaynağa başvuruyor.

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

Yukarıdaki örnekte, iki nesne döndürür. Özellikler nesnesinin aşağıdaki biçimdedir:

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json --parameters storageAccountName=<your-storage-account>
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/referencewithstorage.json -storageAccountName <your-storage-account>
```

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) bu şablonda dağıtılan olmayan bir depolama hesabına başvuruyor. Depolama hesabı aynı kaynak grubunda zaten mevcut.

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json --parameters storageAccountName=<your-storage-account>
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/reference.json -storageAccountName <your-storage-account>
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a>Kaynak grubu
`resourceGroup()`

Geçerli kaynak grubunu temsil eden bir nesne döndürür. 

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

Bir ortak resourceGroup işlevin kaynak grubu ile aynı konumda kaynakları oluşturmak için kullanılır. Aşağıdaki örnek, bir web sitesi için konum atamak için kaynak grubu konumu kullanır.

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

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourcegroup.json) kaynak grubunun özelliklerini döndürür.

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

Yukarıdaki örnekte, aşağıdaki biçimde bir nesne döndürür:

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourcegroup.json 
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Bir kaynağın benzersiz tanımlayıcısını döndürür. Kaynak adı belirsiz veya aynı şablon içinde sağlanan olduğunda bu işlevi kullanın. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| subscriptionId |Hayır |dize (içinde GUID biçimi) |Varsayılan değer geçerli bir aboneliktir. Başka bir Abonelikteki kaynak almak, ihtiyacınız olduğunda bu değeri belirtin. |
| resourceGroupName |Hayır |dize |Geçerli kaynak grubu varsayılan değerdir. Başka bir kaynak grubunda kaynak almak, ihtiyacınız olduğunda bu değeri belirtin. |
| Kaynak türü |Evet |dize |Kaynak sağlayıcısı ad alanı dahil olmak üzere kaynak türü. |
| resourceName1 |Evet |dize |Kaynağın adı. |
| resourceName2 |Hayır |dize |Kaynak iç içe sonraki kaynak adı kesimi. |

### <a name="return-value"></a>Dönüş değeri

Tanımlayıcısı şu biçimde döndürülür:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a>Açıklamalar

Belirttiğiniz parametre değerlerini kaynak geçerli dağıtım aynı abonelik ve kaynak grubunda olmasına göre değişir.

Bir depolama hesabı aynı abonelikte ve kaynak grubu için kaynak Kimliğini almak için kullanın:

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

Bir depolama hesabı aynı abonelikte ancak farklı bir kaynak grubu için kaynak Kimliğini almak için kullanın:

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Bir depolama hesabında farklı bir abonelikte ve kaynak grubu için kaynak Kimliğini almak için kullanın:

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

Farklı bir kaynak grubuna bir veritabanı için kaynak Kimliğini almak için kullanın:

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

Genellikle, bu işlev bir alternatif bir kaynak grubuna bir depolama hesabı veya sanal ağ kullanılırken kullanmanız gerekir. Aşağıdaki örnek, bir dış kaynak grubundan kaynak kolayca nasıl kullanılabileceğini gösterir:

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

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/resourceid.json) kaynak grubunda bir depolama hesabı kaynak kimliği döndürür:

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

Önceki örnekte varsayılan değerlere sahip çıktı.

| Ad | Tür | Değer |
| ---- | ---- | ----- |
| sameRGOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentRGOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| differentSubOutput | Dize | /Subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage |
| nestedResourceOutput | Dize | /Subscriptions/{Current-Sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/Servers/ServerName/Databases/databaseName |

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/resourceid.json 
```

<a id="subscription" />

## <a name="subscription"></a>aboneliği
`subscription()`

Geçerli dağıtım için abonelik ayrıntılarını döndürür. 

### <a name="return-value"></a>Dönüş değeri

İşlev aşağıdaki biçimde veri döndürür:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/subscription.json) abonelik işlevini çağırdı çıkışları bölümünde gösterir. 

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

Azure CLI ile bu örnek şablonu dağıtmak için şunu kullanın:

```azurecli-interactive
az group deployment create -g functionexamplegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json
```

PowerShell ile bu örnek şablonu dağıtmak için şunu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName functionexamplegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/functions/subscription.json 
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

