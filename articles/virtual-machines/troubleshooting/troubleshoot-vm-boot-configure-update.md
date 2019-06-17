---
title: VM başlatma takılı "alma Windows üzerinde hazır. Azure'da, bilgisayarınızı kapatmayın"| Microsoft Docs
description: VM başlatma "alma Windows üzerinde hazır. durumunda sorun giderme adımlarını Ekle Bilgisayarınızı kapatmayın.” iletisinde takılıyor
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: willchen
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: c3592529d20680c6920e569887effee4ffe38344
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64684000"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>VM başlatma takılı "alma Windows üzerinde hazır. Azure'da, bilgisayarınızı kapatmayın"

Bu makalede, sanal makine (VM) takıldığında sorunu gidermenize yardımcı olur. üzerinde "alma Windows hazır. Bilgisayarınızı kapatmayın"başlangıç aşamasında.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="symptoms"></a>Belirtiler

Kullanırken **önyükleme tanılaması** bir VM görüntüsü almak için işletim sistemini tam olarak başlatılması değil. VM "alma Windows hazır. ileti görüntüler. Bilgisayarınızı kapatmayın.” iletisinde takılıyor

![Windows Server 2012 R2 için ileti örneği](./media/troubleshoot-vm-configure-update-boot/message1.png)

![İleti örneği](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Nedeni

Yapılandırmanın değiştirilmesinden sonra sunucunun son başlatma işlemi yaparken, genellikle bu sorun oluşur. Yapılandırma değişikliğinin Windows güncelleştirmeleri veya sunucunun rol/özellik değişiklikleri başlatılması. Güncelleştirmeleri boyutu büyük olduğunda Windows güncelleştirmesi değişiklikleri yeniden yapılandırmak için daha fazla zaman işletim sistemi gerekir.

## <a name="back-up-the-os-disk"></a>İşletim sistemi diski yedeklemeniz

Bu sorunu düzeltmek denemeden önce işletim sistemi diski yedekleyin.

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Şifrelenmiş bir diski olan VM'ler için diskler önce kilidini açmanız gerekir

VM şifreli bir VM olup olmadığını belirlemek için aşağıdaki adımları izleyin.

1. Azure portalında VM'nizi açın ve ardından diskleri göz atın.

2. Bakmak **şifreleme** sütun şifreleme etkin olup olmadığını görmek için.

İşletim sistemi diski şifreli değilse şifrelenmiş diski kilidini açın. Diskin kilidini açmak için aşağıdaki adımları izleyin.

1. Aynı kaynak grubu, depolama hesabı ve konum etkilenen VM bulunan bir kurtarma VM oluşturun.

2. Azure portalında, etkilenen VM'yi silin ve disk tutun.

3. PowerShell'i yönetici olarak çalıştırın.

4. Gizli dizi adı almak için aşağıdaki cmdlet'i çalıştırın.

    ```Powershell
    Login-AzAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Gizli dizi adı oluşturduktan sonra PowerShell'de aşağıdaki komutları çalıştırın.

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Base64 ile kodlanmış değer bayta dönüştürün ve çıktıyı bir dosyaya yazın. 

    > [!Note]
    > USB kullanırsanız seçeneği kilidini, özgün BEK GUID BEK dosya adıyla eşleşmelidir. Bu adımları gerçekleştirmeden önce "BEK" adlı C sürücüsünde bir klasör oluşturun.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. BEK dosyayı bilgisayarınıza oluşturulduktan sonra bağlı kilitli işletim sistemi diski sahip VM kurtarma dosyasını kopyalayın. BEK dosya konumu kullanarak aşağıdaki komutları çalıştırın.

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **İsteğe bağlı**: Bazı senaryolarda bu komutu kullanarak disk şifresini çözmek gerekli olabilir.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Önceki komutun şifrelemek için disk harfi f olduğunu varsayar

8. Günlükleri toplamak istiyorsanız, yolu Git **sürücü HARFİ: \Windows\System32\winevt\Logs**.

9. Sürücünün kurtarma makineden çıkarın.

### <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Anlık görüntü oluşturmak için adımları izleyin. [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Bir işletim sistemi bellek dökümünü Topla

İçindeki adımları kullanın [toplama işletim sistemi döküm](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) VM yapılandırmasını takıldığında işletim sistemi döküm toplamak için bölüm.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurun

Bilgi döküm dosyası topladıktan sonra başvurun [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorunun temel nedenini analiz etmek için.


## <a name="rebuild-the-vm-by-using-powershell"></a>PowerShell kullanarak sanal Makineyi yeniden oluşturun

Bellek dökümü dosyasına topladıktan sonra VM'yi yeniden oluşturmak için aşağıdaki adımları izleyin.

**Yönetilmeyen diskler için**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID “SubscriptionID” | Select-AzSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Yönetilen diskler için**

```powershell
# To log in to Azure Resource Manager
Login-AzAccount

# To view all subscriptions for your account
Get-AzSubscription

# To select a default subscription for your current session
Get-AzSubscription –SubscriptionID "SubscriptionID" | Select-AzSubscription

#Fill in all variables
$subid = "SubscriptionID"
$rgName = "ResourceGroupName";
$loc = "Location";
$vmSize = "VmSize";
$vmName = "VmName";
$nic1Name = "FirstNetworkInterfaceName";
#$nic2Name = "SecondNetworkInterfaceName";
$avName = "AvailabilitySetName";
$osDiskName = "OsDiskName";
$DataDiskName = "DataDiskName"

#This can be found by selecting the Managed Disks you wish you use in the Azure portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
