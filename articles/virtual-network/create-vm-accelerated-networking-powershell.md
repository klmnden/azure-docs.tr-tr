---
title: Hızlandırılmış ağ ile bir Azure sanal makinesi oluşturun | Microsoft Docs
description: Hızlandırılmış ağ ile bir Linux sanal makinesi oluşturmayı öğrenin.
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
ms.openlocfilehash: ef6086afa17f1ab864d70678a6da6df2a78e0c16
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190286"
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir Windows sanal makinesi oluşturun

Bu öğreticide, hızlandırılmış ağ ile Windows sanal makine (VM) oluşturma konusunda bilgi edinin. Hızlandırılmış ağ ile bir Linux VM oluşturmak için bkz [hızlandırılmış ağ ile bir Linux VM oluşturma](create-vm-accelerated-networking-cli.md). Hızlandırılmış ağ, ağ performansını önemli ölçüde geliştirmek, bir sanal makineye tek kökte g/ç Sanallaştırması (SR-IOV) sağlar. Bu yüksek performanslı yolu, gecikme süresi, değişim ve CPU kullanımı, En zorlu ağ iş yükleri desteklenen VM türleri ile kullanmak için azaltma datapath konaktan atlar. Aşağıdaki resimde, hızlandırılmış ağ bağlantısı olmadan ve iki sanal makineler arasındaki iletişimi gösterir:

![Karşılaştırma](./media/create-vm-accelerated-networking/accelerated-networking.png)

Hızlandırılmış ağ bağlantısı olmadan konak ve sanal anahtar ve VM'ye giden tüm ağ trafiğini çapraz gerekir. Sanal anahtarı, ağ güvenlik grupları, erişim denetim listeleri, yalıtım ve diğer ağ trafiğini sanallaştırılmış Ağ Hizmetleri gibi tüm ilke zorlaması sağlar. Sanal anahtarlar hakkında daha fazla bilgi için bkz: [Hyper-V ağ sanallaştırma ve sanal anahtar](https://technet.microsoft.com/library/jj945275.aspx).

Hızlandırılmış ağlı ağ trafiğini sanal makinenin ağ arabirimine (NIC) ulaşır ve ardından VM iletilir. Sanal anahtarın uygulandığı tüm ağ ilkeleri artık Boşaltılan ve donanım uygulanır. Donanım ilkede uygulama konak ve sanal anahtar konağa uygulanan tüm ilke korurken atlayarak doğrudan VM ağ trafiği NIC'ye sağlar.

Hızlandırılmış ağ avantajları yalnızca üzerinde etkin VM için de geçerlidir. En iyi sonuçlar için en az iki VM aynı Azure sanal ağa (VNet) bağlı bu özelliği etkinleştirmek idealdir. Bu özellik, sanal ağlar veya şirket bağlantı iletişim kurarken, toplam gecikme süresi çok az etkisi sahiptir.

## <a name="benefits"></a>Avantajlar
* **Daha düşük gecikme süresi / daha yüksek paket / saniye (pps):** Sanal anahtar datapath kaldırılıyor ana bilgisayar ilke işleme için paketler için harcadığınız zamanı kaldırır ve sanal makine içinde işlenebilecek paketlerin sayısını artırır.
* **Daha az değişim:** Sanal anahtar işleme uygulanması gereken ilke miktarını ve işleme yapılıyor CPU iş yüküne bağlıdır. İlke zorlaması için donanım yük boşaltma, konak VM iletişim ve tüm yazılım kesmelerini ve bağlam anahtarları kaldırılıyor doğrudan VM paketleri sunarak bu değişkenlik kaldırır.
* **Düşük CPU kullanımı:** Konakta sanal anahtar atlayarak ağ trafiğini işlemek için daha az CPU kullanımına neden olur.

## <a name="limitations-and-constraints"></a>Sınırlamalar ve kısıtlamalar

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Azure Galerisi hazır aşağıdaki dağıtımlar desteklenir:
* **Windows Server 2016 Datacenter** 
* **Windows Server 2012 R2 veri merkezi**

### <a name="supported-vm-instances"></a>Desteklenen sanal makine örnekleri
Hızlandırılmış ağ en genel amaçlı ve işlem için iyileştirilmiş örnek boyutları Vcpu, 2 veya daha fazla ile desteklenir.  Bu desteklenen serisi şunlardır: D/DSv2 ve F/Fs

Hiper iş parçacığı destek örneklerinde, hızlandırılmış ağ ile Vcpu, 4 veya daha fazla VM örneklerinde desteklenir. Desteklenen serisi şunlardır: D/Dsv3, E/Esv3, Fsv2, Lsv2, Ms/Mms ve Ms/Mmsv2.

Sanal makine örnekleri hakkında daha fazla bilgi için bkz. [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Bölgeler
Tüm genel Azure bölgeleri ve Azure kamu bulutunda kullanılabilir.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Hızlandırılmış ağ çalışan bir VM'deki etkinleştirme
Desteklenen bir VM boyutu hızlandırılmış ağ etkin olmadan yalnızca özelliğinin durdurulur ve serbest etkin olabilir.

### <a name="deployment-through-azure-resource-manager"></a>Azure Resource Manager üzerinden dağıtım
Hızlandırılmış ağ ile sanal makineler (Klasik) dağıtılamıyor.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Azure hızlandırılmış ağ ile bir Windows VM oluşturma
## <a name="portal-creation"></a>Portal oluşturma
Bu makale Azure Powershell kullanarak hızlandırılmış ağ ile bir sanal makine oluşturmak için adımları sağlar ancak ayrıca [hızlandırılmış ağ ile Azure portalını kullanarak bir sanal makine oluşturma](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Portalda, sanal makine oluştururken **sanal makine oluşturma** dikey penceresinde, seçin **ağ** sekmesi.  Bu sekmede için bir seçenek yoktur **hızlandırılmış**.  Seçtiyseniz bir [işletim sistemi desteklenen](#supported-operating-systems) ve [VM boyutu](#supported-vm-instances), bu seçenek "Açık." otomatik olarak doldurulur  Aksi halde, hızlandırılmış ağ için "Kapalı" seçeneği doldurmak ve kullanıcıya neden olan olması etkin bir neden sağlayın.   
* *Not:* Desteklenen işletim sistemleri, portal üzerinden etkinleştirilebilir.  Lütfen özel bir görüntü kullanıyorsanız ve hızlandırılmış ağ görüntünüzü destekliyorsa, CLI veya Powershell kullanarak VM oluşturma. 

Sanal makine oluşturulduktan sonra aşağıdaki yönergeleri accelerated networking Onayla etkin hızlandırılmış ağ etkin doğrulayabilirsiniz.

## <a name="powershell-creation"></a>PowerShell oluşturma
## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Yükleme [Azure PowerShell](/powershell/azure/install-az-ps) sürüm 1.0.0 veya üzeri. Şu anda yüklü sürümü bulmak için çalıştırın `Get-Module -ListAvailable Az`. Yüklemeniz veya yükseltmeniz gerekirse, Az modülünden'ın en son sürümünü yüklemek [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az). Bir PowerShell oturumunda bir Azure hesabı kullanarak oturum açtığınız [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount).

Aşağıdaki örneklerde, örnek parametre adları kendi değerlerinizle değiştirin. Örnek parametre adları dahil *myResourceGroup*, *Mynıc*, ve *myVM*.

Bir kaynak grubu oluşturun [yeni AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup). Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *centralus* konumu:

```powershell
New-AzResourceGroup -Name "myResourceGroup" -Location "centralus"
```

İlk olarak, bir alt ağ yapılandırması ile oluşturma [yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/New-azVirtualNetworkSubnetConfig). Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır *mySubnet*:

```powershell
$subnet = New-AzVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

İle sanal ağ oluşturma [yeni AzVirtualNetwork](/powershell/module/az.Network/New-azVirtualNetwork), ile *mySubnet* alt ağ.

```powershell
$vnet = New-AzVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

İlk olarak, bir ağ güvenlik grubu kuralı oluşturun [yeni AzNetworkSecurityRuleConfig](/powershell/module/az.Network/New-azNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzNetworkSecurityRuleConfig `
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

Bir ağ güvenlik grubu oluşturun [yeni AzNetworkSecurityGroup](/powershell/module/az.Network/New-azNetworkSecurityGroup) ve atama *Allow-RDP-All* güvenlik kuralı. Ek olarak *Allow-RDP-All* kural, ağ güvenlik grubu, çeşitli varsayılan kurallar içerir. Varsayılan kurallardan biri olmasının nedeni Internet'ten gelen tüm erişimi devre dışı bırakır *Allow-RDP-All* kural oluşturulduktan sonra sanal makineye uzaktan bağlanabilmesi için ağ güvenlik grubuna atanmış.

```powershell
$nsg = New-AzNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Ağ güvenlik grubuyla ilişkilendirdiğiniz *mySubnet* alt ağ ile [kümesi AzVirtualNetworkSubnetConfig](/powershell/module/az.Network/Set-azVirtualNetworkSubnetConfig). Alt ağda dağıtılan tüm kaynaklar için ağ güvenlik grubu kuralı etkilidir.

```powershell
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Hızlandırılmış ağ ile bir ağ arabirimi oluşturma
Bir genel IP adresiyle oluşturma [yeni AzPublicIpAddress](/powershell/module/az.Network/New-azPublicIpAddress). Internet'ten sanal makineye erişmek için ancak bu makaledeki adımları tamamlayabilmeniz için planlamıyorsanız bir genel IP adresi gerekli değilse, gerekli değildir.

```powershell
$publicIp = New-AzPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Bir ağ arabirimi ile oluşturma [yeni AzNetworkInterface](/powershell/module/az.Network/New-azNetworkInterface) accelerated networking etkin olan ve ağ arabiriminde genel IP adresi atayın. Aşağıdaki örnekte adlı bir ağ arabirimi oluşturur *Mynıc* içinde *mySubnet* alt *myVnet* sanal ağ ve atadıkları kişiler *Mypublicıp*  genel IP adresini:

```powershell
$nic = New-AzNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Sanal makineyi oluşturma

VM kimlik bilgilerinizi ayarlayın `$cred` değişken kullanarak [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```powershell
$cred = Get-Credential
```

İlk olarak, VM ile tanımlama [yeni AzVMConfig](/powershell/module/az.compute/new-azvmconfig). Aşağıdaki örnek adlı bir VM tanımlar *myVM* hızlandırılmış ağ iletişimi destekleyen bir VM boyutu ile (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Tüm VM boyutları ve özelliklerini bir listesi için bkz. [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

VM yapılandırmanızı geri kalanını oluşturmak [kümesi AzVMOperatingSystem](/powershell/module/az.compute/set-azvmoperatingsystem) ve [kümesi AzVMSourceImage](/powershell/module/az.compute/set-azvmsourceimage). Aşağıdaki örnek, bir Windows Server 2016 VM oluşturur:

```powershell
$vmConfig = Set-AzVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Daha önce oluşturduğunuz ağ arabirimini Ekle [Ekle AzVMNetworkInterface](/powershell/module/az.compute/add-azvmnetworkinterface):

```powershell
$vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Son olarak, ile VM oluşturma [New-AzVM](/powershell/module/az.compute/new-azvm):

```powershell
New-AzVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Sürücü işletim sisteminde yüklü onaylayın

Azure'da VM oluşturduktan sonra VM'ye bağlanın ve sürücü Windows içinde yüklü olduğunu onaylayın.

1. Azure bir Internet tarayıcısında açın [portalı](https://portal.azure.com) ve Azure hesabınızla oturum açın.
2. Metni içeren kutuya *kaynak Ara* Azure portalının üst kısmında, yazın *myVm*. Zaman **myVm** görünür arama sonuçlarında tıklayın. Varsa **oluşturma** altında görülebilir **Connect** düğmesi, Azure henüz tamamlanmadı VM oluşturma. Tıklayın **Connect** genel bakış yalnızca sonra sol üst köşesindeki artık bkz **oluşturma** altında **Connect** düğmesi.
3. Kullanıcı adını ve parolasını girdiğiniz [sanal makineyi oluşturduğunuzda](#create-the-virtual-machine). Azure'da Windows VM için hiçbir zaman bağlandıysanız olup [sanal makineye bağlanma](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Windows Başlat düğmesini sağ tıklatıp **cihaz Yöneticisi**. Genişletin **ağ bağdaştırıcıları** düğümü. Onaylayın **Mellanox ConnectX-3 sanal işlev Ethernet bağdaştırıcısı** , aşağıdaki resimde gösterildiği gibi görünür:

    ![Cihaz Yöneticisi](./media/create-vm-accelerated-networking/device-manager.png)

Hızlandırılmış ağ, sanal Makineniz için etkinleştirilmiştir.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Hızlandırılmış ağ mevcut vm'lerde etkinleştir
Hızlandırılmış ağ olmayan bir VM oluşturduysanız, varolan bir VM'yi bu özelliği etkinleştirmek mümkündür.  Sanal Makineyi hızlandırılmış ağ, yukarıda özetlenen aşağıdaki gereksinimleri karşılayarak desteklemelidir:

* Sanal Makineyi hızlandırılmış ağ için desteklenen bir boyut olmalıdır
* VM, desteklenen Azure galeri görüntüsü (ve Linux için çekirdek sürümü) olmalıdır.
* Bir kullanılabilirlik kümesindeki tüm sanal makineler veya VMSS hızlandırılmış ağ nıc'lerden birine etkinleştirmeden önce durdurulmuş/serbest bırakılmış olmalıdır

### <a name="individual-vms--vms-in-an-availability-set"></a>Tek Vm'leri & Vm'leri bir kullanılabilirlik kümesi
İlk Durdur/VM'yi serbest bırakın veya bir kullanılabilirlik kümesi, kümedeki tüm sanal makineler:

```azurepowershell
Stop-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Önemli, lütfen ayrı ayrı bir kullanılabilirlik kümesi, yalnızca sanal makinenizin oluşturulduysa Not gerekir hızlandırılmış ağ iletişimi etkinleştirmek için tek tek VM'yi Durdur/serbest bırakın.  Sanal makinenize bir kullanılabilirlik kümesi ile oluşturulmuş olsa bile, kullanılabilirlik kümesinde yer alan tüm Vm'lere hızlandırılmış ağ NIC'lerin herhangi biri üzerinde etkinleştirmeden önce durdurulmuş/serbest bırakılmış olması gerekir. 

Hizmet durdurulduğunda, hızlandırılmış ağ sanal makinenizin NIC'de etkinleştir:

```azurepowershell
$nic = Get-AzureRMNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzureRMNetworkInterface
```

Bir kullanılabilirlik kümesi, kümedeki tüm sanal makineler, VM veya, eğer yeniden ve hızlandırılmış ağ etkin olduğunu doğrulayın:

```azurepowershell
Start-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS biraz farklıdır ama aynı iş akışını izler.  İlk olarak, Vm'leri durdurun:

```azurepowershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

VM durdurulduktan sonra ağ arabirimi'nin altındaki hızlandırılmış ağ özelliğini güncelleştirin:

```azurepowershell
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Lütfen bir VMSS Not otomatik, sıralı ve el ile olmak üzere üç farklı ayarları kullanarak güncelleştirmeleri uygulamak VM yükseltmeleri sahiptir.  VMSS hemen yeniden başlattıktan sonra değişiklikleri alır, böylece bu Yönergelerdeki ilke otomatik olarak ayarlanır.  Değişiklikler hemen alınır şekilde otomatik olarak ayarlamak için:

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Son olarak, VMSS yeniden başlatın:

```azurepowershell
Start-AzVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"
```

Bir kez, yeniden, yükseltmeleri tamamlamak ancak bir kez tamamlandı, VF VM içinde görünür.  (Lütfen desteklenen bir işletim sistemi ve VM boyutu kullandığınızdan emin olun)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Hızlandırılmış ağ ile mevcut Vm'leri yeniden boyutlandırma

Hızlandırılmış ağ etkin Vm'lerle yalnızca VM'ler için hızlandırılmış ağ destekleyen boyutlandırılabilir.  

Hızlandırılmış ağ etkin olan bir VM yeniden boyutlandırma işlemi kullanarak hızlandırılmış ağ desteği olmayan bir VM örneğine yeniden boyutlandırılamaz.  Bunun yerine, bu Vm'lere birini yeniden boyutlandırmak için:

* VM'yi Durdur/serbest veya bir kullanılabilirlik kümesi/VMSS varsa içinde Durdur/set/VMSS tüm sanal makineler serbest bırakın.
* Hızlandırılmış ağ VM veya bir kullanılabilirlik kümesi/VMSS, küme/VMSS içindeki tüm sanal makineleri sahipse NIC üzerinde devre dışı bırakılmalıdır.
* Hızlandırılmış ağ devre dışı bırakıldıktan sonra VM/kullanılabilirlik kümesi/VMSS hızlandırılmış ağ desteklemez ve yeniden yeni bir boyut için taşınabilir.
