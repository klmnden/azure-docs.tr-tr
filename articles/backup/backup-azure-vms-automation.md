---
title: PowerShell kullanılarak Resource Manager ile dağıtılmış VM’ler için yedekleme dağıtma ve yönetme
description: Resource Manager ile dağıtılmış VM'ler için azure'da yedeklemelerini yönetme ve dağıtma için PowerShell kullanma
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 8/06/2018
ms.author: markgal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: faa0908f9317c10713c41d06a22e9251998fe3c7
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44378457"
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>Sanal makineleri yedeklemek için AzureRM.RecoveryServices.Backup cmdlet'lerini kullanma

Bu makalede Azure PowerShell cmdlet'leri yedekleme ve kurtarma Hizmetleri kasasından Azure sanal makine'de (VM) kurtarma için nasıl kullanılacağını gösterir. Bir kurtarma Hizmetleri kasası, Azure Resource Manager kaynağı ve verileri ve varlıkları hem Azure Backup hem de Azure Site Recovery hizmetlerinde korumak için kullanılır. Kurtarma Hizmetleri kasası, Azure Service Manager tarafından dağıtılan Vm'leri ve Azure Resource Manager tarafından dağıtılan Vm'leri korumak için kullanabilirsiniz.

> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager modeli kullanılarak oluşturulan sanal makineler ile kullanımı içindir.
>
>

Bu makalede, bir sanal Makineyi korumak ve veri bir kurtarma noktasından geri yükleme için PowerShell kullanarak adımları gösterilmektedir.

## <a name="concepts"></a>Kavramlar
Hizmetine genel bakış için Azure Backup hizmeti ile aşina değilseniz kullanıma [Azure Backup nedir?](backup-introduction-to-azure-backup.md) Başlamadan önce Azure Backup ve geçerli sanal makine yedekleme çözümü sınırlamaları ile çalışmak için gereken önkoşulları hakkında temel bilgileri kapsayan emin olun.

PowerShell etkili bir şekilde kullanmak için nesnelerin ve nereden başlayacağınızı hiyerarşi anlamak gereklidir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

AzureRm.RecoveryServices.Backup PowerShell cmdlet başvurusu görüntülemek için bkz: [Azure Backup - kurtarma Hizmetleri cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) Azure Kitaplığı'nda.

## <a name="setup-and-registration"></a>Kurulumu ve kaydı
Başlamak için:

1. [PowerShell'in en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (gerekli en düşük sürüm: 1.4.0)

2. Kullanılabilir Azure Backup PowerShell cmdlet'lerini, aşağıdaki komutu yazarak bulabilirsiniz:
    ```PS
    PS C:\> Get-Command *azurermrecoveryservices*
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
    Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
    Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
    Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
    Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
    Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
    Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
    ```
3. Azure oturum açma hesabı kullanarak **Connect-AzureRmAccount**. Bu cmdlet getirir, bir web sayfası için hesap kimlik bilgilerinizi ister: 
    - Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect-AzureRmAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    - Müşteri, Kiracı adına çalışma CSP iş ortağıysanız, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Örneğin: **Connect-AzureRmAccount-Kiracı "fabrikam.com"**
4. Hesabınız birden fazla abonelik olabilir bu yana hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin:

    ```PS
    PS C:\> Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup'ı ilk kez kullanıyorsanız, kullanmalısınız **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```PS
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Aşağıdaki komutları kullanarak sağlayıcılar başarıyla kayıtlı doğrulayabilirsiniz:
    ```PS
    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ``` 
Komut çıktısında **RegistrationState** ayarlamalısınız **kayıtlı**. Aksi takdirde, yalnızca yeniden çalıştırma **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** yukarıda gösterilen cmdlet'i.

PowerShell ile aşağıdaki görevleri otomatik hale getirilebilir:

* [Kurtarma Hizmetleri kasası oluşturma](backup-azure-vms-automation.md#create-a-recovery-services-vault)
* [Azure VM'lerini yedekleme](backup-azure-vms-automation.md#back-up-azure-vms)
* [Bir yedekleme işi tetikleme](backup-azure-vms-automation.md#trigger-a-backup-job)
* [Bir yedekleme işini izleme](backup-azure-vms-automation.md#monitoring-a-backup-job)
* [Azure VM geri yükleme](backup-azure-vms-automation.md#restore-an-azure-vm)

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturma işleminde yol. Bir Backup kasasının kurtarma Hizmetleri kasası farklıdır.

1. Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanın veya bir kaynak grubu oluşturun **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet'i. Bir kaynak grubu oluştururken, kaynak grubunun konumunu ve adını belirtin.  

    ```PS
    PS C:\> New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Kullanım **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet'ini kurtarma Hizmetleri kasası oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirttiğinizden emin olun.

    ```PS
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Kullanılacak depolama yedekliliği türü; belirtin. kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnek, testvault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

    ```PS
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle
Kullanım **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** geçerli abonelikte tüm kasalarının listesi görüntülemek için. Yeni bir kasa oluşturulduğunu denetleyin veya abonelik kullanılabilir kasalarında görmek için bu komutu kullanabilirsiniz.

Tüm kasaları abonelikte görüntülemek için Get-AzureRmRecoveryServicesVault komutu çalıştırın. Aşağıdaki örnek, her kasa için görüntülenen bilgileri gösterir.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Azure VM'lerini yedekleme
Sanal makinelerinizi korumak için bir kurtarma Hizmetleri kasası kullanın. Koruma uyguladığınız önce (kasada korunan veri türü) Kasa bağlamını ayarlamanız ve koruma ilkesini doğrulayın. Koruma İlkesi yedekleme işleri çalıştırma zamanlaması ve her bir yedek anlık görüntüyü ne kadar süreyle tutulduğunu ' dir.

### <a name="set-vault-context"></a>Kasa bağlamı Ayarla
Bir sanal makine üzerindeki korumayı etkinleştirmeden önce kullanmak **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** kasa bağlamını ayarlamak için. Kasa bağlamı ayarlandıktan sonra, sonraki tüm cmdlet’ler için geçerli olur. Aşağıdaki örnek, kasa için kasa bağlamını ayarlar *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Bir koruma ilkesi oluşturma
Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir. Varsayılan koruma ilkesi, her gün belirtilen saatte bir yedekleme işini tetikler. Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur. Farklı ayrıntılarla daha sonra ilkeyi düzenleyebilir ve hızla, sanal Makineyi korumak için varsayılan ilke kullanabilirsiniz.

Kullanım **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** kasaya koruma ilkeleri görüntülemek için. Belirli bir ilke alma ya da bir iş yükü türü ile ilişkili ilkeleri görüntülemek için bu cmdlet'i kullanabilirsiniz. Aşağıdaki örnek, iş yükü türü için AzureVM ilkeleri alır.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> UTC saat dilimi PowerShell BackupTime alanın olur. Ancak, yedekleme zamanını Azure portalında göründüğü zaman, yerel saat dilimine ayarlanır.
>
>

Bir yedekleme koruma İlkesi, en az bir bekletme ilkesi ile ilişkilidir. Bekletme İlkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar. Kullanım **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** varsayılan saklama ilkesini görüntülemek için.  Benzer şekilde kullanabileceğiniz **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** varsayılan zamanlama ilkesini elde edilir. **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur. Zamanlama ve Bekletme İlkesi nesneleri girdi olarak kullanılan **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet'i. Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Örnek, bir koruma ilkesi oluşturulurken parametreler tanımlamak için bu değişkenleri kullanır. *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Korumayı etkinleştir
Yedekleme koruma İlkesi tanımladıktan sonra yine de bir öğe için ilkeyi etkinleştirmeniz gerekir. Kullanım **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** korumasını etkinleştirmek için. Koruma etkinleştirme, iki nesne - öğesini ve ilke gerektirir. İlke kasa ile ilişkilendirildikten sonra yedekleme iş akışı içinde ilke zamanlaması tanımlı zaman tetiklenir.

Aşağıdaki örnek NewPolicy ilke kullanılarak koruma V2VM, öğe için etkinleştirir. Resource Manager vm'lerinde şifrelenmemiş korumasını etkinleştirmek için

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Şifrelenmiş Vm'leri (BEK ve KEK kullanarak şifrelenir) üzerindeki korumayı etkinleştirmek için anahtar kasasından anahtarlar ve gizli anahtarları okumak için Azure Backup hizmeti izin vermeniz gerekir.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Şifrelenmiş (yalnızca BEK kullanarak şifrelenir) Vm'leri üzerinde korumayı etkinleştirmek için anahtar kasasından gizli anahtarları okumak için Azure Backup hizmeti izin vermeniz gerekir.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Azure kamu Bulutu kullanıyorsanız, parametre için değer ff281ffe-705c-4f53-9f37-a40e6f2c68f3 kullanmak **- ServicePrincipalName** içinde [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet'i.
>
>

Klasik VM'ler için

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Bir koruma ilkesini değiştirme
Koruma ilkesini değiştirmek için kullanın [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) SchedulePolicy veya RetentionPolicy nesneleri değiştirmek için.

Aşağıdaki örnek, kurtarma noktası bekletme 365 gün olarak değiştirir.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Bir yedeklemeyi tetikleyin
Kullanabileceğiniz **[Backup-Azurermrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** bir yedekleme işini tetiklemek için. İlk yedekleme ise, tam bir yedeklemedir. Sonraki yedeklemeler, artımlı bir kopya yararlanın. Kullandığınızdan emin olun **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** yedekleme işini tetiklemeden önce kasa bağlamını ayarlamak için. Aşağıdaki örnek, kasa bağlamını ayarlamanız varsayılır.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> UTC saat dilimi PowerShell StartTime ve EndTime alanlarının olur. Ancak, zaman Azure portalında göründüğü zaman, yerel saat dilimine ayarlanır.
>
>

## <a name="monitoring-a-backup-job"></a>Yedekleme işini izleme
Azure portalını kullanarak olmadan yedekleme işleri gibi uzun süre çalışan işlemleri izleyebilirsiniz. Süren işin durumunu almak için kullanın **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet'i. Bu cmdlet, belirli bir kasa için yedekleme işlerinin alır ve bu Kasa Kasa bağlamını içinde belirtilir. Aşağıdaki örnek, bir dizi olarak bir Süren işin durumunu alır ve durum $joblist değişkeninde depolar.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Bu işlerin - gereksiz ek kodu olan - tamamlanması için yoklama yerine kullanmak **[bekleme-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet'i. Bu cmdlet, işi tamamlar veya belirtilen zaman aşımı değerine ulaşılana kadar yürütmeyi duraklatır.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Azure VM geri yükleme
Azure portalını kullanarak bir VM geri yükleme ve PowerShell kullanarak bir VM geri yükleme arasında önemli bir fark yoktur. PowerShell ile disk ve yapılandırma bilgilerini kurtarma noktasından oluşturulduktan sonra geri yükleme işlemi tamamlanır. Geri yüklemek veya bir Azure VM yedeklemesi birkaç dosyaları kurtarmak istiyorsanız, başvurmak [dosya kurtarma bölümüne](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)

> [!NOTE]
> Geri yükleme işlemi, bir sanal makine oluşturmaz.
>
>

Diskten bir sanal makine oluşturmak için konudaki [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). Bir Azure VM'yi geri yüklemek için temel adımlar şunlardır:

* Sanal Makineyi seçin
* Bir kurtarma noktası seçin
* Diskleri geri yükle
* Saklı diskten VM oluşturma

Aşağıdaki grafikte, BackupRecoveryPoint aşağı RecoveryServicesVault gelen nesne hiyerarşisi gösterilmektedir.

![Kurtarma Hizmetleri nesne hiyerarşisi BackupContainer gösteriliyor](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Yedekleme verilerini geri yüklemek için yedekleme öğesi ve zaman içinde nokta verileri tutan bir kurtarma noktası belirleyin. Kullanım **[Restore-Azurermrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** verileri kasadan müşterinin hesabına geri yüklemek için cmdlet'i.

### <a name="select-the-vm"></a>Sanal Makineyi seçin
Doğru yedekleme öğeyi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı doğru şekilde çalışır. VM'yi temsil eden kapsayıcı seçmek için kullanın **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet'i ve için kanal **[ Get-Azurermrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet'i.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin
Kullanım **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Geri yüklemek için kurtarma noktası seçin. Kullanmak için hangi kurtarma noktasının konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır = AppConsistent nokta listesinde.

Aşağıdaki komut, değişken **$rp**, son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi dizin 0 konumunda en son kurtarma noktası ile zaman ters sırada sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, en son kurtarma noktası $rp [0] seçer.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-the-disks"></a>Diskleri geri yükle
Kullanım **[Restore-Azurermrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** bir yedekleme öğesinin verileri ve yapılandırma, bir kurtarma noktasına geri yüklemek için cmdlet'i. Bir kurtarma noktası belirledikten sonra değeri olarak kullanın **- RecoveryPoint** parametresi. Önceki örnek kodda **$rp [0]** kurtarma noktasını kullanmak üzere oluştu. Aşağıdaki örnek kodda **$rp [0]** disk geri yüklemek için kullanılacak kurtarma noktası.

Diskleri ve yapılandırma bilgilerini geri yüklemek için:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Kullanım **[bekleme-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet'ini geri yükleme işinin tamamlanmasını bekleyin.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Geri yükleme işi tamamlandıktan sonra kullanın **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** geri yükleme işleminin ayrıntılarını almak için cmdlet. VM yeniden oluşturmak için gereken bilgileri JobDetails özelliği vardır.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Diskleri geri yükledikten sonra VM oluşturmak için sonraki bölüme gidin.

## <a name="create-a-vm-from-restored-disks"></a>Geri yüklenen diskten VM oluşturma
Diskleri geri yükledikten sonra oluşturmak ve diskten sanal makine yapılandırmak için aşağıdaki adımları kullanın.

> [!NOTE]
> Geri yüklenen disklerden şifreli VM'ler oluşturmak için Azure rolünüz eylemi gerçekleştirme izni olmalıdır **Microsoft.KeyVault/vaults/deploy/action**. Rolünüz bu izne sahip değilse bu eylem ile özel bir rol oluşturun. Daha fazla bilgi için [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md).
>
>

1. İş ayrıntılarını geri yüklenen diski özelliklerini sorgulayın.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $configBlobName = $properties["Config Blob Name"]
  ```

2. Azure depolama bağlamını ayarlayın ve JSON yapılandırma dosyasını geri yükleyin.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. VM yapılandırması oluşturmak için JSON yapılandırma dosyası kullanın.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. İşletim sistemi diski ve veri diskleri ekleme. Sanal makinelerinizin yapılandırmasına bağlı olarak, ilgili cmdlet'ler görüntülemek için ilgili bölüme bakın:

    #### <a name="non-managed-non-encrypted-vms"></a>Yönetilmeyen, şifrelenmemiş VM

    Aşağıdaki örnek, yönetilmeyen, şifreli olmayan VM'ler için kullanın.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Yönetilmeyen, şifrelenmiş VM'ler (yalnızca BEK)

    (Yalnızca BEK kullanarak şifrelenir) yönetilmeyen, şifreli VM'ler için diskler iliştirebilmek için önce gizli anahtar Kasası'na geri yüklemeniz gerekecektir. Daha fazla bilgi için lütfen bkz [şifrelenmiş bir sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri diskleri ekleme işlemi gösterilmektedir.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    ```
    
 İşletim sistemi diski ayarlanırken, uygun işletim sistemi türü belirtilen emin olun   
    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
    
Veri diskleri şifreleme, aşağıdaki komutu kullanarak el ile etkinleştirilmelidir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -VolumeType Data
    ```
    
   #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Yönetilmeyen, şifrelenmiş VM'ler (BEK ve KEK)

   (BEK ve KEK kullanarak şifrelenir) yönetilmeyen, şifreli VM'ler için diskler iliştirebilmek için önce anahtar ve gizli anahtar Kasası'na geri gerekir. Daha fazla bilgi için lütfen bkz [şifrelenmiş bir sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri diskleri ekleme işlemi gösterilmektedir.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

Veri diskleri şifreleme, aşağıdaki komutu kullanarak el ile etkinleştirilmelidir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-non-encrypted-vms"></a>Yönetilen, şifrelenmemiş VM

   Yönetilen şifrelenmiş olmayan VM'ler için blob depolama alanından yönetilen disk oluşturmak ve ardından diski gerekecektir. Ayrıntılı bilgi için bkz [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod için yönetilen şifrelenmemiş VM veri diski gösterilmektedir.

    ```
    PS C:\> $storageType = "Standard_LRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

   #### <a name="managed-encrypted-vms-bek-only"></a>Yönetilen şifrelenmiş Vm'leri (yalnızca BEK)

   (Yalnızca BEK kullanarak şifrelenir) yönetilen şifrelenmiş Vm'leri için blob depolama alanından yönetilen disk oluşturmak ve ardından diski gerekecektir. Ayrıntılı bilgi için bkz [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod için yönetilen şifrelenmiş Vm'leri veri diski gösterilmektedir.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "Standard_LRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

Veri diskleri şifreleme, aşağıdaki komutu kullanarak el ile etkinleştirilmelidir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
   #### <a name="managed-encrypted-vms-bek-and-kek"></a>Yönetilen şifrelenmiş Vm'leri (BEK ve KEK)

   (BEK ve KEK kullanarak şifrelenir) yönetilen şifrelenmiş Vm'leri için blob depolama alanından yönetilen disk oluşturmak ve ardından diski gerekecektir. Ayrıntılı bilgi için bkz [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod için yönetilen şifrelenmiş Vm'leri veri diski gösterilmektedir.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "Standard_LRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```
Veri diskleri şifreleme, aşağıdaki komutu kullanarak el ile etkinleştirilmelidir.

    ```
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $dekUrl -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
    ```
    
5. Ağ ayarlarını yapın.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $virtualNetwork = New-AzureRmVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    PS C:\> $virtualNetwork | Set-AzureRmVirtualNetwork
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $subnetindex=0
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Sanal makineyi oluşturun.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Dosyaları bir Azure sanal makine yedeklemesini geri yükleme

Diskleri geri ek olarak, bir Azure sanal makine yedeklemesini tek tek dosyaları da geri yükleyebilirsiniz. Geri yükleme dosyaları işlevsellik, bir kurtarma noktasındaki tüm dosyalara erişim sağlar ve normal dosyalar için yaptığınız şekilde bunları dosya Gezgini aracılığıyla yönetebilirsiniz.

Bir dosyayı Azure VM yedeklemesi geri yüklemek için temel adımlar şunlardır:

* Sanal Makineyi seçin
* Bir kurtarma noktası seçin
* Disk kurtarma noktası takma
* Gerekli dosyaları Kopyala
* Diski çıkarın


### <a name="select-the-vm"></a>Sanal Makineyi seçin
Doğru yedekleme öğeyi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı doğru şekilde çalışır. VM'yi temsil eden kapsayıcı seçmek için kullanın **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet'i ve için kanal **[ Get-Azurermrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet'i.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin
Kullanım **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Geri yüklemek için kurtarma noktası seçin. Kullanmak için hangi kurtarma noktasının konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır = AppConsistent nokta listesinde.

Aşağıdaki komut, değişken **$rp**, son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi dizin 0 konumunda en son kurtarma noktası ile zaman ters sırada sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, en son kurtarma noktası $rp [0] seçer.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>Disk kurtarma noktası takma

Kullanım **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)** kurtarma noktasının tüm diskleri takmak için betiği almak için cmdlet.

> [!NOTE]
> Diskler, betiğin çalıştırıldığı makinede iSCSI bağlı diskleri olarak bağlanır. Bu nedenle neredeyse anında ve herhangi bir ücrete tabi değildir
>
>

```
PS C:\> Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]

OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```
Betik dosyaları kurtarmak istediğiniz makinede çalıştırın. Betiği çalıştırmak için yukarıda gösterilen parolayı girmeniz gerekir. Bağlı diskleri sonra dosya ve yeni birimleri göz atmak için Windows dosya Gezgini'ni kullanın. Daha fazla bilgi için bkz [dosya kurtarma belgeleri](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>Diskleri çıkarın
Kullanarak gerekli dosyaları kopyaladıktan sonra diskleri çıkarma **[devre dışı bırak AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)** cmdlet'i. Kurtarma noktasının dosyalara erişim kaldırıldığından emin getirir bu kesinlikle önerilir

```
PS C:\> Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```


## <a name="next-steps"></a>Sonraki adımlar
Azure kaynaklarıyla etkileşim kurmak amacıyla PowerShell kullanmayı tercih ederseniz, PowerShell bkz [dağıtma ve yönetme Windows Server için Yedekleme'yi](backup-client-automation.md). DPM yedeklemelerini yönetiyorsanız bkz [dağıtma ve yönetme yedekleme için DPM](backup-dpm-automation.md). Bu makale her ikisi de Resource Manager dağıtımları ve klasik dağıtımlar için bir sürüm var.  
