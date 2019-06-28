---
title: Bir Azure SQL veritabanı yönetilen örnek veritabanında çoğaltma yapılandırma | Microsoft Docs
description: Bir Azure SQL veritabanı yönetilen örnek veritabanında işlemsel çoğaltma yapılandırma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: e4d056aacf8f3969b645747e2303574f3fea3bda
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357115"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Bir Azure SQL veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma

İşlem çoğaltma SQL Server veritabanı veya başka bir örneği veritabanından bir Azure SQL veritabanı yönetilen örnek veritabanına veri çoğaltmanıza olanak sağlar. 

İşlem çoğaltma için Azure SQL veritabanı yönetilen örneğinde bir örneği veritabanına yapılan değişiklikleri göndermek için de kullanabilirsiniz:

- Bir SQL Server veritabanı.
- Azure SQL veritabanı'nda tek bir veritabanı.
- Azure SQL veritabanı elastik havuz havuza alınmış bir veritabanı.
 
İşlem çoğaltma üzerinde genel önizlemede olan [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). Yönetilen örnek, yayımcı ve dağıtıcı abone veritabanlarını barındırabilir. Bkz: [işlem çoğaltması yapılandırmaları](sql-database-managed-instance-transactional-replication.md#common-configurations) kullanılabilir yapılandırmaları için.

  > [!NOTE]
  > Bu makalede bir Azure veritabanı ile çoğaltma yapılandırması bir kullanıcı kılavuzu yönelik kaynak grubu oluşturma ile başlatılıyor yönetilen uçtan uca örneği. Dağıtılan örneğe zaten varsa atlayın [4. adım](#4---create-a-publisher-database) yayımcı veritabanınızı oluşturmak için veya [adım 6](#6---configure-distribution) zaten bir yayımcı ile abone veritabanı varsa ve başlamaya hazır, Çoğaltma yapılandırılıyor.  

## <a name="requirements"></a>Gereksinimler

Bir yayımcı ve/veya bir dağıtıcı olarak çalışması için bir yönetilen örnek yapılandırma gerektirir:

- Yönetilen örnek şu anda bir coğrafi çoğaltma ilişkisine katılmıyor olduğunu.
- Yönetilen örnek yayımcı dağıtımcı ve abone, aynı sanal ağda açıktır. veya [vNet eşlemesi](../virtual-network/tutorial-connect-virtual-networks-powershell.md) üç tüm varlıkların sanal ağlar arasında kuruldu. 
- Bağlantı, çoğaltma katılımcıları arasında SQL Kimlik Doğrulaması kullanır.
- Çoğaltma çalışma dizini için bir Azure depolama hesabı paylaşımı.
- Bağlantı noktası 445 (TCP Giden) Azure dosya paylaşımına erişmek için yönetilen örnekleri için NSG güvenlik kurallarında açık durumdadır. 


 > [!NOTE]
 > Yalnızca tek veritabanlarının ve havuza alınmış veritabanlarını Azure SQL veritabanı'nda abone olabilir. 


## <a name="features"></a>Özellikler

Destekler:

- Şirket içi SQL Server ve Azure SQL veritabanı yönetilen örnekleri karışımını işlem ve anlık görüntü çoğaltma.
- Aboneler, şirket içi SQL Server veritabanları, Azure SQL veritabanı veya Azure SQL veritabanı elastik havuzları havuza alınmış veritabanlarını tek veritabanı/yönetilen örnekleri olabilir.
- Tek yönlü veya çift yönlü çoğaltma.

Aşağıdaki özellikler bir Azure SQL veritabanı yönetilen örneğinde desteklenmiyor:

- [Güncelleştirilebilir abonelikler](/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication).
- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md) işlemsel çoğaltma yapılandırılmışsa kullanılmamalıdır.
 
## <a name="1---create-a-resource-group"></a>1 - bir kaynak grubu oluşturma

Kullanım [Azure portalında](https://portal.azure.com) ada sahip bir kaynak grubu oluşturmak için `SQLMI-Repl`.  

## <a name="2---create-managed-instances"></a>2 - yönetilen örnek oluşturma

Kullanım [Azure portalında](https://portal.azure.com) iki oluşturmak için [yönetilen örnekleri](sql-database-managed-instance-create-tutorial-portal.md) aynı sanal ağ ve alt ağ. İki yönetilen örnekleri yeniden adlandırılması:

- `sql-mi-pub`
- `sql-mi-sub`

Ayrıca gerekecektir [bağlanmak için bir Azure VM yapılandırma](sql-database-managed-instance-configure-vm.md) Azure SQL veritabanınıza yönetilen örnekler. 

## <a name="3---create-azure-storage-account"></a>3 - Azure depolama hesabı oluşturma

[Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) çalışma dizini için ve oluşturup bir [dosya paylaşımı](../storage/files/storage-how-to-create-file-share.md) depolama hesabında. 

Dosya paylaşım yolu biçiminde kopyalayın: `\\storage-account-name.file.core.windows.net\file-share-name`

Depolama erişim anahtarlarını biçiminde kopyalayın: `DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`

 Daha fazla bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme ve kopyalama](../storage/common/storage-account-manage.md#access-keys). 

## <a name="4---create-a-publisher-database"></a>4 - yayımcı veritabanı oluşturma

Bağlanma, `sql-mi-pub` yönetilen örneği SQL Server Management Studio kullanarak ve yayımcı veritabanınızı oluşturmak için aşağıdaki Transact-SQL (T-SQL) kodu çalıştırın:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_PUB]
GO

USE [ReplTran_PUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [ReplTran_PUB]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="5---create-a-subscriber-database"></a>5 - bir abone veritabanı oluşturma

Bağlanma, `sql-mi-sub` yönetilen SQL Server Management Studio kullanarak örnek ve boş abone veritabanınızı oluşturmak için aşağıdaki T-SQL kodu çalıştırın:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_SUB]
GO

USE [ReplTran_SUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO
```

## <a name="6---configure-distribution"></a>6 - dağıtımı yapılandırma

Bağlanma, `sql-mi-pub` yönetilen örneği SQL Server Management Studio kullanarak ve dağıtım veritabanınız yapılandırmak için aşağıdaki T-SQL kodu çalıştırın. 

```sql
USE [master]
GO

EXEC sp_adddistributor @distributor = @@ServerName;
EXEC sp_adddistributiondb @database = N'distribution';
GO
```

## <a name="7---configure-publisher-to-use-distributor"></a>7 - dağıtıcı kullanacak şekilde Oracle yayımcısı'ı yapılandırma 

Yönetilen örnek, yayımcı üzerinde `sql-mi-pub`, sorgu yürütme için değiştirme [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modu ve yeni dağıtıcı yayımcınız ile kaydetmek için aşağıdaki kodu çalıştırın. 

```sql
:setvar username loginUsedToAccessSourceManagedInstance
:setvar password passwordUsedToAccessSourceManagedInstance
:setvar file_storage "\\storage-account-name.file.core.windows.net\file-share-name"
:setvar file_storage_key "DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net"


USE [master]
EXEC sp_adddistpublisher
  @publisher = @@ServerName,
  @distribution_db = N'distribution',
  @security_mode = 0,
  @login = N'$(username)',
  @password = N'$(password)',
  @working_directory = N'$(file_storage)',
  @storage_connection_string = N'$(file_storage_key)'; -- Remove this parameter for on-premises publishers
```

Bu betik, yönetilen örneğinde yerel bir yayımcı yapılandırır, bağlantılı bir sunucu ekler ve SQL Server Aracısı işlerini kümesi oluşturur. 

## <a name="8---create-publication-and-subscriber"></a>8 - yayımlama ve abone oluşturma

Kullanarak [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modu, veritabanınıza yönelik çoğaltmayı etkinleştirmek ve çoğaltma arasında yayımcı, dağıtımcı ve abone yapılandırmak için aşağıdaki T-SQL betiğini çalıştırın. 

```sql
-- Set variables
:setvar username sourceLogin
:setvar password sourcePassword
:setvar source_db ReplTran_PUB
:setvar publication_name PublishData
:setvar object ReplTest
:setvar schema dbo
:setvar target_server "sql-mi-sub.wdec33262scj9dr27.database.windows.net"
:setvar target_username targetLogin
:setvar target_password targetPassword
:setvar target_db ReplTran_SUB

-- Enable replication for your source database
USE [$(source_db)]
EXEC sp_replicationdboption
  @dbname = N'$(source_db)',
  @optname = N'publish',
  @value = N'true';

-- Create your publication
EXEC sp_addpublication
  @publication = N'$(publication_name)',
  @status = N'active';


-- Configure your log reaer agent
EXEC sp_changelogreader_agent
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the publication snapshot
EXEC sp_addpublication_snapshot
  @publication = N'$(publication_name)',
  @frequency_type = 1,
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the ReplTest table to the publication
EXEC sp_addarticle 
  @publication = N'$(publication_name)',
  @type = N'logbased',
  @article = N'$(object)',
  @source_object = N'$(object)',
  @source_owner = N'$(schema)';

-- Add the subscriber
EXEC sp_addsubscription
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @destination_db = N'$(target_db)',
  @subscription_type = N'Push';

-- Create the push subscription agent
EXEC sp_addpushsubscription_agent
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @subscriber_db = N'$(target_db)',
  @subscriber_security_mode = 0,
  @subscriber_login = N'$(target_username)',
  @subscriber_password = N'$(target_password)',
  @job_login = N'$(target_username)',
  @job_password = N'$(target_password)';

-- Initialize the snapshot
EXEC sp_startpublication_snapshot
  @publication = N'$(publication_name)';
```

## <a name="9---modify-agent-parameters"></a>9 - Aracısı parametreleri değiştirin

Azure SQL veritabanı yönetilen örneği şu anda çoğaltma aracıları ile'bağlantısı ile arka uç ilgili bazı sorunlar yaşıyor. Bu sorunu olsa da ele alıyor ele, çoğaltma aracıları için oturum açma zaman aşımı değerini artırmak için geçici çözüm. 

Aşağıdaki T-SQL komutu, oturum açma zaman aşımı süresini artırın yayımcı üzerinde çalıştırın: 

```sql
-- Increase login timeout to 150s
update msdb..sysjobsteps set command = command + N' -LoginTimeout 150' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Yeniden oturum açma zaman aşımı varsayılan değere ayarlamak için aşağıdaki T-SQL komutunu çalıştırarak, bunu yapmanız:

```sql
-- Increase login timeout to 30
update msdb..sysjobsteps set command = command + N' -LoginTimeout 30' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Bu değişiklikleri uygulamak için üç tüm aracılarını yeniden başlatın. 

## <a name="10---test-replication"></a>10 - test yineleme

Çoğaltma yapılandırıldıktan sonra yayımcı yeni öğeler ekleme ve aboneye yaymak değişiklikleri izleme tarafından test edebilirsiniz. 

Abonede satırları görüntülemek için aşağıdaki T-SQL kod parçacığını çalıştırın:

```sql
select * from dbo.ReplTest
```

Yayımcı ek satır eklemek için aşağıdaki T-SQL kod parçacığını çalıştırın ve sonra satırlara yeniden abone olup olmadığını denetleyin. 

```sql
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yayını silmek için aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Drops the publication
USE [ReplTran_PUB]
EXEC sp_droppublication @publication = N'PublishData'
GO
```

Çoğaltma seçeneği veritabanından kaldırmak için aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Disables publishing of the database
USE [ReplTran_PUB]
EXEC sp_removedbreplication
GO
```

Yayınlama ve dağıtımı devre dışı bırakmak için aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Drops the distributor
USE [master]
EXEC sp_dropdistributor @no_checks = 1
GO
```

Azure kaynaklarınızı temizleyebilirsiniz [yönetilen örnek kaynaklar kaynak grubundan silme](../azure-resource-manager/manage-resources-portal.md#delete-resources) ve sonra kaynak grubunu silerek `SQLMI-Repl`. 

   
## <a name="see-also"></a>Ayrıca Bkz.

- [İşlem çoğaltması](sql-database-managed-instance-transactional-replication.md)
- [Yönetilen örnek nedir?](sql-database-managed-instance.md)
