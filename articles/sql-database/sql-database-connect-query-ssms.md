---
title: "SSMS: Azure SQL Veritabanında verileri bağlama ve sorgulama | Microsoft Docs"
description: "SQL Server Management Studio (SSMS) kullanarak Azure&quot;da SQL Database&quot;e nasıl bağlanılacağını öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın."
metacanonical: 
keywords: "sql veritabanına bağlanma,sql server management studio"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: 7ae47bcce700336206d532b414b7d0eea41d87c5
ms.lasthandoff: 04/04/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Veritabanı: SQL Server Management Studio kullanarak verileri bağlama ve sorgulama

Kullanıcı arabiriminde veya betiklerde bulunan SQL Server kaynaklarını oluşturmak ve yönetmek için [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) kullanın. Bu hızlı başlangıçta bir Azure SQL veritabanına bağlanmak ve ardından query, insert, update ve delete deyimlerini yürütmek üzere SSMS kullanmayla ilgili ayrıntılar verilmektedir.

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

Başlamadan önce, en yeni [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) sürümünü yüklediğinizden emin olun. 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. SQL Server Management Studio kullanarak sunucunuza bağlanmak için tam sunucu adını kullanırsınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, **Sunucu adını** bulup kopyalayın.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="connect-to-the-server-and-your-new-database"></a>Sunucuya ve yeni veritabanınıza bağlanma

SQL Server Management Studio’yu kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:
   - **Sunucu türü**: Veritabanı altyapısını belirtin
   - **Sunucu adı**: **mynewserver20170313.database.windows.net** gibi bir tam sunucu adı girin
   - **Kimlik doğrulama**: SQL Server Kimlik Doğrulaması belirtin
   - **Kullanıcı adı**: Sunucu yöneticisi hesabınızı girin
   - **Parola**: Sunucu yöneticisi hesabınızın parolasını girin
 
    <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

    <img src="./media/sql-database-connect-query-ssms/connected.png" alt="connected to server" style="width: 780px;" />

4. Nesne Gezgini’nde **Veritabanları**’nı ve ardından **mySampleDatabase** öğesini genişleterek nesneleri örnek veritabanında görüntüleyin.

## <a name="query-data"></a>Verileri sorgulama

Azure SQL veritabanınızdaki verileri sorgulamak için [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimini kullanın.

1. Nesne Gezgini’nde **mySampleDatabase** öğesine sağ tıklayıp **Yeni Sorgu**’ya tıklayın. Veritabanınıza bağlı boş bir sorgu penceresi açılır.
2. Sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda **Yürüt**’e tıklayarak Product ve ProductCategory tablolarından verileri alın.

    <img src="./media/sql-database-connect-query-ssms/query.png" alt="query" style="width: 780px;" />

## <a name="insert-data"></a>Veri ekleme

Verileri Azure SQL veritabanınıza eklemek için [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimini kullanın.

1. Araç çubuğunda **Yeni Sorgu**'ya tıklayın. Veritabanınıza bağlı bir boş sorgu penceresi açılır.
2. Sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

3. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosuna yeni bir satır ekleyin.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Verileri güncelleştirme

Azure SQL veritabanınızdaki verileri güncelleştirmek için [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini kullanın.

1. Araç çubuğunda **Yeni Sorgu**'ya tıklayın. Veritabanınıza bağlı bir boş sorgu penceresi açılır.
2. Sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosunda belirtilen satırı güncelleştirin.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Verileri silme

Azure SQL veritabanınızdaki verileri silmek için [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimini kullanın.

1. Araç çubuğunda **Yeni Sorgu**'ya tıklayın. Veritabanınıza bağlı bir boş sorgu penceresi açılır.
2. Sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosunda belirtilen satırı silin.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Sonraki adımlar

- SSMS hakkında bilgi için bkz. [SQL Server Management Studio'yu Kullanma](https://msdn.microsoft.com/library/ms174173.aspx).
- Visual Studio Code ile veri sorgulama ve düzenleme hakkında bilgi için bkz. [Visual Studio Code](https://code.visualstudio.com/docs)

