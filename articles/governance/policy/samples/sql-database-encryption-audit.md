---
title: Örnek - SQL veritabanı için Denetim saydam veri şifrelemesi
description: Bu örnek ilkesi, SQL veritabanında saydam veri şifreleme etkin değilse denetler.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: 8bc9fdb6d1008d6d57edaab0bdbf3892584f3be2
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853808"
---
# <a name="audit-sql-database-encryption"></a>SQL veritabanı şifrelemesini denetleme

Bu yerleşik ilke, SQL veritabanında saydam veri şifreleme etkin değilse denetler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/SQL/audit-sql-db-tde-status/azurepolicy.json "Audit TDE for SQL Database")]

[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz. Yerleşik ilkeyi almak için, `17k78e20-9358-41c9-923c-fb736d382a12` kimliğini kullanın.

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

Bir ilke atarken, kullanılabilir yerleşik tanımlardan **Saydam veri şifreleme durumunu denetle**’yi seçin.

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/17k78e20-9358-41c9-923c-fb736d382a12

New-AzPolicyAssignment -name "SQL TDE Audit" -PolicyDefinition $definition -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

İlke atamasını kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzPolicyAssignment -Name "SQL TDE Audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "SQL TDE Audit" --policy 17k78e20-9358-41c9-923c-fb736d382a12
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

İlke atamasını kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az policy assignment delete --name "SQL TDE Audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin