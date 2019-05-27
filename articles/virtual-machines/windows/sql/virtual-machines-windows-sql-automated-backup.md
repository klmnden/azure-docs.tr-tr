---
title: SQL Server 2014 Azure sanal makineleri için otomatik yedekleme | Microsoft Docs
description: SQL Server 2014 Azure'da çalışan Vm'leri için otomatik yedekleme özelliğini açıklar. Bu makalede, Resource Manager kullanarak Vm'lere özeldir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 99439c2b6bd4fdd271dda7a49850c5b6f44330b3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66165575"
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>SQL Server 2014 Virtual Machines'de (Resource Manager) için otomatik yedekleme

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik yedekleme otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) Azure VM'deki SQL Server 2014 Standard veya Enterprise çalıştıran tüm mevcut ve yeni veritabanları için. Bu, kalıcı Azure blob depolama kullanan normal veritabanı yedeklemelerini yapılandırma sağlar. Otomatik yedekleme bağlıdır [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Otomatik yedekleme kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Otomatik olarak SQL Server 2014 ile yedekleme çalışır. SQL Server 2016/2017 kullanıyorsanız, veritabanlarınızı yedeklemek için otomatik yedekleme v2 kullanabilirsiniz. Daha fazla bilgi için [SQL Server 2016 Azure sanal makineleri için otomatik yedekleme v2](virtual-machines-windows-sql-automated-backup-v2.md).

**Veritabanı yapılandırması**:

- Hedef veritabanlarının tam kurtarma modelini kullanmanız gerekir. Daha fazla tam kurtarma modelinin etkisi hakkında yedeklemeler hakkında bilgi için [yedekleme altında tam kurtarma modeli](https://technet.microsoft.com/library/ms190217.aspx).
- Varsayılan SQL Server örneğinde hedef veritabanlarına olmalıdır. SQL Server Iaas uzantısı adlandırılmış örnekler desteklemez.

> [!NOTE]
> Otomatik yedekleme, SQL Server Iaas Aracısı uzantısını kullanır. Geçerli SQL sanal makine galeri görüntüleri, varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar

Aşağıdaki tabloda, otomatik yedekleme için yapılandırılmış seçenekler açıklanmaktadır. Gerçek yapılandırma adımları, Azure portal veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak değişir.

| Ayar | Aralığı (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama dönemi** | 1-30 gün (30 gün) | Bir yedekleme saklanacağı gün sayısı. |
| **Depolama Hesabı** | Azure depolama hesabı | Otomatik yedekleme dosyalarını blob storage'da depolamak için kullanılacak bir Azure depolama hesabı. Tüm yedekleme dosyalarını depolamak için bu konumda bir kapsayıcı oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve makine adı içerir. |
| **Şifreleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkin olduğunda, yedeklemeyi geri yüklemek için kullanılan sertifikaları aynı belirtilen depolama hesabında bulunan `automaticbackup` adlandırma kuralını kullanarak kapsayıcı. Parola değişirse, bu parolayla yeni bir sertifika oluşturulur, ancak önceki yedeklemeleri geri yüklemek için eski sertifika kalır. |
| **Parola** | Parola metin | Şifreleme anahtarları parolası. Bu sadece, şifreleme etkinse gereken. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanıldı ilgili sertifika olmalıdır. |

## <a name="configure-in-the-portal"></a>Portalda yapılandırma

Sağlama sırasında ya da mevcut SQL Server 2014 sanal makineleri için otomatik yedekleme yapılandırmak için Azure portalını kullanabilirsiniz.

## <a name="configure-new-vms"></a>Yeni Vm'leri yapılandırma

Resource Manager dağıtım modelinde yeni bir SQL Server 2014 sanal makine oluşturduğunuzda, otomatik yedeklemeyi yapılandırmak için Azure portalını kullanın.

İçinde **SQL Server ayarları** bölmesinde **otomatik yedekleme**. Aşağıdaki Azure portalı ekran görüntüsü gösterildiği **SQL otomatik yedekleme** ayarları.

![Azure portalında SQL otomatik yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

## <a name="configure-existing-vms"></a>Varolan Vm'leri yapılandırma

Mevcut SQL Server sanal makineleri için SQL Server sanal makinenizi seçin. Ardından **SQL Server Yapılandırması** VM bölümünü **ayarları**.

![Var olan sanal makineler için SQL otomatik yedekleme](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

İçinde **SQL Server Yapılandırması** bölmesinde tıklayın **Düzenle** otomatik yedekleme bölümünde düğmesini.

![SQL otomatik yedekleme, var olan VM'ler için yapılandırma](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

İşiniz bittiğinde tıklayın **Tamam** altındaki düğmesine **SQL Server Yapılandırması** yaptığınız değişiklikleri kaydetmek için ayarları.

Otomatik yedekleme ilk kez etkinleştirirseniz, Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında, otomatik yedekleme yapılandırıldığını göstermeyebilir. Aracının yüklü için yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portalında yeni ayarları ücreti yansıtılır.

> [!NOTE]
> Otomatik yedekleme şablon kullanarak da yapılandırabilirsiniz. Daha fazla bilgi için [Azure Hızlı Başlangıç şablonu için otomatik yedekleme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configure-with-powershell"></a>PowerShell ile yapılandırma

Otomatik yedeklemeyi yapılandırmak için PowerShell kullanabilirsiniz. Başlamadan önce şunları yapmalısınız:

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

Bunu yüklü değil veya sağlanması başarısız oldu, şu komutla yükleyin. Ek olarak VM adı ve kaynak grubu, ayrıca bölgeyi belirtmeniz gerekir (**$region**), sanal makinenizin bulunur.

```powershell
$region = "EASTUS2"
Set-AzVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

> [!IMPORTANT]
> Uzantı zaten yüklü değilse, uzantının yüklenmesi, SQL Server hizmetini yeniden başlatır.

### <a id="verifysettings"></a> Geçerli ayarları doğrulayın

Otomatik yedekleme sağlama sırasında etkinleştirilirse, geçerli yapılandırmayı denetlemek için PowerShell kullanabilirsiniz. Çalıştırma **Get-AzVMSqlServerExtension** inceleyin ve komutu **AutoBackupSettings** özelliği:

```powershell
(Get-AzVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Aşağıdaki çıktıyı almalısınız:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Çıkış gösteriliyorsa **etkinleştirme** ayarlanır **False**, otomatik yedeklemeyi etkinleştirmek sahip. Güzel bir haberimiz var etkinleştirin ve aynı şekilde otomatik yedeklemeyi yapılandırma ' dir. Bu bilgi için sonraki bölüme bakın.

> [!NOTE] 
> Hemen bir değişiklik yaptıktan sonra ayarlarını kontrol edin, eski yapılandırma değerlerini geri alırsınız mümkündür. Birkaç dakika bekleyin ve yeniden değişikliklerinizi uygulandığından emin olmak için ayarları kontrol edin.

### <a name="configure-automated-backup"></a>Otomatik yedeklemeyi yapılandırma
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

Ardından **yeni AzVMSqlServerAutoBackupConfig** etkinleştirmek ve Azure depolama hesabında yedeklemeleri depolamak için otomatik yedekleme ayarlarını yapılandırmak için komutu. Bu örnekte, yedeklemeleri 10 gün boyunca saklanır. İkinci komut, **kümesi AzVMSqlServerExtension**, bu ayarlarla belirtilen Azure sanal Makineyi güncelleştirir.

```powershell
$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Bu, yüklemek ve SQL Server Iaas Aracısı'nı yapılandırmak için birkaç dakika sürebilir.

> [!NOTE]
> Diğer ayarlar için **yeni AzVMSqlServerAutoBackupConfig** yalnızca SQL Server 2016 ve otomatik yedekleme v2 için geçerlidir. SQL Server 2014, aşağıdaki ayarları desteklemez: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, ve **LogBackupFrequencyInMinutes**. Bir SQL Server 2014 sanal makinede bu ayarları yapılandırmak çalışırsanız, hata yoktur, ancak ayarların değil uygulandığından. Bu ayarları bir SQL Server 2016 sanal makinede kullanmak istiyorsanız, bkz. [SQL Server 2016 Azure sanal makineleri için otomatik yedekleme v2](virtual-machines-windows-sql-automated-backup-v2.md).

Şifrelemeyi etkinleştirmek için önceki kodun geçirilecek Değiştir **EnableEncryption** parametresi için bir parola (güvenli dize) yanı sıra **CertificatePassword** parametresi. Aşağıdaki betik, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

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
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>İzleme

Otomatik yedekleme SQL Server 2014'te izlemek için iki ana seçeneğiniz vardır. Otomatik yedekleme, SQL Server yönetilen yedekleme özelliğini kullandığından, aynı izleme teknikleri her ikisi için de geçerlidir.

İlk olarak, çağırarak durumunu yoklamak [msdb.smart_admin.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Ya da sorgu [msdb.smart_admin.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tablo değerli işlev.

> [!NOTE]
> Yönetilen yedekleme, SQL Server 2014 şeması **msdb.smart_admin**. SQL Server 2016'da bu değiştirilecek **msdb.managed_backup**, ve bu yeni şema başvurusu konuları kullanın. Ancak SQL Server 2014 için kullanmaya devam etmelisiniz **smart_admin** tüm yönetilen yedekleme nesneler için şema.

Bildirimleri için yerleşik veritabanı posta özelliğin avantajlarından yararlanmak başka bir seçenektir.

1. Çağrı [msdb.smart_admin.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) saklı yordamı için bir e-posta adresi atamak için **SSMBackup2WANotificationEmailIds** parametresi. 
1. Etkinleştirme [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) Azure VM'den e-postalar gönderilecek.
1. Veritabanı posta yapılandırmak için SMTP sunucusu ve kullanıcı adını kullanın. SQL Server Management Studio veya Transact-SQL komutlarını ile Database Mail yapılandırabilirsiniz. Daha fazla bilgi için [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Veritabanı posta kullanmak için SQL Server Agent'ı yapılandırma](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. SMTP bağlantı noktası hem yerel VM Güvenlik Duvarı ve ağ güvenlik grubu aracılığıyla sanal makine için izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik yedekleme, yedekleme yönetilen Azure Vm'lerinde yapılandırır. Bu nedenle için önemlidir [SQL Server 2014'te yönetilen yedekleme için belgeleri gözden](https://msdn.microsoft.com/library/dn449497(v=sql.120).aspx).

Ek bir yedek bulmak ve Azure vm'lerde SQL Server için bir kılavuz yer alan aşağıdaki makalede geri yükleyebilirsiniz: [Yedekleme ve Azure sanal makineler'de SQL Server için geri yükleme](virtual-machines-windows-sql-backup-recovery.md).

Diğer kullanılabilir otomasyon görevleri hakkında daha fazla bilgi için bkz. [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz. [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md).
