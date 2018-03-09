---
title: "Yönetilen bir Azure VM genelleştirilmiş şirket içi VHD'den oluştur | Microsoft Docs"
description: "Genelleştirilmiş bir VHD Azure'a yükleyin ve yeni VM'ler, Resource Manager dağıtım modelinde oluşturmak için kullanın."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2e78ecf6bd281bd5d30f59413789eb1e6fc7b5bc
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Genelleştirilmiş bir VHD yüklemek ve yeni sanal makineleri oluşturmak için kullanın

Bu konuda, bir VHD genelleştirilmiş bir VM'nin Azure'a yükleyin, VHD'den görüntü oluştur ve bu görüntüden yeni bir VM oluşturmak için PowerShell kullanılarak üzerinden açıklanmaktadır. Bir şirket içi sanallaştırma aracı ya da başka bir bulut dışa aktarılan bir VHD'yi yükleyebilirsiniz. Kullanarak [yönetilen diskleri](managed-disks-overview.md) yeni VM VM yönetimini basitleştirir ve VM bir kullanılabilirlik kümesine yerleştirildiğinde daha iyi kullanılabilirlik sağlar. 

Bir örnek komut dosyası kullanmak istiyorsanız, bkz: [örnek bir VHD Azure'a yükleyin ve yeni bir VM oluşturmak için komut dosyası](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Başlamadan önce

- Herhangi bir VHD'yi Azure'a karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX Azure'a karşıya yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Gözden geçirme [Plan yönetilen disklerin geçiş için](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) geçişinizi başlatmadan önce [yönetilen diskleri](managed-disks-overview.md).
- AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. Yüklemek için aşağıdaki komutu çalıştırın.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).


## <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM generalize

Sysprep tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Sysprep, VHD Azure'a ilk kez karşıya yüklemeden önce çalıştırıyorsanız olduğundan emin olun [VM'nizi hazırlanmış](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

1. Windows sanal makinede oturum açın.
2. Bir yönetici olarak komut istemi penceresi açın. Dizinine değiştirin **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda **girin sistem Out-of-Box deneyimi (OOBE)**, emin olun **Generalize** onay kutusu seçilidir.
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam**’a tıklayın.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. VM yeniden başlatmayın.



## <a name="log-in-to-azure"></a>Azure'da oturum açma
PowerShell sürüm 1.4 zaten yoksa veya üstü yüklü okuma [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

1. Azure PowerShell'i açın ve Azure hesabınızda oturum açın. Azure hesabı kimlik bilgilerinizi girmeniz için bir açılır pencere açılır.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Abonelik kimlikleri kullanılabilir aboneliklerinizi alın.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Abonelik kimliğini kullanarak doğru aboneliğin ayarlayın Değiştir  *<subscriptionID>*  doğru abonelik kimliği.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Depolama hesabı edinin
Karşıya yüklenen VM görüntüsü depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun. 

VHD için bir VM yönetilen bir disk oluşturmak için kullanıyorsanız, depolama hesabı konumu aynı olmalıdır nerede oluşturmakta VM konumu.

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

    Adlı bir kaynak grubu oluşturmak için **myResourceGroup** içinde **Doğu ABD** bölge, türü:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Adlı depolama hesabı oluşturma **mystorageaccount** kullanarak bu kaynak grubundaki [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    -SkuName için geçerli değerler şunlardır:
   
   * **Standard_LRS** -yerel olarak yedekli depolama. 
   * **Standard_ZRS** -bölge olarak yedekli depolama.
   * **Standard_GRS** -coğrafi olarak yedekli depolama. 
   * **Standard_RAGRS** -coğrafi olarak yedekli depolamaya okuma erişimi. 
   * **Premium_LRS** -Premium yerel olarak yedekli depolama. 

## <a name="upload-the-vhd-to-your-storage-account"></a>Depolama hesabınız VHD karşıya yükle

Kullanım [Ekle AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) depolama hesabınızdaki bir kapsayıcıya VHD yüklemek için cmdlet'i. Bu örnek dosya karşıya yükleme *myVHD.vhd* gelen *"C:\Users\Public\Documents\Virtual sabit diskleri\"*  bir depolama hesabına adlı *mystorageaccount* içinde *myResourceGroup* kaynak grubu. Dosya adında kapsayıcısının içine yerleştirilecek *mycontainer* ve yeni dosya adı *myUploadedVHD.vhd*.

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

Ağ bağlantısı ve VHD dosyasının boyutuna bağlı olarak, bu komutun tamamlanması biraz zaman alabilir

Kaydet **hedef URI** yönetilen bir disk veya karşıya yüklenen VHD kullanarak yeni bir VM oluşturmak için kullanacaksanız daha sonra kullanmak üzere yolu.

### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD karşıya yükleme için diğer seçenekleri
 
 
Bir VHD depolama hesabınıza aşağıdakilerden birini kullanarak da yükleyebilirsiniz:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Azure depolama kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Depolama Gezgini karşıya BLOB'ları](https://azurestorageexplorer.codeplex.com/)
- [Depolama içeri/dışarı aktarma hizmeti REST API Başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)
-   İçeri/dışarı aktarma hizmeti 7 günden daha uzun süre karşıya tahmini varsa kullanmanızı öneririz. Kullanabileceğiniz [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) veri boyutu ve aktarım birimi saati tahmin etmek için. 
    İçeri/dışarı aktarma, bir standart depolama hesabına kopyalamak için kullanılabilir. Premium depolama hesabı AzCopy gibi bir araç kullanarak standart depolama biriminden kopyalamanız gerekir.

> [!IMPORTANT]
> VHD Azure'a yüklenmesini AzCopy kullanıyorsanız, belirlediğinizden emin olun [/BlobType:page](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) çalıştırmadan önce komut dosyasını karşıya yükleyin. Hedef blob ise ve bu seçenek, varsayılan olarak, belirtilmemiş bir blok blobu AzCopy oluşturur.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Karşıya yüklenen VHD'den yönetilen bir görüntü oluştur 

Genelleştirilmiş OS VHD kullanılarak yönetilen bir görüntü oluşturun. Değerleri kendi bilgileriyle değiştirin.


1.  İlk olarak, ortak parametreleri ayarlayın:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  Genelleştirilmiş OS VHD kullanarak görüntü oluşturma.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Bu örnek adlı bir alt ağı oluşturur *mySubnet* adres öneki ile *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağı oluşturun. Bu örnek adlı bir sanal ağ oluşturur *myVnet* adres öneki ile *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Ortak IP adresi ve ağ arabirimi oluşturma

Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Bir ortak IP adresi oluşturun. Bu örnek adlı ortak IP adresi oluşturur *myPip*. 
   
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

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma

RDP kullanarak VM'nizi oturum açmak bağlantı noktası 3389 üzerinde RDP erişimine izin veren bir ağ güvenlik kuralı (NSG) olması gerekir. 

Bu örnekte adlı bir NSG oluşturulur *myNsg* adlı kuralı içeren *myRdpRule* 3389 numaralı bağlantı noktası RDP trafiğine izin veren. Nsg'ler hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bir VM için bağlantı noktaları açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Sanal ağ için bir değişken oluşturun

Tamamlanan sanal ağ için bir değişken oluşturun. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>VM için kimlik bilgilerini alma

Aşağıdaki cmdlet'i yeni bir kullanıcı adı ve parola VM uzaktan erişmek için yerel yönetici hesabı olarak kullanılacak gireceğiniz bir pencere açılır. 

```powershell
$cred = Get-Credential
```

## <a name="add-the-vm-name-and-size-to-the-vm-configuration"></a>Sanal makine adını ve boyutunu VM yapılandırmasına ekleyin.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>VM görüntüsü yeni VM için kaynak görüntü olarak ayarlayın

Yönetilen VM görüntüsü Kimliğini kullanarak kaynak görüntüsü ayarlayın.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>İşletim sistemi yapılandırması ayarlayın ve NIC ekleyin

Depolama türü (PremiumLRS veya StandardLRS) ve işletim sistemi disk boyutu girin. Bu örnekte hesap türünü ayarlar *PremiumLRS*, disk boyutu *128 GB* ve disk önbelleğe alma *ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

İçinde depolanan yapılandırmayı kullanarak yeni VM oluşturma **$vm** değişkeni.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Tamamlandığında, yeni oluşturulan VM görmelisiniz [Azure portal](https://portal.azure.com) altında **Gözat** > **sanal makineleri**, veya aşağıdaki PowerShell komutlarını kullanarak:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar

Yeni sanal makinede oturum açmak için Gözat VM [portal](https://portal.azure.com), tıklatın **Bağlan**ve Uzak Masaüstü RDP dosyasını açın. Yeni sanal makinede oturum açmak için özgün sanal makine hesabı kimlik bilgilerini kullanın. Daha fazla bilgi için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

