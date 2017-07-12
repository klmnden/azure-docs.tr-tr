---
title: "Python kullanarak Azure SQL Veritabanı&quot;na bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanı&quot;na bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir Python kod örneği sunar."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 99195b43a1577f978562864bac5fa12cdeb95d63
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017

---
<a id="azure-sql-database-use-python-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Veritabanı: Python kullanarak verileri bağlama ve sorgulama

 Bu hızlı başlangıç kılavuzunda, Azure SQL veritabanına bağlanmak için [Python](https://python.org)'ı kullanma ve ardından Transact-SQL deyimlerini kullanarak macOS, Ubuntu, Linux ve Windows platformlarındaki veritabanı verilerini sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanmıştır.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

<a id="install-the-python-and-database-communication-libraries" class="xliff"></a>

## Python ve veritabanı iletişim kitaplıklarını yükleme

Bu bölümdeki adımlarda, Python kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve Azure SQL Veritabanı ile çalışmaya yeni başladığınız varsayılır. Python ile geliştirmeye yeni başladıysanız [SQL Server kullanarak uygulama oluşturma](https://www.microsoft.com/sql-server/developer-get-started/) bölümüne gidin, **Python**'ı ve ardından işletim sisteminizi seçin.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Terminalinizi açın ve Python betiğinizi oluşturmayı planladığınız dizine gidin. **brew**, **Mac için Microsoft ODBC Sürücüsü** ve **pyodbc** araçlarını yüklemek için aşağıdaki komutları girin. pyodbc, SQL Veritabanlarına bağlanmak için Linux'ta Microsoft ODBC Sürücüsünü kullanır.

``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql
sudo pip install pyodbc==3.1.1
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Terminalinizi açın ve Python betiğinizi oluşturmayı planladığınız dizine gidin. **Linux için Microsoft ODBC** ve **pyodbc** araçlarını yüklemek için aşağıdaki komutları girin. pyodbc, SQL Veritabanlarına bağlanmak için Linux'ta Microsoft ODBC Sürücüsünü kullanır.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev
sudo pip install pyodbc==3.1.1
```

<a id="windows" class="xliff"></a>

### **Windows**
[Microsoft ODBC Sürücüsü 13.1](https://www.microsoft.com/download/details.aspx?id=53339) sürümünü yükleyin. (İstenirse sürücüyü yükseltin.) pyodbc, SQL Veritabanlarına bağlanmak için Linux'ta Microsoft ODBC sürücüsünü kullanır. 

Ardından PIP kullanarak **pyodbc** aracını yükleyin.

```cmd
pip install pyodbc==3.1.1
```

PIP kullanımını etkinleştirme yönergeleri [burada](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows) bulunabilir

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Sunucunuzun oturum açma bilgilerini unuttuysanız SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.     
   
<a id="select-data" class="xliff"></a>

## Verileri seçme

[pyodbc.connect](https://github.com/mkleehammer/pyodbc/wiki) işleviyle birlikte [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) Transact-SQL deyimi kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın. [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) işlevi, SQL Veritabanı'na yönelik bir sorguya ilişkin sonuç kümesi almak için kullanılır. Bu işlev bir sorguyu kabul eder ve **cursor.fetchone()** kullanılarak yinelenebilen bir sonuç kümesi döndürür. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print str(row[0]) + " " + str(row[1])
    row = cursor.fetchone()
```

<a id="insert-data" class="xliff"></a>

## Veri ekleme
[cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) işleviyle birlikte [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) Transact-SQL deyimini kullanarak SalesLT.Product tablosuna yeni bir ürün eklemek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

<a id="update-data" class="xliff"></a>

## Verileri güncelleştirme
[cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) işleviyle birlikte [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) Transact-SQL deyimini kullanarak daha önce eklediğiniz yeni ürünü silmek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```

<a id="delete-data" class="xliff"></a>

## Verileri silme
[cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) işleviyle birlikte [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL deyimini kullanarak daha önce eklediğiniz yeni ürünü silmek için aşağıdaki kodu kullanın. server, database, username ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python Sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)


