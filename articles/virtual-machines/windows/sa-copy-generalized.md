---
title: Azure'da bir genelleştirilmiş VM'nin yönetilmeyen görüntü oluşturma | Microsoft Docs
description: Azure'da bir sanal makine birden çok kopyasını oluşturmak için kullanılacak genelleştirilmiş bir Windows VM unmanged görüntüsü oluşturun.
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
ms.openlocfilehash: e1ed419892412c1fb9334fed74b82c53154723ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60252412"
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a>Bir Azure VM'den yönetilmeyen bir VM görüntüsü oluşturma

Bu makalede, depolama hesaplarını kullanarak kapsar. Yönetilen diskler ve yönetilen görüntüleri yerine bir depolama hesabı kullanmanızı öneririz. Daha fazla bilgi için [azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü yakalama](capture-image-resource.md).

Bu makalede bir depolama hesabı kullanarak genelleştirilmiş Azure VM'yi bir görüntüsünü oluşturmak için Azure PowerShell kullanmayı gösterir. Ardından, başka bir VM oluşturmak için görüntüyü kullanabilirsiniz. Görüntü, işletim sistemi diski ve sanal makineye bağlı veri diskleri içerir. Yeni bir VM oluşturduğunuzda, bu kaynakları ayarlamanız gerekir, böylece görüntü sanal ağ kaynakları dahil değildir. 

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="generalize-the-vm"></a>VM'yi Genelleştirme 
Bu bölümde, Windows sanal makinenizi bir görüntü olarak kullanılmaya generalize gösterilir. VM'yi Genelleştirme, tüm kişisel hesap bilgilerinizi, başka şeylerin yanında kaldırır ve makine bir görüntü olarak kullanılacak hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep işlemini kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> VHD'nizi Azure'a ilk kez yüklüyorsanız olduğundan emin olun [sanal makinenizin hazır](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

Ayrıca kullanarak bir Linux VM genelleştirebilirsiniz `sudo waagent -deprovision+user` ve ardından sanal Makineyi yakalamaya yönelik PowerShell kullanın. Bir sanal Makineyi yakalamaya yönelik CLI'yı kullanma hakkında daha fazla bilgi için bkz: [Genelleştir ve Azure CLI kullanarak bir Linux sanal makinesini yakalama nasıl](../linux/capture-image.md).


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

## <a name="log-in-to-azure-powershell"></a>Azure PowerShell'de oturum açma
1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın.
   
    ```powershell
    Connect-AzAccount
    ```
   
    Azure hesabı kimlik bilgilerinizi girmeniz için bir pencere açılır.
2. ' % S'abonelik kimliği için mevcut aboneliklerinizi alın.
   
    ```powershell
    Get-AzSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliğin ayarlayın
   
    ```powershell
    Select-AzSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>VM'yi serbest bırakın ve genelleştirilmiş durumuna ayarlayın

> [!IMPORTANT] 
> Ekleme, düzenleyemez veya genelleştirildi olarak işaretlendikten sonra etiketleri bir sanal makineden kaldırın. VM için bir etiket eklemek istiyorsanız, genelleştirilmiş olarak işaretleme önce etiketleri eklediğinizden emin olun.
> 

1. Sanal makine kaynaklarının serbest bırakın.
   
    ```powershell
    Stop-AzVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    *Durumu* Azure VM için portalı değişiklikleri **durduruldu** için **durduruldu (serbest bırakıldı)**.
2. Sanal makinenin durumunu **Genelleştirmiş**. 
   
    ```powershell
    Set-AzVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Sanal makine durumunu denetleyin. **OSState ve genelleştirilmiş** VM olması gerekir için bölüm **DisplayStatus** kümesine **VM genelleştirilmiş**.  
   
    ```powershell
    $vm = Get-AzVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Görüntü oluşturma

Bu komutu kullanarak hedef depolama kapsayıcısında bir yönetilmeyen sanal makine görüntüsü oluşturun. Orijinal sanal makine ile aynı depolama hesabındaki bir görüntü oluşturulur. `-Path` Parametresi, kaynak VM için JSON şablonunu bir kopyasını yerel bilgisayarınıza kaydeder. `-DestinationContainerName` Parametredir görüntülerinizi tutmak istediğiniz kapsayıcısının adı. Kapsayıcı mevcut değilse sizin için oluşturulur.
   
```powershell
Save-AzVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
JSON dosyası şablonu, görüntünün URL'sini alabilirsiniz. Git **kaynakları** > **Datadisks** > **osDisk** > **görüntü**  >  **URI** görüntünüzü bölümü için tam yolu. Görüntünün URL'si şuna benzer: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
URI Portalı'nda da doğrulayabilirsiniz. Adlı bir kapsayıcı görüntüsü kopyalanır **sistem** depolama hesabınızda. 

## <a name="create-a-vm-from-the-image"></a>Görüntüden bir VM oluşturun

Şimdi, yönetilmeyen görüntüden bir veya daha fazla sanal makine oluşturabilirsiniz.

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
    $location = "West US"
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
Aşağıdaki PowerShell, sanal makine yapılandırmaları tamamlar ve yönetilmeyen görüntü, yeni yükleme için kaynak olarak kullanır.

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

### <a name="verify-that-the-vm-was-created"></a>Sanal Makinenin oluşturulduğunu doğrulayın.
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portalında](https://portal.azure.com) altında **Gözat** > **sanal makineler**, veya aşağıdaki PowerShell kullanarak komutlar:

```powershell
    $vmList = Get-AzVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell ile yeni sanal makinenize yönetmek için bkz: [Azure Resource Manager ve PowerShell kullanarak sanal makineleri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


