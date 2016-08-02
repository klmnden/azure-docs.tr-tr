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
   ms.date="06/04/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Transact-SQL (TSQL) kullanarak SQL Data Warehouse oluşturma

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Bu makalede Transact SQL (T-SQL) ile nasıl SQL Data Warehouse veritabanı oluşturacağınız gösterilmiştir.

## Önkoşullar
Başlamadan önce şu önkoşullara sahip olduğunuzdan emin olun:

- **Azure Hesabı**: Hesap oluşturmak için bkz. [Azure Ücretsiz Deneme Sürümü][] veya [MSDN Azure Kredileri][].
- **V12 Azure SQL Server**: Bkz. [Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma][] veya [PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma][].
- **Kaynak grubu adı**: V12 Azure SQL Server'ınızdaki Kaynak Grubunu kullanın veya yeni bir kaynak grubu oluşturmak için [kaynak grupları][] bölümüne göz atın.
- **SQL Server Veri Araçları ile Visual Studio**: Yükleme yönergeleri için bkz. [Visual Studio ve SSDT yükleme][].

> [AZURE.NOTE] Yeni bir SQL Data Warehouse'un oluşturulması ek hizmet ücretlerin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Data Warehouse fiyatlandırması][].

## Visual Studio ile veritabanı oluşturma

Visual Studio'yu ilk kez kullanıyorsanız bkz. [Visual Studio ile SQL Data Warehouse'a bağlanma][].  Başlamak için Visual Studio'da bulunan SQL Server Nesne Gezgini'ni açıp SQL Data Warehouse veritabanınızı barındıracak olan sunucuya bağlanın.  Bağlandıktan sonra **ana** veritabanında aşağıdaki SQL komutunu çalıştırarak bir SQL Data Warehouse oluşturabilirsiniz.  Bu komut, Hizmet Hedefi DW400 olan bir MySqlDwDb veritabanı oluşturur ve veritabanının, maksimum boyutu 10 TB olacak şekilde büyümesine olanak sağlar.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## sqlcmd ile veritabanı oluşturma

Ayrıca komut isteminde aşağıdakini çalıştırarak da sqlcmd üzerinde aynı komutu çalıştırabilirsiniz.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

**MAXSIZE** ve **SERVICE_OBJECTIVE** parametreleri, veritabanının disk üzerinde kullanabileceği maksimum alanı ve Data Warehouse örneğiniz için ayrılan işlem kaynaklarını belirtir.  Hizmet Hedefi, DWU boyutuyla doğrusal olarak ölçeklenen bir CPU ve bellek ayırma işlemidir.  

MAXSIZE 250 GB ile 240 TB arasında olabilir.  Hizmet Hedefi DW100 ve DW2000 arasında olabilir.  MAXSIZE ve SERVICE_OBJECTIVE için geçerli tüm değerlerin tam bir listesi için [CREATE DATABASE][] ile ilgili MSDN belgelerine göz atın.  MAXSIZE ve SERVICE_OBJECTIVE bir [ALTER DATABASE][] T-SQL komutuyla da değiştirilebilir.  Hizmetlerin yeniden başlatılmasına ve bunun sonucunda, gönderilen sorguların tümünün iptal edilmesine neden olacağı için SERVICE_OBJECTIVE değiştirilirken dikkatli olunması gerekir.  MAXSIZE parametresinin değiştirilmesi basit bir meta veri işlemi olduğundan hizmetler yeniden başlatılmaz.

## Sonraki adımlar
SQL Data Warehouse'unuz hazırlandıktan sonra [örnek veri yükleyebilir][] veya [geliştirme][], [yükleme][] veya [geçirme][] işlemlerini nasıl gerçekleştirebileceğinizi inceleyebilirsiniz.

<!--Article references-->
[Azure portalından SQL Data Warehouse oluşturma]: ./sql-data-warehouse-get-started-provision.md
[Visual Studio ile SQL Data Warehouse'a bağlanma]: ./sql-data-warehouse-get-started-connect.md
[geçirme]: ./sql-data-warehouse-overview-migrate.md
[geliştirme]: ./sql-data-warehouse-overview-develop.md
[yükleme]: ./sql-data-warehouse-overview-load.md
[örnek veri yükleyebilir]: ./sql-data-warehouse-get-started-manually-load-samples.md
[Azure Portal ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[PowerShell ile Azure SQL Database mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[kaynak grupları]: ../azure-portal/resource-group-portal.md
[Visual Studio ve SSDT yükleme]: ./sql-data-warehouse-install-visual-studio.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[SQL Data Warehouse fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme Sürümü]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Jun16_HO2-->


