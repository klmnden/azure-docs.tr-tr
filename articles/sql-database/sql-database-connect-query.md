---
title: "C# sorgusu ile SQL Veritabanı&quot;na Bağlanma | Microsoft Belgeleri"
description: "Sorgu çalıştırmak ve SQL veritabanına bağlanmak için C# dilinde bir program yazın. IP adresleri, bağlantı dizeleri, güvenli oturum açma ve ücretsiz Visual Studio hakkında bilgiler."
services: sql-database
keywords: "c# veritabanı sorgusu, c# sorgusu, veritabanına bağlanma, SQL C#"
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: stevestein
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 333babec567a4700ca0882c883e4460442d844e1


---
# <a name="connect-to-a-sql-database-with-visual-studio"></a>Visual Studio ile SQL Database'e bağlanma
> [!div class="op_single_selector"]
> * [Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

Visual Studio'da bir Azure SQL veritabanına nasıl bağlanacağınızı öğrenin. 

## <a name="prerequisites"></a>Ön koşullar
Visual Studio'yu kullanarak bir SQL veritabanına bağlanmak için şunlara sahip olmanız gerekir: 

* Bağlanmak için bir SQL veritabanı. Bu makalede **AdventureWorks** örnek veritabanı kullanılmıştır. AdventureWorks örnek veritabanını almak için bkz. [Demo veritabanı oluşturma](sql-database-get-started.md).
* Visual Studio 2013 güncelleştirme 4 (veya sonraki bir sürümü). Microsoft, Visual Studio Community'yi artık *ücretsiz* olarak sunuyor.
  
  * [Visual Studio Community, indir](http://www.visualstudio.com/products/visual-studio-community-vs)
  * [Ücretsiz Visual Studio için daha fazla seçenek](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)

## <a name="open-visual-studio-from-the-azure-portal"></a>Azure portalından Visual Studio'yu açma
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. **Diğer Hizmetler** > **SQL veritabanları** seçeneğine tıklayın
3. *AdventureWorks* veritabanını bulup tıklayarak **AdventureWorks** veritabanı dikey penceresini açın.
4. Veritabanı dikey penceresinin üst tarafındaki **Araçlar** düğmesine tıklayın:
   
    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)
5. **Visual Studio'da aç** seçeneğine (Visual Studio'ya ihtiyaç duyuyorsanız indirme bağlantısına) tıklayın:
   
    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)
6. Visual Studio, zaten sunucuya ve portalda seçtiğiniz veritabanına bağlanacak şekilde ayarlanmış olan **Sunucuya Bağlan** penceresiyle açılır.  (Doğru veritabanıyla bağlantı kurulduğunu doğrulamak için **Seçenekler**'e tıklayın.) Sunucu yönetici parolanızı yazın ve **Bağlan**'a tıklayın.

    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


1. Bilgisayarınızın IP adresi için ayarlanmış bir güvenlik duvarı kuralınız yoksa bu aşamada *Bağlanılamıyor* iletisi alırsınız. Güvenlik duvarı kuralı oluşturmak için bkz. [Azure SQL Veritabanı sunucusu düzeyinde güvenlik kuralı yapılandırma](sql-database-configure-firewall-settings.md).
2. Bağlantı başarıyla kurulduktan sonra, veritabanınıza yönelik bağlantıyla birlikte **SQL Server Nesne Gezgini** penceresi açılır.
   
    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)

## <a name="run-a-sample-query"></a>Örnek sorgu çalıştırma
Artık veritabanına bağlı olduğumuza göre, basit bir sorgunun nasıl çalıştırılacağına ilişkin aşağıdaki adımlara göz atabiliriz:

1. Veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.
   
    ![Yeni sorgu. SQL Database sunucusuna bağlanma: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)
2. Aşağıdaki kodu kopyalayıp sorgu penceresine yapıştırın.
   
        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;
3. Sorguyu çalıştırmak için **Yürüt** düğmesine tıklayın:
   
    ![Başarılı. SQL Database sunucusuna bağlanma: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Sonraki adımlar
* Visual Studio'da SQL veritabanlarının açılması için SQL Server Veri Araçları kullanılır. Daha ayrıntılı bilgi için bkz. [SQL Server Veri Araçları](https://msdn.microsoft.com/library/hh272686.aspx).
* Bir SQL veritabanına kod kullanarak bağlanmak için bkz. [.NET (C#) kullanarak SQL Veritabanı'na bağlanma](sql-database-develop-dotnet-simple.md).




<!--HONumber=Nov16_HO2-->


