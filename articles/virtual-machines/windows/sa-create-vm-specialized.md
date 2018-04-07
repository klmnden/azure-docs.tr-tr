---
title: Azure özel bir diskten VM oluşturma | Microsoft Docs
description: Resource Manager dağıtım modelinde bir özelleştirilmiş yönetilmeyen disk ekleyerek yeni bir VM oluşturun.
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
ms.date: 05/23/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: ffa36967eb987f5e1b66f007ae60a63e640a609a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Bir depolama hesabında özelleştirilmiş bir VHD'den bir VM oluşturma

Powershell kullanarak işletim sistemi diski olarak özel bir yönetilmeyen disk ekleyerek yeni bir VM oluşturun. Özelleştirilmiş bir disk, kullanıcı hesapları, uygulamaları ve diğer Durum verilerini orijinal VM tutar mevcut bir VM'yi VHD'den kopyasıdır. 

İki seçeneğiniz vardır:
* [Bir VHD’yi karşıya yükleme](sa-create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Mevcut bir Azure VM'i VHD'yi kopyalayın](sa-create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a>Başlamadan önce
PowerShell'i kullanırsanız, AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. Yüklemek için aşağıdaki komutu çalıştırın.

```powershell
Install-Module AzureRM.Compute 
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Seçenek 1: özel bir VHD yüklemek

Hyper-V ya da VM başka bir buluttan dışarı gibi bir şirket içi sanallaştırma aracı ile oluşturulan özel bir VM VHD'den karşıya yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
Bir şirket içi VM veya başka bir buluttan dışa aktarılan bir VHD kullanılarak oluşturulmuş özel bir VHD karşıya yükleyebilirsiniz. Özel bir VHD kullanıcı hesapları, uygulamaları ve diğer Durum verilerini orijinal VM tutar. VHD olarak kullanmak istiyorsanız-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandıktan emin olun. 
  
  * [Windows Azure'a yüklemeniz VHD hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Sağlamadığı** Sysprep kullanarak VM genelleştirin.
  * Konuk sanallaştırma araçları ve VM'de (yani VMware araçları) yüklü aracıları kaldırın.
  * VM kendi IP adresini ve DNS ayarlarını DHCP yoluyla isteyecek şekilde yapılandırıldığından emin olun. Bu başladığında sunucu VNet içindeki bir IP adresi alacağını sağlar. 


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
   
### <a name="upload-the-vhd-to-your-storage-account"></a>Depolama hesabınız VHD karşıya yükle
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


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a>Seçenek 2: Kopya var olan bir Azure VM VHD'den

Yeni, yinelenen bir VM oluşturulurken kullanılacak başka bir depolama hesabı için bir VHD kopyalayabilirsiniz.

### <a name="before-you-begin"></a>Başlamadan önce
Olduğundan emin olun:

* Hakkında bilgi sahip **kaynak ve hedef depolama hesapları**. Kaynak VM için depolama hesabı ve kapsayıcı adlara sahip olması gerekir. Genellikle, kapsayıcı adı olacaktır **VHD'ler**. Ayrıca bir hedef depolama hesabına sahip olmanız gerekir. Zaten yoksa, her iki portal kullanarak bir tane oluşturabilirsiniz (**tüm hizmetleri** > depolama hesapları > Ekle) veya kullanarak [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet'i. 
* İndirdiğinizi ve yüklediğinizi [AzCopy aracı](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-the-vm"></a>VM serbest bırakma
Kopyalanacak VHD boşaltır VM serbest bırakma. 

* **Portal**: tıklatın **sanal makineleri** > **myVM** > Durdur
* **PowerShell**: kullanım [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) durdurmak için (deallocate) adlı VM **myVM** kaynak grubunda **myResourceGroup**.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

**Durum** VM Azure portal değişiklikleri **durduruldu** için **durduruldu (serbest bırakıldı)**.

### <a name="get-the-storage-account-urls"></a>Depolama hesabı URL'lerini alma
Kaynak ve hedef depolama hesapları URL'lerini gerekir. URL'leri görünüm ister: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Depolama hesabı ve kapsayıcı adını biliyorsanız, yalnızca URL'nizi oluşturmak için köşeli ayraçlar arasında bilgi değiştirebilirsiniz. 

URL almak için Azure portalında veya Azure Powershell kullanarak yapabilirsiniz:

* **Portal**: tıklatın **>** için **tüm hizmetleri** > **depolama hesapları** > *depolama Hesap* > **BLOB'lar** ve kaynak VHD dosyası büyük olasılıkla kullanımda **VHD'ler** kapsayıcı. Tıklatın **özellikleri** kapsayıcı ve kopyalama etiketli metin **URL**. Hem kaynak hem de hedef kapsayıcılarını URL'lerini gerekir. 
* **PowerShell**: kullanım [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) adlı VM için bilgi almak için **myVM** kaynak grubunda **myResourceGroup**. Sonuçlarda konum **depolama profili** bölümü **Vhd Uri'si**. Kapsayıcı URL'si URI ilk parçası olan ve son bölümü VM için işletim sistemi VHD adıdır.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a>Depolama erişim anahtarı alma
Erişim tuşları kaynak ve hedef için depolama hesapları bulun. Erişim anahtarları hakkında daha fazla bilgi için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

* **Portal**: tıklatın **tüm hizmetleri** > **depolama hesapları** > *depolama hesabı*  >   **Erişim tuşları**. Olarak etiketli anahtarı kopyalayın **key1**.
* **PowerShell**: kullanım [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) depolama hesabı için depolama anahtarını almak için **mystorageaccount** kaynak grubunda **myResourceGroup**. Etiketli anahtarı kopyalayın **key1**.

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a>VHD'yi kopyalayın
AzCopy kullanarak depolama hesapları arasında dosya kopyalayabilirsiniz. Belirtilen kapsayıcı mevcut değilse için hedef kapsayıcı, sizin için oluşturulur. 

AzCopy kullanmak için yerel makinenizde bir komut istemi açın ve AzCopy yüklü olduğu klasöre gidin. Benzer şekilde *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Bir kapsayıcıdaki tüm dosyaların kopyalamak için kullandığınız **/S** geçin. Bu aynı kapsayıcıda olmaları durumunda işletim sistemi VHD ve tüm veri diski kopyalamak için kullanılabilir. Bu örnekte, tüm dosyaları kapsayıcısında kopyalamak gösterilmiştir **mysourcecontainer** depolama hesabındaki **mysourcestorageaccount** kapsayıcısı **mydestinationcontainer**içinde **mydestinationstorageaccount** depolama hesabı. Depolama hesapları ve kapsayıcılar adları kendi ile değiştirin. Değiştir `<sourceStorageAccountKey1>` ve `<destinationStorageAccountKey1>` kendi anahtarlara sahip.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Yalnızca belirli bir VHD'yi bir kapsayıcıda sahip birden çok dosya kopyalamak isterseniz, ayrıca /Pattern anahtar kullanılarak dosya adını belirtebilirsiniz. Bu örnekte, yalnızca adlı dosyayı **myFileName.vhd** kopyalanacak.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Tamamlandığında, aşağıdakine benzer bir ileti alırsınız:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Sorun giderme
* Emin olun "Server" istek kimliğini doğrulama başarısız oldu, hata görürseniz, AZCopy, kullandığınızda Authorization Üstbilgisi değeri doğru imza dahil olmak üzere oluşturulur. Anahtar 2 ya da ikincil depolama anahtarı kullanıyorsanız, birincil ya da 1 depolama anahtarı kullanmayı deneyin.

## <a name="create-the-new-vm"></a>Yeni VM oluşturma 

Yeni VM tarafından kullanılmak üzere ağ ve diğer VM kaynakları oluşturmanız gerekir.

### <a name="create-the-subnet-and-vnet"></a>VNet ve alt ağ oluşturma

VNet ve alt ağı oluşturmak [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Bu örnek adlı bir alt ağı oluşturur **mySubNet**, kaynak grubundaki **myResourceGroup**ve alt ağ adresi öneki ayarlar **10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağ oluşturun. Bu örnek olarak sanal ağ adını ayarlar **myVnetName**, konuma **Batı ABD**ve sanal ağ adres öneki **10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
RDP kullanarak VM'nizi oturum açmak bağlantı noktası 3389 üzerinde RDP erişimine izin veren bir güvenlik kuralı olması gerekir. Mevcut bir VHD yeni VM için oluşturulduğundan özelleştirilmiş VM VM, oluşturulduktan sonra kaynak sanal makineden RDP kullanarak oturum açmak için izne sahip olan mevcut bir hesap kullanabilirsiniz.
Bu ağ arabirimi ile ilişkilendirilecek oluşturma önce tamamlanması gerekir.  
Bu örnek NSG adını ayarlar **myNsg** ve RDP kural adı **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Uç noktaları ve NSG kuralları hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bir VM için bağlantı noktaları açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Bir ortak IP adresi ve NIC oluşturun
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Genel IP oluşturun. Bu örnekte, ortak IP adresi adı ayarlamak **myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC oluşturun Bu örnekte, NIC adı ayarlamak **myNicName**. Bu adım Ayrıca bu NIC ile daha önce oluşturduğunuz ağ güvenlik grubu ilişkilendirir
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
    ```

### <a name="set-the-vm-name-and-size"></a>Sanal makine adını ve boyutunu ayarlama

Bu örnek VM adını "myVM" ve "Standard_A2" VM boyutuna ayarlar.
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekleme
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a>İşletim sistemi diski yapılandırın

1. URI karşıya veya kopyalanan VHD için ayarlayın. Bu örnekte, VHD dosyası adlı **myOsDisk.vhd** adlı bir depolama hesabında tutulur **myStorageAccount** adlı bir kapsayıcıda **myContainer**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. İşletim sistemi diski ekleyin. İşletim sistemi diski oluşturulduğunda, bu örnekte, "osDisk" appened işletim sistemi disk adı oluşturmak için VM adı için bir terimdir. Bu örnek ayrıca, bu Windows tabanlı VHD VM işletim sistemi diski olarak eklenmesi belirtir.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

İsteğe bağlı: VM'ye bağlı olması gereken veri diskler varsa, veri diskleri veri VHD'ler URL'lerini ve uygun mantıksal birim numarası (Lun) kullanarak ekleyin.

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Bir depolama hesabı kullanırken, veri ve işletim sistemi disk URL'leri aşağıdaki gibi görünmelidir: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Bu portalda hedef depolama kapsayıcısı için gözatma, işletim sistemi veya veri kopyalanan VHD tıklatarak ve URL içeriğini kopyalama bulabilirsiniz.


### <a name="complete-the-vm"></a>VM tamamlayın 

Yeni oluşturduğumuz yapılandırmaları kullanarak VM oluşturma.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Bu komutun başarılı olduysa, benzer bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>VM oluşturulduğunu doğrulayın
Yeni oluşturulan VM ya da görmeniz gerekir, [Azure portal](https://portal.azure.com)altında **tüm hizmetleri** > **sanal makineleri**, aşağıdaki PowerShell kullanarak veya komutlar:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinede oturum açmak için Gözat VM [portal](https://portal.azure.com), tıklatın **Bağlan**ve Uzak Masaüstü RDP dosyasını açın. Yeni sanal makinede oturum açmak için özgün sanal makine hesabı kimlik bilgilerini kullanın. Daha fazla bilgi için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md).

