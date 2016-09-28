<properties
    pageTitle="C# sorgusu ile SQL Database'e Bağlanma | Microsoft Azure"
    description="Sorgu çalıştırmak ve SQL veritabanına bağlanmak için C# dilinde bir program yazın. IP adresleri, bağlantı dizeleri, güvenli oturum açma ve ücretsiz Visual Studio hakkında bilgiler."
    services="sql-database"
    keywords="c# veritabanı sorgusu, c# sorgusu, veritabanına bağlanma, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>




# Visual Studio ile SQL Database'e bağlanma

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Visual Studio'da bir Azure SQL veritabanına nasıl bağlanacağınızı öğrenin. 

## Ön koşullar


Visual Studio'yu kullanarak bir SQL veritabanına bağlanmak için şunlara sahip olmanız gerekir: 


- Bağlanmak için bir SQL veritabanı. Bu makalede **AdventureWorks** örnek veritabanı kullanılmıştır. AdventureWorks örnek veritabanını almak için bkz. [Demo veritabanı oluşturma](sql-database-get-started.md).


- Visual Studio 2013 güncelleştirme 4 (veya sonraki bir sürümü). Microsoft, Visual Studio Community'yi artık *ücretsiz* olarak sunuyor.
 - [Visual Studio Community, indir](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Ücretsiz Visual Studio için daha fazla seçenek](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## Azure portalından Visual Studio'yu açma


1. [Azure Portal](https://portal.azure.com/)’da oturum açın.

2. **Diğer Hizmetler** > **SQL veritabanları** seçeneğine tıklayın
3. *AdventureWorks* veritabanını bulup tıklayarak **AdventureWorks** veritabanı dikey penceresini açın.

6. Veritabanı dikey penceresinin üst tarafındaki **Araçlar** düğmesine tıklayın:

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. **Visual Studio'da aç** seçeneğine (Visual Studio'ya ihtiyaç duyuyorsanız indirme bağlantısına) tıklayın:

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio, zaten sunucuya ve portalda seçtiğiniz veritabanına bağlanacak şekilde ayarlanmış olan **Sunucuya Bağlan** penceresiyle açılır.  (Doğru veritabanıyla bağlantı kurulduğunu doğrulamak için **Seçenekler**'e tıklayın.) Sunucu yönetici parolanızı yazın ve **Bağlan**'a tıklayın.


    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Bilgisayarınızın IP adresi için ayarlanmış bir güvenlik duvarı kuralınız yoksa bu aşamada *Bağlanılamıyor* iletisi alırsınız. Güvenlik duvarı kuralı oluşturmak için bkz. [Azure SQL Veritabanı sunucusu düzeyinde güvenlik kuralı yapılandırma](sql-database-configure-firewall-settings.md).


9. Bağlantı başarıyla kurulduktan sonra, veritabanınıza yönelik bağlantıyla birlikte **SQL Server Nesne Gezgini** penceresi açılır.

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## Örnek sorgu çalıştırma

Artık veritabanına bağlı olduğumuza göre, basit bir sorgunun nasıl çalıştırılacağına ilişkin aşağıdaki adımlara göz atabiliriz:

2. Veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Aşağıdaki kodu kopyalayıp sorgu penceresine yapıştırın.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Sorguyu çalıştırmak için **Yürüt** düğmesine tıklayın:

    ![Başarılı. SQL Database sunucusuna bağlanma: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## Sonraki adımlar

- Visual Studio'da SQL veritabanlarının açılması için SQL Server Veri Araçları kullanılır. Daha ayrıntılı bilgi için bkz. [SQL Server Veri Araçları](https://msdn.microsoft.com/library/hh272686.aspx).
- Bir SQL veritabanına kod kullanarak bağlanmak için bkz. [.NET (C#) kullanarak SQL Veritabanı'na bağlanma](sql-database-develop-dotnet-simple.md).






<!--HONumber=Sep16_HO3-->


