---
title: Azure Backup - DPM iş yüklerini yedeklemek için PowerShell kullanma
description: Data Protection Manager (PowerShell kullanarak DPM için) Azure Backup'ı yönetme ve dağıtma hakkında bilgi edinin
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 1/23/2017
ms.author: adigan
ms.openlocfilehash: b16963265c971e604f03b51fd63f7fe411bab36e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66127773"
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>PowerShell kullanarak Data Protection Manager (DPM) sunucuları için Azure’a yedekleme dağıtma ve yönetme

Bu makalede, bir DPM sunucusundaki Azure Backup kurulumu için PowerShell kullanın ve yedekleme ve kurtarma yönetmek için nasıl gösterir.

## <a name="setting-up-the-powershell-environment"></a>PowerShell ortamını ayarlama

Yedeklemeleri Data Protection Manager'dan Azure'a yönetmek için PowerShell kullanmadan önce PowerShell'de doğru ortamı seçtiğinizden gerekir. PowerShell oturumunun başlangıcında, doğru modülleri içeri aktarmak ve doğru DPM cmdlet başvuru izin vermek için aşağıdaki komutu çalıştırdığınızdan emin olun:

```powershell
& "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"
```

```Output
Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Kurulumu ve kaydı

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Başlamak için [en son Azure PowerShell indirme](/powershell/azure/install-az-ps).

PowerShell ile aşağıdaki kurulumu ve kaydı görevleri otomatik hale getirilebilir:

* Kurtarma Hizmetleri kasası oluşturma
* Azure Backup aracısını yükleme
* Azure Backup hizmeti ile kaydetme
* Ağ ayarları
* Şifreleme ayarları

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Aşağıdaki adımlar bir kurtarma Hizmetleri kasası oluşturma işleminde yol. Bir Backup kasasının kurtarma Hizmetleri kasası farklıdır.

1. Azure Backup'ı ilk kez kullanıyorsanız, kullanmalısınız **Register-AzResourceProvider** Azure kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydetmeniz için cmdlet'i.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. Kurtarma Hizmetleri kasası bir ARM kaynağı olduğundan, bir kaynak grubu içinde yerleştirmeniz gerekir. Mevcut bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Yeni bir kaynak grubu oluştururken, kaynak grubunun konumunu ve adını belirtin.

    ```powershell
    New-AzResourceGroup –Name "test-rg" –Location "West US"
    ```

3. Kullanım **yeni AzRecoveryServicesVault** cmdlet'i yeni bir kasa oluşturun. Kasayla aynı konumda kaynak grubu için kullanılan belirttiğinizden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```

4. Kullanılacak depolama yedekliliği türü; belirtin. kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy-grs.md). Aşağıdaki örnek, testVault - BackupStorageRedundancy seçeneği GeoRedundant için ayarlanmış gösterir.

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name "testVault"
    Set-AzRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikte kasalarını görüntüle

Kullanım **Get-AzRecoveryServicesVault** geçerli abonelikte tüm kasalarının listesi görüntülemek için. Yeni bir kasa oluşturulduğunu denetleyin ya da hangi kasaları abonelikte kullanılabilir olduğunu görmek için bu komutu kullanabilirsiniz.

Get-AzRecoveryServicesVault komutu çalıştırın ve Abonelikteki tüm kasalar listelenir.

```powershell
Get-AzRecoveryServicesVault
```

```Output
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Azure Backup aracısını, bir DPM sunucusuna yükleme

Azure Backup aracısını yüklemeden önce Windows Server yükleyici, indirilen ve mevcut olması gerekir. Yükleyicisi'nden en son sürümünü edinebilirsiniz [Microsoft Download Center](https://aka.ms/azurebackup_agent) veya kurtarma Hizmetleri kasasının Pano sayfası. Yükleyici gibi kolayca erişilebilir bir konuma kaydedin * C:\Downloads\*.

Aracıyı yüklemek için yükseltilmiş bir PowerShell konsolunda aşağıdaki komutu çalıştırın **DPM sunucusundaki**:

```powershell
MARSAgentInstaller.exe /q
```

Bu, tüm varsayılan seçeneklerle aracısını yükler. Yükleme arka planda birkaç dakika sürer. Siz belirtmezseniz */nu* seçeneği **Windows Update** sonunda, tüm güncelleştirmeleri denetlemek için bir yükleme penceresi açılır.

Aracı yüklü programlar listesinde gösterilir. Yüklü programlar listesinde görmek için Git **Denetim Masası** > **programlar** > **programlar ve Özellikler**.

![Yüklenen aracı](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Yükleme Seçenekleri

Komut satırı kullanılabilen seçenekleri görmek için aşağıdaki komutu kullanın:

```powershell
MARSAgentInstaller.exe /?
```

Kullanılabilir seçenekler şunlardır:

| Seçenek | Ayrıntılar | Varsayılan |
| --- | --- | --- |
| /q |Sessiz yükleme |- |
| / p: "Konum" |Azure Backup Aracısı yükleme klasörünün yolu. |C:\Program Files\Microsoft Azure Recovery Services Agent |
| / s: "Konum" |Azure Backup aracısı için önbellek klasörünün yolu. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Microsoft Update'e katılım |- |
| /nu |Yükleme tamamlandıktan sonra güncelleştirmeleri denetleme |- |
| /d |Microsoft Azure kurtarma Hizmetleri Aracısı'nı kaldırır |- |
| /ph |Proxy ana bilgisayar adresi |- |
| /PO |Proxy ana bilgisayar bağlantı noktası numarası |- |
| /PU |Proxy ana kullanıcı adı |- |
| /pw |Proxy parola |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a>Kayıt DPM bir kurtarma Hizmetleri kasası

Kurtarma Hizmetleri kasası oluşturduktan sonra en son aracı ve kasa kimlik bilgileri indirin ve C:\Downloads gibi uygun bir konumda depolayın.

```powershell
$credspath = "C:\downloads"
$credsfilename = Get-AzRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
$credsfilename
```

```Output
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

DPM sunucusunda çalıştırın [başlangıç OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) makine kasaya kaydetmek için cmdlet'i.

```powershell
$cred = $credspath + $credsfilename
Start-OBRegistration-VaultCredentials $cred -Confirm:$false
```

```Output
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a>İlk yapılandırma ayarları

DPM sunucusunu kurtarma Hizmetleri kasasına kayıtlı sonra varsayılan Abonelik ayarları ile başlar. Bu abonelik ayarları, ağ, şifreleme ve hazırlama alanı içerir. Önce bir tanıtıcı kullanarak mevcut (varsayılan) ayarlarını almanız gereken abonelik ayarlarını değiştirmek için [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```powershell
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Bu yerel bir PowerShell nesnesine yapılan tüm değişiklikleri ```$setting``` ve ardından tam nesne DPM ve Azure Backup için bunları kaydetmek için kararlıdır kullanarak [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i. Kullanmanız gereken ```–Commit``` değişiklikleri kalıcı olmasını sağlamak için bayrağı. Ayarları değil uygulanan ve kaydedilen sürece Azure Backup tarafından kullanılır.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a>Ağ

Azure Backup hizmeti internet üzerindeki DPM makinenin bağlantı bir proxy sunucu üzerinden ise, Ara sunucu ayarlarını, başarılı yedeklemeler için sağlanmalıdır. Bu kullanılarak yapılır ```-ProxyServer```ve ```-ProxyPort```, ```-ProxyUsername``` ve ```ProxyPassword``` parametrelerle [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i. Bu örnekte, proxy sunucusu yok biz açıkça herhangi bir ara sunucu ile ilgili bilgi temizlemek için.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bant genişliği kullanımı ile seçeneklerini de denetlenebilir ```-WorkHourBandwidth``` ve ```-NonWorkHourBandwidth``` için haftanın günlerini belirli bir dizi. Bu örnekte biz herhangi bir kısıtlama ayarı bulunamadı.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a>Hazırlama alanını yapılandırma

Azure Backup aracısını DPM sunucusunda çalışan (yerel hazırlama alanı) buluttan geri yüklenen veriler için geçici depolama gerekir. Hazırlama alanı kullanarak yapılandırma [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i ve ```-StagingAreaPath``` parametresi.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

Yukarıdaki örnekte, hazırlama alanını ayarlanacak *C:\StagingArea* PowerShell nesnesinde ```$setting```. Belirtilen klasör zaten var, aksi takdirde, Abonelik ayarları son işlemesi başarısız olur emin olun.

### <a name="encryption-settings"></a>Şifreleme ayarları

Verilerin gizliliğini korumak için Azure Backup için gönderilen yedekleme verileri şifrelenir. Şifreleme Parolası "geri yükleme sırasında verilerin şifresini çözmek için parola" dir. Ayarlandıktan sonra bu bilgiyi güvenli tutmak önemlidir.

Aşağıdaki örnekte, ilk komut dizesi dönüştürür ```passphrase123456789``` güvenli bir dize ve adlı güvenli dize değişkenine atar ```$Passphrase```. İkinci komut güvenli dize ayarlar ```$Passphrase``` yedeklemeleri şifrelemek için parolası.

```powershell
$Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Ayarlandıktan sonra parolayı bilgilerin korunmasına ve güvende tutun. Veri bu parolayı Azure'dan geri yükleme olanağınız olmayacaktır.
>
>

Bu noktada, tüm gerekli değişiklikleri yapılan ```$setting``` nesne. Değişiklikleri kaydetmeyi unutmayın.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Azure yedekleme verilerini koruma

Bu bölümde, bir üretim sunucusunu DPM sunucusuna Ekle ve ardından yerel DPM depolama alanı ve ardından Azure yedekleme verileri koruyun. Örneklerde, şu dosya ve klasörleri yedekleme işlemi gösterilecektir. Mantık, yedeklemek istediğiniz DPM tarafından desteklenen veri kaynağı için kolayca genişletilebilir. Tüm DPM yedeklemelerini dört bölümden ile koruma grubu (PG) tarafından yönetilir:

1. **Grup üyeleri** korunabilir tüm nesneleri listesi verilmiştir (olarak da bilinen *veri kaynakları* dpm'de), aynı koruma grubunda korumak istediğiniz. Örneğin, yedekleme farklı gereksinimlere sahip, üretim sanal makineleri bir koruma grubunun ve SQL Server veritabanlarını başka bir koruma grubundaki korumak isteyebilirsiniz. Herhangi bir veri kaynağı emin olmak için gereken bir üretim sunucusunda yedekleyebilmeniz için önce DPM aracısını sunucuda yüklü ve DPM tarafından yönetilir. İçin adımları [DPM aracısını yükleme](https://technet.microsoft.com/library/bb870935.aspx) ve uygun DPM sunucusuna bağlama.
2. **Veri koruma yöntemini** hedef yedekleme konumları - bant, disk ve bulut belirtir. Bizim örneğimizde, biz yerel diske ve bulut veri korur.
3. A **yedekleme zamanlaması** yedeklemeleri yapılması gerektiğinde ve verileri üretim sunucusu DPM sunucusuna arasında ne kadar sıklıkla eşitlenmesi gerektiği belirtir.
4. A **saklama zamanlaması** belirten ne kadar süreyle azure'da kurtarma noktalarını tutmak isteyip.

### <a name="creating-a-protection-group"></a>Koruma grubu oluşturma

Kullanarak yeni bir koruma grubu oluşturarak başlayın [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet'i.

```powershell
$PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Yukarıdaki cmdlet adlı bir koruma grubu oluşturacak *ProtectGroup01*. Mevcut bir koruma grubuna ayrıca daha sonra Azure bulutuna yedekleme eklemek için değiştirilebilir. Ancak, koruma grubuna - değişiklik yapmak için yeni veya mevcut - bir tanıtıcı alınacağı ihtiyacımız bir *değiştirilebilir* kullanarak nesne [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet'i.

```powershell
$MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Grup üyeleri koruma grubuna ekleniyor

Her DPM aracısının yüklü olduğu sunucuda veri kaynakları listesini bilir. Veri kaynağı bir koruma grubuna eklemek için ilk veri kaynaklarının listesini DPM sunucusuna geri göndermek DPM Aracısı gerekir. Bir veya daha fazla veri kaynakları sonra seçili ve koruma gruba eklenir. Bunu başarmak için gerekli olan PowerShell adımları şunlardır:

1. DPM DPM Aracısı aracılığıyla tarafından yönetilen tüm sunucuların listesi getirilemedi.
2. Belirli bir sunucu seçin.
3. Sunucuda tüm veri kaynakları listesini getirir.
4. Bir veya daha fazla veri kaynaklarını seçin ve bunları koruma grubuna Ekle

DPM aracısının yüklü ve DPM sunucusu tarafından yönetilen sunucuların listesi ile alınan [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet'i. Bu örnekte biz filtrelemek ve yalnızca PS adıyla yapılandırın *productionserver01* yedekleme.

```powershell
$server = Get-ProductionServer -DPMServerName "TestingServer" | Where-Object {($_.servername) –contains “productionserver01”}
```

Artık bulunan veri kaynakları listesini getirme ```$server``` kullanarak [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet'i. Bu örnekte biz birimi için filtre *D:\\*  , yedeklemeyi yapılandırmak istiyoruz. Bu veri kaynağı sonra koruma grubuna eklenen kullanarak [Ekle DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet'i. Kullanmayı unutmayın *değiştirilebilir* koruma grubu nesnesi ```$MPG``` ekleme yapma.

```powershell
$DS = Get-Datasource -ProductionServer $server -Inquire | Where-Object { $_.Name -contains “D:\” }

Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Seçilen tüm veri kaynaklarını koruma grubuna ekleyene kadar bu gerektiği gibi birçok kez tekrarlayın. Yalnızca bir datasource ile başlatın ve koruma grubunu oluşturmak için iş akışını tamamlamak ve ileride daha fazla veri kaynaklarını koruma grubuna ekleyin.

### <a name="selecting-the-data-protection-method"></a>Veri koruma yöntemini seçme

Veri kaynaklarını koruma grubuna eklendikten sonra sonraki adıma koruma yöntemini kullanarak belirlemektir [kümesi DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet'i. Bu örnekte, kurulumu yerel disk ile bulut yedekleme için koruma grubudur. Ayrıca kullanarak buluta korumak istediğiniz veri kaynağı belirtmeniz gerekir [Ekle DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet'iyle - Online bayrağı.

```powershell
Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Bekletme aralığı ayarlama

Kullanarak yedekleme noktaları için bekletme ayarlamak [kümesi DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet'i. Bekletme kullanarak yedekleme zamanlaması tanımlanmadan önce ayarlamak tek görünebilir ancak ```Set-DPMPolicyObjective``` cmdlet'i, daha sonra değiştirilebilir bir varsayılan yedekleme zamanlaması otomatik olarak ayarlar. Her zaman, Yedekleme Zamanlama ilk ayarlama ve sonra bekletme ilkesi mümkündür.

Aşağıdaki örnekte, cmdlet for disk backups bekletme parametreleri ayarlar. Bu yedeklemeler 10 gün ve verileri eşitlemek için 6 saatte bir üretim sunucusu DPM sunucusu arasında korur. ```SynchronizationFrequencyMinutes``` Ne sıklıkta bir yedekleme noktasının, ancak nasıl oluşturulduğunu tanımlamıyor genellikle verileri DPM sunucusuna kopyalanır.  Bu ayar, yedeklemeler çok büyük hale gelmesini önler.

```powershell
Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Azure'a giden yedeklemeler için (DPM başvuruyor kendisine çevrimiçi yedeklemeler) için saklama aralığı yapılandırılabilir [Dedenizin bırak Son şeması (GFS) kullanılarak bekletme'uzun süreli](backup-azure-backup-cloud-as-tape.md). Diğer bir deyişle, günlük, haftalık, aylık ve yıllık bekletme ilkeleri içeren bir birleşik bekletme ilkesi tanımlayabilirsiniz. Bu örnekte, biz istiyoruz karmaşık bir bekletme düzeni temsil eden bir dizi oluşturun ve ardından bekletme aralığı kullanarak yapılandırma [kümesi DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet'i.

```powershell
$RRlist = @()
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Yedekleme zamanlaması Ayarla

Koruma hedefi kullanarak belirtirseniz, DPM bir varsayılan yedekleme zamanlaması otomatik olarak ayarlar. ```Set-DPMPolicyObjective``` cmdlet'i. Varsayılan zamanlamalarını değiştirmek için kullanın [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet'ini ve ardından [kümesi DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet'i.

```powershell
$onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

Yukarıdaki örnekte, ```$onlineSch``` GFS düzeni koruma grubunda mevcut çevrimiçi koruma zamanlamasını içeren bir dizi dört öğe ile:

1. ```$onlineSch[0]``` Günlük Zamanlama içerir
2. ```$onlineSch[1]``` Haftalık Zamanlama içerir
3. ```$onlineSch[2]``` Aylık zamanlama içerir
4. ```$onlineSch[3]``` Yıllık çizelge içerir

Haftalık Zamanlama değiştirmeniz gerekiyorsa, başvurmanız gerekir böylece ```$onlineSch[1]```.

### <a name="initial-backup"></a>İlk yedekleme

Bir veri kaynağı bir yedekleme ilk kez, DPM ihtiyaçları için ilk çoğaltma, oluşturduğunda, DPM çoğaltma birimi üzerinde korunacak veri kaynağı tam bir kopyasını oluşturur. Bu etkinlik belirli bir süre için zamanlanabilir veya el ile kullanarak tetiklenebilir [kümesi DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet'ini parametresiyle ```-NOW```.

```powershell
Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```

### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>DPM çoğaltma ve kurtarma noktası birimi boyutunu değiştirme

DPM çoğaltma biriminde ve gölge kopya birimi kullanılarak boyutunu da değiştirebilirsiniz [kümesi DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) aşağıdaki örnekte olduğu gibi cmdlet: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - Protectiongroup'u $MPG-el ile - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Koruma grubuna değişiklikler işleniyor

Son olarak, değişiklikleri DPM yeni koruma grubu yapılandırması başına yedekleme yapmadan önce kaydedilmiş olması gerekir. Bunu kullanarak gerçekleştirilebilir [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet'i.

```powershell
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

## <a name="view-the-backup-points"></a>Yedekleme noktaları görüntüleyin

Kullanabileceğiniz [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) bir veri kaynağı için tüm kurtarma noktalarının bir listesini almak için cmdlet. Bu örnekte, yapacağız:

* DPM sunucusunda PGs getirebilir ve bir dizide depolanan ```$PG```
* karşılık gelen veri kaynaklarını Al ```$PG[0]```
* bir veri kaynağı için tüm kurtarma noktalarını alın.

```powershell
$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Azure'da korunan verileri geri yükleme

Verilerin geri yüklenmesi bir birleşimi olan bir ```RecoverableItem``` nesnesi ve bir ```RecoveryOption``` nesne. Önceki bölümde, bir veri kaynağı için yedekleme noktaları listesini aldık.

Aşağıdaki örnekte, biz bir Hyper-V sanal makinesinin hedef kurtarma için yedekleme noktaları birleştirerek Azure Yedekleme'den geri yükleme gösterilmektedir. Bu örnek içerir:

* Kurtarma seçeneğini kullanarak oluşturma [yeni DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet'i.
* Yedekleme noktaları kullanarak dizi getirilirken ```Get-DPMRecoveryPoint``` cmdlet'i.
* Geri yükleme için bir yedekleme noktasının seçme.

```powershell
$RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Komutlar, herhangi bir veri kaynağı türü için kolayca genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Backup'a DPM hakkında daha fazla bilgi için bkz: [DPM Yedekleme'ye giriş](backup-azure-dpm-introduction.md)
