---
title: "Azure ilke json Örnek - Depolama hesapları ve sanal makineler için izin verilen SKU'lar | Microsoft Docs"
description: "Bu json örnek ilke depolama hesapları ve sanal makineleri onaylanan SKU'ları kullanmanızı gerektirir."
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
ms.openlocfilehash: a25f7738c8db335e6bd43c90009bb51bb3a03696
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="allowed-skus-for-storage-accounts-and-virtual-machines"></a>Depolama hesapları ve sanal makineler için izin verilen SKU'ları

Bu ilke, depolama hesapları ve sanal makineleri onaylanan SKU'ları kullanmanızı gerektirir. Sağlamak için kullandığı yerleşik ilkeleri SKU'ları onaylanmış. Bir dizi onaylanan sanal makinelerin SKU'ları ve bir dizi onaylanan depolama hesabı SKU'ları belirtin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Örnek şablonu

[!code-json[main](../../../policy-templates/samples/PolicyInitiatives/skus-for-mutiple-types/azurepolicyset.json "Allowed SKUs for Storage Accounts and Virtual Machines")]


Bu şablonu kullanarak dağıtabilirsiniz [Azure portal](#deploy-with-the-portal) veya [PowerShell](#deploy-with-powershell).

## <a name="deploy-with-the-portal"></a>Portal ile dağıtma

[![Azure’a dağıtma](http://azuredeploy.net/deploybutton.png)](https://aka.ms/getpolicy)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]


````powershell
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-mutiple-types/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/skus-for-mutiple-types/azurepolicyset.parameters.json"

$policyset= New-AzureRmPolicySetDefinition -Name "skus-for-mutiple-types" -DisplayName "Allowed SKUs for Storage Accounts and Virtual Machines" -Description "This policy allows you to speficy what skus are allowed for storage accounts and virtual machines" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzureRmPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -LISTOFALLOWEDSKUS_1 <VM SKUs> -LISTOFALLOWEDSKUS_2 <Storage Account SKUs >  -Sku @{"Name"="A1";"Tier"="Standard"}
````

### <a name="clean-up-powershell-deployment"></a>PowerShell dağıtım temizleme

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```



## <a name="next-steps"></a>Sonraki adımlar

- Ek Azure ilke şablonu örneklerdir adresindeki [Azure ilke şablonları](../json-samples.md).
