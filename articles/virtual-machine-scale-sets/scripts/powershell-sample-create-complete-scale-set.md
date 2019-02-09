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
ms.openlocfilehash: 22de937c33edc7c8ec6944acf657937a1ce604f7
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981903"
---
# <a name="create-a-complete-virtual-machine-scale-set-with-powershell"></a>PowerShell ile eksiksiz bir sanal makine ölçek kümesi oluşturma

Bu betik, Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur. Kaynakların yapılandırılan ve oluşturulan, kullanmak yerine [yerleşik kaynak oluşturma seçenekleri mevcuttur burada yeni AzVmss içinde](powershell-sample-create-simple-scale-set.md). Betiği çalıştırdıktan sonra sanal makine örneklerine RDP üzerinden erişebilirsiniz.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/complete-scale-set/complete-scale-set.ps1 "Create a complete virtual machine scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Bir alt ağ yapılandırması oluşturur. Bu yapılandırma, sanal ağ oluşturma işlemiyle birlikte kullanılır. |
| [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur. |
| [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig) | Yük dengeleyici için bir ön uç IP yapılandırması oluşturur. |
| [Yeni AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) | Yük dengeleyici için bir arka uç adres havuzu yapılandırması oluşturur. |
| [Yeni AzLoadBalancerInboundNatRuleConfig](/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) | Yük dengeleyici için gelen bir NAT kuralı yapılandırması oluşturur. |
| [Yeni AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer) | Yük dengeleyici oluşturur. |
| [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/new-azloadbalancerprobeconfig) | Yük dengeleyici için bir araştırma yapılandırması oluşturur. |
| [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/new-azloadbalancerruleconfig) | Yük dengeleyici için bir kural yapılandırması oluşturur. |
| [Set-AzLoadBalancer](/powershell/module/az.Network/Set-azLoadBalancer) | Yük dengeleyiciyi sağlanan bilgilerle güncelleştirin. |
| [New-AzVmssIpConfig](/powershell/module/az.Compute/New-azVmssIpConfig) | Ölçek kümesi sanal makine örnekleri için bir IP yapılandırması oluşturun. Sanal makine örnekleri, yük dengeleyici arka uç havuzuna, NAT havuzuna ve sanal ağ alt ağına bağlanır. |
| [New-AzVmssConfig](/powershell/module/az.Compute/New-azVmssConfig) | Bir ölçek kümesi yapılandırması oluşturur. Bu yapılandırma, oluşturulacak sanal makine örneği sayısı, VM SKU (boyut) ve yükseltme ilkesi modu gibi bilgileri içerir. Yapılandırma ek cmdlet’ler tarafından eklenir ve ölçek kümesi oluşturma sırasında kullanılır. |
| [Set-AzVmssStorageProfile](/powershell/module/az.Compute/Set-azVmssStorageProfile) | Sanal makine örnekleri için kullanılacak görüntüyü tanımlayın ve ölçek kümesi yapılandırmasına ekleyin. |
| [Set-AzVmssOsProfile](/powershell/module/az.Compute/Set-azVmssStorageProfile) | Yönetici kullanıcı adı ve parola kimlik bilgilerini ve sanal makine adlandırma önekini tanımlayın. Bu değerleri ölçek kümesi yapılandırmasına ekleyin. |
| [Add-AzVmssNetworkInterfaceConfiguration](/powershell/module/az.Compute/Add-azVmssNetworkInterfaceConfiguration) | IP yapılandırmasına bağlı olarak sanal makine örneklerine bir sanal ağ arabirimi ekleyin. Bu değerleri ölçek kümesi yapılandırmasına ekleyin. |
| [Yeni AzVmss](/powershell/module/az.Compute/New-azVmss) | Ölçek kümesi yapılandırmasında sağlanan bilgilere göre ölçek kümesi oluşturun. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.
