---
title: Yedekleme ve Azure Backup ve PowerShell kullanarak Azure dosyaları geri yükleme
description: Yedekleme ve Azure Backup ve PowerShell kullanarak Azure dosyaları geri yükleme.
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: pullabhk
ms.openlocfilehash: 986414d0bac24d0c7e37b34df473346742fa97fd
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204191"
---
# <a name="back-up-and-restore-azure-files-with-powershell"></a>Yedekleme ve PowerShell ile Azure dosyaları geri yükleme

Bu makalede, yedekleme ve bir Azure dosyaları dosya paylaşımı kullanarak kurtarmak için Azure PowerShell kullanmayı açıklar bir [Azure Backup](backup-overview.md) kurtarma Hizmetleri kasası. 

Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
> * PowerShell'i ayarlayın ve Azure kurtarma Hizmetleri sağlayıcısını kaydedin.
> * Kurtarma Hizmetleri kasası oluşturun.
> * Bir Azure dosya paylaşımına yönelik yedeklemeyi yapılandırın.
> * Bir yedekleme işi çalıştırın.
> * Yedeklenen bir Azure dosya paylaşımı veya ayrı bir dosya paylaşımından geri yükleyin.
> * Yedeklemeyi izleme ve geri yükleme işleri.


## <a name="before-you-start"></a>Başlamadan önce

- [Daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md) kurtarma Hizmetleri kasası hakkında.
- Önizleme özelliklerini okuyun [Azure dosya paylaşımlarının desteklenmesi](backup-azure-files.md).
- Kurtarma Hizmetleri için PowerShell nesne hiyerarşisi gözden geçirin.


## <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Gözden geçirme **Az.RecoveryServices** [cmdlet başvurusu](/powershell/module/az.recoveryservices) Azure Kitaplığı Başvurusu.


## <a name="set-up-and-install"></a>Ayarlama ve yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i aşağıdaki gibi ayarlayın:

1. [Az PowerShell'in en son sürümünü indirin](/powershell/azure/install-az-ps). Gerekli en düşük sürüm 1.0.0 ' dir.

2. Bu komut Azure Backup PowerShell cmdlet'leriyle bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
3. Diğer adlar gözden geçirin ve Azure Backup, Azure Site Recovery ve kurtarma Hizmetleri kasası için cmdlet'leri görünür. Ne görebileceğinize ilişkin bir örnek aşağıda verilmiştir. Cmdlet öğelerinin tam bir listesi değil.

    ![Kurtarma Hizmetleri cmdlet'lerinin listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Azure hesabınızda oturum açın **Connect AzAccount**.
4. Web görünen sayfasında hesabı kimlik bilgilerinizle giriş istenir.

    - Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'iyle **-Credential**.
    - Kiracı adına çalışan bir CSP iş ortağı olduğunuzdan, müşteri, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Bir örnek **Connect AzAccount-Kiracı** fabrikam.com.

4. Hesabınız birden fazla abonelik olabilir çünkü hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup'ı ilk kez kullanıyorsanız, kullanın **Register-AzResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Sağlayıcılar başarıyla kaydedildiğini doğrulayın:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
7. Komut çıktısında, doğrulayın **RegistrationState** değişikliklerini **kayıtlı**. Bu çalışmazsa **Register-AzResourceProvider** cmdlet'ini yeniden.



## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Bir kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

- Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanabilir veya bir kaynak grubu ile oluşturabileceğiniz **yeni AzResourceGroup** cmdlet'i. Bir kaynak grubu oluşturduğunuzda, kaynak grubunun konumunu ve adını belirtin. 

1. Bir kasayı bir kaynak grubuna yerleştirilir. Mevcut bir kaynak grubu, yeni bir yoksa [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0). Bu örnekte, Batı ABD bölgesinde yeni bir kaynak grubu oluştururuz.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```
2. Kullanım [yeni AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) kasası oluşturmak için cmdlet'i. Kasayla aynı konumda kaynak grubu için kullanılan belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Kasa depolamak için kullanılacak yedeklilik türünü belirtin.

   - Kullanabileceğiniz [yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md).
   - Aşağıdaki örnek kümeleri **- BackupStorageRedundancy** seçeneğini[kümesi AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) için cmd **testvault** kümesine  **GeoRedundant**.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Tüm kasaları abonelikte görüntülemek için kullanın [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0).

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. İlişkili kaynak grubunu ve konumu verildiğini unutmayın.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlayın

Kasasının bir değişkende Store ve kasa bağlamını ayarlayın.

- Kasasının bir değişkende depolanması uygundur bunu çoğu Azure Backup cmdlet'i, girdi olarak kurtarma Hizmetleri kasasını gerektirir.
- Kasa bağlamı, kasada korunan veri türüdür. İle ayarlayın [kümesi AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0). Bağlamı ayarlandıktan sonra sonraki tüm cmdlet'ler için geçerlidir.


Aşağıdaki örnek için kasa bağlamını ayarlar **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Getirilemedi vault kimliği

Azure PowerShell yönergelerine uygun olarak ayarlamak kasa bağlamını kullanım dışı bırakılıyor planlıyoruz. Bunun yerine, saklamak veya kasa kimliği getirin ve ilgili komutları gibi geçirin:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma

Bir yedekleme İlkesi yedeklemeler için zamanlamayı belirtir ve ne kadar süreyle yedekleme kurtarma noktaları tutulması gereken:

- Bir yedekleme ilkesi en az bir bekletme ilkesi ile ilişkilidir. Bir bekletme ilkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar.
- Varsayılan yedekleme İlkesi bekletme kullanarak Görünüm [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0).
- Varsayılan yedekleme İlkesi zamanlamayı kullanarak Görünüm [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0).
-  Kullandığınız [yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet'i yeni bir yedekleme ilkesi oluşturmak için. Zamanlama ve Bekletme İlkesi nesneleri girdiğiniz.

Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Bu değişken parametre olarak için yeni bir ilke kullanır (**NewAFSPolicy**). **NewAFSPolicy** günlük bir yedekleme alır ve 30 gün boyunca tutar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdakine benzer.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```



## <a name="enable-backup"></a>Yedeklemeyi etkinleştir

Yedekleme İlkesi tanımladıktan sonra ilke kullanarak Azure dosya paylaşımı için korumayı etkinleştirebilirsiniz.

### <a name="retrieve-a-backup-policy"></a>Bir yedekleme ilkesi alma

İlgili ilke nesnesi ile fetch [Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0). Bu cmdlet, belirli bir ilke alma ya da bir iş yükü türü ile ilişkili ilkeleri görüntülemek için kullanın.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>Bir iş yükü türü için bir ilke alma

Aşağıdaki örnek, iş yükü türü için ilkeleri alır. **depolamasını Azure dosyalarına**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Çıktı aşağıdakine benzer.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```
> [!NOTE]
> Saat dilimini **BackupTime** alandır PowerShell'de Eşgüdümlü Evrensel Saat (UTC). Yedekleme zamanını Azure portalında göründüğü zaman için yerel saat diliminizde ayarlanır.

### <a name="retrieve-a-specific-policy"></a>Belirli bir ilke alma

Adlı bir yedekleme İlkesi şu ilkeyi alır **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Yedeklemeyi etkinleştir ve ilke uygulayın

Koruma etkinleştirme [etkinleştir AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). İlke, kasayla ilişkili sonra yedekleme İlkesi zamanlamaya uygun olarak tetiklenir.

Aşağıdaki örnek, Azure dosya paylaşımının korumasını etkinleştirir **testAzureFileShare** depolama hesabındaki **testStorageAcct**, ilkeyle **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Yapılandırma koruma işi tamamlandı ve benzer bir çıktı verir komutu gösterildiği gibi bekler.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı bir yedeklemeyi tetikleyin

Kullanım [yedekleme AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) korumalı Azure dosya paylaşımı için bir isteğe bağlı yedekleme çalıştırmak için.

1. Dosya ve depolama hesabı, yedekleme verilerinizi tutan kasadaki kapsayıcıdan paylaşmak alma [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer).
2. Bir yedekleme işi başlatmak için VM hakkında bilgi edinmek [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem).
3. İle bir isteğe bağlı yedekleme çalıştırmak[yedekleme AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem).

İsteğe bağlı yedekleme aşağıdaki gibi çalıştırın:
    
```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Aşağıdaki örnekte gösterildiği gibi komut izlenmesi için bir kimlik ile bir iş döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Azure dosya paylaşımı anlık görüntüleri kullanılan yedeklemeleri alınır, ancak komut şu çıktıyı döndürür zamanında bu nedenle genellikle işi tamamlar.

### <a name="modify-the-protection-policy"></a>Koruma ilkesini değiştirme

Azure dosya paylaşımını yedekleme için kullanılan ilkeyi değiştirmek için kullanın [etkinleştir AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0). İlgili yedekleme öğesi ve yeni bir yedekleme İlkesi belirtin.

Aşağıdaki örnek değişiklikleri **testAzureFS** protection ilkesinden **dailyafs** için **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-files"></a>Azure dosya paylaşımlarının ve dosyaların geri yükleme

Dosya paylaşımının tamamını veya belirli bir dosya paylaşımına geri yükleyebilirsiniz. Özgün konuma veya alternatif bir konuma geri yükleyebilirsiniz. 

### <a name="fetch-recovery-points"></a>Getirme kurtarma noktaları

Kullanım [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) yedeklenen öğe için tüm kurtarma noktalarını listelemek için.

Aşağıdaki komut dosyasında:

- Değişken **$rp** son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir.
- Dizi en son kurtarma noktası dizinindeki ile zaman ters sırada sıralanır **0**.
- Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın.
- Örnekte, **$rp [0]** en son kurtarma noktası seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Çıktı aşağıdakine benzer.

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```
İlgili kurtarma noktası seçtikten sonra dosya paylaşımı veya dosya özgün konuma veya alternatif bir konuma geri.

### <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Azure dosya paylaşımını başka bir konuma geri yükleme

Kullanım [geri yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) seçilen kurtarma noktasına geri yüklemek için. Alternatif konumu belirlemek için şu parametreleri belirtin: 

- **TargetStorageAccountName**: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
- **TargetFileShareName**: Hedef depolama içindeki dosya paylaşımları, yedeklenen içeriğin geri için hesap.
- **TargetFolder**: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi, hedef klasör değerler boş bir dize verin.
- **ResolveConflict**: Geri yüklenen verilerle bir çakışma varsa yönerge. Kabul **üzerine** veya **atla**.

Cmdlet parametreleri aşağıdaki gibi çalıştırın:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

Aşağıdaki örnekte gösterildiği gibi komut izlenmesi için bir kimlik ile bir iş döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

### <a name="restore-an-azure-file-to-an-alternate-location"></a>Bir Azure dosya alternatif bir konuma geri yükleme

Kullanım [geri yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) seçilen kurtarma noktasına geri yüklemek için. Alternatif konumu tanımlamak ve geri yüklemek istediğiniz dosyayı benzersiz olarak tanımlanabilmesi için bu parametreleri belirtin.

* **TargetStorageAccountName**: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
* **TargetFileShareName**: Hedef depolama içindeki dosya paylaşımları, yedeklenen içeriğin geri için hesap.
* **TargetFolder**: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi, hedef klasör değerler boş bir dize verin.
* **SourceFilePath**: Dize olarak dosya paylaşımı içinde geri yüklenecek dosyanın mutlak yolu. Bu yol, kullanılan aynı yoldur **Get-AzStorageFile** PowerShell cmdlet'i.
* **SourceFileType**: Bir dizin olup olmadığını veya dosya seçilmedi. Kabul **dizin** veya **dosya**.
* **ResolveConflict**: Geri yüklenen verilerle bir çakışma varsa yönerge. Kabul **üzerine** veya **atla**.

Ek parametreler (SourceFilePath ve SourceFileType) geri yüklemek istediğiniz yalnızca bireysel dosyasına ilgilidir.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Bu komut, önceki bölümde gösterildiği gibi izlenmesi için bir kimlik ile bir iş döndürür.

### <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Azure dosya paylaşımlarının ve dosyaların özgün konuma geri yükleme

Özgün bir konuma geri yüklediğinizde, hedef ve hedef ilgili parametreleri belirtmeniz gerekmez. Yalnızca **ResolveConflict** sağlanmalıdır.

#### <a name="overwrite-an-azure-file-share"></a>Bir Azure dosya paylaşımı üzerine yaz

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Bir Azure dosya üzerine yazma

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Yedekleme izleme ve geri yükleme işleri

İsteğe bağlı yedekleme ve geri yükleme işlemlerini döndürüp bir kimliği ile birlikte bir iş zaman gösterildiği gibi [isteğe bağlı yedekleme çalıştırıldı](#trigger-an-on-demand-backup). Kullanım [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) ayrıntıları ve iş ilerleme durumunu izlemek için cmdlet'i.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```
## <a name="next-steps"></a>Sonraki adımlar
[Hakkında bilgi edinin](backup-azure-files.md) Azure portalında Azure dosyaları yedekleniyor.
