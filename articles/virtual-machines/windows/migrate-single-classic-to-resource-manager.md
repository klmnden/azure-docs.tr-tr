---
title: Bir ARM yönetilen Disk VM'si klasik bir VM geçirme | Microsoft Docs
description: Tek bir Azure VM'ye Klasik dağıtım modelinden Resource Manager dağıtım modelinde yönetilen disklere geçirin.
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
ms.openlocfilehash: 1241f893ca69e3ddaf464e66943caa2697e6d8e7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a>El ile yeni bir ARM yönetilen Disk sanal makineye bir Klasik VM VHD'den geçirme 


Bu bölümde, mevcut Azure sanal makineleri Klasik dağıtım modeli için geçiş yardımcı olacak [yönetilen diskleri](managed-disks-overview.md) Resource Manager dağıtım modelinde.


## <a name="plan-for-the-migration-to-managed-disks"></a>Yönetilen disklere geçişi için planlama

Bu bölümde, VM ve disk türlerinde en iyi kararı yardımcı olur.


### <a name="location"></a>Konum

Azure yönetilen diskleri kullanılabildiği bir konum seçin. Premium yönetilen disklere geçiriyorsanız, ayrıca Premium depolama burada geçiş yapmayı planlıyorsanız bölgede kullanılabilir olduğundan emin olun. Bkz: [Azure Hizmetleri byRegion](https://azure.microsoft.com/regions/#services) kullanılabilir konumları hakkında güncel bilgi.

### <a name="vm-sizes"></a>VM boyutları

Premium yönetilen disklere geçiriyorsanız, Premium depolama yeteneğine sahip bir boyut kullanılabilir VM bulunduğu bölgede VM boyutu güncelleştirmeniz gerekir. Premium depolama özelliğine sahip VM boyutları gözden geçirin. Azure VM boyutu belirtimleri listelenen [sanal makineler için Boyutlar](sizes.md).
Premium Storage ile birlikte çalışmak ve İş yükünüzün uygun en uygun VM boyutunu seçin sanal makineleri performans özelliklerini gözden geçirin. Yeterli bant genişliği kullanılabilir disk trafiği sürücü için de kendi VM'nizi bulunduğundan emin olun.

### <a name="disk-sizes"></a>Disk boyutları

**Premium yönetilen diskleri**

Yedi disk türleri için VM ile kullanılabilen premium yönetilen vardır ve her belirli IOPS ve üretilen iş sahip sınırlar. Kapasite, performans, ölçeklenebilirlik açısından, uygulamanızın gereksinimlerine göre VM için Premium disk türü seçerken bu sınırları göz önünde bulundurun ve en yüksek yükler.

| Premium diskler türü  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Disk boyutu           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Disk başına IOPS       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Disk başına aktarım hızı | Saniye başına 25 MB  | Saniye başına 50 MB  | Saniyede 100 MB | 150 MB / saniye | 200 MB / saniye | Saniye başına 250 MB | Saniye başına 250 MB | 

**Standart yönetilen disk**

VM ile kullanılabilecek standart yönetilen disk yedi türü vardır. Bunların her biri farklı kapasiteye sahip ancak aynı IOPS ve üretilen iş sınırı vardır. Uygulamanızı kapasite gereksinimlerine göre standart yönetilen disk türünü seçin.

| Standart Disk Türü  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Disk boyutu           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2TB)    | 4095 GB (4 TB)   | 
| Disk başına IOPS       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Disk başına aktarım hızı | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | Saniye başına 60 MB | 


### <a name="disk-caching-policy"></a>Önbellek İlkesi disk 

**Premium yönetilen diskleri**

Varsayılan olarak, ilke önbelleğe alma disktir *salt okunur* tüm Premium veri diskleri için ve *okuma-yazma* için Premium işletim sistemi diski VM'ye eklenmiş. Bu yapılandırma ayarının uygulamanızın IOs için en iyi performans elde etmek için önerilir. (SQL Server günlük dosyaları gibi) ağır yazma ya da salt yazılır veri diskleri için daha iyi uygulama performansı elde etmek için disk önbelleğe almayı devre dışı bırakın.

### <a name="pricing"></a>Fiyatlandırma

Gözden geçirme [yönetilen diskler için fiyatlandırma](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Premium diskler yönetilen fiyatlandırma Premium yönetilmeyen diskleri olarak aynıdır. Ancak standart yönetilen disk için fiyatlandırma standart yönetilmeyen disklerden farklı.


## <a name="checklist"></a>Denetim listesi

1.  Premium yönetilen disklere geçiriyorsanız, bu geçiş yaptığınız bölgede kullanılabilir olduğundan emin olun.

2.  Kullanacağınız yeni VM dizisi karar verin. Premium yönetilen disklere geçiriyorsanız özellikli bir Premium depolama olmalıdır.

3.  Geçiş yaptığınız bölgede kullanılabilir olan kullanacağınız tam VM boyutu karar verin. VM boyutu sahip veri diski sayısı desteklemek için büyük olması gerekir. Örneğin, dört veri diski varsa, VM iki veya daha fazla çekirdek olması gerekir. Ayrıca, işlemci gücü, bellek göz önünde bulundurun ve ağ bant genişliği gerekiyor.

4.  Kullanışlı, diskler ve karşılık gelen VHD BLOB'ları listesi dahil olmak üzere geçerli VM ayrıntıları vardır.

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için geçerli sistemdeki tüm işleme durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebilirsiniz tutarlı durumuna alabilir. Kapalı kalma süresi geçirmek için diskleri veri miktarına bağlıdır.


## <a name="migrate-the-vm"></a>VM geçirme

Uygulamanızı kapalı kalma süresi için hazırlayın. Temiz bir geçiş yapmak için geçerli sistemdeki tüm işleme durdurmak zorunda. Ancak bundan sonra yeni platforma geçirebilirsiniz tutarlı durumuna alabilir. Kapalı kalma süresi geçirmek için diskleri veri miktarına bağlıdır.


1.  İlk olarak, ortak parametreleri ayarlayın:

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

2.  Klasik VM VHD'den kullanarak yönetilen bir işletim sistemi diski oluşturun.

    $OsVhdUri parametre OS VHD'ye tam URI'si sağladığınız emin olun. Ayrıca, girin **- AccountType** olarak **PremiumLRS** veya **StandardLRS** temel diskleri (Premium veya standart) türüne için geçirdiğiniz.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  İşletim sistemi diski yeni VM'e ekleyin.

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  Veri VHD dosyasından bir yönetilen veri diski oluşturun ve yeni VM'ye ekleyin.

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  Ortak IP, sanal ağ ve NIC ayarlayarak yeni VM oluşturma

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
>Ek adımlar olabilir, uygulama desteklemek için gerekli kapsanmayan bu kılavuzda.
>
>

## <a name="next-steps"></a>Sonraki adımlar

- Sanal makineye bağlanın. Yönergeler için bkz: [bağlanmayı ve Windows çalıştıran Azure sanal makinesi için oturum](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

