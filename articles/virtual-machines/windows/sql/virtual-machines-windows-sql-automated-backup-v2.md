---
title: "SQL Server 2016 Azure sanal makineler için yedekleme v2 otomatik | Microsoft Docs"
description: "SQL Server 2016 Azure'da çalışan sanal makineler için otomatik yedekleme özelliğini açıklar. Bu makalede, Resource Manager kullanarak VM'ler için özeldir."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: ebd23868-821c-475b-b867-06d4a2e310c7
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/05/2017
ms.author: jroth
ms.openlocfilehash: e7e14b0243f82c672392d5ab4bb6aca01156465b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="automated-backup-v2-for-sql-server-2016-azure-virtual-machines-resource-manager"></a>SQL Server 2016 Azure sanal makineleri için (Resource Manager) yedekleme v2 otomatik

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik yedekleme v2 otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) Azure VM'de SQL Server 2016 Standard, Enterprise veya Geliştirici sürümlerini çalıştıran tüm mevcut ve yeni veritabanları için. Bu, dayanıklı Azure blob depolama alanını normal veritabanı yedeklemeleri yapılandırmanıza olanak sağlar. Otomatik yedekleme v2 bağımlı [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Ön koşullar
Otomatik yedekleme v2 kullanmak için aşağıdaki önkoşulları gözden geçirin:

**İşletim sistemi**:

- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü/yayını**:

- SQL Server 2016 Standard
- SQL Server 2016 Enterprise
- SQL Server 2016 Developer

> [!IMPORTANT]
> Otomatik yedekleme v2 SQL Server 2016 ile çalışır. SQL Server 2014 kullanıyorsanız, veritabanlarını yedeklemek için otomatik yedekleme v1 kullanabilirsiniz. Daha fazla bilgi için bkz: [otomatik yedekleme SQL Server 2014 Azure Virtual Machines için](virtual-machines-windows-sql-automated-backup.md).

**Veritabanı yapılandırması**:

- Hedef veritabanlarına tam kurtarma modeli kullanmanız gerekir. Daha fazla tam kurtarma modeli etkisi hakkında yedekleme hakkında bilgi için [yedekleme altında tam kurtarma modeli](https://technet.microsoft.com/library/ms190217.aspx).
- Sistem veritabanlarının tam kurtarma modelini kullanmak zorunda değil. Ancak, Model veya MSDB için alınacak günlük yedekleri gerektiriyorsa, tam kurtarma modeli kullanmanız gerekir.
- Hedef veritabanlarına varsayılan SQL Server örneğinde olması gerekir. SQL Server Iaas uzantısı adlandırılmış örnekleri desteklemez.

**Azure dağıtım modeli**:

- Resource Manager

> [!NOTE]
> Otomatik yedekleme kullanır **SQL Server Iaas Aracısı uzantısı**. Geçerli SQL sanal makineye Galerisi görüntülerini varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar
Aşağıdaki tabloda otomatik yedekleme v2 için yapılandırılabilir seçenekler açıklanmaktadır. Gerçek yapılandırma adımları Azure portalında veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak farklılık gösterir.

### <a name="basic-settings"></a>Temel ayarlar

| Ayar | Aralık (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya SQL Server 2016 Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama süresi** | 1-30 gün (30 gün) | Yedeklemeleri tutulacağı gün sayısı. |
| **Depolama hesabı** | Azure depolama hesabı | Blob depolama alanına otomatik yedekleme dosyalarını depolamak için kullanılacak bir Azure depolama hesabı. Bir kapsayıcı tüm yedekleme dosyalarını depolamak için bu konumda oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve veritabanı GUID'si içerir. |
| **Şifreleme** |Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkinleştirildiğinde, yedeklemeyi geri yüklemek için kullanılan sertifikaları aynı belirtilen depolama hesabında bulunan **automaticbackup** aynı adlandırma kuralını kullanarak kapsayıcı. Parola değişirse, bu parola ile yeni bir sertifika oluşturulur, ancak önceki yedekleri geri yüklemek için eski sertifika kalır. |
| **Parola** |Parola metin | Şifreleme anahtarları için bir parola. Yalnızca budur şifreleme etkin olup olmadığını gerekli. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanılan ilgili sertifika olması gerekir. |

### <a name="advanced-settings"></a>Gelişmiş Ayarlar

| Ayar | Aralık (varsayılan) | Açıklama |
| --- | --- | --- |
| **Sistem Veritabanı Yedeklemeleri** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirildiğinde, bu özellik ayrıca sistem veritabanlarını yedekleyin: Master, MSDB ve Model. MSDB ve modeli veritabanları için alınacak günlük yedekleri istiyorsanız, bunlar tam kurtarma modunda olduğundan emin olun. Günlük yedeklemeler için ana hiçbir zaman alınır. Ve yedekleme TempDB için alınır. |
| **Yedekleme zamanlaması** | El ile otomatik / (otomatik) | Varsayılan olarak, günlük büyüme tabanlı Yedekleme zamanlaması otomatik olarak belirlenir. El ile yedekleme zamanlaması yedeklemeler için zaman penceresini belirtmesini sağlar. Bu durumda, yedeklemeler her zaman sadece belirtilen sıklığı ve belirli bir günde, belirtilen zaman penceresi sırasında gerçekleşir. |
| **Tam yedekleme sıklığı** | Günlük/Haftalık | Tam yedeklemelerinin sıklığını. Her iki durumda da, sonraki zamanlanmış zaman penceresi sırasında tam yedeklemeler başlar. Haftalık seçili olduğunda, yedeklemelerin tüm veritabanları başarıyla yedeklediyseniz kadar birden fazla gün kapsayabilir. |
| **Tam yedekleme başlangıç saati** | 00:00 – 23:00 (01:00) | Başlangıç sırasında tam yedeklemeler gerçekleştirilebildiği belirli bir gün zamanı. |
| **Tam yedekleme zaman penceresi** | 1 – 23 saat (1 saat) | Belirli bir günde sırasında tam yedeklemeler gerçekleştirilebildiği zaman penceresinin süresi. |
| **Günlük yedekleme sıklığı** | 5 – 60 dakika (60 dakika) | Günlük yedekleme sıklığı. |

## <a name="understanding-full-backup-frequency"></a>Tam yedekleme sıklığını anlama
Günlük ve haftalık tam yedeklemeler arasındaki farkı anlamak önemlidir. Bu çaba içinde iki örnek senaryolar üzerinden size yol gösterir.

### <a name="scenario-1-weekly-backups"></a>Senaryo 1: Haftalık yedekleri
Çok büyük veritabanları sayısını içeren bir SQL Server VM var.

Pazartesi günü, aşağıdaki ayarlarla otomatik yedekleme v2 etkinleştirin:

- Yedekleme zamanlaması: **el ile**
- Tam yedekleme sıklığını: **haftalık**
- Tam yedekleme başlangıç saati: **01:00**
- Tam yedekleme zaman penceresi: **1 saat**

Bu, sonraki kullanılabilir Yedekleme penceresi 1 saat 1 sabah Salı olduğu anlamına gelir. O anda aynı anda veritabanlarınızı bir yedekleme otomatik yedekleme başlar. Bu senaryoda, veritabanlarınızı tam yedeklemeler için ilk birkaç veritabanlarını tamamlayacak yeterli büyüklükte. Ancak, bir saat sonra tüm veritabanları yedeklendi.

Bu durumda, sonraki gün, 1 saat 1 sabah Çarşamba kalan veritabanlarını yedekleme otomatik yedekleme başlar. Bu süre içinde tüm veritabanları yedeklenmiş, sonraki gün aynı zamanda yeniden deneyecek. Tüm veritabanları başarıyla yedeklendi kadar devam eder.

Salı yeniden ulaştıktan sonra bir kez daha tüm veritabanlarını yedekleme otomatik yedekleme başlar.

Bu senaryo, otomatik yedekleme yalnızca belirtilen zaman penceresi içinde çalışır ve her veritabanı haftada bir kez yukarı yedeklenen gösterir. Bu, aynı zamanda birden fazla gün burada tüm yedeklemeler tek bir gün içinde tamamlanmasını olanaklı değil durumda kapsanacak yedeklemeler için olası olduğunu gösterir.

### <a name="scenario-2-daily-backups"></a>Senaryo 2: Günlük yedeklemeler
Çok büyük veritabanları sayısını içeren bir SQL Server VM var.

Pazartesi günü, aşağıdaki ayarlarla otomatik yedekleme v2 etkinleştirin:

- Yedekleme zamanlaması: el ile
- Tam yedekleme sıklığını: günlük
- Tam yedekleme başlangıç saati: 22:00
- Tam yedekleme zaman penceresi: 6 saat

Başka bir deyişle, sonraki kullanılabilir Yedekleme penceresi 10 PM 6 saat boyunca Pazartesi altındadır. O anda aynı anda veritabanlarınızı bir yedekleme otomatik yedekleme başlar.

Ardından, 6 saat boyunca 10 Salı günü, tüm veritabanlarının tam yedeklemeler yeniden başlatılacak.

> [!IMPORTANT]
> Günlük yedeklemeler zamanlarken, bu süre içinde tüm veritabanları yedeklenebilir emin olmak için geniş zaman penceresi zamanlama önerilir. Büyük miktarda veri yedeklemek için sahip olduğu durumda bu durum özellikle önemlidir.

## <a name="configuration-in-the-portal"></a>Portal Yapılandırması

Otomatik yedekleme v2 sağlama sırasında veya var olan SQL Server 2016 VM'ler için yapılandırmak için Azure portalını kullanabilirsiniz.

### <a name="new-vms"></a>Yeni sanal makineleri

Resource Manager dağıtım modelinde yeni bir SQL Server 2016 sanal makine oluşturduğunuzda, otomatik yedekleme v2 yapılandırmak için Azure Portalı'nı kullanın. 

İçinde **SQL Server ayarları** dikey penceresinde, select **otomatik yedekleme**. Aşağıdaki Azure portal ekran görüntüsü gösterildiği **SQL otomatik yedekleme** dikey.

![Azure portalında SQL otomatik yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup-v2/automated-backup-blade.png)

> [!NOTE]
> Otomatik yedekleme v2 varsayılan olarak devre dışıdır.

Bağlam için tam üzerinde konusuna [azure'da bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Var olan sanal makineleri

Var olan SQL Server sanal makineler için SQL Server sanal makine seçin. Ardından **SQL Server yapılandırma** bölümünü **ayarları** dikey.

![Var olan VM'ler için SQL otomatik yedekleme](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration.png)

İçinde **SQL Server yapılandırma** dikey penceresinde tıklatın **Düzenle** otomatik yedekleme bölümdeki düğmesi.

![SQL otomatik yedekleme var olan VM'ler için yapılandırma](./media/virtual-machines-windows-sql-automated-backup-v2/sql-server-configuration-edit.png)

Tamamlandığında, tıklatın **Tamam** alt düğmesinde **SQL Server yapılandırma** yaptığınız değişiklikleri kaydetmek için dikey.

Otomatik yedekleme ilk kez etkinleştirirseniz, Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında otomatik yedekleme yapılandırıldığını gösterilmeyebilir. Aracının yüklü, yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portal'ı yeni ayarları yansıtır.

## <a name="configuration-with-powershell"></a>PowerShell ile yapılandırma

Otomatik yedekleme v2 yapılandırmak için PowerShell kullanın. Başlamadan önce şunları yapmalısınız:

- [En son Azure PowerShell'i yükleyip](http://aka.ms/webpi-azps).
- Windows PowerShell'i açın ve hesabınızla ilişkilendirebilirsiniz. Bu adımları izleyerek yapabilirsiniz [aboneliğinizi yapılandırma](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) bölümüne sağlama.

### <a name="install-the-sql-iaas-extension"></a>SQL Iaas uzantısını yükleyin
Bir SQL Server sanal makine Azure portalından sağlanan, SQL Server Iaas uzantısı zaten yüklü olmalıdır. Bu VM için çağırarak yüklü olup olmadığını belirlemek **Get-AzureRmVM** komutu ve inceleyerek **uzantıları** özelliği.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions 
```

SQL Server Iaas Aracısı uzantısı yüklediyseniz, "Sqlıaasagent" veya "SQLIaaSExtension" olarak listelenen görmeniz gerekir. **ProvisioningState** için uzantı "Başarılı" göstermesi gerekir. 

Bu yüklü değil veya sağlanacak başarısız olursa, aşağıdaki komutla yükleyebilirsiniz. VM adı ve kaynak grubuna ek olarak, ayrıca bölgeyi belirtmeniz gerekir (**$region**), VM bulunur.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region 
```

### <a id="verifysettings"></a>Geçerli ayarlarını doğrulayın
Sağlama işlemi sırasında otomatik yedekleme etkinleştirilirse, geçerli yapılandırmanızı denetlemek için PowerShell'i kullanabilirsiniz. Çalıştırma **Get-AzureRmVMSqlServerExtension** komut ve inceleyin **AutoBackupSettings** özelliği:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Çıktı aşağıdakine benzer almanız gerekir:

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

Çıktı gösteriyorsa **etkinleştirmek** ayarlanır **yanlış**, otomatik yedeklemeyi etkinleştirmek sahip. İyi haber etkinleştirin ve aynı şekilde otomatik yedeklemeyi yapılandırın ' dir. Bu bilgi için sonraki bölüme bakın.

> [!NOTE] 
> Hemen bir değişiklik yaptıktan sonra ayarlarını denetleyin, eski yapılandırma değerlerini geri alırsınız mümkündür. Birkaç dakika bekleyin ve yeniden değişikliklerinizi uygulandığından emin olmak için ayarları kontrol edin.

### <a name="configure-automated-backup-v2"></a>Otomatik yedekleme v2 yapılandırın
Otomatik yedekleme de, yapılandırma ve davranış herhangi bir zamanda değiştirmek sağlamak için PowerShell kullanın. 

İlk olarak, seçin veya yedekleme dosyaları için bir depolama hesabı oluşturun. Aşağıdaki komut dosyasını bir depolama hesabı seçer veya henüz yoksa oluşturur.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region } 
```

> [!NOTE]
> Otomatik yedekleme premium depolama alanına depolanmasını yedeklemeleri desteklemez, ancak Premium depolama kullanan VM diskleri yedeklemelerden alabilir.

Ardından **yeni AzureRmVMSqlServerAutoBackupConfig** Azure depolama hesabında yedeklemelerini depolamak için otomatik yedekleme v2 ayarlarını etkinleştirme ve yapılandırma için komutu. Bu örnekte, yedeklemeler için 10 gün tutacak şekilde ayarlanır. Sistem Veritabanı Yedeklemeleri etkinleştirilir. Tam yedeklemeler için haftalık iki saat 20:00 başlayarak bir zaman penceresi ile zamanlanır. Günlük yedeklemeler için 30 dakikada zamanlanır. İkinci komut **kümesi AzureRmVMSqlServerExtension**, bu ayarlarla belirtilen Azure VM güncelleştirir.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname 
```

Yüklemek ve SQL Server Iaas aracısı yapılandırmak için birkaç dakika sürebilir. 

Şifrelemeyi etkinleştirmek için geçirmek için önceki komut dosyasını değiştirin **EnableEncryption** parametresi için bir parola (güvenli dize) ile birlikte **CertificatePassword** parametresi. Aşağıdaki komut dosyası, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType Manual -FullBackupFrequency Weekly `
    -FullBackupStartHour 20 -FullBackupWindowInHours 2 `
    -LogBackupFrequencyInMinutes 30 

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Ayarlarınızı uygulandığını doğrulamak için [otomatik yedekleme yapılandırması doğrulayın](#verifysettings).

### <a name="disable-automated-backup"></a>Otomatik yedekleme devre dışı bırak
Otomatik yedekleme devre dışı bırakmak için olmadan aynı komut dosyasını Çalıştır **-etkinleştirmek** parametresi **yeni AzureRmVMSqlServerAutoBackupConfig** komutu. Yokluğu **-etkinleştirmek** parametresi sinyalleri özelliğini devre dışı bırakmak için komutu. Yüklemesine gibi otomatik yedekleme devre dışı bırakmak için birkaç dakika sürebilir.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Örnek komut dosyası
Aşağıdaki betik etkinleştirmek ve VM için otomatik yedekleme yapılandırmak için özelleştirebileceğiniz değişkenleri kümesinin sağlar. Sizin durumunuzda, gereksinimlerinize göre betiğini özelleştirme gerekebilir. Örneğin, sistem veritabanlarını yedekleme devre dışı bırakır veya şifrelemeyi etkinleştirmek istiyorsanız değişiklik etmesi gerekir.

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

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname -BackupSystemDbs `
    -BackupScheduleType $backupscheduletype -FullBackupFrequency $fullbackupfrequency `
    -FullBackupStartHour $fullbackupstarthour -FullBackupWindowInHours $fullbackupwindow `
    -LogBackupFrequencyInMinutes $logbackupfrequency

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar
Otomatik yedekleme v2 yönetilen yedekleme Azure Vm'lerinde yapılandırır. İçin önemlidir [yönetilen yedekleme belgelerini gözden](https://msdn.microsoft.com/library/dn449496.aspx) davranışı ve etkilerini anlamak için.

Ek yedekleme bulmak ve Azure vm'lerinde SQL Server Kılavuzu şu konuda geri yükleyebilirsiniz: [yedekleme ve geri yükleme Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-backup-recovery.md).

Diğer kullanılabilir Otomasyon görevler hakkında daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).

