---
title: Azure SQL Veritabanı sorgulamak için Node.js kullanma | Microsoft Docs
description: Bir Azure SQL veritabanına bağlanan ve T-SQL deyimlerini kullanarak bir program oluşturmak için node.js kullanma
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: nodejs
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/26/2018
ms.openlocfilehash: c6e518f644e8fc43db58ef481312f44b2071a0dc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194313"
---
# <a name="quickstart-use-nodejs-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Node.js kullanarak Azure SQL veritabanı sorgulama

Bu makalede nasıl yapılacağı açıklanır [Node.js](https://nodejs.org) bir Azure SQL veritabanına bağlanmak için. Ardından, T-SQL deyimleriyle veri kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- İşletim sisteminiz için node.js ile ilgili yazılım:

  - **MacOS**Homebrew ve Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
  
  - **Ubuntu**Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
  
  - **Windows**, Chocolatey ve Node.js yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="get-database-connection"></a>Veritabanı bağlantı Al

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

> [!IMPORTANT]
> Bu hızlı başlangıç öğreticisinde kullanacağınız bilgisayarın genel IP adresi için bir güvenlik duvarı kuralınız olmalıdır. Farklı bir bilgisayarda olduğunuz veya farklı bir genel IP adresine sahip yoksa, oluşturun bir [Azure portalını kullanarak sunucu düzeyinde güvenlik duvarı kuralı](sql-database-get-started-portal-firewall.md).

## <a name="create-the-project"></a>Proje oluşturma

Komut istemini açın ve *sqltest* adlı bir klasör oluşturun. Oluşturduğunuz klasöre gidin ve aşağıdaki komutu çalıştırın:

  ```bash
  npm init -y
  npm install tedious
  npm install async
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
        userName: 'your_username', // update me
        password: 'your_password', // update me
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
