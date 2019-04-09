---
title: Örnek - sanal ağ uç nokta için Denetim anahtar kasaları
description: Bu örnek ilke tanımını hiçbir sanal ağ hizmet uç noktaları örnekleri algılamak için Key Vault kasaları denetler.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/26/2019
ms.author: dacoulte
ms.openlocfilehash: bc5ce4a6a2e52ed8d21de8db8da1f815293b61f7
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276380"
---
# <a name="sample---key-vault-vaults-with-no-virtual-network-endpoints"></a>Örnek - Key Vault kasaları ile sanal ağ uç nokta yok

Bu ilke için hiçbir sanal ağ uç noktaları olan Key Vault kasalarını denetler. Güvenlik gereksinimlerinizi zorlamak için bu seçeneği kullanın. Daha fazla bilgi için [sanal ağ hizmet uç noktaları anahtar Kasası'nda](../../../key-vault/key-vault-overview-vnet-service-endpoints.md)

Bu örnek ilkeyi aşağıdakileri kullanarak dağıtabilirsiniz:

- [Azure portalı](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLI](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Örnek ilke

### <a name="policy-definition"></a>İlke tanımı

REST API, 'Azure'a Dağıt' düğmeleri ve portalda el ile kullanılan tam JSON ilkesi tanımı.

[!code-json[full](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.json "KeyVault vnet rules")]

> [!NOTE]
> Portalda el ile ilke oluşturuyorsanız yukarıdaki girişin **properties.parameters** ve **properties.policyRule** bölümlerini kullanın. Geçerli bir JSON kodu haline getirmek için iki bölümü küme ayraçları `{}` arasına alın.

### <a name="policy-rules"></a>İlke kuralları

Azure CLI ve Azure PowerShell tarafından kullanılan, ilke kurallarını tanımlayan JSON.

[!code-json[rule](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>İlke parametreleri

Bu örnek ilke tanımı, tanımlanan hiç parametre yok.

## <a name="azure-portal"></a>Azure portal

[![İlke örneği Azure'a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)
[![Azure kamu için ilke örneği dağıtma](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name "audit-keyvault-vnet-rules" -DisplayName "Audit if Key Vault has no virtual network rules" -description "Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' -Mode Indexed

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-keyvault-vnet-rules-assignment' -DisplayName 'Audit Key Vault Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition
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
definition=$(az policy definition create --name 'audit-keyvault-vnet-rules' --display-name 'Audit if Key Vault has no virtual network rules' --description 'Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' --mode Indexed)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'audit-keyvault-vnet-rules-assignment' --display-name 'Audit Key Vault Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
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

## <a name="rest-api"></a>REST API

[ARMClient](https://github.com/projectkudu/ARMClient) veya PowerShell gibi Resource Manager REST API'si ile etkileşim kurmak için kullanılabilecek birçok araç vardır.

### <a name="deploy-with-rest-api"></a>REST API'si ile dağıtma

- İlke Tanımını (Abonelik kapsamı) oluşturun. İstek Gövdesi için [ilke tanımı](#policy-definition) JSON kodunu kullanın.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

- İlke Atamasını (Kaynak Grubu kapsamı) oluşturma

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

  İstek Gövdesi için aşağıdaki JSON örneğini kullanın:

  ```json
  {
      "properties": {
          "displayName": "Audit Key Vault Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules"
      }
  }
  ```

### <a name="remove-with-rest-api"></a>REST API'si ile kaldırma

- İlke Atamasını kaldırma

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

- İlke Tanımını kaldırma

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
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