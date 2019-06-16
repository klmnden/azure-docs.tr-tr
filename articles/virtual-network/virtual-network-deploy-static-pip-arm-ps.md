---
title: VM bir statik genel IP adresiyle - PowerShell oluşturma | Microsoft Docs
description: PowerShell kullanarak bir statik genel IP adresiyle VM oluşturma konusunda bilgi edinin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: 208cff3c816b8243bc31b3db819f13dafe58c1d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683199"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>PowerShell kullanarak bir statik genel IP adresiyle bir sanal makine oluşturun

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir statik genel IP adresiyle bir sanal makine oluşturabilirsiniz. Genel bir IP adresi bir sanal makineye internet üzerinden iletişim kurmasına olanak tanır. Adresi hiçbir zaman değiştirdiğinden emin olmak için bir dinamik adres yerine bir statik genel IP adresi atayın. Daha fazla bilgi edinin [statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method). Varolan bir sanal makineye gelen dinamik statik olarak atanmış bir genel IP adresini değiştirmek için veya özel IP adresleri ile çalışmak için bkz: [ekleme, değiştirme veya kaldırma IP adresleri](virtual-network-network-interface-addresses.md). Genel IP adreslerine sahip bir [nominal bir ücret](https://azure.microsoft.com/pricing/details/ip-addresses)İşte bir [sınırı](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) için abonelik başına kullanabileceğiniz ortak IP adresi sayısı.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Yerel bilgisayarınızdan veya Azure Cloud Shell'i kullanarak aşağıdaki adımları tamamlayabilirsiniz. Yerel bilgisayarınıza kullanılacak olduğundan emin olun [Azure PowerShell'in](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Azure Cloud Shell'i kullanmak için **deneyin** takip eden herhangi bir komut kutusunu sağ üst köşesindeki içinde. Cloud Shell oturumunuzu Azure'da oturum açar.

1. Cloud Shell kullanıyorsanız, 2. adıma atlayın. Azure'a komut oturumuna ve oturum açma `Connect-AzAccount`.
2. Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurepowershell-interactive
   New-AzResourceGroup -Name myResourceGroup -Location EastUS
   ```

3. Bir sanal makine oluşturma [New-AzVM](/powershell/module/az.Compute/New-azVM) komutu. `-AllocationMethod "Static"` Seçeneği, sanal makine için statik genel IP adresi atar. Aşağıdaki örnekte adlı bir statik, temel SKU genel IP adresi ile bir Windows Server sanal makinesi oluşturur *Mypublicıpaddress*. İstendiğinde, bir kullanıcı adı ve parola sanal makine için oturum açma kimlik bilgileri olarak kullanılmak üzere sağlayın:

   ```azurepowershell-interactive
   New-AzVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
   ```

   Standart SKU genel IP adresi olması gerekiyorsa, zorunda [genel IP adresi oluşturma](virtual-network-public-ip-address.md#create-a-public-ip-address), [ağ arabirimini oluşturun](virtual-network-network-interface.md#create-a-network-interface), [ağarabirimindegenelIPadresiniAta](virtual-network-network-interface-addresses.md#add-ip-addresses), ardından [ağ arabirimi ile bir sanal makine oluşturma](virtual-network-network-interface-vm.md#add-existing-network-interfaces-to-a-new-vm), adımları ayrı. Daha fazla bilgi edinin [SKU genel IP adresi](virtual-network-ip-addresses-overview-arm.md#sku). Sanal makine genel bir Azure Load Balancer arka uç havuzuna eklenecek, sanal makinenin genel IP adresinin SKU yük dengeleyicinin genel IP adresinin SKU eşleşmesi gerekir. Ayrıntılar için bkz [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

4. Atanan genel IP adresini görüntüleyin ve ile statik bir adres olarak oluşturulduğunu onaylayın [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress):

   ```azurepowershell-interactive
   Get-AzPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
   ```

   Azure, sanal makineyi oluşturduğunuz bölge içinde kullanılan adreslerinden genel bir IP adresi atanır. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

> [!WARNING]
> Sanal makinenin işletim sistemi içinde IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerini farkında değil. Özel IP adresi ayarları işletim sistemine ekleyebilirsiniz ancak sürece bunu öneririz gerektiği kadar edindikten sonra değil [bir işletim sistemine özel bir IP adresi Ekle](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) azure'da
- Tüm hakkında daha fazla bilgi [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)
- Daha fazla bilgi edinin [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) ve atama bir [statik özel IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) bir Azure sanal makinesi için
- Oluşturma hakkında daha fazla bilgi edinin [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineler
