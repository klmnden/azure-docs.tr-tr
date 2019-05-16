---
title: Azure SQL Veritabanı sorgulamak için Node.js kullanma | Microsoft Docs
description: Bir Azure SQL veritabanına bağlanan ve T-SQL deyimlerini kullanarak bir program oluşturmak için node.js kullanma
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: v-masebo
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 8d050fe92af7b22363b0a9207201412bc12d9082
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65753980"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Node.js kullanarak Azure SQL veritabanı sorgulama

Bu makalede nasıl yapılacağı açıklanır [Node.js](https://nodejs.org) bir Azure SQL veritabanına bağlanmak için. Ardından, T-SQL deyimleriyle veri kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandır | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Verileri yükleyin|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.


- İşletim sisteminiz için node.js ile ilgili yazılım:

  - **MacOS**Homebrew ve Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
  
  - **Ubuntu**Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
  
  - **Windows**, Chocolatey ve Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi. 

## <a name="create-the-project"></a>Proje oluşturma

Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasöre gidin ve aşağıdaki komutu çalıştırın:

  ```bash
  npm init -y
  npm install tedious@5.0.3
  npm install async@2.6.2
  ```

## <a name="add-code-to-query-database"></a>Veritabanını sorgula için kod ekleyin

1. Sık kullandığınız metin düzenleyicinizde yeni bir dosya oluşturun *sqltest.js*.

1. Dosyanın içeriğini aşağıdaki kodla değiştirin. Ardından, sunucu, veritabanı, kullanıcı ve parola için uygun değerleri ekleyin.

    ```js
    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;

    // Create connection to database
    var config =
    {
        authentication: {
            options: {
                userName: 'userName', // update me
                password: 'password' // update me
            },
            type: 'default'
        },
        server: 'your_server.database.windows.net', // update me
        options:
        {
            database: 'your_database', //update me
            encrypt: true
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
    {
        console.log('Reading rows from the Table...');

        // Read all rows from table
        var request = new Request(
            "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc "
                + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
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

> [!NOTE]
> Kod örneği kullanan **AdventureWorksLT** Azure SQL veritabanı örneği.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde programı çalıştırın.

    ```bash
    node sqltest.js
    ```

1. En çok 20 satırlar döndürülür ve uygulama penceresini kapatın doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server için Microsoft Node.js Sürücüsü](/sql/connect/node-js/node-js-driver-for-sql-server)

- Bağlanma ve sorgulama Windows/Linus/macos'ta ile [.NET core](sql-database-connect-query-dotnet-core.md), [Visual Studio Code](sql-database-connect-query-vscode.md), veya [SSMS](sql-database-connect-query-ssms.md) (yalnızca Windows)

- [Windows/Linus/macos'ta komut satırını kullanarak .NET Core ile çalışmaya başlama](/dotnet/core/tutorials/using-with-xplat-cli)

- İlk Azure SQL veritabanı kullanarak tasarım [.NET](sql-database-design-first-database-csharp.md) veya [SSMS](sql-database-design-first-database.md)
