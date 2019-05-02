---
title: Öğretici - Azure PowerShell ile Azure disklerini yönetme | Microsoft Docs
description: Bu öğreticide, Azure CLI PowerShell kullanarak sanal makineler için Azure diskleri oluşturup yönetmeyi öğrenirsiniz
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/29/2018
ms.author: cynthn
ms.custom: mvc
ms.subservice: disks
ms.openlocfilehash: 4a60d3d77408e7c05311a2bd6bcceeb9331bd1af
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64924691"
---
# <a name="tutorial---manage-azure-disks-with-azure-powershell"></a>Öğretici - Azure PowerShell ile Azure disklerini yönetme

Azure sanal makineleri, VM’lerin işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır. VM oluştururken, beklenen iş yüküne uygun disk boyutu ve yapılandırmasını seçmek önemlidir. Bu öğreticide, VM disklerini dağıtma ve yönetme gösterilmektedir. Şunları öğreneceksiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

## <a name="default-azure-disks"></a>Varsayılan Azure diskleri

Azure sanal makinesi oluşturulduğunda, sanal makineye otomatik olarak iki disk eklenir. 

**İşletim sistemi diski** - İşletim sistemi diskleri 4 terabayta kadar boyutlandırılabilir ve VM'nin işletim sistemini barındırır.  İşletim sistemi diskine, varsayılan olarak *C:* sürücü harfi atanır. İşletim sistemi diskinin yapılandırmasını önbelleğe alan disk, işletim sistemi performansı için iyileştirilir. İşletim sistemi diski, uygulama veya veri **barındırmamalıdır**. Uygulamalar ve veriler için, bu makalede daha sonra ayrıntılı olarak açıklanan bir veri diski kullanın.

**Geçici disk** - Geçici diskler, VM ile aynı Azure ana bilgisayarında bulunan bir katı hal sürücüsü kullanır. Geçici diskler yüksek performansa sahiptir ve geçici veri işleme gibi işlemler için kullanılabilir. Ancak VM yeni bir konağa taşındığında, geçici diskte depolanan tüm veriler kaldırılır. Geçici diskin boyutu, [VM boyutu](sizes.md) tarafından belirlenir. Geçici disklere, varsayılan olarak bir *D:* sürücü harfi atanır.

## <a name="azure-data-disks"></a>Azure veri diskleri

Uygulama yüklemek ve veri depolamak için ek veri diskleri eklenebilir. Dayanıklı ve duyarlı veri depolama gerektiren her koşulda veri diskleri kullanılmalıdır. Sanal makinenin boyutu, bir VM’ye kaç veri diskinin eklenebileceğini belirler. Her VM vCPU için dört veri diski eklenebilir.

## <a name="vm-disk-types"></a>VM disk türleri

Azure iki disk türü sağlar.

**Standart diskler** - HDD’ler ile desteklenir ve uygun maliyetli bir depolama sağlarken iyi bir performans da sunar. Standart diskler, uygun maliyetli bir geliştirme ve iş yükü testi için idealdir.

**Premium diskler** - SSD tabanlı, yüksek performanslı ve düşük gecikme süreli disk ile desteklenir. Üretim iş yükü çalıştıran VM'ler için son derece uygundur. Premium Depolama, DS serisi, DSv2 serisi, GS serisi ve FS-serisi VM'lerini destekler. Premium diskler beş türde gelir (P10, P20, P30, P40, P50), diskin boyutu diskin türünü belirler. Disk boyutunu seçerken, boyutun değeri sonraki türe yuvarlanır. Örneğin, boyut 128 GB'ın altındaysa disk türü P10 veya 129 GB ile 512 GB arasındaysa disk P20’dir.

### <a name="premium-disk-performance"></a>Premium disk performansı
[!INCLUDE [disk-storage-premium-ssd-sizes](../../../includes/disk-storage-premium-ssd-sizes.md)]

Yukarıdaki tabloda, disk başına maksimum IOPS tanımlanmış olsa da birden çok veri diski bölümlenerek daha yüksek performansa ulaşılabilir. Örneğin, Standard_GS5 VM’ye 64 veri diski eklenebilir. Bu disklerin her biri P30 olarak boyutlandırılırsa, en fazla 80.000 IOPS’ye ulaşılabilir. VM başına maksimum IOPS hakkında ayrıntılı bilgi için bkz. [VM türleri ve boyutları](./sizes.md).

## <a name="create-and-attach-disks"></a>Disk oluşturma ve ekleme

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, aşağıdaki komutlarla bir sanal makine oluşturun.

[Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile sanal makinede yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın:


İle sanal makine oluşturma [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). VM’nin yönetici hesabı için bir kullanıcı adı ve parola girmeniz istenir.

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" 
```


İle ilk yapılandırmayı oluşturun [yeni AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig). Aşağıdaki örnek boyutu 128 gigabayt olan bir diski yapılandırır.

```azurepowershell-interactive
$diskConfig = New-AzDiskConfig `
    -Location "EastUS" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

Veri diskini oluşturun [yeni AzDisk](https://docs.microsoft.com/powershell/module/az.compute/new-Azdisk) komutu.

```azurepowershell-interactive
$dataDisk = New-AzDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

Veri diskini eklemek istediğiniz sanal makineyi Al [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) komutu.

```azurepowershell-interactive
$vm = Get-AzVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

Sanal makine yapılandırmasıyla veri diski ekleme [Ekle AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/add-azvmdatadisk) komutu.

```azurepowershell-interactive
$vm = Add-AzVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

Sanal makineyi güncelleştirin [güncelleştirme-AzVM](https://docs.microsoft.com/powershell/module/az.compute/add-azvmdatadisk) komutu.

```azurepowershell-interactive
Update-AzVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>Veri disklerini hazırlama

Bir disk sanal makineye eklendikten sonra, diskin kullanılması için işletim sisteminin yapılandırılması gerekir. Aşağıdaki örnek, VM’ye eklenen ilk diski el ile yapılandırmayı gösterir. Bu işlem [özel betik uzantısı](./tutorial-automate-vm-deployment.md) kullanılarak otomatik olarak da yapılabilir.

### <a name="manual-configuration"></a>El ile yapılandırma

Sanal makine ile bir RDP bağlantısı oluşturun. PowerShell'i açın ve bu betiği çalıştırın.

```azurepowershell
Get-Disk | Where partitionstyle -eq 'raw' |
    Initialize-Disk -PartitionStyle MBR -PassThru |
    New-Partition -AssignDriveLetter -UseMaximumSize |
    Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="verify-the-data-disk"></a>Veri diskini doğrulama

Veri diskinin eklendiğini doğrulamak amacıyla, eklenen `DataDisks` için `StorageProfile` öğesini görüntüleyin.

```azurepowershell-interactive
$vm.StorageProfile.DataDisks
```

Çıkış şu örneğe benzer olmalıdır:

```
Name            : myDataDisk
DiskSizeGB      : 128
Lun             : 1
Caching         : None
CreateOption    : Attach
SourceImage     :
VirtualHardDisk :
```


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki VM disk konularını öğrendiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

VM yapılandırmasını otomatikleştirme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM yapılandırmasını otomatikleştirme](./tutorial-automate-vm-deployment.md)
