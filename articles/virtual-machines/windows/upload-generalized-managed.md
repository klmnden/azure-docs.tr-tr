---
title: Yönetilen bir Azure VM genelleştirilmiş şirket içi VHD'den oluştur | Microsoft Docs
description: Genelleştirilmiş bir VHD Azure'a yükleyin ve yeni VM'ler, Resource Manager dağıtım modelinde oluşturmak için kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: cynthn
ms.openlocfilehash: 9ebe1f67c7c662af6d9e1888580149834a007200
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657480"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Genelleştirilmiş bir VHD yüklemek ve yeni sanal makineleri oluşturmak için kullanın

Bu konuda, bir VHD genelleştirilmiş bir VM'nin Azure'a yükleyin, VHD'den görüntü oluştur ve bu görüntüden yeni bir VM oluşturmak için PowerShell kullanılarak üzerinden açıklanmaktadır. Bir şirket içi sanallaştırma aracı ya da başka bir bulut dışa aktarılan bir VHD'yi yükleyebilirsiniz. Kullanarak [yönetilen diskleri](managed-disks-overview.md) yeni VM VM yönetimini basitleştirir ve VM bir kullanılabilirlik kümesine yerleştirildiğinde daha iyi kullanılabilirlik sağlar. 

Bir örnek komut dosyası kullanmak istiyorsanız, bkz: [örnek bir VHD Azure'a yükleyin ve yeni bir VM oluşturmak için komut dosyası](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Başlamadan önce

- Herhangi bir VHD'yi Azure'a karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX Azure'a karşıya yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Gözden geçirme [Plan yönetilen disklerin geçiş için](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) geçişinizi başlatmadan önce [yönetilen diskleri](managed-disks-overview.md).
- Bu makalede AzureRM modülü 5.6 veya sonraki bir sürümü gerektiriyor. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM.Compute` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps).


## <a name="generalize-the-source-vm-using-sysprep"></a>Kaynak VM generalize Sysprep kullanma

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Sysprep, VHD Azure'a ilk kez karşıya yüklemeden önce çalıştırıyorsanız olduğundan emin olun [VM'nizi hazırlanmış](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Sysprep çalıştırılmadan önce. 
> 
> 

1. Windows sanal makinede oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. Dizinine değiştirin **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.
3. **Sistem Hazırlama Aracı** iletişim kutusunda  **Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam**’a tıklayın.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep tamamlandığında, sanal makineyi kapatır. VM yeniden başlatmayın.


## <a name="get-the-storage-account"></a>Depolama hesabı edinin

Karşıya yüklenen VM görüntüsü depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabını kullanabilir veya yeni bir tane oluşturun. 

VHD için bir VM yönetilen bir disk oluşturmak için kullanıyorsanız, depolama hesabı konumu aynı olmalıdır nerede oluşturmakta VM konumu.

Kullanılabilir depolama hesaplarını görüntülemek için aşağıdakileri yazın:

```azurepowershell
Get-AzureRmStorageAccount | Format-Table
```

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

### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD karşıya yükleme için diğer seçenekleri
 
Bir VHD depolama hesabınıza aşağıdakilerden birini kullanarak da yükleyebilirsiniz:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Azure depolama kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Depolama Gezgini karşıya BLOB'ları](https://azurestorageexplorer.codeplex.com/)
- [Depolama içeri/dışarı aktarma hizmeti REST API Başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)
-   İçeri/dışarı aktarma hizmeti 7 günden daha uzun süre karşıya tahmini varsa kullanmanızı öneririz. Kullanabileceğiniz [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) veri boyutu ve aktarım birimi saati tahmin etmek için. 
    İçeri/dışarı aktarma, bir standart depolama hesabına kopyalamak için kullanılabilir. Premium depolama hesabı AzCopy gibi bir araç kullanarak standart depolama biriminden kopyalamanız gerekir.

> [!IMPORTANT]
> VHD Azure'a yüklenmesini AzCopy kullanıyorsanız, belirlediğinizden emin olun [/BlobType:page](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) çalıştırmadan önce komut dosyasını karşıya yükleyin. Hedef blob ise ve bu seçenek, varsayılan olarak, belirtilmemiş bir blok blobu AzCopy oluşturur.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Karşıya yüklenen VHD'den yönetilen bir görüntü oluştur 

Genelleştirilmiş OS VHD kullanılarak yönetilen bir görüntü oluşturun. Değerleri kendi bilgileriyle değiştirin.


İlk olarak, bazı parametreler ayarlayın:

```powershell
$location = "East US" 
$imageName = "myImage"
```

Genelleştirilmiş OS VHD kullanarak görüntü oluşturma.

```powershell
$imageConfig = New-AzureRmImageConfig `
   -Location $location
$imageConfig = Set-AzureRmImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzureRmImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Sanal makine oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Bu örnek, adlandırılmış bir VM'nin oluşturur *myVM* gelen *myImage*, *myResourceGroup*.


```powershell
New-AzureRmVm `
    -ResourceGroupName $rgName `
    -Name "myVM" `
    -ImageName $imageName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNSG" `
    -PublicIpAddressName "myPIP" `
    -OpenPorts 3389
```


## <a name="next-steps"></a>Sonraki adımlar

Yeni sanal makine için oturum açın. Daha fazla bilgi için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

