---
title: Örnek - denetim SQL Server denetim ayarları
description: Bu örnek ilke tanımını auditIfNotExists ile SQL sunucusunun denetim ayarlarını denetler.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 04/27/2018
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: c4510a316760fa948aa39627c9a9c517437b1d77
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60926737"
---
# <a name="sample---audit-sql-server-audit-settings"></a>Örneği - denetim SQL server denetim ayarları

Bu yerleşik ilke, denetim ayarlarının etkin olup olmamasına göre SQL sunucusunu denetler.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

```json
{
  "if": {
    "field": "type",
    "equals": "Microsoft.SQL/servers"
  },
  "then": {
    "effect": "auditIfNotExists",
    "details": {
      "type": "Microsoft.SQL/servers/auditingSettings",
      "name": "default",
      "existenceCondition": {
        "allOf": [
          {
            "field": "Microsoft.Sql/auditingSettings.state",
            "equals": "[parameters('setting')]"
          }
        ]
      }
    }
  }
}
```

[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz. Yerleşik ilkeyi almak için, `a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9` kimliğini kullanın.

## <a name="parameters"></a>Parametreler

Parametre değerinde geçirmek için aşağıdaki biçimi kullanın:

```json
{"setting": {"value":"enabled"}}
```

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

Bir ilke atarken, kullanılabilir yerleşik tanımlardan **SQL Server Düzeyi Denetim Ayarını denetle**’yi seçin.

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9

New-AzPolicyAssignment -name "SQL Audit audit" -PolicyDefinition $definition -PolicyParameter '{"setting": {"value":"enabled"}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

İlke atamasını kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzPolicyAssignment -Name "SQL Audit audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```cli
az policy assignment create --scope <scope> --name "SQL Audit audit" --policy a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9 --params '{"setting": {"value":"enabled"}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

İlke atamasını kaldırmak için aşağıdaki komutu çalıştırın.

```cli
az policy assignment delete --name "SQL Audit audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin