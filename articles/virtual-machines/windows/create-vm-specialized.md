---
title: Özelleştirilmiş bir VHD'yi azure'da Windows VM oluşturma | Microsoft Docs
description: Özel bir yönetilen disk Resource Manager dağıtım modelinde kullanarak işletim sistemi diski olarak ekleyerek yeni bir Windows VM oluşturun.
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
ms.openlocfilehash: 34bfe7733c60337d6ab7d81c498d2fb0fd15e1fd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338495"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>PowerShell kullanarak özel bir diskten Windows VM oluşturma

Özel bir yönetilen diski işletim sistemi diski olarak ekleyerek yeni bir VM oluşturun. Özelleştirilmiş disk bir sanal sabit disk (VHD) kullanıcı hesaplarını, uygulamaları ve diğer Durum verilerini orijinal VM'yi korur mevcut bir VM'den kopyasıdır. 

Yeni bir VM oluşturmak için özelleştirilmiş bir VHD'yi kullandığınızda, özgün VM bilgisayar adı yeni VM'yi korur. Diğer bilgisayara özgü bilgileri de olması tutulur ve bazı durumlarda, yinelenen bu bilgileri sorunlara neden. Ne tür bilgisayara özgü bilgileri, uygulama bir VM kopyalarken dayanır dikkat edin.

Birkaç seçeneğiniz vardır:
* [Mevcut bir yönetilen disk kullanmak](#option-1-use-an-existing-disk). Düzgün çalışmayan bir VM'niz varsa, bu yararlıdır. Yeni bir VM oluşturmak için yönetilen disk sanal makine yeniden silebilirsiniz. 
* [Bir VHD’yi karşıya yükleme](#option-2-upload-a-specialized-vhd) 
* [Anlık görüntülerini kullanarak mevcut bir Azure VM'yi kopyalama](#option-3-copy-an-existing-azure-vm)

Azure portalında da kullanabilirsiniz [özelleştirilmiş bir VHD'den yeni VM oluşturma](create-vm-specialized-portal.md).

Bu konu başlığı altında yönetilen diskleri kullanma işlemini göstermektedir. Eski dağıtım varsa gerektiren bir depolama hesabı kullanma, bkz: [bir depolama hesabında özelleştirilmiş bir VHD'den VM oluşturma](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Başlamadan önce
PowerShell kullanıyorsanız, AzureRM.Compute PowerShell modülünün en son sürümüne sahip olduğunuzdan emin olun. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Daha fazla bilgi için [Azure PowerShell sürüm oluşturma](/powershell/azure/overview).

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
VHD olarak kullanmak istiyorsanız,-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandığından emin olun. 
  
  * [Windows Azure'a karşıya yüklenecek VHD'yi hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Sağlamadığı** Sysprep kullanarak VM'yi Genelleştirme.
  * Herhangi bir konuk sanallaştırma araçları ve (gibi VMware araçları) VM'de yüklü aracıları kaldırın.
  * VM, IP adresi ve DNS ayarlarını DHCP yoluyla çekmek için yapılandırılan emin olun. Bu, başladığında sunucunun sanal ağ içindeki bir IP adresi alacağını sağlar. 


### <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VHD depolamak için Azure depolama hesabında ihtiyacınız var. Mevcut bir depolama hesabı kullanabilir veya yeni bir tane oluşturun. 

Kullanılabilir depolama hesaplarını göstermek için şunu yazın:

```powershell
Get-AzureRmStorageAccount
```

Mevcut bir depolama hesabı kullanmak istiyorsanız, devam [VHD'nizi karşıya yüklemeyi](#upload-the-vhd-to-your-storage-account) bölümü.

Bir depolama hesabı oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. Burada depolama hesabının oluşturulması gereken kaynak grubunun adını ihtiyacınız vardır. Aboneliğinizdeki tüm kaynak gruplarını bulmak için şunu yazın:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Adlı bir kaynak grubu oluşturmak için *myResourceGroup* içinde *Batı ABD* bölge, türü:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Adlı bir depolama hesabı oluşturma *mystorageaccount* kullanarak bu kaynak grubundaki [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>VHD, depolama hesabınıza yükleme 
Kullanım [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) VHD'nin depolama hesabınızdaki bir kapsayıcıya yüklemek için cmdlet'i. Bu örnek dosyayı yükler *myVHD.vhd* gelen `"C:\Users\Public\Documents\Virtual hard disks\"` adlı bir depolama hesabına *mystorageaccount* içinde *myResourceGroup* kaynak grubu. Dosya adında bir kapsayıcıda depolanır *mycontainer* ve yeni dosya adı *myUploadedVHD.vhd*.

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

Ağ bağlantınızı ve VHD dosyasının boyutuna bağlı olarak, bu komutun tamamlanması biraz sürebilir

### <a name="create-a-managed-disk-from-the-vhd"></a>VHD'den yönetilen disk oluşturma

Depolama hesabı kullanarak özel bir VHD'den yönetilen disk oluşturma [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnekte **myOSDisk1** disk disk adı için koyar *Standard_LRS* depolama ve kullandığı *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* kaynak VHD URI'sini olarak.

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

Bir VM anlık görüntüsü tarafından yönetilen diskler kullanır, sonra bu anlık görüntü kullanarak yeni bir disk ve yeni bir sanal makine yönetilen bir sanal makinenin bir kopyasını oluşturabilirsiniz.


### <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diskinin anlık görüntüsünü alma

(Tüm diskler dahil), anlık görüntü ve tüm VM alabilir veya tek bir diskin. Aşağıdaki adımları uygulayarak vm'nizin yalnızca işletim sistemi diskinin anlık görüntüsünü almak nasıl Göster [yeni AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet'i. 

Bazı parametrelerini ayarlayın. 

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


Anlık görüntü, yüksek performanslı olması gereken bir VM oluşturmak için kullanmayı planlıyorsanız, ilgili parametreyi kullanın `-AccountType Premium_LRS` yeni AzureRmSnapshot komutu. Premium yönetilen Disk olarak depolanır, böylece parametresi anlık görüntü oluşturur. Premium yönetilen diskler, standart daha pahalıdır. Bu nedenle parametre kullanmadan önce Premium ihtiyacınız emin olun.

### <a name="create-a-new-disk-from-the-snapshot"></a>Anlık görüntüden yeni bir disk oluşturma

Anlık görüntü kullanarak yönetilen disk oluşturma [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Bu örnekte *myOSDisk* disk adı.

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

### <a name="create-the-subnet-and-vnet"></a>VNet ve alt ağ oluşturma

Sanal ağ oluşturup alt [sanal ağ](../../virtual-network/virtual-networks-overview.md).

Alt ağ oluşturun. Bu örnek adlı bir alt ağ oluşturur **mySubNet**, kaynak grubundaki **myDestinationResourceGroup**ve alt ağ adres ön eki ayarlar **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Sanal ağ oluşturun. Bu örnek, sanal ağ adı olacak şekilde ayarlar **myVnetName**, konuma **Batı ABD**ve sanal ağa ait adres ön ekini **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
Sanal makinenizde RDP kullanarak oturum açmak 3389 numaralı bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralı olması gerekir. Yeni VM için VHD'yi özelleştirilmiş mevcut bir VM'den oluşturulduğundan, RDP için kaynak sanal makineden bir hesap kullanabilirsiniz.

NSG adı Bu örnekte ayarlar **myNsg** ve RDP kuralının adını **myRdpRule**.

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
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

Genel IP oluşturun. Bu örnekte, genel IP adresi adı kümesine **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

NIC oluşturma Bu örnekte, NIC adı kümesine **myNicName**.
   
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

Yapılandırma kullanarak işletim sistemi diskini ekleyin [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Bu örnek için disk boyutunu ayarlar *128 GB* ve yönetilen diski olarak bağlayan bir *Windows* işletim sistemi diski.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>VM tamamlayın 

Kullanarak VM oluşturma [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)oluşturduğumuz yapılandırmaları.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Bu komut başarılı olduysa, şunun gibi bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Sanal Makinenin oluşturulduğunu doğrulayın.
Yeni oluşturulan VM ya da görmeniz gerekir, [Azure portalında](https://portal.azure.com)altında **Gözat** > **sanal makineler**, veya aşağıdaki PowerShell komutlarını kullanarak:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenize oturum açın. Daha fazla bilgi için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md).

