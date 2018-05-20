---
title: Bir Windows Azure PowerShell ile VM sorun giderme kullanın | Microsoft Docs
description: Kurtarma Azure PowerShell kullanarak bir VM için işletim sistemi diski bağlanarak Azure Windows VM sorunlarını giderme hakkında bilgi edinin
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
ms.openlocfilehash: 408429d0f8697b8b807e386dbcf2eade29938249
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Bir Windows VM kurtarma Azure PowerShell kullanarak bir VM için işletim sistemi diski ekleyerek sorun giderme
Azure, Windows sanal makine (VM) önyükleme veya disk bir hatayla karşılaştığında, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek VM başarıyla önyükleme engeller başarısız uygulama güncelleştirmesi olacaktır. Bu makale Azure PowerShell, sanal sabit diski başka bir Windows hataları düzeltin, sonra özgün VM'yi yeniden oluşturmak için VM'e bağlanmak için nasıl kullanılacağını ayrıntılarını verir.


## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sorunlarla, sanal sabit diskleri tutmak VM silin.
2. Ekleme ve sorun giderme amacıyla başka bir Windows VM için sanal sabit diski bağlama.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunlarını gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Özgün sanal sabit disk kullanan bir VM oluşturun.

Yönetilen disk kullanan VM için bkz: [yeni bir işletim sistemi diski ekleyerek bir yönetilen Disk VM sorun giderme](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).


Bilgisayarınızda yüklü olduğundan emin olun [en son Azure PowerShell](/powershell/azure/overview) yüklü ve aboneliğinize oturum:

```powershell
Connect-AzureRmAccount
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.


## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Önyükleme sorunlarını gidermenize yardımcı olması için bir ekran görüntüsü, VM görüntüleyebilirsiniz. Bu ekran, önyükleme bir VM başarısız neden tanımlamaya yardımcı olabilir. Aşağıdaki örnek ekran görüntüsünde Windows adlı VM'den alır `myVM` kaynak grubunda adlı `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

VM önyükleme neden başarısız olduğunu belirlemek için ekran görüntüsünü gözden geçirin. Herhangi bir özel hata iletileri veya sağlanan hata kodları unutmayın.


## <a name="view-existing-virtual-hard-disk-details"></a>Var olan sanal sabit disk ayrıntılarını görüntüleyin
Sanal sabit disk için başka bir VM iliştirebilirsiniz önce sanal sabit disk (VHD) adını tanımlamak gerekir.

Aşağıdaki örnek adlı VM için bilgilerini alır `myVM` kaynak grubunda adlı `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Ara `Vhd URI` içinde `StorageProfile` önceki komutun çıktısından bölümü. Aşağıdaki örnek çıktısında kesilmiş `Vhd URI` kod bloğunu sonuna:

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


## <a name="delete-existing-vm"></a>Var olan VM silme
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı ' dir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynakları başvuran meta verilerdir. Her sanal sabit disk için bir VM eklendiğinde atanmış bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Bu VM durduruldu ve deallocated durumda olduğunda bile işletim sistemi diski bir VM ile ilişkilendirilecek kira sürdürür.

VM kurtarmak için ilk adım, VM kaynak silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir VM'e ekleyin.

Aşağıdaki örnek adlı VM siler `myVM` kaynak grubundan adlı `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

VM için başka bir VM sanal sabit disk eklemeden önce silme tamamlanana kadar bekleyin. VM ile ilişkilendirir sanal sabit disk üzerindeki kira süresini sanal sabit disk için başka bir VM eklemeden önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Varolan bir sanal sabit diski başka bir VM'e ekleyin
Sonraki birkaç adımda için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için sorun giderme bu VM'e ekleyin. Bu işlem, yapılandırma hataları düzeltin ya da ek uygulama veya sistem günlük dosyalarını, örneğin gözden olanak sağlar. Seçin veya sorun giderme amacıyla kullanmak için başka bir VM oluşturun.

Varolan bir sanal sabit diski taktığınızda, önceki elde disk URL'yi belirtmek `Get-AzureRmVM` komutu. Aşağıdaki örnek sorun giderme VM adlı varolan bir sanal sabit diski iliştirir `myVMRecovery` kaynak grubunda adlı `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Disk ekleme diskin boyutunu belirtmenizi gerektirir. Biz varolan bir diski ilişti gibi `-DiskSizeInGB` olarak belirtilen `$null`. Bu değer veri diski düzgün bağlı sağlar ve veri diski true boyutunu belirlemek için gerek kalmadan.


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski bağlama

1. Uygun kimlik bilgilerini kullanarak sorun giderme, VM için RDP. Aşağıdaki örnek adlı VM için RDP bağlantı dosyası karşıdan `myVMRecovery` kaynak grubunda adlı `myResourceGroup`ve kendisine indirir `C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Veri diski otomatik olarak algılanır ve bağlı. Sürücü harfi şu şekilde belirlemek için bağlı birimlerin listesini görüntüleyin:

    ```powershell
    Get-Disk
    ```

    Aşağıdaki örnek çıkış sanal sabit diske bağlı bir disk gösterir **2**. (Aynı zamanda `Get-Volume` sürücü harfi görüntülemek için):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskinde ilgili sorunları giderme
Varolan bir sanal sabit takılı diski ile tüm bakım ve sorun giderme adımları gereken şekilde artık gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk ayırma
Hatalarınızı çözüldükten sonra çıkarın ve varolan bir sanal sabit diski, sorun giderme sanal makineden ayırın. Sorun giderme VM için sanal sabit disk ekleme kira serbest kadar herhangi bir VM ile sanal sabit disk kullanamazsınız.

1. Gelen RDP oturumu içinde veri diski, Kurtarma VM çıkarın. Disk numarası önceki gereksinim `Get-Disk` cmdlet'i. Ardından, `Set-Disk` disk çevrimdışı olarak ayarlamak için:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Disk, çevrimdışı kullanılarak olarak şimdi ayarlanır onaylayın `Get-Disk` yeniden. Aşağıdaki örnek çıkış disk şimdi çevrimdışı olarak ayarlandı gösterir:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. RDP oturumunuzu kapatın. Azure PowerShell oturumunuzda, sorun giderme sanal makineden sanal sabit diski kaldırın.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Özgün sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanmak [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Gerçek JSON şablon aşağıdaki bağlantıda şöyledir:

- https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json

Önceki komutu VHD URL'yi kullanarak mevcut sanal ağda, bir VM şablonu dağıtır. Aşağıdaki örnek adlı kaynak grubunu şablon dağıtır `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

VM adı, işletim sistemi türü ve VM boyutu gibi şablonu için istemleri yanıtlayın. `osDiskVhdUri` Daha önce kullanıldığında aynı varolan bir sanal sabit diski sorun giderme VM'ye ekleniyor.


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin

Var olan sanal sabit diskten VM'nizi oluşturduğunuzda, önyükleme tanılaması otomatik olarak etkinleştirilmemiş olabilir. Aşağıdaki örnek adlı VM'de tanılama uzantısını etkinleştirir `myVMDeployed` kaynak grubunda adlı `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Yeni bir işletim sistemi diski ekleyerek bir yönetilen Disk VM sorun giderme
1. Etkilenen yönetilen Disk Windows VM durdurun.
2. [Yönetilen disk anlık görüntü](snapshot-copy-managed-disk.md) yönetilen Disk VM işletim sistemi disk.
3. [Yönetilen bir disk anlık görüntüden oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [Yönetilen diski VM bir veri diski Ekle](attach-disk-ps.md).
5. [Veri diski 4. adımdan işletim sistemi diski değiştirmek](os-disk-swap.md).

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, bkz: [bir Azure VM için RDP sorun giderme bağlantılarını](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). VM üzerinde çalışan uygulamalara erişim sorunları için bkz: [Windows VM üzerinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
