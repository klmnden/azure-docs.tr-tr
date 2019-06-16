---
title: Azure Resource Manager şablonu işlevleri - kaynakları | Microsoft Docs
description: Kaynaklarla ilgili değerleri almak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 05/21/2019
ms.author: tomfitz
ms.openlocfilehash: dcad4b988f37d46a0b843fbf905e18011bc4e313
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65990753"
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Azure Resource Manager şablonları için kaynak işlevleri

Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [Liste *](#list)
* [sağlayıcıları](#providers)
* [Başvuru](#reference)
* [resourceGroup](#resourcegroup)
* [resourceId](#resourceid)
* [aboneliği](#subscription)

Parametreleri, değişkenleri veya geçerli dağıtım değerlerini almak için bkz. [dağıtım değeri işlevlerini](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="list"></a>Liste *

`list{Value}(resourceName or resourceIdentifier, apiVersion, functionValues)`

Bu işlev sözdizimi listeleme işlemleri adına göre değişir. Her uygulama, bir liste işlemi destekleyen kaynak türünün değerlerini döndürür. İşlem adı ile başlamalıdır `list`. Bazı yaygın kullanımları, `listKeys` ve `listSecrets`. 

### <a name="parameters"></a>Parametreler

| Parametre | Gerekli | Tür | Açıklama |
|:--- |:--- |:--- |:--- |
| resourceName veya Resourceıdentifier |Evet |dize |Kaynağın benzersiz tanımlayıcısı. |
| apiVersion |Evet |dize |Kaynak çalışma zamanı durumu API sürümü. Genellikle, biçiminde **yyyy-aa-gg**. |
| functionValues |Hayır |object | İşlev için değerleri içeren nesne. Yalnızca bu nesne için parametre değerlerini içeren bir nesne gibi almayı destekleyen işlevleri sağlar **listAccountSas** bir depolama hesabı üzerinde. Bu makalede işlevi değerler geçirerek bir örnek gösterilmektedir. | 

### <a name="implementations"></a>Uygulamalar

Olası listesini * kullanımları aşağıdaki tabloda gösterilmektedir.

| Kaynak türü | İşlev adı |
| ------------- | ------------- |
| Microsoft.AnalysisServices/servers | [listGatewayStatus](/rest/api/analysisservices/servers/listgatewaystatus) |
| Microsoft.Automation/automationAccounts | [Listkeys'i](/rest/api/automation/keys/listbyautomationaccount) |
| Microsoft.Batch/batchAccounts | [listkeys'i](/rest/api/batchmanagement/batchaccount/getkeys) |
| Microsoft.BatchAI/workspaces/experiments/jobs | [listoutputfiles](/rest/api/batchai/jobs/listoutputfiles) |
| Microsoft.Cache/redis | [Listkeys'i](/rest/api/redis/redis/listkeys) |
| Microsoft.CognitiveServices/accounts | [Listkeys'i](/rest/api/cognitiveservices/accountmanagement/accounts/listkeys) |
| Microsoft.ContainerRegistry/registries | [listBuildSourceUploadUrl](/rest/api/containerregistry/registries%20(tasks)/getbuildsourceuploadurl) |
| Microsoft.ContainerRegistry/registries | [listCredentials](/rest/api/containerregistry/registries/listcredentials) |
| Microsoft.ContainerRegistry/registries | [listPolicies](/rest/api/containerregistry/registries/listpolicies) |
| Microsoft.ContainerRegistry/registries | [listUsages](/rest/api/containerregistry/registries/listusages) |
| Microsoft.ContainerRegistry/registries/webhooks | [listEvents](/rest/api/containerregistry/webhooks/listevents) |
| Microsoft.ContainerService/managedClusters | [listClusterAdminCredential](/rest/api/aks/managedclusters/listclusteradmincredentials) |
| Microsoft.ContainerService/managedClusters | [listClusterUserCredential](/rest/api/aks/managedclusters/listclusterusercredentials) |
| Microsoft.DataFactory/datafactories/gateways | listauthkeys |
| Microsoft.DataFactory/factories/integrationruntimes | [listauthkeys](/rest/api/datafactory/integrationruntimes/listauthkeys) |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers | [listSasTokens](/rest/api/datalakeanalytics/storageaccounts/listsastokens) |
| Microsoft.Devices/ıothubs | [listkeys'i](/rest/api/iothub/iothubresource/listkeys) |
| Microsoft.Devices/provisioningServices/keys | [listkeys'i](/rest/api/iot-dps/iotdpsresource/listkeysforkeyname) |
| Microsoft.Devices/provisioningServices | [listkeys'i](/rest/api/iot-dps/iotdpsresource/listkeys) |
| Microsoft.DevTestLab/labs | [ListVhds](/rest/api/dtl/labs/listvhds) |
| Microsoft.DevTestLab/labs/schedules | [ListApplicable](/rest/api/dtl/schedules/listapplicable) |
| Microsoft.DevTestLab/labs/users/serviceFabrics | [ListApplicableSchedules](/rest/api/dtl/servicefabrics/listapplicableschedules) |
| Microsoft.DevTestLab/labs/virtualMachines | [ListApplicableSchedules](/rest/api/dtl/virtualmachines/listapplicableschedules) |
| Microsoft.DocumentDB/databaseAccounts | [listConnectionStrings](/rest/api/cosmos-db-resource-provider/databaseaccounts/listconnectionstrings) |
| Microsoft.DocumentDB/databaseAccounts | [Listkeys'i](/rest/api/cosmos-db-resource-provider/databaseaccounts/listkeys) |
| Microsoft.DomainRegistration | [listDomainRecommendations](/rest/api/appservice/domains/listrecommendations) |
| Microsoft.DomainRegistration/topLevelDomains | [listAgreements](/rest/api/appservice/topleveldomains/listagreements) |
| Microsoft.EventGrid/topics | [Listkeys'i](/rest/api/eventgrid/topics/listsharedaccesskeys) |
| Microsoft.EventHub/namespaces/authorizationRules | [listkeys'i](/rest/api/eventhub/namespaces/listkeys) |
| Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys'i](/rest/api/eventhub/disasterrecoveryconfigs/listkeys) |
| Microsoft.EventHub/namespaces/eventhubs/authorizationRules | [listkeys'i](/rest/api/eventhub/eventhubs/listkeys) |
| Microsoft.ImportExport/jobs | [listBitLockerKeys](/rest/api/storageimportexport/bitlockerkeys/list) |
| Microsoft.Logic/integrationAccounts/agreements | [listContentCallbackUrl](/rest/api/logic/agreements/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/assemblies | [listContentCallbackUrl](/rest/api/logic/integrationaccountassemblies/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listCallbackUrl](/rest/api/logic/integrationaccounts/getcallbackurl) |
| Microsoft.Logic/integrationAccounts | [listKeyVaultKeys](/rest/api/logic/integrationaccounts/listkeyvaultkeys) |
| Microsoft.Logic/integrationAccounts/maps | [listContentCallbackUrl](/rest/api/logic/maps/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/partners | [listContentCallbackUrl](/rest/api/logic/partners/listcontentcallbackurl) |
| Microsoft.Logic/integrationAccounts/schemas | [listContentCallbackUrl](/rest/api/logic/schemas/listcontentcallbackurl) |
| Microsoft.Logic/workflows | [listCallbackUrl](/rest/api/logic/workflows/listcallbackurl) |
| Microsoft.Logic/workflows | [listSwagger](/rest/api/logic/workflows/listswagger) |
| Microsoft.MachineLearning/webServices | [listkeys'i](/rest/api/machinelearning/webservices/listkeys) |
| Microsoft.MachineLearning/Workspaces | listworkspacekeys |
| Microsoft.MachineLearningServices/workspaces/computes | Listkeys'i |
| Microsoft.MachineLearningServices/workspaces | Listkeys'i |
| Microsoft.Maps/accounts | [Listkeys'i](/rest/api/maps-management/accounts/listkeys) |
| Microsoft.Media/mediaservices/assets | [listContainerSas](/rest/api/media/assets/listcontainersas) |
| Microsoft.Media/mediaservices/assets | [listStreamingLocators](/rest/api/media/assets/liststreaminglocators) |
| Microsoft.Media/mediaservices/streamingLocators | [listContentKeys](/rest/api/media/streaminglocators/listcontentkeys) |
| Microsoft.Media/mediaservices/streamingLocators | [listPaths](/rest/api/media/streaminglocators/listpaths) |
| Microsoft.NotificationHubs/Namespaces/authorizationRules | [listkeys'i](/rest/api/notificationhubs/namespaces/listkeys) |
| Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules | [listkeys'i](/rest/api/notificationhubs/notificationhubs/listkeys) |
| Microsoft.OperationalInsights/workspaces | [Listkeys'i](/rest/api/loganalytics/workspaces%202015-03-20/listkeys) |
| Microsoft.Relay/namespaces/authorizationRules | [listkeys'i](/rest/api/relay/namespaces/listkeys) |
| Microsoft.Relay/namespaces/HybridConnections/authorizationRules | [listkeys'i](/rest/api/relay/hybridconnections/listkeys) |
| Microsoft.Relay/namespaces/WcfRelays/authorizationRules | [listkeys'i](/rest/api/relay/wcfrelays/listkeys) |
| Microsoft.Search/searchServices | [listAdminKeys](/rest/api/searchmanagement/adminkeys/get) |
| Microsoft.Search/searchServices | [listQueryKeys](/rest/api/searchmanagement/querykeys/listbysearchservice) |
| Microsoft.ServiceBus/namespaces/authorizationRules | [listkeys'i](/rest/api/servicebus/namespaces/listkeys) |
| Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules | [listkeys'i](/rest/api/servicebus/disasterrecoveryconfigs/listkeys) |
| Microsoft.ServiceBus/namespaces/queues/authorizationRules | [listkeys'i](/rest/api/servicebus/queues/listkeys) |
| Microsoft.ServiceBus/namespaces/topics/authorizationRules | [listkeys'i](/rest/api/servicebus/topics/listkeys) |
| Microsoft.SignalRService/SignalR | [listkeys'i](/rest/api/signalr/signalr/listkeys) |
| Microsoft.Storage/storageAccounts | [listAccountSas](/rest/api/storagerp/storageaccounts/listaccountsas) |
| Microsoft.Storage/storageAccounts | [listkeys'i](/rest/api/storagerp/storageaccounts/listkeys) |
| Microsoft.Storage/storageAccounts | [listServiceSas](/rest/api/storagerp/storageaccounts/listservicesas) |
| Microsoft.StorSimple/managers/devices | [listFailoverSets](/rest/api/storsimple/devices/listfailoversets) |
| Microsoft.StorSimple/managers/devices | [listFailoverTargets](/rest/api/storsimple/devices/listfailovertargets) |
| Microsoft.StorSimple/managers | [listActivationKey](/rest/api/storsimple/managers/getactivationkey) |
| Microsoft.StorSimple/managers | [listPublicEncryptionKey](/rest/api/storsimple/managers/getpublicencryptionkey) |
| Microsoft.Web/connectionGateways | ListStatus |
| Microsoft.Web/Connections | listconsentlinks |
| Microsoft.Web/customApis | listWsdlInterfaces |
| Microsoft.Web/Locations | listwsdlinterfaces |
| Microsoft.Web/Sites/Functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecrets) |
| Microsoft.Web/Sites/hybridconnectionnamespaces/relays | [listkeys'i](/rest/api/appservice/webapps/listhybridconnectionkeys) |
| Microsoft.Web/Sites | [listsyncfunctiontriggerstatus](/rest/api/appservice/webapps/listsyncfunctiontriggers) |
| Microsoft.Web/Sites/slots/Functions | [listsecrets](/rest/api/appservice/webapps/listfunctionsecretsslot) |

Hangi kaynak türlerinin bir listesini işlemi olduğunu belirlemek için aşağıdaki seçenekleriniz vardır:

* Görünüm [REST API işlemleri](/rest/api/) bir kaynak sağlayıcısı ve liste işlemleri arayın. Örneğin, depolama hesaplarının [Listkeys'i işlemi](/rest/api/storagerp/storageaccounts).
* Kullanım [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) PowerShell cmdlet'i. Aşağıdaki örnek, depolama hesapları için tüm listeleme işlemleri alır:

  ```powershell
  Get-AzProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Listeleme işlemleri filtrelemek için aşağıdaki Azure CLI komutunu kullanın:

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

### <a name="return-value"></a>Dönüş değeri

Kullandığınız listesi işlevi tarafından döndürülen nesne değişir. Örneğin, bir depolama hesabı için Listkeys'i aşağıdaki biçimde veri döndürür:

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

Kaynak adı kullanarak bir kaynak belirtin veya [ResourceId işlevi](#resourceid). Başvurulan kaynak dağıtan aynı şablonda bir liste işlevini kullanarak, kaynak adı kullanın.

Kullanıyorsanız bir **listesi** koşullu olarak dağıtılan, kaynak işlevi işlevinde, kaynağın dağıtılan değilse bile değerlendirilir. Varsa bir hata alıyorsunuz **listesi** işlevi, var olmayan bir kaynağa başvuruyor. Kullanım **varsa** işlevi yalnızca kaynak dağıtıldığında değerlendirme emin olmak için işlevi. Bkz: [varsa işlevi](resource-group-template-functions-logical.md#if) kullanıyorsa örnek şablonu ve koşullu olarak dağıtılan bir kaynak listesi.

### <a name="example"></a>Örnek

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/listkeys.json) nasıl çıktılar bölümünü bir depolama hesabı birincil ve ikincil anahtarları döndürüleceğini gösterir. Ayrıca, depolama hesabı için bir SAS belirtecini döndürür. 

SAS belirteci almak için bir nesne için süre sonu geçirin. Süre sonu gelecekteki olması gerekir. Bu örnekte liste işlevlerini nasıl kullanacağınız göstermek için tasarlanmıştır. Genellikle, bir kaynak değerinde bir SAS belirteci kullanabilir yerine, bir çıkış değeri döndürür. Çıkış değerleri dağıtım geçmişini depolanır ve güvenli değildir.

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

Başvuru işlevi, daha önceden dağıtılan bir kaynak ya da geçerli şablon dağıtılan bir kaynak çalışma zamanı durumunu alır. Bu makalede her iki senaryo için örnekler gösterilmektedir. Geçerli şablon kaynağında başvuran bir parametre olarak yalnızca kaynak adı belirtin. Daha önceden dağıtılan bir kaynağa başvuran, kaynak Kimliğine ve bir API sürümü için kaynak sağlar. Kaynağınızda için geçerli API sürümlerini belirlemek [şablon başvurusu](/azure/templates/).

Başvuru işlevi yalnızca bir kaynak tanımı özelliklerini ve bir şablonu veya dağıtım çıktılar bölümünü kullanılabilir. İle kullanıldığında [özelliği yineleme](resource-group-create-multiple.md#property-iteration), başvuru işlev için kullanabileceğiniz `input` ifade kaynak özelliğine atanan olduğundan. İle kullanamazsınız `count` olduğundan başvuru işlevi daha çözümlenir önce sayısı belirlenmesi gerekir.

Başvuru işlevini kullanarak, aynı şablonu içinde başvurulan kaynak sağlandıktan ve kaynağa adıyla (kaynak kimliği değil) başvurun bir kaynak başka bir kaynaktaki bağlıdır örtük olarak bildirdiğiniz. Ayrıca dependsOn özelliği kullanmanız gerekmez. Başvurulan kaynak dağıtımı tamamlanana kadar işlevi değerlendirilmez.

Kullanırsanız **başvuru** koşullu olarak dağıtılan, kaynak işlevi işlevinde, kaynağın dağıtılan değilse bile değerlendirilir.  Varsa bir hata alıyorsunuz **başvuru** işlevi, var olmayan bir kaynağa başvuruyor. Kullanım **varsa** işlevi yalnızca kaynak dağıtıldığında değerlendirme emin olmak için işlevi. Bkz: [varsa işlevi](resource-group-template-functions-logical.md#if) kullanıyorsa örnek şablonu ve koşullu olarak dağıtılan bir kaynak başvurusu.

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
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Aşağıdaki [örnek şablonu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/reference.json) bu şablonda dağıtılan olmayan bir depolama hesabına başvuruyor. Depolama hesabı aynı abonelik içinde zaten var.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageResourceGroup": {
            "type": "string"
        },
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2018-07-01')]",
            "type": "object"
        }
    }
}
```

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

`resourceGroup()` Olan bir şablon işlevi kullanılamaz [abonelik düzeyinde dağıtılan](deploy-to-subscription.md). Yalnızca bir kaynak grubuna dağıtıldığını şablonlarındaki de kullanılabilir.

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

İle kullanıldığında bir [abonelik düzeyinde dağıtım](deploy-to-subscription.md), `resourceId()` işlevi yalnızca o seviyede dağıtılan kaynakların Kimliğini alın. Örneğin, bir ilke tanımı veya rol tanımı kimliği, ancak bir depolama hesabı kimliği değil alabilirsiniz. Bir kaynak grubu için dağıtımlar için bunun tersi de geçerlidir. Abonelik düzeyinde dağıtılan kaynakların kaynak kimliği alınamıyor.

Belirttiğiniz parametre değerlerini kaynak geçerli dağıtım aynı abonelik ve kaynak grubunda olmasına göre değişir. Bir depolama hesabı aynı abonelikte ve kaynak grubu için kaynak Kimliğini almak için kullanın:

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

Abonelik kapsamında dağıtırken bir abonelik düzeyinde kaynak kaynak Kimliğini almak için kullanın:

```json
"[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
```

Genellikle, bu işlev bir alternatif bir kaynak grubuna bir depolama hesabı veya sanal ağ kullanılırken kullanmanız gerekir. Aşağıdaki örnek, bir dış kaynak grubundan kaynak kolayca nasıl kullanılabileceğini gösterir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
      "subnet1Ref": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnet1Name'))]"
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

## <a name="next-steps"></a>Sonraki adımlar

* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md).
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

