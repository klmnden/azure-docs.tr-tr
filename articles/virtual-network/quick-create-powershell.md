---
title: Sanal ağ oluşturma - hızlı başlangıç - Azure PowerShell | Microsoft Docs
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Sanal ağ, sanal makineler gibi Azure kaynaklarının birbiriyle ve İnternet ile özel olarak iletişim kurmasına olanak sağlar.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: b8b67b235b54fb5bde738ed5cc1605e08d182a69
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38688094"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak sanal ağ oluşturma

Sanal ağ, sanal makineler (VM) gibi Azure kaynaklarının birbiriyle ve İnternet ile özel olarak iletişim kurmasına olanak sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından İnternet’ten bir sanal makineye bağlanacak ve iki sanal makine arasında özel olarak iletişim kuracaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

PowerShell’i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, AzureRM PowerShell modülü 5.4.1 veya üzeri bir sürümü gerektirir. Yüklü sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturabilmeniz için önce sanal ağı içerecek bir kaynak grubu oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek, *EastUS* konumunda *myVirtualNetwork* adlı bir varsayılan sanal ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Azure kaynakları bir sanal ağ içindeki alt ağa dağıtılır; bu nedenle bir alt ağ oluşturmanız gerekir. [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) ile bir alt ağ yapılandırması oluşturun. 

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağ içinde alt ağı oluşturan [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork) ile sanal ağa alt ağ yapılandırmasını yazın:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun:

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile bir sanal makine oluşturun. Sonraki komutu çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Aşağıdaki örnek çıktıya benzer bir çıktı döndürülür ve Azure arka planda sanal makineyi oluşturmaya başlar.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma 

Aşağıdaki komutu girin:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Önceki komut yürütülüp PowerShell’e çıktı döndürülünceye kadar, sonraki adımla devam etmeyin.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bir sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Aşağıdaki örnek,*myVm1* sanal makinesinin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Aşağıdaki komuttaki `<publicIpAddress>` öğesini, önceki komuttan döndürülen genel IP adresiyle değiştirin ve sonra aşağıdaki komutu girin: 

```
mstsc /v:<publicIpAddress>
```

Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilir. İndirilen rdp dosyasını açın. İstendiğinde **Bağlan**’ı seçin. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin. Sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan** seçeneğini belirlemeniz gerekebilir. **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, bağlantıya devam etmek için **Evet**’i veya **Bağlan**’ı seçin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

*myVm1* sanal makinesindeki PowerShell’den `ping myvm2` komutunu girin. Ping, İnternet Denetim İletisi Protokolü’nü (ICMP) kullandığından ve varsayılan olarak Windows güvenlik duvarı üzerinden ICMP’ye izin verilmediğinden ping başarısız olur.

*myVm2*’nin sonraki bir adımda *myVm1*’e ping komutu göndermesine izin vermek için, PowerShell’den, Windows güvenlik duvarı üzerinden gelen ICMP’ye izin veren aşağıdaki komutu girin:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

*myVm1* ile uzak masaüstü bağlantısını kapatın. 

[İnternet'ten bir sanal makineye bağlanma](#connect-to-a-vm-from-the-internet) bölümündeki adımları tekrar tamamlayın, ancak *myVm2*’ye bağlanın. 

*myVm2* sanal makinesindeki bir komut isteminden `ping myvm1` komutunu girin.

Önceki bir adımda *myVm1* sanal makinesinde Windows güvenlik duvarı üzerinden ICMP’ye izin verdiğinizden, *myVm1*’den yanıt alırsınız.

*myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet’ten bir sanal makineye bağlandınız ve sanal makine ile başka bir sanal makine arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın. 

Varsayılan olarak Azure, sanal makineler arasında kısıtlanmamış özel iletişime olanak sağlar, ancak yalnızca İnternet’ten Windows sanal makinelerine gelen uzak masaüstü bağlantılarına izin verir. Sanal makinelere gelen ve sanal makinelerden giden farklı ağ iletişimi türlerine izin verme veya bunları kısıtlama hakkında bilgi edinmek için [Ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğreticisiyle devam edin.
