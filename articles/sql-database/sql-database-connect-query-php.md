---
title: Azure SQL veritabanını sorgulamak için PHP kullanma | Microsoft Docs
description: PHP kullanarak Azure SQL veritabanına bağlanan ve T-SQL deyimlerini kullanarak bir program oluşturma işleminin nasıl.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: php
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: v-masebo
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: b3fe6e0249143b27cb763401a8d328922ed1fe99
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173930"
---
# <a name="quickstart-use-php-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: PHP kullanarak Azure SQL veritabanı sorgulama

Bu makalede nasıl yapılacağı açıklanır [PHP](http://php.net/manual/en/intro-whatis.php) bir Azure SQL veritabanına bağlanmak için. Ardından, T-SQL deyimleriyle veri kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Yüklü işletim sisteminiz için PHP ile ilgili yazılım:

    - **MacOS**, PHP, ODBC sürücüsünü yükledikten sonra SQL Server için PHP sürücüsü yükleyin. Bkz: [1, 2 ve 3. adım](/sql/connect/php/installation-tutorial-linux-mac).

    - **Linux**, PHP, ODBC sürücüsünü yükledikten sonra SQL Server için PHP sürücüsü yükleyin. Bkz: [1, 2 ve 3. adım](/sql/connect/php/installation-tutorial-linux-mac).

    - **Windows**, IIS Express ve Chocolatey için PHP'yi yükleyin ve ardından ODBC sürücüsü ile SQLCMD'yi yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).

## <a name="get-database-connection"></a>Veritabanı bağlantı Al

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="add-code-to-query-database"></a>Veritabanını sorgula için kod ekleyin

1. Sık kullandığınız metin düzenleyicisinde *sqltest.php* adında yeni bir dosya oluşturun.  

1. Dosyanın içeriğini aşağıdaki kodla değiştirin. Ardından, sunucu, veritabanı, kullanıcı ve parola için uygun değerleri ekleyin.

   ```PHP
   <?php
       $serverName = "your_server.database.windows.net"; // update me
       $connectionOptions = array(
           "Database" => "your_database", // update me
           "Uid" => "your_username", // update me
           "PWD" => "your_password" // update me
       );
       //Establishes the connection
       $conn = sqlsrv_connect($serverName, $connectionOptions);
       $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
            FROM [SalesLT].[ProductCategory] pc
            JOIN [SalesLT].[Product] p
            ON pc.productcategoryid = p.productcategoryid";
       $getResults= sqlsrv_query($conn, $tsql);
       echo ("Reading data from table" . PHP_EOL);
       if ($getResults == FALSE)
           echo (sqlsrv_errors());
       while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
        echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
       }
       sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde, uygulamayı çalıştırın.

   ```bash
   php sqltest.php
   ```

1. En çok 20 satırlar döndürülür ve uygulama penceresini kapatın doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)

- [SQL Server için Microsoft PHP Sürücüleri](https://github.com/Microsoft/msphpsql/)

- [Sorun bildirin veya soru sorun](https://github.com/Microsoft/msphpsql/issues)

- [Yeniden deneme mantığı örneği: Dayanıklı PHP ile SQL bağlantısı kurma](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)
