---
title: "SSMS: Azure SQL veritabanı'nda verileri bağlama ve sorgulama | Microsoft Docs"
description: SQL Server Management Studio (SSMS) kullanarak Azure'da SQL Database'e nasıl bağlanılacağını öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma,sql server management studio
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 636adcf15479fe8e072b833b59ddc78b5f820550
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53745083"
---
# <a name="quickstart-use-sql-server-management-studio-to-connect-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için SQL Server Management Studio'yu kullanın.

Bu hızlı başlangıçta kullanacaksınız [SQL Server Management Studio] [ ssms-install-latest-84g] (SSMS) kullanarak Azure SQL veritabanına bağlanmak için. Ardından, Transact-SQL deyimleriyle sorgulama, ekleme, güncelleştirme ve verileri silmek çalıştıracaksınız. SSMS, SQL Server'dan SQL veritabanı için Microsoft Windows tüm SQL altyapılarını yönetmek için kullanabilirsiniz.  

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

Bu çalıştırma [seçin](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL kodunu sorgulamak için ilk 20 ürünü kategoriye göre.

1. Nesne Gezgini'nde sağ **mySampleDatabase** seçip **yeni sorgu**. Veritabanınıza bağlı yeni bir sorgu penceresi açılır.

2. Bu SQL sorgusunu sorgu penceresine yapıştırın.

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Araç çubuğunda **yürütme** verilerin alınacağı `Product` ve `ProductCategory` tablolar.

    ![iki tablodan verileri almak için sorgu](./media/sql-database-connect-query-ssms/query2.png)

## <a name="insert-data"></a>Veri ekleme

Bu çalıştırma [Ekle](https://msdn.microsoft.com/library/ms174335.aspx) yeni bir ürün oluşturmak için Transact-SQL kodu `SalesLT.Product` tablo.

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

2. Seçin **yürütme** yeni bir satır eklemek için `Product` tablo. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

## <a name="view-the-result"></a>Görünüm sonucu

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   SELECT * FROM [SalesLT].[Product] 
   WHERE Name='myNewProduct' 

2. Select **Execute**. The following result appears. 

   ![result](./media/sql-database-connect-query-ssms/result.png)

 
## Update data

Run this [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL code to modify your new product.

1. Replace the previous query with this one.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Seçin **yürütme** belirtilen satırı güncelleştirmek için `Product` tablo. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

## <a name="delete-data"></a>Verileri silme

Bu çalıştırma [Sil](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL kodunu yeni ürün kaldırmak için.

1. Önceki sorguyu Bununla değiştirin.

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Seçin **yürütme** belirtilen satırı silmek için `Product` tablo. **İletileri** bölmesini görüntüler **(1 satır etkilendi)**.

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

