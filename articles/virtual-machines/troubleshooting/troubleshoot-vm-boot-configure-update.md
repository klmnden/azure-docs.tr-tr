---
title: VM başlatma işlemi “Windows Hazırlanıyor. Bilgisayarınızı kapatmayın.” iletisinde takılıyor azure'da | Microsoft Docs
description: Hangi VM başlatma durumlar sorun giderme adımlarını tanıtmak üzerinde "alma Windows hazır. Bilgisayarınızı kapatmayın".
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
ms.openlocfilehash: 2bcdb2b458327a5e87dc36e4a5f50f0ac46bf96a
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621046"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>VM başlatma işlemi “Windows Hazırlanıyor. Bilgisayarınızı kapatmayın.” iletisinde takılıyor Azure'da

Bu makalede, sanal makine (VM) takıldığında sorunu gidermenize yardımcı olur. üzerinde "alma Windows hazır. Bilgisayarınızı kapatmayın.” iletisinde takılıyor başlangıç aşamasında.

## <a name="symptoms"></a>Belirtiler

Kullanırken **önyükleme tanılaması** bir VM görüntüsü almak için işletim sistemini tam olarak başlatılamadığı bulun. Ayrıca, VM görüntüleyen bir **"alma Windows hazır. Bilgisayarınızı kapatmayın."** İleti.

![İleti örneği](./media/troubleshoot-vm-configure-update-boot/message1.png)

![İleti örneği](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Nedeni

Yapılandırmanın değiştirilmesinden sonra sunucunun son başlatma işlemi yaparken, genellikle bu sorun oluşur. Yapılandırma değişikliğinin Windows güncelleştirmeleri veya sunucunun rol/özellik değişiklikleri tarafından başlatılabiliyordu. Güncelleştirmeleri boyutu büyük olduğunda, Windows güncelleştirmesi değişiklikleri yeniden yapılandırmak için daha fazla zaman işletim sistemi gerekir.

## <a name="back-up-the-os-disk"></a>İşletim sistemi diski yedeklemeniz

Bu sorunu düzeltmek denemeden önce işletim sistemi diski yedekleyin:

### <a name="for-vms-with-an-encrypted-disk-you-must-unlock-the-disks-first"></a>Şifrelenmiş bir diski olan VM'ler için diskler önce kilidini açmanız gerekir

VM şifreli bir VM ise doğrulayın. Bunu yapmak için şu adımları uygulayın:

1. Portal, sanal makinenizin açın ve ardından diskleri göz atın.

2. "Şifreleme etkin olup olmadığını size bildirir, şifreleme," çağrısı bir sütun görürsünüz.

İşletim sistemi diski şifreli değilse şifrelenmiş diski kilidini açın. Bunu yapmak için şu adımları uygulayın:

1. Aynı kaynak grubu, depolama hesabı ve konum etkilenen VM bulunan bir kurtarma VM oluşturun.

2. Azure portalında etkilenen VM'yi silin ve disk tutun.

3. PowerShell'i yönetici olarak çalıştırın.

4. Gizli dizi adı almak için aşağıdaki cmdlet'i çalıştırın.

    ```Powershell
    Login-AzureRmAccount
 
    $vmName = “VirtualMachineName”
    $vault = “AzureKeyVaultName”
 
    # Get the Secret for the C drive from Azure Key Vault
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq $vmName) -and ($_.Tags.VolumeLetter -eq “C:\”) -and ($_.ContentType -eq ‘BEK‘)}

    # OR Use the below command to get BEK keys for all the Volumes
    Get-AzureKeyVaultSecret -VaultName $vault | where {($_.Tags.MachineName -eq   $vmName) -and ($_.ContentType -eq ‘BEK’)}
    ```

5. Gizli dizi adı oluşturduktan sonra PowerShell'de aşağıdaki komutları çalıştırın:

    ```Powershell
    $secretName = 'SecretName'
    $keyVaultSecret = Get-AzureKeyVaultSecret -VaultName $vault -Name $secretname
    $bekSecretBase64 = $keyVaultSecret.SecretValueText
    ```

6. Base64 kodlu bayta dönüştürün ve çıktıyı bir dosyaya yazın. 

    > [!Note]
    > Özgün BEK dosya adı eşleşmelidir BEK USB kullanırsanız GUID kilidini açma seçeneği. Ayrıca, C sürücünüzde aşağıdaki adımları çalışmadan önce "BEK" adlı bir klasör oluşturmanız gerekir.
    
    ```Powershell
    New-Item -ItemType directory -Path C:\BEK
    $bekFileBytes = [Convert]::FromBase64String($bekSecretbase64)
    $path = “c:\BEK\$secretName.BEK”
    [System.IO.File]::WriteAllBytes($path,$bekFileBytes)
    ```

7. BEK dosyayı bilgisayarınıza oluşturulduktan sonra bağlı kilitli işletim sistemi diski sahip VM kurtarma dosyasını kopyalayın. Aşağıdaki komutu çalıştırın BEK kullanarak dosya konumu:

    ```Powershell
    manage-bde -status F:
    manage-bde -unlock F: -rk C:\BEKFILENAME.BEK
    ```
    **İsteğe bağlı** bazı senaryolarda bu komutla disk de şifre çözme gerekli olabilir.
   
    ```Powershell
    manage-bde -off F:
    ```

    > [!Note]
    > Bu, disk şifreleme harfi F: olduğunu dikkate almaktır.

8. Günlükleri toplamak istiyorsanız, yolu gidebilirsiniz **sürücü HARFİ: \Windows\System32\winevt\Logs**.

9. Sürücünün kurtarma makineden çıkarın.

### <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Anlık görüntü oluşturmak için adımları izleyin. [bir diskin anlık görüntüsünü alma](..\windows\snapshot-copy-managed-disk.md).

## <a name="collect-an-os-memory-dump"></a>Bir işletim sistemi bellek dökümünü Topla

İçindeki adımları kullanın [toplama işletim sistemi döküm](troubleshoot-common-blue-screen-error.md#collect-memory-dump-file) VM yapılandırmasını takıldığında işletim sistemi döküm toplamak için bölüm.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurun

Bilgi döküm dosyası topladıktan sonra başvurun [Microsoft Destek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) kök neden analizi için.


## <a name="rebuild-the-vm-using-powershell"></a>PowerShell kullanarak VM yeniden oluşturun

Bellek dökümü dosyasına topladıktan sonra VM'yi yeniden oluşturmak için aşağıdaki adımları kullanın.

**Yönetilmeyen diskler için**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID “SubscriptionID” | Select-AzureRmSubscription

$rgname = "RGname"
$loc = "Location"
$vmsize = "VmSize"
$vmname = "VmName"
$vm = New-AzureRmVMConfig -VMName $vmname -VMSize $vmsize;

$nic = Get-AzureRmNetworkInterface -Name ("NicName") -ResourceGroupName $rgname;
$nicId = $nic.Id;

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId;

$osDiskName = "OSdiskName"
$osDiskVhdUri = "OSdiskURI"

$vm = Set-AzureRmVMOSDisk -VM $vm -VhdUri $osDiskVhdUri -name $osDiskName -CreateOption attach -Windows

New-AzureRmVM -ResourceGroupName $rgname -Location $loc -VM $vm -Verbose
```

**Yönetilen diskler için**

```PowerShell
# To login to Azure Resource Manager
Login-AzureRmAccount

# To view all subscriptions for your account
Get-AzureRmSubscription

# To select a default subscription for your current session
Get-AzureRmSubscription –SubscriptionID "SubscriptionID" | Select-AzureRmSubscription

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

#This can be found by selecting the Managed Disks you wish you use in the Azure Portal if the format below doesn't match
$osDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$osDiskName";
$dataDiskResourceId = "/subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/disks/$DataDiskName";

$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize;

#Uncomment to add Availability Set
#$avSet = Get-AzureRmAvailabilitySet –Name $avName –ResourceGroupName $rgName;
#$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id;

#Get NIC Resource Id and add
$nic1 = Get-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $rgName;
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic1.Id -Primary;

#Uncomment to add a secondary NIC
#$nic2 = Get-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $rgName;
#$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic2.Id;

#Windows VM
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Windows;

#Linux VM
#$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDiskResourceId -name $osDiskName -CreateOption Attach -Linux;

#Uncomment to add additional Data Disk
#Add-AzureRmVMDataDisk -VM $vm -ManagedDiskId $dataDiskResourceId -Name $dataDiskName -Caching None -DiskSizeInGB 1024 -Lun 0 -CreateOption Attach;

New-AzureRmVM -ResourceGroupName $rgName -Location $loc -VM $vm;
```
