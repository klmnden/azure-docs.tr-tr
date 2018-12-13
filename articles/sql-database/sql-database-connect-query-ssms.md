---
title: 'SSMS: Azure SQL Veritabanında verileri bağlama ve sorgulama | Microsoft Docs'
description: SQL Server Management Studio (SSMS) kullanarak Azure'da SQL Database'e nasıl bağlanılacağını öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma,sql server management studio
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b3342164aec49967e819c316827dca9a65f2674f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098966"
---
# <a name="quickstart-use-sql-server-management-studio-to-connect-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak kullanım SQL Server Management Studio

Kullanabileceğiniz [SQL Server Management Studio] [ ssms-install-latest-84g] (SSMS) Microsoft Windows için SQL veritabanı için SQL Server'dan tüm SQL altyapılarını yönetme. Bu hızlı başlangıçta SSMS kullanarak Azure SQL veritabanına bağlanan ve ardından sorgulama, ekleme, güncelleştirme için Transact-SQL deyimleri çalıştırın ve verileri silmek için nasıl kullanılacağını gösterir. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

* Yapılandırılmış sunucu düzeyinde güvenlik duvarı kuralı. Daha fazla bilgi için [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-get-started-portal-firewall.md).

#### <a name="install-the-latest-ssms"></a>En son SSMS’yi yükleyin

Başlamadan önce en son yüklediğinizden emin olun [SSMS][ssms-install-latest-84g]. 

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

SMSS Azure SQL veritabanı sunucunuza bağlanın. 

> [!IMPORTANT]
> Azure SQL Veritabanı mantıksal sunucusu 1433 numaralı bağlantı noktasında dinler. Kurumsal bir güvenlik duvarının korumasında mantıksal sunucusuna bağlanmak için güvenlik duvarının Bu bağlantı noktası açık olması gerekir.
>

1. SSMS’i açın. **Sunucuya Bağlan** iletişim kutusu görüntülenir.

2. Aşağıdaki bilgileri girin:

   | Ayar      | Önerilen değer    | Açıklama | 
   | ------------ | ------------------ | ----------- | 
   | **Sunucu türü** | Veritabanı altyapısı | Gerekli değer. |
   | **Sunucu adı** | Tam sunucu adı | Aşağıdakine benzer: **mynewserver20170313.database.windows.net**. |
   | **Kimlik doğrulaması** | SQL Server Kimlik Doğrulaması | Bu öğreticide, SQL kimlik doğrulaması kullanır. |
   | **Oturum açma** | Sunucu yönetici hesabının kullanıcı kimliği | Sunucu oluşturmak için kullanılan sunucu yönetici hesabına ait kullanıcı kimliği. |
   | **Parola** | Sunucu yönetici hesabı parolası | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı parolası. |
   ||||

   ![sunucuya bağlan](./media/sql-database-connect-query-ssms/connect.png)  

3. Seçin **seçenekleri** içinde **sunucuya Bağlan** iletişim kutusu. İçinde **veritabanına bağlan** açılan menüsünde, select **mySampleDatabase**.

   ![sunucuda veritabanına bağlanma](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. **Bağlan**’ı seçin. Nesne Gezgini penceresi açılır. 

5. Veritabanı nesneleri görüntülemek için genişletin **veritabanları** ve ardından **mySampleDatabase**.

   ![Veritabanı nesneleri görüntüle](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="query-data"></a>Verileri sorgulama

Aşağıdaki [seçin](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL kodunu sorgulamak için ilk 20 ürünü kategoriye göre.

1. Nesne Gezgini'nde sağ **mySampleDatabase** seçip **yeni sorgu**. Veritabanınıza bağlı boş bir sorgu penceresi açılır.

1. Bu SQL sorgusunu sorgu penceresine yapıştırın.

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda **yürütme** verilerin alınacağı `Product` ve `ProductCategory` tablolar.

    ![2 tablolarından veri almak için sorgu](./media/sql-database-connect-query-ssms/query2.png)

## <a name="insert-data"></a>Veri ekleme

Aşağıdaki [Ekle](https://msdn.microsoft.com/library/ms174335.aspx) yeni bir ürün oluşturmak için Transact-SQL kodu `SalesLT.Product` tablo.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate] )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

2. Seçin **yürütme** Product tablosuna yeni bir satır eklemek için. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

## <a name="view-the-result"></a>Görünüm sonucu

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   SELECT * FROM [SalesLT].[Product] 
   WHERE Name='myNewProduct' 

2. Select **Execute**. The following result appears. 

   ![result](./media/sql-database-connect-query-ssms/result.png)

 
## Update data

Use the following [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL code to modify the new product you just added.

1. Replace the previous query with this one.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Seçin **yürütme** Product tablosunda belirtilen satırı güncelleştirin. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

## <a name="delete-data"></a>Verileri silme

Aşağıdaki [Sil](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL kod daha önce eklemiş olduğunuz yeni ürünü kaldırın.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Seçin **yürütme** Product tablosunda belirtilen satırı silin. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

## <a name="next-steps"></a>Sonraki adımlar

- SSMS hakkında daha fazla bilgi için bkz: [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Azure portalını kullanarak bağlanmak ve sorgulamak için bkz. [Azure Portal SQL Sorgusu düzenleyicisiyle bağlanma ve sorgulama](sql-database-connect-query-portal.md).
- Visual Studio Code’u kullanarak bağlanmak ve sorgulamak için bkz. [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).


<!-- Article link references. -->

[ssms-install-latest-84g]: https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms

