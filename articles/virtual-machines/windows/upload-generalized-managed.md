---
title: Şirket genelleştirilmiş VHD'den yönetilen bir Azure VM oluşturma | Microsoft Docs
description: Azure'da genelleştirilmiş VHD yükleme ve yeni sanal makineler Resource Manager dağıtım modelinde oluşturmak için bunu kullanın.
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
ms.date: 09/25/2018
ms.author: cynthn
ms.openlocfilehash: ee2fe91d915faf7e09dee004891edfc6bef38d6f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64685411"
---
# <a name="upload-a-generalized-vhd-and-use-it-to-create-new-vms-in-azure"></a>Genelleştirilmiş VHD yükleme ve Azure'da yeni VM'ler oluşturmak için bunu kullanın

Bu makalede, Azure'da bir genelleştirilmiş VM'nin VHD yükleme, bir VHD'den görüntü oluştur ve bu görüntüden yeni bir VM oluşturmak için PowerShell kullanarak adımları gösterilmektedir. Başka bir bulut veya şirket içi sanallaştırma aracından dışarı aktardığınız bir VHD yükleyebilirsiniz. Kullanarak [yönetilen diskler](managed-disks-overview.md) yeni VM VM yönetimini basitleştirir ve sanal Makineyi bir kullanılabilirlik kümesine yerleştirildiğinde daha iyi kullanılabilirlik sunar. 

Örnek betik için bkz: [örnek Azure'a VHD yükleme ve yeni bir VM oluşturmak için komut dosyası](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md).

## <a name="before-you-begin"></a>Başlamadan önce

- Azure'a herhangi bir VHD'yi karşıya yüklemeden önce uygulamanız gereken [Windows VHD veya VHDX yüklemek için hazırlama](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Gözden geçirme [yönetilen Diskler'e geçiş planı](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) için geçişiniz başlamadan önce [yönetilen diskler](managed-disks-overview.md).

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]


## <a name="generalize-the-source-vm-by-using-sysprep"></a>Sysprep kullanarak kaynak VM generalize

Sysprep diğer öğelerin yanı sıra tüm kişisel hesap bilgilerinizi kaldırır ve makineyi bir görüntü olarak kullanılacak şekilde hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [Sysprep genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için [sunucu rolleri için Sysprep Destek](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> VHD'nizi Azure'a ilk kez yüklemeden önce Sysprep çalıştırmayı planlıyorsanız, olduğundan emin olun [sanal makinenizin hazır](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
> 
> 

1. Windows sanal makinede oturum açın.
2. Yönetici olarak Komut İstemi penceresini açın. % Windir%\system32\sysprep için dizini değiştirin ve ardından çalıştırın `sysprep.exe`.
3. İçinde **sistem hazırlığı aracı** iletişim kutusunda **girin sistem kullanıma hazır deneyimi (OOBE)** , emin olun **Generalize** onay kutusunu etkin.
4. İçin **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam**’ı seçin.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)
6. Sysprep sona erdiğinde, sanal makineyi kapatır. VM yeniden başlatmayın.


## <a name="get-a-storage-account"></a>Bir depolama hesabı edinin

Karşıya yüklenen VM görüntüsü depolamak için Azure depolama hesabı gerekir. Mevcut bir depolama hesabı kullanabilir veya yeni bir tane oluşturun. 

VHD bir sanal makine için yönetilen disk oluşturmak için kullanacaksanız, depolama hesabı konumu burada VM oluşturmayı aynı konumda olmalıdır.

Kullanılabilir depolama hesapları gösterecek şekilde girin:

```azurepowershell
Get-AzStorageAccount | Format-Table
```

## <a name="upload-the-vhd-to-your-storage-account"></a>VHD, depolama hesabınıza yükleme

Kullanım [Ekle AzVhd](https://docs.microsoft.com/powershell/module/az.compute/add-azvhd) VHD'nin depolama hesabınızdaki bir kapsayıcıya yüklemek için cmdlet'i. Bu örnek dosyayı yükler *myVHD.vhd* gelen *C:\Users\Public\Documents\Virtual sabit diskleri\\*  adlı bir depolama hesabına *mystorageaccount* içinde *myResourceGroup* kaynak grubu. Dosya adlı bir kapsayıcının içine yerleştirilecek *mycontainer* ve yeni dosya adı *myUploadedVHD.vhd*.

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

### <a name="other-options-for-uploading-a-vhd"></a>Bir VHD'yi karşıya yüklemek için diğer seçenekler
 
Depolama hesabınızı aşağıdaki birini kullanarak bir VHD da karşıya yükleyebilirsiniz:

- [AzCopy](https://aka.ms/downloadazcopy)
- [Azure depolama kopyalama Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Depolama Gezgini Blobları karşıya yükleme](https://azurestorageexplorer.codeplex.com/)
- [Depolama içeri/dışarı aktarma hizmeti REST API Başvurusu](https://msdn.microsoft.com/library/dn529096.aspx)
-   İçeri/dışarı aktarma hizmeti yedi günden uzun süre karşıya tahmin kullanmanızı öneririz. Kullanabileceğiniz [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) veri boyutu ve aktarım birimi saati tahmin etmek için. 
    İçeri/dışarı aktarma, bir standart depolama hesabına kopyalamak için kullanılabilir. AzCopy gibi bir araç kullanarak standart depolamadan premium depolama hesabına kopyalamak gerekir.

> [!IMPORTANT]
> VHD'nizi yüklemek için AzCopy kullanıyorsanız ayarladığınızdan emin olun [ **/BlobType:page** ](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#blobtypeblock--page--append) karşıya yükleme betiğinizi çalıştırmadan önce. Hedef blob ise ve bu seçenek belirtilmezse, varsayılan olarak, bir blok blobu AzCopy oluşturur.
> 
> 



## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Karşıya yüklenen VHD'den yönetilen görüntüsünü oluşturma 

Genelleştirilmiş işletim sistemi VHD'den yönetilen bir görüntü oluşturun. Aşağıdaki değerleri kendi bilgilerinizle değiştirin.


İlk olarak, bazı parametrelerini ayarla:

```powershell
$location = "East US" 
$imageName = "myImage"
```

İşletim sistemi VHD'niz genelleştirilmiş kullanarak görüntüsünü oluşturun.

```powershell
$imageConfig = New-AzImageConfig `
   -Location $location
$imageConfig = Set-AzImageOsDisk `
   -Image $imageConfig `
   -OsType Windows `
   -OsState Generalized `
   -BlobUri $urlOfUploadedImageVhd `
   -DiskSizeGB 20
New-AzImage `
   -ImageName $imageName `
   -ResourceGroupName $rgName `
   -Image $imageConfig
```


## <a name="create-the-vm"></a>Sanal makine oluşturma

Artık bir görüntünüz olduğuna göre, görüntüden bir veya daha fazla yeni VM oluşturabilirsiniz. Bu örnek adlı bir VM oluşturur *myVM* gelen *Myımage*, *myResourceGroup*.


```powershell
New-AzVm `
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

Yeni sanal makinenize oturum açın. Daha fazla bilgi için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

