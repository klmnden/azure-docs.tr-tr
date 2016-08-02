<properties
    pageTitle="C# sorgusu ile SQL Database'e Bağlanma | Microsoft Azure"
    description="Sorgu çalıştırmak ve SQL veritabanına bağlanmak için C# dilinde bir program yazın. IP adresleri, bağlantı dizeleri, güvenli oturum açma ve ücretsiz Visual Studio hakkında bilgiler."
    services="sql-database"
    keywords="c# database query, c# query, connect to database, SQL C#"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="annemill"/>


# Visual Studio ile SQL Database'e bağlanma

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Visual Studio'da Azure SQL Database'e nasıl bağlanılacağını öğrenin. 

## Önkoşullar


Visual Studio'yu kullanarak SQL Database'e bağlanmak için şunlara sahip olmanız gerekir: 


- Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.


- Azure SQL Database hizmetinde bulunan bir **AdventureWorksLT** demo veritabanı.
 - Dakikalar içinde [demo veritabanı oluşturun](sql-database-get-started.md).


- Visual Studio 2013 güncelleştirme 4 (veya sonraki bir sürümü). Microsoft, Visual Studio Community'yi artık *ücretsiz* olarak sunuyor.
 - [Visual Studio Community, indir](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Ücretsiz Visual Studio için daha fazla seçenek](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)
 - Veya bu konu başlığının sonraki bölümlerinde bulunan bir [adımda](#InstallVSForFree) [Azure Portal](https://portal.azure.com/)'ın sizi Visual Studio yüklemesi hakkında nasıl yönlendireceğini öğrenebilirsiniz.


<a name="InstallVSForFree" id="InstallVSForFree"></a>

&nbsp;

## 1. Adım: Visual Studio Community'yi ücretsiz olarak yükleme


Visual Studio'yu yüklemeniz gerekiyorsa şunları yapabilirsiniz:

- Tarayıcınızdan ücretsiz indirme seçenekleri ve başka seçenekler sunan Visual Studio ürün web sayfalarına giderek Visual Studio Community'yi ücretsiz olarak yükleyebilir veya
- sonraki bölümde açıklandığı üzere [Azure Portal](https://portal.azure.com/)'ın sizi indirme web sayfasına yönlendirmesine izin verebilirsiniz.


### Azure Portal aracılığıyla Visual Studio


1. http://portal.azure.com/ adresine giderek [Azure Portal](https://portal.azure.com/) aracılığıyla oturum açın.

2. **TÜMÜNE* GÖZAT** > **SQL veritabanları** düğmesine tıklayın. Veritabanlarını arayabileceğiniz bir dikey pencere açılır.

3. Üst kısmın yanında bulunan filtre metni kutusuna **AdventureWorksLT** veritabanınızın adını yazmaya başlayın.

4. Sunucunuzdaki veritabanınızın bulunduğu satırı gördüğünüzde satıra tıklayın. Veritabanınız için bir dikey pencere açılır.

5. Kolaylık olması açısından, önceki tüm dikey pencereleri simge durumuna küçültün.

6. Veritabanı dikey pencerenizin üst kısmınının yanında bulunan **Visual Studio'da aç** düğmesine tıklayın. Visual Studio için yükleme konumlarına yönelik bağlantıların bulunduğu yeni bir dikey pencere açılır.

    ![Visual Studio'da aç düğmesi][20-OpenInVisualStudioButton]

7. **Community (ücretsiz)** bağlantısına veya benzer bir bağlantıya tıklayın. Yeni bir web sayfası eklenir.

8. Visual Studio'yu yüklemek için yeni web sayfasındaki bağlantıları kullanın.

9. Visual Studio yüklendikten sonra **Visual Studio'da Aç** dikey penceresinde **Visual Studio'da Aç** düğmesine tıklayın. Visual Studio açılır.

10. Visual Studio, **SQL Server Nesne Gezgini** bölmesi için bir iletişim kutusundaki bağlantı dizesi alanlarını doldurmanızı ister.
 - **Windows Kimlik Doğrulaması**'nı değil **SQL Server Kimlik Doğrulaması**'nı seçin.
 - **AdventureWorksLT** veritabanınızı belirtmeyi (iletişim kutusunda bulunan **Seçenekler** > **Bağlantı Özellikleri** aracılığıyla) unutmayın.

11. **SQL Server Nesne Gezgini**'nde, veritabanınız için düğümü genişletin.


## 2. Adım: Örnek sorgu çalıştırma

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

    ![Başarılı. SQL Database sunucusuna bağlanma: SVisual Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Sonraki Adımlar

[.NET (C#) kullanarak SQL Database'e bağlanma](sql-database-develop-dotnet-simple.md) 


<!-- Image references. -->

[20-OpenInVisualStudioButton]: ./media/sql-database-connect-query/connqry-free-vs-e.png




<!---HONumber=Jun16_HO2-->


