---
title: "Azure yedekleme: DPM iş yüklerini yedeklemeye kullanım PowerShell | Microsoft Docs"
description: "Dağıtma ve Data Protection Manager (PowerShell kullanarak DPM için) Azure Backup yönetme hakkında bilgi edinin"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>PowerShell kullanarak Data Protection Manager (DPM) sunucuları için Azure’a yedekleme dağıtma ve yönetme
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Klasik](backup-dpm-automation-classic.md)
>
>

Bu makalede PowerShell yedeklemek ve bir yedekleme Kasası'nı DPM verileri kurtarmak için nasıl kullanılacağı açıklanmaktadır. Microsoft, tüm yeni dağıtımlar için kurtarma Hizmetleri kasalarının kullanılmasını önerir. Yeni bir Azure yedekleme kullanıcı, makale kullanırsanız [dağıtma ve PowerShell kullanarak Azure Data Protection Manager verilerini yönetmek](backup-dpm-automation.md), bir kurtarma Hizmetleri kasasına verilerinizi depolamak için.

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir. 15 Ekim 2017’den itibaren, PowerShell kullanarak Backup kasaları oluşturamayacaksınız. **1 Kasım 2017’ye kadar**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="setting-up-the-powershell-environment"></a>PowerShell ortamını ayarlama
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Yedeklemeler için Azure Data Protection Manager'dan yönetmek için PowerShell kullanmadan önce PowerShell'de sağ ortama sahip olmanız gerekir. PowerShell oturumu başlangıcında sağ modülleri alın ve doğru DPM cmdlet başvuru izin vermek için aşağıdaki komutu çalıştırdığınızdan emin olun:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Kurulumu'nu ve kaydı
Başlamak için:

1. [En son PowerShell indirme](https://github.com/Azure/azure-powershell/releases) (gerekli minimum sürüm: 1.0.0)
2. Azure Backup cmdlet'leri geçerek etkinleştirmek *AzureResourceManager* kullanarak moduna **Switch-AzureMode** komutunu:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

Aşağıdaki kurulumunu ve kaydını görevleri PowerShell ile otomatik olarak yapılabilir:

* Yedekleme kasası oluşturma
* Azure Backup aracısını yükleme
* Azure Backup hizmeti ile kaydetme
* Ağ ayarları
* Şifreleme ayarları

### <a name="create-a-backup-vault"></a>Yedekleme kasası oluşturma
> [!WARNING]
> İlk olarak Azure Yedekleme'yi kullanarak müşteriler için aboneliğiniz ile birlikte kullanılacak Azure Backup sağlayıcı kaydetmeniz gerekir. Bu aşağıdaki komutu çalıştırarak yapılabilir: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"
>
>

Kullanarak yeni bir yedekleme kasası oluşturma **yeni AzureRMBackupVault** komutunu. Yedekleme kasası bir ARM kaynak olduğundan, bir kaynak grubu içindeki yerleştirmeniz gerekir. Yükseltilmiş bir Azure PowerShell konsolunda aşağıdaki komutları çalıştırın:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

Belirtilen abonelik kullanarak tüm yedekleme kasalarının listesi elde edebilirsiniz **Get-AzureRMBackupVault** komutunu.

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>Bir DPM sunucusu üzerinde Azure Backup aracısını yükleme
Azure Backup aracısını yüklemeden önce Windows Server yükleyici indirildi ve mevcut olması gerekir. Yükleyicisi'nden en son sürümünü almak [Microsoft Download Center](http://aka.ms/azurebackup_agent) veya yedekleme kasasının Pano sayfası. Yükleyici gibi kolay erişilebilir bir konuma kaydedin * C:\Downloads\*.

Aracıyı yüklemek için yükseltilmiş bir PowerShell konsolunda aşağıdaki komutu çalıştırın **DPM sunucusundaki**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Bu, tüm varsayılan seçeneklerle Aracısı'nı yükler. Yükleme arka planda birkaç dakika sürer. Belirtmezseniz, */nu* seçeneği **Windows Update** tüm güncelleştirmeleri denetlemek için yükleme işleminin sonunda penceresi açılır.

Aracı yüklü programlar listesinde gösterilir. Yüklü programların listesini görmek için şu adrese gidin **Denetim Masası** > **programları** > **programlar ve Özellikler**.

![Yüklü aracı](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Yükleme Seçenekleri
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

### <a name="registering-with-the-azure-backup-service"></a>Azure Backup hizmeti ile kaydetme
Azure Backup hizmeti ile kaydedebilmek için emin olmak gereken [Önkoşullar](backup-azure-dpm-introduction.md) karşılanır. Yapmanız gerekir:

* Geçerli bir Azure aboneliğiniz
* Bir yedekleme kasası sahip

Kasa kimlik bilgilerini indirmek için çalıştırın **Get-AzureBackupVaultCredentials** uygun bir konumda ister bir Azure PowerShell konsolunda ve deposu komutunu * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Makine kasası ile kaydediliyor yapılır kullanarak [başlangıç DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

Bu Microsoft Azure belirtilen kasa kimlik bilgilerini kullanarak kasası "TestingServer" adlı DPM sunucusuna kaydeder.

> [!IMPORTANT]
> Kasa kimlik bilgileri dosyası belirtmek için göreli yolların kullanmayın. Cmdlet'i bir girdi olarak mutlak bir yol sağlamalısınız.
>
>

### <a name="initial-configuration-settings"></a>İlk yapılandırma ayarları
DPM sunucusu Azure yedekleme kasası ile kaydedildikten sonra varsayılan abonelik ayarlarını başlayacaktır. Bu abonelik ayarlar, ağ, şifreleme ve hazırlama alanı içerir. Önce bir tanıtıcı kullanarak mevcut (varsayılan) ayarları almanız abonelik ayarlarını değiştirme başlamaya [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Bu yerel PowerShell nesneye yapılan değişikliklerden ```$setting``` ve ardından tam nesne DPM ve bunları kaydetmek için Azure Backup için kararlıdır kullanarak [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i. Kullanmanız gereken ```–Commit``` değişiklikler kalıcı emin olmak için bayrak. Ayarları değil uygulanan ve olması kaydedilen sürece Azure yedekleme tarafından kullanılır.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Ağ
Azure Backup hizmeti internet üzerindeki DPM makineye bağlantısını bir proxy sunucu üzerinden ise, Ara sunucu ayarlarını yedeklemelerinin başarılı olması için sağlanmalıdır. Bu kullanılarak yapılır ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` ve ```ProxyPassword``` parametrelerle [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i. Bu örnekte, olmadığından hiçbir Ara sunucunun biz açıkça herhangi bir proxy ile ilgili bilgi temizleme.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bant genişliği kullanımı ile seçenekleri de denetlenebilir ```-WorkHourBandwidth``` ve ```-NonWorkHourBandwidth``` haftanın belirli bir dizi için. Bu örnekte, biz herhangi azaltma ayarlıyorsanız değil.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a>Hazırlama alanı yapılandırma
DPM sunucusunda çalışan Azure Yedekleme aracısı, (yerel hazırlama alanı) buluttan geri yüklenen veriler için geçici depolama gerekir. Hazırlama alanı kullanarak yapılandırma [kümesi DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet'i ve ```-StagingAreaPath``` parametresi.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

Yukarıdaki örnekte, hazırlama alanı ayarlanacak *C:\StagingArea* PowerShell nesnesindeki ```$setting```. Belirtilen klasör zaten var, aksi takdirde Abonelik ayarları son yürütme başarısız olur emin olun.

### <a name="encryption-settings"></a>Şifreleme ayarları
Yedekleme verilerini Azure Backup için gönderilen veri gizliliğini korumak için şifrelenir. Şifreleme Parolası "geri yükleme sırasındaki verileri şifrelemek için parola" dir. Ayarlandıktan sonra bu bilgileri güvenli tutmak önemlidir.

Aşağıdaki örnekte, ilk komut dizesini sayıya dönüştürür ```passphrase123456789``` güvenli bir dize ve güvenli dize değişkenine adlı atar ```$Passphrase```. İkinci komut güvenli dize ayarlar ```$Passphrase``` yedeklemeleri şifrelemek için parolayı olarak.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> Ayarlandıktan sonra parola bilgilerini güvenli ve güvenli tutar. Bu parola Azure kaynağından veri geri olmaz.
>
>

Bu noktada, tüm gerekli değişiklikleri yaptınız ```$setting``` nesnesi. Değişiklikleri kaydetmek unutmayın.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Azure yedekleme verilerini koruma
Bu bölümde, bir üretim sunucusunu DPM sunucusuna Ekle ve ardından yerel DPM depolama ve ardından Azure yedekleme verileri koruyun. Örneklerde dosyaları ve klasörleri yedeklemek göstereceğiz. Mantığı, herhangi bir DPM desteklenen veri kaynağı yedeklemek için kolayca genişletilebilir. Tüm DPM yedeklemelerini dört bölümden ile koruma grubu (PG) tarafından yönetilir:

1. **Grup üyeleri** korunabilir tüm nesnelerin bir listesi verilmiştir (olarak da bilinen *veri kaynakları* dpm'de) aynı koruma grubunda korumak istediğiniz. Örneğin, farklı yedekleme gereksinimleri olabilir olarak üretim sanal makineleri bir koruma grubu ve SQL Server veritabanlarını başka bir koruma grubunda korumak isteyebilirsiniz. Herhangi bir veri kaynağı emin olmak için gereken bir üretim sunucusunda yedeklenmeden önce DPM aracısını sunucuda yüklü ve DPM tarafından yönetilir. Adımları için [DPM aracısını yüklemeye](https://technet.microsoft.com/library/bb870935.aspx) ve uygun DPM sunucusuna bağlama.
2. **Veri koruma yöntemini** hedef yedekleme konumları - bant, disk ve bulut belirtir. Bizim örneğimizde biz verileri yerel diske ve bulut için koruma sağlar.
3. A **yedekleme zamanlaması** yedeklemeler yapılması gerektiğinde ve verileri DPM sunucusu ve üretim sunucusu arasında ne sıklıkla eşitlenmesi gerektiği belirtir.
4. A **bekletme zamanlaması** belirleyen ne kadar süreyle Azure kurtarma noktalarını korumak için.

### <a name="creating-a-protection-group"></a>Koruma grubu oluşturma
Başlangıç kullanarak yeni bir koruma grubu oluşturarak [yeni DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet'i.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

Yukarıdaki cmdlet adlı bir koruma grubu oluşturacak *ProtectGroup01*. Mevcut bir koruma grubunun de daha sonra Azure bulut yedekleme eklemek için değiştirilebilir. Ancak, koruma grubuna - herhangi bir değişiklik yapmak için yeni veya varolan - bir tanıtıcı almaya ihtiyacımız bir *değiştirilebilir* kullanarak nesne [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet'i.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Grup üyeleri koruma grubuna ekleniyor
Her DPM aracısının yüklü olduğu sunucuda veri kaynakları listesi bilir. Bir veri kaynağını koruma grubuna eklemek için DPM Aracısı önce veri kaynaklarının listesini DPM sunucusuna geri göndermesi gerekir. Bir veya daha fazla veri kaynakları sonra seçili ve koruma grubuna eklenir. Almak için gerekli olan PowerShell adımları elde etmek bu şunlardır:

1. DPM Aracısı üzerinden DPM tarafından yönetilen tüm sunucuların listesi getirilemedi.
2. Belirli bir sunucu seçin.
3. Tüm veri kaynakları listesini sunucuda getirin.
4. Bir veya daha fazla veri kaynakları seçin ve bunları koruma grubuna Ekle

DPM aracısının yüklü olduğundan ve DPM sunucusu tarafından yönetilen sunucuları listesi ile edinilen [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet'i. Biz filtrelemek ve yalnızca PS adıyla yapılandırın, bu örnekte *productionserver01* yedekleme için.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Şimdi üzerinde veri kaynaklarının listesini getirme ```$server``` kullanarak [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet'i. Bu örnekte şu birim için filtre * D:\* yedekleme için yapılandırmak istediğiniz. Bu veri kaynağı sonra koruma grubuna eklenen kullanarak [Ekle DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet'i. Kullanmayı unutmayın *modifable* koruma grubu nesnesi ```$MPG``` eklemeleri yapma.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Tüm seçilen veri kaynakları koruma grubuna ekleyene kadar gerektiği gibi birçok kez bu adımı yineleyin. Yalnızca bir datasource ile başlatın ve koruma grubunu oluşturmak için iş akışı tamamlamak ve sonraki bir noktada daha fazla veri kaynakları koruma grubuna ekleyin.

### <a name="selecting-the-data-protection-method"></a>Veri koruma yöntemini seçme
Veri kaynakları koruma grubuna eklendikten sonra sonraki adım koruma yöntemini kullanarak belirtmektir [kümesi DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet'i. Bu örnekte, kurulumu yerel disk ve bulut yedekleme için koruma grubu olacaktır. Ayrıca kullanarak buluta korumak istediğiniz veri kaynağını belirtmek zorunda [Ekle DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet'iyle - çevrimiçi bayrağı.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Bekletme aralığını ayarlama
Kullanarak yedekleme noktaları için bekletme ayarlamak [kümesi DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet'i. Kullanarak yedekleme zamanlaması tanımlanmış önce bekletme ayarlamak tek görünebilir ancak ```Set-DPMPolicyObjective``` cmdlet'i sonra değiştirilebilen varsayılan yedekleme zamanlaması otomatik olarak ayarlar. Yedeklemeyi zamanlama ilk kümesi ve sonra bekletme ilkesi için her zaman mümkündür.

Aşağıdaki örnek cmdlet disk yedeklemeleri bekletme parametreleri ayarlar. Bu yedeklemeler 10 gün ve eşitleme verileri için 6 saatte bir ve üretim sunucusu DPM sunucusu arasında korur. ```SynchronizationFrequencyMinutes``` Ne sıklıkta bir yedekleme noktasının, ancak nasıl oluşturulduğunu tanımlamıyor genellikle verileri DPM sunucusuna kopyalanır; bu yedeklemeler çok büyük hale gelmesini engeller.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Azure'a giden yedeklemeleri (DPM başvurduğu çevrimiçi yedeklemeler olarak) bekletme aralıkları için yapılandırılabilir [üst öğe Son şeması (GFS) kullanarak bekletme'uzun süreli](backup-azure-backup-cloud-as-tape.md). Diğer bir deyişle, günlük, haftalık, aylık ve yıllık bekletme ilkeleri içeren bir birleşik bekletme ilkesi tanımlayabilirsiniz. Bu örnekte, biz istiyoruz karmaşık bekletme düzeni temsil eden bir dizi oluşturmak ve bekletme aralığı kullanarak yapılandırma [kümesi DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet'i.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Yedekleme zamanlamasını ayarlayın
Koruma hedefi kullanarak belirtirseniz, DPM bir varsayılan yedekleme zamanlaması otomatik olarak ayarlar. ```Set-DPMPolicyObjective``` cmdlet'i. Varsayılan zamanlamaların değiştirmek için kullanın [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet arkasından [kümesi DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet'i.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

Yukarıdaki örnekte ```$onlineSch``` GFS düzeni koruma grubunda varolan çevrimiçi koruma zamanlamasını içeren bir dizi dört öğelerle:

1. ```$onlineSch[0]```Günlük Zamanlama içerir
2. ```$onlineSch[1]```Haftalık Zamanlama içerir
3. ```$onlineSch[2]```Aylık zamanlama içerir
4. ```$onlineSch[3]```Yıllık zamanlama içerir

Haftalık Zamanlama değiştirmeniz gerekiyorsa, başvurmanız gerekir böylece ```$onlineSch[1]```.

### <a name="initial-backup"></a>İlk yedekleme
Bir veri kaynağı için ilk kez yedeklerken, DPM DPM çoğaltma birimi üzerinde korunacak veri kaynağı bir kopyasını oluşturacak olan bir ilk çoğaltma oluşturması gerekir. Bu etkinlik için belirli bir saat ya da zamanlanabilir veya kullanarak el ile tetiklenebilir [kümesi DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) parametresi cmdlet'iyle ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>DPM çoğaltma ve kurtarma noktası biriminin boyutunu değiştirme
Ayrıca DPM çoğaltma birimi yanı sıra birim gölge kopya kullanarak boyutunu değiştirebilirsiniz [kümesi DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet gibi örnek aşağıda: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - Protectiongroup'u $MPG-el ile - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Koruma grubu değişiklikleri işleme
Son olarak, değişiklikleri DPM yeni koruma grubu yapılandırması başına yedek almadan önce kaydedilmiş olması gerekir. Bu yapılır kullanarak [kümesi DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet'i.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a>Yedekleme noktaları görüntüleyin
Kullanabileceğiniz [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) bir veri kaynağı için tüm kurtarma noktalarının bir listesini almak için cmdlet. Bu örnekte, şunları yapacağız:

* bir dizi depolanan DPM sunucusundaki tüm PGs getirme```$PG```
* karşılık gelen veri kaynakları alma```$PG[0]```
* veri kaynağı için tüm kurtarma noktalarının alın.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Azure üzerinde korunan verileri geri yükleme
Verileri geri birleşimidir bir ```RecoverableItem``` nesne ve ```RecoveryOption``` nesne. Önceki bölümde biz bir veri kaynağı için yedekleme noktalarının bir listesini aldı.

Aşağıdaki örnekte, kurtarma için hedef yedekleme noktaları birleştirerek bir Hyper-V sanal makinesi Azure Yedekleme'den geri yükleme göstermektedir. Buna aşağıdakiler dahildir:

* Kurtarma seçeneğini kullanarak oluşturma [yeni DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet'i.
* Kullanarak yedekleme noktaları dizisi getirme ```Get-DPMRecoveryPoint``` cmdlet'i.
* Geri yüklemek için bir yedekleme noktasının seçme.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

Komutları, herhangi bir veri kaynağı türü için kolayca genişletilebilir.

## <a name="next-steps"></a>Sonraki adımlar
* DPM Azure yedekleme hakkında daha fazla bilgi için bkz: [DPM yedekleme giriş](backup-azure-dpm-introduction.md)
