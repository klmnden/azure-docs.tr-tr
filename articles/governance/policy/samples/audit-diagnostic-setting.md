---
title: Örnek - denetim tanılama ayarı
description: Bu örnek ilke tanımı, kaynak türleri için etkinleştirilmemiş tanılama ayarları belirtilmiş olup olmadığını denetler.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
origin.date: 04/27/2018
ms.date: 03/11/2019
ms.author: v-biyu
ms.openlocfilehash: 66c9c1c21cad7fb4058a91be826a50059691877c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60545648"
---
# <a name="sample---audit-diagnostic-setting"></a>Örnek - denetim tanılama ayarı

Bu yerleşik ilke, tanılama ayarları belirli kaynak türleri için etkinleştirilmediyse denetler. Tanılama ayarlarının etkin olup olmadığını denetlemek için bir kaynak türü dizisi belirtirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon
```json
{
    "name": "audit-diagnostic-setting",
    "properties": {
        "displayName": "Audit diagnostic setting",
        "description": "Audit diagnostic setting for selected resource types",
        "mode": "all",
        "parameters": {
            "listOfResourceTypes": {
                "type": "Array",
                "metadata": {
                    "displayName": "Resource Types",
                    "strongType": "resourceTypes"
                }
            }
        },
        "policyRule": {
            "if": {
                "field": "type",
                "in": "[parameters('listOfResourceTypes')]"
            },
            "then": {
                "effect": "auditIfNotExists",
                "details": {
                    "type": "Microsoft.Insights/diagnosticSettings",
                    "existenceCondition": {
                        "allOf": [
                            {
                                "field": "Microsoft.Insights/diagnosticSettings/logs.enabled",
                                "equals": "true"
                            },
                            {
                                "field": "Microsoft.Insights/diagnosticSettings/metrics.enabled",
                                "equals": "true"
                            }
                        ]
                    }
                }
            }
        }
    }
}
```
[Azure portalı](#deploy-with-the-portal) kullanarak, [PowerShell](#deploy-with-powershell) ile veya [Azure CLI](#deploy-with-azure-cli) ile bu şablonu dağıtabilirsiniz. Yerleşik ilkeyi almak için, `7f89b1eb-583c-429a-8828-af049802c1d9` kimliğini kullanın.

## <a name="parameters"></a>Parametreler

Parametre değerinde geçirmek için aşağıdaki biçimi kullanın:

```json
{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}
```

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

Bir ilke atarken, kullanılabilir yerleşik tanımlardan **Tanılama ayarını denetle**’yi seçin.

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```powershell
$definition = Get-AzPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/7f89b1eb-583c-429a-8828-af049802c1d9

New-AzPolicyAssignment -name "Audit diagnostics" -PolicyDefinition $definition -PolicyParameter '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzPolicyAssignment -Name "Audit diagnostics" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

```cli
az policy assignment create --scope <scope> --name "Audit diagnostics" --policy 7f89b1eb-583c-429a-8828-af049802c1d9 --params '{"listOfResourceTypes":{"value":["Microsoft.Cache/Redis","Microsoft.Compute/virtualmachines"]}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```cli
az policy assignment delete --name "Audit diagnostics" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin
