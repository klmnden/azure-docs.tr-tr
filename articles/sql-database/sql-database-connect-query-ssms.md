<properties
    pageTitle="SQL Database'e bağlanma - SQL Server Management Studio | Microsoft Azure"
    description="SQL Server Management Studio (SSMS) kullanarak Azure'da SQL Database'e nasıl bağlanılacağını öğrenin. Ardından, Transact-SQL (T-SQL) kullanarak bir örnek sorgu çalıştırın."
    metaCanonical=""
    keywords="connect to sql database,sql server management studio"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sstein;carlrab" />

# SQL Server Management Studio ile SQL Database'e bağlanma ve örnek T-SQL sorgusu yürütme

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Bu makalede SQL Server Management Studio'nun (SSMS) en güncel sürümünü kullanarak Azure SQL veritabanına nasıl bağlanacağınız ve Transact-SQL (T-SQL) deyimleri ile nasıl basit bir sorgu gerçekleştirebileceğiniz gösterilmiştir.

[AZURE.INCLUDE [Sign in](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

Güvenlik duvarı kuralları hakkında bilgi edinmek için bkz. [Güvenlik Duvarı Ayarlarını Yapılandırma (Azure SQL Database)](sql-database-configure-firewall-settings.md).

## Örnek sorgu çalıştırma

Mantıksal sunucunuza bağlandıktan sonra bir veritabanına bağlanabilir ve bir örnek sorgu çalıştırabilirsiniz. 

1. **Nesne Gezgini**'nde, sunucuda bulunan ve **AdventureWorks** örnek veritabanı gibi erişim izniniz olan bir veritabanına gidin.
2. Veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. Aşağıdaki kodu kopyalayıp sorgu penceresine yapıştırın.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. **Yürüt** düğmesine tıklayın.  Aşağıdaki ekran görüntüsü, sorgunun başarılı olduğunu gösterir.

    ![Başarılı. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Sonraki adımlar

SQL Server'dakine benzer şekilde, T-SQL deyimlerini kullanarak Azure'da veritabanı oluşturup yönetebilirsiniz. Daha önce SQL Server ile T-SQL kullandıysanız farklılıklara ilişkin özet niteliğinde bir açıklama için bkz. [Azure SQL Database Transact-SQL bilgileri](sql-database-transact-sql-information.md).

T-SQL'i ilk kez kullanıyorsanız bkz. [Öğretici: Transact-SQL Deyimi Yazma](https://msdn.microsoft.com/library/ms365303.aspx) ve [Transact-SQL Başvuru (Veritabanı Altyapısı)](https://msdn.microsoft.com/library/bb510741.aspx).

Veritabanı kullanıcıları ve veritabanı kullanıcı yöneticileri oluşturmaya başlamak için bkz. [Azure SQL Database Güvenliğine Başlama](sql-database-get-started-security.md).



<!--HONumber=Jun16_HO2-->


