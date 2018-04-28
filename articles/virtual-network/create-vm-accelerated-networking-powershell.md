---
title: Hızlandırılmış ağ ile bir Azure sanal makine oluşturma | Microsoft Docs
description: Hızlandırılmış ağ ile Linux sanal makine oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: jdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: jimdial
ms.openlocfilehash: 995f40599c059434c419bea95019f8700f756ad8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile Windows sanal makine oluşturma

> [!IMPORTANT]
> Sanal makineler hızlandırılmış etkin ağ ile oluşturulması gerekir. Bu özellik, varolan sanal makinelere etkinleştirilemez. Hızlandırılmış ağ iletişimi etkinleştirmek için aşağıdaki adımları tamamlayın:
>   1. Sanal makineyi silin
>   2. Hızlandırılmış ağ ile sanal makine oluşturun
>

Bu öğreticide, bir Windows sanal makine (VM) ağ hızlandırılmış oluşturmayı öğrenin. Hızlandırılmış ağ önemli ölçüde ağ performansını iyileştirme, bir VM tek köklü g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu gecikme, değişim ve desteklenen VM türlerinde zorlu ağ iş yükleri ile kullanmak için CPU kullanımını azaltır ancak konaktan datapath atlar. Aşağıdaki resimde, iki VM ile ve hızlandırılmış ağ olmadan arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ desteği olmadan, VM ve dışındaki tüm ağ trafiğini konak ve sanal anahtar çapraz geçiş gerekir. Sanal anahtar ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiği sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorunluluğu sağlar. Sanal anahtarları hakkında daha fazla bilgi için okuma [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesi.

Hızlandırılmış ağ ile ağ trafiğini VM ağ arabiriminin (NIC) ulaştığında ve ardından VM iletilir. Sanal anahtar uygular tüm ağ ilkeleri artık Boşaltılan ve donanım uygulanır. Donanım ilkesinde uygulama konak ve sanal anahtarın konak uygulanan tüm ilke korurken atlayarak doğrudan VM'ye iletme ağ trafiğini NIC'ye sağlar.

Hızlandırılmış ağ yararları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Sanal ağlar veya içi bağlantı üzerinden iletişim kurarken, bu özellik genel gecikme en az bir etkisi yoktur.

## <a name="benefits"></a>Avantajlar
* **Düşük gecikme / saniye başına daha yüksek paket (pps):** datapath sanal anahtarı kaldırmak ana bilgisayar ilke işleme için paketler harcadığı zamanı kaldırır ve VM içinde işlenebilecek paketlerin sayısını artırır.
* **Değişim azaltılmış:** sanal anahtar işleme uygulanması gereken ilke miktarını ve işlem yaptığını CPU iş yüküne bağlıdır. İlke zorlama donanım yük boşaltma, VM iletişim ve tüm yazılım kesmelerini ve İçerik Geçişi konağa kaldırma doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** konakta sanal anahtar atlayarak müşteri adayları ağ trafiğini işlemek için daha az CPU kullanımı için.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Microsoft Windows Server 2012 R2 Datacenter ve Windows Server 2016.

## <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem iyileştirilmiş örnek boyutları 4 veya daha fazla Vcpu'lar ile desteklenir. Hiper iş parçacığı destekleyen örneklerinde D/DSv3 veya E/ESv3 gibi hızlandırılmış ağ 8 veya daha fazla Vcpu'lar ile VM örneklerinde desteklenir. Desteklenen serisi şunlardır: D/DSv2, D/DSv3, E/ESv3, Fs/F/Fsv2 ve Ms/Mms.

VM örnekleri hakkında daha fazla bilgi için bkz: [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Bölgeler
Tüm ortak Azure bölgeleri ve Azure Bulutu kullanılabilir.

## <a name="limitations"></a>Sınırlamalar
Bu özelliği kullanırken aşağıdaki sınırlamalar bulunmaktadır:

* **Ağ arabirimi oluşturma:** hızlandırılmış ağ yalnızca yeni bir NIC için etkinleştirilmesi Varolan bir NIC için etkinleştirilemez
* **VM oluşturma:** etkin hızlandırılmış ağ ile bir NIC yalnızca eklenebilir bir VM VM oluşturulduğunda. NIC için mevcut bir VM'yi eklenemiyor. VM için mevcut bir kullanılabilirlik ekleme ayarlarsanız, kullanılabilirlik kümesindeki tüm sanal makineleri de etkin ağ hızlandırılmış gerekir.
* **Yalnızca aracılığıyla Azure Resource Manager dağıtımı:** hızlandırılmış ağ ile sanal makineleri (Klasik) dağıtılamıyor.

Bu makale Azure PowerShell kullanarak hızlandırılmış ağ iletişimi ile bir sanal makine oluşturmak için adımlar sağlar ancak şunları da yapabilirsiniz. [ile hızlandırılmış ağ Azure portalını kullanarak bir sanal makine oluşturmak](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bir sanal makine portalında altında oluştururken **ayarları**seçin **etkin**altında **ağ hızlandırılmış**. Seçtiğiniz sürece hızlandırılmış ağ etkinleştirme seçeneği Portalı'nda görünmüyor bir [işletim sistemi desteklenen](#supported-operating-systems) ve [VM boyutu](#supported-vm-instances). Sanal makine oluşturulduktan sonra yönergeleri tamamlamak gereken [sürücü işletim sisteminde yüklü onaylayın](#confirm-the-driver-is-installed-in-the-operating-system).

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Yükleme [Azure PowerShell](/powershell/azure/install-azurerm-ps) sürüm 5.1.1 veya sonraki bir sürümü. Şu anda yüklü sürümünü bulmak için Çalıştır `Get-Module -ListAvailable AzureRM`. AzureRM modülünden en son sürümünü yüklemek veya yükseltmek gerekiyorsa, yükleme [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM). Bir PowerShell oturumunda bir Azure hesabı kullanarak oturum açtığınız [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *myNic*, ve *myVM*.

[New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup) komutunu kullanarak bir kaynak grubu oluşturun. Aşağıdaki örnek, bir kaynak grubu oluşturur *myResourceGroup* içinde *centralus* konumu:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "centralus"
```

İlk olarak, bir alt ağ yapılandırması ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetworkSubnetConfig). Aşağıdaki örnek adlı bir alt ağı oluşturur *mySubnet*:

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Bir sanal ağ ile oluşturma [New-AzureRmVirtualNetwork](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork), ile *mySubnet* alt ağ.

```powershell
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

İlk olarak, bir ağ güvenlik grubu kuralı oluşturma [yeni AzureRmNetworkSecurityRuleConfig](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzureRmNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
    -Description 'Allow RDP' `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 100 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389
```

Bir ağ güvenlik grubu oluşturma [yeni AzureRmNetworkSecurityGroup](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityGroup) ve Ata *izin RDP tüm* güvenlik kuralı. Ek olarak *izin RDP tüm* kural, ağ güvenlik grubu birkaç varsayılan kurallar içerir. Bir varsayılan kural, tüm gelen internetten olmasının nedeni, devre dışı bırakır *izin RDP tüm* kural, ağ güvenlik grubuna atanır, böylece bir kez oluşturulduğunda sanal makineye uzaktan bağlanabilirsiniz.

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *mySubnet* alt ağ ile [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/Set-AzureRmVirtualNetworkSubnetConfig). Ağ güvenlik grubu kuralında alt ağda dağıtılan tüm kaynaklar için geçerli değildir.

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Bir ağ arabirimi ile hızlandırılmış ağ oluşturma
[New-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/New-AzureRmPublicIpAddress) ile genel IP adresi oluşturun. Sanal makine Internet'ten erişmek için ancak bu makaledeki adımları tamamlamak için planlamıyorsanız bir ortak IP adresi gerekli değilse, gerekli değildir.

```powershell
$publicIp = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Bir ağ arabirimi oluştur [yeni AzureRmNetworkInterface](/powershell/module/AzureRM.Network/New-AzureRmNetworkInterface) ile etkin ağ hızlandırılmış ve ağ arabirimi genel IP adresi atayın. Aşağıdaki örnek adlı ağ arabirimi oluşturur *myNic* içinde *mySubnet* alt *myVnet* sanal ağ ve atar *myPublicIp*  ona genel IP adresi:

```powershell
$nic = New-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

VM kimlik bilgilerinizi ayarlamak `$cred` değişken kullanarak [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```powershell
$cred = Get-Credential
```

İlk olarak, VM ile tanımlama [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). Aşağıdaki örnek, adlandırılmış bir VM'nin tanımlar *myVM* hızlandırılmış ağ iletişimi destekleyen bir VM boyutu (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Tüm VM boyutları ve özellikleri listesi için bkz: [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

VM yapılandırmanızı kalan oluşturma [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) ve [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Aşağıdaki örnek, bir Windows Server 2016 VM oluşturur:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Daha önce ile oluşturulan ağ arabirimi ekleme [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Son olarak, VM oluşturma [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Sürücü işletim sisteminde yüklü doğrulayın

Azure'da VM oluşturduktan sonra VM'ye bağlanın ve Windows'da sürücüsünün yüklü olduğunu onaylayın.

1. Bir Internet tarayıcısından Azure açın [portal](https://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Metni içeren kutusunda *arama kaynakları* Azure portalının en üstünde yazın *myVm*. Zaman **myVm** görünür arama sonuçlarında tıklatın. Varsa **oluşturma** altında görülebilir **Bağlan** düğmesi, Azure henüz tamamlanmadı VM oluşturma. Tıklatın **Bağlan** yalnızca sonra genel bakış sol üst köşesindeki artık bkz **oluşturma** altında **Bağlan** düğmesi.
3. Kullanıcı adını ve parolasını girdiğiniz [sanal makine oluşturma](#create-the-virtual-machine). Bir Windows VM Azure hiçbir zaman bağlantı kurduğunuz olup [sanal makineye Bağlan](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Windows Başlat düğmesine sağ tıklatın ve **Aygıt Yöneticisi'ni**. Genişletme **ağ bağdaştırıcıları** düğümü. Onaylayın **Mellanox ConnectX-3 sanal işlev Ethernet bağdaştırıcısı** , aşağıdaki resimde gösterildiği gibi görünür:

    ![Cihaz Yöneticisi](./media/create-vm-accelerated-networking/device-manager.png)

Hızlandırılmış ağ, VM için etkinleştirilmiştir.
