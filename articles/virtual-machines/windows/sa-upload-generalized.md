---
title: "Generalize birden çok VM oluşturmak için VHD karşıya | Microsoft Docs"
description: "Resource Manager dağıtım modeli ile kullanmak için bir Windows VM oluşturmak için bir Azure depolama hesabı genelleştirilmiş bir VHD yüklemek."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: e6fc49855b449a7723a7f8a0c1c41516b3a44ee5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a>Yeni bir VM oluşturmak için Azure'da genelleştirilmiş bir VHD yüklemek

Bu konu, genelleştirilmiş bir yönetilmeyen disk bir depolama hesabına yükleniyor ve karşıya yüklenen diski kullanarak yeni bir VM oluşturma kapsar. Genelleştirilmiş bir VHD görüntüsü tüm kişisel hesap bilgilerinizi Sysprep kullanılarak kaldırılıncaya oluşturdu. 

Bir depolama hesabında özelleştirilmiş bir VHD'den bir VM oluşturmak istiyorsanız, bkz: [özelleştirilmiş bir VHD'den bir VM oluşturma](sa-create-vm-specialized.md).

Depolama hesaplarını kullanarak bu konu şunları içerir, ancak bunun yerine yönetilen diskleri kullanarak müşteriler taşıma öneririz. Hazırlama, karşıya yükleme ve yönetilen diskleri kullanarak yeni bir VM oluşturmak nasıl tam bir kılavuz için bkz: [yeni bir VM genelleştirilmiş bir VHD'den karşıya yönetilen diskleri kullanarak Azure oluşturma](upload-generalized-managed.md).



## <a name="prepare-the-vm"></a>VM’yi hazırlama

Genelleştirilmiş bir VHD tüm kişisel hesap bilgilerinizi Sysprep kullanılarak kaldırılıncaya oluşturdu. Yeni VM'lerin oluşturmak için bir resim olarak VHD kullanmayı düşünüyorsanız, aşağıdakileri yapmalısınız:
  
  * [Windows Azure'a yüklemeniz VHD hazırlama](prepare-for-upload-vhd-image.md). 
  * Sysprep kullanarak sanal makine generalize

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Sysprep kullanarak bir Windows sanal makine generalize
Bu bölümde bir görüntü olarak kullanmak için Windows sanal makine generalize gösterilmiştir. Sysprep tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Sysprep, VHD Azure'a ilk kez karşıya yüklemeden önce çalıştırıyorsanız olduğundan emin olun [VM'nizi hazırlanmış](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

1. Windows sanal makinede oturum açın.
2. Bir yönetici olarak komut istemi penceresi açın. Dizinine değiştirin **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda **girin sistem Out-of-Box deneyimi (OOBE)**, emin olun **Generalize** onay kutusu seçilidir.
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam** düğmesine tıklayın.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. 

> [!IMPORTANT]
> VHD için Azure yükleyerek veya sanal makineden bir görüntü oluşturma işleminiz kadar VM yeniden başlatmayın. VM yanlışlıkla yeniden, yeniden genelleştirmek için Sysprep'i çalıştırın.
> 
> 


## <a name="upload-the-vhd"></a>VHD'nin yüklenmesi

VHD için bir Azure depolama hesabı karşıya yükleyin.

### <a name="log-in-to-azure"></a>Azure'da oturum açma
PowerShell sürüm 1.4 zaten yoksa veya üstü yüklü okuma [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın. Azure hesabı kimlik bilgilerinizi girmeniz için bir açılır pencere açılır.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Abonelik kimlikleri kullanılabilir aboneliklerinizi alın.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliğin ayarlayın Değiştir `<subscriptionID>` doğru abonelik kimliği.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VM görüntüsü depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun. 

Kullanılabilir depolama hesaplarını görüntülemek için aşağıdakileri yazın:

```powershell
Get-AzureRmStorageAccount
```

Varolan bir depolama hesabı kullanmak istiyorsanız, devam [VM görüntüsü karşıya](#upload-the-vm-vhd-to-your-storage-account) bölümü.

Bir depolama hesabı oluşturmanız gerekiyorsa, şu adımları izleyin:

1. Depolama hesabı nerede oluşturulacağını kaynak grubunun adı gerekir. Aboneliğinizde olan tüm kaynak grupları bulmak için şunu yazın:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Adlı bir kaynak grubu oluşturmak için **myResourceGroup** içinde **Batı ABD** bölge, türü:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Adlı depolama hesabı oluşturma **mystorageaccount** kullanarak bu kaynak grubundaki [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a>Karşıya Yüklemeyi Başlat 

Kullanım [Ekle AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet'ini depolama hesabınızdaki bir kapsayıcıya görüntüyü karşıya yükleme. Bu örnek dosya karşıya yükleme **myVHD.vhd** gelen `"C:\Users\Public\Documents\Virtual hard disks\"` bir depolama hesabına adlı **mystorageaccount** içinde **myResourceGroup** kaynak grubu. Dosya adında kapsayıcısının içine yerleştirilecek **mycontainer** ve yeni dosya adı **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
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

Ağ bağlantısı ve VHD dosyasının boyutuna bağlı olarak, bu komutun tamamlanması biraz zaman alabilir.


## <a name="create-a-new-vm"></a>Yeni bir VM oluşturun 

Karşıya yüklenen VHD artık yeni bir VM oluşturmak için de kullanabilirsiniz. 

### <a name="set-the-uri-of-the-vhd"></a>VHD URI ayarlayın

Kullanmak VHD için URI şu biçimdedir: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Bu örnekte adlı VHD **myVHD** depolama hesabında olduğunu **mystorageaccount** kapsayıcısında **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Aşağıdaki örnek adlı bir alt ağı oluşturur **mySubnet** kaynak grubunda **myResourceGroup** adres öneki ile **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağı oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur **myVnet** içinde **Batı ABD** adres öneki ile konum **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Ortak IP adresi ve ağ arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Bir ortak IP adresi oluşturun. Bu örnek adlı ortak IP adresi oluşturur **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC oluşturun Bu örnek, adlandırılmış bir NIC oluşturur **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
RDP kullanarak VM'nizi oturum açmak bağlantı noktası 3389 üzerinde RDP erişimine izin veren bir güvenlik kuralı olması gerekir. 

Bu örnekte adlı bir NSG oluşturulur **myNsg** adlı kuralı içeren **myRdpRule** 3389 numaralı bağlantı noktası RDP trafiğine izin veren. Nsg'ler hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bir VM için bağlantı noktaları açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için bir değişken oluşturun
Tamamlanan sanal ağ için bir değişken oluşturun. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>Sanal makine oluşturma
Aşağıdaki PowerShell komut dosyası, sanal makine yapılandırmalarını ayarlamak ve karşıya yüklenen VM görüntüsü, yeni yükleme için kaynak olarak kullanmak gösterilmiştir.



```powershell
# Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portal](https://portal.azure.com) altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için bkz: [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


