---
title: "Azure PowerShell ile Azure disklerini yönetme | Microsoft Docs"
description: "Öğretici - Azure PowerShell ile Azure disklerini yönetme"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ea38fe599960db42c518603b59a60a920d1f1daf
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="manage-azure-disks-with-powershell"></a>PowerShell ile Azure disklerini yönetme

Azure sanal makineleri, VM’lerin işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır. Bir VM oluştururken, beklenen iş yüküne uygun disk boyutu ve yapılandırmasını seçmek önemlidir. Bu öğreticide, VM disklerini dağıtma ve yönetme gösterilir. Bu konu hakkında bilgi edinirsiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 

## <a name="default-azure-disks"></a>Varsayılan Azure diskleri

Azure sanal makinesi oluşturulduğunda, sanal makineye otomatik olarak iki disk eklenir. 

**İşletim sistemi diski** - İşletim sistemi diskleri 4 terabayta kadar boyutlandırılabilir ve VM'nin işletim sistemini barındırır.  İşletim sistemi diskine, varsayılan olarak bir *c:* sürücü harfi atanır. İşletim sistemi diskinin yapılandırmasını önbelleğe alan disk, işletim sistemi performansı için iyileştirilir. İşletim sistemi diski, uygulama veya veri **barındırmamalıdır**. Uygulamalar ve veriler için, bu makalede daha sonra ayrıntılı olarak açıklanan bir veri diski kullanın.

**Geçici disk** - Geçici diskler, VM ile aynı Azure ana bilgisayarında bulunan bir katı hal sürücüsü kullanır. Geçici diskler yüksek performansa sahiptir ve geçici veri işleme gibi işlemler için kullanılabilir. Ancak, VM yeni bir ana bilgisayara taşındığında, geçici bir diskte depolanan tüm veriler kaldırılır. Geçici diskin boyutu, VM boyutu tarafından belirlenilir. Geçici disklere, varsayılan olarak bir *d:* sürücü harfi atanır.

### <a name="temporary-disk-sizes"></a>Geçici disk boyutları

| Tür | Ortak boyutlar | En yüksek geçici disk boyutu (GiB) |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A, B ve D serisi | 1600 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 576 |
| [Bellek için iyileştirilmiş](sizes-memory.md) | D, E, G ve M serisi | 6144 |
| [Depolama için iyileştirilmiş](sizes-storage.md) | L serisi | 5630 |
| [GPU](sizes-gpu.md) | N serisi | 1440 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 2000 |

## <a name="azure-data-disks"></a>Azure veri diskleri

Uygulama yüklemek ve veri depolamak için ek veri diskleri eklenebilir. Dayanıklı ve duyarlı veri depolamanın istenildiği her koşulda veri diskleri kullanılmalıdır. Her veri diski 4 terabaytlık maksimum kapasiteye sahiptir. Sanal makinenin boyutu, bir VM’ye kaç veri diskinin eklenebileceğini belirler. Her VM vCPU için iki veri diski eklenebilir. 

### <a name="max-data-disks-per-vm"></a>VM başına en fazla veri diski

| Tür | Ortak boyutlar | VM başına en fazla veri diski |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A, B ve D serisi | 64 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 64 |
| [Bellek için iyileştirilmiş](sizes-memory.md) | D, E, G ve M serisi | 64 |
| [Depolama için iyileştirilmiş](sizes-storage.md) | L serisi | 64 |
| [GPU](sizes-gpu.md) | N serisi | 64 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 64 |

## <a name="vm-disk-types"></a>VM disk türleri

Azure iki disk türü sağlar.

### <a name="standard-disk"></a>Standart disk

Standart Depolama, HDD’ler ile desteklenir ve yüksek performans sunarken uygun maliyetli depolama sağlar. Standart diskler, uygun maliyetli bir geliştirme ve iş yükü testi için idealdir.

### <a name="premium-disk"></a>Premium disk

Premium diskler, SSD tabanlı yüksek performanslı, düşük gecikme süreli disk tarafından desteklenir. Üretim iş yükü çalıştıran VM'ler için mükemmeldir. Premium Depolama, DS serisi, DSv2 serisi, GS serisi ve FS-serisi VM'lerini destekler. Premium diskler beş türde gelir (P10, P20, P30, P40, P50), diskin boyutu diskin türünü belirler. Disk boyutunu seçerken, boyutun değeri sonraki türe yuvarlanır. Örneğin, boyut 128 GB'ın altındaysa disk türü P10 veya 129 GB ile 512 GB arasındaysa disk P20’dir.

### <a name="premium-disk-performance"></a>Premium disk performansı

|Premium depolama diski türü | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Disk boyutu (yuvarlanmış değer) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024 GB (1 TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| Disk başına en fazla IOPS | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Disk başına aktarım hızı | 25 MB/sn | 50 MB/sn | 100 MB/s | 150 MB/s | 200 MB/sn | 250 MB/sn | 250 MB/sn |

Yukarıdaki tablo, disk başına maksimum IOPS tanımlamış olsa da, daha yüksek düzeyde performansa birden çok veri diskini bölümleyerek ulaşılabilir. Örneğin, Standard_GS5 VM’ye 64 veri diski eklenebilir. Bu disklerin her biri P30 olarak boyutlandırılırsa, en fazla 80.000 IOPS’a ulaşılabilir. VM başına maksimum IOPS hakkında ayrıntılı bilgi için bkz. [VM türleri ve boyutları](./sizes.md).

## <a name="create-and-attach-disks"></a>Disk oluşturma ve ekleme

Bu öğreticideki örneği tamamlamak için, mevcut bir sanal makinenizin olması gerekir. Gerekirse, aşağıdaki komutlarla bir sanal makine oluşturun.

[Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile sanal makinede yönetici hesabı için gereken kullanıcı adı ve parolasını ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile sanal makineyi oluşturun.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -AsJob
```

PowerShell komut istemlerinin size döndürülmesi için `-AsJob` parametresi VM’yi arka plan görevi olarak oluşturur. Arka plan işlerinin ayrıntılarını `Job` cmdlet'i ile görüntüleyebilirsiniz.

[New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) ile ilk yapılandırmayı oluşturun. Aşağıdaki örnek boyutu 128 gigabayt olan bir diski yapılandırır.

```azurepowershell-interactive
$diskConfig = New-AzureRmDiskConfig `
    -Location "EastUS" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

[New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) komutuyla veri diskini oluşturun.

```azurepowershell-interactive
$dataDisk = New-AzureRmDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) komutuyla veri diskini eklemek istediğiniz sanal makineyi alın.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

[Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) komutuyla veri diskini sanal makine yapılandırmasına ekleyin.

```azurepowershell-interactive
$vm = Add-AzureRmVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

[Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) komutuyla sanal makineyi güncelleştirin.

```azurepowershell-interactive
Update-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>Veri disklerini hazırlama

Bir disk sanal makineye eklendikten sonra, diskin kullanılması için işletim sisteminin yapılandırılması gerekir. Aşağıdaki örnek, VM’ye eklenen ilk diski el ile yapılandırmayı gösterir. Bu işlem [özel betik uzantısı](./tutorial-automate-vm-deployment.md) kullanılarak otomatik olarak da yapılabilir.

### <a name="manual-configuration"></a>El ile yapılandırma

Sanal makine ile bir RDP bağlantısı oluşturun. PowerShell'i açın ve bu betiği çalıştırın.

```azurepowershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunun gibi VM disk konularını öğrendiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama

VM yapılandırmasını otomatikleştirme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [VM yapılandırmasını otomatikleştirme](./tutorial-automate-vm-deployment.md)
