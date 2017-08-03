---
title: "Azure SQL Veritabanı sorgulamak için Node.js kullanma | Microsoft Docs"
description: "Bu konu başlığı altında, Node.js kullanarak Azure SQL Veritabanına bağlanan ve Transact-SQL deyimleriyle veritabanını sorgulayan bir program oluşturma işlemi gösterilir."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 1907a95df9132c059d7985b6d5cd913536bf3403
ms.contentlocale: tr-tr
ms.lasthandoff: 07/28/2017

---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a>Node.js kullanarak Azure SQL veritabanı sorgulama

Bu hızlı başlangıç öğreticisi, [Node.js](https://nodejs.org/en/) kullanarak Azure SQL veritabanına bağlanan ve Transact-SQL deyimleriyle veri sorgulayan bir program oluşturmayı gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç öğreticisini tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

- Bir Azure SQL veritabanı. Bu hızlı başlangıçta, aşağıdaki hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır: 

   - [DB Oluşturma - Portal](sql-database-get-started-portal.md)
   - [DB oluşturma - CLI](sql-database-get-started-cli.md)
   - [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

- Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için [sunucu düzeyinde bir güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).
- İşletim sisteminiz için Node.js ve ilgili yazılımları yüklediniz.
    - **MacOS**: Homebrew ve Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD’yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
    - **Ubuntu**: Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD’yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
    - **Windows**: Chocolatey ve Node.js yükleyin ve ardından ODBC sürücüsünü ve SQL CMD’yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="sql-server-connection-information"></a>SQL Server bağlantı bilgileri

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.

> [!IMPORTANT]
> Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için bir güvenlik duvarı kuralınız olmalıdır. Farklı bir bilgisayar kullanıyorsanız veya farklı bir genel IP adresiniz varsa [Azure portal kullanarak bir sunucu düzeyi güvenlik duvarı kuralı](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) oluşturun. 

## <a name="create-a-nodejs-project"></a>Node.js projesi oluşturma

Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasöre gidin ve aşağıdaki komutu çalıştırın:

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a>SQL veritabanını sorgulamak için kod girme

1. Geliştirme ortamınızda veya sık kullandığınız metin düzenleyicisinde **sqltest.js** adında yeni bir dosya oluşturun.

2. İçeriğini aşağıdaki kod ile değiştirin ve sunucunuz, veritabanınız, kullanıcınız ve parolanız için uygun değerleri ekleyin.

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde aşağıdaki komutları çalıştırın:

   ```js
   node sqltest.js
   ```

2. En üst 20 satırın döndürüldüğünü doğrulayın ve sonra uygulama penceresini kapatın.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server için Microsoft Node.js Sürücüsü](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/) hakkında bilgi edinin
- Windows/Linus/macOS’ta [.NET Core kullanarak Azure SQL veritabanını bağlamayı ve sorgulamayı](sql-database-connect-query-dotnet-core.md) öğrenin.  
- [Komut satırını kullanarak Windows/Linus/macOS’ta .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli) hakkında bilgi edinin.
- [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database.md) veya [.NET kullanarak ilk Azure SQL veritabanınızı tasarlamayı](sql-database-design-first-database-csharp.md) öğrenin.
- [SSMS ile bağlanma ve sorgulamayı](sql-database-connect-query-ssms.md) öğrenin
- [Visual Studio Code ile bağlanma ve sorgulamayı](sql-database-connect-query-vscode.md) öğrenin.



