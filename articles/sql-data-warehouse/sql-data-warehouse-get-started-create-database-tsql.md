---
title: "TSQL ile SQL Veri Ambarı oluşturma | Microsoft Belgeleri"
description: "TSQL ile Azure SQL Data Warehouse oluşturmayı öğrenin"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5a101aa78dbac4f1a0edb7f414b44c14db392652
ms.openlocfilehash: bbfce3f3cf329792f270632e2244e33e3fafb7ef


---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Transact-SQL (TSQL) kullanarak SQL Data Warehouse oluşturma
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Bu makalede T-SQL ile SQL Veri Ambarı oluşturma işlemi gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar
Başlamak için gerekli olanlar:

* **Azure hesabı**: Hesap oluşturmak için [Azure Ücretsiz Deneme][Azure Ücretsiz Deneme] veya [MSDN Azure Kredileri][MSDN Azure Kredileri] sayfasını ziyaret edin.
* **Azure SQL sunucusu**: Daha fazla ayrıntı için bkz. [Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma][Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma] veya [PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma][PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma].
* **Kaynak grubu**: Azure SQL sunucunuz ile aynı kaynak grubunu kullanın veya [nasıl kaynak grubu oluşturulacağına][nasıl kaynak grubu oluşturulacağına] bakın.
* **T-SQL yürütme ortamı**: T-SQL yürütmek için [Visual Studio][Visual Studio ve SSDT’yi yükleme], [sqlcmd][sqlcmd] veya [SSMS][SSMS] işlemlerini kullanabilirsiniz.

> [!NOTE]
> Bir SQL Veri Ambarı'nın oluşturulması ek hizmet ücretlerinin alınmasına neden olabilir.  Fiyatlandırmayla ilgili ayrıntılı bilgi için bkz. [SQL Veri Ambarı fiyatlandırması][SQL Veri Ambarı fiyatlandırması].
>
>

## <a name="create-a-database-with-visual-studio"></a>Visual Studio ile veritabanı oluşturma
Visual Studio'yu kullanmaya yeni başladıysanız [Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama][Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama] başlıklı makaleye göz atın.  Başlamak için Visual Studio'da bulunan SQL Server Nesne Gezgini'ni açıp SQL Data Warehouse veritabanınızı barındıracak olan sunucuya bağlanın.  Bağlandıktan sonra **ana** veritabanında aşağıdaki SQL komutunu çalıştırarak bir SQL Data Warehouse oluşturabilirsiniz.  Bu komut, Hizmet Hedefi DW400 olan bir MySqlDwDb veritabanı oluşturur ve veritabanının, maksimum boyutu 10 TB olacak şekilde büyümesine olanak sağlar.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>sqlcmd ile veritabanı oluşturma
Ayrıca komut isteminde aşağıdakini çalıştırarak da sqlcmd üzerinde aynı komutu çalıştırabilirsiniz.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Belirtilmediğinde varsayılan harmanlama COLLATE SQL_Latin1_General_CP1_CI_AS şeklindedir.  `MAXSIZE` boyutu 250 GB ile 240 TB arasında olabilir.  `SERVICE_OBJECTIVE` değeri DW100 ve DW2000 [DWU][DWU] arasında olabilir.  Geçerli tüm değerlerin listesi için [CREATE DATABASE][CREATE DATABASE] MSDN belgelerine bakın.  Hem MAXSIZE hem de SERVICE_OBJECTIVE öğesi bir [ALTER DATABASE][ALTER DATABASE] T-SQL komutuyla değiştirilebilir.  Bir veritabanının harmanlaması oluşturulduktan sonra değiştirilemez.   DWU’nun değiştirilmesi hizmetlerin yeniden başlatılmasına ve bunun sonucunda gönderilen sorguların tümünün iptal edilmesine neden olacağı için SERVICE_OBJECTIVE değiştirilirken dikkatli olunması gerekir.  MAXSIZE parametresinin değiştirilmesi basit bir meta veri işlemi olduğundan hizmetler yeniden başlatılmaz.

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Ambarınız sağlandıktan sonra [örnek veri yükleyebilir][örnek veri yükleyebilir] ya da [geliştirme][geliştirme], [yükleme][yükleme] veya [geçirme][geçirme] işlemlerini nasıl gerçekleştirebileceğinizi inceleyebilirsiniz.

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Azure portal'dan SQL Veri Ambarı oluşturma]: sql-data-warehouse-get-started-provision.md
[Azure SQL Veri Ambarı’nı (Visual Studio) Sorgulama]: sql-data-warehouse-query-visual-studio.md
[geçirme]: sql-data-warehouse-overview-migrate.md
[geliştirme]: sql-data-warehouse-overview-develop.md
[yükleme]: sql-data-warehouse-overview-load.md
[örnek veri yükleyebilir]: sql-data-warehouse-load-sample-databases.md
[Azure Portal ile Azure SQL Veritabanı mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started.md#create-logical-server-bk
[PowerShell ile Azure SQL Veritabanı mantıksal sunucusu oluşturma]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[nasıl kaynak grubu oluşturulacağına]: ../resource-group-template-deploy-portal.md#create-resource-group
[Visual Studio ve SSDT’yi yükleme]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Veri Ambarı fiyatlandırması]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Ücretsiz Deneme]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Kredileri]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Nov16_HO2-->


