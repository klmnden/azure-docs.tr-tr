---
title: Çoğaltma ile Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği ile SQL Server çoğaltma kullanma hakkında bilgi edinin
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
ms.date: 09/25/2018
ms.openlocfilehash: 25d13ba53eb5a8b411a557b5eaf05d278faa3733
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869321"
---
# <a name="replication-with-sql-database-managed-instance"></a>Çoğaltma ile SQL veritabanı yönetilen örneği

' Te genel önizlemesi için çoğaltma kullanılabilir [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). Bir yönetilen örnek, yayımcı ve dağıtıcı abone veritabanlarını barındırabilir.

## <a name="common-configurations"></a>Ortak yapılandırmaları

Genel olarak, yayımcı ve dağıtıcı hem de Bulut veya şirket içinde olması gerekir. Aşağıdaki yapılandırmalar desteklenir:

- **Yayımcı ile yönetilen örneğinde yerel dağıtıcı**

   ![Replication-With-Azure-SQL-DB-Single-Managed-instance-Publisher-Distributor](./media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

   Yayımcı ve dağıtıcı veritabanlarını tek bir yönetilen örneğinde yapılandırılır.

- **İle yönetilen örneğinde hale getirirken uzak dağıtımcı yayımcı**

   ![Replication-With-Azure-SQL-DB-separate-Managed-instances-Publisher-Distributor](./media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

   Yayımcı ve dağıtıcı iki yönetilen örnekler üzerinde yapılandırılır. Bu yapılandırmada:

  - Aynı sanal ağda hem yönetilen örnekleridir.

  - Aynı konumda hem yönetilen örnekleridir.

- **Yayımcı ve dağıtıcı ile şirket içi abone üzerinde yönetilen örnek**

   ![Replication-from-on-Premises-to-Azure-SQL-DB-Subscriber](./media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)

   Bu yapılandırmada, Azure SQL veritabanı abone durumda. Bu yapılandırma şirket içinden azure'a geçişi destekler. Abone rolünde yönetilen örnek, SQL veritabanı gerektirmez, ancak bir SQL veritabanı yönetilen örneği, bir adımda geçiş şirket içinden azure'a olarak kullanabilir. Azure SQL veritabanı abone hakkında daha fazla bilgi için bkz: [SQL veritabanı için çoğaltma](replication-to-sql-database.md).

## <a name="requirements"></a>Gereksinimler

Yayımcı ve dağıtıcı Azure SQL veritabanı gerektirir:

- Azure SQL veritabanı yönetilen örneği.

   >[!NOTE]
   >Yönetilen örnek sayesinde yapılandırılmamış olan Azure SQL veritabanları, yalnızca abone olabilir.

- SQL Server'ın tüm örnekleri aynı vNet üzerinde olması gerekir.

- Bağlantı çoğaltma katılımcılar SQL kimlik doğrulaması kullanır.

- Çoğaltma çalışma dizini için bir Azure depolama hesabı paylaşımı.

## <a name="features"></a>Özellikler

Desteklediği Özel Uygulamalar:

- Şirket içi ve Azure SQL veritabanı yönetilen örneği örnekleri karışımını işlem ve anlık görüntü çoğaltma.

- Aboneler, şirket içi, Azure SQL veritabanı'nda tek veritabanları veya havuza alınmış veritabanlarını Azure SQL veritabanı elastik havuzları olabilir.

- Tek yönlü veya çift yönlü çoğaltma

## <a name="configure-publishing-and-distribution-example"></a>Yayımlama ve dağıtım örneği yapılandırma

1. [Bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-create-tutorial-portal.md) portalında.
2. [Bir Azure depolama hesabı oluşturma](http://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) için çalışma dizini.

   Depolama anahtarlarını kopyaladığınızdan emin olun. Bkz: [depolama erişim anahtarlarını görüntüleme ve kopyalama](../storage/common/storage-account-manage.md#access-keys
).
3. Yayımcı için bir veritabanı oluşturun.

   Aşağıdaki örnek betiklerde değiştirin `<Publishing_DB>` ile bu veritabanının adı.

4. Bir veritabanı kullanıcısı için dağıtıcı SQL kimlik doğrulaması ile oluşturun. Bkz, [veritabanı kullanıcıları oluşturma](http://docs.microsoft.com/azure/sql-database/sql-database-security-tutorial#creating-database-users). Güvenli bir parola kullanın.

   Aşağıdaki örnek komut dosyalarında `<SQL_USER>` ve `<PASSWORD>` bu SQL Server hesabı ile veritabanı kullanıcı adı ve parola.

5. [SQL veritabanı yönetilen örneğine bağlanın](http://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms).

6. Dağıtıcı ve dağıtım veritabanı eklemek için aşağıdaki sorguyu çalıştırın.

   ```sql
   USE [master]
   GO
   EXEC sp_adddistributor @distributor = @@ServerName;
   EXEC sp_adddistributiondb @database = N'distribution';
   ```

7. Bir yayımcı belirtilen dağıtım veritabanını kullanacak şekilde yapılandırmak için güncelleştirme ve aşağıdaki sorguyu çalıştırın.

   Değiştirin `<SQL_USER>` ve `<PASSWORD>` SQL Server hesabı ve parola.

   Değiştirin `\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>` depolama hesabınızın değerine sahip.  

   Değiştirin `<STORAGE_CONNECTION_STRING>` alınan bağlantı dizesi ile **erişim anahtarları** Microsoft Azure depolama hesabınızın sekmesi.

   Aşağıdaki sorguda güncelleştirdikten sonra çalıştırın.

   ```sql
   USE [master]
   EXEC sp_adddistpublisher @publisher = @@ServerName,
                @distribution_db = N'distribution',
                @security_mode = 0,
                @login = N'<SQL_USER>',
                @password = N'<PASSWORD>',
                @working_directory = N'\\<STORAGE_ACCOUNT>.file.core.windows.net\<SHARE>',
                @storage_connection_string = N'<STORAGE_CONNECTION_STRING>';
   GO
   ```

8. Yayımcı çoğaltma için yapılandırın.

    Aşağıdaki sorguda değiştirin `<Publishing_DB>` , yayımcı veritabanının adı.

    Değiştirin `<Publication_Name>` yayınınızın ada sahip.

    Değiştirin `<SQL_USER>` ve `<PASSWORD>` SQL Server hesabı ve parola.

    Sorgu güncelleştirdikten sonra yayın oluşturmak için çalıştırın.

   ```sql
   USE [<Publishing_DB>]
   EXEC sp_replicationdboption @dbname = N'<Publishing_DB>',
                @optname = N'publish',
                @value = N'true';

   EXEC sp_addpublication @publication = N'<Publication_Name>',
                @status = N'active';

   EXEC sp_changelogreader_agent @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>';

   EXEC sp_addpublication_snapshot @publication = N'<Publication_Name>',
                @frequency_type = 1,
                @publisher_security_mode = 0,
                @publisher_login = N'<SQL_USER>',
                @publisher_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   ```

9. Makale, abonelik ve gönderme temelli bir abonelik ekleyin.

   Bu nesneleri eklemek için aşağıdaki komut dosyasını güncelleştirin.

   - Değiştirin `<Object_Name>` yayın nesne adı.
   - Değiştirin `<Object_Schema>` kaynak şema adı.
   - Açılı ayraçlar içinde diğer parametreleri değiştirmek `<>` önceki komut değerleriyle eşleşecek şekilde.

   ```sql
   EXEC sp_addarticle @publication = N'<Publication_Name>',
                @type = N'logbased',
                @article = N'<Object_Name>',
                @source_object = N'<Object_Name>',
                @source_owner = N'<Object_Schema>'

   EXEC sp_addsubscription @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @destination_db = N'<Subscribing_DB>',
                @subscription_type = N'Push'

   EXEC sp_addpushsubscription_agent @publication = N'<Publication_Name>',
                @subscriber = @@ServerName,
                @subscriber_db = N'<Subscribing_DB>',
                @subscriber_security_mode = 0,
                @subscriber_login = N'<SQL_USER>',
                @subscriber_password = N'<PASSWORD>',
                @job_login = N'<SQL_USER>',
                @job_password = N'<PASSWORD>'
   GO
   ```

## <a name="limitations"></a>Sınırlamalar

Aşağıdaki özellikler desteklenmez:

- Güncelleştirilebilir abonelikler

- Etkin coğrafi çoğaltma

## <a name="see-also"></a>Ayrıca Bkz.

- [Yönetilen örnek nedir?](http://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)
