---
title: PowerShell kullanarak Azure dosya paylaşımları için yedeklemeler yönetme ve dağıtma
description: Azure dosya paylaşımları için azure'da yedeklemelerini yönetme ve dağıtma için PowerShell kullanma
services: backup
author: pvrk
manager: shivamg
keywords: PowerShell; Azure dosyaları yedeklemesi; Azure dosyaları geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 4ead84ef415dcb85682b15414380055d8799b54c
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54051229"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Yedekleme ve geri yükleme Azure dosya paylaşımları için PowerShell kullanma

Bu makalede, Azure PowerShell cmdlet'leri yedekleme ve kurtarma Hizmetleri kasasından Azure dosya paylaşımının kurtarma için nasıl kullanılacağını gösterir. Kurtarma Hizmetleri kasası ve Azure Backup ve Azure Site Recovery varlıklarını korumak için kullanılan bir Azure Resource Manager kaynağıdır.

## <a name="concepts"></a>Kavramlar

Azure Backup ile tanıdık hizmetine genel bakış için değilseniz bkz [Azure Backup nedir?](backup-introduction-to-azure-backup.md). Başlamadan önce dosya paylaşımları, Azure yedekleme için kullanılan Önizleme özellikleri görmek [Azure yedekleme, dosya paylaşımları](backup-azure-files.md).

PowerShell etkili bir şekilde kullanmak için nesneleri ve başlangıç uygulanacağı hiyerarşisini anlamak gereklidir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Görüntülenecek **AzureRm.RecoveryServices.Backup** PowerShell cmdlet başvurusu bkz [Azure Backup - kurtarma Hizmetleri cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) Azure Kitaplığı'nda.

## <a name="setup-and-registration"></a>Kurulumu ve kaydı

> [!NOTE]
> Belirtilen [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.13.0), AzureRM modülü biten Kasım 2018'de yeni özelliklerine yönelik destek. Genel kullanıma sunulmuştur yeni Az PowerShell modülü ile Azure dosya paylaşımlarının yedeklenmesi için destek sağlanır.

Başlamak için aşağıdaki adımları izleyin.

1. [Az PowerShell'in en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0). Gerekli en düşük sürüm 1.0.0 ' dir.

2. Bulma **Azure Backup PowerShell** cmdlet'lerini aşağıdaki komutu girerek kullanılabilir.

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Azure Backup, Azure Site Recovery ve kurtarma Hizmetleri kasası için cmdlet'leri ve diğer adları görüntülenir. Aşağıdaki görüntüde gördüğünüz bir örnektir. Cmdlet öğelerinin tam listesi değil.

    ![Kurtarma Hizmetleri cmdlet'lerinin listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Azure hesabınızı kullanarak oturum açın **Connect AzAccount**. Bu cmdlet için hesap kimlik bilgilerinizi isteyen bir web sayfası getirir:

    * Alternatif olarak, bir parametre olarak bir hesap kimlik bilgilerinizi içerebilir **Connect AzAccount** cmdlet'ini kullanarak **-Credential** parametresi.
    * Kiracı adına çalışan bir CSP iş ortağı olduğunuzdan, müşteri kendi Kiracı kimliği veya Kiracı birincil etki alanı adını kullanarak Kiracı olarak belirtin. Bir örnek **Connect AzAccount-Kiracı** fabrikam.com.

4. Hesabınız birden fazla abonelik olabilir çünkü hesapla kullanmak istediğiniz aboneliği ilişkilendirin.

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Backup'ı ilk kez kullanıyorsanız, kullanın **Register-AzResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Aşağıdaki komutu kullanarak sağlayıcılar başarıyla kaydedildiğini doğrulayın.
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    Komut çıktısında **RegistrationState** değişikliklerini **kayıtlı**. Bu değişikliği göremezsiniz çalıştırırsanız **Register-AzResourceProvider** cmdlet'ini yeniden.

PowerShell ile aşağıdaki görevleri otomatik hale getirilebilir:

* Kurtarma Hizmetleri kasası oluşturun.
* Azure dosya paylaşımları için yedeklemeyi yapılandırın.
* Bir yedekleme işi tetikler.
* Bir yedekleme işi izleyin.
* Azure dosya paylaşımını geri yükleyin.
* Tek bir Azure dosya, bir Azure dosya paylaşımından geri yükleyin.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Bir kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

1. Kurtarma Hizmetleri kasası bir Resource Manager kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanabilir veya bir kaynak grubu ile oluşturabileceğiniz **yeni AzResourceGroup** cmdlet'i. Bir kaynak grubu oluşturduğunuzda, kaynak grubunun konumunu ve adını belirtin.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Kullanım **yeni AzRecoveryServicesVault** cmdlet'ini kurtarma Hizmetleri kasası oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Kullanılacak depolama yedekliliği türü belirtin. Kullanabileceğiniz [yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnekte gösterildiği **- BackupStorageRedundancy** seçeneğini **testvault** kümesine **GeoRedundant**.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Tüm kasaları abonelikte görüntülemek için kullanın **Get-AzRecoveryServicesVault**.

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdaki örneğe benzerdir. Dikkat ilişkili **ResourceGroupName** ve **konumu** sağlanır.

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

Kullanım **kümesi AzRecoveryServicesVaultContext** kasa bağlamını ayarlamak için. Kasa bağlamı ayarlandıktan sonra sonraki tüm cmdlet'ler için geçerlidir. Aşağıdaki örnek için kasa bağlamını ayarlar **testvault**.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Azure PowerShell yönergelerine göre kasa bağlamını ayarı kullanımdan kaldırmayı planlıyoruz. Bunun yerine kullanıcılar aşağıdaki yönergelerde yer belirtildiği gibi kasa kimliği geçirmenizi öneriyoruz.

Alternatif olarak, depolama veya PowerShell işlemi gerçekleştirmek ve ilgili komutu iletmek istediğiniz kasaya Kimliğini getirir.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Bir Azure dosya paylaşımı için yedeklemeyi yapılandırma

### <a name="create-a-protection-policy"></a>Bir koruma ilkesi oluşturma

Bir yedekleme koruma İlkesi, en az bir bekletme ilkesi ile ilişkilidir. Bir bekletme ilkesi, silinmeden önce ne kadar bir kurtarma noktası tutulur tanımlar. Kullanım **Get-AzRecoveryServicesBackupRetentionPolicyObject** varsayılan saklama ilkesini görüntülemek için. 

Benzer şekilde, kullanabileceğiniz **Get-AzRecoveryServicesBackupSchedulePolicyObject** varsayılan zamanlama ilkesini elde edilir. **Yeni AzRecoveryServicesBackupProtectionPolicy** cmdlet'i, yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur. Zamanlama ve Bekletme İlkesi nesneleri girdi olarak kullanılan **yeni AzRecoveryServicesBackupProtectionPolicy** cmdlet'i. 

Aşağıdaki örnek zamanlama ilkesini ve bekletme ilkesi değişkenlerinde depolar. Örnek parametreleri tanımlamak için bu değişkenleri kullanır, **NewPolicy** koruma ilkesi oluşturulur.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdaki örneğe benzerdir.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

**NewAFSPolicy** günlük bir yedekleme alır ve 30 gün boyunca tutar.

### <a name="enable-protection"></a>Korumayı etkinleştir

Koruma İlkesi tanımladıktan sonra bu ilke ile Azure dosya paylaşımı için korumayı etkinleştirebilirsiniz.

İlk olarak, ilgili ilke nesnesi ile fetch **Get-AzRecoveryServicesBackupProtectionPolicy** cmdlet'i. Bu cmdlet, belirli bir ilke alma veya bir iş yükü türü ile ilişkili ilkeleri görüntülemek için kullanın.

Aşağıdaki örnek, iş yükü türü için ilkeleri alır **depolamasını Azure dosyalarına**.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Çıktı aşağıdaki örneğe benzerdir.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> Saat dilimini **BackupTime** alandır PowerShell'de Eşgüdümlü Evrensel Saat (UTC). Yedekleme zamanını Azure portalında göründüğü zaman için yerel saat diliminizde ayarlanır.
>
>

Adlı bir yedekleme İlkesi şu ilkeyi alır **dailyafs**.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Kullanım **etkinleştir AzRecoveryServicesBackupProtection** öğenin belirtilen İlkesi ile korumayı etkinleştirmek için. İlke, kasayla ilişkili sonra yedekleme iş akışı içinde ilke zamanlaması tanımlı zaman tetiklenir.

Aşağıdaki örnek, Azure dosya paylaşımının korumasını etkinleştirir **testAzureFileShare** depolama hesabı altında **testStorageAcct** ilkeyle **dailyafs**.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Yapılandırma koruma işi tamamlandı ve benzer bir çıktı verir komutu gösterildiği gibi bekler.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı bir yedeklemeyi tetikleyin

Kullanım **yedekleme AzRecoveryServicesBackupItem** korumalı Azure dosya paylaşımı için yedekleme işini tetiklemek için. Aşağıdaki komutları kullanarak depolama hesabı ve dosya paylaşımı içindeki almak ve isteğe bağlı yedekleme tetikleyin.

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

İle Azure dosya paylaşımının korumalı ilkeyi değiştirmek için **etkinleştir AzRecoveryServicesBackupProtection** ilgili yedekleme öğesi ve yeni koruma ilkesine sahip.

Aşağıdaki örnek değişiklikleri **testAzureFS** protection ilkesinden **dailyafs** için **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-shares-and-azure-files"></a>Azure dosya paylaşımları ve Azure dosyaları geri yükleme

Dosya paylaşımının tamamını özgün konumuna veya alternatif bir konuma geri yükleyebilirsiniz. Benzer şekilde, tek tek dosyaların dosya paylaşımı, çok geri yüklenebilir.

### <a name="fetch-recovery-points"></a>Getirme kurtarma noktaları

Kullanım **Get-AzRecoveryServicesBackupRecoveryPoint** yedekleme öğesi için tüm kurtarma noktalarını listelemek için kullanın. Aşağıdaki komut, değişken **$rp** son yedi güne ilişkin seçili yedekleme öğesi için kurtarma noktalarını dizisidir. Dizi en son kurtarma noktası dizinindeki ile zaman ters sırada sıralanır **0**. Kurtarma noktasını seçmek için standart PowerShell dizi dizini'oluşturma ' yı kullanın. Örnekte, **$rp [0]** en son kurtarma noktası seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Çıktı aşağıdaki örneğe benzerdir.

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

İlgili kurtarma noktası seçtikten sonra dosya paylaşımı veya dosya özgün konuma veya alternatif bir konuma burada açıklandığı gibi geri yükleyin.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Azure dosya paylaşımlarını alternatif bir konuma geri yükleme

#### <a name="restore-an-azure-file-share"></a>Bir Azure dosya paylaşımını geri yükleme

Alternatif bir konuma aşağıdaki bilgileri sağlayarak tanımlayın:

* **TargetStorageAccountName**: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
* **TargetFileShareName**: Hedef depolama içindeki dosya paylaşımları, yedeklenen içeriğin geri için hesap.
* **TargetFolder**: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi, hedef klasör değerler boş bir dize verin.
* **ResolveConflict**: Geri yüklenen verilerle bir çakışma varsa yönerge. Kabul **üzerine** veya **atla**.

Yedeklenen dosya paylaşımı için alternatif bir konuma geri yüklemek için restore komutu bu parametreleri belirtin.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

Aşağıdaki örnekte gösterildiği gibi komut izlenmesi için bir kimlik ile bir iş döndürür.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restore-an-azure-file"></a>Bir Azure dosya geri yükleme

Tek bir dosyayı yerine dosya paylaşımının tamamını geri yüklemek için benzersiz bir şekilde aşağıdaki parametreleri sağlayarak tek tek dosya tanımlayın:

* **TargetStorageAccountName**: Depolama hesabı yedeklenen içeriğin geri yüklenir. Hedef depolama hesabı, kasa ile aynı konumda olmalıdır.
* **TargetFileShareName**: Hedef depolama içindeki dosya paylaşımları, yedeklenen içeriğin geri için hesap.
* **TargetFolder**: Klasörü altında dosya paylaşımı verileri geri yüklenir. Yedeklenen içeriğin kök klasörüne geri yüklenmesi, hedef klasör değerler boş bir dize verin.
* **SourceFilePath**: Dize olarak dosya paylaşımı içinde geri yüklenecek dosyanın mutlak yolu. Bu yol, kullanılan aynı yoldur **Get-AzStorageFile** PowerShell cmdlet'i.
* **SourceFileType**: Bir dizin olup olmadığını veya dosya seçilmedi. Kabul **dizin** veya **dosya**.
* **ResolveConflict**: Geri yüklenen verilerle bir çakışma varsa yönerge. Kabul **üzerine** veya **atla**.

Ek parametreleri, geri yüklenmesi için yalnızca tek bir dosyaya ilgilidir.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Bu komut, daha önce gösterildiği gibi izlenmesi için bir kimlik ile bir iş ayrıca döndürür.

### <a name="restore-azure-file-shares-to-the-original-location"></a>Azure dosya paylaşımlarını özgün konuma geri yükleme

Özgün bir konuma geri yüklediğinizde, tüm hedef ve hedef ilgili parametreleri belirtilmesi gerekmez. Yalnızca **ResolveConflict** sağlanmalıdır.

#### <a name="overwrite-an-azure-file-share"></a>Bir Azure dosya paylaşımı üzerine yaz

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Bir Azure dosya üzerine yazma

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Yedekleme izleme ve geri yükleme işleri

İsteğe bağlı yedekleme ve geri yükleme işlemleri önceki bölümde gösterildiği gibi bir kimliği ile birlikte bir iş dönüş ["talep üzerine yedekleme tetikleyin."](#trigger-an-on-demand-backup) Kullanım **Get-AzRecoveryServicesBackupJobDetails** işinin ilerleme durumunu izlemek ve daha fazla ayrıntı getirmek için cmdlet'i.

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
