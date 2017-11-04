---
title: "Azure ilke json örnek - denetim DB düzeyi tehdit algılama ayarı | Microsoft Docs"
description: "Bu ilkeleri belirli durumda ayarlanmamışsa bu json örnek ilke SQL veritabanı güvenlik uyarısı ilkelerini denetler."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: c7e1a7ef8ba37666e11598f61e27104edf9e77b7
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="audit-db-level-threat-detection-setting"></a>DB düzeyi tehdit algılama ayarı denetleme

Bu ilkeleri belirli durumda ayarlanmamışsa bu ilkeyi SQL veritabanı güvenlik uyarısı ilkelerini denetler. Tehdit algılama etkin mi yoksa devre dışı mı olduğunu belirten bir değer belirtin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablonu

[!code-json[main](../../../policy-templates/samples/SQL/audit-sql-db-threat-detection/azurepolicy.json "Audit DB level threat detection setting")]


Bu şablonu kullanarak dağıtabilirsiniz [Azure portal](#deploy-with-the-portal), ile [PowerShell](#deploy-with-powershell) veya [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![Azure’a dağıtma](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]


````powershell
$definition = New-AzureRmPolicyDefinition -Name "audit-sql-db-threat-detection" -DisplayName "Audit DB level threat detection setting" -description "Audit threat detection setting for SQL databases" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
````

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtım temizleme

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```


## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]



````cli

az policy definition create --name 'audit-sql-db-threat-detection' --display-name 'Audit DB level threat detection setting' --description 'Audit threat detection setting for SQL databases' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-threat-detection/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-sql-db-threat-detection"

````

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtım temizleme

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- Ek Azure ilke şablonu örneklerdir adresindeki [Azure ilke şablonları](../json-samples.md).
