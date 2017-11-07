---
title: "Azure portalı: Sorgu Düzenleyicisi'ni kullanarak Azure SQL Veritabanını sorgulama | Microsoft Docs"
description: "SQL Sorgu Düzenleyicisi'ni kullanarak Azure portalında SQL Veritabanına bağlanmayı öğrenin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın."
metacanonical: 
keywords: "sql veritabanına bağlanma,azure portalı, portal, sorgu düzenleyicisi"
services: sql-database
documentationcenter: 
author: ayoolubeko
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/01/2017
ms.author: ayolubek
ms.openlocfilehash: 788b9d2e7ff9e1dba2aca5eca982e4ddf045a6c4
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-portal-use-the-sql-query-editor-to-connect-and-query-data"></a>Azure portalı: SQL Sorgu Düzenleyicisi'ni kullanarak bağlanma ve veri sorgulama

SQL Sorgu Düzenleyicisi, Azure portalından ayrılmadan Azure SQL Veritabanı veya Azure SQL Veri Ambarı’nda SQL sorguları yürütmenin verimli ve kolay bir yolunu sunan bir tarayıcı sorgulama aracıdır. Bu hızlı başlangıçta Sorgu Düzenleyicisi’ni kullanarak bir SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)


## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.


## <a name="connect-using-sql-authentication"></a>SQL Kimlik Doğrulaması kullanarak bağlanma
> [!NOTE]
> SQL Server güvenlik duvarı ayarlarınızda "Azure Hizmetlerine erişime izin ver" seçeneğinin "Açık" olarak ayarlandığından emin olun. Bu seçenek, SQL Sorgu Düzenleyicisi'nin veritabanlarınıza ve veri ambarlarınıza erişmesini sağlar.

1. Soldaki menüden **SQL veritabanları**’na ve sorgulamak istediğiniz veritabanına tıklayın.

2. Veritabanınız için SQL veritabanı sayfasında araç çubuğundaki **Araçlar**’a tıklayın. Araçlar sayfası açılır.

    ![araçlar menüsü](./media/sql-database-connect-query-portal/tools-menu.png)

3. **Sorgu düzenleyicisi (önizleme)**’ye tıklayın, **Önizleme koşulları** onay kutusuna tıklayın ve ardından **Tamam**’a tıklayın. Sorgu düzenleyicisi sayfası açılır.

4. **Oturum aç**’a tıklayın ve **SQL Server kimlik doğrulaması**’nı seçtikten sonra veritabanını oluştururken sağladığınız sunucu yöneticisi oturum açma bilgilerini ve parolayı girin.

    ![oturum açma](./media/sql-database-connect-query-portal/login-menu.png) 

5. Oturum açmak için **Tamam**’a tıklayın.


## <a name="connect-using-azure-ad"></a>Azure AD kullanarak bağlanma

Bir Active Directory yöneticisinin yapılandırılması, Azure portalında ve SQL veritabanınızda oturum açmak için tek bir kimlik kullanmanıza olanak sağlar. Oluşturduğunuz SQL Server’a ait bir active directory yöneticisi yapılandırmak için aşağıdaki adımları izleyin.

> [!NOTE]
> E-posta hesapları (örneğin outlook.com, hotmail.com, live.com, gmail.com, yahoo.com), Active Directory yöneticileri olarak henüz desteklenmemektedir. Azure Active Directory’de yerel olarak oluşturulmuş veya Azure Active Directory ile birleştirilmiş bir kullanıcı seçtiğinizden emin olun.

1. Soldaki menüden **SQL Server’lar** öğesini seçin ve sunucu listesinden SQL Server’ınızı seçin.

2. SQL Server’ınızın ayarlar menüsünden **Active Directory Yöneticisi** ayarını seçin.

3. Active Directory yöneticisi dikey penceresinde **Yönetici ayarla** komutuna tıklayın ve Active Directory yöneticisi olacak kullanıcıyı veya grubu seçin.

    ![active directory seçme](./media/sql-database-connect-query-portal/select-active-directory.png) 

4. Active Directory yöneticinizi ayarlamak için Active Directory yöneticisi dikey penceresinin üstündeki **Kaydet** komutuna tıklayın.

Sorgulamak istediğiniz SQL veritabanına gidin, araç çubuğundaki **Araçlar komutuna** tıklayın ve **Sorgu düzenleyicisi (önizleme)** seçeneğini belirleyin. Sorgu düzenleyicisi sayfası açılır ve sizi otomatik olarak veritabanına bağlar.


## <a name="run-query-using-query-editor"></a>Sorgu Düzenleyicisi'ni kullanarak sorgu çalıştırma

Kimlik doğrulaması yaptıktan sonra kategoriye göre ilk 20 ürünü sorgulamak için Sorgu düzenleyicisi bölmesine aşağıdaki sorguyu yazın.

```sql
 SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
 FROM SalesLT.ProductCategory pc
 JOIN SalesLT.Product p
 ON pc.productcategoryid = p.productcategoryid;
```

**Çalıştır**’a tıklayın ve **Sonuçlar** bölmesindeki sorgu sonuçlarını gözden geçirin.

![sorgu düzenleyicisi sonuçları](./media/sql-database-connect-query-portal/query-editor-results.png)

## <a name="insert-data-using-query-editor"></a>Sorgu Düzenleyicisi'ni kullanarak veri ekleme

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

2. Araç çubuğunda **Çalıştır**’a tıklayarak Product tablosuna yeni bir satır ekleyin.

## <a name="update-data-using-query-editor"></a>Sorgu Düzenleyicisi'ni kullanarak veri güncelleştirme

[UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü güncelleştirmek için aşağıdaki kodu kullanın.

1. Sorgu penceresinde önceki sorguyu aşağıdaki sorguyla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Araç çubuğunda **Çalıştır**’a tıklayarak Product tablosunda belirtilen satırı güncelleştirin.

## <a name="delete-data-using-query-editor"></a>Sorgu Düzenleyicisi'ni kullanarak veri silme

[DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü silmek için aşağıdaki kodu kullanın.

1. Sorgu penceresinde önceki sorguyu aşağıdaki sorguyla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Araç çubuğunda **Çalıştır**’a tıklayarak Product tablosunda belirtilen satırı silin.


## <a name="query-editor-considerations"></a>Sorgu Düzenleyicisi konuları

Sorgu Düzenleyicisi önizleme durumundayken onunla çalışma hakkında bilmeniz gereken birkaç şey vardır:

1. Azure SQL Server güvenlik duvarı ayarlarınızda "Azure Hizmetlerine erişime izin ver" seçeneğini "Açık" olarak ayarladığınızdan emin olun. Bu seçenek, SQL Sorgu Düzenleyicisi'nin SQL veritabanlarınıza ve veri ambarlarınıza erişmesini sağlar.

2. Azure Active Directory Yöneticisi oturum açma işlemi, 2 faktörlü kimlik doğrulama özelliğinin etkin olduğu hesaplarla çalışmaz. 

3. E-posta hesapları (örneğin outlook.com, hotmail.com, live.com, gmail.com, yahoo.com), Active Directory yöneticileri olarak henüz desteklenmemektedir. Azure Active Directory’de yerel olarak oluşturulmuş veya Azure Active Directory ile birleştirilmiş bir kullanıcı seçtiğinizden emin olun

4. Uzamsal veri türleri, Sorgu Düzenleyicisi'nde henüz desteklenmemektedir. Uzamsal sütunların sorgulanması bir 'System.IO.FileNotFoundException' hatasına neden olur.

5. Veritabanı tabloları ve görünümleri için IntelliSense desteği yoktur. Ancak, düzenleyici zaten girilmiş adları otomatik olarak tamamlamayı destekler. 

6. F5 tuşuna basıldığında Sorgu düzenleyicisi sayfası yenilenir ve üzerinde çalışılan sorgu kaybedilir. Sorguları yürütmek için araç çubuğundaki Çalıştır düğmesini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanlarında desteklenen Transact-SQL hakkında bilgi almak için [SQL veritabanında Transact-SQL farklılıkları](sql-database-transact-sql-information.md) konusunu inceleyin.
