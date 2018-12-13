---
title: Örnek - denetim SQL DB düzeyinde denetim ayarını denetle
description: Bu örnek ilkesi, bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 24313c83809734367459937d2b02441896b6a5fb
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315324"
---
# <a name="audit-sql-db-level-audit-setting"></a>SQL DB düzeyinde denetim ayarını denetle

Bu ilke, bu ayarlar belirli bir ayarla eşleşmiyorsa SQL veritabanı denetim ayarlarını denetler. Denetim ayarlarının etkin veya devre dışı olması gerektiğini gösteren bir değer belirtirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/SQL/audit-sql-db-auditing/azurepolicy.json "Audit SQL DB Level Audit Setting")]

[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz.

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![Azure’a dağıtma](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Faudit-sql-db-auditing%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = New-AzureRmPolicyDefinition -Name "audit-sql-db-auditing" -DisplayName "Audit SQL DB Level Audit Setting" -description "Audit DB level audit setting for SQL databases" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-auditing/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-auditing/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -setting <Audit Setting> -PolicyDefinition $definition
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
az policy definition create --name 'audit-sql-db-auditing' --display-name 'Audit SQL DB Level Audit Setting' --description 'Audit DB level audit setting for SQL databases' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-auditing/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-auditing/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-sql-db-auditing"
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin