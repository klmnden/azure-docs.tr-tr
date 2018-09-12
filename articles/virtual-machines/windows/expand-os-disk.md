---
title: Azure'da Windows sanal makinenin işletim sistemi sürücüsünü genişletme | Microsoft Docs
description: Azure Powershell kullanarak Resource Manager dağıtım modelinde bir sanal makinenin işletim sistemi sürücüsünü boyutunu genişletebilirsiniz.
services: virtual-machines-windows
documentationcenter: ''
author: kirpasingh
manager: roshar
editor: ''
tags: azure-resource-manager
ms.assetid: d9edfd9f-482f-4c0b-956c-0d2c2c30026c
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: kirpas
ms.openlocfilehash: 3ea57a834bfbb1583c53bbb1be80daffe1f05de6
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44380276"
---
# <a name="how-to-expand-the-os-drive-of-a-virtual-machine"></a>Nasıl bir sanal makinenin işletim sistemi sürücüsünü genişletme

Oluşturduğunuzda, yeni bir sanal makine (VM) bir kaynak grubunda bir görüntüyü dağıtarak [Azure Marketi](https://azure.microsoft.com/marketplace/), varsayılan işletim sistemi sürücüsü 127 GB (bazı görüntüleri küçük işletim sistemi disk boyutları varsayılan olarak sahip) genellikle olduğu. VM’ye veri diskleri eklemek (kaç tane ekleyebileceğiniz seçtiğiniz SKU’ya bağlıdır) mümkün olmasına, hatta uygulamaları ve yoğun CPU kullanımlı iş yüklerini bu ek disklere yüklemeniz önerilmesine rağmen, sıklıkla müşterilerin aşağıdaki gibi belirli senaryoları etkinleştirmesi için işletim sistemi sürücüsünü genişletmesi gerekir:

- İşletim sistemi sürücüsüne bileşen yükleyen eski uygulamaları destekleme.
- Fiziksel bir bilgisayarı veya sanal makineyi daha büyük bir işletim sistemi sürücüsüne sahip şirket içi kaynaktan geçirme.


> [!IMPORTANT]
> İşletim sistemi diski, bir Azure sanal makinesi yeniden boyutlandırma, yeniden başlatmaya neden olur.
>
> Diski genişlettikten sonra yapmanız [işletim sistemi içindeki birimi genişletmek](#expand-the-volume-within-the-os) büyük diskte yararlanmak için.
> 

## <a name="resize-a-managed-disk"></a>Bir yönetilen diski yeniden boyutlandırma

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Microsoft Azure hesabınızda kaynak yönetimi modunda oturum açın ve aboneliğinizi şu şekilde seçin:
   
   ```powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Yönetilen işletim sistemi diskini bir başvuru alın. Yönetilen işletim sistemi diskinin boyutunu istenen değere ayarlayın ve diski aşağıdaki gibi güncelleştirin:
   
   ```Powershell
   $disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
   $disk.DiskSizeGB = 1023
   Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
   ```   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İzin verilen en yüksek işletim sistemi diskleri için 2048 GB'dir. (Bu boyut ötesinde VHD blobunun genişletmek mümkündür, ancak işletim sistemi yalnızca ilk 2048 GB ile alanı çalışmaya mümkün olacaktır.)
   > 
   > 
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Hepsi bu! Şimdi RDP ile sanal makinenize girin, Bilgisayar Yönetimi’ni (veya Disk Yönetimi) açın ve yeni ayrılan alanı kullanarak sürücüyü genişletin.

## <a name="resize-an-unmanaged-disk"></a>Yönetilmeyen disk yeniden boyutlandırma

Powershell ISE veya Powershell pencerenizi yönetim modunda açın ve aşağıdaki adımları izleyin:

1. Microsoft Azure hesabınızda kaynak yönetimi modunda oturum açın ve aboneliğinizi şu şekilde seçin:
   
   ```Powershell
   Connect-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Aşağıdakileri yaparak kaynak grubunuzun adını ve VM adını ayarlayın:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Aşağıdakileri yaparak sanal makineniz için bir başvuru edinin:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Aşağıdakileri yaparak diski yeniden boyutlandırmadan önce VM’yi durdurun:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Yönetilmeyen işletim sistemi diskinin boyutunu istenen değere ayarlayın ve VM'yi şu şekilde güncelleştirin:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > Yeni boyut mevcut disk boyutundan büyük olmalıdır. İzin verilen en yüksek işletim sistemi diskleri için 2048 GB'dir. (Bu boyut ötesinde VHD blobunun genişletmek mümkündür, ancak işletim sistemi yalnızca ilk 2048 GB ile alanı çalışmaya mümkün olacaktır.)
   > 
   > 
   
6. VM güncelleştirmesi biraz zaman alabilir. Komutun yürütülmesi tamamlandığında aşağıdakileri yaparak VM’yi yeniden başlatın:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```


## <a name="scripts-for-os-disk"></a>İşletim sistemi diski için komut dosyaları

Tam betiği hem yönetilen hem de yönetilmeyen diskler için başvuru amacıyla aşağıda verilmiştir:


**Yönetilen diskler**

```Powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.OsDisk.Name
$disk.DiskSizeGB = 1023
Update-AzureRmDisk -ResourceGroupName $rgName -Disk $disk -DiskName $disk.Name
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

**Yönetilmeyen diskler**

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="resizing-data-disks"></a>Veri diskleri yeniden boyutlandırma

Bu makalede öncelikli olarak sanal Makinenin işletim sistemi diskini genişletmeye odaklanmış, ancak komut dosyası VM'ye bağlı veri disklerinin genişletilmesi için de kullanılabilir. Örneğin, VM’ye bağlı ilk veri diskini genişletmek için `StorageProfile` öğesinin `OSDisk` nesnesini `DataDisks` dizisi ile değiştirin ve aşağıda gösterildiği gibi sayısal bir dizin kullanarak ilk bağlanan veri diskinin başvurusunu edinin:

**Yönetilen disk**

```powershell
$disk= Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $vm.StorageProfile.DataDisks[0].Name
$disk.DiskSizeGB = 1023
```


**Yönetilmeyen disk**

```powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```



Benzer şekilde sanal bir dizin yukarıda da gösterildiği gibi kullanarak ya da bağlı diğer veri disklerine de başvurabilirsiniz veya **adı** diskin özelliği:


**Yönetilen disk**

```powershell
(Get-AzureRmDisk -ResourceGroupName $rgName -DiskName ($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'})).Name).DiskSizeGB = 1023
```

**Yönetilmeyen disk**

```powershell
($vm.StorageProfile.DataDisks | Where ({$_.Name -eq 'my-second-data-disk'}).DiskSizeGB = 1023
```

## <a name="expand-the-volume-within-the-os"></a>İçindeki işletim sistemi birimi genişletin

VM için diski genişlettikten sonra işletim sistemi gidin ve yeni alan kapsayacak şekilde birimini genişletin gerekir. Bir bölüm genişletmek için çeşitli yöntemler vardır. Bu bölümde ele alınmaktadır bölüm kullanarak genişletmek için RDP bağlantısı kullanarak VM'ye bağlanma **DiskPart**.

1. Sanal makinenize yönelik RDP bağlantısı açın.

2.  Bir komut istemi açıp **diskpart**.

2.  Konumunda **DISKPART** isteminde, yazın `list volume`. Genişletmek istediğiniz birim not edin.

3.  Konumunda **DISKPART** isteminde, yazın `select volume <volumenumber>`. Bu birimi seçer *birimnumarası* aynı disk üzerinde bitişik, boş alana genişletmek istediğiniz.

4.  Konumunda **DISKPART** isteminde, yazın `extend [size=<size>]`. Bu seçilen birimi tarafından genişletir *boyutu* megabayt (MB).


##<a name="next-steps"></a>Sonraki adımlar

Diskler kullanarak da ekleyebilirsiniz [Azure portalında](attach-managed-disk-portal.md).



