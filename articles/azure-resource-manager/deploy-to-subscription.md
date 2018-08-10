---
title: Kaynakları Azure aboneliğinize dağıtmak | Microsoft Docs
description: Abonelik kapsamındaki kaynaklar dağıtan bir Azure Resource Manager şablonunun nasıl oluşturulacağını açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: tomfitz
ms.openlocfilehash: 766534bfa02146e894916e2f9c953ef631913764
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40025165"
---
# <a name="deploy-resources-to-an-azure-subscription"></a>Bir Azure aboneliğine kaynakları dağıtma

Genellikle, kaynakları Azure aboneliğinizde bir kaynak grubuna dağıtın. Ancak, bazı kaynaklar, Azure aboneliğinizin düzeyinde dağıtılabilir. Bu kaynaklar, aboneliğiniz uygulanır. [İlkeleri](../azure-policy/azure-policy-introduction.md), [rol tabanlı erişim denetimi](../role-based-access-control/overview.md), ve [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) abonelik yerine düzeyinde şifreleme, kaynak grubu düzeyinde uygulamak isteyebileceğiniz hizmetleridir.

Bu makalede, şablonları dağıtmak için Azure CLI kullanılmaktadır. Şu anda PowerShell bir abonelik için bir şablonu dağıtmayı desteklemiyor.

## <a name="assign-policy"></a>İlke atama

Aşağıdaki örnek, var olan bir ilke tanımı aboneliğe atar. İlke parametreleri alırsa, bunları bir nesne olarak sağlayın. İlke parametre almasa bile varsayılan boş nesnesi kullanın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Azure Aboneliğinize bir yerleşik ilke uygulamak için aşağıdaki Azure CLI komutlarını kullanın. Bu örnekte, ilke parametresi mevcut değil

```azurecli-interactive
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Azure Aboneliğinize bir yerleşik ilke uygulamak için aşağıdaki Azure CLI komutlarını kullanın. Bu örnekte, ilke parametre yok.

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

## <a name="define-and-assign-policy"></a>Tanımlar ve ilke atama

Yapabilecekleriniz [tanımlamak](../azure-policy/policy-definition.md) ve aynı şablonu ilkesinde atayın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-03-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-03-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Aboneliğinizde bir ilke tanımı oluşturup aboneliğiniz için geçerli için aşağıdaki CLI komutunu kullanın.

```azurecli-interactive
az deployment create \
  -n definePolicy \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role"></a>Rol atama

Aşağıdaki örnek, bir kullanıcı veya gruba bir rolü atar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-05-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Bir Active Directory grubu aboneliğiniz için bir rol atamak için aşağıdaki Azure CLI komutlarını kullanın.

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group tomfitzexample --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Güvenlik Merkezi çalışma alanı ayarlarını dağıtma örneği için bkz: [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için [şablonları yazma](resource-group-authoring-templates.md). 
* Bir şablonda kullanabileceğiniz işlevler listesi için bkz. [şablon işlevleri](resource-group-template-functions.md).

