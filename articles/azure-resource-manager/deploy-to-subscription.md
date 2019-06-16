---
title: Kaynak grubu ve kaynak aboneliği - Azure Resource Manager şablonu oluşturma
description: Bir Azure Resource Manager şablonunda bir kaynak grubu oluşturmayı açıklar. Ayrıca Azure aboneliği kapsamında kaynakları dağıtma işlemini de gösterir.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: bdba294e1ee776d90b93f715e930ec26765abb7f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129162"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Kaynak grubu ve kaynak abonelik düzeyinde oluşturun.

Genellikle, Azure kaynakları Azure aboneliğinizde bir kaynak grubuna dağıtın. Ancak, aynı zamanda Azure kaynak grupları oluşturabilir ve abonelik düzeyinde Azure kaynakları oluşturma. Abonelik düzeyinde şablonlarını dağıtmak için Azure CLI ve Azure PowerShell kullanırsınız. Azure portalında dağıtım abonelik düzeyinde desteklemiyor.

Bir Azure Resource Manager şablonunda bir kaynak grubu oluşturmak için tanımladığınız bir [ **Microsoft.Resources/resourceGroups** ](/azure/templates/microsoft.resources/allversions) kaynak adı ve kaynak grubu için konum. Bir kaynak grubu oluşturun ve kaynakları bu kaynak grubunda aynı şablonu dağıtın. Abonelik düzeyinde dağıttığınız kaynakları şunlardır: [İlkeleri](../governance/policy/overview.md), ve [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment-considerations"></a>Dağıtma konuları

Abonelik düzeyi dağıtımı, kaynak grubu dağıtımı aşağıdaki noktalarda farklıdır:

### <a name="schema-and-commands"></a>Şema ve komutlar

Abonelik düzeyinde dağıtımlar için kullandığınız komutlar ve şema, kaynak grubu dağıtımlarında farklıdır. 

İçin şemayı kullanın `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Azure CLI dağıtım komutu için kullanmak [az dağıtım oluşturma](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create). Örneğin, bir kaynak grubu oluşturmak için bir şablon aşağıdaki CLI komutunu dağıtır:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

PowerShell dağıtım komutu için kullanmak [yeni AzDeployment](/powershell/module/az.resources/new-azdeployment). Örneğin, aşağıdaki PowerShell komutu, bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurepowershell
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>Dağıtım adı ve konumu

Aboneliğinize dağıtılırken dağıtım için bir konum sağlamalısınız. Dağıtım için bir ad sağlar. Dağıtım için bir ad belirtmezseniz, şablonun adını dağıtım adı kullanılır. Örneğin, adlandırılmış bir şablon dağıtımı **azuredeploy.json** varsayılan dağıtım adı oluşturur **azuredeploy**.

Abonelik düzeyi dağıtımları konumunu sabittir. Aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda ancak farklı bir konuma bir dağıtım oluşturulamıyor. Hata kodu alırsanız `InvalidDeploymentLocation`, farklı bir ad veya aynı konumdaki önceki dağıtım için bu adı kullanın.

### <a name="use-template-functions"></a>Şablon işlevleri

Abonelik düzeyinde dağıtımlar için bazı önemli noktalar şablon işlevleri kullanarak oluşan vardır:

* [ResourceGroup()](resource-group-template-functions-resource.md#resourcegroup) işlevi **değil** desteklenir.
* [ResourceId()](resource-group-template-functions-resource.md#resourceid) işlevi desteklenir. Abonelik düzeyi dağıtımları sırasında kullanılan kaynaklar için kaynak Kimliğini almak için kullanın. Örneğin, bir ilke tanımı için kaynak Kimliğini alın `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* [Reference()](resource-group-template-functions-resource.md#reference) ve [list()](resource-group-template-functions-resource.md#list) işlevler desteklenir.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturun

Aşağıdaki şablon boş bir kaynak grubu oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Şablon Şeması şu yolda bulunabilir: [burada](/azure/templates/microsoft.resources/allversions). Benzer şablonları bulunabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments).

## <a name="create-multiple-resource-groups"></a>Birden fazla kaynak grubu oluşturun

Kullanım [copy öğesinde](resource-group-create-multiple.md) kaynak gruplarıyla birden fazla kaynak grubu oluşturun. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Kaynak yineleme hakkında daha fazla bilgi için bkz. [bir kaynağa veya Azure Resource Manager şablonları özelliğinde birden fazla örneğini dağıtma](./resource-group-create-multiple.md), ve [Öğreticisi: Resource Manager şablonları ile birden çok kaynak örneğini oluşturma](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="create-resource-group-and-deploy-resources"></a>Kaynak grubu oluşturun ve kaynakları dağıtma

Kaynak grubu oluşturun ve kaynakları dağıtmak için bir iç içe geçmiş şablon kullanın. İç içe geçmiş şablon kaynaklar kaynak grubuna dağıtmak için tanımlar. İç içe geçmiş şablon bağımlı kaynakları dağıtmadan önce kaynak grubunun mevcut olduğundan emin olmak için kaynak grubu olarak ayarlayın.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve bir depolama hesabı kaynak grubuna dağıtır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>İlkeleri oluşturma

### <a name="assign-policy"></a>İlke atama

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

Azure Aboneliğinize bir yerleşik ilke uygulamak için aşağıdaki Azure CLI komutları kullanın:

```azurecli
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Azure Aboneliğinize bir yerleşik ilke uygulamak için aşağıdaki Azure CLI komutları kullanın:

```azurecli
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>Tanımlar ve ilke atama

Yapabilecekleriniz [tanımlamak](../governance/policy/concepts/definition-structure.md) ve aynı şablonu ilkesinde atayın.

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

Aboneliğinizde bir ilke tanımı oluşturup aboneliğiniz için geçerli için aşağıdaki CLI komutunu kullanın:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="create-roles"></a>Rol oluşturma

### <a name="assign-role-at-subscription"></a>Abonelik rolü atama

Aşağıdaki örnek bir kullanıcı veya grup abonelik için bir rolü atar. Kapsam aboneliğine otomatik olarak ayarlandığından, bu örnekte, atama için bir kapsam belirtmeyin.

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

Bir Active Directory grubu aboneliğiniz için bir rol atamak için aşağıdaki Azure CLI komutları kullanın:

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

### <a name="assign-role-at-scope"></a>Kapsamda rolü atama

Aşağıdaki abonelik düzeyinde şablonu bir kullanıcı veya grup, abonelik içindeki kaynak grubu için kapsamlı bir rolü atar. Kapsam veya dağıtım düzeyi altında olması gerekir. Bir aboneliğe dağıtabilir ve bu Abonelikteki kaynak grubu için kapsamlı bir rol ataması belirtin. Ancak, bir kaynak grubuna dağıtmak ve bir abonelik için rol atama kapsamı belirtin.

Bir kapsamda bir rol atamak için bir iç içe geçmiş dağıtım'ı kullanın. Kaynak grubu adı hem de dağıtım kaynak özelliklerini rol ataması kapsam özelliğinde belirtilen dikkat edin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        },
        "rgName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "assignRole",
            "resourceGroup": "[parameters('rgName')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/roleAssignments",
                            "name": "[guid(parameters('principalId'), deployment().name)]",
                            "apiVersion": "2017-09-01",
                            "properties": {
                                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                                "principalId": "[parameters('principalId')]",
                                "scope": "[concat(subscription().id, '/resourceGroups/', parameters('rgName'))]"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Bir Active Directory grubu aboneliğiniz için bir rol atamak için aşağıdaki Azure CLI komutları kullanın:

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json \
  --parameters principalId=$principalid roleDefinitionId=$role rgName demoRg
```

PowerShell ile bu şablonu dağıtmak için şunu kullanın:

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id `
  -rgName demoRg
```

## <a name="next-steps"></a>Sonraki adımlar

* Azure Güvenlik Merkezi çalışma alanı ayarlarını dağıtma örneği için bkz: [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için [şablonları yazma](resource-group-authoring-templates.md). 
* Bir şablonda kullanabileceğiniz işlevler listesi için bkz. [şablon işlevleri](resource-group-template-functions.md).
