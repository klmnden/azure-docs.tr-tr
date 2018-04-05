---
title: Azure PowerShell Örnekleri - Bölgesel olarak yedekli ölçek kümesi | Microsoft Docs
description: Azure PowerShell Örnekleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b121684b3b9a5d03fe89e0892179140e9f5de80f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>PowerShell ile bölgesel olarak yedekli sanal makine ölçek kümesi oluşturma
Bu betik, birden çok Kullanılabilirlik Alanında Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur. Betiği çalıştırdıktan sonra sanal makineye RDP üzerinden erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1 "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Sanal ağ alt ağını tanımlayan bir yapılandırma nesnesi oluşturur. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir sanal ağ ve alt ağ oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Statik genel IP adresi oluşturur. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Genel IP adresi de dahil, yük dengeleyici ön ucunu tanımlayan bir yapılandırma nesnesi oluşturur. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Yük dengeleyici arka ucunu tanımlayan bir yapılandırma nesnesi oluşturur. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Ön uç ve arka uç yapılandırması nesnelerine dayalı bir yük dengeleyici oluşturur. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | 80 numaralı TCP bağlantı noktası üzerinde trafiği izlemek için bir yük dengeleyici durum araştırması oluşturur. 15 saniyelik aralıklarla art arda iki hatayla karşılaşılırsa uç noktasının sağlıksız olduğu kanısına varılır. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | 80 numaralı TCP bağlantı noktası üzerinde trafiği ön uç havuzundan arka uç havuzuna dağıtmak için yük dengeleyici kurallarını tanımlayan bir yapılandırma nesnesi oluşturur. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Durum araştırması ve yük dengeleyici kuralları ile yük dengeleyiciyi güncelleştirir. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | 80 numaralı TCP bağlantı noktası üzerinde trafiğe izin vermek için ağ güvenlik grubu kuralı tanımlayan bir yapılandırma nesnesi oluşturur. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Ağ güvenlik grubu ve kuralı oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Ağ güvenlik grubuyla ilişkilendirilmiş sanal ağ alt ağını güncelleştirir. Alt ağa bağlı tüm sanal makine örnekleri, ağ güvenlik grubu kurallarını devralır. |
| [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) | Alt ağ ve ağ güvenlik grubu yapılandırması ile sanal ağı güncelleştirir. |
| [New-AzureRmVmssIpConfig](/powershell/module/azurerm.compute/new-azurermvmssipconfig) | Yük dengeleyici arka uç havuzuna ve gelen NAT havuzuna sanal makine örneklerini bağlamak için sanal makine ölçek kümesi IP adresi bilgilerini tanımlayan bir yapılandırma nesnesi oluşturur.
| [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) | Bir ölçek kümesindeki sanal makine örneği sayısını, sanal makine örneği boyutunu ve yükseltme ilkesi modunu tanımlayan bir yapılandırma nesnesi oluşturur. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Sanal makine örnekleri için kullanılacak sanal makine görüntüsünü tanımlayan bir yapılandırma nesnesi oluşturur. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Sanal makine örneği adını ve kullanıcı kimlik bilgilerini tanımlayan bir yapılandırma profili oluşturur. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Her bir sanal makine örneği için sanal ağ arabirimi kartını tanımlayan bir yapılandırma nesnesi oluşturur. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Sanal makine ölçek kümesi oluşturmak için tüm yapılandırma nesnelerini birleştirir. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Sanal makine ölçek kümesi hakkında bilgi alır. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Özel Betik için sanal makine uzantısı ekleyerek temel bir web uygulaması yükler. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Sanal makine uzantısını uygulamak için sanal makine ölçek kümesi modelini güncelleştirir. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Yük dengeleyici tarafından kullanılan atanan genel IP adresi hakkında bilgi alır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |


## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.