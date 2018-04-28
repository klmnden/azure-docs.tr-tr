---
title: Yönetilmeyen genelleştirilmiş VM görüntüsünü oluşturma | Microsoft Docs
description: Azure'da VM birden çok kopyasını oluşturmak için kullanılacak genelleştirilmiş bir Windows VM unmanged görüntüsü oluşturun.
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
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3737ea08e593ae1018489633e23e80e1099296ae
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a>Bir Azure sanal makineden bir yönetilmeyen VM görüntüsü oluşturma

Bu makalede, depolama hesapları kullanmayı ele alır. Bir depolama hesabı yerine yönetilen diskleri ve yönetilen görüntüleri kullanmanızı öneririz. Daha fazla bilgi için bkz: [genelleştirilmiş VM Azure ile yönetilen bir görüntüsünü yakalama](capture-image-resource.md).

Bu makalede bir depolama hesabı kullanarak Azure PowerShell genelleştirilmiş bir Azure VM görüntüsünü oluşturmak için nasıl kullanılacağı gösterilmektedir. Ardından, başka bir VM oluşturmak için görüntüyü kullanabilirsiniz. Görüntü, işletim sistemi diski ve sanal makineye bağlı veri disklerinden içerir. Yeni VM oluşturduğunuzda, bu kaynakları ayarlamak gereken şekilde görüntünün sanal ağ kaynaklarına içermez. 

## <a name="prerequisites"></a>Önkoşullar
Azure PowerShell sürüm gerek 1.0.x ya da daha yeni yüklü. PowerShell henüz yüklemediyseniz, okuma [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) yükleme adımları için.

## <a name="generalize-the-vm"></a>VM generalize 
Bu bölümde bir görüntü olarak kullanmak için Windows sanal makine generalize gösterilmiştir. Bir VM genelleme tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> VHD Azure'a ilk kez yüklüyorsanız olduğundan emin olun [VM'nizi hazırlanmış](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

Kullanarak bir Linux VM genelleştirmek `sudo waagent -deprovision+user` ve VM yakalama için PowerShell kullanın. Bir VM yakalama için CLI kullanma hakkında daha fazla bilgi için bkz: [nasıl generalize ve Azure CLI kullanarak bir Linux sanal makine yakalama ](../linux/capture-image.md).


1. Windows sanal makinede oturum açın.
2. Bir yönetici olarak komut istemi penceresi açın. Dizinine değiştirin **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda **girin sistem Out-of-Box deneyimi (OOBE)**, emin olun **Generalize** onay kutusu seçilidir.
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam**’a tıklayın.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. 

> [!IMPORTANT]
> VHD için Azure yükleyerek veya sanal makineden bir görüntü oluşturma işleminiz kadar VM yeniden başlatmayın. VM yanlışlıkla yeniden, yeniden genelleştirmek için Sysprep'i çalıştırın.
> 
> 

## <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma
1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın.
   
    ```powershell
    Connect-AzureRmAccount
    ```
   
    Azure hesabı kimlik bilgilerinizi girmeniz için bir açılır pencere açılır.
2. Abonelik kimlikleri kullanılabilir aboneliklerinizi alın.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliğin ayarlayın
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>VM serbest bırakma ve genelleştirilmiş durumuna ayarlayın
1. VM kaynakları serbest bırakma.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    *Durum* VM Azure portal değişiklikleri **durduruldu** için **durduruldu (serbest bırakıldı)**.
2. Sanal makine durumunu ayarlamak **Genelleştirmiş**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. VM durumunu denetleyin. **OSState ve genelleştirilmiş** VM'ye sahip olması gerekir için bölüm **DisplayStatus** kümesine **VM genelleştirilmiş**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Görüntü oluşturma

Bir yönetilmeyen sanal makine görüntüsü bu komutunu kullanarak hedef depolama kapsayıcısını oluşturun. Görüntü, özgün sanal makine aynı depolama hesabındaki oluşturulur. `-Path` Parametresi, yerel bilgisayarınıza bir kopyasını bir kaynak VM için JSON şablonunu kaydeder. `-DestinationContainerName` Parametredir görüntüleri tutmak istediğiniz kapsayıcısının adı. Kapsayıcı yoksa, sizin için oluşturulur.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
JSON dosyası şablonu görüntünüzü URL'sini alabilirsiniz. Git **kaynakları** > **storageProfile** > **osDisk** > **görüntü** > **URI** tam yol için bir bölüm görüntünüzün. Görüntünün URL'si aşağıdaki gibi görünür: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
Portal URI da doğrulayabilirsiniz. Görüntü adlı bir kapsayıcı kopyalanır **sistem** depolama hesabınızdaki. 

## <a name="create-a-vm-from-the-image"></a>Bir VM görüntüsünü oluşturma

Şimdi, yönetilmeyen görüntüden bir veya daha fazla sanal makine oluşturabilirsiniz.

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
Aşağıdaki PowerShell sanal makine yapılandırmalarının tamamlar ve yönetilmeyen görüntü, yeni yükleme için kaynak olarak kullanır.

</br>

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

### <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portal](https://portal.azure.com) altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenizi Azure PowerShell ile yönetmek için bkz: [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


