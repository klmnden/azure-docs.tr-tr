<properties
   pageTitle="TSQL ile SQL Data Warehouse oluşturma | Microsoft Azure"
   description="TSQL ile Azure SQL Data Warehouse oluşturmayı öğrenin"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Transact-SQL (TSQL) kullanarak SQL Data Warehouse oluşturma

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Bu makalede T-SQL ile SQL Data Warehouse oluşturma işlemi gösterilmektedir.

## Ön koşullar

Başlamak için şunlar gereklidir: 

- **Azure hesabı**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü][] veya [MSDN Azure Kredileri][] sayfasını ziyaret edin.
- **Azure SQL server**: Daha fazla bilgi için bkz. [Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma][] veya [PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma][].
- **Kaynak grubu**: Azure SQL sunucunuz ile aynı kaynak grubunu kullanın veya [kaynak grubu oluşturma][] işlemine bakın.
- **T-SQL yürütme ortamı**: T-SQL yürütmek için [Visual Studio][Visual Studio ve SSDT Yükleme], [sqlcmd][] veya [SSMS][] işlemlerini kullanabilirsiniz.

> [AZURE.NOTE] Yeni bir SQL Data Warehouse'un oluşturulması ek hizmet ücretlerin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Data Warehouse fiyatlandırması][].

## Visual Studio ile veritabanı oluşturma

Visual Studio'yu kullanmaya yeni başladıysanız [Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama][] başlıklı makaleye göz atın.  Başlamak için Visual Studio'da bulunan SQL Server Nesne Gezgini'ni açıp SQL Data Warehouse veritabanınızı barındıracak olan sunucuya bağlanın.  Bağlandıktan sonra **ana** veritabanında aşağıdaki SQL komutunu çalıştırarak bir SQL Data Warehouse oluşturabilirsiniz.  Bu komut, Hizmet Hedefi DW400 olan bir MySqlDwDb veritabanı oluşturur ve veritabanının, maksimum boyutu 10 TB olacak şekilde büyümesine olanak sağlar.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## sqlcmd ile veritabanı oluşturma

Ayrıca komut isteminde aşağıdakini çalıştırarak da sqlcmd üzerinde aynı komutu çalıştırabilirsiniz.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

`MAXSIZE` boyutu 250 GB ile 240 TB arasında olabilir.  `SERVICE_OBJECTIVE` değeri DW100 ve DW2000 [DWU][] arasında olabilir.  Tüm geçerli değerlerin listesi için [VERİTABANI OLUŞTURMA][] MSDN belgelerine bakın.  MAXSIZE ve SERVICE_OBJECTIVE bir [ALTER DATABASE][] T-SQL komutuyla da değiştirilebilir.  Hizmetlerin yeniden başlatılmasına ve bunun sonucunda, gönderilen sorguların tümünün iptal edilmesine neden olacağı için SERVICE_OBJECTIVE değiştirilirken dikkatli olunması gerekir.  MAXSIZE parametresinin değiştirilmesi basit bir meta veri işlemi olduğundan hizmetler yeniden başlatılmaz.

## Sonraki adımlar

SQL Data Warehouse'unuz hazırlandıktan sonra [örnek veri yükleyebilir][] veya [geliştirme][], [yükleme][] veya [geçirme][] işlemlerini nasıl gerçekleştirebileceğinizi inceleyebilirsiniz.

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Azure portalından SQL Data Warehouse oluşturma]: sql-data-warehouse-get-started-provision.md
[Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama]: sql-data-warehouse-query-visual-studio.md
[geçirme]: sql-data-warehouse-overview-migrate.md
[geliştirme]: sql-data-warehouse-overview-develop.md
[yükleme]: sql-data-warehouse-overview-load.md
[örnek veri yükleyebilir]: sql-data-warehouse-load-sample-databases.md
[Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[kaynak grubu oluşturma]: ../resource-group-template-deploy-portal.md#create-resource-group
[Visual Studio ve SSDT Yükleme]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[VERİTABANI OLUŞTURMA]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme Sürümü]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Aug16_HO4-->


