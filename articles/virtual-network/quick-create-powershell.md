---
title: Sanal ağ - hızlı başlangıç - Azure PowerShell oluşturma
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ, sanal makineler gibi Azure kaynaklarının sağlar, birbiriyle ve internet ile özel olarak iletişim.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: jdial
ms.openlocfilehash: 725e03ded6d6f2e3b5d7a41d2053f418a5933ef8
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023252"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Hızlı Başlangıç: PowerShell kullanarak sanal ağ oluşturma

Sanal makineler (VM) gibi Azure kaynaklarını bir sanal ağ sağlar, birbiriyle ve internet ile özel olarak iletişim. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından internet'ten sanal makinelere bağlanın ve sanal ağ üzerinden birbiriyle iletişim kurmasına.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip yerine karar verirseniz, bu hızlı başlangıçta AzureRM PowerShell modülünün 5.4.1 kullanmanızı gerekli hale getirmiş veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Bkz: [Azure PowerShell modülü yükleme](/powershell/azure/install-azurerm-ps) yükleme ve yükseltme bilgileri için.

PowerShell'i yerel olarak çalıştırıyorsanız, son olarak, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzureRmAccount`. Bu komut, Azure ile bir bağlantı oluşturur.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Bir kaynak grubunu ve sanal ağ oluşturma

Birkaç adımları, kaynak grubunu ve sanal ağ yapılandırılmış almak için size yol gerek vardır.

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Bir sanal ağ oluşturabilmeniz için önce sanal ağ'ı barındırmak için bir kaynak grubu oluşturmanız gerekir. [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutunu kullanarak bir kaynak grubu oluşturun. Bu örnek adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) ile sanal ağ oluşturun. Bu örnek adlı varsayılan bir sanal ağ oluşturur *myVirtualNetwork* içinde *EastUS* konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Alt ağ ekleme

Bir alt ağ oluşturmanız gerekir, böylece azure kaynakları bir sanal ağ içindeki alt ağa dağıtır. Adlı bir alt ağ yapılandırması *varsayılan* ile [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Sanal ağ alt ağını ilişkilendirin

Sanal ağ için alt ağ yapılandırmasını yazma [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork). Bu komut, alt ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

İlk VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Sonraki komutu çalıştırdığınızda, kimlik bilgileri istenir. VM için bir kullanıcı adı ve parola girin:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

`-AsJob` Seçeneği arka planda sanal Makineyi oluşturur. Sonraki adıma devam edebilirsiniz.

Azure VM oluşturma arka planda başladığında, şunun gibi geri alırsınız:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Bu komutla birlikte ikinci bir sanal makine oluşturun:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Başka bir kullanıcı adı ve parola oluşturmanız gerekir. Azure VM oluşturmak için birkaç dakika sürer.

> [!IMPORTANT]
> Azure'nın tamamlandı kadar sonraki adıma geçmeyin.  PowerShell'e çıktı döndürdüğünde yapıldığını anlarsınız.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bir sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Bu örnekte genel IP adresini döndürür *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızda bir komut istemi açın. `mstsc` komutunu çalıştırın. Değiştirin `<publicIpAddress>` genel IP adresiyle son adımından döndürülen:

> [!NOTE]
> Çalıştırmakta yerel bilgisayarınıza ve PowerShell isteminden aşağıdaki komutları olduğunuz AzureRM PowerShell modülünün 5.4.1 veya daha sonra bu arabirimde devam edebilirsiniz.

```cmd
mstsc /v:<publicIpAddress>
```

Bir Uzak Masaüstü Protokolü (*.rdp*) dosyasını bilgisayarınıza indirir ve Uzak Masaüstü açılır.

1. İstendiğinde **Bağlan**’ı seçin.

1. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

    > [!NOTE]
    > Seçmek için gerek duyabileceğiniz **daha fazla seçenek** > **farklı bir hesap kullan**sanal Makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için.

1. **Tamam**’ı seçin.

1. Bir sertifika uyarısı alabilirsiniz. Seçerseniz **Evet** veya **devam**.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. Uzak masaüstünde *myVm1*, PowerShell'i açın.

1. `ping myVm2` yazın.

    Bu arka gibi alırsınız:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudap
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Internet Denetim İletisi Protokolü (ICMP) kullandığından ping başarısız olur. Varsayılan olarak, Windows Güvenlik Duvarı üzerinden ICMP'ye izin verilmiyor.

1. İzin vermek için *myVm2* ping *myVm1* daha sonraki bir adımda bu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    ICMP komut sağlar Windows Güvenlik Duvarı üzerinden, gelen.

1. *myVm1* ile uzak masaüstü bağlantısını kapatın.

1. Adımları yineleyin [internet'ten bir sanal Makineye bağlanma](#connect-to-a-vm-from-the-internet). Bu kez, bağlanmak *myVm2*.

1. *myVm2* sanal makinesindeki bir komut isteminden `ping myvm1` komutunu girin.

    Bu arka gibi alırsınız:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Önceki bir adımda *myVm1* sanal makinesinde Windows güvenlik duvarı üzerinden ICMP’ye izin verdiğinizden, *myVm1*’den yanıt alırsınız.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve sanal makineleri ile işiniz bittiğinde kullanın [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) kaynak grubunu ve sahip tüm kaynakları kaldırmak için:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet’ten bir sanal makineye bağlandınız ve sanal makine ile başka bir sanal makine arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar. Varsayılan olarak, Azure yalnızca gelen Uzak Masaüstü bağlantıları için Windows Vm'leri internet'ten sağlar. Farklı türde bir VM ağ iletişimini yapılandırma hakkında daha fazla bilgi edinmek için Git [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğretici.
