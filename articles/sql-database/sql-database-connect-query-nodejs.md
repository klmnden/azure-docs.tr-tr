---
title: "Node.js Kullanarak Azure SQL Veritabanına Bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanına bağlanmak ve sorgu göndermek için kullanabileceğiniz bir Node.js kod örneği sunar."
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: lbosq
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: a7b1a504c6ae7850e5f986895adcf4d92fd5adc1
ms.contentlocale: tr-tr
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-nodejs-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Veritabanı: Verilere bağlanmak ve verileri sorgulamak için Node.js kullanma

Bu hızlı başlangıçta [Node.js](https://nodejs.org/en/) kullanarak Azure SQL Veritabanına bağlanma; ardından Windows, Ubuntu Linux ve Mac platformları için Transact-SQL deyimleri ile veritabanındaki verileri sorgulama, ekleme, güncelleştirme ve silme işlemleri gösterilmektedir.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

<a id="install-nodejs" class="xliff"></a>

## Node.js yükleme 

Bu bölümdeki adımlar, Node.js ile geliştirme hakkında bilgi sahibi olduğunuzu ve Azure SQL Veritabanı ile yeni çalışmaya başladığınızı varsayar. Node.js ile geliştirmeye yeni başladıysanız [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/en-us/sql-server/developer-get-started/) bölümüne gidip **Node.js**’yi ve ardından işletim sisteminizi seçin.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Mac OS X ve **Node.js**’ye yönelik kullanımı kolay bir paket yöneticisi olan **brew** programını yüklemek için aşağıdaki komutları girin.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
**Node.js**’yi ve Node.js’nin paket yöneticisi olan **npm**’yi yüklemek için aşağıdaki komutları girin.

```bash
sudo apt-get install -y nodejs npm
```

<a id="windows" class="xliff"></a>

### **Windows**
[Node.js indirme sayfasını](https://nodejs.org/en/download/) ziyaret edin ve istediğiniz Windows yükleyici seçeneğini belirleyin.


<a id="install-the-tedious-sql-server-driver-for-nodejs" class="xliff"></a>

## Node.js için Tedious SQL Server sürücüsünü yükleme
Node.js için önerilen sürücü **[tedious](https://github.com/tediousjs/tedious)** sürücüsüdür. Tedious, Microsoft’un herhangi bir platform üzerinde Node.js uygulamaları için katkıda bulunduğu açık kaynaklı bir girişimdir. Bu öğreticide kodunuzu ve yükleyeceğimiz `npm` bağımlılıklarını içeren boş bir dizin gereklidir.

**Tedious** sürücüsünü yüklemek için dizininizde aşağıdaki komutu çalıştırın:

```cmd
npm install tedious
```

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Azure SQL Veritabanı sunucunuzun oturum açma bilgilerini unuttuysanız, SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.
    
<a id="select-data" class="xliff"></a>

## Verileri seçme

Kategoriye göre ilk 20 ürün için Azure SQL veritabanınızı sorgulamak için aşağıdaki kodu kullanın. İlk olarak tedious sürücü kitaplığından sürücünün Connect ve Request sınıflarını içeri aktarın. Daha sonra, yapılandırma nesnesini oluşturup **username**, **password**, **server** ve **database** değişkenlerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. Belirtilen `config` nesnesini kullanarak bir `Connection` nesnesi oluşturun. Bundan sonra, `queryDatabase()` işlevini yürütmek için `connection` nesnesinin `connect` olayına yönelik geri çağırmayı tanımlayın.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}
var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        queryDatabase()
    }
});

function queryDatabase(){
    console.log('Reading rows from the Table...');

    // Read all rows from table
    request = new Request(
        "SELECT TOP 1 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
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

<a id="insert-data-into-the-database" class="xliff"></a>

## Veritabanına veri ekleme
`insertIntoDatabase()` işlevi ile [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) Transact-SQL deyimini kullanarak SalesLT.Product tablosuna yeni bir ürün eklemek için aşağıdaki kodu kullanın. **username**, **password**, **server** ve **database** değişkenlerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        insertIntoDatabase()
    }
});

function insertIntoDatabase(){
    console.log("Inserting a brand new product into database...");
    request = new Request(
        "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) inserted');
        }
    );
    connection.execSql(request);
}
```

<a id="update-data-in-the-database" class="xliff"></a>

## Veritabanındaki verileri güncelleştirme
`updateInDatabase()` işlevi ile [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) Transact-SQL deyimini kullanarak daha önce eklediğiniz yeni ürünü silmek için aşağıdaki kodu kullanın. **username**, **password**, **server** ve **database** değişkenlerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. Bu örnek, önceki örnekte eklenen Ürün adını kullanır.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        updateInDatabase()
    }
});

function updateInDatabase(){
    console.log("Updating the price of the brand new product in database...");
    request = new Request(
        "UPDATE SalesLT.Product SET ListPrice = 50 WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) updated');
        }
    );
    connection.execSql(request);
}
```

<a id="delete-data-from-the-database" class="xliff"></a>

## Veritabanından verileri silme
Verileri veritabanından silmek için aşağıdaki kodu kullanın. **username**, **password**, **server** ve **database** değişkenlerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. Bu kez, `deleteFromDatabase()` işlevinde bir **DELETE deyimi** kullanın. Bu örnek de önceki örnekte eklenen Ürün adını kullanır.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        deleteFromDatabase()
    }
});

function deleteFromDatabase(){
    console.log("Deleting the brand new product in database...");
    request = new Request(
        "DELETE FROM SalesLT.Product WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    connection.execSql(request);
}
```


<a id="next-steps" class="xliff"></a>

## Sonraki Adımlar
- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Node.js Sürücüsü](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- [Visual Studio Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md).



