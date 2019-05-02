---
title: Statik özel IP adresiyle - Azure PowerShell VM oluşturma | Microsoft Docs
description: PowerShell kullanarak özel bir IP adresi ile bir sanal makine oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2019
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 9115386b0543e1ac840aec29fc7f57e7c98c03bb
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64685344"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>PowerShell kullanarak bir statik özel IP adresli bir sanal makine oluşturun

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Statik özel IP adresli bir sanal makine (VM) oluşturabilirsiniz. Hangi adresi bir alt ağdan bir sanal makineye atanır seçmek istiyorsanız, bir dinamik adres yerine özel bir statik IP adresi atayın. Daha fazla bilgi edinin [statik özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method). Mevcut bir VM'ye gelen dinamik statik olarak atanmış bir özel IP adresini değiştirmek için veya genel IP adresleri ile çalışmak için bkz: [ekleme, değiştirme veya kaldırma IP adresleri](virtual-network-network-interface-addresses.md).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Yerel bilgisayarınızdan veya Azure Cloud Shell'i kullanarak aşağıdaki adımları tamamlayabilirsiniz. Yerel bilgisayarınıza kullanılacak olduğundan emin olun [Azure PowerShell'in](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure Cloud Shell'i kullanmak için **deneyin** takip eden herhangi bir komut kutusunu sağ üst köşesindeki içinde. Cloud Shell oturumunuzu Azure'da oturum açar.

1. Cloud Shell kullanıyorsanız, 2. adıma atlayın. Azure'a komut oturumuna ve oturum açma `Connect-AzAccount`.
2. Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurepowershell-interactive
   $RgName = "myResourceGroup"
   $Location = "eastus"
   New-AzResourceGroup -Name $RgName -Location $Location
   ```

3. Bir alt ağ yapılandırması ve sanal ağ oluşturma [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) ve [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) komutları:

   ```azurepowershell-interactive
   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object for use in a later step.
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

4. Sanal ağdaki bir ağ arabirimi oluşturun ve ağ arabirimi ile alt ağdan özel IP adresi atama [yeni AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) ve [yeni AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) komutlar:

   ```azurepowershell-interactive
   $IpConfigName1 = "IPConfig-1"
   $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

   $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
   ```

5. İle bir VM yapılandırması oluşturun [yeni AzVMConfig](/powershell/module/Az.Compute/New-AzVMConfig), ardından ile VM oluşturma ve [New-AzVM](/powershell/module/az.Compute/New-azVM). İstendiğinde, bir kullanıcı adı ve parola sanal makine için oturum açma kimlik bilgileri olarak kullanılmak üzere sağlayın:

   ```azurepowershell-interactive
   $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
   $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
   $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
   $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
   New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
   ```

> [!WARNING]
> Özel IP adresi ayarları işletim sistemine ekleyebilirsiniz ancak kadar edindikten sonra bunu öneririz [bir işletim sistemine özel bir IP adresi Ekle](virtual-network-network-interface-addresses.md#private).
> 
> 
> <a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>
> 
> [!IMPORTANT]
> İnternet'ten sanal Makineye erişmek için VM'ye bir genel IP adresi atamanız gerekir. Bir statik atama için dinamik bir özel IP adresi ataması da değiştirebilirsiniz. Ayrıntılar için bkz [IP adresleri ekleme veya değiştirme](virtual-network-network-interface-addresses.md). Ayrıca, ağ arabirimi, ağ arabiriminin oluşturduğunuz alt ağ veya her ikisi de bir ağ güvenlik grubu ilişkilendirerek, sanal makinenizde ağ trafiğini sınırlandırmak önerilir. Ayrıntılar için bkz [ağ güvenlik gruplarını yönetme](manage-network-security-group.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) ve atama bir [statik özel IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) bir Azure sanal makinesi için.
- Oluşturma hakkında daha fazla bilgi edinin [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineler.
