---
title: "PHP kullanarak Azure SQL Veritabanı&quot;na bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanı&quot;na bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir PHP kod örneği sağlanmıştır."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 5b90c9b49448c6bc225f9d6ba7227782b81fc5ed
ms.contentlocale: tr-tr
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-php-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Veritabanı: PHP kullanarak verileri bağlama ve sorgulama

Bu hızlı başlangıç kılavuzunda, Azure SQL veritabanına bağlanmak için [PHP](http://php.net/manual/en/intro-whatis.php)'yi kullanma ve ardından Transact-SQL deyimlerini kullanarak macOS, Ubuntu, Linux ve Windows platformlarındaki veritabanı verilerini sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanmıştır.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

<a id="install-php-and-database-communications-software" class="xliff"></a>

## PHP ve veritabanı iletişimleri yazılımını yükleme

Bu bölümdeki adımlarda, PHP kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve Azure SQL Veritabanı ile çalışmaya yeni başladığınız varsayılır. PHP ile geliştirmeye yeni başladıysanız, [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/en-us/sql-server/developer-get-started/) konusuna gidin, **PHP** dilini ve ardından işletim sisteminizi seçin.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Terminalinizi açın ve **brew**, **Mac için Microsoft ODBC Sürücüsü** ve **SQL Server için Microsoft PHP Sürücüleri** araçlarını yüklemek için aşağıdaki komutları girin. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql 
pecl install sqlsrv-4.1.7preview
pecl install pdo_sqlsrv-4.1.7preview
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
**Linux için Microsoft ODBC Sürücüsü** ve **SQL Server için Microsoft PHP Sürücüleri** araçlarını yüklemek için aşağıdaki komutları girin.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev gcc g++ php-dev
sudo pecl install sqlsrv pdo_sqlsrv
sudo echo "extension= pdo_sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
sudo echo "extension= sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
```

<a id="windows" class="xliff"></a>

### **Windows**
- [WebPlatform Installer aracından](https://www.microsoft.com/web/downloads/platform.aspx?lang=) PHP 7.1.1 (x64) yazılımını yükleyin 
- [Microsoft ODBC Sürücüsü 13.1](https://www.microsoft.com/download/details.aspx?id=53339) yazılımını yükleyin. 
- [SQL Server için Microsoft PHP Sürücüsü](https://pecl.php.net/package/sqlsrv/4.1.6.1/windows)'ne yönelik, iş parçacığı güvenli olmayan DLL'leri indirin ve ikili dosyaları PHP\v7.x\ext klasörüne yerleştirin.
- Ardından başvuruyu DLL dosyasına ekleyerek php.ini (C:\Program Files\PHP\v7.1\php.ini) dosyanızı düzenleyin. Örneğin:
      
      extension=php_sqlsrv.dll
      extension=php_pdo_sqlsrv.dll

Bu noktada, DLL'lerin PHP'ye kayıtlı olması gerekir.

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Sunucunuzun oturum açma bilgilerini unuttuysanız SQL Veritabanı sunucusu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.     
    
<a id="select-data" class="xliff"></a>

## Verileri seçme
[sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) işleviyle birlikte [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) Transact-SQL deyimini kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın. sqlsrv_query işlevi, SQL veritabanına yönelik bir sorguya ilişkin sonuç kümesi almak için kullanılır. Bu işlev bir sorguyu kabul eder ve [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php) kullanılarak yinelenebilen bir sonuç kümesi döndürür. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
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


<a id="insert-data" class="xliff"></a>

## Veri ekleme
[sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) işleviyle birlikte [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) Transact-SQL deyimini kullanarak SalesLT.Product tablosuna yeni bir ürün eklemek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";
$params = array("BrandNewProduct", "200989", "Blue", 75, 80, "7/1/2016");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) inserted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="update-data" class="xliff"></a>

## Verileri güncelleştirme
[sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) işleviyle birlikte [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) Transact-SQL deyimini kullanarak Azure SQL veritabanınızdaki verileri güncelleştirmek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "UPDATE SalesLT.Product SET ListPrice =? WHERE Name = ?";
$params = array(50,"BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) updated" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="delete-data" class="xliff"></a>

## Verileri silme
[sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) işleviyle birlikte [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL deyimini kullanarak daha önce eklediğiniz yeni ürünü silmek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```PHP
<?php
$serverName = "your_server.database.windows.net";
$connectionOptions = array(
    "Database" => "your_database",
    "Uid" => "your_username",
    "PWD" => "your_password"
);
//Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
$tsql= "DELETE FROM SalesLT.Product WHERE Name = ?";
$params = array("BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) deleted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
```

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft PHP Sürücüleri](https://github.com/Microsoft/msphpsql/)
- [Sorun bildirin/soru sorun](https://github.com/Microsoft/msphpsql/issues)


