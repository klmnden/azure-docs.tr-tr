---
title: "VS Code: Azure SQL Veritabanında verileri bağlama ve sorgulama | Microsoft Docs"
description: "Visual Studio Code kullanarak SQL Veritabanına bağlanma hakkında bilgi edinin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın."
metacanonical: 
keywords: "sql veritabanına bağlanma"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 86471fe29bbc9076624d96b83f7001d8755363bc
ms.lasthandoff: 03/25/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Veritabanı: Visual Studio Code kullanarak verileri bağlama ve sorgulama

[Visual Studio Code](https://code.visualstudio.com/docs) Linux, macOS ve Windows için uzantıları destekleyen bir grafiksel kod düzenleyicisidir. Bir Azure SQL veritabanına bağlanmak ve veritabanını sorgulamak için Visual Studio Code’u [mssql uzantısı](https://aka.ms/mssql-marketplace) ile birlikte kullanın. Bu kılavuzda bir Azure SQL veritabanına bağlanmak ve ardından query, insert, update ve delete deyimlerini yürütmek üzere Visual Studio Code kullanmayla ilgili ayrıntılar verilmektedir.

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

Başlamadan önce en yeni [Visual Studio Code](https://code.visualstudio.com/Download) sürümünü yüklediğinizden [mssql uzantısının](https://aka.ms/mssql-marketplace) yüklü olduğundan emin olun. mssql uzantısına ilişkin yükleme yönergeleri için bkz. [VS Code Yükleme](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code). 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. Tam sunucu adını, Visual Studio Code kullanarak sunucunuza bağlanmak için kullanırsınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın Azure portal sayfasındaki **Temel Bilgiler** bölmesinde, bu hızlı başlangıcın sonraki bölümlerinde kullanılacak **Sunucu adını** bulup kopyalayın.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="set-language-mode-to-sql"></a>Dili modunu SQL’e ayarlama

mssql komutlarını ve T-SQL IntelliSense’i etkinleştirmek için dil modunu Visual Studio Code’da **SQL** olarak ayarlayın.

1. Yeni bir Visual Studio Code penceresi açın. 

2. **CTRL+K,M** tuşlarına basın, **SQL** yazın ve **ENTER** tuşuna basarak dil modunu SQL’e ayarlayın. 

<img src="./media/sql-database-connect-query-vscode/vscode-language-mode.png" alt="SQL language mode" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Sunucuya bağlanma

Visual Studio Code’u kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

1. VS Code’da **CTRL+SHIFT+P** (veya **F1**) tuşlarına basarak Komut Paletini açın.

2. **sqlcon** yazıp **ENTER** tuşuna basın.

3. Dili **SQL**’e ayarlamak için **Evet**’e tıklayın.

4. **ENTER** tuşuna basarak **Bağlantı Profili Oluştur**’u seçin. Bu işlem, SQL Server örneğiniz için bir bağlantı profili oluşturur.

5. Yeni bağlantı profilinin bağlantı özelliklerini belirtmek için istemleri izleyin. Her bir değeri belirttikten sonra **ENTER** tuşuna basarak devam edin. 

   Aşağıdaki tabloda Bağlantı Profili özellikleri açıklanmaktadır.

   | Ayar | Açıklama |
   |-----|-----|
   | **Sunucu adı** | **mynewserver20170313.database.windows.net** gibi bir tam sunucu adı girin |
   | **Veritabanı adı** | **mySampleDatabase** gibi bir veritabanı adı girin |
   | **Kimlik doğrulaması** | SQL Oturum Açma’yı seçin |
   | **Kullanıcı adı** | Sunucu yöneticisi hesabınızı girin |
   | **Parola (SQL Oturum Açma)** | Sunucu yöneticisi hesabınızın parolasını girin | 
   | **Parola kaydedilsin mi?** | **Evet** veya **Hayır**'ı seçin |
   | **[İsteğe bağlı] Bu profil için bir ad girin** | **mySampleDatabase** gibi bir bağlantı profili adı girin. 

6. Profilin oluşturulup bağlandığını bildiren bilgi iletisini kapatmak için **ESC** tuşuna basın.

7. Durum çubuğunda bağlantınızı doğrulayın.

   <img src="./media/sql-database-connect-query-vscode/vscode-connection-status.png" alt="Connection status" style="width: 780px;" />

## <a name="query-data"></a>Verileri sorgulama

Azure SQL veritabanınızdaki verileri sorgulamak için [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimini kullanın.

1. **Düzenleyici** penceresinde boş sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. **CTRL+SHIFT+E** tuşlarına basarak Product ve ProductCategory tablolarından verileri alın.

    <img src="./media/sql-database-connect-query-vscode/query.png" alt="Query" style="width: 780px;" />

## <a name="insert-data"></a>Veri ekleme

Verileri Azure SQL veritabanınıza eklemek için [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) Transact-SQL deyimini kullanın.

1. **Düzenleyici** penceresinde önceki sorguyu silip aşağıdaki sorguyu girin:

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

3. Product tablosuna yeni bir satır eklemek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="update-data"></a>Verileri güncelleştirme

Azure SQL veritabanınızdaki verileri güncelleştirmek için [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini kullanın.

1.  **Düzenleyici** penceresinde önceki sorguyu silip aşağıdaki sorguyu girin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Product tablosunda belirtilen satırı güncelleştirmek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="delete-data"></a>Verileri silme

Azure SQL veritabanınızdaki verileri silmek için [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimini kullanın.

1. **Düzenleyici** penceresinde önceki sorguyu silip aşağıdaki sorguyu girin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Product tablosunda belirtilen satırı silmek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="next-steps"></a>Sonraki adımlar

- Visual Studio Code hakkında bilgi için bkz. [Visual Studio Code](https://code.visualstudio.com/docs)
- SQL Server Management Studio kullanarak veri sorgulama ve düzenleme hakkında bilgi için bkz. [SSMS](https://msdn.microsoft.com/library/ms174173.aspx).

