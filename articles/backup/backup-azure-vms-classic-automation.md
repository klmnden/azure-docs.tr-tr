---
title: "Dağıtma ve PowerShell kullanarak Azure VM'ler için yedekleme yönetme | Microsoft Docs"
description: "Dağıtma ve Azure PowerShell kullanarak yedekleme yönetme hakkında bilgi edinin."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/9/2017
ms.author: cwatson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 379686f70518b3c7773f1e0e0461eda2a4934d64
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="use-azurermbackup-cmdlets-to-back-up-virtual-machines"></a>Sanal makineleri yedeklemek için AzureRM.Backup cmdlet'leri kullanın
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klasik](backup-azure-vms-classic-automation.md)
>
>

Bu makalede Azure PowerShell'in yedekleme ve kurtarma işlemlerini Azure Vm'leri için nasıl kullanılacağı gösterilmektedir. Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır: Resource Manager ve Klasik. Bu makalede, bir yedekleme Kasası'na verileri yedeklemek için Klasik dağıtım modeli kullanarak kapsar. Aboneliğinizde bir yedekleme kasası oluşturmadıysanız, bu makalede, Resource Manager sürümüne bakın [sanal makineleri yedeklemek için kullanım AzureRM.RecoveryServices.Backup cmdlet'leri](backup-azure-vms-automation.md). Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 30 Kasım 2017 sonra yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz.<br/> **30 Kasım 2017 tarafından**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="concepts"></a>Kavramlar
Bu makale, sanal makineleri yedeklemek için kullanılan PowerShell cmdlet'lerini özgü bilgileri sağlar. Azure sanal makinelerini koruma hakkında giriş bilgileri için lütfen bkz [azure'da VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md).

> [!NOTE]
> Başlamadan önce okuyun [Önkoşullar](backup-azure-vms-prepare.md) Azure yedekleme ile çalışmak için gereken ve [sınırlamalar](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) geçerli VM yedekleme çözümü.
>
>

PowerShell etkili bir şekilde kullanmak için nesnelerin ve başlangıç nereden hiyerarşisini anlamak için bir dakikanızı ayırın.

![Nesne hiyerarşisi](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

İki en önemli akışı VM korumasını etkinleştirmek ve bir kurtarma noktasından geri yüklenmesi. Bu iki senaryoları etkinleştirmek için PowerShell cmdlet'leri ile çalışma sırasında adept hale yardımcı olması için bu makalenin odak noktasıdır.

## <a name="setup-and-registration"></a>Kurulumu'nu ve kaydı
Başlamak için:

1. [En son PowerShell indirme](https://github.com/Azure/azure-powershell/releases) (gerekli minimum sürüm: 1.0.0)
2. Azure yedekleme PowerShell cmdlet'leri kullanılabilir, aşağıdaki komutu yazarak bulabilirsiniz:

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

Aşağıdaki kurulumunu ve kaydını görevleri PowerShell ile otomatik olarak yapılabilir:

* Yedekleme kasası oluşturma
* Vm'leri Azure Backup hizmeti ile kaydetme

### <a name="create-a-backup-vault"></a>Yedekleme kasası oluşturma
> [!WARNING]
> İlk olarak Azure Yedekleme'yi kullanarak müşteriler için aboneliğiniz ile birlikte kullanılacak Azure Backup sağlayıcı kaydetmeniz gerekir. Bu aşağıdaki komutu çalıştırarak yapılabilir: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"
>
>

Kullanarak yeni bir yedekleme kasası oluşturma **yeni AzureRmBackupVault** cmdlet'i. Yedekleme kasası bir ARM kaynak olduğundan, bir kaynak grubu içindeki yerleştirmeniz gerekir. Yükseltilmiş bir Azure PowerShell konsolunda aşağıdaki komutları çalıştırın:

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

Belirtilen abonelik kullanarak tüm yedekleme kasalarının listesi elde edebilirsiniz **Get-AzureRmBackupVault** cmdlet'i.

> [!NOTE]
> Yedekleme kasası nesneyi bir değişkene depolamak uygundur. Kasa nesne birçok Azure yedekleme cmdlet'lerini için bir giriş olarak gereklidir.
>
>

### <a name="registering-the-vms"></a>Sanal makineleri kaydetme
Yedekleme Azure yedekleme ile yapılandırma doğru ilk makine ya da VM Azure yedek kasası ile kaydetmek için adımdır. **Register-AzureRmBackupContainer** cmdlet'i bir Azure Iaas sanal makinenin giriş bilgilerini alır ve belirtilen kasa ile kaydeder. Kayıt işlemi, Azure sanal makine yedekleme kasası ile ilişkilendirir ve yedekleme döngüsü boyunca VM izler.

Azure Backup hizmeti ile VM kaydetme bir üst düzey kapsayıcısı nesnesi oluşturur. Bir kapsayıcı genellikle yedeklenebilir birden çok öğe içeriyor, ancak söz konusu olduğunda VM'ler olacaktır kapsayıcısı için yalnızca bir yedekleme öğesi.

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a>Azure sanal makinelerini yedekleme
### <a name="create-a-protection-policy"></a>Bir koruma ilkesi oluşturun
Bu, Vm'leri yedekleme başlatmak için yeni bir koruma ilkesi oluşturmak için zorunlu değildir. Bir 'varsayılan hızlı bir şekilde korumasını etkinleştirmek için kullanılan ve daha sonra sağ ayrıntılarla düzenlenebilir ilke' kasası gelir. Kasaya kullanılabilir ilkelerinin bir listesini kullanarak alabileceğiniz **Get-AzureRmBackupProtectionPolicy** cmdlet:

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> PowerShell BackupTime alanın saat dilimi UTC değil. Ancak, Azure portalında yedekleme saati gösterildiğinde UTC uzaklığı yanı sıra yerel sistem saat dilimi hizalanır.
>
>

Bir yedekleme İlkesi, en az bir bekletme ilkesi ile ilişkilidir. Bekletme İlkesi ne kadar bir kurtarma noktası Azure yedekleme ile tutulan tanımlar. **Yeni AzureRmBackupRetentionPolicy** cmdlet bekletme ilkesi bilgileri tutun PowerShell nesneler oluşturur. Giriş olarak kullanılan bu Bekletme İlkesi nesneleri *yeni AzureRmBackupProtectionPolicy* cmdlet'ini veya doğrudan *etkinleştir AzureRmBackupProtection* cmdlet'i.

Bir yedekleme İlkesi zaman ve ne sıklıkta bir öğenin yedekleme yapıldıktan tanımlar. **Yeni AzureRmBackupProtectionPolicy** cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur. Yedekleme İlkesi, bir girdi olarak kullanılır *etkinleştir AzureRmBackupProtection* cmdlet'i.

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a>Korumayı etkinleştir
Koruma etkinleştirme işlemi iki nesne - öğesini ve ilke kapsar ve her ikisi de aynı kasaya ait olmaları gerekir. İlke öğeyle ilişkili silindikten sonra yedekleme iş akışı tanımlı zamanlamasında ilkenin etkisini gösterip.

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a>İlk yedekleme
Yedekleme zamanlaması öğesi için tam ilk kopyayı ve artımlı kopya için sonraki yedeklemeler yapma ilgilenebilmek. Ancak, ilk yedekleme durum belirli bir tarihte veya hatta hemen zorla istiyorsanız ardından kullanın **yedekleme AzureRmBackupItem** cmdlet:

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> StartTime ve EndTime alanlarının PowerShell'de gösterilen saat dilimi UTC değil. Ancak, benzer bilgiler Azure portalında gösterildiğinde, saat dilimi Yerel Sistem saatiniz hizalanır.
>
>

### <a name="monitoring-a-backup-job"></a>Bir yedekleme işi izleme
Azure Backup çoğu uzun süre çalışan işlemleri, bir iş olarak modelled. Bu, Azure portalında tutmak zorunda kalmadan ilerleme durumunu izlemek her zaman açık kolaylaştırır.

Devam eden işin en son durumunu almak için **Get-AzureRmBackupJob** cmdlet'i.

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

Bu işleri - gereksiz, ek kod olan - tamamlanması için yoklama yerine bunu kullanmak daha basittir **bekleme AzureRmBackupJob** cmdlet'i. Bir komut dosyası kullanıldığında, cmdlet işi tamamlar veya belirtilen zaman aşımı değerine ulaşılana kadar yürütme duraklatılır.

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a>Bir Azure VM geri yükleme
Yedekleme verileri geri yüklemek için yedeklenen öğesi ve zaman içinde nokta verilerini tutan bir kurtarma noktası tanımlamak gerekir. Bu bilgileri Kasası'ndan veri müşterinin hesabına geri yüklemesini başlatmak için geri yükleme-AzureRmBackupItem cmdlet'i için sağlanır.

### <a name="select-the-vm"></a>VM seçin
Doğru yedekleme öğesi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatmak ve yolunuzu nesne hiyerarşisi aşağı çalışması gerekir. VM'yi temsil kapsayıcı seçmek için kullanın **Get-AzureRmBackupContainer** cmdlet'i ve için kanal **Get-AzureRmBackupItem** cmdlet'i.

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin
Yedekleme öğesi kullanan tüm kurtarma noktalarını şimdi listeleyebilirsiniz **Get-AzureRmBackupRecoveryPoint** cmdlet'ini ve geri yüklemek için kurtarma noktası seçin. Genellikle kullanıcılar en son çekme *AppConsistent* noktası listesinde.

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

Değişkeni ```$rp``` kurtarma noktaları dizisidir seçili yedekleme öğesi, ters sırada süresini sıralanmış - en son kurtarma noktası dizini 0 için. Standart PowerShell dizi dizin kurtarma noktası seçmek için kullanın. Örneğin: ```$rp[0]``` en son kurtarma noktası seçin.

### <a name="restoring-disks"></a>Diskleri geri yükleme
Azure PowerShell ve Azure portal aracılığıyla yapılır geri yükleme işlemleri arasındaki en önemli fark yoktur. PowerShell ile diskleri ve yapılandırma bilgilerini kurtarma noktasından geri yükleme sırasında geri yükleme işlemi durdurur. Bir sanal makine oluşturmaz.

> [!WARNING]
> Geri yükleme AzureRmBackupItem VM oluşturmaz. Yalnızca belirtilen depolama hesabına diskleri yükler. Bu Azure portalında yaşar aynı davranış değildir.
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

Geri yükleme işlemini kullanarak ayrıntılarını alabilirsiniz **Get-AzureRmBackupJobDetails** geri yükleme işi tamamlandıktan sonra cmdlet'i. *ErrorDetails* özelliği VM yeniden oluşturmak için gereken bilgileri olacaktır.

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-the-vm"></a>VM oluşturma
Geri yüklenen diskleri dışında VM oluşturma eski Azure Service Management PowerShell cmdlet'leri, yeni Azure Resource Manager şablonları veya bile Azure portalını kullanarak yapılabilir. Hızlı bir örnekte Azure Hizmet Yönetimi cmdlet'leri kullanılarak nasıl gideceğinizi göstereceğiz.

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

Geri yüklenen disklerden VM oluşturma hakkında daha fazla bilgi için aşağıdaki cmdlet'leri hakkında okuyun:

* [Ekleme AzureDisk](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [AzureVMConfig yeni](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [Yeni-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a>Kod örnekleri
### <a name="1-get-the-completion-status-of-job-sub-tasks"></a>1. İş alt görevlerin tamamlanma durumunu Al
Tek tek alt görevlerin tamamlanma durumunu izlemek için kullanabileceğiniz **Get-AzureRmBackupJobDetails** cmdlet:

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data to Backup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a>2. Yedekleme işleri günlük/haftalık raporunu oluşturma
Yöneticiler genellikle ne yedekleme işleri son 24 saat içinde bu yedekleme işlerini durumunu çalışan bilmek ister. Ayrıca, aktarılan veri miktarını Yöneticiler aylık veri kullanımlarını tahmin etmek için bir yol sağlar. Aşağıdaki komut dosyası Azure Backup hizmeti ham veri çeker ve PowerShell konsolunda bilgileri görüntüler.

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -To $enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for the last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract the information for the reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

Bu rapor çıktısı grafik yetenekleri eklemek istiyorsanız, TechNet blog gönderisi öğrenin [PowerShell ile Grafikleme](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)

## <a name="next-steps"></a>Sonraki adımlar
PowerShell ile Azure kaynaklarınızı devreye kullanmayı tercih ederseniz, Windows Server, koruma için PowerShell makaleyi kullanıma [dağıtma ve yönetme yedekleme için Windows Server](backup-client-automation-classic.md). Ayrıca DPM yedeklemelerini yönetmek için PowerShell makale olduğu [dağıtma ve yönetme yedekleme DPM için](backup-dpm-automation-classic.md). Bu makaleler her ikisi de Resource Manager dağıtımları ve bunun yanı sıra klasik dağıtımlar için bir sürümüne sahipsiniz.
