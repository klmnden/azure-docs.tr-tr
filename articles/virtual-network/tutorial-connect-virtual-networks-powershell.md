---
title: "Sanal Ağ eşlemesi ile - sanal ağlara bağlanabilir PowerShell | Microsoft Docs"
description: "Sanal Ağ eşlemesi ile sanal ağları bağlamayı öğrenin."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c7b3fa2b566ab02e7fb4a03055db83f1545895e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>PowerShell kullanarak sanal ağ eşlemesi ile sanal ağlara bağlanabilir

Sanal ağlar birbirlerine sanal ağ eşlemesi ile bağlayabilirsiniz. Sanal ağlar eşlendikten sonra iki sanal ağlarda bulunan kaynaklar kaynaklar aynı sanal ağda değilmiş gibi aynı gecikme süresi ve bant genişliği ile birbirleri ile iletişim kuramıyor. Bu makalede, oluşturma ve iki sanal ağlar arasındaki eşleme kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * İki sanal ağ oluşturma
> * Sanal ağlar arasında eşleme oluşturma
> * Test eşleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Yükleme ve yerel olarak PowerShell kullanma seçerseniz, bu makalede Azure PowerShell modülü sürüm 3,6 veya üstü gerektirir. Çalıştırma ` Get-Module -ListAvailable AzureRM` yüklü olan sürümü bulunamıyor. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ ve bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur *myVirtualNetwork1* adres ön ekine sahip *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Aşağıdaki örnek, bir alt ağ yapılandırması 10.0.0.0/24 adres ön eki ile oluşturur:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Sanal ağ ile alt ağ yapılandırması yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), alt ağı oluşturur:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

10.1.0.0/16 adres öneki ve bir alt ağı ile bir sanal ağ oluşturun:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

Adres ön eki için *myVirtualNetwork2* sanal ağ adres ön ekiyle çakışmaması *myVirtualNetwork1* sanal ağ. Adres öneklerini örtüşen sanal ağlar eş olamaz.

## <a name="peer-virtual-networks"></a>Eş sanal ağlar

İle eşlemesi oluşturmak [Ekle AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). Aşağıdaki örnek eş *myVirtualNetwork1* için *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **PeeringState** olan *başlatılan*. Eşleme kalırken *başlatılan* gelen eşlemesi oluşturmak kadar durum *myVirtualNetwork2* için *myVirtualNetwork1*. Gelen eşlemesi oluşturmak *myVirtualNetwork2* için *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

Önceki komutu yürütüldükten sonra döndürülen çıktısında, gördüğünüz **PeeringState** olan *bağlı*. Azure eşleme durumunu da değişir *myVirtualNetwork1 myVirtualNetwork2* için eşleme *bağlı*. Onaylamak için eşleme durumu *myVirtualNetwork1 myVirtualNetwork2* eşliği değiştirildi *bağlı* ile [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Kadar diğer sanal ağ kaynaklarında bir sanal ağ olamaz iletişim kaynaklarla **PeeringState** hem de sanal ağlarda eşlemeleri için *bağlı*. 

Eşlemeler iki sanal ağ arasında ancak geçişli değil. Böylece, örneğin, ayrıca eş istemeniz durumunda *myVirtualNetwork2* için *myVirtualNetwork3*, ek sanal ağlar arasında eşlemeyi oluşturmanıza gerek *myVirtualNetwork2* ve *myVirtualNetwork3*. Olsa bile *myVirtualNetwork1* ile eşlenen *myVirtualNetwork2*, kaynakları içinde *myVirtualNetwork1* yalnızca kaynaklara erişim  *myVirtualNetwork3* varsa *myVirtualNetwork1* de ile eşlenen *myVirtualNetwork3*. 

Eşleme önce üretim sanal ağlar, baştan sona ile öğrenmeniz olduğunu önerilir [eşleme genel bakış](virtual-network-peering-overview.md), [eşliği yönetme](virtual-network-manage-peering.md), ve [sanal ağ sınırları ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Bu makalede bir iki sanal ağ aynı abonelik ve konum arasında eşleme gösterir ancak sanal ağlarda ayrıca eş [farklı bölgelerde](#register) ve [farklı Azure abonelikleri](create-peering-different-subscriptions.md#powershell). Ayrıca oluşturabilirsiniz [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) eşliği ile.

## <a name="test-peering"></a>Test eşleme

Bir eşleme aracılığıyla farklı sanal ağlardaki sanal makineler arasındaki ağ iletişimi sınamak için her alt ağda bir sanal makine dağıtın ve sanal makineler iletişim. 

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki adımda aralarında iletişim doğrulamak için bir sanal makinenin her sanal ağ oluşturun.

Bir sanal makine oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki örnek, bir sanal makine adlı oluşturur *myVm1* içinde *myVirtualNetwork1* sanal ağ. `-AsJob` Seçeneği bir sonraki adıma devam etmek için bu sanal makine arka planda oluşturur. İstendiğinde, kullanıcı adı ve sanal makine ile oturum açmak için istediğiniz parolayı girin.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

Azure otomatik olarak atar 10.0.0.4 sanal makinenin özel IP adresi 10.0.0.4 ilk kullanılabilir IP adresi olduğundan *Subnet1* , *myVirtualNetwork1*. 

Bir sanal makine oluşturmak *myVirtualNetwork2* sanal ağ.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Sanal makine oluşturmak için birkaç dakika sürer. 10.1.0.4 ilk kullanılabilir IP adresi olduğundan döndürülen çıkış Azure 10.1.0.4 sanal makinenin özel IP adresi atanmamış olsa *Subnet1* , *myVirtualNetwork2*. 

Azure sanal makinesi oluşturur ve PowerShell çıkışı döndürür kadar sonraki adımlara devam etmeyin.

### <a name="test-virtual-machine-communication"></a>Test sanal makinesi iletişimi

Bir sanal makineye ait genel IP adresi için Internet'ten bağlanabilir. Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ortak IP adresi, bir sanal makinenin dönün. Aşağıdaki örnek genel IP adresi döndürür *myVm1* sanal makine:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

İle Uzak Masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın *myVm1* yerel bilgisayarınızdan sanal makine. Değiştir `<publicIpAddress>` IP adresi ile döndürülen önceki komutu.

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilmeden ve açılır. Kullanıcı adı ve parola girin (seçmek için gerek duyabileceğiniz **daha fazla seçenek**, sonra **farklı bir hesap kullan**, sanal makineyi oluşturduğunuzda girdiğiniz kimlik bilgileri belirtmek için) ve 'ıtıklatın **Tamam**. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

Bu sanal makineden ping işlemi yapmak için bir komut isteminden ping Windows Güvenlik Duvarı üzerinden etkinleştirme *myVm2* sonraki adımda.

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Ping test etmek için bu makaledeki kullanılsa da ICMP üretim dağıtımları için Windows Güvenlik Duvarı aracılığıyla izin vererek önerilmez.

Bağlanmak için *myVm2* sanal makine, bir komut isteminden aşağıdaki komutu girin *myVm1* sanal makine:

```
mstsc /v:10.1.0.4
```

Ping etkinleştirilmiş olduğundan *myVm1*, şimdi onu bir komut isteminden IP adresiyle üzerinde ping *myVm2* sanal makine:

```
ping 10.0.0.4
```

Dört yanıt alırsınız. Sanal makinenin adına göre ping (*myVm1*), IP adresini yerine ping, çünkü başarısız *myVm1* bir bilinmeyen ana bilgisayar adıdır. Azure'nın varsayılan ad çözümlemesi, aynı sanal ağdaki sanal makineler arasında ancak farklı sanal ağlardaki sanal makineler arasında değil çalışır. Sanal ağlar arasında adları çözümlemek için şunları yapmanız gerekir [kendi DNS sunucusu dağıtma](virtual-networks-name-resolution-for-vms-and-role-instances.md) veya [Azure DNS özel etki alanları](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Hem sizin RDP oturumların bağlantısını kesmek *myVm1* ve *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerektiğinde kullanmak [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Genel sanal ağ eşleme Önizleme için kaydolun**

Aynı bölgedeki sanal ağları eşleme özelliği genel kullanıma açıktır. Sanal ağlar farklı bölgelerde şu anda önizlemede eşleme. Bkz: [sanal ağı güncelleştirmelerini](https://azure.microsoft.com/updates/?product=virtual-network) bölgeleri için kullanılabilir. Sanal ağlar bölgeler arasında eş için önce (içinde eş istediğiniz her sanal ağ kullanılıyor abonelik) aşağıdaki adımları tamamlayarak Önizleme için kaydetmeniz gerekir:

1. Eş istediğiniz her sanal ağ içinde önizleme için aşağıdaki komutları girerek aboneliğinizin kaydedin:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Aşağıdaki komutu girerek Önizleme için kayıtlı olduklarını doğrulayın:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Sanal ağlar farklı bölgelerde önce eş çalışırsanız **RegistrationState** önceki komutunu girdikten sonra aldığınız çıktı **kayıtlı** hem de abonelikleri için başarısız eşleme .

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ eşlemesi ile iki ağlara bağlanmak nasıl öğrendiniz. Yapabilecekleriniz [kendi bilgisayarınızda bir sanal ağa bağlanmak](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir VPN üzerinden ve sanal ağ veya eşlenmiş sanal ağlar kaynakları ile etkileşim.

Birçok sanal ağ makalelerinde ele görevi tamamlamak yeniden kullanılabilir komut dosyaları için kod örnekleri devam edin.

> [!div class="nextstepaction"]
> [Sanal ağ kod örnekleri](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
