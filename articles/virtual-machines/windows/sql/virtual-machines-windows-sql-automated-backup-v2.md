---
title: SQL Server 2016/2017 Azure Vm'leri için yedekleme v2 otomatik | Microsoft Docs
description: SQL Server 2016/2017 Azure'da çalışan sanal makineler için otomatik yedekleme özelliğini açıklar. Bu makalede, Resource Manager kullanarak Vm'lere özeldir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: d03d4bd86367aa29bbf93062f7cc03f57f4cad83
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075962"
---
# <a name="automated-backup-v2-for-azure-virtual-machines-resource-manager"></a>Azure Virtual Machines'de (Resource Manager) için otomatik yedekleme v2

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik yedekleme v2 otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) Azure VM'deki SQL Server 2016/2017 Standard, Enterprise veya Developer sürümleri çalıştıran tüm mevcut ve yeni veritabanları için. Bu, kalıcı Azure blob depolama kullanan normal veritabanı yedeklemelerini yapılandırma sağlar. Otomatik yedekleme v2 bağlıdır [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Otomatik yedekleme v2 kullanmak için aşağıdaki önkoşulları gözden geçirin:

**İşletim sistemi**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü**:

- SQL Server 2016: Geliştirici, standart veya Kurumsal
- SQL Server 2017: Geliştirici, standart veya Kurumsal

> [!IMPORTANT]
> Otomatik yedekleme v2, SQL Server 2016 veya üzeri sürümleri çalışır. SQL Server 2014 kullanıyorsanız, veritabanlarınızı yedeklemek için otomatik yedekleme v1'i kullanabilirsiniz. Daha fazla bilgi için [otomatik yedekleme SQL Server 2014'ten Azure sanal makineleri için](virtual-machines-windows-sql-automated-backup.md).

**Veritabanı yapılandırması**:

- Hedef veritabanlarının tam kurtarma modelini kullanmanız gerekir. Daha fazla tam kurtarma modelinin etkisi hakkında yedeklemeler hakkında bilgi için [yedekleme altında tam kurtarma modeli](https://technet.microsoft.com/library/ms190217.aspx).
- Sistem veritabanlarının tam kurtarma modelini kullanmanız gerekmez. Ancak, günlük yedeklerinin modeli veya MSDB alınması gerekiyorsa, tam kurtarma modelini kullanmanız gerekir.
- Hedef veritabanları ya da varsayılan SQL Server örneğinde olmalıdır veya [yüklendiğinden](virtual-machines-windows-sql-server-iaas-faq.md#administration) adlandırılmış örneği. 

> [!NOTE]
> Otomatik yedekleme dayanır **SQL Server Iaas Aracısı uzantısı**. Geçerli SQL sanal makine galeri görüntüleri, varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Otomatik yedekleme v2 için yapılandırılabilir seçenekleri aşağıdaki tabloda açıklanmaktadır. Gerçek yapılandırma adımları, Azure portal veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak değişir.

### <a name="basic-settings"></a>Temel ayarları

| Ayar | Aralığı (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya SQL Server 2016/2017 Developer, Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama dönemi** | 1-30 gün (30 gün) | Yedeklemeleri saklanacağı gün sayısı. |
| **Depolama Hesabı** | Azure depolama hesabı | Otomatik yedekleme dosyalarını blob storage'da depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve veritabanı GUID'si içerir. |
| **Şifreleme** |Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkin olduğunda, bir yedeklemeyi geri yüklemek için kullanılan sertifikaları belirtilen depolama hesabında yer alır. Aynı kullanan **otomatik yedekleme** kapsayıcı adlandırma kuralını ile. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metin | Şifreleme anahtarları parolası. Bu parola yalnızca olan şifreleme etkinse gereken. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanıldı ilgili sertifika olmalıdır. |

### <a name="advanced-settings"></a>Gelişmiş ayarlar

| Ayar | Aralığı (varsayılan) | Açıklama |
| --- | --- | --- |
| **Sistem Veritabanı Yedeklemeleri** | Etkinleştir/devre dışı bırak (devre dışı) | Etkin olduğunda, bu özelliği sistem veritabanlarını da yedekler: Master, MSDB ve modeli. MSDB ve modeli veritabanları için günlük yedeklerinin alınması istiyorsanız, tam kurtarma modunda olduğundan emin olun. Günlük yedeklemeler için ana hiçbir zaman alınır. Ve yedekleme TempDB için alınır. |
| **Yedekleme zamanlaması** | El ile otomatik / (otomatik) | Varsayılan olarak, yedekleme zamanlaması üzerinde günlük büyümesini göre otomatik olarak belirlenir. El ile yedekleme zamanlaması yedeklemeler için zaman penceresi belirtmesini sağlar. Bu durumda, yedeklemeler yalnızca belirtilen sıklıkta ve belirtilen zaman çerçevesinde belirli bir gün gerçekleşir. |
| **Tam yedekleme sıklığı** | Günlük/Haftalık | Tam yedeklemelerinin sıklığını. Her iki durumda da, sonraki zamanlanmış zaman penceresi boyunca tam yedeklemeler başlayın. Haftalık seçildiğinde, yedeklemeleri tüm veritabanları başarıyla yedekledim kadar birden çok gün kapsayabilir. |
| **Tam yedekleme başlangıç saati** | 00:00 – 23:00 (01:00) | Başlangıç saati belirli bir gün boyunca tam yedeklemeler yer alabilir. |
| **Tam yedekleme zaman penceresi** | 1 – 23 saat (1 saat) | Belirli bir gün boyunca tam yedeklemeler gerçekleştirilebildiği zaman penceresi süresi. |
| **Günlük yedekleme sıklığı** | 5-60 dakika (60 dakika) | Günlük yedekleme sıklığı. |

## <a name="understanding-full-backup-frequency"></a>Tam yedekleme sıklığı anlama
Günlük ve haftalık tam yedeklemeler arasındaki farkı anlamak önemlidir. Aşağıdaki iki örnek senaryolar göz önünde bulundurun.

### <a name="scenario-1-weekly-backups"></a>Senaryo 1: Haftalık yedeklemeler
Büyük veritabanlarının sayısını içeren bir SQL Server VM'si var.

Pazartesi günü aşağıdaki ayarlarla otomatik yedekleme v2 etkinleştir:

- Yedekleme zamanlaması: **El ile**
- Tam yedekleme sıklığı: **Haftalık**
- Tam yedekleme başlangıç saati: **01:00**
- Tam yedekleme zaman penceresi: **1 saat**

Bu sonraki kullanılabilir Yedekleme penceresi 1 saat 1 sabah Salı olduğu anlamına gelir. Bu sırada, teker teker veritabanlarınızı bir yedeklemeyi otomatik yedekleme başlar. Bu senaryoda, veritabanlarınızı tam yedeklemeler için ilk birkaç veritabanlarını tamamlamak yeteri kadar büyük. Ancak, bir saat sonra tüm veritabanları yedeklendi.

Bu durumda, otomatik yedekleme sonraki 01'da bir saatlik Çarşamba günü kalan veritabanlarını yedekleme başlar. Tüm veritabanları, bu süre içinde yedeklenmiş, aynı anda yeniden ertesi gün çalışır. Tüm veritabanları başarıyla yedeklendi kadar bu devam eder.

Salı yeniden ulaştıktan sonra otomatik yedekleme, tüm veritabanlarını yeniden yedekleme başlar.

Bu senaryo, otomatik yedekleme, yalnızca belirtilen zaman aralığında çalışır ve her veritabanı, haftada bir kez yedeklenen gösterir. Bu, ayrıca burada tek bir gün içinde tüm yedeklemeler tamamlanması mümkün değildir durumda birden çok gün yedeklemeler için mümkün olduğunu gösterir.

### <a name="scenario-2-daily-backups"></a>Senaryo 2: Günlük yedekleme
Büyük veritabanlarının sayısını içeren bir SQL Server VM'si var.

Pazartesi günü aşağıdaki ayarlarla otomatik yedekleme v2 etkinleştir:

- Yedekleme zamanlaması: Manual
- Tam yedekleme sıklığı: Günlük
- Tam yedekleme başlangıç saati: 22:00
- Tam yedekleme zaman penceresi: 6 saat

Bu, yedekleme sonraki kullanılabilir pencerede 6 saat boyunca 10 PM, Pazartesi olduğu anlamına gelir. Bu sırada, teker teker veritabanlarınızı bir yedeklemeyi otomatik yedekleme başlar.

Ardından, 6 saat boyunca 10 Salı günü, tüm veritabanlarının tam yedeklemeler yeniden başlatın.

> [!IMPORTANT]
> Günlük yedekleme zamanlama, tüm veritabanları, bu süre içinde yedeklenebilir emin olmak için bir geniş bir zaman penceresi zamanlamak önerilir. Yedeklenecek veriler büyük miktarda sahip olduğu durumda bu durum özellikle önemlidir.

## <a name="configure-in-the-portal"></a>Portalda yapılandırma

Sağlama sırasında ya da mevcut SQL Server 2016/2017 Vm'leri için otomatik yedekleme v2 yapılandırmak için Azure portalını kullanabilirsiniz.

## <a name="configure-for-new-vms"></a>Yeni sanal makineler için yapılandırma

Resource Manager dağıtım modelinde, yeni bir SQL Server 2016 veya 2017 sanal makinesi oluşturduğunuzda, otomatik yedekleme v2 yapılandırmak için Azure portalını kullanın.

İçinde **SQL Server ayarları** sekmesinde **etkinleştirme** altında **otomatik yedekleme**. Aşağıdaki Azure portalı ekran görüntüsü gösterildiği **SQL otomatik yedekleme** ayarları.

![Azure portalında SQL otomatik yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Otomatik yedekleme v2, varsayılan olarak devre dışıdır.

## <a name="configure-existing-vms"></a>Varolan Vm'leri yapılandırma

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineleri için gidin [SQL sanal makineleri kaynak](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) seçip **yedeklemeleri** , otomatik yedekleri yapılandırmak için.

![Var olan sanal makineler için SQL otomatik yedekleme](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)


İşiniz bittiğinde tıklayın **Uygula** altındaki düğmesinde **yedeklemeleri** Ayarları sayfasında, yaptığınız değişiklikleri kaydetmek için.

Otomatik yedekleme ilk kez etkinleştirirseniz, Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında, otomatik yedekleme yapılandırıldığını göstermeyebilir. Aracının yüklü için yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portalında yeni ayarları ücreti yansıtılır.

## <a name="configure-with-powershell"></a>PowerShell ile yapılandırma

Otomatik yedekleme v2 yapılandırmak için PowerShell kullanabilirsiniz. Başlamadan önce şunları yapmalısınız:

- [En son Azure PowerShell'i yükleyip](https://aka.ms/webpi-azps).
- Windows PowerShell'i açın ve hesabınızla ilişkilendirmek **Connect AzAccount** komutu.

[!INCLUDE [updated-for-az.md](../../../../includes/updated-for-az.md)]

### <a name="install-the-sql-iaas-extension"></a>SQL Iaas uzantısı yükleme
Azure portalından bir SQL Server sanal makinesi sağladıysanız, SQL Server Iaas uzantısı zaten yüklü olması gerekir. Sanal makinenizin çağırarak yüklü olup olmadığını belirlemek **Get-AzVM** komut ve İnceleme **uzantıları** özelliği.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

SQL Server Iaas Aracısı uzantısı yüklü değilse, "Sqlıaasagent" veya "SQLIaaSExtension" olarak listelenen görmeniz gerekir. **ProvisioningState** için uzantı ayrıca "Başarılı" göstermelidir. 

Bunu yüklü değil veya sağlanması başarısız oldu, şu komutla yükleyin. Ek olarak VM adı ve kaynak grubu, ayrıca bölgeyi belirtmeniz gerekir ( **$region**), sanal makinenizin bulunur.

```powershell
$region = “EASTUS2”
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <a id="verifysettings"></a> Geçerli ayarları doğrulayın
Otomatik yedekleme sağlama sırasında etkinleştirilirse, geçerli yapılandırmayı denetlemek için PowerShell kullanabilirsiniz. Çalıştırma **Get-AzVMSqlServerExtension** inceleyin ve komutu **AutoBackupSettings** özelliği:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Aşağıdaki çıktıyı almalısınız:

```
Enable                      : True
EnableEncryption            : False
RetentionPeriod             : 30
StorageUrl                  : https://test.blob.core.windows.net/
StorageAccessKey            :  
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : Manual
FullBackupFrequency         : WEEKLY
FullBackupStartTime         : 2
FullBackupWindowHours       : 2
LogBackupFrequency          : 60
```

Çıkış gösteriliyorsa **etkinleştirme** ayarlanır **False**, otomatik yedeklemeyi etkinleştirmek sahip. Güzel bir haberimiz var etkinleştirin ve aynı şekilde otomatik yedeklemeyi yapılandırma ' dir. Bu bilgi için sonraki bölüme bakın.

> [!NOTE] 
> Hemen bir değişiklik yaptıktan sonra ayarlarını kontrol edin, eski yapılandırma değerlerini geri alırsınız mümkündür. Birkaç dakika bekleyin ve yeniden değişikliklerinizi uygulandığından emin olmak için ayarları kontrol edin.

### <a name="configure-automated-backup-v2"></a>Otomatik yedekleme v2 yapılandırın
Herhangi bir zamanda, yapılandırma ve davranışını değiştirmek de otomatik yedekleme sağlamak için PowerShell kullanabilirsiniz. 

İlk olarak, seçin veya yedekleme dosyaları için bir depolama hesabı oluşturun. Aşağıdaki betik, bir depolama hesabı seçer veya henüz yoksa oluşturur.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Otomatik yedekleme, premium depolama alanında depolama yedeklemeleri desteklemez, ancak, Premium depolama kullanan sanal makine diskleri yedeklerden alabilir.

Ardından **yeni AzVMSqlServerAutoBackupConfig** etkinleştirmek ve Azure depolama hesabında yedeklemeleri depolamak için otomatik yedekleme v2 ayarlarını yapılandırmak için komutu. Bu örnekte, 10 gün boyunca bekletilecek yedeklemeleri ayarlanır. Sistem Veritabanı Yedeklemeleri etkinleştirilir. Tam yedeklemeler için bir haftalık, iki saat 20:00 başlayan bir zaman penceresi ile zamanlanır. Günlük yedeklemeler için 30 dakikada zamanlanır. İkinci komut, **kümesi AzVMSqlServerExtension**, bu ayarlarla belirtilen Azure sanal Makineyi güncelleştirir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Bu, yüklemek ve SQL Server Iaas Aracısı'nı yapılandırmak için birkaç dakika sürebilir. 

Şifrelemeyi etkinleştirmek için önceki kodun geçirilecek Değiştir **EnableEncryption** parametresi için bir parola (güvenli dize) yanı sıra **CertificatePassword** parametresi. Aşağıdaki betik, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Ayarlarınızı uygulandığını doğrulamak için [otomatik yedekleme yapılandırmasını doğrulama](#verifysettings).

### <a name="disable-automated-backup"></a>Otomatik yedeklemeyi devre dışı bırak
Otomatik yedekleme devre dışı bırakmak için aynı betiği olmadan çalıştırın **-etkinleştirme** parametresi **yeni AzVMSqlServerAutoBackupConfig** komutu. Olmaması **-etkinleştirme** parametresi sinyalleri özelliğini devre dışı bırakma komutu. Yükleme gibi ile otomatik yedekleme devre dışı bırakmak için birkaç dakika sürebilir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Örnek betik
Aşağıdaki betiği etkinleştirmek ve sanal Makineniz için otomatik yedekleme yapılandırmak için özelleştirebileceğiniz değişkenleri sunmaktadır. Sizin durumunuzda, gereksinimlerinize göre komut dosyasını özelleştirmeniz gerekebilir. Örneğin, sistem veritabanlarının yedeklenmesini devre dışı bırakın veya şifrelemeyi etkinleştirmek istiyorsanız, değişiklik yapmak gerekir.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10
$backupscheduletype = "Manual"
$fullbackupfrequency = "Weekly"
$fullbackupstarthour = "20"
$fullbackupwindow = "2"
$logbackupfrequency = "30"

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension 

Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>İzleme

Otomatik yedekleme üzerinde SQL Server 2016/2017 izlemek için iki ana seçeneğiniz vardır. Otomatik yedekleme, SQL Server yönetilen yedekleme özelliğini kullandığından, aynı izleme teknikleri her ikisi için de geçerlidir.

İlk olarak, çağırarak durumunu yoklamak [msdb.managed_backup.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Ya da sorgu [msdb.managed_backup.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tablo değerli işlev.

Bildirimleri için yerleşik veritabanı posta özelliğin avantajlarından yararlanmak başka bir seçenektir.

1. Çağrı [msdb.managed_backup.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) saklı yordamı için bir e-posta adresi atamak için **SSMBackup2WANotificationEmailIds** parametresi. 
1. Etkinleştirme [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) Azure VM'den e-postalar gönderilecek.
1. Veritabanı posta yapılandırmak için SMTP sunucusu ve kullanıcı adını kullanın. SQL Server Management Studio veya Transact-SQL komutlarını ile Database Mail yapılandırabilirsiniz. Daha fazla bilgi için [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Veritabanı posta kullanmak için SQL Server Agent'ı yapılandırma](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. SMTP bağlantı noktası hem yerel VM Güvenlik Duvarı ve ağ güvenlik grubu aracılığıyla sanal makine için izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
Otomatik yedekleme v2, Azure Vm'leri üzerinde yönetilen yedekleme yapılandırır. Bu nedenle için önemlidir [yönetilen yedekleme için belgeleri gözden](https://msdn.microsoft.com/library/dn449496.aspx) davranışını ve etkilerini anlamak için.

Ek bir yedek bulmak ve Azure vm'lerde SQL Server için bir kılavuz yer alan aşağıdaki makalede geri yükleyebilirsiniz: [Yedekleme ve Azure sanal makineler'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md).

Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).

