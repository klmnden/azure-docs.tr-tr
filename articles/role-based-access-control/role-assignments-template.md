---
title: RBAC ve Azure Resource Manager şablonları kullanarak Azure kaynaklarına erişimi yönetme | Microsoft Docs
description: Kullanıcılar, gruplar ve rol tabanlı erişim denetimi (RBAC) ve Azure Resource Manager şablonlarını kullanarak uygulamaları için Azure kaynaklarına erişimini yönetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 537ee35e96a41cd02605319e244d39c6567c3bf1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60344601"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>RBAC ve Azure Resource Manager şablonları kullanarak Azure kaynaklarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) Azure kaynaklarına erişimi yönetme yoludur. Azure PowerShell veya Azure CLI kullanmanın yanı sıra, RBAC kullanarak Azure kaynaklarına erişimi yönetebilir ve [Azure Resource Manager şablonları](../azure-resource-manager/resource-group-authoring-templates.md). Şablonlar, kaynakları tutarlı ve sürekli dağıtımı yapmanız gerekirse yararlı olabilir. Bu makalede RBAC ve şablonları kullanarak erişimini nasıl yönetebileceğiniz açıklanır.

## <a name="example-template-to-create-a-role-assignment"></a>Bir rol ataması oluşturmak için örnek şablonu

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir. Aşağıdaki şablonu gösterilmektedir:
- Bir kullanıcı, Grup veya uygulama kaynak grup kapsamında bir rol atama
- Bir parametre olarak sahibi, katkıda bulunan ve okuyucu rolleri belirtme

Şablonu kullanmak için aşağıdaki girişleri belirtmeniz gerekir:
- Bir kaynak grubu adı
- Bir kullanıcı, Grup veya uygulama rolüne atamak için benzersiz tanımlayıcısı
- Rol atamak için
- Rol ataması için kullanılacak benzersiz bir tanımlayıcı

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "scope": "[resourceGroup().id]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2017-05-01",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('scope')]"
      }
    }
  ]
}
```

Aşağıdaki örnek bir okuyucunun şablonu dağıttıktan sonra bir kullanıcı için rol ataması gösterir.

![Şablon kullanarak rol ataması](./media/role-assignments-template/role-assignment-template.png)

## <a name="deploy-template-using-azure-powershell"></a>Azure PowerShell kullanarak şablonu dağıtma

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Azure PowerShell kullanarak önceki şablonu dağıtmak için aşağıdaki adımları izleyin.

1. RBAC rg.json adlı yeni bir dosya oluşturun ve önceki şablonu kopyalayın.

1. [Azure PowerShell](/powershell/azure/authenticate-azureps) oturumu açın.

1. Bir kullanıcı, Grup veya uygulamanın benzersiz kimliğini alın. Örneğin, kullanabileceğiniz [Get-AzADUser](/powershell/module/az.resources/get-azaduser) Azure AD kullanıcılarının listelemek için komutu.

    ```azurepowershell
    Get-AzADUser
    ```

1. Rol ataması için kullanılacak bir benzersiz tanımlayıcısını oluşturmak için bir GUID aracını kullanın. Tanımlayıcı şu biçimdedir: `11111111-1111-1111-1111-111111111111`

1. Bir örnek kaynak grubu oluşturun.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Kullanım [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) dağıtımı başlatmak için komutu.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    ```

    Gerekli parametreleri belirtmeniz istenir. Çıktının bir örneği gösterilmektedir.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    
    cmdlet New-AzResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    principalId: 22222222-2222-2222-2222-222222222222
    builtInRoleType: Reader
    roleNameGuid: 11111111-1111-1111-1111-111111111111
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="deploy-template-using-the-azure-cli"></a>Azure CLI kullanarak şablonu dağıtma

Önceki Azure CLI kullanarak şablonu dağıtmak için bu adımları izleyin.

1. RBAC rg.json adlı yeni bir dosya oluşturun ve önceki şablonu kopyalayın.

1. Oturum [Azure CLI](/cli/azure/authenticate-azure-cli).

1. Bir kullanıcı, Grup veya uygulamanın benzersiz kimliğini alın. Örneğin, kullanabileceğiniz [az ad kullanıcı listesi](/cli/azure/ad/user#az-ad-user-list) Azure AD kullanıcılarının listelemek için komutu.

    ```azurecli
    az ad user list
    ```

1. Rol ataması için kullanılacak bir benzersiz tanımlayıcısını oluşturmak için bir GUID aracını kullanın. Tanımlayıcı şu biçimdedir: `11111111-1111-1111-1111-111111111111`

1. Bir örnek kaynak grubu oluşturun.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Kullanım [az grubu dağıtım oluşturma](/cli/azure/group/deployment#az-group-deployment-create) dağıtımı başlatmak için komutu.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    ```

    Gerekli parametreleri belirtmeniz istenir. Çıktının bir örneği gösterilmektedir.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    Please provide string value for 'principalId' (? for help): 22222222-2222-2222-2222-222222222222
    Please provide string value for 'builtInRoleType' (? for help):
     [1] Owner
     [2] Contributor
     [3] Reader
    Please enter a choice [1]: 3
    Please provide string value for 'roleNameGuid' (? for help): 11111111-1111-1111-1111-111111111111
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```
    
## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Oluşturma ve Azure portalını kullanarak Azure Resource Manager şablonlarını dağıtma](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=rbac)
