---
title: "Atama ve Azure kaynak ilkelerini yönetme | Microsoft Docs"
description: "Abonelikleriniz ve kaynak gruplarınız için Azure kaynak ilkeleri uygulamak ve kaynak ilkeleri görüntülemeyi açıklar."
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
ms.date: 09/19/2017
ms.author: tomfitz
ms.openlocfilehash: 64bdd6ed41e98079c8d4112e895aaeddcd629282
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="assign-and-manage-resource-policies"></a>Atama ve kaynak ilkelerini yönetme

Bir ilkeyi uygulamak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. İlke tanımları (Azure tarafından sağlanan yerleşik ilkeleri dahil) denetleyin gereksinimlerinizi karşılayan, aboneliğinizde zaten bir tane varsa görmek için.
2. Varsa, adını alın.
3. Bir mevcut değilse, JSON İlkesi kuralıyla tanımlamak ve bir ilke tanımı aboneliğinizde olarak ekleyin. Bu adım ilke ataması için kullanılabilir hale getirir, ancak kurallar aboneliğiniz için geçerli değildir.
4. Her iki durumda, ilke kapsamı (örneğin, bir abonelik veya kaynak grubu) atayın. İlke kuralları zorunlu tutulmaz.

Bu makalede, bir ilke tanımı oluşturun ve REST API'si, PowerShell veya Azure CLI aracılığıyla bir kapsam tanımın atamak için adımları odaklanır. İlkeler atamak için bkz: Portalı'nı kullanmayı tercih ederseniz, [atamak ve kaynak ilkelerini yönetmek için kullanın Azure portal](resource-manager-policy-portal.md). Bu makalede ilke tanımı oluşturmak için söz dizimi odaklanılmaktadır değil. İlke sözdizimi hakkında daha fazla bilgi için bkz: [kaynak ilkesine genel bakış](resource-manager-policy.md).

## <a name="exclusion-scopes"></a>Dışlama kapsamları

Bir ilkeyi atarken ne kapsam hariç tutabilirsiniz. Abonelik düzeyinde bir ilke atama ancak burada ilke uygulanmaz belirtmek için bu özelliği ilke atamalarını basitleştirir. Örneğin, aboneliğinizde, ağ altyapısı için amaçlanan bir kaynak grubu gerekir. Uygulama ekipleri, kaynakları diğer kaynak gruplarının dağıtın. Güvenlik sorunlarına yol açabilir ağ kaynaklarına oluşturmak için bu ekiplerle istemezsiniz. Ancak ağ kaynak grubunda ağ kaynaklarına izin ister. İlkeyi abonelik düzeyinde atamak ancak ağ kaynak gruptan çıkar. Birden çok alt kapsamlar belirtebilirsiniz.

```json
{
    "properties":{
        "policyDefinitionId":"<ID for policy definition>",
        "notScopes":[
            "/subscriptions/<subid>/resourceGroups/networkresourceGroup1"
        ]
    }
}
```

Bir dışlama kapsamını, atama belirtirseniz, kullanması **2017-06-01-Önizleme** API sürümü.

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>İlke tanımı oluşturun

İle bir ilke oluşturduğunuzda [ilke tanımları için REST API](/rest/api/resources/policydefinitions). REST API oluşturmak ve ilke tanımları silmek ve varolan tanımları hakkında bilgi almak etkinleştirir.

Bir ilke tanımı oluşturmak için çalıştırın:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

Aşağıdaki örneğe benzer bir istek gövdesi şunları içerir:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>İlke atama

İlke tanımı istenilen kapsamda uygulayabilirsiniz [ilke atamaları için REST API](/rest/api/resources/policyassignments). REST API oluşturmak ve ilke atamalarını silin ve varolan atamaları hakkında bilgi almak etkinleştirir.

Bir ilke ataması oluşturmak için çalıştırın:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

{İlkesi-atama} ilke ataması adıdır.

Aşağıdaki örneğe benzer bir istek gövdesi şunları içerir:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>İlkesini görüntüle
Bir ilkeyi almak üzere kullanmak [alma ilke tanımı](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) işlemi.

### <a name="get-aliases"></a>Diğer adlar Al
Diğer adlar REST API'si aracılığıyla alabilir:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

Aşağıdaki örnek, bir diğer ad tanımını gösterir. Gördüğünüz gibi bir özellik adı değişikliği olduğunda bile bir diğer ad yolları farklı API sürümlerde tanımlar. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

PowerShell örnekleriyle devam etmeden önce olduğundan emin olun [en son sürümü yüklü](/powershell/azure/install-azurerm-ps) Azure PowerShell. İlke parametreleri sürüm 3.6.0 eklendi. Önceki bir sürümü varsa, örnekleri parametresi bulunamıyor belirten bir hata döndürür.

### <a name="view-policy-definitions"></a>Görünüm ilke tanımları
Aboneliğinizdeki tüm ilke tanımları görmek için aşağıdaki komutu kullanın:

```powershell
Get-AzureRmPolicyDefinition
```

Yerleşik ilkeleri de dahil olmak üzere tüm kullanılabilir ilke tanımları döndürür. Her ilke şu biçimde verilir:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Bir ilke tanımı oluşturmak için devam etmeden önce yerleşik ilkelerine bakın. Gereksinim duyduğunuz limitleri geçerlidir yerleşik bir ilke bulursanız, bir ilke tanımı oluşturma atlayabilirsiniz. Bunun yerine, yerleşik ilkesini istenen kapsamı atayın.

### <a name="create-policy-definition"></a>İlke tanımı oluşturun
Kullanarak bir ilke tanımı oluşturabilirsiniz `New-AzureRmPolicyDefinition` cmdlet'i.

Bir dosyadan bir ilke tanımı oluşturmak için dosya yolu geçirin. Dış bir dosya için kullanın:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -DisplayName "Deny cool access tiering for storage" `
    -Policy 'https://raw.githubusercontent.com/Azure/azure-policy-samples/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json'
```

Yerel bir dosya için kullanın:

```powershell
$definition = New-AzureRmPolicyDefinition `
    -Name denyCoolTiering `
    -Description "Deny cool access tiering for storage" `
    -Policy "c:\policies\coolAccessTier.json"
```

Bir ilke tanımı sahip bir satır içi kuralı oluşturmak için kullanın:

```powershell
$definition = New-AzureRmPolicyDefinition -Name denyCoolTiering -Description "Deny cool access tiering for storage" -Policy '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'
```            

Çıktı depolanan bir `$definition` ilke ataması sırasında kullanılan nesne. 

Aşağıdaki örnek, parametreleri içeren bir ilke tanımı oluşturur:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>İlke atama

Kullanarak istenen kapsamı ilkesi uygulamak `New-AzureRmPolicyAssignment` cmdlet'i. Aşağıdaki örnek, bir kaynak grubu için ilke atar.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

Parametreler gerektiren bir ilke atamak için oluşturmak ve bu değerleri nesne. Aşağıdaki örnek, bir yerleşik ilkesini alır ve parametre değerleri geçirir:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Görünüm ilke ataması

Belirli bir ilke ataması almak için kullanın:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

Bir ilke tanımı için ilke kuralı görüntülemek için kullanın:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>İlke atamasını Kaldır 

Bir ilke atamasını kaldırmak için kullanın:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Görünüm ilke tanımları
Aboneliğinizdeki tüm ilke tanımları görmek için aşağıdaki komutu kullanın:

```azurecli
az policy definition list
```

Yerleşik ilkeleri de dahil olmak üzere tüm kullanılabilir ilke tanımları döndürür. Her ilke şu biçimde verilir:

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Bir ilke tanımı oluşturmak için devam etmeden önce yerleşik ilkelerine bakın. Gereksinim duyduğunuz limitleri geçerlidir yerleşik bir ilke bulursanız, bir ilke tanımı oluşturma atlayabilirsiniz. Bunun yerine, yerleşik ilkesini istenen kapsamı atayın.

### <a name="create-policy-definition"></a>İlke tanımı oluşturun

İlke tanımı komutu ile Azure CLI kullanarak bir ilke tanımı oluşturabilirsiniz.

Bir ilke tanımı sahip bir satır içi kuralı oluşturmak için kullanın:

```azurecli
az policy definition create --name denyCoolTiering --description "Deny cool access tiering for storage" --rules '{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}'    
```

### <a name="assign-policy"></a>İlke atama

İlke ataması komutunu kullanarak, istenen kapsamı ilkesi uygulayabilir. Aşağıdaki örnek, bir kaynak grubu için bir ilke atar.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Görünüm ilke ataması

Bir ilke atamasını görüntülemek için ilke ataması adı ve kapsam sağlar:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>İlke atamasını Kaldır 

Bir ilke atamasını kaldırmak için kullanın:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Sonraki adımlar
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

