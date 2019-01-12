---
title: "VS Code'u: Azure SQL veritabanı'nda verileri bağlama ve sorgulama | Microsoft Docs"
description: Visual Studio Code kullanarak SQL Veritabanına bağlanma hakkında bilgi edinin. Ardından, verileri sorgulamak ve düzenlemek için Transact-SQL (T-SQL) deyimleri çalıştırın.
keywords: sql veritabanına bağlanma
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
ms.date: 01/11/2019
ms.openlocfilehash: 53c325e586aa96f96a51ce4dd8b320bb7b50b4b2
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247801"
---
# <a name="quickstart-use-visual-studio-code-to-connect-and-query-an-azure-sql-database"></a>Hızlı Başlangıç: Bağlanmak ve bir Azure SQL veritabanı sorgulamak için Visual Studio Code'u kullanma

[Visual Studio Code](https://code.visualstudio.com/docs) Linux, macOS ve Windows için bir grafiksel kod düzenleyicisidir. Dahil olmak üzere uzantıları destekleyen [mssql uzantısı](https://aka.ms/mssql-marketplace) Microsoft SQL Server, Azure SQL veritabanı ve SQL veri ambarı'nı sorgulamak için. Bu hızlı başlangıçta, Visual Studio Code kullanarak Azure SQL veritabanına bağlanan ve ardından sorgulama, ekleme, güncelleştirme için Transact-SQL deyimleri çalıştırın ve verileri silmek için kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

#### <a name="install-visual-studio-code"></a>Visual Studio Kodu'nu yükle

En son yüklediğinizden emin olun [Visual Studio Code](https://code.visualstudio.com/Download) yüklenip yüklenmediğini [mssql uzantısı](https://aka.ms/mssql-marketplace). Mssql uzantısı yükleme hakkında yönergeler için bkz. [VS Code yükleme](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-and-start-visual-studio-code) ve [Visual Studio Code için mssql ](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql).

## <a name="configure-visual-studio-code"></a>Visual Studio Code'u yapılandırma 

### <a name="mac-os"></a>**Mac OS**
MacOS için OpenSSL, yüklemeniz gerekir. bir önkoşul olan.Net Core için mssql uzantısının kullanır. **brew** ve **OpenSSL**’yi yüklemek için terminalinizi açın aşağıdaki komutları girin. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install openssl
mkdir -p /usr/local/lib
ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Hiçbir özel yapılandırma gerekmez.

### <a name="windows"></a>**Windows**

Hiçbir özel yapılandırma gerekmez.

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="set-language-mode-to-sql"></a>Dili modunu SQL’e ayarlama

Dil modunu Visual Studio Code'da kümesine **SQL** mssql komutlarını ve T-SQL IntelliSense'i etkinleştirmek için.

1. Yeni bir Visual Studio Code penceresi açın. 

2. Tuşuna **Ctrl**+**N**. Yeni bir düz metin dosyası açılır. 

3. Seçin **düz metin** durum çubuğunun sağ alt köşesindeki içinde.

4. İçinde **dil modu Seç** açıldığında seçin açılan menüsü **SQL**. 

## <a name="connect-to-your-database"></a>Veritabanınıza bağlanın

Visual Studio Code’u kullanarak Azure SQL Veritabanı sunucunuzla bağlantı kurun.

> [!IMPORTANT]
> Devam etmeden önce sunucunuzun olması ve hazır bilgi oturum emin olun. Odağınızı Visual Studio Code'dan değiştirirseniz bağlantı profili bilgilerini girmeye başladıktan sonra profil oluşturma işlemini yeniden başlatmanız gerekir.
>

1. Visual Studio Code'da basın **Ctrl + Shift + P** (veya **F1**) komut paletini açın.

2. Seçin **MS SQL: Connect** ve **Enter**.

3. Seçin **bağlantı profili oluşturmak**.

4. Yeni profilinin bağlantı özelliklerini belirtmek için istemleri izleyin. Her bir değeri belirttikten sonra seçin **Enter** devam etmek için. 

   | Özellik       | Önerilen değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Sunucu adı** | Tam sunucu adı | Aşağıdakine benzer: **mynewserver20170313.database.windows.net**. |
   | **Veritabanı adı** | mySampleDatabase | Bağlanmak için veritabanı. |
   | **Kimlik doğrulaması** | SQL Oturum Açma| Bu öğreticide, SQL kimlik doğrulaması kullanır. |
   | **Kullanıcı adı** | Kullanıcı adı | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı kullanıcı adı. |
   | **Parola (SQL Oturum Açma)** | Parola | Sunucu oluşturmak için kullanılan sunucu yönetici hesabı parolası. |
   | **Parola kaydedilsin mi?** | Evet veya Hayır | Seçin **Evet** her defasında parolayı girmek istemiyorsanız. |
   | **Bu profil için bir ad girin** | Gibi bir profil adı **mySampleProfile** | Kaydedilen bir profilin bağlantınızı sonraki oturum açma hızlandırır. | 

   Başarılı olursa, profilin oluşturulup bağlandığını söyleyen bir bildirim görüntülenir.

## <a name="query-data"></a>Verileri sorgulama

Aşağıdaki komutu çalıştırın [seçin](https://msdn.microsoft.com/library/ms189499.aspx) sorgulamak için ilk 20 ürünü kategoriye göre Transact-SQL deyimi.

1. Düzenleyicisi penceresinde, aşağıdaki SQL sorgusunu yapıştırın.

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

2. Tuşuna **Ctrl**+**Shift**+**E** sorgu çalıştırmak ve sonuçları görüntülemek için `Product` ve `ProductCategory` tablolar.

    ![2 tablolarından veri almak için sorgu](./media/sql-database-connect-query-vscode/query.png)

## <a name="insert-data"></a>Veri ekleme

Aşağıdaki komutu çalıştırın [Ekle](https://msdn.microsoft.com/library/ms174335.aspx) yeni bir ürün eklemek için Transact-SQL deyimini `SalesLT.Product` tablo.

1. Önceki sorguyu Bununla değiştirin.

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

2. Tuşuna **Ctrl**+**Shift**+**E** yeni bir satır eklemek için `Product` tablo.

## <a name="update-data"></a>Verileri güncelleştirme

Aşağıdaki komutu çalıştırın [güncelleştirme](https://msdn.microsoft.com/library/ms177523.aspx) eklenen ürünü güncelleştirmek için Transact-SQL deyimi.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Tuşuna **Ctrl**+**Shift**+**E** belirtilen satırı güncelleştirmek için `Product` tablo.

## <a name="delete-data"></a>Verileri silme

Aşağıdaki komutu çalıştırın [Sil](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) yeni ürünü kaldırmak için Transact-SQL deyimi.

1. Önceki sorguyu Bununla değiştirin:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Tuşuna **Ctrl**+**Shift**+**E** belirtilen satırı silmek için `Product` tablo.

## <a name="next-steps"></a>Sonraki adımlar

- SQL Server Management Studio kullanarak bağlanıp sorgulamak için bkz: [hızlı başlangıç: Bir Azure SQL veritabanı ve sorgu verilerine bağlanmak için SQL Server Management Studio'yu kullanın](sql-database-connect-query-ssms.md).
- Azure portalını kullanarak bağlanmak ve sorgulamak için bkz: [hızlı başlangıç: Bağlanmak ve veri sorgulamak için Azure portalında SQL sorgu Düzenleyicisi'ni kullanmak](sql-database-connect-query-portal.md).
- Visual Studio Code'u kullanmaya ilişkin MSDN dergisi makalesi için bkz. [MSSQL uzantısı blog gönderisinden yararlanarak veritabanı IDE'si oluşturma](https://msdn.microsoft.com/magazine/mt809115).
