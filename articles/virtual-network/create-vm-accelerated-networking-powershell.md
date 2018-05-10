---
title: Hızlandırılmış ağ ile bir Azure sanal makine oluşturma | Microsoft Docs
description: Hızlandırılmış ağ ile Linux sanal makine oluşturmayı öğrenin.
services: virtual-network
documentationcenter: ''
author: gsilva5
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: gsilva
ms.openlocfilehash: de69cdf69f30639d048dccd7d433c86f6cb9db7b
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile Windows sanal makine oluşturma

Bu öğreticide, bir Windows sanal makine (VM) ağ hızlandırılmış oluşturmayı öğrenin. Hızlandırılmış ağ ile bir Linux VM oluşturmak için bkz: [hızlandırılmış ağ ile bir Linux VM oluşturma](create-vm-accelerated-networking-cli.md). Hızlandırılmış ağ önemli ölçüde ağ performansını iyileştirme, bir VM tek köklü g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu gecikme, değişim ve desteklenen VM türlerinde zorlu ağ iş yükleri ile kullanmak için CPU kullanımını azaltır ancak konaktan datapath atlar. Aşağıdaki resimde, iki VM ile ve hızlandırılmış ağ olmadan arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ desteği olmadan, VM ve dışındaki tüm ağ trafiğini konak ve sanal anahtar çapraz geçiş gerekir. Sanal anahtar ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiği sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorunluluğu sağlar. Sanal anahtarları hakkında daha fazla bilgi için okuma [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx) makalesi.

Hızlandırılmış ağ ile ağ trafiğini VM ağ arabiriminin (NIC) ulaştığında ve ardından VM iletilir. Sanal anahtar uygular tüm ağ ilkeleri artık Boşaltılan ve donanım uygulanır. Donanım ilkesinde uygulama konak ve sanal anahtarın konak uygulanan tüm ilke korurken atlayarak doğrudan VM'ye iletme ağ trafiğini NIC'ye sağlar.

Hızlandırılmış ağ yararları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Sanal ağlar veya içi bağlantı üzerinden iletişim kurarken, bu özellik genel gecikme en az bir etkisi yoktur.

## <a name="benefits"></a>Avantajlar
* **Düşük gecikme / saniye başına daha yüksek paket (pps):** datapath sanal anahtarı kaldırmak ana bilgisayar ilke işleme için paketler harcadığı zamanı kaldırır ve VM içinde işlenebilecek paketlerin sayısını artırır.
* **Değişim azaltılmış:** sanal anahtar işleme uygulanması gereken ilke miktarını ve işlem yaptığını CPU iş yüküne bağlıdır. İlke zorlama donanım yük boşaltma, VM iletişim ve tüm yazılım kesmelerini ve İçerik Geçişi konağa kaldırma doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** konakta sanal anahtar atlayarak müşteri adayları ağ trafiğini işlemek için daha az CPU kullanımı için.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Kutunun Azure galerisinden dışında aşağıdaki dağıtımlar desteklenir: 
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 Datacenter** 

### <a name="supported-vm-instances"></a>Desteklenen VM örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem iyileştirilmiş örnek boyutları 2 veya daha fazla Vcpu'lar ile desteklenir.  Bu desteklenen serisi şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destek örneklerinde hızlandırılmış ağ 4 veya daha fazla Vcpu'lar VM örnekleriyle desteklenir. Desteklenen serisi şunlardır: D/DSv3, E/ESv3, Fsv2 ve Ms/Mms

VM örnekleri hakkında daha fazla bilgi için bkz: [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Bölgeler
Tüm ortak Azure bölgeleri ve Azure Bulutu kullanılabilir.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Hızlandırılmış ağ üzerinde çalışan bir VM etkinleştirme
Desteklenen bir VM boyutu hızlandırılmış ağ olmadan yalnızca özelliğinin durduruldu ve serbest etkin olabilir.

### <a name="deployment-through-azure-resource-manager"></a>Dağıtım aracılığıyla Azure Kaynak Yöneticisi
Sanal makineler (Klasik), ağ hızlandırılmış dağıtılamıyor.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Windows VM ile Azure hızlandırılmış ağ oluşturma

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

## <a name="enable-accelerated-networking-on-existing-vms"></a>Hızlandırılmış ağ mevcut vm'lerde etkinleştir
Bir VM hızlandırılmış ağ olmadan oluşturduysanız, mevcut bir VM'yi bu özelliği etkinleştirmek mümkündür.  VM hızlandırılmış ağ da yukarıda özetlenen aşağıdaki önkoşulları karşılayarak desteklemesi gerekir:

* VM hızlandırılmış ağ için desteklenen bir boyut olmalıdır
* VM desteklenen Azure galeri görüntüsü (ve Linux için çekirdek sürümü) olmalıdır
* Tüm sanal makineleri bir kullanılabilirlik kümesinde veya VMSS hızlandırılmış ağ üzerindeki herhangi bir NIC etkinleştirmeden önce durduruldu ve serbest olmalıdır

### <a name="individual-vms--vms-in-an-availability-set"></a>Tek tek sanal makineleri ve sanal makineleri bir kullanılabilirlik kümesi
İlk Dur / VM serbest bırakma veya bir kullanılabilirlik kümesi ise, kümedeki tüm sanal makineleri:

```azurepowershell
Stop-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Önemli, lütfen VM'yi ayrı ayrı bir kullanılabilirlik kümesi, yalnızca oluşturulduysa Not gereken hızlandırılmış ağ iletişimi etkinleştirmek için tek tek VM Dur/serbest bırakma.  VM'yi bir kullanılabilirlik kümesiyle oluşturduysanız, kullanılabilirlik kümesinde bulunan tüm VM'ler hızlandırılmış ağ NIC'ler hiçbirinde etkinleştirmeden önce durduruldu ve serbest olması gerekir. 

Hizmet durdurulduğunda hızlandırılmış ağ, VM NIC üzerinde etkinleştirin:

```azurepowershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzureRmNetworkInterface
```

VM veya, eğer bir kullanılabilirlik kümesi, kümedeki tüm sanal makineleri yeniden başlatın ve hızlandırılmış ağ etkinleştirilmiş olduğunu doğrulayın: 

```azurepowershell
Start-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS biraz farklıdır, ancak aynı iş akışı izler.  İlk olarak, sanal makineleri durdurun:

```azurepowershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Sanal makineleri durdurulduğunda, ağ arabirimi'nin altındaki hızlandırılmış ağ özelliğini güncelleştirin:

```azurepowershell
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Lütfen unutmayın, bir VMSS otomatik, çalışırken ve el ile olmak üzere üç farklı ayarları kullanarak güncelleştirmeleri uygulamak VM yükseltmeler sahiptir.  VMSS değişiklikleri hemen yeniden başlatıldıktan sonra almak için bu yönergeleri ilke otomatik olarak ayarlanır.  Değişiklikler hemen çekilir şekilde otomatik olarak ayarlamak için: 

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Son olarak, VMSS yeniden başlatın:

```azurepowershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Bir kez, yeniden başlatın, yükseltmeleri bitmesini bekleyin, ancak, VF VM içinde görünür tamamlandıktan sonra.  (Lütfen desteklenen bir işletim sistemi ve VM boyutu kullandığınızdan emin olun)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Hızlandırılmış ağ ile var olan sanal makineleri yeniden boyutlandırma

Hızlandırılmış etkin ağ ile sanal makineleri yalnızca hızlandırılmış ağ destek sanal makineleri yeniden boyutlandırılabilir.  

Hızlandırılmış etkin ağ ile VM hızlandırılmış yeniden boyutlandırma işlemi kullanarak ağ desteklemeyen bir VM örneğine boyutlandırılamaz.  Bunun yerine, bu Vm'lere birini yeniden boyutlandırmak için: 

* VM'yi Durdur/Deallocate veya bir kullanılabilirlik kümesi/VMSS varsa içinde Dur/set/VMSS tüm Vm'lerde serbest bırakma.
* Hızlandırılmış ağ VM veya bir kullanılabilirlik kümesi/VMSS, kümesi/VMSS tüm Vm'lerde içindeki IF NIC üzerinde devre dışı bırakılması gerekir.
* Hızlandırılmış ağ devre dışı bırakıldığında VM/kullanılabilirlik kümesi/VMSS hızlandırılmış ağ desteklemez ve yeniden yeni bir boyut için taşınabilir.  



