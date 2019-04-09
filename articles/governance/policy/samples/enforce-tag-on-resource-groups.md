---
title: Örnek - etiketi ve değerini kaynak grupları zorunlu kıl
description: Bu örnek ilke tanımı, bir etiket ve bir kaynak grubu üzerinde bir değer gerektirir.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/31/2019
ms.author: dacoulte
ms.openlocfilehash: a7a76fbde74ab80f8aa0f5e67e6445504c3aafa5
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276669"
---
# <a name="sample---enforce-tag-and-its-value-on-resource-groups"></a>Örnek - etiketi ve değerini kaynak grupları zorunlu kıl

Bu ilke, kaynak grubunda bir etiket ve değer gerektirir. Gerekli etiket adını ve değerini belirtirsiniz.

Bu örnek ilkeyi aşağıdakileri kullanarak dağıtabilirsiniz:

- [Azure portalı](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLI](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Örnek ilke

### <a name="policy-definition"></a>İlke tanımı

REST API, 'Azure'a Dağıt' düğmeleri ve portalda el ile kullanılan tam JSON ilkesi tanımı.

[!code-json[main](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.json "Enforce tag and its value on resource groups")]

> [!NOTE]
> Portalda el ile ilke oluşturuyorsanız yukarıdaki girişin **properties.parameters** ve **properties.policyRule** bölümlerini kullanın. Geçerli bir JSON kodu haline getirmek için iki bölümü küme ayraçları `{}` arasına alın.

### <a name="policy-rules"></a>İlke kuralları

Azure CLI ve Azure PowerShell tarafından kullanılan, ilke kurallarını tanımlayan JSON.

[!code-json[rule](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>İlke parametreleri

Azure CLI ve Azure PowerShell tarafından kullanılan, ilke parametrelerini tanımlayan JSON.

[!code-json[parameters](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json "Policy parameters (JSON)")]

|Ad |Type |Alan |Açıklama |
|---|---|---|---|
|tagName |String |etiketler |Etiketin adı; örneğin costCenter|
|tagValue |String |etiketler |Etiketin değeri; örneğin headquarter|

PowerShell veya Azure CLI ile atama oluştururken parametre verileri `-PolicyParameter` (PowerShell) veya `--params` (Azure CLI) kullanılarak dize ya da dosya şeklinde JSON biçiminde iletilebilir.
PowerShell aynı zamanda cmdlet'e bir Ad/Değer hashtable iletilmesini gereken `-PolicyParameterObject` parametresini de destekler. Burada **Ad** parametrenin adı, **Değer** ise atama sırasında iletilen tek bir değer veya değer dizisidir.

Bu örnek parametrede _tagName_ alanı için **costCenter** değeri, _tagValue_ alanı için de **headquarter** değeri tanımlanmıştır.

```json
{
    "tagName": {
        "value": "costCenter"
    },
    "tagValue": {
        "value": "headquarter"
    }
}
```

## <a name="azure-portal"></a>Azure portal

[![İlke örneği Azure'a dağıtma](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)
[![Azure kamu için ilke örneği dağıtma](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-resourceGroup-tags' -DisplayName 'Enforce tag and its value on resource groups' -description 'Enforces a required tag and its value on resource groups.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyParam = '{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-resourceGroup-tags-assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyParam
```

### <a name="remove-with-azure-powershell"></a>Azure PowerShell ile kaldırma

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell açıklaması

Betikleri dağıtmak ve kaldırmak için aşağıdaki komutları kullanın. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [Yeni AzPolicyDefinition](/powershell/module/az.resources/New-Azpolicydefinition) | Yeni bir Azure İlkesi tanımı oluşturur. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-Azresourcegroup) | Tek bir kaynak grubunu alır. |
| [Yeni AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) | Yeni bir Azure İlkesi ataması oluşturur. Bu örnekte bir tanım sağlıyoruz ancak girişim de kullanılabilir. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-Azpolicyassignment) | Var olan bir Azure İlkesi atamasını kaldırır. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-Azpolicydefinition) | Var olan bir Azure İlkesi tanımını kaldırır. |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'enforce-resourceGroup-tags' --display-name 'Enforce tag and its value on resource groups' --description 'Enforces a required tag and its value on resource groups.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyParam='{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
assignment=$(
az policy assignment create --name 'enforce-resourceGroup-tags-assignment' --display-name 'Enforce tag and its value on resource groups'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### <a name="remove-with-azure-cli"></a>Azure CLI ile kaldırma

Önceki atamayı ve tanımını kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure CLI açıklaması

| Komut | Notlar |
|---|---|
| [az ilke tanımı oluştur](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | Yeni bir Azure İlkesi tanımı oluşturur. |
| [az grubunu Göster](/cli/azure/group?view=azure-cli-latest#az-group-show) | Tek bir kaynak grubunu alır. |
| [az ilke ataması oluşturma](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | Yeni bir Azure İlkesi ataması oluşturur. Bu örnekte bir tanım sağlıyoruz ancak girişim de kullanılabilir. |
| [az ilke atamasını Sil](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | Var olan bir Azure İlkesi atamasını kaldırır. |
| [az ilke tanımını sil](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | Var olan bir Azure İlkesi tanımını kaldırır. |

[ARMClient](https://github.com/projectkudu/ARMClient) veya PowerShell gibi Resource Manager REST API'si ile etkileşim kurmak için kullanılabilecek birçok araç vardır. PowerShell'den REST API'sini çağırma örneği, [İlke tanımı yapısı](../concepts/definition-structure.md#aliases) bölümünün **Diğer Adlar** kısmında bulunabilir.

## <a name="rest-api"></a>REST API

### <a name="deploy-with-rest-api"></a>REST API'si ile dağıtma

- İlke Tanımını (Abonelik kapsamı) oluşturun. İstek Gövdesi için [ilke tanımı](#policy-definition) JSON kodunu kullanın.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
  ```

- İlke Atamasını (Kaynak Grubu kapsamı) oluşturma

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

  İstek Gövdesi için aşağıdaki JSON örneğini kullanın:

```json
  {
      "properties": {
          "displayName": "Enforce tag and its value Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags",
          "parameters": {
              "tagName": {
                  "value": "costCenter"
              },
              "tagValue": {
                  "value": "headquarter"
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>REST API'si ile kaldırma

- İlke Atamasını kaldırma

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

- İlke Tanımını kaldırma

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
  ```

### <a name="rest-api-explanation"></a>REST API'si açıklaması

| Hizmet | Grup | İşlem | Notlar |
|---|---|---|---|
| Kaynak Yönetimi | İlke Tanımları | [Oluştur](/rest/api/resources/policydefinitions/createorupdate) | Abonelikte yeni bir Azure İlkesi tanımı oluşturur. Alternatif: [Yönetim grubu oluşturma](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| Kaynak Yönetimi | İlke Atamaları | [Oluştur](/rest/api/resources/policyassignments/create) | Yeni bir Azure İlkesi ataması oluşturur. Bu örnekte bir tanım sağlıyoruz ancak girişim de kullanılabilir. |
| Kaynak Yönetimi | İlke Atamaları | [Sil](/rest/api/resources/policyassignments/delete) | Var olan bir Azure İlkesi atamasını kaldırır. |
| Kaynak Yönetimi | İlke Tanımları | [Sil](/rest/api/resources/policydefinitions/delete) | Var olan bir Azure İlkesi tanımını kaldırır. Alternatif: [Yönetim Grubu Sil](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>Sonraki adımlar

- Ek [Azure İlkesi örneklerini](index.md) gözden geçirme
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirme