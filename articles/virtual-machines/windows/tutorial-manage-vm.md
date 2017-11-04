---
title: "Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme | Microsoft Docs"
description: "Öğretici - oluşturun ve Azure PowerShell modülü ile Windows sanal makineleri yönetme"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2237f2e5cb67df019d0975e764602babe7f4c8f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Oluşturma ve Azure PowerShell modülü ile Windows sanal makineleri yönetme

Azure sanal makineler tam olarak yapılandırılabilir ve esnek bir bilgi işlem ortamı sağlar. Bu öğretici, bir VM boyutu seçerek, bir VM görüntüsü seçme ve bir VM dağıtma gibi temel Azure sanal makine dağıtım öğeleri kapsar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oluşturma ve bir VM'ye bağlanın
> * Seçin ve VM görüntüleri kullanmak
> * Görüntüleme ve belirli VM boyutları kullanma
> * VM’yi yeniden boyutlandırma
> * Görüntüleyin ve VM durumunu anlamak


[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutu ile yeni bir kaynak grubu oluşturun. 

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bir kaynak grubu bir sanal makine önce oluşturulması gerekir. Bu örnekte, bir kaynak grubu adında *myResourceGroupVM* oluşturulan *EastUS* bölge. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

Kaynak grubu oluştururken veya değiştirirken Bu öğretici görülebilir bir VM belirtilir.

## <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine bir sanal ağa bağlanması gerekir. Bir ağ arabirimi kartı bir ortak IP adresi kullanarak sanal makine ile iletişim kurar.

### <a name="create-virtual-network"></a>Sanal ağ oluşturma

Bir alt ağ ile oluşturma [yeni AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Bir sanal ağ ile oluşturma [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Ortak IP adresi oluştur

Bir ortak IP adresiyle oluşturma [yeni AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Ağ arabirimi kartı oluşturun

Bir ağ arabirimi kartı ile oluşturma [yeni AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Ağ güvenlik grubu oluşturun

Bir Azure [ağ güvenlik grubu](../../virtual-network/virtual-networks-nsg.md) (NSG) bir veya daha çok sanal makineler için gelen ve giden trafik denetler. Ağ güvenlik grubu kurallarının izin verin veya belirli bir bağlantı noktası veya bağlantı noktası aralığı ağ trafiği engelle. Önceden tanımlanmış bir kaynakta kaynaklanan trafiğin yalnızca bir sanal makineyle iletişim kurabilmesi için bu kurallar kaynak adres öneki de içerir. Yüklemekte olduğunuz IIS Web sunucusu erişmek için bir gelen NSG kuralı eklemeniz gerekir.

Gelen bir NSG kuralı oluşturmak için kullanın [Ekle AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Aşağıdaki örnek, adlandırılmış bir NSG kuralı oluşturur *myNSGRule* bağlantı noktası açar *3389* sanal makine için:

```azurepowershell-interactive
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

NSG kullanarak oluşturduğunuz *myNSGRule* ile [yeni AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Alt ağ ile sanal ağ içindeki NSG eklemek [kümesi AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Sanal ağ ile güncelleştirme [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```azurepowershell-interactive
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Sanal makine oluşturma

Bir sanal makine oluştururken, işletim sistemi görüntüsü, disk boyutlandırma ve yönetici kimlik bilgileri gibi birkaç seçenek bulunur. Bu örnekte, bir sanal makine adı ile oluşturulan *myVM* Windows Server 2016 Datacenter'ın en son sürümünü çalıştıran.

Kullanıcı adı ve parola ile sanal makinede yönetici hesabı için gerekli ayarlayın [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Sanal makine için başlangıç yapılandırmasını oluşturma [yeni AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Sanal makine yapılandırması için işletim sistemi bilgilerini ekleyin [kümesi AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```azurepowershell-interactive
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Sanal makine yapılandırmasıyla görüntü bilgilerini eklemek [kümesi AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Sanal makine yapılandırması için işletim sistemi disk ayarları eklemek [kümesi AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Ekle, önceden oluşturduğunuz sanal makine yapılandırması için ağ arabirim kartı [Ekle AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```azurepowershell-interactive
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a>VM'ye bağlanın

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

Sanal makinenin genel IP adresini döndürmek için aşağıdaki komutları çalıştırın. Sonraki bir adımda web bağlantısını test etmek üzere tarayıcınızla bağlanabilmek için bu IP Adresini not alın.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Sanal makine ile bir uzak masaüstü oturumu oluşturmak için yerel makinenizde aşağıdaki komutu kullanın. IP adresini, sanal makinenizin *publicIPAddress* değeriyle değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>VM görüntüleri anlama

Azure Market yeni bir sanal makine oluşturmak için kullanılan çok sayıda sanal makine görüntülerini içerir. Önceki adımlarda, bir sanal makine Windows Server 2016-Datacenter görüntüsü kullanılarak oluşturuldu. Bu adımda, PowerShell modülü de yeni VM'ler için temel olarak kullanabilirsiniz diğer Windows görüntüleri Market aramak için kullanılır. Bu işlem, yayımcı, teklif ve görüntü adı (Sku) bulma oluşur. 

Kullanım [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) görüntü yayımcıların listesini döndürmek için komutu.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Kullanım [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) görüntü teklifleri listesini döndürmek için. Bu komutla, belirtilen yayımcı üzerinde döndürülen listesi filtrelenir. 

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

[Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) komutu daha sonra resim adları listesini döndürmek için yayımcı ve teklif adına filtre.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Bu bilgiler, belirli bir resim'yle bir VM'yi dağıtmak için kullanılabilir. Bu örnek, görüntü adı VM nesnesinde ayarlar. Bu öğreticide tam dağıtım adımları için önceki örneklere başvurun.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>VM boyutları anlama

Bir sanal makine boyutu, sanal makine için kullanılabilir hale getirilir işlem kaynaklarını CPU, GPU ve bellek gibi miktarını belirler. Sanal makineler expect iş yükü için uygun bir boyutu ile oluşturulması gerekir. İş yükü artarsa, var olan bir sanal makine yeniden boyutlandırılabilir.

### <a name="vm-sizes"></a>VM boyutları

Aşağıdaki tabloda, kullanım örneklerine boyutları kategorilere ayırır.  

| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Genel amaçlı         |DSv2, Dv2, DS, D, Av2, A0-7| Dengeli CPU bellekten. Geliştirme için ideal / test ve küçük ve orta uygulamaları ve verileri çözümler.  |
| İşlem için iyileştirilmiş      | FS, F             | Yüksek CPU bellekten. Orta düzey trafik uygulamalar, ağ uygulamaları ve toplu işlemler için iyidir.        |
| Bellek için iyileştirilmiş       | GS, G, DSv2, DS, Dv2, D   | Yüksek bellek için-çekirdek. İlişkisel veritabanları, Orta ve büyük önbellekler ve bellek içi analizi için mükemmel.                 |
| Depolama için iyileştirilmiş       | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| GPU           | NV, NC            | Yoğun Grafik işleme ve video düzenleme için hedeflenen özel VM'ler.       |
| Yüksek performans | H, A8-11          | Bizim en güçlü CPU VM'ler isteğe bağlı yüksek verimlilik ağ arabirimlerine (RDMA) sahip. 


### <a name="find-available-vm-sizes"></a>Kullanılabilir VM boyutları Bul

Belirli bir bölgede kullanılabilir VM boyutlarının listesini görmek için [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) komutu.

```azurepowershell-interactive
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>VM’yi yeniden boyutlandırma

Bir VM dağıtıldıktan sonra artırmak veya kaynak ayırma azaltmak için yeniden boyutlandırılabilir.

Bir VM'yi yeniden boyutlandırılırken önce istenen boyut geçerli VM kümede kullanılabilir olup olmadığını denetleyin. [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) komut boyutlarının listesini döndürür. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

İstenen boyut varsa, işlemi sırasında yeniden başlatılıncaya kadar ancak VM bir gücü açma durumundan boyutlandırılabilir.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

İstenen boyut geçerli kümede değilse, VM yeniden boyutlandırma işlemi oluşabilmesi için öncelikle serbest gerekir. , VM geri açık olduğundan, geçici diskteki tüm verilerin kaldırılır ve statik bir IP adresi kullanılmadığı sürece genel IP adresi değişikliği unutmayın. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>VM güç durumları

Bir Azure VM birçok güç durumlarını birine sahip. Bu durum, hiper yönetici açısından VM geçerli durumunu temsil eder. 

### <a name="power-states"></a>Güç durumları

| Güç durumu | Açıklama
|----|----|
| Başlangıç | Sanal makinenin başlatıldığı gösterir. |
| Çalışıyor | Sanal makinenin çalıştığını gösterir. |
| Durduruluyor | Sanal makinenin durdurulması olduğunu gösterir. | 
| Durduruldu | Sanal makine durdurulduğunda gösterir. Sanal makine durdurulmuş durumunda hala bilgi işlem ücretleri olduğunu unutmayın.  |
| Ayırmayı kaldırma | Sanal makine ayırması olduğunu gösterir. |
| Serbest bırakıldı | Sanal makine hiper yönetici alanından tamamen kaldırılacak, ancak denetim düzeyi hala kullanılabilir olduğunu gösterir. Deallocated durumunda sanal makineler bilgi işlem ücretleri değil. |
| - | Sanal makinenin güç durumunun bilinmediğini gösterir. |

### <a name="find-power-state"></a>Güç durumu Bul

Belirli bir VM durumunu almak için kullanın [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) komutu. Bir sanal makine ve kaynak grubu için geçerli bir ad belirttiğinizden emin olun. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName myResourceGroupVM `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Çıktı:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Yönetim görevleri

Bir sanal makine yaşam döngüsü sırasında başlatma, durdurma veya bir sanal makine silme gibi yönetim görevleri çalıştırmak isteyebilirsiniz. Ayrıca, yinelenen veya karmaşık görevleri otomatikleştirmek için komut dosyaları oluşturmak isteyebilirsiniz. Azure PowerShell kullanarak, birçok ortak yönetim görevlerinin komut satırından veya komut dosyalarında çalıştırabilirsiniz.

### <a name="stop-virtual-machine"></a>Sanal makineyi durdurma

Durdurun ve bir sanal makineyle ayırması [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Sanal makine sağlanan bir durumda tutmak istiyorsanız, - StayProvisioned parametresini kullanın.

### <a name="start-virtual-machine"></a>Sanal makineyi Başlat

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Kaynak grubunu silme

Bir kaynak grubunu silme içinde içerdiği tüm kaynaklar da siler.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel VM oluşturmayı ve yönetmeyi nasıl gibi hakkında öğrenilen:

> [!div class="checklist"]
> * Oluşturma ve bir VM'ye bağlanın
> * Seçin ve VM görüntüleri kullanmak
> * Görüntüleme ve belirli VM boyutları kullanma
> * VM’yi yeniden boyutlandırma
> * Görüntüleyin ve VM durumunu anlamak

VM diskleri hakkında bilgi edinmek için sonraki öğretici ilerleyin.  

> [!div class="nextstepaction"]
> [Oluşturma ve yönetme VM diskleri](./tutorial-manage-data-disk.md)
