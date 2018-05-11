---
title: Azure özel bir VHD'den bir Windows VM oluşturma | Microsoft Docs
description: Resource Manager dağıtım modelinde kullanarak işletim sistemi diski olarak özel bir yönetilen disk ekleyerek yeni bir Windows VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: be7933b038fb5a648249e9b0c73415bff778930b
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>PowerShell kullanarak özel bir diskten bir Windows VM oluşturma

Özel bir yönetilen disk işletim sistemi diski olarak ekleyerek yeni bir VM oluşturun. Özelleştirilmiş bir disk kullanıcı hesapları, uygulamaları ve diğer Durum verilerini orijinal VM tutar mevcut bir VM'yi sanal sabit disk (VHD) bir kopyasını arasındadır. 

Yeni bir VM oluşturmak için özel bir VHD kullandığınızda, yeni VM orijinal VM bilgisayar adını korur. Diğer bilgisayar özgü bilgiler de olması tutulur ve bazı durumlarda, bu yinelenen bilgiler sorunlara neden. Ne tür bilgisayara özgü bilgileri uygulamalarınızı VM kopyalarken kullanır dikkat edin.

Birkaç seçeneğiniz vardır:
* [Varolan bir yönetilen diski kullanmak](#option-1-use-an-existing-disk). Düzgün çalışmayan bir VM'niz varsa, bu yararlıdır. VM yeniden yeni bir VM oluşturmak için yönetilen disk silebilirsiniz. 
* [Bir VHD’yi karşıya yükleme](#option-2-upload-a-specialized-vhd) 
* [Var olan bir Azure VM anlık görüntülerini kullanarak kopyalayın](#option-3-copy-an-existing-azure-vm)

Azure portal için de kullanabilirsiniz [özelleştirilmiş bir VHD'den yeni bir VM oluşturmak](create-vm-specialized-portal.md).

Bu konuda yönetilen diskleri kullanmayı gösterir. Eski dağıtım varsa gerektiren bir depolama hesabı kullanarak, bkz: [depolama hesabındaki özelleştirilmiş bir VHD'den bir VM oluşturma](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Başlamadan önce
PowerShell'i kullanırsanız, AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>Seçenek 1: varolan bir diski kullan

Sildiğiniz bir VM olan ve yeni bir VM oluşturun, kullanmak için işletim sistemi diski yeniden istiyorsanız [Get-AzureRmDisk](/azure/powershell/get-azurermdisk).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Şimdi bu disk için işletim sistemi diski olarak iliştirebilirsiniz bir [yeni VM](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Seçenek 2: özel bir VHD yüklemek

Hyper-V ya da VM başka bir buluttan dışarı gibi bir şirket içi sanallaştırma aracı ile oluşturulan özel bir VM VHD'den karşıya yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
VHD olarak kullanmak istiyorsanız-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandıktan emin olun. 
  
  * [Windows Azure'a yüklemeniz VHD hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Sağlamadığı** Sysprep kullanarak VM genelleştirin.
  * Konuk sanallaştırma araçları ve (gibi VMware araçları) VM yüklenmiş aracıları kaldırın.
  * VM kendi IP adresini ve DNS ayarlarını DHCP yoluyla isteyecek şekilde yapılandırıldığından emin olun. Bu başladığında sunucu VNet içindeki bir IP adresi alacağını sağlar. 


### <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VHD depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun. 

Kullanılabilir depolama hesaplarını görüntülemek için aşağıdakileri yazın:

```powershell
Get-AzureRmStorageAccount
```

Varolan bir depolama hesabı kullanmak istiyorsanız, devam [VHD'nin yüklenmesi](#upload-the-vhd-to-your-storage-account) bölümü.

Bir depolama hesabı oluşturmanız gerekiyorsa, şu adımları izleyin:

1. Depolama hesabı nerede oluşturulacağını kaynak grubunun adı gerekir. Aboneliğinizde olan tüm kaynak grupları bulmak için şunu yazın:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Adlı bir kaynak grubu oluşturmak için *myResourceGroup* içinde *Batı ABD* bölge, türü:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Adlı depolama hesabı oluşturma *mystorageaccount* kullanarak bu kaynak grubundaki [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Depolama hesabınız VHD karşıya yükle 
Kullanım [Ekle AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) depolama hesabınızdaki bir kapsayıcıya VHD yüklemek için cmdlet'i. Bu örnek dosya karşıya yükleme *myVHD.vhd* gelen `"C:\Users\Public\Documents\Virtual hard disks\"` bir depolama hesabına adlı *mystorageaccount* içinde *myResourceGroup* kaynak grubu. Dosya adında kapsayıcısında depolanır *mycontainer* ve yeni dosya adı *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Başarılı olursa, şuna benzer bir yanıt alın:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Ağ bağlantısı ve VHD dosyasının boyutuna bağlı olarak, bu komutun tamamlanması biraz zaman alabilir

### <a name="create-a-managed-disk-from-the-vhd"></a>Yönetilen bir disk VHD'den oluştur

Depolama hesabı kullanarak özel VHD'den yönetilen bir disk oluşturmak [yeni AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnek kullanır **myOSDisk1** disk adı için disk koyar *Standard_LRS* depolama ve kullandığı *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* VHD kaynağı için URI olarak.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Yeni işletim sistemi diski karşıya yüklenen VHD'den oluşturun. 

```powershell
$sourceUri = (https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Seçenek 3: mevcut bir Azure VM'yi kopyalama

Bir VM anlık görüntüsü tarafından yönetilen disklerde kullanır ve ardından bu anlık görüntü kullanarak yeni bir oluşturmak için disk ve yeni bir VM yönetilen VM bir kopyasını oluşturabilirsiniz.


### <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diski bir anlık görüntüsünü

(Tüm diskler dahil), anlık görüntü ve tüm VM alabilir veya yalnızca tek bir disk. Aşağıdaki adımları kullanarak VM, yalnızca işletim sistemi diski bir anlık görüntüsünü nasıl Göster [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

Bazı parametreler ayarlayın. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

VM nesnesini alın.

```powershell
$vm = Get-AzureRmVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
İşletim sistemi disk adı alın.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Anlık görüntü yapılandırmasını oluşturun. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Anlık görüntü alın.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Yüksek performanslı olması gereken bir VM oluşturmak için anlık görüntü kullanmayı planlıyorsanız, parametresini kullanın `-AccountType Premium_LRS` yeni AzureRmSnapshot komutu. Parametre anlık görüntü oluşturur, böylece yönetilen bir Premium Disk depolanır. Premium yönetilen diskleri standart daha pahalıdır. Bu nedenle, Premium parametresi kullanmadan önce gerçekten emin olun.

### <a name="create-a-new-disk-from-the-snapshot"></a>Yeni bir disk anlık görüntüden oluştur

Anlık görüntü kullanımından yönetilen bir disk oluşturmak [yeni AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnekte *myOSDisk* disk adı.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

İşletim sistemi disk adı ayarlayın. 

```powershell
$osDiskName = 'myOsDisk'
```

Yönetilen diski oluşturun.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Yeni VM oluşturma 

Yeni VM tarafından kullanılmak üzere ağ ve diğer VM kaynakları oluşturun.

### <a name="create-the-subnet-and-vnet"></a>VNet ve alt ağ oluşturma

VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

Alt ağ oluşturun. Bu örnek adlı bir alt ağı oluşturur **mySubNet**, kaynak grubundaki **myDestinationResourceGroup**ve alt ağ adresi öneki ayarlar **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Sanal ağ oluşturun. Bu örnek olarak sanal ağ adını ayarlar **myVnetName**, konuma **Batı ABD**ve sanal ağ adres öneki **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
RDP kullanarak VM'nizi oturum açmak bağlantı noktası 3389 üzerinde RDP erişimine izin veren bir güvenlik kuralı olması gerekir. VHD yeni VM için var olan bir özel sanal makineden oluşturulduğundan, kaynak sanal makinedeki bir hesap için RDP kullanabilirsiniz.

Bu örnek NSG adını ayarlar **myNsg** ve RDP kural adı **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Uç noktaları ve NSG kuralları hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bir VM için bağlantı noktaları açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Bir ortak IP adresi ve NIC oluşturun
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

Genel IP oluşturun. Bu örnekte, ortak IP adresi adı ayarlamak **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

NIC oluşturun Bu örnekte, NIC adı ayarlamak **myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location -SubnetId $vnet.Subnets[0].Id `
   -PublicIpAddressId $pip.Id `
   -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>Sanal makine adını ve boyutunu ayarlama

Bu örnek VM adını ayarlar *myVM* ve VM boyutu *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekleme
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>İşletim sistemi diski Ekle 

Kullanarak yapılandırma işletim sistemi diski ekleyin [kümesi AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Bu örnek için disk boyutunu ayarlar *128 GB* ve yönetilen diski olarak bağlayan bir *Windows* işletim sistemi diski.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>VM tamamlayın 

VM kullanarak oluşturduğunuz [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)yeni oluşturduğumuz yapılandırmaları.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Bu komutun başarılı olduysa, benzer bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Yeni oluşturulan VM ya da görmeniz gerekir, [Azure portal](https://portal.azure.com)altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makine için oturum açın. Daha fazla bilgi için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md).

