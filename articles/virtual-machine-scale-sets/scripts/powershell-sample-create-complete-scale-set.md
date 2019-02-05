---
title: Azure PowerShell Örnekleri - Eksiksiz bir sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Azure PowerShell Örnekleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/29/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 20bf851cc9b2965a355286699e1ef255887d7650
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55697097"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>PowerShell ile eksiksiz bir sanal makine ölçek kümesi oluşturma
Bu betik, Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur. Kaynakların yapılandırılan ve oluşturulan, kullanmak yerine [yerleşik kaynak oluşturma seçenekleri kullanılabilir burada New-AzureRmVmss](powershell-sample-create-simple-scale-set.md). Betiği çalıştırdıktan sonra sanal makine örneklerine RDP üzerinden erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/complete-scale-set/complete-scale-set.ps1 "Create a complete virtual machine scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Sanal ağ oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Yük dengeleyici için bir ön uç IP yapılandırması oluşturur. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Yük dengeleyici için bir arka uç adres havuzu yapılandırması oluşturur. |
| [New-AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Yük dengeleyici için gelen bir NAT kuralı yapılandırması oluşturur. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Yük dengeleyici oluşturur. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Yük dengeleyici için bir araştırma yapılandırması oluşturur. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Yük dengeleyici için bir kural yapılandırması oluşturur. |
| [Set-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/Set-AzureRmLoadBalancer) | Yük dengeleyiciyi sağlanan bilgilerle güncelleştirin. |
| [New-AzureRmVmssIpConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssIpConfig) | Ölçek kümesi sanal makine örnekleri için bir IP yapılandırması oluşturun. Sanal makine örnekleri, yük dengeleyici arka uç havuzuna, NAT havuzuna ve sanal ağ alt ağına bağlanır. |
| [New-AzureRmVmssConfig](/powershell/module/AzureRM.Compute/New-AzureRmVmssConfig) | Bir ölçek kümesi yapılandırması oluşturur. Bu yapılandırma, oluşturulacak sanal makine örneği sayısı, VM SKU (boyut) ve yükseltme ilkesi modu gibi bilgileri içerir. Yapılandırma ek cmdlet’ler tarafından eklenir ve ölçek kümesi oluşturma sırasında kullanılır. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Sanal makine örnekleri için kullanılacak görüntüyü tanımlayın ve ölçek kümesi yapılandırmasına ekleyin. |
| [Set-AzureRmVmssOsProfile](/powershell/module/AzureRM.Compute/Set-AzureRmVmssStorageProfile) | Yönetici kullanıcı adı ve parola kimlik bilgilerini ve sanal makine adlandırma önekini tanımlayın. Bu değerleri ölçek kümesi yapılandırmasına ekleyin. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/AzureRM.Compute/Add-AzureRmVmssNetworkInterfaceConfiguration) | IP yapılandırmasına bağlı olarak sanal makine örneklerine bir sanal ağ arabirimi ekleyin. Bu değerleri ölçek kümesi yapılandırmasına ekleyin. |
| [New-AzureRmVmss](/powershell/module/AzureRM.Compute/New-AzureRmVmss) | Ölçek kümesi yapılandırmasında sağlanan bilgilere göre ölçek kümesi oluşturun. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.
