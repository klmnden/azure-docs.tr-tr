---
title: Dağıtmak ve yönetmek için PowerShell kullanarak Azure dosya paylaşımlarını yedekleme
description: Azure dosya paylaşımları için azure'da yedeklemelerini yönetme ve dağıtma için PowerShell kullanma
services: backup
author: pvrk
manager: shivamg
keywords: PowersShell; Azure dosyaları yedeklemesi; Azure dosyaları geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 30fc36f29a7602e2bc3f192b445474bfc50e9434
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53632644"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Yedekleme ve geri yükleme Azure dosya paylaşımları için PowerShell kullanma

Bu makalede, Azure PowerShell cmdlet'leri yedekleme ve kurtarma Hizmetleri kasasından Azure dosya paylaşımının kurtarma için nasıl kullanılacağını gösterir. Kurtarma Hizmetleri kasası, verilere ve varlıklara Azure Backup ve Azure Site Recovery hizmetlerinde korumak için kullanılan bir Azure Resource Manager kaynağıdır.

## <a name="concepts"></a>Kavramlar

Hizmetine genel bakış için Azure Backup hizmeti ile aşina değilseniz bkz [Azure Backup nedir?](backup-introduction-to-azure-backup.md). Başlamadan önce belgelenen Azure dosya paylaşımlarını yedekleme Önizleme özellikleri not alın olun [burada](backup-azure-files.md).

PowerShell etkili bir şekilde kullanmak için nesnelerin ve nereden başlayacağınızı hiyerarşi anlamak gereklidir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

AzureRm.RecoveryServices.Backup PowerShell cmdlet başvurusu görüntülemek için bkz: [Azure Backup - kurtarma Hizmetleri cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) Azure Kitaplığı'nda.

## <a name="setup-and-registration"></a>Kurulumu ve kaydı

> [!NOTE]
> Belirtildiği gibi [burada](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.13.0), AzureRM modülü biter Kasım 2018'de yeni özellikler için destek. Bu nedenle Azure dosya paylaşımlarını yedekleme desteği ile yeni 'Az' PS modülüne artık genel kullanıma sunulana sağlıyoruz

Başlamak için:

1. ['Az' PowerShell'in en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0) (gerekli en düşük sürüm: 1.0.0)

2. Kullanılabilir Azure Backup PowerShell cmdlet'lerini, aşağıdaki komutu yazarak bulabilirsiniz:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Azure Backup, Azure Site Recovery ve kurtarma Hizmetleri kasası için cmdlet'leri ve diğer adları görüntülenir. Aşağıdaki görüntüde, görürsünüz, bir örnektir. Cmdlet öğelerinin tam listesi değil.

    ![Kurtarma Hizmetleri listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Azure oturum açma hesabı kullanarak **Connect AzAccount**. Bu cmdlet getirir, bir web sayfası için hesap kimlik bilgilerinizi ister:

    * Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    * Müşteri, Kiracı adına çalışma CSP iş ortağıysanız, Kiracı kimliği veya Kiracı birincil etki alanı adlarını kullanarak Kiracı olarak belirtin. Örneğin: **Connect AzAccount-Kiracı "fabrikam.com"**

4. Hesabınız birden fazla abonelik olabilir bu yana hesabı ile kullanmak istediğiniz aboneliği ilişkilendirin:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup'ı ilk kez kullanıyorsanız, kullanmalısınız **Register-AzResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Aşağıdaki komutları kullanarak sağlayıcılar başarıyla kayıtlı doğrulayabilirsiniz:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    Komut çıktısında **RegistrationState** değiştirilmelidir **kayıtlı**. Değil, yalnızca çalıştırırsanız **Register-AzResourceProvider** cmdlet'ini yeniden.

PowerShell ile aşağıdaki görevleri otomatik hale getirilebilir:

* Kurtarma Hizmetleri kasası oluşturma
* Azure dosya paylaşımları için yedeklemeyi yapılandırma
* Bir yedekleme işi tetikleme
* Bir yedekleme işini izleme
* Bir Azure dosya paylaşımını geri yükleme
* Tek bir Azure dosya bir Azure dosya paylaşımından geri yükleme

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturma işleminde yol.

1. Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanın veya bir kaynak grubu oluşturun **yeni AzResourceGroup** cmdlet'i. Bir kaynak grubu oluştururken, kaynak grubunun konumunu ve adını belirtin.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Kullanım **yeni AzRecoveryServicesVault** cmdlet'ini kurtarma Hizmetleri kasası oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirttiğinizden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Kullanılacak depolama yedekliliği türü; belirtin. kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnek, testvault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Tüm kasaları abonelikte görüntülemek için kullanın **Get-AzRecoveryServicesVault**:

```powershell
Get-AzRecoveryServicesVault
```

Aşağıdaki örneğe benzer bir çıkış, ilişkili ResourceGroupName ve konum sağlanan dikkat edin.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir.

Kullanım **kümesi AzRecoveryServicesVaultContext** kasa bağlamını ayarlamak için. Kasa bağlamı ayarlandıktan sonra, sonraki tüm cmdlet’ler için geçerli olur. Aşağıdaki örnek, kasa için kasa bağlamını ayarlar *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Azure PowerShell yönergelerine göre kasa bağlamını ayarını kullanımdan kaldırmaya planlıyorsunuz. Bunun yerine kullanıcılar aşağıda belirtildiği gibi kasa kimliği geçirilecek öneririz

Alternatif olarak, depolama/PowerShell işlemi gerçekleştirmek ve ilgili komutu iletmek istediğiniz kasaya kimliği getirme.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Bir Azure dosya paylaşımı için yedeklemeyi yapılandırma

### <a name="create-protection-policy"></a>Koruma ilkesi oluşturma

Bir yedekleme koruma İlkesi, en az bir bekletme ilkesi ile ilişkilidir. Bekletme İlkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar. Kullanım **Get-AzRecoveryServicesBackupRetentionPolicyObject** varsayılan saklama ilkesini görüntülemek için.  Benzer şekilde kullanabileceğiniz **Get-AzRecoveryServicesBackupSchedulePolicyObject** varsayılan zamanlama ilkesini elde edilir. **Yeni AzRecoveryServicesBackupProtectionPolicy** cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur. Zamanlama ve Bekletme İlkesi nesneleri girdi olarak kullanılan **yeni AzRecoveryServicesBackupProtectionPolicy** cmdlet'i. Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Örnek, bir koruma ilkesi oluşturulurken parametreler tanımlamak için bu değişkenleri kullanır. *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdaki örneğe benzer:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

'NewAFSPolicy' günlük yedekleme gerçekleştirir ve 30 gün boyunca saklar.

### <a name="enable-protection"></a>Korumayı etkinleştir

Koruma İlkesi tanımladınız sonra bu ilke ile Azure dosya paylaşımı için korumayı etkinleştirebilirsiniz.

İlk ilgili ilke nesnesi ile fetch **Get-AzRecoveryServicesBackupProtectionPolicy** cmdlet'i. Belirli bir ilke alma ya da bir iş yükü türü ile ilişkili ilkeleri görüntülemek için bu cmdlet'i kullanabilirsiniz.

Aşağıdaki örnek, iş yükü türü için depolamasını Azure dosyalarına ilkeleri alır.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Çıktı aşağıdaki örneğe benzer:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> UTC saat dilimi PowerShell BackupTime alanın olur. Ancak, yedekleme zamanını Azure portalında göründüğü zaman, yerel saat dilimine ayarlanır.
>
>

Aşağıdaki ilke "dailyafs" adlı yedekleme ilkesini alır.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Kullanım **etkinleştir AzRecoveryServicesBackupProtection** öğenin belirtilen İlkesi ile korumayı etkinleştirmek için. İlke kasa ile ilişkilendirildikten sonra yedekleme iş akışı içinde ilke zamanlaması tanımlı zaman tetiklenir.

Aşağıdaki örnek, "testAzureFileShare", depolama hesabı "testStorageAcct", altında ilke dailyafs ile Azure dosya paylaşımının korumasını etkinleştirir.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Komutu, yapılandırma koruma işi tamamlandıktan ve aşağıda gösterildiği gibi benzer bir çıktı sağlar kadar bekler.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı bir yedeklemeyi tetikleyin

Kullanım **yedekleme AzRecoveryServicesBackupItem** korumalı Azure dosya paylaşımı için yedekleme işini tetiklemek için. Aşağıdaki komutları kullanarak içinde depolama hesabı ve dosya paylaşımı almak ve isteğe bağlı yedekleme tetikleyin.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Komut aşağıdaki örnekteki gibi bir Kimliğe sahip izlenecek bir işin döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Yedeklemeleri almaya çalışırken size Azure dosya paylaşımı anlık görüntüleri yararlanıyor ve bu nedenle genellikle iş komut şu çıktıyı döndürür zamanında tamamlar

### <a name="modify-protection-policy"></a>Koruma ilkesini değiştirme

İlke ile değiştirmek istiyorsanız, Azure dosya paylaşımının korunur, kullanın **etkinleştir AzRecoveryServicesBackupProtection** ilgili yedekleme öğesi ve yeni koruma ilkesine sahip.

Aşağıdaki örnekler "monthlyafs" için "dailyafs" öğesinden "testAzureFS" koruma İlkesi değişiklikleri

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-sharesazure-files"></a>Azure dosya paylaşımlar geri yükleme / Azure dosyaları

Dosya paylaşımının tamamını özgün veya alternatif bir konuma geri yükleyebilirsiniz. Benzer şekilde dosya paylaşımı tek tek dosyaların de geri yüklenebilir.

### <a name="fetching-recovery-points"></a>Kurtarma noktaları getiriliyor

Kullanım **Get-AzRecoveryServicesBackupRecoveryPoint** yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Aşağıdaki komut, değişken **$rp**, son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi dizin 0 konumunda en son kurtarma noktası ile zaman ters sırada sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, en son kurtarma noktası $rp [0] seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Çıktı aşağıdaki örneğe benzer:

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

İlgili kurtarma noktası seçtikten sonra dosya paylaşımı/dosya aşağıda açıklandığı gibi bir alternatif/özgün konuma geri yüklemek için devam edin.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Azure dosya paylaşımlarını alternatif bir konuma geri yükleme

#### <a name="restoring-an-azure-file-share"></a>Azure dosya paylaşımını geri yükleme

Alternatif bir konuma aşağıdaki bilgileri sağlayarak tanımlayın:

* ***TargetStorageAccountName***: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
* ***TargetFileShareName***: Yedeklenen içeriğin geri yüklenir hedef depolama hesabında dosya paylaşımları
* ***TargetFolder***: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi gerekiyorsa, hedef klasör değerleri, boş dize olarak verin.
* ***ResolveConflict***: Yönerge geri yüklenen verilerle bir çakışma olması durumunda. "Üzerine yaz" veya "atlanmaya" kabul eder.

Yedeklenen bir alternatif bir konuma dosya paylaşımını yedekleme geri yüklemek için restore komutu bu parametreleri belirtin.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

Komut bir iş aşağıdaki örnekte gösterildiği gibi izlenmesi kimliği döndürür.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restoring-an-azure-file"></a>Bir Azure dosya geri yükleme

Tüm bir dosya paylaşımı yerine tek bir dosyayı geri yüklemek istediğiniz durumlarda, tek tek dosya aşağıdaki parametreleri sağlayarak benzersiz olarak tanımlanmalıdır.

* ***TargetStorageAccountName***: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
* ***TargetFileShareName***: Yedeklenen içeriğin geri yüklenir hedef depolama hesabında dosya paylaşımları
* ***TargetFolder***: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi gerekiyorsa, hedef klasör değerleri, boş dize olarak verin.
* ***SourceFilePath***: Dize olarak dosya paylaşımı içinde geri yüklenecek dosyanın mutlak yolu. Kullanılan aynı yol budur ```Get-AzStorageFile``` PS cmdlet'i.
* ***SourceFileType***: Bir dizin olup olmadığını veya dosya seçilmedi. "Dizin" veya "Dosya" kabul eder.
* ***ResolveConflict***: Yönerge geri yüklenen verilerle bir çakışma olması durumunda. "Üzerine yaz" veya "atlanmaya" kabul eder.

Gördüğünüz gibi ek parametreler yalnızca bireysel dosyasına geri ilgilidir.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Ayrıca, yukarıda gösterildiği gibi bir kimlik ile izlenecek bir işin da döndürür.

### <a name="restore-azure-file-shares-to-original-location"></a>Azure dosya paylaşımlarını özgün konuma geri yükleme

Özgün konuma geri yükleme durumunda, tüm hedef/hedef parametreleri belirtilmedi ilgili. Yalnızca ```ResolveConflict``` sağlanmalıdır

#### <a name="overwrite-an-azure-file-share"></a>Bir Azure dosya paylaşımı üzerine yaz

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Bir Azure dosya üzerine yazma

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Yedekleme izleme ve geri yükleme işleri

İsteğe bağlı yedekleme ve geri yükleme işlemlerini döndürüp Kimliğiniz ile birlikte bir iş gösterildiği [yukarıda](#trigger-an-on-demand-backup). Kullanım ```Get-AzRecoveryServicesBackupJobDetails``` işinin ilerleme durumunu izlemek ve daha fazla ayrıntı getirmek için komutu.

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
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support
```
