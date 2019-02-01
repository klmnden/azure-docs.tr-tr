---
title: Windows Server'ı Azure'a yedekleme için PowerShell'i kullanma
description: Azure PowerShell kullanarak yedekleme dağıtma ve yönetme hakkında bilgi edinin
services: backup
author: pvrk
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 5/24/2018
ms.author: pvrk
ms.openlocfilehash: d430f6252157c5d34aa236ef88f8490b4ad6a184
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497953"
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>PowerShell kullanarak Windows Server/Windows İstemcisi için Azure’a yedekleme dağıtma ve yönetme
Bu makalede Azure Backup Windows Server veya Windows istemci ayarlama ve yedekleme ve kurtarma yönetmek için PowerShell kullanmayı gösterir.

## <a name="install-azure-powershell"></a>Azure PowerShell'i yükleme
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Bu makalede Azure Resource Manager (ARM) ve bir kaynak grubunda bir kurtarma Hizmetleri kasası kullanmanıza olanak sağlayan MS çevrimiçi yedekleme PowerShell cmdlet'leri odaklanır.

Azure PowerShell 1.0 Ekim 2015'te yayımlanmıştır. Bu sürüm 0.9.8 başarılı oldu. sürüm ve bazı önemli değişiklikler hakkında özellikle cmdlet'leri adlandırma desenini getirdi. 1.0 cmdlet'leri {fiil}-AzureRm{isim} adlandırma desenini izler; Buna karşın, 0.9.8 adlarında **Rm** bulunmaz (örneğin, New- AzureResourceGroup yerine New-AzureRmResourceGroup). Azure PowerShell 0.9.8 kullanıldığında, önce **Switch-AzureMode AzureResourceManager** komutunu çalıştırarak Resource Manager modunu etkinleştirmelisiniz. Bu komut, 1.0 veya üzeri gerekli değildir.

Betiklerinizin 0.9.8 için yazılan kullanmak istiyorsanız ortamında 1.0 veya üzeri ortam dikkatlice güncelleştirin ve bunları üretimde beklenmeyen etkiden kullanmadan önce bir ön üretim ortamında test betikleri.

[En son PowerShell sürümünü indirin](https://github.com/Azure/azure-powershell/releases) (gerekli en düşük sürümü şöyledir: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma
Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturma işleminde yol. Bir Backup kasasının kurtarma Hizmetleri kasası farklıdır.

1. Azure Backup'ı ilk kez kullanıyorsanız, kullanmalısınız **Register-AzureRMResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Kurtarma Hizmetleri kasası bir ARM kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Yeni bir kaynak grubu oluştururken, kaynak grubunun konumunu ve adını belirtin.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Kullanım **New-AzureRmRecoveryServicesVault** cmdlet'ini yeni kasa oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirttiğinizden emin olun.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Kullanılacak depolama yedekliliği türü; belirtin. kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnek, testVault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle
Kullanım **Get-AzureRmRecoveryServicesVault** geçerli abonelikte tüm kasalarının listesi görüntülemek için. Yeni bir kasa oluşturulduğunu denetleyin ya da hangi kasaları abonelikte kullanılabilir olduğunu görmek için bu komutu kullanabilirsiniz.

Komutu **Get-AzureRmRecoveryServicesVault**, ve Abonelikteki tüm kasalar listelenir.

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


[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="installing-the-azure-backup-agent"></a>Azure Backup aracısını yükleme
Azure Backup aracısını yüklemeden önce Windows Server yükleyici, indirilen ve mevcut olması gerekir. Yükleyicisi'nden en son sürümünü edinebilirsiniz [Microsoft Download Center](https://aka.ms/azurebackup_agent) veya kurtarma Hizmetleri kasasının Pano sayfası. Yükleyici gibi kolayca erişilebilir bir konuma kaydedin * C:\Downloads\*.

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

Bu, tüm varsayılan seçeneklerle aracısını yükler. Yükleme arka planda birkaç dakika sürer. Siz belirtmezseniz */nu* seçeneği sonra **Windows Update** sonunda, tüm güncelleştirmeleri denetlemek için bir yükleme penceresi açılacaktır. Yüklendikten sonra aracı yüklü programlar listesinde gösterilir.

Yüklü programlar listesinde görmek için Git **Denetim Masası** > **programlar** > **programlar ve Özellikler**.

![Aracı yüklü](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Yükleme Seçenekleri
Komut satırı kullanılabilen seçenekleri görmek için aşağıdaki komutu kullanın:

```
PS C:\> MARSAgentInstaller.exe /?
```

Kullanılabilir seçenekler şunlardır:

| Seçenek | Ayrıntılar | Varsayılan |
| --- | --- | --- |
| /q |Sessiz yükleme |- |
| / p: "Konum" |Azure Backup Aracısı yükleme klasörünün yolu. |C:\Program Files\Microsoft Azure Recovery Services Agent |
| / s: "Konum" |Azure Backup aracısı için önbellek klasörünün yolu. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /dk |Microsoft Update'e katılım |- |
| /nu |Yükleme tamamlandıktan sonra güncelleştirmeleri denetleme |- |
| /g |Microsoft Azure kurtarma Hizmetleri Aracısı'nı kaldırır |- |
| /ph |Proxy ana bilgisayar adresi |- |
| /PO |Proxy ana bilgisayar bağlantı noktası numarası |- |
| /PU |Proxy ana kullanıcı adı |- |
| /pw |Proxy parola |- |

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Windows Server veya Windows istemci makinesine bir kurtarma Hizmetleri kasasına kaydediliyor
Kurtarma Hizmetleri kasası oluşturduktan sonra en son aracı ve kasa kimlik bilgileri indirin ve C:\Downloads gibi uygun bir konumda depolayın.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

Windows Server veya Windows istemci makinesi üzerinde çalıştırma [başlangıç OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) makine kasaya kaydetmek için cmdlet'i.
Bu ve diğer cmdlet'leri yedekleme için yükleme işleminin bir parçası Mars AgentInstaller eklenen MSONLINE modülü arasındadır. 

Aracı yükleyicisini $Env güncelleştirmez: PSModulePath değişkeni. Bu modül otomatik yükle başarısız anlamına gelir. Bu sorunu gidermek için aşağıdakileri yapabilirsiniz:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules'
```

Alternatif olarak, el ile modülü betiğinizde şu şekilde yükleyebilirsiniz:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

Çevrimiçi Yedekleme cmdlet'leri yüklendikten sonra kasa kimlik bilgilerini kaydedin:


```

PS C:\> Start-OBRegistration -VaultCredentials $credsfilename.FilePath -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Göreli yollar, kasa kimlik bilgileri dosyası belirtmek için kullanmayın. Cmdlet'i bir giriş olarak mutlak bir yol sağlamanız gerekir.
>
>

## <a name="networking-settings"></a>Ağ ayarları
Bağlantı Windows makinenin İnternet'e bir proxy sunucu üzerinden olduğunda aracı için proxy ayarlarını da sağlanabilir. Bu örnekte, proxy sunucusu yok, biz açıkça herhangi bir ara sunucu ile ilgili bilgi temizlemek için.

Bant genişliği kullanımı ile seçenekleri de denetlenebilir ```work hour bandwidth``` ve ```non-work hour bandwidth``` için haftanın günlerini belirli bir dizi.

Ayar proxy ve bant genişliği ayrıntıları yapılır kullanarak [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Şifreleme ayarları
Verilerin gizliliğini korumak için Azure Backup için gönderilen yedekleme verileri şifrelenir. Şifreleme Parolası "geri yükleme sırasında verilerin şifresini çözmek için parola" dir.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> Ayarlandıktan sonra parolayı bilgilerin korunmasına ve güvende tutun. Bu parola olmadan verilerini Azure'dan geri yükleyemezsiniz.
>
>

## <a name="back-up-files-and-folders"></a>Dosya ve klasörleri yedekleme
Tüm Windows sunucularını ve istemcilerini yedeklemelerden Azure yedekleme İlkesi tarafından yönetilir. İlke, üç bölümden oluşur:

1. A **yedekleme zamanlaması** yedeklemeleri alınır ve hizmetle eşitlenmesi gerektiğinde belirtir.
2. A **saklama zamanlaması** belirten ne kadar süreyle azure'da kurtarma noktalarını tutmak isteyip.
3. A **dosya ekleme/çıkarma belirtimi** belirleyen ne yedeklenmesi gerektiğidir.

Yedekleme, biz otomatikleştirme olduğundan bu belgede, hiçbir şey yapılandırılmış varsayıyoruz. Kullanarak yeni bir yedekleme ilkesi oluşturarak başlayın [yeni OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet'i.

```
PS C:\> $newpolicy = New-OBPolicy
```

İlke şu anda boş olur ve diğer cmdlet'ler öğeleri tanımlamak için dahil veya hariç tutulan, ne zaman yedeklemeler çalışacağını ve yedeklemeleri depolanan burada.

### <a name="configuring-the-backup-schedule"></a>Yedekleme zamanlamasını yapılandırma
Bir ilke 3 bölümlerini kullanılarak oluşturulan yedekleme zamanlaması, davranıştır [yeni OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet'i. Yedekleme zamanlaması yedeklemeleri yapılması gerektiğinde tanımlar. Bir zamanlama oluştururken 2 giriş parametrelerini belirten gerekir:

* **Haftanın günleri** , yedekleme çalıştırmanız gerekir. Yedekleme işi yalnızca bir gün veya haftanın her günü veya arasındaki herhangi bir birleşimini çalıştırabilirsiniz.
* **Gün saatler** yedeklemenin ne zaman çalıştırılacağını. En fazla 3 farklı saatlerinde yedekleme harekete geçirildiğinde tanımlayabilirsiniz.

Örneğin, her Cumartesi ve Pazar günü saat 4'te çalışan bir yedekleme ilkesi yapılandırabilirsiniz.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Yedekleme zamanlaması bir ilkesiyle ilişkilendirilmesi gerekir ve bu kullanarak gerçekleştirilebilir [kümesi OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet'i.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Bir bekletme ilkesi yapılandırma
Bekletme İlkesi, yedekleme işlerini oluşturulan kurtarma noktalarının ne kadar süre bekletileceğini tanımlar. Kullanarak yeni bir bekletme ilkesi oluştururken [yeni OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, Azure Backup ile korunacak yedekleme kurtarma noktalarına ihtiyaç gün sayısını belirtebilirsiniz. Aşağıdaki örnek, 7 günlük bir bekletme ilkesi ayarlar.

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
### <a name="including-and-excluding-files-to-be-backed-up"></a>Dahil etme ve yedeklenecek dosyaları dışlama
Bir ```OBFileSpec``` nesne dosyaları dahil ve hariç tutulan bir yedeklemede tanımlar. Bu korumalı dosyaları ve klasörleri bir makinede kapsam kurallarının bir kümesidir. Çoğu gerektiği gibi ekleme veya çıkarma kurallar dosyası ve bunları bir ilke ile ilişkilendirmek, olabilir. Yeni bir OBFileSpec nesnesi oluştururken şunları yapabilirsiniz:

* Dahil edilecek klasörleri ve dosyaları belirtin
* Hariç tutulacak klasörler ve dosyaları belirtin
* Yedekleme verilerinin bir klasöre (veya) olup yalnızca üst düzey dosyaları belirtilen klasöre yedeklenmelidir özyinelemeli yedeklemesi belirtin.

İkinci yeni OBFileSpec komut içinde - özyinelemesiz bayrağı kullanılarak sağlanır.

Aşağıdaki örnekte, biz birimin C: ve D: yedekleyin ve OS ikili dosyalarının Windows klasöründeki ve tüm geçici klasörleri dışarıda. İki dosya belirtimleri kullanarak oluşturacağımız Bunu yapmak için [yeni OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - bir dahil etme ve dışlama için. Dosya belirtimleri oluşturulduktan sonra ilkeyi kullanarak ilişkili oldukları [Ekle OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet'i.

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
Artık ilke nesnesi tamamlandıktan ve ilişkili bir yedekleme zamanlaması, bekletme ilkesi ve bir dosyalar ekleme/çıkarma listesi vardır. Bu ilke, artık kullanmak Azure Backup için kaydedilmiş olabilir. Yeni oluşturulan ilke, uygulamadan önce sağlamak hiçbir mevcut yedekleme ilkelerini kullanarak sunucuyla ilişkili olduğunu [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet'i. İlkeyi kaldırma, onay ister. Onay kullanımı atlamak için ```-Confirm:$false``` cmdlet'i ile bayrağı.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

İlke nesnesi yürüten yapılır kullanarak [kümesi OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet'i. Bu ayrıca onay isteyecektir. Onay kullanımı atlamak için ```-Confirm:$false``` cmdlet'i ile bayrağı.

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

Yedekleme İlkesi mevcut kullanma ayrıntılı bilgi görüntüleyebileceğiniz [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet'i. Daha fazla kullanılarak kilitlenmemiş inebilir [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet'i için yedekleme zamanlamasını ve [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet'i için bekletme ilkeleri için

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

### <a name="performing-an-ad-hoc-backup"></a>Bir geçici yedekleme gerçekleştirme
Bir yedekleme İlkesi ayarlandıktan sonra yedeklemeler zamanlama ortaya çıkar. Geçici bir yedeklemeyi tetikleme olduğunu da olası kullanarak [başlangıç OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:

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
Bu bölümde, veri kurtarma Azure Backup otomatikleştirme adımlarında size yol gösterir. Bunun yapılması, aşağıdaki adımları içerir:

1. Kaynak birim seçin
2. Geri yüklemek için bir yedekleme noktası seçin
3. Geri yüklemek için bir öğe seçin
4. Tetikleyici geri yükleme işlemi

### <a name="picking-the-source-volume"></a>Kaynak birim çekme
Azure yedeklemeden bir öğe için ilk öğenin kaynağını belirlemek gerekir. Komutlar bir Windows Server veya Windows İstemcisi bağlamında yürütüyorsunuz olduğundan makine zaten tanımlanmış. Kaynağı tanımlayan bir sonraki adımınız onu içeren birim belirlemektir. Birim veya bu makineden yedeklenen alınabilir yürüterek kaynakları listesini [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet'i. Bu komut, bu sunucu/istemciden yedeklenen tüm kaynakların bir dizi döndürür.

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

### <a name="choosing-a-backup-point-from-which-to-restore"></a>Geri yüklemek bir yedekleme noktası seçme
Yürüterek yedekleme noktalarının bir listesini alın [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) uygun parametrelerle cmdlet'i. Bizim örneğimizde, en son yedekleme noktası için kaynak birim seçeceğiz *D:* ve belirli bir dosyayı kurtarmak için kullanabilirsiniz.

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
Nesne ```$rps``` yedekleme noktaları dizisidir. İlk öğe en son noktadır ve n. öğeye eski noktasıdır. En son noktasını seçmek için kullanacağız ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Geri yüklemek için bir öğe seçme
Tam dosya veya klasörü geri yüklemek için yinelemeli olarak kullanım tanımlamak için [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet'i. Böylece yalnızca kullanarak klasör hiyerarşisinde görüntülenebilir ```Get-OBRecoverableItem```.

Bu örnekte, dosyayı geri yüklemek istiyorsanız *finances.xls* biz nesneyi kullanan başvurabilirsiniz ```$filesFolders[1]```.

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

Öğeleri kullanarak geri yüklemek için arama da yapabilirsiniz ```Get-OBRecoverableItem``` cmdlet'i. Aranacak örneğimizde *finances.xls* Biz bu komutu çalıştırarak dosya bir tanıtıcı alabilir:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Geri yükleme işlemi tetikleniyor
Geri yükleme işlemi tetiklemek için öncelikle kurtarma seçeneklerini belirtmek ihtiyacımız var. Bu kullanarak yapılabilir [yeni OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet'i. Bu örnekte, varsayalım dosyaları geri yüklemek istiyoruz *C:\temp*. Ayrıca hedef klasörde zaten mevcut dosyaları atlamayı istediğimizi varsayalım *C:\temp*. Bu tür bir kurtarma seçeneğini oluşturmak için aşağıdaki komutu kullanın:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Kullanarak geri yükleme işlemi şimdi Tetikle [başlangıç OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) komutunu seçili ```$item``` çıktısından ```Get-OBRecoverableItem``` cmdlet:

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

Aracı ikili dosyaları makineden kaldırılması dikkate alınması gereken bazı sonuçları vardır:

* Dosya Filtresi makineden kaldırır ve değişiklik izleme durduruldu.
* Tüm ilke bilgilerine makineden kaldırılır ancak ilke bilgilerini hizmetinde depolanması devam eder.
* Tüm yedekleme zamanlamaları kaldırılır ve daha fazla yedekleme alınır.

Ancak, verileri Azure saklanır depolanan ve bekletme ilkesi Kurulumu göre sizin tarafınızdan korunur. Eski noktalarını otomatik olarak eski.

## <a name="remote-management"></a>Uzaktan yönetim
Azure Backup Aracısı, ilkeler ve veri kaynakları tüm yönetimi uzaktan PowerShell aracılığıyla yapılabilir. Bilgisayarın uzaktan yönetilmesine doğru hazırlanması gerekir.

Varsayılan olarak, WinRM hizmeti el ile başlatma için yapılandırılır. Başlangıç türü ayarlanmalıdır *otomatik* ve hizmetin başlatılması. WinRM hizmetinin çalıştığını doğrulamak için durum özelliğinin değeri olmalıdır *çalıştıran*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell uzaktan iletişim için yapılandırılmalıdır.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Makine artık uzaktan - aracı yüklemesinden başlatma yönetilebilir. Örneğin, aşağıdaki betik aracısını uzak makineye kopyalar ve yükler.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için Azure Backup için Windows Server'a/istemcisine bakın

* [Azure Backup'a giriş](backup-introduction-to-azure-backup.md)
* [Windows sunucularını yedekleme](backup-configure-vault.md)
