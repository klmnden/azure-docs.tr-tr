---
title: Örnek - İlkesi girişimi fatura etiketleri
description: Bu örnek ilke tanımını kümesi, maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/23/2019
ms.author: dacoulte
ms.openlocfilehash: 5b084bd5a1b34dc2ce8ac8abb2a4af03a22262d2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787340"
---
# <a name="sample---billing-tags-policy-initiative"></a>Örnek - İlkesi girişimi fatura etiketleri

Bu ilke kümesi, maliyet merkezi ve ürün adı için belirtilen etiket değerlerini gerektirir. Gerekli etiketleri uygulamak ve zorlamak için yerleşik ilkeleri kullanır. Etiketler için gereken değerleri belirtin.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablon

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

Bu şablonu kullanarak dağıtabileceğiniz [PowerShell](#deploy-with-powershell).

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtımını temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Mevcut kaynaklara etiket uygulama

İlkeleri atadıktan sonra, eklediğiniz etiket ilkelerini zorlamak için tüm mevcut kaynaklara bir güncelleştirme tetikleyebilirsiniz. Aşağıdaki betik, kaynaklarda var olmuş diğer tüm etiketleri korur:

```azurepowershell-interactive
$resources = Get-AzResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örnekleri](index.md) sayfasındaki diğer örnekleri inceleyin