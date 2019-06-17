---
title: Azure'da yönetilen bir görüntü oluşturma | Microsoft Docs
description: Azure'da yönetilen genelleştirilmiş bir sanal makine veya VHD görüntüsü oluşturun. Görüntüler, yönetilen diskler kullanan birden çok VM oluşturmak için kullanılabilir.
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
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: 3a6781387121a691c6599ffaeb5722ecc6e16132
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64704686"
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Azure'da bir genelleştirilmiş VM'nin yönetilen görüntüsünü oluşturma

Yönetilen bir görüntü kaynağı genelleştirilmiş sanal makineden bir yönetilen diskin veya depolama hesabındaki yönetilmeyen disk olarak depolanan (VM) oluşturulabilir. Görüntü daha sonra birden çok VM oluşturmak için kullanılabilir. Yönetilen görüntüleri faturalandırılır hakkında bilgi için bkz: [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/). 

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM'sini genelleştirme

Sysprep tüm kişisel hesap ve güvenlik bilgilerinizi kaldırır ve ardından bir görüntü olarak kullanılacak makinenin hazırlar. Sysprep hakkında daha fazla bilgi için bkz: [Sysprep genel bakış](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için [sunucu rolleri için Sysprep Destek](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep-support-for-server-roles).

> [!IMPORTANT]
> Bir VM'de Sysprep çalıştırdıktan sonra o sanal makine olarak kabul edilir *genelleştirilmiş* ve yeniden başlatılamıyor. VM’yi genelleştirme işlemi geri döndürülemez. Özgün VM çalışmıyor tutmak için ihtiyacınız varsa oluşturmalısınız bir [VM kopyasını](create-vm-specialized.md#option-3-copy-an-existing-azure-vm) ve kendi kopyalama generalize. 
>
> Sanal sabit disk (VHD) Azure'a ilk kez yüklemeden önce Sysprep çalıştırmayı planlıyorsanız, olduğundan emin olun [sanal makinenizin hazır](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  
> 
> 

Windows VM'nizi genelleştirmek için şu adımları izleyin:

1. Windows VM için oturum açın.
   
2. Yönetici olarak bir komut istemi penceresi açın. % Windir%\system32\sysprep için dizini değiştirin ve ardından çalıştırın `sysprep.exe`.
   
3. İçinde **sistem hazırlığı aracı** iletişim kutusunda **girin sistem kullanıma hazır deneyimi (OOBE)** seçip **Generalize** onay kutusu.
   
4. İçin **kapatma seçenekleri**seçin **kapatma**.
   
5. **Tamam**’ı seçin.
   
    ![Sysprep Başlat](./media/upload-generalized-managed/sysprepgeneral.png)

6. Sysprep tamamlandığında, VM'yi kapatır. VM yeniden başlatmayın.


## <a name="create-a-managed-image-in-the-portal"></a>Portalda yönetilen bir görüntü oluşturma 

1. [Azure portalı](https://portal.azure.com) açın.

2. Soldaki menüde **sanal makineler** ve ardından listeden VM'yi seçin.

3. İçinde **sanal makine** üst menüsünde, VM için sayfayı seçin **yakalama**.

   **Görüntüsü oluştur** sayfası görüntülenir.

4. İçin **adı**, önceden doldurulmuş adını kabul edin veya görüntüsü için kullanmak istediğiniz bir ad girin.

5. İçin **kaynak grubu**, yi **Yeni Oluştur** ve bir ad girin veya seçin **var olanı kullan** ve açılır listeden kullanılacak bir kaynak grubu seçin.

6. Görüntü alındıktan sonra oluşturulan, select, kaynak VM silmek istiyorsanız **görüntüyü oluşturduktan sonra bu sanal makineyi otomatik olarak Sil**.

7. Görüntü birinde kullanma olanağı istiyorsanız [kullanılabilirlik alanı](../../availability-zones/az-overview.md)seçin **üzerinde** için **bölge dayanıklılığı**.

8. Seçin **Oluştur** görüntüyü oluşturmak için.

9. Görüntü oluşturulduktan sonra olarak bulabilirsiniz bir **görüntü** kaynak kaynak grubundaki kaynaklar listesinde.



## <a name="create-an-image-of-a-vm-using-powershell"></a>Powershell kullanarak bir VM görüntüsü oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Doğrudan VM'den görüntü oluşturma, görüntü işletim sistemi diski ve veri diskleri dahil olmak üzere VM ile ilişkili tüm diskleri içerdiğinden sağlar. Bu örnekte, yönetilen diskleri kullanan bir VM'den yönetilen bir görüntü oluşturma işlemi gösterilmektedir.

Başlamadan önce Azure PowerShell modülünün en son sürümüne sahip olduğunuzdan emin olun. Sürümü bulmak için çalıştırın `Get-Module -ListAvailable Az` PowerShell'de. Yükseltmeniz gerekirse bkz [PowerShellGet ile Windows üzerindeki Azure PowerShell yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, çalıştırma `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için.


> [!NOTE]
> Görüntünüzü bölgesel olarak yedekli depolamada depolamak istiyorsanız, desteklediği bir bölgede oluşturmanız gerekir [kullanılabilirlik](../../availability-zones/az-overview.md) ve `-ZoneResilient` görüntü yapılandırması parametresinde (`New-AzImageConfig` komutu).

Bir VM görüntüsü oluşturmak için aşağıdaki adımları izleyin:

1. Bazı değişkenler oluşturun.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. VM serbest emin olun.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Sanal makinenin durumunu **Genelleştirmiş**. 
   
    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Sanal makineyi alın. 

    ```azurepowershell-interactive
    $vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $image = New-AzImageConfig -Location $location -SourceVirtualMachineId $vm.Id 
    ```
6. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 

## <a name="create-an-image-from-a-managed-disk-using-powershell"></a>PowerShell kullanarak bir yönetilen diskten görüntü oluşturma

Yalnızca işletim sistemi disk görüntüsü oluşturmak istiyorsanız, işletim sistemi diski olarak yönetilen diskin Kimliğini belirtin:

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```

2. VM Al.

   ```azurepowershell-interactive
   $vm = Get-AzVm -Name $vmName -ResourceGroupName $rgName
   ```

3. Yönetilen diskin Kimliğini alın.

    ```azurepowershell-interactive
    $diskID = $vm.StorageProfile.OsDisk.ManagedDisk.Id
    ```
   
3. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -ManagedDiskId $diskID
    ```
    
4. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-snapshot-using-powershell"></a>Powershell kullanarak anlık görüntüden bir görüntü oluşturma

Aşağıdaki adımları izleyerek, genelleştirilmiş bir sanal makinenin bir anlık görüntüden yönetilen bir görüntü oluşturabilirsiniz:

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Anlık görüntü alın.

   ```azurepowershell-interactive
   $snapshot = Get-AzSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Görüntü yapılandırması oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Görüntü oluşturun.

    ```azurepowershell-interactive
    New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-vhd-in-a-storage-account"></a>Bir depolama hesabında bir VHD'den Görüntü Oluştur

Genelleştirilmiş bir işletim sistemi VHD'si bir depolama hesabı, yönetilen bir görüntü oluşturun. Şu biçimde olduğundan depolama hesabında VHD URI'sini gerekir: https://*mystorageaccount*.blob.core.windows.net/*vhdcontainer* /  *vhdfilename.vhd*. Bu örnekte, VHD'nin bulunduğu *mystorageaccount*, adlı bir kapsayıcı içinde *vhdcontainer*, ve VHD dosya adı *vhdfilename.vhd*.


1.  Bazı değişkenler oluşturun.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    $osVhdUri = "https://mystorageaccount.blob.core.windows.net/vhdcontainer/vhdfilename.vhd"
    ```
2. Durdur/VM'yi serbest bırakın.

    ```azurepowershell-interactive
    Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. VM genelleştirilmiş olarak işaretleyin.

    ```azurepowershell-interactive
    Set-AzVm -ResourceGroupName $rgName -Name $vmName -Generalized  
    ```
4.  Görüntünün genelleştirilmiş işletim sistemi VHD'niz kullanarak oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzImageConfig -Location $location
    $imageConfig = Set-AzImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

    
## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen bir görüntüden VM oluşturma](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    

