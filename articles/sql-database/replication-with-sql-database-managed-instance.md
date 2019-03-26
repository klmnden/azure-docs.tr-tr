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
ms.openlocfilehash: b20a119a69ac796bc9ea85083d335f0a7d2fdf2d
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417964"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Bir Azure SQL veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma

İşlem çoğaltma SQL Server veritabanı veya başka bir örneği veritabanından bir Azure SQL veritabanı yönetilen örnek veritabanına veri çoğaltmanıza olanak sağlar. İşlem çoğaltma, bir örneği veritabanında bir SQL Server veritabanını Azure SQL veritabanı yönetilen örneği'nde tek bir Azure SQL veritabanı, veritabanı bir havuza alınmış Azure SQL veritabanı elastik havuz veritabanına yapılan değişiklikleri göndermek için de kullanabilirsiniz. İşlem çoğaltma üzerinde genel önizlemede olan [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). Yönetilen örnek, yayımcı ve dağıtıcı abone veritabanlarını barındırabilir. Bkz: [işlem çoğaltması yapılandırmaları](sql-database-managed-instance-transactional-replication.md#common-configurations) kullanılabilir yapılandırmaları için.

## <a name="requirements"></a>Gereksinimler

Bir yayımcı ya da bir dağıtıcı olarak çalışması için bir yönetilen örnek yapılandırma gerektirir:

- Yönetilen örnek şu anda bir coğrafi çoğaltma ilişkisine katılmıyor olduğunu.

   >[!NOTE]
   >Yalnızca tek veritabanlarının ve havuza alınmış veritabanlarını Azure SQL veritabanı'nda abone olabilir.

- Tüm yönetilen örnekleri aynı vNet üzerinde olmalıdır.

- Bağlantı çoğaltma katılımcılar SQL kimlik doğrulaması kullanır.

- Çoğaltma çalışma dizini için bir Azure depolama hesabı paylaşımı.

- Bağlantı noktası 445 (TCP Giden) Azure dosya paylaşımına erişmek için yönetilen örnek alt ağ güvenliği kurallarının açılması gerekiyor

## <a name="features"></a>Özellikler

Desteklediği Özel Uygulamalar:

- Şirket içi SQL Server ve Azure SQL veritabanı yönetilen örnekleri karışımını işlem ve anlık görüntü çoğaltma.
- Aboneler, şirket içi SQL Server veritabanları, Azure SQL veritabanı veya Azure SQL veritabanı elastik havuzları havuza alınmış veritabanlarını tek veritabanı/yönetilen örnekleri olabilir.
- Tek yönlü veya çift yönlü çoğaltma.

Aşağıdaki özellikler bir Azure SQL veritabanı yönetilen örneğinde desteklenmiyor:

- Güncelleştirilebilir abonelikler.
- [Etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) ve [otomatik yük devretme grupları](sql-database-auto-failover-group.md) işlemsel çoğaltma yapılandırılmışsa kullanılmamalıdır.

## <a name="configure-publishing-and-distribution-example"></a>Yayımlama ve dağıtım örneği yapılandırma

1. [Bir Azure SQL veritabanı yönetilen örnek oluşturma](sql-database-managed-instance-create-tutorial-portal.md) portalında.
2. [Bir Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) için çalışma dizini.

   Depolama anahtarlarını kopyaladığınızdan emin olun. Bkz: [depolama erişim anahtarlarını görüntüleme ve kopyalama](../storage/common/storage-account-manage.md#access-keys
).
3. Yayımcı için bir örnek veritabanı oluşturun.

   Aşağıdaki örnek betiklerde değiştirin `<Publishing_DB>` ile örnek veritabanının adı.

4. Bir veritabanı kullanıcısı için dağıtıcı SQL kimlik doğrulaması ile oluşturun. Güvenli bir parola kullanın.

   Aşağıdaki örnek komut dosyalarında `<SQL_USER>` ve `<PASSWORD>` bu SQL Server hesabı ile veritabanı kullanıcı adı ve parola.

5. [SQL veritabanı yönetilen örneğine bağlanın](sql-database-connect-query-ssms.md).

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
   
## <a name="see-also"></a>Ayrıca Bkz.

- [İşlem çoğaltması](sql-database-managed-instance-transactional-replication.md)
- [Yönetilen örnek nedir?](sql-database-managed-instance.md)
