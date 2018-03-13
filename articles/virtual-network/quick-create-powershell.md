---
title: "Bir Azure sanal ağı - PowerShell oluşturma | Microsoft Docs"
description: "Hızlı bir şekilde PowerShell kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ özel olarak birbirleriyle ve internet ile iletişim kurmak için sanal makineler gibi Azure kaynakları sağlar."
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
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 13d36e6861a30473e6cb5d54d94a3c23a1e4cc59
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>PowerShell kullanarak bir sanal ağ oluşturma

Bir sanal ağ sanal makineler (VM) gibi Azure kaynakları özel olarak birbirleriyle ve internet ile iletişim kurmasını sağlar. Bu makalede, bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ oluşturduktan sonra iki VM sanal ağa dağıtın. Ardından internet'ten bir VM'ye bağlanın ve özel olarak iki VM iletişim.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Yükleme ve yerel olarak PowerShell kullanma seçerseniz, bu makalede AzureRM PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır ` Get-Module -ListAvailable AzureRM`. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmadan önce sanal ağ içerecek şekilde bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu ile oluşturmak [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Aşağıdaki örnek adlı bir varsayılan sanal ağ oluşturur *myVirtualNetwork* içinde *EastUS* konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Bir alt ağ oluşturmak gereken şekilde azure kaynaklarını bir sanal ağ içindeki bir alt ağ olarak dağıtılır. Bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). 

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağ ile alt ağ yapılandırması yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), sanal ağ içindeki alt ağı oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

İki VM sanal ağ oluşturun:

### <a name="create-the-first-vm"></a>İlk VM oluşturma

Bir VM ile oluşturma [AzureRmVM yeni](/powershell/module/azurerm.compute/new-azurermvm). Aşağıdaki komutu çalıştırırken, kimlik bilgileri istenir. Girdiğiniz değerleri VM için kullanıcı adı ve parola yapılandırılır. `-AsJob` Seçeneği bir sonraki adıma devam edebilmesi için bu VM arka planda oluşturur.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Aşağıdaki örnek çıkış benzer bir çıktı döndürülür ve Azure arka planda VM oluşturma başlatır.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-vm"></a>İkinci VM oluşturma 

Aşağıdaki komutu girin:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

VM oluşturmak için birkaç dakika sürer. Sonraki adıma, önceki komutu yürütür ve çıktı için PowerShell döndürülür kadar devam etmeyin.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir VM'ye bağlanın

Kullanım [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) VM genel IP adresi dönün. Aşağıdaki örnek genel IP adresi döndürür *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Değiştir `<publicIpAddress>` aşağıdaki komutta ortak IP adresi önceki komuttan döndürülen ve aşağıdaki komutu girin: 

```
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur ve bilgisayarınıza indirilmeden. İndirilen rdp dosyasını açın. İstenirse, seçin **Bağlan**. Kullanıcı adı ve VM oluştururken belirttiğiniz parolayı girin. Seçmek için gerek duyabileceğiniz **daha fazla seçenek**, ardından **farklı bir hesap kullan**, VM oluşturduğunuz sırada girdiğiniz kimlik bilgileri belirtmek için. **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Uyarı alırsanız, seçin **Evet** veya **devam**, bağlantı ile devam etmek için.

## <a name="communicate-privately-between-vms"></a>Özel olarak VM'ler arasında iletişim

Üzerinde powershell'den *myVm1* VM girin `ping myvm2`. Ping başarısız olursa, Internet Denetim İletisi Protokolü (ICMP) ve ICMP ping kullandığı için Windows Güvenlik Duvarı üzerinden varsayılan olarak izin verilmiyor.

İzin vermek için *myVm2* ping işlemi yapmak için *myVm1* bir sonraki adımda ICMP verir Powershell'den aşağıdaki komutu girin Windows Güvenlik Duvarı üzerinden gelen:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Uzak Masaüstü Bağlantısı kapatmak *myVm1*. 

Bölümündeki adımları tamamlamanız [VM Bağlan internet'ten](#connect-to-a-vm-from-the-internet) yeniden, ancak bağlanmak *myVm2*. 

Bir komut isteminden *myVm2* VM girin `ping myvm1`.

Yanıt aldığınız *myVm1*, üzerinde Windows Güvenlik Duvarı üzerinden ICMP izin *myVm1* bir önceki adımda VM.

Uzak Masaüstü Bağlantısı kapatmak *myVm2*.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubu ve içerdiği kaynakların tümünü kaldırmak için:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir varsayılan sanal ağ ve iki VM oluşturdunuz. Internet'ten bir VM'ye bağlı ve özel olarak VM ve başka bir VM iletişim. Sanal ağ ayarları hakkında daha fazla bilgi için bkz: [sanal ağ yönetme](manage-virtual-network.md). 

Varsayılan olarak, Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar, ancak yalnızca gelen Uzak Masaüstü bağlantıları Windows VM'ler için Internet'ten izin verir. İzin vermek veya farklı türlerde ağ iletişimi için ve VM'ler kısıtlamak nasıl öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Ağ trafiği filtreleme](virtual-networks-create-nsg-arm-ps.md)
