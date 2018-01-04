---
title: "Dağıtma ve PowerShell kullanarak Resource Manager tarafından dağıtılan VM'ler için yedeklemeleri yönetme | Microsoft Docs"
description: "Dağıtma yedeklemeler ve Azure Resource Manager tarafından dağıtılan VM'ler için yönetmek için PowerShell kullanma"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/20/2017
ms.author: markgal;trinadhk;pullabhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474c5a6d0e7d3647ca14cb61e7b2718c99fdfa72
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>Sanal makineleri yedeklemek için AzureRM.RecoveryServices.Backup cmdlet'leri kullanın

Bu makalede Azure PowerShell cmdlet'leri yedekleme ve kurtarma Hizmetleri Kasası'nı bir Azure sanal makinesini (VM) kurtarmak için nasıl kullanılacağı gösterilmektedir. Kurtarma Hizmetleri kasası bir Azure Resource Manager kaynaktır ve veri ve varlıkların Azure Backup ve Azure Site Recovery Services korumak için kullanılır. Azure Service Manager tarafından dağıtılan VM'ler ve Azure Resource Manager tarafından dağıtılan Vm'leri korumak için bir kurtarma Hizmetleri kasası kullanabilirsiniz.

> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager modeli kullanılarak oluşturulan VM ile birlikte kullanılır.
>
>

Bu makalede bir VM korumak ve verileri bir kurtarma noktasından geri yüklemek için PowerShell kullanılarak üzerinden anlatılmaktadır.

## <a name="concepts"></a>Kavramlar
Hizmeti genel bir bakış için Azure Backup hizmeti hakkında bilgi sahibi değilseniz kullanıma [Azure Backup nedir?](backup-introduction-to-azure-backup.md) Başlamadan önce Azure Backup ve geçerli VM yedekleme çözümü sınırlamaları ile çalışmak için gereken önkoşulları hakkında essentials kapak emin olun.

PowerShell etkili bir şekilde kullanmak için nesnelerin ve başlangıç nereden hiyerarşisini anlamak gereklidir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

AzureRm.RecoveryServices.Backup PowerShell cmdlet başvurusunun görüntülemek için bkz: [Azure Backup - kurtarma Hizmetleri cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) Azure Kitaplığı'nda.

## <a name="setup-and-registration"></a>Kurulumu'nu ve kaydı
Başlamak için:

1. [PowerShell en son sürümünü indirme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (gerekli en düşük sürüm: 1.4.0)
2. Azure yedekleme PowerShell cmdlet'leri kullanılabilir, aşağıdaki komutu yazarak bulabilirsiniz:

```
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
3. Azure oturum açma hesabı kullanarak **Login-AzureRmAccount**. Bu cmdlet getirir bir web sayfası için hesap kimlik bilgilerinizi ister: 
    - Alternatif olarak, bir parametresi olarak hesabı kimlik bilgilerinizi içerebilir **Login-AzureRmAccount** cmdlet'ini kullanarak **-kimlik bilgisi** parametresi.
    - Kiracı adına çalışma CSP ortağı varsa, müşteri, Tenantıd veya Kiracı birincil etki alanı adlarını kullanarak bir kiracı olarak belirtin. Örneğin: **Login-AzureRmAccount-Kiracı "fabrikam.com"**
4. Bir hesap birkaç abonelikleri olabileceği için hesap ile kullanmak istediğiniz aboneliği ilişkilendirin:

    ```
    PS C:\> Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup ilk kez kullanıyorsanız, kullanmalısınız  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  aboneliğinizle Azure Recovery hizmeti sağlayıcısını kaydetmek için cmdlet.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"
    ```

6. Aşağıdaki komutları kullanarak sağlayıcıları başarıyla kayıtlı doğrulayabilirsiniz:
    ```
    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace  "Microsoft.RecoveryServices"
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"
    ``` 
Komut çıktısında **RegistrationState** ayarlamalıdır **kayıtlı**. Aksi durumda, yeniden çalıştırmanız yeterlidir  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  yukarıda gösterilen cmdlet'i.

Aşağıdaki görevleri PowerShell ile otomatik olarak yapılabilir:

* Kurtarma Hizmetleri kasası oluşturma
* Azure VM'lerini yedekleme
* Bir yedekleme işi tetikleyeceğinden
* İzleyici bir yedekleme işi
* Bir Azure VM geri yükleme

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma
Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturmada size yol açar. Kurtarma Hizmetleri kasasına yedekleme Kasası ' farklıdır.

1. Kurtarma Hizmetleri kasası bir Resource Manager kaynak olduğundan, bir kaynak grubu içindeki yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanın veya bir kaynak grubu ile oluşturmak  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  cmdlet'i. Bir kaynak grubu oluştururken, ad ve kaynak grubu için konum belirtin.  

    ```
    PS C:\> New-AzureRmResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Kullanım  **[yeni AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  kurtarma Hizmetleri kasası oluşturmak için cmdlet'i. Kaynak grubu için kullanılan kasa için aynı konumu belirttiğinizden emin olun.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
3. Kullanılacak depolama artıklığı türünü belirtin; kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Aşağıdaki örnek, testvault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault -Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Çok sayıda Azure yedekleme cmdlet'lerini girdi olarak kurtarma Hizmetleri kasası nesnesi gerektirir. Bu nedenle, bir değişkende yedekleme kurtarma Hizmetleri kasası nesne depolamak uygundur.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüleyin
Kullanım  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  geçerli abonelikte tüm kasalarının listesini görüntülemek için. Bu komut, yeni bir kasa oluşturulduğunu denetleyin veya abonelik kullanılabilir kasalarında görmek için kullanabilirsiniz.

Abonelikteki tüm kasalarını görüntülemek için Get-AzureRmRecoveryServicesVault komutunu çalıştırın. Aşağıdaki örnekte, her kasa için görüntülenen bilgileri gösterir.

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
Sanal makinelerinizi korumak için bir kurtarma Hizmetleri kasası kullanın. Koruma uygulamadan önce set kasası bağlam (kasaya korumalı veri türü) ve koruma ilkesini doğrulayın. Koruma İlkesi yedekleme işleri çalıştırdığınızda, zamanlama ve her yedekleme anlık görüntüsünü ne kadar süreyle tutulduğunu ' dir.

### <a name="set-vault-context"></a>Set kasası bağlamı
Bir VM korumasını etkinleştirmeden önce kullanmak  **[kümesi AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  kasası bağlam ayarlamak için. Kasa bağlam ayarladıktan sonra sonraki tüm cmdlet'ler için geçerlidir. Aşağıdaki örnek, kasa için kasa bağlamı ayarlar *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Bir koruma ilkesi oluşturun
Kurtarma Hizmetleri kasası oluşturduğunuzda, varsayılan koruma ve bekletme ilkeleri ile birlikte gelir. Varsayılan koruma İlkesi, bir yedekleme işi her gün belirtilen zamanda tetikler. Varsayılan saklama İlkesi 30 gün boyunca günlük kurtarma noktası korur. Varsayılan ilke, VM hızlı bir şekilde korumak ve daha sonra farklı ayrıntılarla ilkesini düzenlemek için kullanabilirsiniz.

Kullanım  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  koruma ilkeleri kasaya görüntülemek için. Belirli bir ilke alma veya bir iş yükü türü ile ilişkili ilkeler görüntülemek için bu cmdlet'i kullanabilirsiniz. Aşağıdaki örnek iş yükü türünün AzureVM ilkelerini alır.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> PowerShell BackupTime alanın saat dilimi UTC değil. Ancak, Azure portalında yedekleme saati gösterildiğinde zaman, yerel saat dilimine ayarlanır.
>
>

En az bir bekletme ilkesiyle ilişkili bir yedekleme koruma ilkesidir. Bekletme İlkesi silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar. Kullanım  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  varsayılan bekletme ilkesini görüntülemek için.  Benzer şekilde kullanabilirsiniz  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  varsayılan zamanlama ilkesi elde edilir.  **[Yeni AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur. Zamanlama ve Bekletme İlkesi nesneleri giriş olarak kullanılan  **[yeni AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  cmdlet'i. Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesini değişkenleri depolar. Örnek, bir koruma ilkesi oluşturulurken parametreleri tanımlamak için bu değişkenleri kullanır. *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Korumayı etkinleştir
Yedekleme koruma İlkesi tanımladıktan sonra bir öğe için ilke hala etkinleştirmeniz gerekir. Kullanım  **[etkinleştir AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  korumasını etkinleştirmek için. Koruma etkinleştirme iki nesne - öğesini ve ilke gerektirir. İlke kasayla ilişkili silindikten sonra yedekleme iş akışı ilke zamanlama içinde tanımlı zaman tetiklenir.

Aşağıdaki örnek NewPolicy ilke kullanılarak koruma V2VM, öğe için etkinleştirir. Resource Manager vm'lerde şifrelenmemiş korumasını etkinleştirmek için

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

(BEK ve KEK kullanılarak şifrelenmiş) şifrelenmiş vm'lerinde korumayı etkinleştirmek için anahtarlar ve gizli anahtar Kasası'nı okuyun Azure Backup hizmeti izni vermeniz gerekir.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Şifrelenmiş (yalnızca BEK kullanılarak şifrelenmiş) VM'ler korumayı etkinleştirmek için gizli anahtar Kasası'nı okumak için Azure Backup hizmeti izni vermeniz gerekir.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Azure Bulutu kullanıyorsanız, parametre için değer ff281ffe-705c-4f53-9f37-a40e6f2c68f3 kullanmak **- ServicePrincipalName** içinde [kümesi AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet'i.
>
>

Klasik VM'ler için

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Bir koruma ilkesini değiştirme
Koruma ilkesini değiştirmek için kullanmak [kümesi AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) SchedulePolicy veya RetentionPolicy nesneleri değiştirmek için.

Aşağıdaki örnek, kurtarma noktası bekletme 365 gün olarak değiştirir.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Bir yedeklemeyi tetikleyin
Kullanabileceğiniz  **[yedekleme AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  yedekleme işini tetikleyecek. Bu ilk yedekleme ise, tam yedekleme olur. Sonraki yedek bir artımlı kopya alabilir. Kullandığınızdan emin olun  **[kümesi AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  yedekleme işini tetiklemeden önce kasası bağlam ayarlamak için. Aşağıdaki örnek, kasa bağlamını ayarlayın varsayar.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> PowerShell'de StartTime ve EndTime alanlarının saat dilimi UTC değil. Ancak, zaman zaman Azure portalında gösterildiğinde yerel saat dilimi olarak ayarlanır.
>
>

## <a name="monitoring-a-backup-job"></a>Bir yedekleme işi izleme
Azure portalını kullanmadan yedekleme işleri gibi uzun süre çalışan işlemleri izleyebilirsiniz. Devam eden işin durumunu almak için  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  cmdlet'i. Belirli bir kasa yönelik yedekleme işleri Bu cmdlet'i alır ve bu kasası kasası bağlamda belirtilir. Aşağıdaki örnek, bir dizi olarak devam eden işin durumunu alır ve durum $joblist değişkeninde depolar.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Bu işleri - gereksiz ek kod olan - tamamlanması için yoklama yerine kullanmak  **[bekleme AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet'i. Bu cmdlet yürütme iş tamamlandığında veya belirtilen zaman aşımı değerine ulaşılana kadar duraklar.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Bir Azure VM geri yükleme
Azure Portalı'nı kullanarak bir VM geri yükleme ve PowerShell kullanarak bir VM'i geri arasındaki en önemli fark yoktur. PowerShell ile diskleri ve yapılandırma bilgilerini kurtarma noktasından oluşturulduktan sonra geri yükleme işlemi tamamlanır. Geri yüklemek veya bir Azure VM yedekten birkaç dosyaları kurtarmak istiyorsanız, başvurmak [dosya kurtarma bölümü](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)

> [!NOTE]
> Geri yükleme işlemi, bir sanal makine oluşturmaz.
>
>

Diskten bir sanal makine oluşturmak için bölümüne bakın [saklı disklerden VM oluşturmak](backup-azure-vms-automation.md#create-a-vm-from-stored-disks). Bir Azure VM geri yüklemek için temel adımlar şunlardır:

* VM seçin
* Bir kurtarma noktası seçin
* Diskleri geri yükleme
* Saklı disklerden VM oluşturma

Aşağıdaki grafikte nesne hiyerarşisi BackupRecoveryPoint aşağıya doğru RecoveryServicesVault gelen gösterir.

![Kurtarma Hizmetleri nesne hiyerarşisi BackupContainer gösterme](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Yedekleme verilerini geri yüklemek için yedeklenen öğesi ve zaman içinde nokta verilerini tutan bir kurtarma noktası belirleyin. Kullanım  **[geri yükleme AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  verileri kasadan müşterinin hesabına geri yüklemek için cmdlet'i.

### <a name="select-the-vm"></a>VM seçin
Doğru yedekleme öğesi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı, şekilde çalışır. VM'yi temsil kapsayıcı seçmek için kullanın  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet'i ve için kanal  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet'i.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin
Kullanım  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  yedekleme öğesi için tüm kurtarma noktalarını listelemek için cmdlet. Daha sonra geri yüklemek için kurtarma noktası seçin. Kullanılacak olan kurtarma noktasını konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır AppConsistent noktası listesinde =.

Aşağıdaki komut, değişken **$rp**, seçili yedek öğesini, son yedi gün için kurtarma noktalarını dizisidir. Dizi en son kurtarma noktası dizin 0 konumunda ile süreyi ters sırada sıralanır. Standart PowerShell dizi dizin kurtarma noktası seçmek için kullanın. Örnekte, $rp [0], en son kurtarma noktası seçer.

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



### <a name="restore-the-disks"></a>Diskleri geri yükleme
Kullanım  **[geri yükleme AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  bir yedekleme öğesi'nin veri ve yapılandırmaları bir kurtarma noktasına geri yüklemek için cmdlet'i. Bir kurtarma noktası belirledikten sonra değeri olarak kullanın **- RecoveryPoint** parametresi. Önceki örnek kodda **$rp [0]** kullanılacak olan kurtarma noktasını oluştu. Aşağıdaki örnek kodda **$rp [0]** disk geri yüklemek için kullanılacak bir kurtarma noktası.

Diskleri ve yapılandırma bilgilerini geri yüklemek için:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Kullanım  **[bekleme AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  cmdlet'ini tamamlamak geri yükleme işi bekleyin.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Geri yükleme işi tamamlandıktan sonra kullanmak  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  geri yükleme işleminin ayrıntılarını almak için cmdlet. JobDetails özelliği VM yeniden oluşturmak için gereken bilgileri topladı.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Diskleri geri sonra VM'yi oluşturmak için sonraki bölüme gidin.

## <a name="create-a-vm-from-restored-disks"></a>Geri yüklenen disklerden bir VM oluşturma
Diskleri geri yükledikten sonra oluşturup diskten sanal makine yapılandırmak için aşağıdaki adımları kullanın.

> [!NOTE]
> Geri yüklenen disklerden şifrelenmiş VM'ler oluşturmak için Azure rolünüze eylemi gerçekleştirmek için izni olmalıdır **Microsoft.KeyVault/vaults/deploy/action**. Özel bir rol rolünüzün bu izne sahip değilse bu eylemle oluşturun. Daha fazla bilgi için bkz: [Azure rbac'de özel roller](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. İş ayrıntılarını geri yüklenen disk özelliklerini sorgu.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. Azure depolama bağlamını ayarlayın ve JSON yapılandırma dosyasını geri yükleyin.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. VM yapılandırması oluşturmak için JSON yapılandırma dosyası kullanın.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. İşletim sistemi diski ve veri diskleri ekleyin. Vm'leriniz yapılandırmasına bağlı olarak, ilgili cmdlet'ler görüntülemek için ilgili bölümüne bakın:

    #### <a name="non-managed-non-encrypted-vms"></a>Yönetilmeyen, şifrelenmemiş VM'ler

    Aşağıdaki örnek, yönetilmeyen, şifrelenmemiş VM'ler için kullanın.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Yönetilmeyen, şifrelenmiş VM'ler (yalnızca BEK)

    (Yalnızca BEK kullanılarak şifrelenmiş) yönetilmeyen, şifrelenmiş VM'ler için diskler ekleyebilmek için gizli anahtar Kasası'na geri yüklemeniz gerekir. Daha fazla bilgi için lütfen makalesine bakın [şifrelenmiş bir sanal makine bir Azure yedekleme kurtarma noktasından geri](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifrelenmiş VM'ler için işletim sistemi ve veri diskleri ekleme gösterilmektedir.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Yönetilmeyen, şifrelenmiş VM'ler (BEK ve KEK)

    (BEK ve KEK kullanılarak şifrelenmiş) yönetilmeyen, şifrelenmiş VM'ler için diskleri eklemeden önce anahtarı ve gizli anahtar Kasası'na geri yüklemeniz gerekir. Daha fazla bilgi için lütfen makalesine bakın [şifrelenmiş bir sanal makine bir Azure yedekleme kurtarma noktasından geri](backup-azure-restore-key-secret.md). Aşağıdaki örnek, şifrelenmiş VM'ler için işletim sistemi ve veri diskleri ekleme gösterilmektedir.

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

    #### <a name="managed-non-encrypted-vms"></a>Yönetilen, şifrelenmemiş VM'ler

    Yönetilen şifrelenmemiş VM'ler için blob depolama alanından yönetilen diskleri oluşturma ve diskleri bağlamanız gerekir. Ayrıntılı bilgi için makalesine bakın [bir veri diski bir Windows PowerShell kullanarak VM'e](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod, yönetilen şifrelenmemiş VM'ler için veri diski ekleme gösterilmektedir.

    ```
    PS C:\> $storageType = "StandardLRS"
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

    #### <a name="managed-encrypted-vms-bek-only"></a>Yönetilen, şifrelenmiş VM'ler (yalnızca BEK)

    (Yalnızca BEK kullanılarak şifrelenmiş) yönetilen şifrelenmiş VM'ler için blob depolama alanından yönetilen diskleri oluşturma ve diskleri bağlamanız gerekir. Ayrıntılı bilgi için makalesine bakın [bir veri diski bir Windows PowerShell kullanarak VM'e](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod, yönetilen şifrelenmiş VM'ler için veri diski ekleme gösterilmektedir.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Yönetilen, şifrelenmiş VM'ler (BEK ve KEK)

    (BEK ve KEK kullanılarak şifrelenmiş) yönetilen şifrelenmiş VM'ler için blob depolama alanından yönetilen diskleri oluşturma ve diskleri bağlamanız gerekir. Ayrıntılı bilgi için makalesine bakın [bir veri diski bir Windows PowerShell kullanarak VM'e](../virtual-machines/windows/attach-disk-ps.md). Aşağıdaki örnek kod, yönetilen şifrelenmiş VM'ler için veri diski ekleme gösterilmektedir.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
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

5. Ağ ayarları ayarlayın.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Sanal makineyi oluşturun.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="restore-files-from-an-azure-vm-backup"></a>Dosyaları bir Azure VM yedekten geri yükleyin

Diskleri geri ek olarak, bir Azure VM yedekten tek dosyalar da geri yükleyebilirsiniz. Geri yükleme dosyaları işlevselliğini bir kurtarma noktası tüm dosyalarda erişmelerini sağlar ve normal dosyalar için yaptığınız gibi bunları dosya Gezgini yönetebilirsiniz.

Bir dosyayı Azure VM yedekten geri yüklemek için temel adımlar şunlardır:

* VM seçin
* Bir kurtarma noktası seçin
* Kurtarma noktası disklerinin bağlama
* Gerekli dosyaları kopyalama
* Disk çıkarın


### <a name="select-the-vm"></a>VM seçin
Doğru yedekleme öğesi tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcısından başlatın ve nesne hiyerarşide aşağı, şekilde çalışır. VM'yi temsil kapsayıcı seçmek için kullanın  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  cmdlet'i ve için kanal  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  cmdlet'i.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Bir kurtarma noktası seçin
Kullanım  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  yedekleme öğesi için tüm kurtarma noktalarını listelemek için cmdlet. Daha sonra geri yüklemek için kurtarma noktası seçin. Kullanılacak olan kurtarma noktasını konusunda emin değilseniz, en son RecoveryPointType seçmek için iyi bir uygulamadır AppConsistent noktası listesinde =.

Aşağıdaki komut, değişken **$rp**, seçili yedek öğesini, son yedi gün için kurtarma noktalarını dizisidir. Dizi en son kurtarma noktası dizin 0 konumunda ile süreyi ters sırada sıralanır. Standart PowerShell dizi dizin kurtarma noktası seçmek için kullanın. Örnekte, $rp [0], en son kurtarma noktası seçer.

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

### <a name="mount-the-disks-of-recovery-point"></a>Kurtarma noktası disklerinin bağlama

Kullanım  **[Get-AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprpmountscript)**  cmdlet'ini kurtarma noktası tüm diskleri bağlamak için betik alın.

> [!NOTE]
> Diskler, komut dosyası çalıştırdığı makineye iSCSI bağlı diskleri olarak bağlanır. Bu nedenle neredeyse anında ve herhangi bir ücret doğurur değil
>
>

```
PS C:\> Get-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]

OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```
Komut dosyaları kurtarmak istediğiniz makinede çalıştırın. Komut dosyası yürütme, yukarıda gösterilen parola girmeniz gerekir. Diskleri ekli sonra Windows dosya Gezgini yeni birimleri ve dosyaları göz atmak için kullanın. Daha fazla bilgi için bkz [dosya kurtarma belgeleri](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>Diskleri çıkarın
Gerekli dosyaları kopyaladıktan sonra diskleri kullanarak dosyayı çıkarın  **[devre dışı bırakma AzureRmRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/disable-azurermrecoveryservicesbackuprpmountscript?view=azurermps-5.0.0)**  cmdlet'i. Kurtarma noktası dosyalara erişim kaldırıldığından emin getirir bu özellikle önerilir

```
PS C:\> Disable-AzureRmRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0]
```


## <a name="next-steps"></a>Sonraki adımlar
Azure kaynaklarınızı bulunmaya PowerShell kullanmayı tercih ederseniz, PowerShell makalesine bakın. [dağıtma ve yönetme yedekleme için Windows Server](backup-client-automation.md). DPM yedeklemelerini yönetiyorsanız makalesine bakın [dağıtma ve yönetme yedekleme DPM için](backup-dpm-automation.md). Bu makaleler her ikisi de Resource Manager dağıtımları ve klasik dağıtımları için bir sürümüne sahipsiniz.  
