---
title: Bir generalize Azure'da birden çok VM oluşturmak için VHD karşıya yükle | Microsoft Docs
description: Resource Manager dağıtım modeliyle kullanmak için bir Windows VM oluşturmak için Azure depolama hesabınız için genelleştirilmiş VHD yükleme.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 5b38d022d372e7d35ba2dbeaef90660ce95f73fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60250747"
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a>Yeni bir VM oluşturmak için Azure'da genelleştirilmiş VHD yükleme

Bu konu, genelleştirilmiş bir yönetilmeyen disk bir depolama hesabına yüklemeye ve sonra karşıya yüklenen diski kullanarak yeni bir VM oluşturmayı kapsamaktadır. Genelleştirilmiş bir VHD görüntüsü tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırılan oluşturdu. 

Bir depolama hesabında özelleştirilmiş bir VHD'den VM oluşturmak istiyorsanız, bkz. [özelleştirilmiş bir VHD'den VM oluşturma](sa-create-vm-specialized.md).

Depolama hesaplarını kullanarak bu konuda ele alınmaktadır ancak bunun yerine yönetilen diskler müşteriler taşıma öneririz. Hazırlama, karşıya yükleme ve yönetilen diskleri kullanarak yeni bir VM oluşturma hakkında tam talimatları için bkz [oluştur yeni bir VM genelleştirilmiş bir VHD'den yönetilen Diskler'i kullanarak Azure'a yüklenmiş](upload-generalized-managed.md).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Genelleştirilmiş VHD tüm kişisel hesap bilgilerinizi Sysprep kullanarak kaldırılan oluşturdu. VHD, yeni sanal makineler oluşturmak için bir görüntü olarak kullanmak istiyorsanız, şunları yapmalısınız:
  
  * [Windows Azure'a karşıya yüklenecek VHD'yi hazırlama](prepare-for-upload-vhd-image.md). 
  * Sysprep kullanarak sanal makineyi Genelleştir

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Sysprep kullanarak bir Windows sanal makineyi Genelleştir
Bu bölümde, Windows sanal makinenizi bir görüntü olarak kullanılmaya generalize gösterilir. Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep işlemini kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> VHD'nizi Azure'a ilk kez yüklemeden önce Sysprep çalıştırıyorsanız, olduğundan emin olun [sanal makinenizin hazır](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

1. Windows sanal makinede oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizinine **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.
3. **Sistem Hazırlama Aracı** iletişim kutusunda  **Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam** düğmesine tıklayın.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. 

> [!IMPORTANT]
> Azure'a VHD yükleme veya VM'den görüntü oluşturma işlemi kadar sanal Makineyi yeniden başlatmayın. Sanal makine yanlışlıkla yeniden, yeniden genelleştirmek için Sysprep çalıştırın.
> 
> 


## <a name="upload-the-vhd"></a>VHD'yi karşıya yükleme

VHD bir Azure depolama hesabına yükleyin.

### <a name="log-in-to-azure"></a>Azure'da oturum açma
PowerShell sürüm 1.4 yoksa veya üstü yüklü okuma [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın. Azure hesabı kimlik bilgilerinizi girmeniz için bir pencere açılır.
   
    ```powershell
    Connect-AzAccount
    ```
2. ' % S'abonelik kimliği için mevcut aboneliklerinizi alın.
   
    ```powershell
    Get-AzSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliğin ayarlayın Değiştirin `<subscriptionID>` doğru aboneliğin kimliği.
   
    ```powershell
    Select-AzSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VM görüntüsü depolamak için Azure depolama hesabında ihtiyacınız var. Mevcut bir depolama hesabı kullanabilir veya yeni bir tane oluşturun. 

Kullanılabilir depolama hesaplarını göstermek için şunu yazın:

```powershell
Get-AzStorageAccount
```

Karşıya yükleme için mevcut bir depolama hesabı kullanmak istiyorsanız, VM görüntü bölümü devam edin.

Bir depolama hesabı oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:

1. Burada depolama hesabının oluşturulması gereken kaynak grubunun adını ihtiyacınız vardır. Aboneliğinizdeki tüm kaynak gruplarını bulmak için şunu yazın:
   
    ```powershell
    Get-AzResourceGroup
    ```

    Adlı bir kaynak grubu oluşturmak için **myResourceGroup** içinde **Batı ABD** bölge, türü:

    ```powershell
    New-AzResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Adlı bir depolama hesabı oluşturma **mystorageaccount** kullanarak bu kaynak grubundaki [yeni AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet:
   
    ```powershell
    New-AzStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a>Karşıya Yüklemeyi Başlat 

Kullanım [Ekle AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) görüntüyü depolama hesabınızdaki bir kapsayıcıya yüklemek için cmdlet'i. Bu örnek dosyayı yükler **myVHD.vhd** gelen `"C:\Users\Public\Documents\Virtual hard disks\"` adlı bir depolama hesabına **mystorageaccount** içinde **myResourceGroup** kaynak grubu. Dosya adlı bir kapsayıcının içine yerleştirilecek **mycontainer** ve yeni dosya adı **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
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

Ağ bağlantınızı ve VHD dosyasının boyutuna bağlı olarak, bu komutun tamamlanması biraz sürebilir.


## <a name="create-a-new-vm"></a>Yeni VM oluşturma 

Karşıya yüklenen VHD artık yeni bir VM oluşturmak için de kullanabilirsiniz. 

### <a name="set-the-uri-of-the-vhd"></a>VHD URI'sini ayarlayın

Kullanılacak VHD için URI şu biçimdedir: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Bu örnekte adlı VHD **myVHD** depolama hesabında **mystorageaccount** kapsayıcısında **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
Sanal ağ oluşturup alt [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Aşağıdaki örnek adlı bir alt ağ oluşturur **mySubnet** kaynak grubundaki **myResourceGroup** adres ön eki ile **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağı oluşturun. Aşağıdaki örnek adlı bir sanal ağ oluşturur **myVnet** içinde **Batı ABD** konumu adres ön eki ile **10.0.0.0/16**.  
   
    ```powershell
    $location = "WestUS"
    $vnetName = "myVnet"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Genel bir IP adresi ve ağ arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Genel bir IP adresi oluşturun. Bu örnek adlı bir genel IP adresi oluşturur **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC oluşturma Bu örnek adlı bir NIC oluşturur **Mynıc**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
Sanal makinenizde RDP kullanarak oturum açmak 3389 numaralı bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralı olması gerekir. 

Bu örnekte adlı bir NSG oluşturulur **myNsg** adlı kuralı içeren **myRdpRule** 3389 numaralı bağlantı noktası üzerinden RDP trafiğine izin veren. Nsg'ler hakkında daha fazla bilgi için bkz. [PowerShell kullanarak Azure'da bağlantı noktalarını VM'ye açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için bir değişken oluşturun
Tamamlanan sanal ağ için bir değişken oluşturun. 

```powershell
$vnet = Get-AzVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a>Sanal makine oluşturma
Aşağıdaki PowerShell Betiği, sanal makine yapılandırmalarını ve karşıya yüklenen VM görüntüsü, yeni yükleme için kaynak olarak kullanmak gösterilmektedir.



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
    $storageAcc = Get-AzStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Sanal Makinenin oluşturulduğunu doğrulayın.
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portalında](https://portal.azure.com) altında **Gözat** > **sanal makineler**, veya aşağıdaki PowerShell kullanarak komutlar:

```powershell
    $vmList = Get-AzVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell ile yeni sanal makinenize yönetmek için bkz: [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


