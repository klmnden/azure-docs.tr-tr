---
title: Yönetilen bir görüntü oluşturma | Microsoft Docs
description: Genelleştirilmiş bir VM veya VHD yönetilen bir görüntüsünü oluşturma. Görüntüleri yönetilen diskler kullanan birden çok VM oluşturmak için kullanılabilir.
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
ms.date: 03/06/2018
ms.author: cynthn
ms.openlocfilehash: 0b0bd48b95ad9393b4cd82081436e561326df6da
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Yönetilen bir genelleştirilmiş bir VM görüntüsü oluşturma

Yönetilen görüntü kaynağı olarak yönetilen bir disk veya yönetilmeyen bir disk depolama hesabında depolanan genelleştirilmiş bir VM oluşturulabilir. Görüntü sonra birden çok VM oluşturmak için kullanılabilir. 

## <a name="generalize-the-windows-vm-using-sysprep"></a>Sysprep kullanarak Windows VM generalize

Sysprep tüm kişisel hesap bilgilerinize, başka şeylerin kaldırır ve bir görüntü olarak kullanılacak makine hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx).

Makinede çalışan sunucu rollerini Sysprep tarafından desteklendiğinden emin olun. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Bir VM kabul sysprep çalıştırdıktan sonra *genelleştirilmiş* ve yeniden başlatılamıyor. Bir VM genelleme işlemi ters çevrilebilir değil. VM özgün çalışmasını tutmak gerekiyorsa, almanız gereken bir [VM kopyasını](create-vm-specialized.md#option-3-copy-an-existing-azure-vm) ve kopyayı genelleştirin. 
>
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


## <a name="create-a-managed-image-in-the-portal"></a>Portalda yönetilen bir görüntü oluşturma 

1. Açık [portal](https://portal.azure.com).
2. Soldaki menüde sanal makineleri tıklayın ve ardından VM listeden seçin.
3. Sayfa üst menüsünde VM için tıklatın **yakalama**.
3. İçinde **adı**, görüntü için kullanmak istediğiniz adı yazın.
4. İçinde **kaynak grubu** ya da seçin **Yeni Oluştur** ve bir ad yazın veya seçin **var olanı kullan** ve aşağı açılan listeden kullanmak için bir kaynak grubu seçin.
5. Kaynak VM görüntü alındıktan sonra oluşturulan, select silmek istiyorsanız **görüntüsünü oluşturduktan sonra bu sanal makine otomatik olarak Sil**.
6. İşiniz bittiğinde **Oluştur**’a tıklayın.
16. Görüntü oluşturulduktan sonra bunu olarak görür bir **görüntü** kaynak kaynak grubundaki kaynaklar listesinde.



## <a name="create-an-image-of-a-vm-using-powershell"></a>Powershell kullanarak bir VM görüntüsü oluşturma

Doğrudan sanal makineden bir görüntü oluşturma görüntünün işletim sistemi diski ve veri diskleri gibi VM ile ilişkili tüm diskleri içeren sağlar. Bu örnek yönetilen disklerde kullanan bir sanal makineden bir yönetilen görüntüsü oluşturulacağını gösterir.


Başlamadan önce AzureRM.Compute PowerShell modülü en son sürümüne sahip olduğunuzdan emin olun. Yüklemek için aşağıdaki komutu çalıştırın. (Kullanım `Get-Module` hangi sürüm denetlemek için.)

```azurepowershell-interactive
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Daha fazla bilgi için bkz: [Azure PowerShell sürüm](/powershell/azure/overview).


1. Bazı değişkenler oluşturun.

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. VM serbest emin olun.

    ```azurepowershell-interactive
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Sanal makine durumunu ayarlamak **Genelleştirmiş**. 
   
    ```azurepowershell-interactive
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Sanal makine Al. 

    ```azurepowershell-interactive
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Görüntü yapılandırmasını oluşturun.

    ```azurepowershell-interactive
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Görüntü oluşturma.

    ```azurepowershell-interactive
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 
## <a name="create-an-image-from-a-managed-disk-using-powershell"></a>PowerShell kullanarak yönetilen bir diskten görüntü oluşturma

İşletim sistemi diski olarak yönetilen disk kimliği belirterek, yalnızca işletim sistemi diski görüntüsünü oluşturmak istiyorsanız, bir görüntü oluşturabilirsiniz.

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. VM Al

   ```azurepowershell-interactive
   $vm = Get-AzureRmVm -Name $vmName -ResourceGroupName $rgName
   ```

3. Yönetilen disk Kimliğini alın.

    ```azurepowershell-interactive
    $diskID = $vm.StorageProfile.OsDisk.ManagedDisk.Id
    ```
   
3. Görüntü yapılandırmasını oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -ManagedDiskId $diskID
    ```
    
4. Görüntü oluşturma.

    ```azurepowershell-interactive
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-an-image-from-a-snapshot-using-powershell"></a>Powershell kullanarak bir anlık görüntüden görüntü oluşturma

Genelleştirilmiş bir VM'nin bir anlık görüntüden yönetilen bir görüntü oluşturabilirsiniz.

    
1. Bazı değişkenler oluşturun. 

    ```azurepowershell-interactive
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Anlık görüntü alın.

   ```azurepowershell-interactive
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Görüntü yapılandırmasını oluşturun.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Görüntü oluşturma.

    ```azurepowershell-interactive
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 


## <a name="create-image-from-a-vhd-in-a-storage-account"></a>Bir depolama hesabındaki bir VHD'den Görüntü Oluştur

Bir depolama hesabında genelleştirilmiş bir işletim sistemi VHD'den yönetilen bir görüntü oluşturun. VHD biçimi https:// depolama hesabındaki URI'sini gereksinim*mystorageaccount*.blob.core.windows.net/*kapsayıcı*/*vhd_filename.vhd*. Bu örnekte, kullanıyoruz VHD bulunduğu *mystorageaccount* adlı bir kapsayıcıda *vhdcontainer* ve VHD dosya adı *osdisk.vhd*.


1.  İlk olarak, ortak parametreleri ayarlayın:

    ```azurepowershell-interactive
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    $osVhdUri = "https://mystorageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate VM.

    ```azurepowershell-interactive
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. VM genelleştirilmiş olarak işaretleyin.

    ```azurepowershell-interactive
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  Genelleştirilmiş OS VHD kullanarak görüntü oluşturma.

    ```azurepowershell-interactive
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

    
## <a name="next-steps"></a>Sonraki adımlar
- Artık [bir VM genelleştirilmiş yönetilen görüntüden oluşturma](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

