---
title: Örnek - sanal makine ağ arabirimleri için sanal ağ kullanım Onaylandı
description: Bu örnek ilke tanımı, ağ arabirimleri onaylı bir sanal ağı kullanmanız gerekir.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: 39ecfacae8c5d5260384182eba55443b3c928c14
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394475"
---
# <a name="sample---use-approved-vnet-for-vm-network-interfaces"></a>Örnek - sanal makine ağ arabirimleri için sanal ağ kullanım Onaylandı

Bu ilke, ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/Network/vm-creation-in-approved-vnet/azurepolicy.json "Use approved vNet for VM network interfaces")]

[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz.

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![İlke örneği Azure'a dağıtma](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fvm-creation-in-approved-vnet%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$definition = New-AzPolicyDefinition -Name "vm-creation-in-approved-vnet" -DisplayName "Use approved vNet for VM network interfaces" -description "This policy enforces VM network interfaces to use vNet." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzPolicyAssignment -Name <assignmentname> -Scope <scope>  -vNetId <vNet Id> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'vm-creation-in-approved-vnet' --display-name 'Use approved vNet for VM network interfaces' --description 'This policy enforces VM network interfaces to use vNet.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/vm-creation-in-approved-vnet/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "vm-creation-in-approved-vnet"
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin