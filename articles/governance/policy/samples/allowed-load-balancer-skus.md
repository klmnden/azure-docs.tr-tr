---
title: Örnek - izin verilen yük dengeleyici SKU'ları
description: Bu örnek ilkesi, yük dengeleyicilerin onaylı bir SKU kullanmasını gerektirir.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 71d72c8f000ada3aeedb2bd3b59feb96ff892e98
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318598"
---
# <a name="allowed-load-balancer-skus"></a>İzin verilen yük dengeleyici SKU'ları

Bu ilke, yük dengeleyicilerin onaylı bir SKU kullanmasını gerektirir. İzin verilen bir SKU dizisi belirtirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/Network/load-balancer-skus/azurepolicy.json "Allowed Load Balancer SKUs")]

[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz.

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![Azure’a dağıtma](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FNetwork%2Fload-balancer-skus%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name "load-balancer-skus" -DisplayName "Allowed Load Balancer SKUs" -description "This policy enables you to specify a set of load balancer SKUs that your organization can deploy." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -listOfAllowedSKUs <Allowed SKUs> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'load-balancer-skus' --display-name 'Allowed Load Balancer SKUs' --description 'This policy enables you to specify a set of load balancer SKUs that your organization can deploy.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Network/load-balancer-skus/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "load-balancer-skus"
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin