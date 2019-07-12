---
title: Azure'da özel bir diskten VM oluşturma | Microsoft Docs
description: Resource Manager dağıtım modelinde özel bir yönetilmeyen disk ekleyerek yeni bir VM oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
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
ms.openlocfilehash: 8833ddf487c36446b5e5b4ce1d6cfc6363d3ceeb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710384"
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Bir depolama hesabında özelleştirilmiş bir VHD'den VM oluşturma

Özel bir yönetilmeyen disk Powershell kullanarak işletim sistemi diski ekleyerek yeni bir VM oluşturun. Özelleştirilmiş disk VHD kullanıcı hesaplarını, uygulamaları ve diğer Durum verilerini orijinal VM'yi korur mevcut bir VM'den bir kopyasıdır. 

İki seçeneğiniz vardır:
* [Bir VHD’yi karşıya yükleme](sa-create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Mevcut bir Azure VM'yi VHD'yi kopyalayın](sa-create-vm-specialized.md#option-2-copy-the-vhd-from-an-existing-azure-vm)

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]


## <a name="option-1-upload-a-specialized-vhd"></a>1\. seçenek: Özelleştirilmiş bir VHD'yi karşıya yükleme

Hyper-V veya sanal makine başka bir buluttan dışarı gibi bir şirket içi sanallaştırma aracı ile oluşturulan özel bir VM VHD yükleyebilirsiniz.

### <a name="prepare-the-vm"></a>VM’yi hazırlama
Bir şirket içi VM veya başka bir buluttan dışarı aktarılan bir VHD kullanılarak oluşturulmuş özelleştirilmiş bir VHD'yi karşıya yükleyebilirsiniz. Özelleştirilmiş bir VHD'yi kullanıcı hesaplarını, uygulamaları ve orijinal VM'yi diğer Durum verilerini tutar. VHD olarak kullanmak istiyorsanız,-olan yeni bir VM oluşturmak için aşağıdaki adımları tamamlandığından emin olun. 
  
  * [Windows Azure'a karşıya yüklenecek VHD'yi hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Sağlamadığı** Sysprep kullanarak VM'yi Genelleştirme.
  * Herhangi bir konuk sanallaştırma araçları ve (yani, VMware araçları) VM'de yüklü aracıları kaldırın.
  * VM, IP adresi ve DNS ayarlarını DHCP yoluyla çekmek için yapılandırılan emin olun. Bu, başladığında sunucunun sanal ağ içindeki bir IP adresi alacağını sağlar. 


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
   
### <a name="upload-the-vhd-to-your-storage-account"></a>VHD, depolama hesabınıza yükleme
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


## <a name="option-2-copy-the-vhd-from-an-existing-azure-vm"></a>2\. seçenek: Mevcut bir Azure VM'den VHD'yi kopyalayın

Yeni ve yinelenen bir VM oluşturulurken kullanılacak başka bir depolama hesabına VHD kopyalayabilirsiniz.

### <a name="before-you-begin"></a>Başlamadan önce
Emin olun:

* Hakkında bilgilere sahip **kaynak ve hedef depolama hesapları**. Kaynak VM için depolama hesabı ve kapsayıcı adları olması gerekir. Genellikle, kapsayıcı adı olacaktır **VHD'ler**. Bir hedef depolama hesabının olması gerekir. Zaten yoksa, her iki portal kullanarak bir tane oluşturabilirsiniz (**tüm hizmetleri** > depolama hesapları > Ekle) veya bu adı kullanıyor [yeni AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount) cmdlet'i. 
* İndirdiğinizi ve yüklediğinizi [AzCopy aracı](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-the-vm"></a>VM'yi serbest bırakın
Kopyalanacak VHD boşaltır VM'yi serbest bırakın. 

* **Portal**: Tıklayın **sanal makineler** > **myVM** > Durdur
* **PowerShell**: Kullanım [Stop-AzVM](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) durdurmak için (serbest bırakın) adlı bir sanal makine **myVM** kaynak grubundaki **myResourceGroup**.

```powershell
Stop-AzVM -ResourceGroupName myResourceGroup -Name myVM
```

**Durumu** Azure VM için portalı değişiklikleri **durduruldu** için **durduruldu (serbest bırakıldı)** .

### <a name="get-the-storage-account-urls"></a>Depolama hesabı URL'lerini alma
Kaynak ve hedef depolama hesapları URL'lerini ihtiyacınız vardır. Aşağıdaki gibi URL'leri görünür: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Depolama hesabı ve kapsayıcı adını biliyorsanız, yalnızca URL'nizi oluşturmak için köşeli ayraçlar arasındaki bilgileri değiştirebilirsiniz. 

URL almak için Azure portal veya Azure Powershell kullanabilirsiniz:

* **Portal**: Tıklayın **>** için **tüm hizmetleri** > **depolama hesapları** > *depolama hesabı*  >  **Blobları** ve kaynak VHD dosyanız büyük olasılıkla kullanımda **VHD'ler** kapsayıcı. Tıklayın **özellikleri** etiketli metni kopyalayın ve kapsayıcı **URL**. Kapsayıcılar kaynak ve hedef URL'leri gerekir. 
* **PowerShell**: Kullanım [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) adlı VM için bilgi almak için **myVM** kaynak grubundaki **myResourceGroup**. Sonuçlarda konum **depolama profili** bölümü **Vhd Uri'si**. URI'ın ilk bölümü URL'dir kapsayıcıya ve son bölümü VM için işletim sistemi VHD'si adıdır.

```powershell
Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-the-storage-access-keys"></a>Depolama erişim anahtarlarını alma
Erişim anahtarları, depolama hesapları kaynak ve hedef için bulun. Erişim anahtarları hakkında daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

* **Portal**: Tıklayın **tüm hizmetleri** > **depolama hesapları** > *depolama hesabı* > **erişim anahtarları**. Olarak etiketlenen tuşunu kopyalamak **key1**.
* **PowerShell**: Kullanım [Get-AzStorageAccountKey](https://docs.microsoft.com/powershell/module/az.storage/get-azstorageaccountkey) depolama hesabı için depolama anahtarı almak için **mystorageaccount** kaynak grubundaki **myResourceGroup**. Etiketlenen tuşunu kopyalamak **key1**.

```powershell
Get-AzStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-the-vhd"></a>VHD'yi kopyalayın
AzCopy kullanarak depolama hesapları arasında dosya kopyalayabilirsiniz. Belirtilen kapsayıcı mevcut değilse hedef kapsayıcı için bunu sizin için oluşturulur. 

AzCopy kullanmak için yerel makinenizde bir komut istemi açın ve AzCopy yüklü olduğu klasöre gidin. Benzer şekilde *C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

Bir kapsayıcı içindeki dosyaların tümünü kopyalamak için kullandığınız **/S** geçin. Bu işletim sistemi VHD'si ile tüm veri disklerinin aynı kapsayıcıda olmaları durumunda kopyalamak için kullanılabilir. Bu örnek kapsayıcıda tüm dosyaları kopyalamak nasıl gösterir **mysourcecontainer** depolama hesabındaki **mysourcestorageaccount** kapsayıcıya **mydestinationcontainer**içinde **mydestinationstorageaccount** depolama hesabı. Depolama hesabı ve kapsayıcı adları kendi değerlerinizle değiştirin. Değiştirin `<sourceStorageAccountKey1>` ve `<destinationStorageAccountKey1>` kendi anahtarlarınızı ile.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Yalnızca bir kapsayıcıdaki belirli bir VHD'yi sahip birden çok dosya kopyalamak isterseniz, /Pattern anahtarını kullanarak dosya adını belirtebilirsiniz. Bu örnekte, yalnızca adlı dosyayı **myFileName.vhd** kopyalanır.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Tamamlandığında, şuna benzer bir ileti alırsınız:

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
* AZCopy, emin olun "Server", isteğin kimliğini doğrulayamadı hata görürseniz kullandığınızda yetkilendirme üst bilgisi değeri doğru imza dahil olmak üzere oluşturulur. Anahtar 2 ya da ikincil depolama anahtarını kullanarak, birincil ya da 1. depolama anahtarı kullanmayı deneyin.

## <a name="create-the-new-vm"></a>Yeni VM oluşturma 

Kullanarak yeni sanal makine tarafından kullanılan ağ ve VM kaynakları oluşturmanız gerekir.

### <a name="create-the-subnet-and-vnet"></a>VNet ve alt ağ oluşturma

Sanal ağ oluşturup alt [sanal ağ](../../virtual-network/virtual-networks-overview.md).

1. Alt ağ oluşturun. Bu örnek adlı bir alt ağ oluşturur **mySubNet**, kaynak grubundaki **myResourceGroup**ve alt ağ adres ön eki ayarlar **10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Sanal ağ oluşturun. Bu örnek, sanal ağ adı olacak şekilde ayarlar **myVnetName**, konuma **Batı ABD**ve sanal ağa ait adres ön ekini **10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
   ### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Ağ güvenlik grubu ve bir RDP kuralı oluşturma
   Sanal makinenizde RDP kullanarak oturum açmak 3389 numaralı bağlantı noktasında RDP erişimine izin veren bir güvenlik kuralı olması gerekir. Mevcut bir VHD için yeni bir VM oluşturulduğundan, VM oluşturulduktan sonra özel VM kaynak sanal makineden RDP kullanarak oturum açmasına izin olan mevcut bir hesap kullanabilirsiniz.
   Bu ağ arabirimi ile ilişkilendirilecektir oluşturmadan önce tamamlanması gerekir.  
   NSG adı Bu örnekte ayarlar **myNsg** ve RDP kuralının adını **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Uç noktaları ve NSG kuralları hakkında daha fazla bilgi için bkz: [PowerShell kullanarak Azure'da bağlantı noktalarını VM'ye açma](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Bir genel IP adresi ve ağ Arabirimi oluşturma
Sanal makinenin sanal ağda iletişimini etkinleştirmeniz için, [genel IP adresi](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) ve ağ arabirimi gereklidir.

1. Genel IP oluşturun. Bu örnekte, genel IP adresi adı kümesine **myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. NIC oluşturma Bu örnekte, NIC adı kümesine **myNicName**. Bu adım Ayrıca bu NIC ile daha önce oluşturduğunuz ağ güvenlik grubu ilişkilendirir.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
    ```

### <a name="set-the-vm-name-and-size"></a>VM adını ve boyutunu ayarlayın

Bu örnekte, "myVM" VM adına ve "Standard_a2 =" için VM boyutunu ayarlar.
```powershell
$vmName = "myVM"
$vmConfig = New-AzVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>NIC ekleme
    
```powershell
$vm = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-the-os-disk"></a>İşletim sistemi diskini yapılandırma

1. URI karşıya veya kopyalanan VHD için ayarlayın. Bu örnekte, VHD dosyasının adlı **myOsDisk.vhd** adlı bir depolama hesabında tutulur **myStorageAccount** adlı bir kapsayıcı içinde **myContainer**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. İşletim sistemi diskini ekleyin. İşletim sistemi diski oluşturulduğunda, bu örnekte, "osDisk" terimi işletim sistemi disk adı oluşturmak için VM adına eklenir. Bu örnek, ayrıca bu Windows tabanlı bir VHD VM işletim sistemi diski olarak eklenmesi belirtir.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

İsteğe bağlı: VM'ye eklenmesine gerek veri diskleri varsa, veri VHD'leri URL'leri ve uygun mantıksal birim numarası (Lun) kullanarak veri diski ekleyin.

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Bir depolama hesabını kullanırken, veri ve işletim sistemi disk URL'leri aşağıdaki gibi görünmelidir: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Bunu portalı hedef depolama kapsayıcısına gözatma işletim sistemi veya veri kopyalanan VHD tıklayarak ve sonra URL'yi içeriğini kopyalamayı bulabilirsiniz.


### <a name="complete-the-vm"></a>VM tamamlayın 

Yeni oluşturduğumuz yapılandırmaları kullanarak VM oluşturun.

```powershell
#Create the new VM
New-AzVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Bu komut başarılı olduysa, şunun gibi bir çıktı görürsünüz:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Sanal Makinenin oluşturulduğunu doğrulayın.
Yeni oluşturulan VM ya da görmeniz gerekir, [Azure portalında](https://portal.azure.com)altında **tüm hizmetleri** > **sanal makineler**, veya aşağıdaki PowerShell kullanarak komutlar:

```powershell
$vmList = Get-AzVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Yeni sanal makinenize oturum açın. Daha fazla bilgi için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md).

