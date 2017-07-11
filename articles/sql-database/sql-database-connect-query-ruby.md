---
title: "Ruby kullanarak Azure SQL Veritabanına bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanı’na bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir Ruby kod örneği sunar."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: c5d09cf03c87c8da1d8588be62fea3f0cc3eec4f
ms.contentlocale: tr-tr
ms.lasthandoff: 06/22/2017

---

<a id="azure-sql-database-use-ruby-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Veritabanı: Ruby kullanarak verileri bağlama ve sorgulama

Bu hızlı başlangıç kılavuzunda, Azure SQL veritabanına bağlanmak için [Ruby](https://www.ruby-lang.org)’yi kullanma ve ardından Transact-SQL deyimlerini kullanarak Mac OS ve Ubuntu Linux platformlarındaki veritabanı verilerini sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanmıştır.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

<a id="install-ruby-and-database-communication-libraries" class="xliff"></a>

## Ruby ve veritabanı iletişim kitaplıklarını yükleme

Bu bölümdeki adımlarda, Ruby’yi kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve Azure SQL Veritabanı ile çalışmaya yeni başladığınız varsayılır. Ruby ile geliştirmeye yeni başladıysanız, [SQL Server kullanarak uygulama geliştirme](https://www.microsoft.com/en-us/sql-server/developer-get-started/) konusuna gidin, **Ruby** dilini ve sonra da işletim sisteminizi seçin.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Terminalinizi açın ve Ruby betiğinizi oluşturmayı planladığınız dizine gidin. **brew**, **FreeTDS** ve **TinyTDS**’yi yüklemek için aşağıdaki komutları girin.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install FreeTDS
gem install tiny_tds
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Terminalinizi açın ve Ruby betiğinizi oluşturmayı planladığınız dizine gidin. **FreeTDS** ve **TinyTDS**’yi yüklemek için aşağıdaki komutları girin.

```bash
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.27.tar.gz
tar -xzf freetds-1.00.27.tar.gz
cd freetds-1.00.27
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
gem install tiny_tds
```

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
[SELECT](https://github.com/rails-sqlserver/tiny_tds) Transact-SQL deyimi ile [TinyTDS::Client](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) işlevini kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın. TinyTDS::Client işlevi sorguyu kabul eder ve bir sonuç kümesi döndürür. Sonuç kümesi [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds) kullanılarak yinelenir. Sunucu, veritabanı, kullanıcı adı ve parola parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

<a id="insert-data" class="xliff"></a>

## Veri ekleme
[TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) işlevini [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) Transact-SQL deyimiyle kullanarak SalesLT.Product tablosuna yeni ürün eklemek için aşağıdaki kodu kullanın. Sunucu, veritabanı, kullanıcı adı ve parola parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

Bu örnekte bir INSERT deyimini güvenli şekilde yürütme, uygulamanızı [SQL ekleme](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) güvenlik açığından koruyan parametreler geçirme ve otomatik olarak oluşturulan [Birincil Anahtar](https://docs.microsoft.com/sql/relational-databases/tables/primary-and-foreign-key-constraints) değerini alma işlemi gösterilmektedir.    
  
TinyTDS’yi Azure ile kullanmak için, geçerli oturumun belirli bilgileri nasıl işlediğini değiştiren birkaç `SET` deyimi yürütmeniz gerekir. Önerilen `SET` deyimleri kod örneğinde belirtilir. Örneğin `SET ANSI_NULL_DFLT_ON`, sütunun null atanabilirlik durumu açıkça belirtilmemişse bile null değerlere izin vermek için yeni sütunların oluşturulmasına izin verir.  
  
Microsoft SQL Server [datetime](https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql) biçimi ile uyumlu hale getirmek için, [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) işlevini kullanarak ilgili datetime biçimine dönüştürün.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def insert(name, productnumber, color, standardcost, listprice, sellstartdate)
    tsql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) 
        VALUES (N'#{name}', N'#{productnumber}',N'#{color}',N'#{standardcost}',N'#{listprice}',N'#{sellstartdate}')"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
insert('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')
```

<a id="update-data" class="xliff"></a>

## Verileri güncelleştirme
[UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) Transact-SQL deyimi ile [TinyTDS:Client](https://github.com/rails-sqlserver/tiny_tds) işlevini kullanarak daha önce eklemiş olduğunuz yeni ürünü güncelleştirmek için aşağıdaki kodu kullanın. Sunucu, veritabanı, kullanıcı adı ve parola parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true
    
def update(name, listPrice, client)
    tsql = "UPDATE SalesLT.Product SET ListPrice = N'#{listPrice}' WHERE Name =N'#{name}'";
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
update('BrandNewProduct', 500, client)
```

<a id="delete-data" class="xliff"></a>

## Verileri silme
[DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL deyimi ile [TinyTDS:Client](https://github.com/rails-sqlserver/tiny_tds) işlevini kullanarak daha önce eklemiş olduğunuz yeni ürünü silmek için aşağıdaki kodu kullanın. Sunucu, veritabanı, kullanıcı adı ve parola parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def delete(name, client)
    tsql = "DELETE FROM SalesLT.Product WHERE Name = N'#{name}'"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
delete('BrandNewProduct', client)
```

<a id="next-steps" class="xliff"></a>

## Sonraki Adımlar
- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [TinyTDS için GitHub deposu](https://github.com/rails-sqlserver/tiny_tds)
- [Sorun bildirin/soru sorun](https://github.com/rails-sqlserver/tiny_tds/issues)
- [SQL Server için Ruby Sürücüleri](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)


