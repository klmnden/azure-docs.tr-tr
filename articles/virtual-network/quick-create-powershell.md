---
title: Sanal ağ - hızlı başlangıç - Azure PowerShell oluşturma
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ, sanal makineler gibi Azure kaynaklarının sağlar, birbiriyle ve internet ile özel olarak iletişim.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: kumud
ms.openlocfilehash: 280649edd61e076a286b7e3dae44dbbf9b3e3b3e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61487875"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Hızlı Başlangıç: PowerShell kullanarak sanal ağ oluşturma

Sanal makineler (VM) gibi Azure kaynaklarını bir sanal ağ sağlar, birbiriyle ve internet ile özel olarak iletişim. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Ardından internet'ten sanal makinelere bağlanın ve sanal ağ üzerinden birbiriyle iletişim kurmasına.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip yerine karar verirseniz, bu hızlı başlangıçta Azure PowerShell modülü sürüm 1.0.0 kullanmanızı gerekli hale getirmiş veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Bkz: [Azure PowerShell modülü yükleme](/powershell/azure/install-az-ps) yükleme ve yükseltme bilgileri için.

PowerShell'i yerel olarak çalıştırıyorsanız, son olarak, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount`. Bu komut, Azure ile bir bağlantı oluşturur.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Bir kaynak grubunu ve sanal ağ oluşturma

Birkaç adımları, kaynak grubunu ve sanal ağ yapılandırılmış almak için size yol gerek vardır.

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Bir sanal ağ oluşturabilmeniz için önce sanal ağ'ı barındırmak için bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). Bu örnek adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). Bu örnek adlı varsayılan bir sanal ağ oluşturur *myVirtualNetwork* içinde *EastUS* konumu:

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Alt ağ ekleme

Bir alt ağ oluşturmanız gerekir, böylece azure kaynakları bir sanal ağ içindeki alt ağa dağıtır. Adlı bir alt ağ yapılandırması *varsayılan* ile [Ekle AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Sanal ağ alt ağını ilişkilendirin

Sanal ağ için alt ağ yapılandırmasını yazma [kümesi AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork). Bu komut, alt ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

İlk VM oluşturma [New-AzVM](/powershell/module/az.compute/new-azvm). Sonraki komutu çalıştırdığınızda, kimlik bilgileri istenir. VM için bir kullanıcı adı ve parola girin:

```azurepowershell-interactive
New-AzVm `
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
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Bu komutla birlikte ikinci bir sanal makine oluşturun:

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Başka bir kullanıcı adı ve parola oluşturmanız gerekir. Azure VM oluşturmak için birkaç dakika sürer.

> [!IMPORTANT]
> Azure'nın tamamlandı kadar sonraki adıma geçmeyin.  PowerShell'e çıktı döndürdüğünde yapıldığını anlarsınız.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Kullanım [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) bir VM'nin genel IP adresini döndürmek için. Bu örnekte genel IP adresini döndürür *myVm1* VM:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızda bir komut istemi açın. `mstsc` komutunu çalıştırın. Değiştirin `<publicIpAddress>` genel IP adresiyle son adımından döndürülen:

> [!NOTE]
> Bu komutları bir PowerShell isteminden yerel bilgisayarınızda çalıştırıyorsunuz ve Az PowerShell modülü sürüm 1.0 veya üzeri kullanıyorsanız, bu arabirimde devam edebilirsiniz.

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

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudapp.net
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

Sanal ağ ve sanal makineleri ile işiniz bittiğinde kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu ve sahip tüm kaynakları kaldırmak için:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet’ten bir sanal makineye bağlandınız ve sanal makine ile başka bir sanal makine arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure sanal makineler arasında Kısıtlanmamış özel iletişime olanak sağlar. Varsayılan olarak, Azure yalnızca gelen Uzak Masaüstü bağlantıları için Windows Vm'leri internet'ten sağlar. Farklı türde bir VM ağ iletişimini yapılandırma hakkında daha fazla bilgi edinmek için Git [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğretici.
