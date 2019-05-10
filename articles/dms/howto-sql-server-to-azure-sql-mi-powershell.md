---
title: Veritabanı geçiş hizmeti ve PowerShell ile Azure SQL veritabanı yönetilen örneği SQL Server'ı geçirme | Microsoft Docs
description: Azure PowerShell kullanarak şirket içi SQL Server'dan Azure SQL DB yönetilen örneği için geçirmeyi öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/29/2019
ms.openlocfilehash: d83410efd26f8c2078d3abdb01d061db0b83d33d
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233720"
---
# <a name="migrate-sql-server-on-premises-to-an-azure-sql-database-managed-instance-using-azure-powershell"></a>Şirket içi SQL Server, Azure PowerShell kullanarak Azure SQL veritabanı yönetilen örneğine geçirme
Bu makalede, geçiş **Adventureworks2016** yukarıda bir Azure SQL veritabanı'na Microsoft Azure PowerShell kullanarak yönetilen örnek veya bir şirket içi SQL Server 2005 örneğine geri veritabanı. Kullanarak bir Azure SQL veritabanı yönetilen örneği için bir şirket içi SQL Server örneğinden veritabanları geçirebilirsiniz `Az.DataMigration` Microsoft Azure PowerShell modülü.

Bu makalede şunları öğreneceksiniz:
> [!div class="checklist"]
>
> * Bir kaynak grubu oluşturun.
> * Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Bir geçiş projesi, Azure veritabanı geçiş Hizmeti'nin bir örneğini oluşturun.
> * Geçişi çalıştırma.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Bu makale, hem çevrimiçi hem de çevrimdışı geçiş gerçekleştirmek için daha fazla ayrıntı içerir.

## <a name="prerequisites"></a>Önkoşullar

Bu adımları tamamlamak için ihtiyacınız vardır:

* [SQL Server 2016 veya üzeri](https://www.microsoft.com/sql-server/sql-server-downloads) (herhangi bir sürümü).
* Yerel bir kopyasını **AdventureWorks2016** indirme için kullanılabilir olan veritabanı [burada](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017).
* SQL Server Express yüklemesi ile varsayılan olarak devre dışıdır TCP/IP Protokolü etkinleştirmek için. Makaleyi izleyerek TCP/IP protokolünü etkinleştirin [etkinleştirmek veya devre dışı sunucu ağ protokolünü](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* Yapılandırmak için [veritabanı altyapısı erişimi için Windows Güvenlik Duvarı](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
* Bir Azure SQL veritabanı yönetilen örneği. Bir Azure SQL veritabanı yönetilen örneğine makalesinde ayrıntılı olarak izleyerek oluşturabilirsiniz [bir Azure SQL veritabanı yönetilen örnek oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* İndirmek ve yüklemek için [Data Migration Yardımcısı](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 veya üzeri.
* Bir Azure sanal ya da kullanarak Azure veritabanı geçiş hizmeti ile şirket içi kaynak sunucularınıza siteden siteye bağlantı sağlar Azure Resource Manager dağıtım modeli kullanılarak oluşturulan ağ (VNet) [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Data Migration Yardımcısı makalesinde açıklanan şekilde kullanarak şirket içi veritabanı ve şema geçiş tamamlanmış bir değerlendirme [bir SQL Server Geçiş değerlendirmesi gerçekleştirmek](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem).
* İndirmek ve yüklemek için `Az.DataMigration` Modülü (0.7.2 sürümü veya üzeri) kullanarak PowerShell Galerisi'ndeki [Install-Module PowerShell cmdlet'i](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1).
* Kaynak SQL Server örneğine bağlanmak için kullanılan kimlik bilgilerini sağlamak için [denetimi sunucusu](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) izni.
* Hedef Azure SQL veritabanına bağlanmak için kullanılan kimlik bilgilerini sağlamak için hedef Azure SQL veritabanı yönetilen örnek veritabanları üzerinde yönetilen örnek CONTROL DATABASE iznine sahip.

    > [!IMPORTANT]
    > Çevrimiçi geçişler için zaten gerekir, Azure Active Directory kimlik bilgilerini ayarladınız. Daha fazla bilgi için bkz [Azure AD'yi kaynaklara erişebilen uygulaması ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Microsoft Azure aboneliğiniz için oturum açın

PowerShell kullanarak Azure aboneliğinizde oturum açın. Daha fazla bilgi için bkz [oturum Azure PowerShell ile oturum açma](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Bir Azure kaynak grubu, Azure kaynaklarını dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır.

Kullanarak bir kaynak grubu oluşturma [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) komutu.

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myResourceGroup* içinde *Doğu ABD* bölge.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneği oluşturma

Yeni Azure veritabanı geçiş hizmeti örneğini kullanarak oluşturabileceğiniz `New-AzDataMigrationService` cmdlet'i.
Bu cmdlet, aşağıdaki gerekli parametreler bekliyor:

* *Azure kaynak grubu adı*. Kullanabileceğiniz [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) daha önce gösterildiği gibi bir Azure kaynak grubu oluşturun ve adını parametre olarak sağlamak için komutu.
* *Hizmet adı*. İçin Azure veritabanı geçiş hizmeti istenen benzersiz bir hizmet adına karşılık gelen dize.
* *Konum*. Hizmet konumunu belirtir. Batı ABD veya Güneydoğu Asya gibi bir Azure veri merkezi konumu belirtin.
* *SKU*. Bu parametre, DMS Sku adına karşılık gelir. Şu anda desteklenen Sku adları *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
* *Sanal alt ağ tanımlayıcısı*. Cmdlet'i kullanabilirsiniz [ `New-AzVirtualNetworkSubnetConfig` ](https://docs.microsoft.com//powershell/module/az.network/new-azvirtualnetworksubnetconfig) bir alt ağ oluşturmak için.

Aşağıdaki örnekte adlı bir hizmet oluşturur *MyDMS* kaynak grubundaki *MyDMSResourceGroup* bulunan *Doğu ABD* bölge adlıbirsanalağkullanma *MyVNET* ve adlı bir alt ağ *MySubnet*.

> [!IMPORTANT]
> Aşağıdaki kod parçacığında, bir Premium SKU üzerinde temel Azure veritabanı geçiş hizmeti örneği gerektirmeyen bir çevrimdışı geçiş içindir. Çevrimiçi bir geçiş için bir Premium SKU - Sku parametresi değerini içermelidir.

```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Geçiş projesi oluşturma

Azure veritabanı geçiş hizmeti örneği oluşturduktan sonra bir geçiş projesi oluşturun. Bir Azure veritabanı geçiş hizmeti projesi, hem kaynak ve hedef örneklerin yanı projenin bir parçası geçirmek istediğiniz veritabanlarının bir listesi için bağlantı bilgilerini gerektirir.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Kaynak ve hedef bağlantıları için bir veritabanı bağlantı bilgisi nesnesi oluşturun

Bir veritabanı bağlantı bilgisi nesnesi kullanarak oluşturabileceğiniz `New-AzDmsConnInfo` cmdlet'ini aşağıdaki parametre bekliyor:

* *ServerType*. Örneğin, SQL, Oracle veya MySQL istendi, veritabanı bağlantısı türü. SQL için SQL Server ve Azure SQL kullanın.
* *Veri kaynağı*. Adı veya IP bir SQL Server örneğine veya Azure SQL veritabanı örneği.
* *AuthType*. SqlAuthentication ya da ServiceCredentials bağlantı için kimlik doğrulaması türü.
* *TrustServerCertificate*. Bu parametre, kanal güven doğrulamak için sertifika zinciri walking atlayarak sırasında şifrelenmiş olup olmadığını gösteren bir değer ayarlar. Değer olabilir `$true` veya `$false`.

Aşağıdaki örnek, SQL Server adlı bir kaynak için bir bağlantı bilgisi nesnesi oluşturur. *MySourceSQLServer* sql kimlik doğrulaması kullanarak:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Sonraki örnek, 'sql kimlik doğrulaması kullanarak targetmanagedinstance.database.windows.net' adlı bir Azure SQL veritabanı yönetilen örnek sunucusu için bağlantı bilgileri oluşturulmasını gösterir:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Veritabanları için geçiş projesi belirtin

Bir liste oluşturur `AzDataMigrationDatabaseInfo` projeyi oluşturmak için parametre sağlanabilir Azure veritabanı geçiş hizmeti projenin bir parçası olarak veritabanları belirten nesneleri. Cmdlet'i kullanabilirsiniz `New-AzDataMigrationDatabaseInfo` oluşturmak için `AzDataMigrationDatabaseInfo`.

Aşağıdaki örnek, oluşturur `AzDataMigrationDatabaseInfo` için proje **AdventureWorks2016** veritabanı ve proje oluşturma için parametre olarak sağlanacak listesine ekler.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Bir proje oluşturur

Son olarak, adlı bir Azure veritabanı geçiş hizmeti projesi oluşturabilirsiniz *MyDMSProject* bulunan *Doğu ABD* kullanarak `New-AzDataMigrationProject` ve önceden oluşturduğunuz kaynak ve hedef bağlantı ekleme ve Geçirilecek veritabanları listesi.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Bir geçiş görevi oluşturup başlatmak

Ardından, bir Azure veritabanı geçiş hizmeti görevi oluşturup başlatmak. Bu görev, hem kaynak ve hedef, hem de veritabanı tablolarına geçirilmesi için bağlantı kimlik bilgilerini ve bir önkoşul olarak oluşturulan proje zaten sağlanan bilgileri gerektirir.

### <a name="create-credential-parameters-for-source-and-target"></a>Kaynak ve hedef için kimlik bilgisi parametreleri oluşturma

Güvenlik kimlik bilgileri olarak mumbai'ye bir [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) nesne.

Aşağıdaki örnek oluşturulmasını gösterir *PSCredential* parola dizesi değişkenleri olarak sağlayarak, hem kaynak hem de hedef bağlantıları için nesneleri *$sourcePassword* ve *$ HedefParolası*.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-backup-fileshare-object"></a>Bir yedek dosya paylaşımını nesnesi oluşturun

Şimdi yerel SMB ağ paylaşımı için Azure veritabanı geçiş hizmeti gerçekleştirebileceğiniz kaynak kullanarak veritabanı yedeklemeleri temsil eden bir dosya paylaşımını nesnesi oluşturma `New-AzDmsFileShare` cmdlet'i.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Seçilen veritabanı nesnesi oluşturma

Sonraki adım kaynak ve hedef veritabanlarını kullanarak seçmektir `New-AzDmsSelectedDB` cmdlet'i.

Aşağıdaki örnek, tek bir veritabanının SQL Server'dan Azure SQL veritabanı yönetilen örneğine geçirme için verilmiştir:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

Bir SQL Server örneğinin gerekiyorsa bir lift-and-shift ile taşıma bir Azure SQL veritabanı yönetilen örneği, ardından tüm veritabanlarını kaynaktan almak için bir döngü aşağıda verilmiştir. Aşağıdaki örnekte, $Server, $SourceUserName ve $SourcePassword, kaynak SQL Server ayrıntılarını sağlayın.

```powershell
$Query = "(select name as Database_Name from master.sys.databases where Database_id>4)";
$Databases= (Invoke-Sqlcmd -ServerInstance "$Server" -Username $SourceUserName
-Password $SourcePassword -database master -Query $Query)
$selectedDbs=@()
foreach($DataBase in $Databases.Database_Name)
    {
      $SourceDB=$DataBase
      $TargetDB=$DataBase
      
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
                                              -Name $SourceDB `
                                              -TargetDatabaseName $TargetDB `
                                              -BackupFileShare $backupFileShare
      }
```

### <a name="sas-uri-for-azure-storage-container"></a>Azure depolama kapsayıcısının SAS URI'si

Azure veritabanı geçiş hizmeti hizmet yedekleme dosyaları karşıya yükler depolama hesabının kapsayıcıya erişimi veren SAS URI'sini içeren bir değişken oluşturun.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="additional-configuration-requirements"></a>Ek yapılandırma gereksinimleri

İlgilenmeniz gereken bazı ek gereksinimleri vardır, ancak çevrimiçi veya çevrimdışı bir geçiş mi gerçekleştiriyorsunuz bağlı olarak farklılık gösterir.

#### <a name="offline-migrations"></a>Çevrimdışı geçiş

Yalnızca çevrimdışı geçiş işleminde, aşağıdaki ek yapılandırma görevleri gerçekleştirin.

* **Oturum açmaları seçin**. Aşağıdaki örnekte gösterildiği gibi geçirilmesi için oturumların listesini oluşturun:

    ```powershell
    $selectedLogins = @(“user1”, “user2”)
    ```

    > [!IMPORTANT]
    > Şu anda Azure veritabanı geçiş hizmeti, yalnızca geçirme SQL oturum açma bilgileri destekler.

* **Aracı işleri seçin**. Aracı bir listesini aşağıdaki örnekte gösterildiği gibi geçirilecek işler oluşturun:

    ```powershell
    $selectedAgentJobs = @("agentJob1", "agentJob2")
    ```

    > [!IMPORTANT]
    > Şu anda Azure veritabanı geçiş hizmeti, yalnızca T-SQL alt sistemi iş adımları işlerle destekler.

#### <a name="online-migrations"></a>Online geçişleri

Yalnızca çevrimiçi geçişler için aşağıdaki ek yapılandırma görevleri gerçekleştirin.

* **Azure Active Directory uygulamasını yapılandırma**

    ```powershell
    $pwd = "Azure App Key"
    $appId = "Azure App Id"
    $AppPasswd = ConvertTo-SecureString $pwd -AsPlainText -Force
    $app = New-AzDmsAdApp -ApplicationId $appId -AppKey $AppPasswd
    ```

* **Depolama kaynağı yapılandırın**

    ```powershell
    $storageResourceId = "Storage Resource Id"
    ```

### <a name="create-and-start-the-migration-task"></a>Geçiş görevi oluşturup başlatmak

Kullanım `New-AzDataMigrationTask` geçiş görevi oluşturup başlatmak için cmdlet.

#### <a name="specify-parameters"></a>Parametreleri belirtin

##### <a name="common-parameters"></a>Ortak parametreleri

Çevrimiçi veya çevrimdışı bir geçiş gerçekleştirme bağımsız olarak `New-AzDataMigrationTask` cmdlet şu parametreleri bekliyor:

* *TaskType*. SQL Server için Azure SQL veritabanı yönetilen örneğine geçiş türüne oluşturmak için geçiş görevinin türü *MigrateSqlServerSqlDbMi* beklenir. 
* *Kaynak grubu adı*. Görev oluşturulacağı Azure kaynak grubunun adı.
* *ServiceName*. Hangi görev oluşturmak Azure veritabanı geçiş hizmeti örneği.
* *ProjectName*. Görev oluşturmak Azure veritabanı geçiş hizmeti projesi adı. 
* *TaskName*. Oluşturulacak görev adı. 
* *KaynakBağlantı*. Kaynak SQL Server bağlantısını temsil eden AzDmsConnInfo nesnesi.
* *TargetConnection*. Hedef Azure SQL veritabanı yönetilen örneği bağlantıyı temsil eden AzDmsConnInfo nesnesi.
* *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) kaynak sunucuya bağlanmak için nesne.
* *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) hedef sunucuya bağlanmak için nesne.
* *SelectedDatabase*. Kaynak ve hedef veritabanı eşleme temsil eden AzDataMigrationSelectedDB nesnesi.
* *BackupFileShare*. Azure veritabanı geçiş Hizmeti'nin veritabanı yedeklemeleri oluşturmak için kaynak sürebilir yerel ağ paylaşımı temsil eden dosya paylaşımı nesnesi.
* *BackupBlobSasUri*. Azure veritabanı geçiş Hizmeti'nin hizmeti yedekleme dosyaları karşıya yükler depolama hesabının kapsayıcıya erişimi sağlayan SAS URI'si. Blob kapsayıcısı için SAS URI'sini almayı öğrenin.
* *SelectedLogins*. Geçiş için seçilen oturumları listesi.
* *SelectedAgentJobs*. Geçiş için seçilen Aracısı işleri listesi.

##### <a name="additional-parameters"></a>Ek parametreler

`New-AzDataMigrationTask` Cmdlet ayrıca gerçekleştirdiğiniz çevrimiçi veya çevrimdışı geçiş türüne özgü parametreler bekliyor.

* **Çevrimdışı geçiş**. Çevrimdışı geçiş `New-AzDataMigrationTask` cmdlet ayrıca şu parametreleri bekliyor:

  * *SelectedLogins*. Geçiş için seçilen oturumları listesi.
  * *SelectedAgentJobs*. Geçiş için seçilen Aracısı işleri listesi.

* **Çevrimiçi geçişi**. Çevrimiçi geçiş `New-AzDataMigrationTask` cmdlet ayrıca şu parametreleri bekliyor.

* *AzureActiveDirectoryApp*. Active Directory uygulaması.
* *StorageResourceID*. Depolama hesabı kaynak kimliği.

#### <a name="create-and-start-an-offline-migration-task"></a>Çevrimdışı geçiş görevi oluşturup başlatmak

Aşağıdaki örnek, oluşturur ve adlı bir çevrimdışı geçiş görevini başlatır **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

#### <a name="create-and-start-an-online-migration-task"></a>Çevrimiçi geçiş görevi oluşturup başlatmak

Aşağıdaki örnek, oluşturur ve adlı bir çevrimiçi geçiş görevini başlatır **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMiSync `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -SelectedDatabase $selectedDbs `
  -AzureActiveDirectoryApp $app `
  -StorageResourceId $storageResourceId
```

## <a name="monitor-the-migration"></a>Geçişi izleme

Geçiş izlemek için aşağıdaki görevleri gerçekleştirin.

1. Geçiş ayrıntıları $CheckTask adında bir değişkene birleştirin.

    Geçiş ayrıntıları gibi özelliklerini, durumunu ve geçiş ile ilişkili veritabanı bilgileri birleştirmek için aşağıdaki kod parçacığını kullanın:

    ```powershell
    $CheckTask= Get-AzDataMigrationTask     -ResourceGroupName myResourceGroup `
                                            -ServiceName $service.Name `
                                        -ProjectName $project.Name `
                                            -Name myDMSTask `
                                            -ResultType DatabaseLevelOutput `
                        -Expand 
    Write-Host ‘$CheckTask.ProjectTask.Properties.Output’
    ```

2. Kullanım `$CheckTask` geçiş görevi geçerli durumunu almak için değişkeni.

    Kullanılacak `$CheckTask` değişken geçiş görevi geçerli durumunu almak için aşağıdaki örnekte gösterildiği gibi görevin durumu özelliği sorgulanarak çalıştıran geçiş görevi izleyebilmeniz için:

    ```powershell
    if (($CheckTask.ProjectTask.Properties.State -eq "Running") -or ($CheckTask.ProjectTask.Properties.State -eq "Queued"))
    {
      write-host "migration task running"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Succeeded")
    { 
      write-host "Migration task is completed Successfully"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Failed" -or $CheckTask.ProjectTask.Properties.State -eq "FailedInputValidation"  -or $CheckTask.ProjectTask.Properties.State -eq "Faulted")
    { 
      write-host “Migration Task Failed”
    }
    ```

## <a name="performing-the-cutover-online-migrations-only"></a>Tam geçişi (çevrimiçi geçiş yalnızca) gerçekleştirme

Bir çevrimiçi geçiş ile bir tam yedekleme ve geri yükleme veritabanı gerçekleştirilir ve ardından iş, BackupFileShare içinde depolanan işlem günlüklerini geri yüklemeye devam eder.

Bir Azure SQL veritabanı yönetilen örneği veritabanında en son veriler ile güncelleştirilir ve kaynak veritabanıyla eşit olduğunda, sistem geçişi gerçekleştirebilirsiniz.

Aşağıdaki örnek, cutover\migration tamamlanır. Kullanıcıların takdirine bağlı olarak, bu komutu çağırır.

```powershell
$command = Invoke-AzDmsCommand -CommandType CompleteSqlMiSync `
                               -ResourceGroupName myResourceGroup `
                               -ServiceName $service.Name `
                               -ProjectName $project.Name `
                               -TaskName myDMSTask `
                               -DatabaseName "Source DB Name"
```

## <a name="deleting-the-instance-of-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti örneğini silme

Geçiş tamamlandıktan sonra Azure veritabanı geçiş hizmeti örneği silebilirsiniz:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="additional-resources"></a>Ek kaynaklar

Ek geçirme senaryoları (kaynak/hedef çiftleri) hakkında daha fazla bilgi için bkz: Microsoft [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).

## <a name="next-steps"></a>Sonraki adımlar

Azure veritabanı geçiş hizmeti hakkında daha fazla makaleyi bulun [Azure veritabanı geçiş hizmeti nedir?](https://docs.microsoft.com/azure/dms/dms-overview).
