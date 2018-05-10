---
title: SQL Server 2014 Azure sanal makineler için otomatik yedekleme | Microsoft Docs
description: SQL Server 2014 Azure'da çalışan sanal makineler için otomatik yedekleme özelliğini açıklar. Bu makalede, Resource Manager kullanarak VM'ler için özeldir.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/03/2018
ms.author: jroth
ms.openlocfilehash: 43ce94653197933a13830003dd07e5b21be2a585
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>SQL Server 2014 sanal makineler (Resource Manager) için otomatik yedekleme

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016/2017](virtual-machines-windows-sql-automated-backup-v2.md)

Otomatik yedekleme otomatik olarak yapılandırır [yönetilen Microsoft Azure yedekleme](https://msdn.microsoft.com/library/dn449496.aspx) SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM üzerindeki tüm mevcut ve yeni veritabanları için. Bu, dayanıklı Azure blob depolama alanını normal veritabanı yedeklemeleri yapılandırmanıza olanak sağlar. Otomatik yedekleme bağımlı [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar
Otomatik yedekleme kullanmak için aşağıdaki önkoşulları göz önünde bulundurun:

**İşletim sistemi**:

- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

**SQL Server sürümü/yayını**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Otomatik yedekleme SQL Server 2014 ile çalışır. SQL Server 2016/2017 kullanıyorsanız, veritabanlarını yedeklemek için otomatik yedekleme v2 kullanabilirsiniz. Daha fazla bilgi için bkz: [otomatik yedekleme v2 SQL Server 2016 Azure Virtual Machines için](virtual-machines-windows-sql-automated-backup-v2.md).

**Veritabanı yapılandırması**:

- Hedef veritabanlarına tam kurtarma modeli kullanmanız gerekir. Daha fazla tam kurtarma modeli etkisi hakkında yedekleme hakkında bilgi için [yedekleme altında tam kurtarma modeli](https://technet.microsoft.com/library/ms190217.aspx).
- Hedef veritabanlarına varsayılan SQL Server örneğinde olması gerekir. SQL Server Iaas uzantısı adlandırılmış örnekleri desteklemez.

> [!NOTE]
> Otomatik yedekleme SQL Server Iaas Aracısı uzantısını kullanır. Geçerli SQL sanal makineye Galerisi görüntülerini varsayılan olarak bu uzantı ekleyin. Daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Ayarlar

Aşağıdaki tabloda otomatik yedekleme için yapılandırılmış seçenekler açıklanmaktadır. Gerçek yapılandırma adımları Azure portalında veya Azure Windows PowerShell komutlarını kullanmadığınıza bağlı olarak farklılık gösterir.

| Ayar | Aralık (varsayılan) | Açıklama |
| --- | --- | --- |
| **Otomatik Yedekleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya SQL Server 2014 Standard veya Enterprise çalıştıran bir Azure VM için otomatik yedekleme devre dışı bırakır. |
| **Saklama süresi** | 1-30 gün (30 gün) | Bir yedekleme saklanacağı gün sayısı. |
| **Depolama Hesabı** | Azure depolama hesabı | Blob depolama alanına otomatik yedekleme dosyalarını depolamak için kullanılacak bir Azure depolama hesabı. Bir kapsayıcı tüm yedekleme dosyalarını depolamak için bu konumda oluşturulur. Yedekleme dosyası adlandırma kuralı, tarih, saat ve makine adını içerir. |
| **Şifreleme** | Etkinleştir/devre dışı bırak (devre dışı) | Etkinleştirir veya şifreleme devre dışı bırakır. Şifreleme etkinleştirildiğinde, yedeklemeyi geri yüklemek için kullanılan sertifikaları aynı belirtilen depolama hesabında bulunan `automaticbackup` aynı adlandırma kuralını kullanarak kapsayıcı. Parola değişirse, bu parola ile yeni bir sertifika oluşturulur, ancak önceki yedekleri geri yüklemek için eski sertifika kalır. |
| **Parola** | Parola metin | Şifreleme anahtarları için bir parola. Yalnızca budur şifreleme etkin olup olmadığını gerekli. Şifrelenmiş bir yedeklemeyi geri yüklemek için doğru parolayı ve yedeğin alındığı anda kullanılan ilgili sertifika olması gerekir. |

## <a name="configure-in-the-portal"></a>Portalda yapılandırma

Otomatik yedekleme sağlama sırasında veya var olan SQL Server 2014 VM'ler için yapılandırmak için Azure portalını kullanabilirsiniz.

## <a name="configure-new-vms"></a>Yeni sanal makineleri yapılandırma

Resource Manager dağıtım modelinde yeni bir SQL Server 2014 sanal makine oluşturduğunuzda, otomatik yedekleme yapılandırması için Azure portalını kullanın.

İçinde **SQL Server ayarları** bölmesinde, **otomatik yedekleme**. Aşağıdaki Azure portal ekran görüntüsü gösterildiği **SQL otomatik yedekleme** ayarlar.

![Azure portalında SQL otomatik yedekleme yapılandırması](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

## <a name="configure-existing-vms"></a>Var olan sanal makineleri yapılandırma

Var olan SQL Server sanal makineler için SQL Server sanal makine seçin. Ardından **SQL Server yapılandırma** VM bölümünü **ayarları**.

![Var olan VM'ler için SQL otomatik yedekleme](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

İçinde **SQL Server yapılandırma** bölmesinde tıklatın **Düzenle** otomatik yedekleme bölümünde düğmesini.

![SQL otomatik yedekleme var olan VM'ler için yapılandırma](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Tamamlandığında, tıklatın **Tamam** alt düğmesinde **SQL Server yapılandırma** yaptığınız değişiklikleri kaydetmek için ayarları.

Otomatik yedekleme ilk kez etkinleştirirseniz, Azure SQL Server Iaas Aracısı arka planda yapılandırır. Bu süre boyunca, Azure portalında otomatik yedekleme yapılandırıldığını gösterilmeyebilir. Aracının yüklü, yapılandırılmış birkaç dakika bekleyin. Bundan sonra Azure portal'ı yeni ayarları yansıtır.

> [!NOTE]
> Otomatik yedekleme bir şablon kullanarak da yapılandırabilirsiniz. Daha fazla bilgi için bkz: [otomatik yedekleme için Azure Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configure-with-powershell"></a>PowerShell ile yapılandır

Otomatik yedekleme yapılandırması için PowerShell kullanın. Başlamadan önce şunları yapmalısınız:

- [En son Azure PowerShell'i yükleyip](http://aka.ms/webpi-azps).
- Windows PowerShell'i açın ve hesabınızla ilişkilendirmek **Connect-AzureRmAccount** komutu.

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
$region = "EASTUS2"
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

> [!IMPORTANT]
> Uzantısı yükleme uzantısı zaten yüklü değilse, SQL Server hizmetini yeniden başlatır.

### <a id="verifysettings"></a> Geçerli ayarlarını doğrulayın

Sağlama işlemi sırasında otomatik yedekleme etkinleştirilirse, geçerli yapılandırmanızı denetlemek için PowerShell'i kullanabilirsiniz. Çalıştırma **Get-AzureRmVMSqlServerExtension** komut ve inceleyin **AutoBackupSettings** özelliği:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Çıktı aşağıdakine benzer almanız gerekir:

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

Çıktı gösteriyorsa **etkinleştirmek** ayarlanır **yanlış**, otomatik yedeklemeyi etkinleştirmek sahip. İyi haber etkinleştirin ve aynı şekilde otomatik yedeklemeyi yapılandırın ' dir. Bu bilgi için sonraki bölüme bakın.

> [!NOTE] 
> Hemen bir değişiklik yaptıktan sonra ayarlarını denetleyin, eski yapılandırma değerlerini geri alırsınız mümkündür. Birkaç dakika bekleyin ve yeniden değişikliklerinizi uygulandığından emin olmak için ayarları kontrol edin.

### <a name="configure-automated-backup"></a>Otomatik yedeklemeyi yapılandırın
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

Ardından **yeni AzureRmVMSqlServerAutoBackupConfig** Azure depolama hesabında yedeklemelerini depolamak için otomatik yedekleme ayarlarını etkinleştirme ve yapılandırma için komutu. Bu örnekte, yedeklemeleri 10 gün boyunca saklanır. İkinci komut **kümesi AzureRmVMSqlServerExtension**, bu ayarlarla belirtilen Azure VM güncelleştirir.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Yüklemek ve SQL Server Iaas aracısı yapılandırmak için birkaç dakika sürebilir.

> [!NOTE]
> Diğer ayarları için **yeni AzureRmVMSqlServerAutoBackupConfig** yalnızca SQL Server 2016 ve otomatik yedekleme v2 için geçerlidir. SQL Server 2014, aşağıdaki ayarları desteklemiyor: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours**, ve **LogBackupFrequencyInMinutes**. Bir SQL Server 2014 sanal makinede bu ayarları yapılandırmak çalışırsanız, bir hata, ancak ayarları uygulanmamış. Bu ayarları bir SQL Server 2016 sanal makinede kullanmak istiyorsanız, bkz: [otomatik yedekleme v2 SQL Server 2016 Azure Virtual Machines için](virtual-machines-windows-sql-automated-backup-v2.md).

Şifrelemeyi etkinleştirmek için geçirmek için önceki komut dosyasını değiştirin **EnableEncryption** parametresi için bir parola (güvenli dize) ile birlikte **CertificatePassword** parametresi. Aşağıdaki komut dosyası, önceki örnekte otomatik yedekleme ayarlarını etkinleştirir ve şifreleme ekler.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

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

### <a name="example-script"></a>Örnek betik

Aşağıdaki betik etkinleştirmek ve VM için otomatik yedekleme yapılandırmak için özelleştirebileceğiniz değişkenleri kümesinin sağlar. Sizin durumunuzda, gereksinimlerinize göre betiğini özelleştirme gerekebilir. Örneğin, sistem veritabanlarını yedekleme devre dışı bırakır veya şifrelemeyi etkinleştirmek istiyorsanız değişiklik etmesi gerekir.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

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
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="monitoring"></a>İzleme

Otomatik yedekleme SQL Server 2014 üzerinde izlemek için iki ana seçeneğiniz vardır. Otomatik yedekleme SQL Server yönetilen yedekleme özelliğini kullandığından, aynı izleme teknikleri hem de geçerlidir.

Çağırarak durumu ilk olarak, yoklayabilir [msdb.smart_admin.sp_get_backup_diagnostics](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-get-backup-diagnostics-transact-sql). Ya da sorgu [msdb.smart_admin.fn_get_health_status](https://docs.microsoft.com/sql/relational-databases/system-functions/managed-backup-fn-get-health-status-transact-sql) tablo değerli işlevi.

> [!NOTE]
> Yönetilen yedekleme SQL Server 2014 için şema **msdb.smart_admin**. SQL Server 2016'da bu değiştirildi **msdb.managed_backup**, ve bu yeni şema başvuru konuları kullanın. SQL Server 2014 için kullanmaya devam etmeniz gerekir, ancak **smart_admin** tüm yönetilen yedekleme nesneleri için şema.

Bildirimler için yerleşik Database Mail özelliğinin avantajlarından yararlanmak başka bir seçenektir.

1. Çağrı [msdb.smart_admin.sp_set_parameter](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/managed-backup-sp-set-parameter-transact-sql) saklı yordamı bir e-posta adresi atamak için **SSMBackup2WANotificationEmailIds** parametresi. 
1. Etkinleştirme [SendGrid](../../../sendgrid-dotnet-how-to-send-email.md) Azure sanal makineden e-postalar gönderme.
1. Database Mail yapılandırmak için SMTP sunucusu ve kullanıcı adı kullanın. SQL Server Management Studio'da veya Transact-SQL komutlarıyla Database Mail yapılandırabilirsiniz. Daha fazla bilgi için bkz: [Database Mail](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail).
1. [Database Mail kullanmak için SQL Server Agent yapılandırma](https://docs.microsoft.com/sql/relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail).
1. SMTP bağlantı noktası hem yerel VM Güvenlik Duvarı ve ağ güvenlik grubu aracılığıyla VM için izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik yedekleme yönetilen yedekleme Azure Vm'lerinde yapılandırır. İçin önemlidir [yönetilen yedekleme SQL Server 2014 üzerinde belgelerini gözden](https://msdn.microsoft.com/library/dn449497(v=sql.120).aspx).

Ek yedekleme bulmak ve Azure vm'lerinde SQL Server için yönergeler yer alan aşağıdaki makalede geri yükleyebilirsiniz: [yedekleme ve geri yükleme Azure Virtual Machines'de SQL Server için](virtual-machines-windows-sql-backup-recovery.md).

Diğer kullanılabilir Otomasyon görevler hakkında daha fazla bilgi için bkz: [SQL Server Iaas Aracısı uzantısı](virtual-machines-windows-sql-server-agent-extension.md).

Azure Vm'lerinde SQL Server çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md).
