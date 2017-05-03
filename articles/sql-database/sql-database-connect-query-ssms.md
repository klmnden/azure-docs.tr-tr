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
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 9ffad92e668b76c9a4e2941b20d075bf52132d16
ms.lasthandoff: 04/19/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Veritabanı: SQL Server Management Studio kullanarak verileri bağlama ve sorgulama

[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS), kullanıcı arabiriminden veya betiklerden SQL Server kaynakları oluşturup yönetmek için kullanılan bir yönetim aracıdır. Bu hızlı başlangıçta SSMS kullanarak bir Azure SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır. 

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

Başlamadan önce, en yeni [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) sürümünü yüklediğinizden emin olun. 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz.

   ![bağlantı bilgileri](./media/sql-database-connect-query-ssms/connection-information.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın. 

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>SQL Veritabanı mantıksal sunucusunda veritabanınıza bağlanma

SQL Server Management Studio’yu kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun. 

> [!IMPORTANT]
> Azure SQL Veritabanı mantıksal sunucusu 1433 numaralı bağlantı noktasında dinler. Azure SQL Veritabanı mantıksal sunucusuna bir şirket ağından bağlanmaya çalışıyorsanız, bu bağlantı noktası başarıyla bağlanmanız için şirket güvenlik ağında açık olmalıdır.
>

1. SQL Server Management Studio’yu açın.

2. **Sunucuya Bağlan** iletişim kutusuna şu bilgileri girin:
   - **Sunucu türü**: Veritabanı altyapısını belirtin
   - **Sunucu adı**: **mynewserver20170313.database.windows.net** gibi bir tam sunucu adı girin
   - **Kimlik doğrulama**: SQL Server Kimlik Doğrulaması belirtin
   - **Kullanıcı adı**: Sunucu yöneticisi hesabınızı girin
   - **Parola**: Sunucu yöneticisi hesabınızın parolasını girin

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)  

3. **Sunucuya bağlan** iletişim kutusunda **Seçenekler**’e tıklayın. **Veritabanına bağlan** bölümünde bu veritabanına bağlanmak için **mySampleDatabase** yazın.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**'a tıklayın. SSMS’te Nesne Gezgini penceresi açılır. 

   ![sunucuya bağlanıldı](./media/sql-database-connect-query-ssms/connected.png)  

5. Nesne Gezgini’nde **Veritabanları**’nı ve ardından **mySampleDatabase** öğesini genişleterek nesneleri örnek veritabanında görüntüleyin.

## <a name="query-data"></a>Verileri sorgulama

[SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimini kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın.

1. Nesne Gezgini’nde **mySampleDatabase** öğesine sağ tıklayıp **Yeni Sorgu**’ya tıklayın. Veritabanınıza bağlı boş bir sorgu penceresi açılır.
2. Sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda **Yürüt**’e tıklayarak Product ve ProductCategory tablolarından verileri alın.

    ![sorgu](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a>Veri ekleme

[INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimini kullanarak SalesLT.Product tablosuna yeni ürün eklemek için aşağıdaki kodu kullanın.

1. Sorgu penceresinde önceki sorguyu aşağıdaki sorguyla değiştirin:

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

2. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosuna yeni bir satır ekleyin.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Verileri güncelleştirme

[UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü güncelleştirmek için aşağıdaki kodu kullanın.

1. Sorgu penceresinde önceki sorguyu aşağıdaki sorguyla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosunda belirtilen satırı güncelleştirin.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Verileri silme

[DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü silmek için aşağıdaki kodu kullanın.

1. Sorgu penceresinde önceki sorguyu aşağıdaki sorguyla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Araç çubuğunda **Yürüt**’e tıklayarak Product tablosunda belirtilen satırı silin.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Sonraki adımlar

- SSMS hakkında bilgi için bkz. [SQL Server Management Studio'yu Kullanma](https://msdn.microsoft.com/library/ms174173.aspx).
- Visual Studio Code’u kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).

