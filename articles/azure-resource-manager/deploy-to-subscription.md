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
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: 1d281ebe80c6089c559cfaa77f4875a856566092
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079387"
---
# <a name="deploy-resources-to-an-azure-subscription"></a>Bir Azure aboneliğine kaynakları dağıtma

Genellikle, kaynakları Azure aboneliğinizde bir kaynak grubuna dağıtın. Ancak, bazı kaynaklar, Azure aboneliğinizin düzeyinde dağıtılabilir. Bu kaynaklar, aboneliğiniz uygulanır. [İlkeleri](../azure-policy/azure-policy-introduction.md), [rol tabanlı erişim denetimi](../role-based-access-control/overview.md), ve [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) abonelik yerine düzeyinde şifreleme, kaynak grubu düzeyinde uygulamak isteyebileceğiniz hizmetleridir.

Bu makalede, şablonları dağıtmak için Azure CLI ve PowerShell kullanır.

## <a name="name-and-location-for-deployment"></a>Dağıtım için ad ve konum

Aboneliğinize dağıtılırken dağıtım için bir konum sağlamalısınız. Dağıtım için bir ad sağlar. Dağıtım için bir ad belirtmezseniz, şablonun adını dağıtım adı kullanılır. Örneğin, adlandırılmış bir şablon dağıtımı **azuredeploy.json** varsayılan dağıtım adı oluşturur **azuredeploy**.

Abonelik düzeyi dağıtımları konumunu sabittir. Aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda ancak farklı bir konuma bir dağıtım oluşturulamıyor. Hata kodu alırsanız `InvalidDeploymentLocation`, farklı bir ad veya aynı konumdaki önceki dağıtım için bu adı kullanın.

## <a name="using-template-functions"></a>Şablon işlevlerini kullanma

Abonelik düzeyi dağıtımları vardır bazı önemli noktalar şablon işlevleri kullanırken:

* [ResourceGroup()](resource-group-template-functions-resource.md#resourcegroup) işlevi **değil** desteklenir.
* [ResourceId()](resource-group-template-functions-resource.md#resourceid) işlevi desteklenir. Abonelik düzeyi dağıtımları sırasında kullanılan kaynaklar için kaynak Kimliğini almak için kullanın. Örneğin, bir ilke tanımı için kaynak Kimliğini alın `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* [Reference()](resource-group-template-functions-resource.md#reference) ve [list()](resource-group-template-functions-resource.md#list) işlevler desteklenir.

## <a name="assign-policy"></a>İlke ata

Aşağıdaki örnek, var olan bir ilke tanımı aboneliğe atar. İlke parametreleri alırsa, bunları bir nesne olarak sağlayın. İlke parametre almasa bile varsayılan boş nesnesi kullanın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
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

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
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

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

## <a name="define-and-assign-policy"></a>Tanımlar ve ilke atama

Yapabilecekleriniz [tanımlamak](../azure-policy/policy-definition.md) ve aynı şablonu ilkesinde atayın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
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
            "apiVersion": "2018-05-01",
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

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name definePolicy `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role"></a>Rol atama

Aşağıdaki örnek, bir kullanıcı veya gruba bir rolü atar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
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
            "apiVersion": "2017-09-01",
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
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Güvenlik Merkezi çalışma alanı ayarlarını dağıtma örneği için bkz: [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Bir kaynak grubu oluşturmak için bkz: [Azure Resource Manager şablonlarında kaynak grupları oluşturma](create-resource-group-in-template.md).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için [şablonları yazma](resource-group-authoring-templates.md). 
* Bir şablonda kullanabileceğiniz işlevler listesi için bkz. [şablon işlevleri](resource-group-template-functions.md).

