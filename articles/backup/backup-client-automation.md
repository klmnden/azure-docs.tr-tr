---
title: "Azure için Windows Server'ı Yedekle için PowerShell kullanma | Microsoft Docs"
description: "Dağıtma ve Azure PowerShell kullanarak yedekleme yönetme hakkında bilgi edinin"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: 5a7189d9ccc8ab7aee61cd32e465b2c9b63680d2
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>PowerShell kullanarak Windows Server/Windows İstemcisi için Azure’a yedekleme dağıtma ve yönetme
Bu makalede Azure yedekleme Windows Server veya Windows İstemcisi ayarlama ve yedekleme ve kurtarma yönetmek için PowerShell kullanmayı gösterir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Bu makalede Azure Resource Manager (ARM) ve bir kaynak grubu bir kurtarma Hizmetleri kasası kullanmanızı sağlamak MS çevrimiçi yedekleme PowerShell cmdlet'leri odaklanır.

Azure PowerShell 1.0 Ekim 2015'te yayımlanmıştır. Bu sürüm 0.9.8 başarılı bir şekilde serbest bırakır ve bazı önemli değişiklikler hakkında özellikle cmdlet'leri adlandırma desenini getirildi. 1.0 cmdlet'leri {fiil}-AzureRm{isim} adlandırma desenini izler; Buna karşın, 0.9.8 adlarında **Rm** bulunmaz (örneğin, New- AzureResourceGroup yerine New-AzureRmResourceGroup). Azure PowerShell 0.9.8 kullanıldığında, önce **Switch-AzureMode AzureResourceManager** komutunu çalıştırarak Resource Manager modunu etkinleştirmelisiniz. Bu komut 1.0 veya üstü gerekli değildir.

0.9.8 için yazılan komut dosyalarınızı kullanmak isteyip istemediğinizi ortamında 1.0 veya üstü ortamı dikkatlice güncelleştirmek ve komut dosyalarını üretimde beklenmeyen etkiyi önlemek için kullanmadan önce bir ön üretim ortamında test.

[En son PowerShell sürümü indirme](https://github.com/Azure/azure-powershell/releases) (gerekli minimum sürüm: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma
Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturmada size yol açar. Kurtarma Hizmetleri kasasına yedekleme Kasası ' farklıdır.

1. Azure Backup ilk kez kullanıyorsanız, kullanmalısınız **Register-AzureRMResourceProvider** aboneliğinizle Azure Recovery hizmeti sağlayıcısını kaydetmek için cmdlet.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Kurtarma Hizmetleri kasası bir ARM kaynak olduğundan, bir kaynak grubu içindeki yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Yeni bir kaynak grubu oluştururken, ad ve kaynak grubu için konum belirtin.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Kullanım **yeni AzureRmRecoveryServicesVault** yeni kasası oluşturmak için cmdlet'i. Kaynak grubu için kullanılan kasa için aynı konumu belirttiğinizden emin olun.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Kullanılacak depolama artıklığı türünü belirtin; kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Aşağıdaki örnek, testVault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

   > [!TIP]
   > Çok sayıda Azure yedekleme cmdlet'lerini girdi olarak kurtarma Hizmetleri kasası nesnesi gerektirir. Bu nedenle, bir değişkende yedekleme kurtarma Hizmetleri kasası nesne depolamak uygundur.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüleyin
Kullanım **Get-AzureRmRecoveryServicesVault** geçerli abonelikte tüm kasalarının listesini görüntülemek için. Bu komut, yeni bir kasa oluşturulduğunu denetleyin veya hangi kasalarını abonelikte bulunan görmek için kullanabilirsiniz.

Komutu çalıştırmak **Get-AzureRmRecoveryServicesVault**, ve Abonelikteki tüm kasalarını listelenir.

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


## <a name="installing-the-azure-backup-agent"></a>Azure Backup aracısını yükleme
Azure Backup aracısını yüklemeden önce Windows Server yükleyici indirildi ve mevcut olması gerekir. Yükleyicisi'nden en son sürümünü almak [Microsoft Download Center](http://aka.ms/azurebackup_agent) veya kurtarma Hizmetleri Kasası'nın Pano sayfası. Yükleyici gibi kolay erişilebilir bir konuma kaydedin * C:\Downloads\*.

Alternatif olarak, yükleyici almak için PowerShell kullanın:
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

Aracıyı yüklemek için yükseltilmiş bir PowerShell konsolunda aşağıdaki komutu çalıştırın:

```
PS C:\> MARSAgentInstaller.exe /q
```

Bu, tüm varsayılan seçeneklerle Aracısı'nı yükler. Yükleme arka planda birkaç dakika sürer. Belirtmezseniz, */nu* seçeneği sonra **Windows Update** tüm güncelleştirmeleri denetlemek için yükleme işleminin sonunda penceresi açılır. Yüklendikten sonra aracı yüklü programlar listesinde gösterilir.

Yüklü programların listesini görmek için şu adrese gidin **Denetim Masası** > **programları** > **programlar ve Özellikler**.

![Yüklü aracı](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Yükleme Seçenekleri
Komut satırı kullanılabilir tüm seçenekleri görmek için aşağıdaki komutu kullanın:

```
PS C:\> MARSAgentInstaller.exe /?
```

Mevcut seçenekler şunlardır:

| Seçenek | Ayrıntılar | Varsayılan |
| --- | --- | --- |
| /q |Sessiz yükleme |- |
| p: "Konum" |Azure Backup Aracısı yükleme klasörünün yolu. |C:\Program Files\Microsoft Azure kurtarma Hizmetleri Aracısı |
| / s: "Konum" |Azure Backup aracısı için önbellek klasör yolu. |C:\Program Files\Microsoft Azure kurtarma Hizmetleri Agent\Scratch |
| /m |Microsoft Update'e kabulü |- |
| /nu |Yükleme tamamlandıktan sonra güncelleştirmeleri denetleme |- |
| /d |Microsoft Azure kurtarma Hizmetleri Aracısı kaldırır |- |
| /ph |Proxy konağı adresi |- |
| /PO |Proxy ana bilgisayar bağlantı noktası numarası |- |
| /PU |Proxy konağı kullanıcı |- |
| /pw |Proxy parolası |- |

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Windows Server veya Windows istemci makinesi bir kurtarma Hizmetleri kasasına kaydetme
Kurtarma Hizmetleri kasası oluşturduktan sonra en son aracı ve kasa kimlik bilgilerini indirin ve C:\Downloads gibi uygun bir konumda saklayın.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

Windows Server veya Windows istemci makinesi üzerinde çalıştırmak [başlangıç OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) makine kasaya kaydetmek için cmdlet.
Bu ve diğer cmdlet'leri yedekleme için kullanılan Mars AgentInstaller yükleme işleminin bir parçası olarak eklenen MSONLINE modülü arasındadır. 

Aracı yükleyici $Env güncelleştirmez: PSModulePath değişkeni. Bu modül otomatik-yüklemesi başarısız anlamına gelir. Bu sorunu çözmek için aşağıdakileri yapabilirsiniz:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

Alternatif olarak, el ile modülü betiğinizde şu şekilde yükleyebilirsiniz:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

Çevrimiçi Yedekleme cmdlet'leri yükledikten sonra kasa kimlik bilgilerini Kaydet:


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Kasa kimlik bilgileri dosyası belirtmek için göreli yolların kullanmayın. Cmdlet'i bir girdi olarak mutlak bir yol sağlamalısınız.
>
>

## <a name="networking-settings"></a>Ağ ayarları
Windows makine internet bağlantısını bir proxy sunucu üzerinden olduğunda, proxy ayarlarını aracıya de girilebilir. Bu örnekte, olmadığından hiçbir Ara sunucunun biz açıkça herhangi bir proxy ile ilgili bilgi temizleme.

Bant genişliği kullanımı seçeneklerle da denetlenebilir ```work hour bandwidth``` ve ```non-work hour bandwidth``` haftanın belirli bir dizi için.

Proxy ve bant ayrıntılarını ayarlama yapılır kullanarak [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Şifreleme ayarları
Yedekleme verilerini Azure Backup için gönderilen veri gizliliğini korumak için şifrelenir. Şifreleme Parolası "geri yükleme sırasındaki verileri şifrelemek için parola" dir.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> Ayarlandıktan sonra parola bilgilerini güvenli ve güvenli tutar. Bu parola veri Azure'dan geri yükleyemezsiniz.
>
>

## <a name="back-up-files-and-folders"></a>Dosya ve klasörleri yedekleme
Windows sunucularını ve istemcilerini tüm yedeklemeleri Azure yedekleme için bir ilke tarafından yönetilir. İlke üç bölümden oluşur:

1. A **yedekleme zamanlaması** yedeklemeler alınır ve hizmeti ile eşitlenmiş gerektiğinde belirtir.
2. A **bekletme zamanlaması** belirleyen ne kadar süreyle Azure kurtarma noktalarını korumak için.
3. A **dosya ekleme/çıkarma belirtimi** belirleyen ne yedeklenmelidir.

Yedekleme, otomatikleştirme beri bu belgede, hiçbir şey yapılandırılmış varsayıyoruz. Kullanarak yeni bir yedekleme ilkesi oluşturarak başlayın [yeni OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet'i.

```
PS C:\> $newpolicy = New-OBPolicy
```

İlke şu anda boş olur ve diğer cmdlet'ler öğeleri tanımlamak için kapsanan veya dışlanan, ne zaman yedeklemeleri çalışacağını ve yedeklemeleri depolanacak burada.

### <a name="configuring-the-backup-schedule"></a>Yedekleme zamanlamasını yapılandırma
Bir ilke 3 bölümlerinin ilk kullanılarak oluşturulan yedekleme zamanlaması olan [yeni OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet'i. Yedekleme zamanlaması yedeklemeleri yapılması gerektiğinde tanımlar. Bir zamanlama oluştururken 2 giriş parametreleri belirtmeniz gerekir:

* **Haftanın günleri** , yedekleme çalıştırmanız gerekir. Yedekleme işi yalnızca bir gün veya haftanın her gün veya arasındaki herhangi bir birleşimini çalıştırabilirsiniz.
* **Günün kez** yedeklemenin ne zaman çalışmalı. Yedekleme harekete geçirildiğinde günün en fazla 3 farklı saatleri tanımlayabilirsiniz.

Örneğin, 4'e her Cumartesi ve Pazar çalışan bir yedekleme ilkesi yapılandırabilirsiniz.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Yedekleme zamanlaması bir ilkesiyle ilişkilendirilmesi gerekir ve bu kullanarak elde [kümesi OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet'i.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Bir bekletme ilkesi yapılandırma
Bekletme İlkesi yedekleme işlerini oluşturulan kurtarma noktaları ne kadar süreyle saklanacağını tanımlar. Kullanarak yeni bir bekletme ilkesi oluşturulurken [yeni OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, yedekleme kurtarma noktalarının Azure yedekleme ile korunması gereken gün sayısını belirtebilirsiniz. Aşağıdaki örnek bir bekletme ilkesi 7 gün olarak ayarlar.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Bekletme İlkesi cmdlet'ini kullanarak ana ilkesiyle ilişkili olmalıdır [kümesi OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a>Dahil ve yedeklenecek dosyaları dışlama
Bir ```OBFileSpec``` nesnesi bulunan ve bir yedek dışlanan dosyaları tanımlar. Bu, korumalı dosyaları ve klasörleri bir makinede kullanıma kapsam kurallar kümesidir. Birçok ekleme veya hariç tutma kuralları gerektiği gibi dosya ve bir ilke ile ilişkilendirmek gibi sahip olabilir. Yeni bir OBFileSpec nesnesi oluştururken şunları yapabilirsiniz:

* Dosyaları ve dahil edilecek klasörleri belirtin
* Dosya ve klasörleri dışarıda belirtin
* Özyinelemeli bir klasör (veya) olup yalnızca üst düzey dosyaları belirtilen klasöre yedeklenmelidir verilerin yedeğini yukarı belirtin.

İkinci yeni OBFileSpec komutta - özyinelemesiz bayrağı kullanılarak sağlanır.

Aşağıdaki örnekte, biz birimi yedekleyin C: ve D: ve işletim sistemi ikili dosyaları Windows klasöründeki ve geçici klasörleri dışarıda. Oluşturacağız Bunu yapmak için iki belirtimleri kullanarak dosya [yeni OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - eklenmesi, diğeri dışarıda bırakma için. Dosya belirtimleri oluşturduktan sonra ilkeyi kullanarak ilişkili oldukları [Ekle OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet'i.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>İlke uygulama
Şimdi İlkesi tamamlandıktan ve ilişkili bir yedekleme zamanlaması, bekletme ilkesi ve dosyaların bir ekleme/çıkarma listesi vardır. Bu ilkeyi şimdi kullanmak Azure Backup için kaydedilmiş olabilir. Yeni oluşturulan İlkesi, uygulamadan önce mevcut hiçbir yedekleme ilkeleri kullanılarak sunucu ile ilişkilendirilen olduğundan emin olun [Kaldır OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet'i. İlkeyi kaldırmayı onay ister. Onay Kullan'ı atlamanızı ```-Confirm:$false``` bayrağı cmdlet ile.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

İlke nesnesi yürüten yapılır kullanarak [kümesi OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet'i. Bu ayrıca onaylamanız istenir. Onay Kullan'ı atlamanızı ```-Confirm:$false``` bayrağı cmdlet ile.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Kullanarak varolan yedekleme ilkesi ayrıntılarını görüntüleyebilirsiniz [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet'i. Daha fazla kullanarak aşağı inebilir [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet'i için yedekleme zamanlamasını ve [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) bekletme ilkeleri için cmdlet

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Geçici bir yedeklemenin
Bir yedekleme İlkesi ayarladıktan sonra yedeklemeler zamanlama meydana gelir. Geçici bir yedeklemeyi tetikleme olduğunu da olası kullanarak [başlangıç OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Verileri Azure yedekten geri yükleyin
Bu bölümde, veri kurtarma Azure Backup otomatikleştirmek için adımlarda size kılavuzluk eder. Bunun yapılması, aşağıdaki adımları içerir:

1. Kaynak birim seçin
2. Geri yüklemek için bir yedekleme noktası seçin
3. Geri yüklemek için bir öğe seçin
4. Tetikleyici geri yükleme işlemi

### <a name="picking-the-source-volume"></a>Kaynak birim çekme
Bir öğeyi Azure yedeklemeden geri yüklemek için ilk öğenin kaynağını tanımlamak gerekir. Biz Windows Server veya Windows İstemcisi bağlamında komutları yürütülürken bu yana makine zaten tanımlanır. Kaynağı tanımlayan bir sonraki adım, onu içeren birim belirlemektir. Bu makineden yedeklenen alınabilir yürüterek kaynakları veya birimlerin listesini [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet'i. Bu komut, bu sunucu/istemciden yedeklenen tüm kaynakları bir dizi döndürür.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-to-restore"></a>Geri yüklemek yedek bir noktası seçme
Yürüterek yedekleme noktalarının bir listesini almak [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) uygun parametreleri cmdlet'iyle. Bizim örneğimizde, en son yedekleme noktası kaynak birimin seçeceğiz *D:* ve belirli bir dosya kurtarmak için kullanın.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
Nesne ```$rps``` yedekleme noktaları dizisidir. En son noktası ilk öğedir ve eski noktası n. öğedir. En son noktası seçmek için kullanacağız ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Geri yüklemek için bir öğe seçme
Tam dosya ya da geri yüklemek için yinelemeli kullanım klasörü tanımlamak için [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet'i. Klasör hiyerarşisi göz atabilirsiniz yalnızca kullanarak bu şekilde ```Get-OBRecoverableItem```.

Biz dosyayı geri yüklemek istiyorsanız, bu örnekte, *finances.xls* biz nesne kullanan başvurabilir ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Kullanarak geri yüklemek öğeler için arama yapabilirsiniz ```Get-OBRecoverableItem``` cmdlet'i. Aranacak örneğimizde *finances.xls* biz şu komutu çalıştırarak dosyayı bir tanıtıcı alabilir:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Geri yükleme işlemini tetikler
Geri yükleme işlemi tetiklemek için öncelikle kurtarma seçeneklerini belirtmek ihtiyacımız. Bu kullanılarak yapılabilir [yeni OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet'i. Bu örnek için dosyaları geri yüklemek istiyoruz varsayalım *C:\temp*. Ayrıca hedef klasörde zaten mevcut dosyaların atlamak istiyoruz varsayalım *C:\temp*. Bu tür bir kurtarma seçeneğini oluşturmak için aşağıdaki komutu kullanın:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Kullanarak geri yükleme işlemi şimdi tetikleyebilir [başlangıç OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) seçilen komutu ```$item``` çıktısından ```Get-OBRecoverableItem``` cmdlet:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>Azure Backup aracısını kaldırma
Azure Backup aracısını kaldırma, aşağıdaki komutu kullanarak gerçekleştirilebilir:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Aracısı ikili dosyalarının makineden kaldırma dikkate alınması gereken bazı sonuçları vardır:

* Dosya Filtresi makineden kaldırır ve değişiklikleri izleme durduruldu.
* Tüm ilke bilgilerine makineden kaldırıldı, ancak hizmetinde depolanması ilke bilgilerini sürdürür.
* Tüm yedekleme zamanlamaları kaldırılır ve daha fazla yedekleme alınır.

Ancak, verileri Azure kalırken depolanır ve bekletme ilkesi Kurulum göredir, tarafından korunur. Eski noktalarını otomatik olarak eski.

## <a name="remote-management"></a>Uzaktan Yönetim
Azure Yedekleme aracısı, ilkeler ve veri kaynakları çevresinde tüm yönetim uzaktan PowerShell aracılığıyla gerçekleştirilebilir. Uzaktan yönetilecek makine doğru hazırlanması gerekir.

Varsayılan olarak, WinRM hizmeti el ile başlatma için yapılandırılır. Başlangıç türünü ayarlamak *otomatik* ve hizmetin başlatılması. WinRM hizmetinin çalıştığından emin olun, durum özelliğinin değeri olmalıdır *çalıştıran*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell uzaktan iletişim için yapılandırılmış olması gerekir.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Makine artık uzaktan - aracı yüklemesinden başlatma yönetilebilir. Örneğin, aşağıdaki komut dosyası Aracısı uzak makineye kopyalar ve onu yükler.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Sonraki adımlar
Azure yedekleme için Windows Server/istemcisi bakın hakkında daha fazla bilgi için

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Windows sunucularını yedekleme](backup-configure-vault.md)
