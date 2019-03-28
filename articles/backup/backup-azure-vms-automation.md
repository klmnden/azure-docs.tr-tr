---
title: Yedekleme ve kurtarma PowerShell ile Azure Backup'ı kullanarak Azure Vm'leri
description: Yedekleme ve PowerShell ile Azure Backup'ı kullanarak Azure Vm'leri kurtarma açıklar
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: raynew
ms.openlocfilehash: 230c68b0b1de1ef452de51b7b0661a3c3786ea76
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521712"
---
# <a name="back-up-and-restore-azure-vms-with-powershell"></a>Yedekleme ve PowerShell ile Azure Vm'lerini geri yükleme

Bu makalede bir Azure sanal Makinesinde geri açıklanmaktadır bir [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası PowerShell cmdlet'lerini kullanarak. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Tetikleyici, önce korumalı sanal makineler için bir isteğe bağlı yedekleme işi yedekleyebilir (veya korumak) bir sanal makine, tamamlamalısınız [önkoşulları](backup-azure-arm-vms-prepare.md) sanal makinelerinizi korumak için ortamınızı hazırlamak için. 




## <a name="before-you-start"></a>Başlamadan önce

- [Daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasası hakkında.
- [Gözden geçirme](backup-architecture.md#architecture-direct-backup-of-azure-vms) mimari Azure VM yedeklemesi için [hakkında bilgi edinin](backup-azure-vms-introduction.md) yedekleme işlemi ve [gözden](backup-support-matrix-iaas.md) desteği, sınırlamalar ve önkoşullar.
- Kurtarma Hizmetleri için PowerShell nesne hiyerarşisi gözden geçirin.


## <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Gözden geçirme **Az.RecoveryServices** [cmdlet başvurusu](https://docs.microsoft.com/powershell/module/Az.RecoveryServices/?view=azps-1.4.0) Azure Kitaplığı Başvurusu.



## <a name="set-up-and-register"></a>Ayarlama ve kaydetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Başlamak için:

1. [PowerShell'in en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/install-az-ps)

2. Kullanılabilir Azure Backup PowerShell cmdlet'lerini, aşağıdaki komutu yazarak bulabilirsiniz:

    ```powershell
    Get-Command *azrecoveryservices*
    ```   
 
    Azure Backup, Azure Site Recovery ve kurtarma Hizmetleri kasası için cmdlet'leri ve diğer adları görüntülenir. Aşağıdaki görüntüde, görürsünüz, bir örnektir. Cmdlet öğelerinin tam listesi değil.

    ![Kurtarma Hizmetleri listesi](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. Azure oturum açma hesabı kullanarak **Connect AzAccount**. Bu cmdlet getirir, bir web sayfası için hesap kimlik bilgilerinizi ister:

    * Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    * Müşteri, Kiracı adına çalışma CSP iş ortağıysanız, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Örneğin: **Connect AzAccount-Kiracı "fabrikam.com"**

4. Hesabınız birden fazla abonelik olabilir bu yana hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin:

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup'ı ilk kez kullanıyorsanız, kullanmalısınız **[Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Aşağıdaki komutları kullanarak sağlayıcılar başarıyla kayıtlı doğrulayabilirsiniz:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    Komut çıktısında **RegistrationState** değiştirilmelidir **kayıtlı**. Değil, yalnızca çalıştırırsanız **[Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** cmdlet'ini yeniden.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturma işleminde yol. Bir Backup kasasının kurtarma Hizmetleri kasası farklıdır.

1. Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanın veya bir kaynak grubu oluşturun **[yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)** cmdlet'i. Bir kaynak grubu oluştururken, kaynak grubunun konumunu ve adını belirtin.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Kullanım [yeni AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault?view=azps-1.4.0) cmdlet'ini kurtarma Hizmetleri kasası oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirttiğinizden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Kullanılacak depolama yedekliliği türü; belirtin. kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnek, testvault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Tüm kasaları abonelikte görüntülemek için kullanın [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0):

```powershell
Get-AzRecoveryServicesVault
```

Aşağıdaki örneğe benzer bir çıkış, ilişkili ResourceGroupName ve konum sağlanan dikkat edin.

```
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

Bir sanal makine üzerindeki korumayı etkinleştirmeden önce kullanmak [kümesi AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0) kasa bağlamını ayarlamak için. Kasa bağlamı ayarlandıktan sonra, sonraki tüm cmdlet’ler için geçerli olur. Aşağıdaki örnek, kasa için kasa bağlamını ayarlar *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Bir koruma ilkesi oluşturma

Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir. Varsayılan koruma ilkesi, her gün belirtilen saatte bir yedekleme işini tetikler. Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur. Farklı ayrıntılarla daha sonra ilkeyi düzenleyebilir ve hızla, sanal Makineyi korumak için varsayılan ilke kullanabilirsiniz.

Kullanım **[Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) kasaya kullanılabilir koruma ilkeleri görüntülemek için. Belirli bir ilke alma ya da bir iş yükü türü ile ilişkili ilkeleri görüntülemek için bu cmdlet'i kullanabilirsiniz. Aşağıdaki örnek, iş yükü türü için AzureVM ilkeleri alır.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
```

Çıktı aşağıdaki örneğe benzer:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> UTC saat dilimi PowerShell BackupTime alanın olur. Ancak, yedekleme zamanını Azure portalında göründüğü zaman, yerel saat dilimine ayarlanır.
>
>

Bir yedekleme koruma İlkesi, en az bir bekletme ilkesi ile ilişkilidir. Bir bekletme ilkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar.

- Kullanım [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject) varsayılan saklama ilkesini görüntülemek için.
- Benzer şekilde kullanabileceğiniz [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject) varsayılan zamanlama ilkesini elde edilir.
- [Yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur.
- Zamanlama ve Bekletme İlkesi nesneleri yeni AzRecoveryServicesBackupProtectionPolicy cmdlet'e girdi olarak kullanılır.

Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Örnek, bir koruma ilkesi oluşturulurken parametreler tanımlamak için bu değişkenleri kullanır. *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdaki örneğe benzer:

```
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>Korumayı etkinleştir

Koruma İlkesi tanımladınız sonra hala bir öğe için ilkeyi etkinleştirmeniz gerekir. Kullanım [etkinleştir AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) korumasını etkinleştirmek için. Koruma etkinleştirme, iki nesne - öğesini ve ilke gerektirir. İlke kasa ile ilişkilendirildikten sonra yedekleme iş akışı içinde ilke zamanlaması tanımlı zaman tetiklenir.

Aşağıdaki örnekler NewPolicy ilke kullanılarak koruma V2VM, öğe için etkinleştirin. Örneklerde VM şifreli bağlı olarak farklılık gösterir ve ne şifrelenmesi yazın.

Korumayı etkinleştirmek için **şifrelenmemiş Resource Manager Vm'lerinde**:

```powershell
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Şifrelenmiş Vm'leri (BEK ve KEK kullanarak şifrelenir) üzerindeki korumayı etkinleştirmek için anahtar kasasından anahtarlar ve gizli anahtarları okumak için Azure Backup hizmeti izin vermeniz gerekir.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Korumayı etkinleştirmek için **şifrelenmiş (yalnızca BEK kullanarak şifrelenir) Vm'leri**, anahtar kasasından gizli anahtarları okumak için Azure Backup hizmeti izin vermeniz gerekir.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Azure kamu Bulutu kullanıyorsanız ServicePrincipalName parametresi için değer ff281ffe-705c-4f53-9f37-a40e6f2c68f3 kullanın, [kümesi AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet'i.
>


### <a name="modify-a-protection-policy"></a>Bir koruma ilkesini değiştirme

Koruma ilkesini değiştirmek için kullanın [kümesi AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy) SchedulePolicy veya RetentionPolicy nesneleri değiştirmek için.

Aşağıdaki örnek, kurtarma noktası bekletme 365 gün olarak değiştirir.

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Bir yedeklemeyi tetikleyin

Kullanım [yedekleme AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) bir yedekleme işini tetiklemek için. İlk yedekleme ise, tam bir yedeklemedir. Sonraki yedeklemeler, artımlı bir kopya yararlanın. Kullandığınızdan emin olun **[kümesi AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext)** yedekleme işini tetiklemeden önce kasa bağlamını ayarlamak için. Aşağıdaki örnek, zaten kasa bağlamını ayarlamanız varsayar.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
$job = Backup-AzRecoveryServicesBackupItem -Item $item
```

Çıktı aşağıdaki örneğe benzer:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> UTC saat dilimi PowerShell StartTime ve EndTime alanlarının olur. Ancak, zaman Azure portalında göründüğü zaman, yerel saat dilimine ayarlanır.
>
>

## <a name="monitoring-a-backup-job"></a>Yedekleme işini izleme

Azure portalını kullanarak olmadan yedekleme işleri gibi uzun süre çalışan işlemleri izleyebilirsiniz. Süren işin durumunu almak için kullanın [Get-AzRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) cmdlet'i. Bu cmdlet, belirli bir kasa için yedekleme işlerinin alır ve bu Kasa Kasa bağlamını içinde belirtilir. Aşağıdaki örnek, bir dizi olarak bir Süren işin durumunu alır ve durum $joblist değişkeninde depolar.

```powershell
$joblist = Get-AzRecoveryservicesBackupJob –Status "InProgress"
$joblist[0]
```

Çıktı aşağıdaki örneğe benzer:

```
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Bu işlerin - gereksiz ek kodu olan - tamamlanması için yoklama yerine kullanmak [bekleme AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet'i. Bu cmdlet, işi tamamlar veya belirtilen zaman aşımı değerine ulaşılana kadar yürütmeyi duraklatır.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Azure VM geri yükleme

Azure portalını kullanarak bir VM geri yükleme ve PowerShell kullanarak bir VM geri yükleme arasında önemli bir fark yoktur. PowerShell ile disk ve yapılandırma bilgilerini kurtarma noktasından oluşturulduktan sonra geri yükleme işlemi tamamlanır. Geri yükleme işlemi, sanal makine oluşturmaz. Diskten bir sanal makine oluşturmak için konudaki [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). Tüm VM'yi geri yüklemek istemiyorsanız, ancak geri yüklemek veya bir Azure VM yedeklemesi birkaç dosyaları kurtarmak istiyorsanız, başvurmak [dosya kurtarma bölümüne](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup).

> [!Tip]
> Geri yükleme işlemi, sanal makine oluşturmaz.
>
>

Aşağıdaki grafikte, BackupRecoveryPoint aşağı RecoveryServicesVault gelen nesne hiyerarşisi gösterilmektedir.

![Kurtarma Hizmetleri nesne hiyerarşisi BackupContainer gösteriliyor](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Yedekleme verilerini geri yüklemek için yedekleme öğesi ve zaman içinde nokta verileri tutan bir kurtarma noktası belirleyin. Kullanım [geri yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) verileri kasadan hesabınıza geri yüklemek için.

Bir Azure VM'yi geri yüklemek için temel adımlar şunlardır:

* VM’yi seçin.
* Bir kurtarma noktası seçin.
* Diskleri geri yükleyin.
* VM saklı disklerden oluşturun.

### <a name="select-the-vm"></a>Sanal Makineyi seçin

Doğru yedekleme öğeyi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı doğru şekilde çalışır. VM'yi temsil eden kapsayıcı seçmek için kullanın [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet'i ve için kanal [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet'i.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin

Kullanım [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Geri yüklemek için kurtarma noktası seçin. Kullanmak için hangi kurtarma noktasının konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır = AppConsistent nokta listesinde.

Aşağıdaki komut, değişken **$rp**, son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi dizin 0 konumunda en son kurtarma noktası ile zaman ters sırada sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, en son kurtarma noktası $rp [0] seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

Çıktı aşağıdaki örneğe benzer:

```powershell
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

Kullanım [geri yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) bir yedekleme öğesinin verileri ve yapılandırma, bir kurtarma noktasına geri yüklemek için cmdlet'i. Bir kurtarma noktası tanımladıktan sonra değeri olarak kullanın **- RecoveryPoint** parametresi. Yukarıdaki örnekte, **$rp [0]** kurtarma noktasını kullanmak üzere oluştu. Aşağıdaki örnek kodda **$rp [0]** disk geri yüklemek için kullanılacak kurtarma noktası.

Diskleri ve yapılandırma bilgilerini geri yüklemek için:

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
$restorejob
```

#### <a name="restore-managed-disks"></a>Yönetilen diskleri geri yükle

> [!NOTE]
> Yedeklenen sanal makine yönetilen diskleri ve bunları yönetilen diskler olarak geri yüklemek istiyorsanız, Azure PowerShell RM modülünden v 6.7.0 yeteneği ekledik. ve sonraki sürümler
>
>

Ek bir parametre sağlayın **TargetResourceGroupName** için yönetilen diskleri geri yükleneceği RG belirtmek için. 

> [!NOTE]
> Kullanılacak önemle tavsiye edilir **TargetResourceGroupName** önemli performans geliştirmeleri sonuçları olduğundan geri yükleme parametresi yönetilen diskler. Ayrıca, Azure Powershell Az modülünden 1.0 veya sonraki sürümleri, bu parametre yönetilen disklere sahip bir geri yükleme durumunda zorunludur
>
>


```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -TargetResourceGroupName "DestRGforManagedDisks"
```

**VMConfig.JSON** dosya depolama hesabına geri yükleneceği ve yönetilen diskler, belirtilen hedef RG kurulacaktır.

Çıktı aşağıdaki örneğe benzer:

```powershell
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Kullanım [bekleme AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet'ini geri yükleme işinin tamamlanmasını bekleyin.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Geri yükleme işi tamamlandıktan sonra kullanın [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) geri yükleme işleminin ayrıntılarını almak için cmdlet. VM yeniden oluşturmak için gereken bilgileri JobDetails özelliği vardır.

```powershell
$restorejob = Get-AzRecoveryServicesBackupJob -Job $restorejob
$details = Get-AzRecoveryServicesBackupJobDetails -Job $restorejob
```

Diskleri geri yükledikten sonra VM oluşturmak için sonraki bölüme gidin.

## <a name="create-a-vm-from-restored-disks"></a>Geri yüklenen diskten VM oluşturma

Diskleri geri yükledikten sonra oluşturmak ve diskten sanal makine yapılandırmak için aşağıdaki adımları kullanın.

> [!NOTE]
> Geri yüklenen disklerden şifreli VM'ler oluşturmak için Azure rolünüz eylemi gerçekleştirme izni olmalıdır **Microsoft.KeyVault/vaults/deploy/action**. Rolünüz bu izne sahip değilse bu eylem ile özel bir rol oluşturun. Daha fazla bilgi için [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md).
>
>

> [!NOTE]
> Diskleri geri yükledikten sonra artık doğrudan yeni bir VM oluşturmak için kullanabileceğiniz bir dağıtım şablonu alabilirsiniz. Şifrelenmiş ve şifrelenmemiş olan yönetilen veya yönetilmeyen VM oluşturmak için daha fazla farklı PS cmdlet'leri.

Sonuç iş ayrıntılarını sorgulanabilir ve dağıtılan URI şablonu sağlar.

```powershell
   $properties = $details.properties
   $templateBlobURI = $properties["Template Blob Uri"]
```

Yalnızca açıklandığı gibi yeni bir VM oluşturmak için şablon dağıtma [burada](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment ResourceGroupName ExampleResourceGroup -TemplateUri $templateBlobURI -storageAccountType Standard_GRS
```

Aşağıdaki bölümde "VMConfig" dosyasını kullanarak bir VM oluşturmak için gerekli adımları listeler.

> [!NOTE]
> Bir VM oluşturmak için yukarıdaki ayrıntılı dağıtım şablonu kullanmak için önerilir. Bu bölümde (1-6 nokta) yakında kullanımdan kaldırılacak.

1. İş ayrıntılarını geri yüklenen diski özelliklerini sorgulayın.

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. Azure depolama bağlamını ayarlayın ve JSON yapılandırma dosyasını geri yükleyin.

   ```powershell
   Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. VM yapılandırması oluşturmak için JSON yapılandırma dosyası kullanın.

   ```powershell
   $vm = New-AzVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. İşletim sistemi diski ve veri diskleri ekleme. Bu adım, örnekler çeşitli için yönetilen ve VM yapılandırmaları şifrelenmiş sağlar. VM yapılandırmanızı uygun örneği kullanın.

   * **Yönetilmeyen ve şifreli olmayan VM'ler** -yönetilmeyen, şifrelenmemiş sanal makineler için aşağıdaki örneği kullanın.

       ```powershell
       Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
       ```

   * **Azure AD (yalnızca BEK) ile yönetilmeyen ve şifrelenmiş Vm'leri** -Azure AD (yalnızca BEK kullanarak şifrelenir) ile yönetilmeyen, şifreli VM'ler için diskler iliştirebilmek için önce gizli anahtar Kasası'na geri yüklemeniz gereken. Daha fazla bilgi için [şifrelenmiş bir sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri diskleri ekleme işlemi gösterilmektedir. İşletim sistemi diski ayarlarken, uygun işletim sistemi türü bahsetmek emin olun.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Azure AD (BEK ve KEK) ile yönetilmeyen ve şifrelenmiş Vm'leri** - (şifrelenmiş BEK ve KEK kullanarak), Azure AD ile yönetilmeyen, şifreli VM'ler için anahtar ve gizli dizi diskleri eklemeden önce anahtar Kasası'na geri yükleyin. Daha fazla bilgi için [şifrelenmiş bir sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri diskleri ekleme işlemi gösterilmektedir.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
      ```

   * **Yönetilmeyen ve şifrelenmiş sanal makineleri Azure AD (yalnızca BEK) olmadan** - (BEK yalnızca kullanılarak şifrelenmiş), Azure AD olmadan yönetilmeyen, şifreli VM'ler için kaynak **keyVault/parola kullanılamaz** gizli dizileri anahtar Kasası'na geri yükleme yordamı kullanarak [bir şifrelenmemiş sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Sonra şifreleme ayrıntıları (Bu adım veri blob'u için gerekli değildir), geri yüklenen işletim sistemi blob ayarlamak için aşağıdaki komut yürütün. Geri yüklenen anahtar kasası $dekurl getirilebilir.<br>

   Aşağıdaki komut dosyası yalnızca kaynak keyVault/parola kullanılabilir olmadığında yürütülen gerekiyor.

      ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
      ```

    Sonra **gizli dizileri kullanılabilir** ve şifreleme ayrıntıları da işletim sistemi Bloba ayarlamak, aşağıda verilen betiği kullanarak diskleri bağlayın.<br>

    Kaynak keyVault/gizli zaten varsa, ardından yukarıdaki betik yürütülmedi.

      ```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Yönetilmeyen ve şifrelenmiş VM'ler (BEK ve KEK) Azure AD olmadan** - (şifrelenmiş. BEK ve KEK kullanarak), Azure AD olmadan yönetilmeyen, şifreli VM'ler için kaynak **keyVault/anahtar/parola kullanılamaz** anahtarını geri yükleyin ve yordamı kullanarak anahtar kasası için gizli dizileri [bir şifrelenmemiş sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Sonra şifreleme ayrıntıları (Bu adım veri blob'u için gerekli değildir), geri yüklenen işletim sistemi blob ayarlamak için aşağıdaki komut yürütün. Geri yüklenen anahtar kasası $dekurl ve $kekurl getirilebilir.

   Aşağıdaki komut dosyası yalnızca keyVault/anahtar/parola kaynak kullanılabilir olmadığında yürütülen gerekiyor.

    ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""},""keyEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""keyUrl"":""$kekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
      ```
   Sonra **anahtarı/gizli kullanılabilir** ve işletim sistemi Bloba kümesi şifreleme ayrıntıları, aşağıda verilen betiği kullanarak diskleri bağlayın.

    KeyVault/anahtar/gizli kaynak kullanamıyorsanız, ardından yukarıdaki betik yürütülmedi.

    ```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
      ```

   * **Yönetilen ve şifreli olmayan Vm'leri** - şifrelenmemiş yönetilen sanal makineler için geri yüklenen yönetilen diskleri bağlayın. Ayrıntılı bilgi için bkz: [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md).

   * **Yönetilen ve Azure AD (yalnızca BEK) ile VM'ler şifreli** : (yalnızca BEK kullanarak şifrelenir), Azure AD ile yönetilen şifrelenmiş Vm'leri eklemek için geri yüklenen yönetilen diskler. Ayrıntılı bilgi için bkz: [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md).

   * **Yönetilen ve şifrelenmiş Vm'leri (BEK ve KEK) Azure AD ile** : (şifrelenmiş BEK ve KEK kullanarak), Azure AD ile yönetilen şifrelenmiş Vm'leri eklemek için geri yüklenen yönetilen diskler. Ayrıntılı bilgi için bkz: [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md).

   * **Yönetilen ve Azure AD (yalnızca BEK) olmayan VM'ler şifreli** -yönetilen için şifrelenmiş Vm'leri (BEK yalnızca kullanılarak şifrelenmiş), Azure AD, kaynak **keyVault/parola kullanılamaz** gizli dizileri anahtar kasasını kullanarak geri yordamda [bir şifrelenmemiş sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Sonra (Bu adım veri diski için gerekli değildir), geri yüklenen işletim sistemi disk şifreleme ayrıntıları ayarlamak için aşağıdaki komut yürütün. Geri yüklenen anahtar kasası $dekurl getirilebilir.

     Aşağıdaki komut dosyası yalnızca kaynak keyVault/parola kullanılabilir olmadığında yürütülen gerekiyor.  

     ```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
      $diskupdateconfig = Set-AzDiskUpdateDiskEncryptionKey -DiskUpdate $diskupdateconfig -SecretUrl $dekUrl -SourceVaultId $keyVaultId  
      Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
      ```

     Gizli dizileri kullanılabilir ve işletim sistemi diskinde kümesi şifreleme ayrıntıları sonra geri yüklenen yönetilen diskleri eklemek için bkz: [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md).

   * **Yönetilen ve Azure AD (BEK ve KEK) olmayan VM'ler şifreli** - yönetilen için şifrelenmiş VM'ler (şifrelenmiş BEK ve KEK kullanarak), Azure AD olmadan, kaynak **keyVault/anahtar/parola kullanılamaz** anahtar ve gizli anahtarı geri yükleme yordamı kullanarak kasa [bir şifrelenmemiş sanal makine bir Azure Backup kurtarma noktasından geri yükleme](backup-azure-restore-key-secret.md). Sonra (Bu adım veri diski için gerekli değildir), geri yüklenen işletim sistemi disk şifreleme ayrıntıları ayarlamak için aşağıdaki komut yürütün. Geri yüklenen anahtar kasası $dekurl ve $kekurl getirilebilir.

   Aşağıdaki komut dosyası yalnızca keyVault/anahtar/parola kaynak kullanılabilir olmadığında yürütülen gerekiyor.

   ```powershell
     $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
     $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
     $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
     $diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
     $diskupdateconfig = Set-AzDiskUpdateDiskEncryptionKey -DiskUpdate $diskupdateconfig -SecretUrl $dekUrl -SourceVaultId $keyVaultId  
     $diskupdateconfig = Set-AzDiskUpdateKeyEncryptionKey -DiskUpdate $diskupdateconfig -KeyUrl $kekUrl -SourceVaultId $keyVaultId  
     Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
    ```

    Anahtar/gizli dizileri kullanılabilir ve işletim sistemi diskinde kümesi şifreleme ayrıntıları sonra geri yüklenen yönetilen diskleri eklemek için bkz: [Windows PowerShell kullanarak bir VM'ye veri diski](../virtual-machines/windows/attach-disk-ps.md).

5. Ağ ayarlarını yapın.

    ```powershell
    $nicName="p1234"
    $pip = New-AzPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzVirtualNetwork
    $vnet = Get-AzVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
    ```

6. Sanal makineyi oluşturun.

    ```powershell  
    New-AzVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

7. ADE uzantı gönderin.

   * **Azure AD ile bir VM için** -el ile veri diskleri için şifrelemeyi etkinleştirmek için aşağıdaki komutu kullanın.  

     **BEK yalnızca**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

     **BEK ve KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId  -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **Azure AD olmayan VM için** -el ile veri diskleri için şifrelemeyi etkinleştirmek için aşağıdaki komutu kullanın.

     Varsa, Azure PowerShell güncelleştirmeye gerek duyduğunuz sonra Aadclientıd için ister komut yürütme sırasında halinde.

     **BEK yalnızca**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

      **BEK ve KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Dosyaları bir Azure sanal makine yedeklemesini geri yükleme

Diskleri geri ek olarak, bir Azure sanal makine yedeklemesini tek tek dosyaları da geri yükleyebilirsiniz. Geri yükleme dosyaları işlevsellik tüm dosyalara bir kurtarma noktası erişimi sağlar. Normal dosyaları gibi dosyalar dosya Gezgini aracılığıyla yönetin.

Bir dosyayı bir Azure sanal makine yedeklemesini geri yükleme için temel adımlar şunlardır:

* Sanal Makineyi seçin
* Bir kurtarma noktası seçin
* Disk kurtarma noktası takma
* Gerekli dosyaları Kopyala
* Diski çıkarın

### <a name="select-the-vm"></a>Sanal Makineyi seçin

Doğru yedekleme öğeyi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı doğru şekilde çalışır. VM'yi temsil eden kapsayıcı seçmek için kullanın [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet'i ve için kanal [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet'i.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin

Kullanım [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Geri yüklemek için kurtarma noktası seçin. Kullanmak için hangi kurtarma noktasının konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır = AppConsistent nokta listesinde.

Aşağıdaki komut, değişken **$rp**, son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi dizin 0 konumunda en son kurtarma noktası ile zaman ters sırada sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, en son kurtarma noktası $rp [0] seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
$rp[0]
```

Çıktı aşağıdaki örneğe benzer:

```
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

Kullanım [Get-AzRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprpmountscript) kurtarma noktasının tüm diskleri takmak için betiği almak için cmdlet.

> [!NOTE]
> Diskler, betiğin çalıştırıldığı makinede iSCSI bağlı diskleri olarak bağlanır. Herhangi bir ücret oluşmaması ve bağlama hemen oluşur.
>
>

```powershell
Get-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

Çıktı aşağıdaki örneğe benzer:

```powershell
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

Betik dosyaları kurtarmak istediğiniz makinede çalıştırın. Betiği çalıştırmak için sağlanan parolayı girmeniz gerekir. Bağlı diskleri sonra dosya ve yeni birimleri göz atmak için Windows dosya Gezgini'ni kullanın. Daha fazla bilgi için yedekleme makaleye bakın [dosyaları Azure sanal makine yedekten kurtarma](backup-azure-restore-files-from-vm.md).

### <a name="unmount-the-disks"></a>Diskleri çıkarın

Gerekli dosyaları kopyaladıktan sonra kullanın [devre dışı bırak AzRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackuprpmountscript) diskleri çıkarma için. Kurtarma noktasının dosyalara erişimi kaldırılır diskleri çıkarma emin olun.

```powershell
Disable-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynaklarıyla etkileşim kurmak amacıyla PowerShell kullanmayı tercih ederseniz, PowerShell bkz [dağıtma ve yönetme Windows Server için Yedekleme'yi](backup-client-automation.md). DPM yedeklemelerini yönetiyorsanız bkz [dağıtma ve yönetme yedekleme için DPM](backup-dpm-automation.md). 
