---
title: Bir Windows Azure PowerShell ile sanal makine sorunlarını giderme kullanın | Microsoft Docs
description: İşletim sistemi diskini bir kurtarma için Azure PowerShell kullanarak VM bağlanarak azure'da Windows VM sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/09/2018
ms.author: genli
ms.openlocfilehash: 94abf9c8621e842605a4fab521fa4df853e1fb4a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709295"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>İşletim sistemi diskini bir kurtarma için Azure PowerShell kullanarak VM ekleyerek bir Windows sanal makinesinin sorunlarını giderme
Azure'da Windows sanal makinesi (VM), önyükleme veya disk bir hatasıyla karşılaşırsa, diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın olarak karşılaşılan örneklerden VM başarıyla önyükleme engelleyen bir uygulamanın güncelleştirme olacaktır. Bu makalede, diski başka bir Windows varsa hataları düzeltin ve ardından orijinal VM'yi onarın VM'ye bağlanmak için Azure PowerShell kullanma işlemi açıklanmaktadır. 

> [!Important]
> Bu makalede komut dosyalarını kullanan VM'ler için yalnızca geçerli [yönetilen Disk](../windows/managed-disks-overview.md). 

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Artık Azure PowerShell bir VM için işletim sistemi diskini değiştirmek için kullanabiliriz. Artık VM silip ihtiyacımız var.

Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen sanal Makineyi durdurun.
2. Sanal makinenin işletim sistemi diskinden anlık görüntüsünü oluşturun.
3. Bir diski işletim sistemi disk anlık görüntüden oluşturun.
4. Diski bir kurtarma sanal Makinesine veri diski olarak ekleyin.
5. Kurtarma sanal Makinesine bağlanın. Dosyaları düzenleyin veya kopyalanmış işletim sistemi diskinde sorunlarını düzeltmek için herhangi bir aracı çalıştırın.
6. Çıkarın ve kurtarma sanal Makinesine disk ayırma.
7. Etkilenen sanal makine için işletim sistemi diski değiştirin.

VM kurtarma betikleri, 1, 2, 3, 4, 6 ve 7. adımları otomatikleştirmek için kullanabilirsiniz. Daha fazla belge ve yönergeler için bkz. [Resource Manager VM için VM kurtarma betikleri](https://github.com/Azure/azure-support-scripts/tree/master/VMRecovery/ResourceManager).

Sahip olduğunuzdan emin olun [en son Azure PowerShell](/powershell/azure/overview) yüklü ve aboneliğinize oturum:

```powershell
Connect-AzAccount
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. 

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Azure'da önyükleme sorunlarını gidermenize yardımcı olması için VM görüntüsü görüntüleyebilirsiniz. Bu ekran görüntüsünde, bir VM önyükleme başaramıyor neden belirlemenize yardımcı olabilir. Aşağıdaki örnek ekran görüntüsünde Windows adlı VM'den alır `myVM` adlı kaynak grubunda `myResourceGroup`:

```powershell
Get-AzVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Sanal Makineyi önyüklemek neden başarısız olduğunu belirlemek için ekran gözden geçirin. Herhangi bir özel hata iletileri veya sağlanan hata kodlarını unutmayın.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnekte adlı VM durdurur `myVM` kaynak grubundan adlı `myResourceGroup`:

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

VM sonraki adıma işlemeden önce silme işlemlerinin tamamlanmasını bekleyin.


## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>Sanal makinenin işletim sistemi diskinden anlık görüntü oluşturma

Aşağıdaki örnek, ada sahip bir anlık görüntü oluşturur `mySnapshot` OS diski sanal makinenin 'myVM' adlı. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```

Anlık görüntü, bir VHD, tam ve salt okunur bir kopyasıdır. Bir VM'ye bağlı olamaz. Sonraki adımda, bu anlık görüntüden disk oluşturacağız.

## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu betik bir yönetilen disk adla oluşturur `newOSDisk` adlı anlık görüntüden `mysnapshot`.  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
$storageType = 'StandardLRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
Artık özgün işletim sistemi diskinin bir kopyasını var. Sorun giderme amacıyla başka bir Windows VM için bu diski bağlayabilir.

## <a name="attach-the-disk-to-another-windows-vm-for-troubleshooting"></a>Sorun giderme için başka bir Windows VM'ye disk ekleme

Şimdi biz özgün işletim sistemi diskinin bir kopyasını bir VM'ye veri diski olarak ekleyin. Bu işlem, yapılandırma hataları düzeltin veya ek uygulama veya sistem günlük dosyalarında disk gözden geçirmek sağlar. Aşağıdaki örnekte adlı disk ekler `newOSDisk` adlı sanal makineye `RecoveryVM`.

> [!NOTE]
> Diski eklemek için özgün işletim sistemi diski ve VM kurtarma kopyasını aynı konumda olmalıdır.

```powershell
$rgName = "myResourceGroup"
$vmName = "RecoveryVM"
$location = "eastus" 
$dataDiskName = "newOSDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="connect-to-the-recovery-vm-and-fix-issues-on-the-attached-disk"></a>Kurtarma sanal Makinesine bağlanın ve ekli diskte sorunları giderin

1. RDP için uygun kimlik bilgilerini kullanarak VM kurtarma. Aşağıdaki örnekte adlı VM için RDP bağlantı dosyası karşıdan `RecoveryVM` adlı kaynak grubunda `myResourceGroup`ve kendisine indirir `C:\Users\ops\Documents`"

    ```powershell
    Get-AzRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "RecoveryVM" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Veri diski otomatik olarak algılandı ve bağlı. Sürücü harfini şu şekilde belirlemek için bağlı birimlerin listesini görüntüleyin:

    ```powershell
    Get-Disk
    ```

    Aşağıdaki örnek çıktıda, disk bağlı bir disk gösterir **2**. (Ayrıca `Get-Volume` sürücü harfini görüntülemek için):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        newOSDisk                                  Healthy             Online       127 GB MBR
    ```

Özgün işletim sistemi diskinin bir kopyasını bağlandıktan sonra herhangi bir bakım ve sorun giderme adımlarını gereken şekilde gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-original-os-disk"></a>Çıkarın ve özgün işletim sistemi diski ayırma
Hataları çözümlendikten sonra çıkarın ve mevcut disk ayırma, Kurtarma VM. Disk kurtarma sanal Makinesine ekleniyor kira ilişkisini sonlandırana kadar diskinizi başka bir VM ile kullanamazsınız.

1. Gelen RDP oturumu içinde kurtarma sanal makinesinde veri diski çıkarın. Önceki disk numarası ihtiyacınız `Get-Disk` cmdlet'i. Ardından, `Set-Disk` disk çevrimdışı olarak ayarlamak için:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Disk, çevrimdışı kullanılarak olarak artık ayarlanır onaylayın `Get-Disk` yeniden. Disk artık çevrimdışı olarak ayarlıdır aşağıdaki örnek çıktı gösterilmektedir:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. RDP oturumundan çıkın. Adlı disk, Azure PowerShell oturumunuzda kaldırmak `newOSDisk` 'RecoveryVM' adlı VM'den.

    ```powershell
    $myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "RecoveryVM"
    Remove-AzVMDataDisk -VM $myVM -Name "newOSDisk"
    Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen sanal makine için işletim sistemi diskini değiştirme

İşletim sistemi diskleri takas etmek için Azure PowerShell kullanabilirsiniz. VM'yi silip yeniden yükleme gerekmez.

Bu örnek adlı bir sanal makine durdurur `myVM` ve adlı disk atar `newOSDisk` yeni işletim sistemi diski olarak. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="verify-and-enable-boot-diagnostics"></a>Doğrulayın ve önyükleme tanılamasını etkinleştirme

Aşağıdaki örnekte adlı VM'de tanılama uzantısını etkinleştirir `myVMDeployed` adlı kaynak grubunda `myResourceGroup`:

```powershell
$myVM = Get-AzVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [Azure VM'ye RDP sorunlarını giderme bağlantıları](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Windows sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Resource Manager kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).
