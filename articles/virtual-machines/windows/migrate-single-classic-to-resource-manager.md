---
title: Bir ARM yönetilen Disk VM'si Klasik VM'yi geçirme | Microsoft Docs
description: Tek bir Azure sanal makine Klasik dağıtım modelinden Resource Manager dağıtım modelinde yönetilen disklere geçirme.
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: a662a61d737dbb620d07fa6d114649e70c082796
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329778"
---
# <a name="migrate-a-classic-vm-to-use-a-managed-disk"></a>Yönetilen bir Disk kullanmak üzere Klasik VM'yi geçirme 


Bu bölümde, Klasik dağıtım modeli için mevcut bir Azure sanal makinelerinizin geçiş yardımcı olur. [yönetilen diskler](managed-disks-overview.md) Resource Manager dağıtım modelinde.


## <a name="plan-for-the-migration-to-managed-disks"></a>Yönetilen Diskler'e geçiş planlama

Bu bölümde sanal makine disk türlerinde en iyi kararın yapmanıza yardımcı olur.


### <a name="location"></a>Konum

Yönetilen diskler olduğu bir konum seçin. Premium Depolama tarafından yedeklenen diske yönetilen geçiriyorsanız, ayrıca Premium depolama bu bölgede kullanılabilir olduğundan emin olun. Bkz: [Azure Hizmetleri byRegion](https://azure.microsoft.com/regions/#services) kullanılabilir konumların hakkında güncel bilgi için.

### <a name="vm-sizes"></a>VM boyutları

Bir yönetilen kullanarak Premium depolama diskleri için geçiriyorsanız, VM boyutu Premium depolama özellikli boyutu kullanılabilir VM bulunduğu bölgede güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makine boyutları](sizes.md).
Premium depolama ile çalışır ve iş yükünüz gereksinimlerinize en uygun bir VM boyutu seçme sanal makineleri performans özelliklerini gözden geçirin. Kullanılabilir yeterli bant genişliği, VM diski trafiği yönlendirmek emin olun.

### <a name="disk-sizes"></a>Disk boyutları

**Premium**

VM'nizi kullanılabilir Premium depolama yedi türleri vardır ve her belirli IOPS ve aktarım hızı sahip sınırları. Bu limitler kapasite, performans, ölçeklenebilirlik açısından, uygulamanızın gereksinimlerine göre VM için Premium disk türü seçerken göz önünde bulundurun ve en yüksek yükler.

| Premium disk türü  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniye başına 100 MB | 150 MB / saniye | Saniye başına 200 MB | Saniye başına 250 MB | Saniye başına 250 MB | 

**Standart**

VM'nizi ile kullanılabilecek standart disk yedi türleri vardır. Bunların her biri farklı bir kapasiteye sahip ancak aynı IOPS ve aktarım hızı sınırlarına sahip. Standart yönetilen diskler, uygulamanızın kapasite ihtiyaçlarını temel alarak türünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| Disk başına IOPS       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Disk başına aktarım hızı | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | Saniyede 60 MB | 


### <a name="disk-caching-policy"></a>Diski önbelleğe alma İlkesi 

**Premium yönetilen diskler**

Varsayılan olarak, önbelleğe alma İlkesi disktir *salt okunur* tüm Premium veri disklerinde, ve *okuma-yazma* Premium işletim sistemi diski, VM'ye bağlı. Bu yapılandırma ayarının, uygulamanızın IOs için en iyi performans elde etmek için önerilir. (Örneğin, SQL Server günlük dosyası) yazma yoğunluklu veya salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

### <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/pricing/details/managed-disks/). Yönetilen Premium disklere yönelik fiyatlandırma, yönetilmeyen premium diskler olarak aynıdır. Ancak, standart yönetilen diskler için fiyatlandırmaya göz standart yönetilmeyen disklerden farklıdır.


## <a name="checklist"></a>Denetim listesi

1.  Premium yönetilen disklere geçiriyorsanız, için geçirdiğiniz bölgede kullanılabilir olduğundan emin olun.

2.  Kullanacağınız yeni VM serisi karar verin. Premium yönetilen disklere geçiriyorsanız bir Premium depolama özelliğine sahip olması gerekir.

3.  İçin geçirdiğiniz bölgede kullanılabilir olan kullanacağınız tam VM boyutuna karar verin. VM boyutu, sahip olduğunuz veri diski sayısı destekleyecek kadar büyük olması gerekiyor. Örneğin, dört veri diskleri varsa, VM en az iki çekirdek olmalıdır. Ayrıca, işleme güç, bellek ve ağ bant genişliği gereksinimlerini göz önünde bulundurun.

4.  Geçerli sanal makine ayrıntıları kullanışlı, diskler ve karşılık gelen VHD bloblarını listesi dahil olmak üzere vardır.

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için tüm işlemlerin geçerli sistemde durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebileceğiniz tutarlı bir duruma alabilirsiniz. Kapalı kalma süresi geçirmek için disklerde veri miktarına bağlıdır.


## <a name="migrate-the-vm"></a>VM'yi geçirme

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için tüm işlemlerin geçerli sistemde durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebileceğiniz tutarlı duruma alabilirsiniz. Kapalı kalma süresi geçirmek için disklerde veri miktarı bağlıdır.

Azure PowerShell modülü sürüm 6.0.0'dan bu bölümü gerektirir veya üzeri. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). Ayrıca Azure ile bağlantı oluşturmak için `Connect-AzureRmAccount` komutunu çalıştırmanız gerekir.


Ortak parametre değişkenleri oluşturun.

```powershell
$resourceGroupName = 'yourResourceGroupName'

$location = 'your location' 

$virtualNetworkName = 'yourExistingVirtualNetworkName'

$virtualMachineName = 'yourVMName'

$virtualMachineSize = 'Standard_DS3'

$adminUserName = "youradminusername"

$adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force

$imageName = 'yourImageName'

$osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'

$dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'

$dataDiskName = 'dataDisk1'
```

Klasik VM'den VHD kullanarak bir yönetilen işletim sistemi diski oluşturun. İşletim sistemi VHD'si $osVhdUri parametresine tam URI'si sağladığınızdan emin olun. Ayrıca, girin **- AccountType** olarak **Premium_LRS** veya **Standard_LRS** bağlı diskler (premium veya standart) türüne göre için geçirdiğiniz.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreateOption Import '
   -SourceUri $osVhdUri) '
   -ResourceGroupName $resourceGroupName
```

İşletim sistemi diski yeni sanal Makineye ekleyin.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
$VirtualMachine = Set-AzureRmVMOSDisk '
   -VM $VirtualMachine '
   -ManagedDiskId $osDisk.Id '
   -StorageAccountType Premium_LRS '
   -DiskSizeInGB 128 '
   -CreateOption Attach -Windows
```

Veri VHD dosyasından yönetilen veri diski oluşturmak ve yeni sanal Makineye ekleyin.

```powershell
$dataDisk1 = New-AzureRmDisk '
   -DiskName $dataDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreationOption Import '
   -SourceUri $dataVhdUri ) '
   -ResourceGroupName $resourceGroupName
    
$VirtualMachine = Add-AzureRmVMDataDisk '
   -VM $VirtualMachine '
   -Name $dataDiskName '
   -CreateOption Attach '
   -ManagedDiskId $dataDisk1.Id '
   -Lun 1
```

Genel IP, sanal ağ ve NIC ayarlayarak yeni VM oluşturma

```powershell
$publicIp = New-AzureRmPublicIpAddress '
   -Name ($VirtualMachineName.ToLower()+'_ip') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -AllocationMethod Dynamic
    
$vnet = Get-AzureRmVirtualNetwork '
   -Name $virtualNetworkName '
   -ResourceGroupName $resourceGroupName
    
$nic = New-AzureRmNetworkInterface '
   -Name ($VirtualMachineName.ToLower()+'_nic') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -SubnetId $vnet.Subnets[0].Id '
   -PublicIpAddressId $publicIp.Id
    
$VirtualMachine = Add-AzureRmVMNetworkInterface '
   -VM $VirtualMachine '
   -Id $nic.Id
    
New-AzureRmVM -VM $VirtualMachine '
   -ResourceGroupName $resourceGroupName '
   -Location $location
```

> [!NOTE]
>Ek adımlar olması, uygulamanızı desteklemek için gerekli değil Bu kılavuzda ele.
>
>

## <a name="next-steps"></a>Sonraki adımlar

- Sanal makineye bağlanın. Yönergeler için [bağlanma ve bir Azure Windows çalıştıran sanal makine için oturum açma nasıl](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

