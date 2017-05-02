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
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 5b623c78f8b8eac846c5ca244f1e0b25ee4f400f
ms.lasthandoff: 04/18/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Veritabanı: Visual Studio Code kullanarak verileri bağlama ve sorgulama

Linux, macOS ve Windows’a yönelik bir grafik kod düzenleyicisi olan [Visual Studio Code](https://code.visualstudio.com/docs); Microsoft SQL Server, Azure SQL Veritabanı ve SQL Veri Ambarı’nı sorgulamak için kullanılabilen [mssql uzantısı](https://aka.ms/mssql-marketplace) gibi uzantıları destekler. Bu hızlı başlangıçta Visual Studio Code’u kullanarak bir Azure SQL veritabanına bağlanma ve daha sonra Transact-SQL deyimlerini kullanarak veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanır.

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)

Başlamadan önce en yeni [Visual Studio Code](https://code.visualstudio.com/Download) sürümünü yüklediğinizden [mssql uzantısının](https://aka.ms/mssql-marketplace) yüklü olduğundan emin olun. mssql uzantısına ilişkin yükleme yönergeleri için bkz. [VS Code Yükleme](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) ve [Visual Studio Code için mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql). 

## <a name="configure-vs-code-mac-os-only"></a>VS Code’u Yapılandırma (yalnızca Mac OS)

### <a name="mac-os"></a>**Mac OS**
macOS için, mssql uzantısının kullandığı DotNet Core’a yönelik bir ön koşul olan OpenSSL’yi yüklemeniz gerekir. **brew** ve **OpenSSL**’yi yüklemek için terminalinizi açın aşağıdaki komutları girin. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure SQL Veritabanı sunucunuzun tam sunucu adını Azure portaldan alabilirsiniz. Tam sunucu adını, Visual Studio Code kullanarak sunucunuza bağlanmak için kullanırsınız.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz.

   ![bağlantı bilgileri](./media/sql-database-connect-query-ssms/connection-information.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın. 

## <a name="set-language-mode-to-sql"></a>Dili modunu SQL’e ayarlama

mssql komutlarını ve T-SQL IntelliSense’i etkinleştirmek için dil modunu Visual Studio Code’da **SQL** olarak ayarlayın.

1. Yeni bir Visual Studio Code penceresi açın. 

2. Durum çubuğunun sağ alt köşesindeki **Düz Metin**’e tıklayın.
3. Görüntülenen **Dil modu seç** açılır menüsüne **SQL** yazın ve **ENTER** tuşuna basarak dil modunu SQL’e ayarlayın. 

   ![SQL dil modu](./media/sql-database-connect-query-vscode/vscode-language-mode.png)

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>SQL Veritabanı mantıksal sunucusunda veritabanınıza bağlanma

Visual Studio Code’u kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

> [!IMPORTANT]
> Devam etmeden önce sunucu, veritabanı ve oturum açma bilgilerinizin hazır olduğundan emin olun. Bağlantı profili bilgilerini girmeye başladıktan sonra, odağınızı Visual Studio Code’dan değiştirirseniz bağlantı profili oluşturma işlemini yeniden başlatmanız gerekir.
>

1. VS Code’da **CTRL+SHIFT+P** (veya **F1**) tuşlarına basarak Komut Paletini açın.

2. **sqlcon** yazıp **ENTER** tuşuna basın.

3. **ENTER** tuşuna basarak **Bağlantı Profili Oluştur**’u seçin. Bu işlem, SQL Server örneğiniz için bir bağlantı profili oluşturur.

4. Yeni bağlantı profilinin bağlantı özelliklerini belirtmek için istemleri izleyin. Her bir değeri belirttikten sonra **ENTER** tuşuna basarak devam edin. 

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

5. Profilin oluşturulup bağlandığını bildiren bilgi iletisini kapatmak için **ESC** tuşuna basın.

6. Durum çubuğunda bağlantınızı doğrulayın.

   ![Bağlantı durumu](./media/sql-database-connect-query-vscode/vscode-connection-status.png)

## <a name="query-data"></a>Verileri sorgulama

Azure SQL veritabanınızdaki verileri sorgulamak için [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) Transact-SQL deyimini kullanın.

1. **Düzenleyici** penceresinde boş sorgu penceresine aşağıdaki sorguyu girin:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. **CTRL+SHIFT+E** tuşlarına basarak Product ve ProductCategory tablolarından verileri alın.

    ![Sorgu](./media/sql-database-connect-query-vscode/query.png)

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

2. Product tablosuna yeni bir satır eklemek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="update-data"></a>Verileri güncelleştirme

Azure SQL veritabanınızdaki verileri güncelleştirmek için [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) Transact-SQL deyimini kullanın.

1.  **Düzenleyici** penceresinde önceki sorguyu silip aşağıdaki sorguyu girin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Product tablosunda belirtilen satırı güncelleştirmek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="delete-data"></a>Verileri silme

Azure SQL veritabanınızdaki verileri silmek için [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) Transact-SQL deyimini kullanın.

1. **Düzenleyici** penceresinde önceki sorguyu silip aşağıdaki sorguyu girin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Product tablosunda belirtilen satırı silmek için **CTRL+SHIFT+E** tuşlarına basın.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio kullanarak bağlanmak ve sorgu yürütmek için, bkz. [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- .NET kullanarak bağlanıp sorgulamak için bkz. [.NET ile bağlanma ve sorgulama](sql-database-connect-query-dotnet.md).
- PHP kullanarak bağlanıp sorgulamak için bkz. [PHP ile bağlanma ve sorgulama](sql-database-connect-query-php.md).
- Node.js kullanarak bağlanıp sorgulamak için bkz. [Node.js ile bağlanma ve sorgulama](sql-database-connect-query-nodejs.md).
- Java kullanarak bağlanıp sorgulamak için bkz. [Java ile bağlanma ve sorgulama](sql-database-connect-query-java.md).
- Python kullanarak bağlanıp sorgulamak için bkz. [Python ile bağlanma ve sorgulama](sql-database-connect-query-python.md).
- Ruby kullanarak bağlanıp sorgulamak için bkz. [Ruby ile bağlanma ve sorgulama](sql-database-connect-query-ruby.md).

