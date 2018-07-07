---
title: Bir Windows Azure PowerShell ile sanal makine sorunlarını giderme kullanın | Microsoft Docs
description: İşletim sistemi diskini bir kurtarma için Azure PowerShell kullanarak VM bağlanarak azure'da Windows VM sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 1e87704e7d8cf3c7cc21e537d36f95a97265061b
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903525"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>İşletim sistemi diskini bir kurtarma için Azure PowerShell kullanarak VM ekleyerek bir Windows sanal makinesinin sorunlarını giderme
Azure'da Windows sanal makinesi (VM), önyükleme veya disk bir hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın olarak karşılaşılan örneklerden VM başarıyla önyükleme engelleyen bir uygulamanın güncelleştirme olacaktır. Bu makalede, sanal sabit diskinizi başka bir Windows varsa hataları düzeltin ve ardından orijinal VM'yi yeniden oluşturmak için VM'ye bağlanmak için Azure PowerShell kullanma işlemi açıklanmaktadır.


## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sanal sabit diskleri tutmak, sorun yaşayan VM'yi silin.
2. Ekleme ve sorun giderme amacıyla başka bir Windows VM için sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunları gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Orijinal sanal sabit diski kullanarak bir VM oluşturun.

Yönetilen disk kullanan bir VM için bkz: [yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).


Sahip olduğunuzdan emin olun [en son Azure PowerShell](/powershell/azure/overview) yüklü ve aboneliğinize oturum:

```powershell
Connect-AzureRmAccount
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.


## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Azure'da önyükleme sorunlarını gidermenize yardımcı olması için VM görüntüsü görüntüleyebilirsiniz. Bu ekran görüntüsünde, bir VM önyükleme başaramıyor neden belirlemenize yardımcı olabilir. Aşağıdaki örnek ekran görüntüsünde Windows adlı VM'den alır `myVM` adlı kaynak grubunda `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Sanal Makineyi önyüklemek neden başarısız olduğunu belirlemek için ekran gözden geçirin. Herhangi bir özel hata iletileri veya sağlanan hata kodlarını unutmayın.


## <a name="view-existing-virtual-hard-disk-details"></a>Mevcut sanal sabit disk ayrıntıları görüntüleyin
Sanal sabit diskinizi başka bir sanal makineye iliştirebilmek için önce sanal sabit disk (VHD) adını tanımlamak gerekir.

Aşağıdaki örnekte adlı VM'nin bilgileri alır `myVM` adlı kaynak grubunda `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Aranacak `Vhd URI` içinde `StorageProfile` önceki komutun çıktısından bölümü. Aşağıdaki örnek çıktıda gösterildiği kesilmiş `Vhd URI` kod bloğunun sonuna doğru:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Mevcut VM'yi silin
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı yerdir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynaklara başvurur meta verilerdir. Her sanal sabit disk bir VM'ye atanan bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kira, VM durdurulmuş ve serbest bırakılmış durumda olsa bile işletim sistemi diski ile bir VM ile ilişkisini sürdürür.

VM'nizi kurtarmanın ilk adımı, VM kaynağını silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir sanal makineye ekleyin.

Aşağıdaki örnekte adlı sanal makine silinir `myVM` kaynak grubundan adlı `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

VM sanal sabit diski başka bir sanal makineye eklemeden önce silme işlemlerinin tamamlanmasını bekleyin. Kira VM ile ilişkilendiren sanal sabit diski sanal sabit diski başka bir sanal makineye iliştirebilmek için önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Mevcut sanal sabit diski başka bir VM'ye
Sonraki birkaç adımı için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için bu sorun giderme sanal makinesine ekleyebilir. Bu işlem, yapılandırma hataları düzeltin veya ek uygulama veya sistem günlük dosyalarını, örneğin gözden geçirmek sağlar. Seçin veya sorun giderme amacıyla kullanmak üzere başka bir VM oluşturun.

Mevcut sanal sabit disk eklediğinizde, önceki elde disk URL'sini belirtin `Get-AzureRmVM` komutu. Aşağıdaki örnekte mevcut bir sanal sabit disk adlı sorun giderme vm'siyle `myVMRecovery` adlı kaynak grubunda `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Disk ekleme, diskin boyutunu belirtmek gerektirir. Biz varolan bir disk eklemek gibi `-DiskSizeInGB` olarak belirtilen `$null`. Veri diski doğru bir şekilde bağlı bu değeri sağlar ve gerçek veri diski boyutunu belirlemek zorunda kalmadan.


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski takma

1. Uygun kimlik bilgilerini kullanarak sorun giderme sanal makinenize yönelik RDP. Aşağıdaki örnekte adlı VM için RDP bağlantı dosyası karşıdan `myVMRecovery` adlı kaynak grubunda `myResourceGroup`ve kendisine indirir `C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Veri diski otomatik olarak algılandı ve bağlı. Sürücü harfini şu şekilde belirlemek için bağlı birimlerin listesini görüntüleyin:

    ```powershell
    Get-Disk
    ```

    Aşağıdaki örnek çıktı, sanal sabit disk bağlı bir disk gösterir **2**. (Ayrıca `Get-Volume` sürücü harfini görüntülemek için):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları düzeltme
Mevcut sanal sabit bağlı disk ile artık tüm bakım ve sorun giderme adımlarını gereken şekilde gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk detach
Hataları çözümlendikten sonra çıkarın ve mevcut sanal sabit diski, sorun giderme sanal makinesinden ayrılamadı. Sorun giderme sanal makinesine sanal sabit disk ekleme kira ilişkisini sonlandırana kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

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

2. RDP oturumundan çıkın. Azure PowerShell oturumunuzda, sanal sabit diski sorun giderme sanal makineden kaldırın.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Orijinal sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanın [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Gerçek JSON şablonunu aşağıdaki bağlantıda verilmiştir:

- https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json

Şablonun, önceki komuttan VHD URL'sini kullanarak mevcut bir sanal ağına bir VM dağıtır. Aşağıdaki örnek şablonu adlı bir kaynak grubuna dağıtır `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

VM adı, işletim sistemi türü ve VM boyutu gibi şablonun istemlerini yanıtlayın. `osDiskVhdUri` Aynı daha önce kullanıldığında varolan bir sanal sabit diski sorun giderme sanal makinesine ekleniyor.


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin

Mevcut sanal sabit diskten VM oluşturma, önyükleme tanılamaları otomatik olarak etkinleştirilmemiş olabilir. Aşağıdaki örnekte adlı VM'de tanılama uzantısını etkinleştirir `myVMDeployed` adlı kaynak grubunda `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme
1. Etkilenen yönetilen Disk Windows VM'yi durdurun.
2. [Yönetilen disk anlık görüntü oluşturma](snapshot-copy-managed-disk.md) yönetilen Disk sanal makinenin işletim sistemi diskinin.
3. [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [VM veri diski olarak yönetilen diski](attach-disk-ps.md).
5. [Veri diski 4. adımdan işletim sistemi diskini değiştirmek](os-disk-swap.md).

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [Azure VM'ye RDP sorunlarını giderme bağlantıları](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Windows sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Resource Manager kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
