---
title: Özelleştirilmiş bir VHD'yi azure'da Windows VM oluşturma | Microsoft Docs
description: Resource Manager dağıtım modeli kullanarak özel bir yönetilen diski işletim sistemi diski olarak ekleyerek yeni bir Windows VM oluşturun.
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
ms.date: 10/10/2018
ms.author: cynthn
ms.openlocfilehash: e90c5953456e585c9fb804fef1384b9e5b9ee912
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115815"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-by-using-powershell"></a>PowerShell kullanarak özel bir diskten Windows VM oluşturma

Özel bir yönetilen diski işletim sistemi diski olarak ekleyerek yeni bir VM oluşturun. Özelleştirilmiş disk bir sanal sabit disk (VHD) kullanıcı hesaplarını, uygulamaları ve orijinal VM'yi diğer Durum verilerini içeren mevcut bir VM'den kopyasıdır. 

Yeni bir VM oluşturmak için özelleştirilmiş bir VHD'yi kullandığınızda, özgün VM bilgisayar adı yeni VM'yi korur. Diğer bilgisayara özgü bilgileri de tutulur ve bazı durumlarda, yinelenen bu bilgileri sorunlara neden. Bir VM kopyalama yapılırken, bilgisayara özgü bilgilerin ne tür uygulamalarınızın kullanabileceği dikkat edin.

Birkaç seçeneğiniz vardır:
* [Mevcut bir yönetilen disk kullanmak](#option-1-use-an-existing-disk). Düzgün çalışmayan bir VM'niz varsa, bu seçenek kullanışlıdır. VM'yi silin ve yeni bir VM oluşturmak için yönetilen disk daha sonra yeniden kullanabilirsiniz. 
* [Bir VHD’yi karşıya yükleme](#option-2-upload-a-specialized-vhd) 
* [Anlık görüntüleri kullanarak mevcut bir Azure VM'yi kopyalama](#option-3-copy-an-existing-azure-vm)

Azure portalında da kullanabilirsiniz [özelleştirilmiş bir VHD'den yeni VM oluşturma](create-vm-specialized-portal.md).

Bu makalede, yönetilen diskleri kullanma işlemini göstermektedir. Eski dağıtım varsa gerektiren bir depolama hesabı kullanma, bkz: [bir depolama hesabında özelleştirilmiş bir VHD'den VM oluşturma](sa-create-vm-specialized.md).

## <a name="before-you-begin"></a>Başlamadan önce
PowerShell kullanmak için AzureRM.Compute PowerShell modülünün en son sürümüne sahip olduğunuzdan emin olun. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Daha fazla bilgi için [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>1. seçenek: var olan bir diski kullanma

Sildiğiniz bir sanal makine vardı ve yeni bir VM oluşturun, kullanın işletim sistemi diskini yeniden kullanmak istiyorsanız [Get-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermdisk?view=azurermps-6.8.1).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Artık bu disk için işletim sistemi diski olarak iliştirilebilir bir [yeni VM](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>2. seçenek: özelleştirilmiş bir VHD'yi karşıya yükleme

Hyper-V veya sanal makine başka bir buluttan dışarı gibi bir şirket içi sanallaştırma aracı ile oluşturulan özel bir VM VHD yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
VHD'yi olarak kullanın-yeni bir VM oluşturmaktır. 
  
  * [Windows Azure'a karşıya yüklenecek VHD'yi hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Sağlamadığı** Sysprep kullanarak VM'yi Genelleştirme.
  * Herhangi bir konuk sanallaştırma araçları ve (örneğin, VMware araçları) VM'de yüklü aracıları kaldırın.
  * VM, IP adresi ve DNS ayarları DHCP'den almak için yapılandırıldığından emin olun. Bu, başladığında sunucu sanal ağ içindeki bir IP adresi alacağını sağlar. 


### <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VHD depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabı kullanabilir veya yeni bir tane oluşturun. 

Kullanılabilir depolama hesaplarını gösterir.

```powershell
Get-AzureRmStorageAccount
```

Mevcut bir depolama hesabı kullanmak için devam etmek için [VHD'nizi karşıya yüklemeyi](#upload-the-vhd-to-your-storage-account) bölümü.

Depolama hesabı oluşturma.

1. Depolama hesabının oluşturulacağı kaynak grubunun adı gerekir. Kullanım Get-AzureRmResourceGroup aboneliğinizdeki tüm kaynak grupları bakın.
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Adlı bir kaynak grubu oluşturma *myResourceGroup* içinde *Batı ABD* bölge.

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Adlı bir depolama hesabı oluşturma *mystorageaccount* kullanarak yeni kaynak grubunda [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet'i.
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>VHD, depolama hesabınıza yükleme 
Kullanım [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) VHD'nin depolama hesabınızdaki bir kapsayıcıya yüklemek için cmdlet'i. Bu örnek dosyayı yükler *myVHD.vhd* gelen "C:\Users\Public\Documents\Virtual sabit diskleri\" adlı bir depolama hesabına *mystorageaccount* içinde  *myResourceGroup* kaynak grubu. Dosya adında bir kapsayıcıda depolanır *mycontainer* ve yeni dosya adı *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Komutlar başarılı olursa, şuna benzer bir yanıt elde edersiniz:

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

Bu komut, ağ bağlantınızı ve VHD dosyasının boyutuna bağlı olarak tamamlanması biraz sürebilir.

### <a name="create-a-managed-disk-from-the-vhd"></a>VHD'den yönetilen disk oluşturma

Kullanarak, depolama hesabınızdaki özelleştirilmiş bir VHD'den yönetilen disk oluşturma [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnekte *myOSDisk1* disk disk adı için koyar *Standard_LRS* depolama ve kullandığı *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* kaynak VHD URI'sini olarak.

Yeni VM için yeni bir kaynak grubu oluşturun.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Karşıya yüklenen VHD'den yeni işletim sistemi diski oluşturun. 

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

Bir VM anlık görüntüsü tarafından yönetilen diskler kullanır ve ardından bu anlık görüntü kullanarak yeni bir disk ve yeni bir sanal makine yönetilen bir sanal makinenin bir kopyasını oluşturabilirsiniz.


### <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diskinin anlık görüntüsünü alma

(Tüm diskler dahil) VM'nin tamamını veya tek bir diskin anlık görüntüsünü alabilir. Yalnızca işletim sistemi diski ile bir sanal makinenizin anlık görüntüsünü almak nasıl bir aşağıdaki adımlarda [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

İlk olarak, bazı parametrelerini ayarlayın. 

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

Anlık görüntüsünü alın.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Yüksek performanslı olmasını gerektiren bir VM oluşturmak için bu anlık görüntüyü kullanmak için parametreyi eklemek `-AccountType Premium_LRS` yeni AzureRmSnapshot komutu. Premium yönetilen Disk olarak depolanır, böylece bu parametre, anlık görüntü oluşturur. Premium yönetilen diskler standart daha pahalıdır, bu nedenle bu parametre kullanmadan önce Premium gerekir emin olun.

### <a name="create-a-new-disk-from-the-snapshot"></a>Anlık görüntüden yeni bir disk oluşturma

Kullanarak anlık görüntüden yönetilen disk oluşturma [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnekte *myOSDisk* disk adı.

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

Yönetilen disk oluşturun.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Yeni VM oluşturma 

Yeni sanal makine tarafından kullanılan ağ ve VM kaynakları oluşturun.

### <a name="create-the-subnet-and-virtual-network"></a>Alt ağ ve sanal ağ oluşturma

Oluşturma [sanal ağ](../../virtual-network/virtual-networks-overview.md) ve sanal makine için alt ağ.

1. Alt ağ oluşturun. Bu örnek adlı bir alt ağ oluşturur *mySubNet*, kaynak grubundaki *myDestinationResourceGroup*ve alt ağ adres ön eki ayarlar *10.0.0.0/24*.
   
    ```powershell
    $subnetName = 'mySubNet'
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
       -Name $subnetName `
       -AddressPrefix 10.0.0.0/24
    ```
    
2. Sanal ağı oluşturun. Bu örnekte sanal ağ adı ayarlar *myVnetName*, konuma *Batı ABD*ve sanal ağa ait adres ön ekini *10.0.0.0/16*. 
   
    ```powershell
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork `
       -Name $vnetName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AddressPrefix 10.0.0.0/16 `
       -Subnet $singleSubnet
    ```    
    

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
Sanal makinenize Uzak Masaüstü Protokolü (RDP) ile oturum açabilmesi için 3389 numaralı bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralı olması gerekir. Kaynak sanal makinede RDP için var olan bir hesabı kullanabilmeniz için Bizim örneğimizde, var olan özel bir sanal makineden yeni sanal makine için VHD oluşturuldu.

Bu örnekte ağ güvenlik grubu (NSG) adını ayarlar *myNsg* ve RDP kuralının adını *myRdpRule*.

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

Uç noktaları ve NSG kuralları hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bağlantı noktalarını VM'ye açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Bir genel IP adresi ve ağ Arabirimi oluşturma
Sanal ağda sanal makineyle iletişimi etkinleştirmek için ihtiyacınız olacak bir [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve bir ağ arabirimi.

1. Genel IP oluşturun. Bu örnekte, genel IP adresi adı kümesine *myIP*.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress `
       -Name $ipName -ResourceGroupName $destinationResourceGroup `
       -Location $location `
       -AllocationMethod Dynamic
    ```       
    
2. NIC oluşturma Bu örnekte, NIC adı kümesine *myNicName*.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName `
       -ResourceGroupName $destinationResourceGroup `
       -Location $location -SubnetId $vnet.Subnets[0].Id `
       -PublicIpAddressId $pip.Id `
       -NetworkSecurityGroupId $nsg.Id
    ```
    


### <a name="set-the-vm-name-and-size"></a>VM adını ve boyutunu ayarlayın

Bu örnekte VM adını ayarlar *myVM* ve VM boyutu için *standard_a2 =*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekleme
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>İşletim sistemi diski Ekle 

İşletim sistemi diski kullanarak yapılandırmaya ekleyin [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Bu örnek için disk boyutunu ayarlar *128 GB* ve yönetilen diski olarak bağlayan bir *Windows* işletim sistemi diski.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>VM tamamlayın 

Kullanarak VM oluşturma [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) oluşturduğumuz yapılandırmaya sahip.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Bu komutun başarılı olursa, şunun gibi bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Sanal Makinenin oluşturulduğunu doğrulayın.
Yeni oluşturulan VM ya da görmeniz gerekir, [Azure portalında](https://portal.azure.com) altında **Gözat** > **sanal makineler**, veya aşağıdaki PowerShell komutlarını kullanarak.

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenize oturum açın. Daha fazla bilgi için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md).

