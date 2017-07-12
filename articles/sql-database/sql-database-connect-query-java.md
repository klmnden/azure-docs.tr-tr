---
title: "Java kullanarak Azure SQL Veritabanı'na bağlanma | Microsoft Docs"
description: "Azure SQL Veritabanı'na bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir Java kod örneği sunar."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/23/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 0df7bd56d3aac462a86e31b5512e48371918bbf2
ms.contentlocale: tr-tr
ms.lasthandoff: 07/04/2017


---
<a id="azure-sql-database-use-java-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Veritabanı: Java kullanarak verileri bağlama ve sorgulama

Bu hızlı başlangıç kılavuzunda, Azure SQL veritabanına bağlanmak için [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server)'yı kullanma ve ardından Transact-SQL deyimlerini kullanarak macOS, Ubuntu, Linux ve Windows platformlarındaki veritabanı verilerini sorgulama, ekleme, güncelleştirme ve silme işlemlerinin nasıl yapılacağı açıklanmıştır.

<a id="prerequisites" class="xliff"></a>

## Ön koşullar

Bu hızlı başlangıçta başlangıç noktası olarak bu hızlı başlangıçlardan birinde oluşturulan kaynaklar kullanılır:

- [DB Oluşturma - Portal](sql-database-get-started-portal.md)
- [DB oluşturma - CLI](sql-database-get-started-cli.md)
- [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

<a id="install-java-software" class="xliff"></a>

## Java yazılımı yükleme

Bu bölümdeki adımlarda, Java kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve Azure SQL Veritabanı ile çalışmaya yeni başladığınız varsayılır. Java ile geliştirmeye yeni başladıysanız [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/en-us/sql-server/developer-get-started/) konusuna gidin, **Java** dilini ve ardından işletim sisteminizi seçin.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Terminalinizi açın ve Java projenizi oluşturmayı planladığınız dizine gidin. Aşağıdaki komutları girerek **brew** ve **Maven** araçlarını yükleyin: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Terminalinizi açın ve Java projenizi oluşturmayı planladığınız dizine gidin. Aşağıdaki komutları girerek **Maven** aracını yükleyin:

```bash
sudo apt-get install maven
```

<a id="windows" class="xliff"></a>

### **Windows**
Resmi yükleyiciyi kullanarak [Maven](https://maven.apache.org/download.cgi) aracını yükleyin. Bağımlılıkları yönetmede, Java projenizi oluşturmada, test etmede ve çalıştırmada yardımcı olması için Maven'i kullanın. 

<a id="get-connection-information" class="xliff"></a>

## Bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Sonraki yordamlarda tam sunucu adına, veritabanı adına ve oturum açma bilgilerine ihtiyacınız olacaktır.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüden **SQL Veritabanları**’nı seçin ve **SQL veritabanları** sayfasında veritabanınıza tıklayın. 
3. Veritabanınızın **Genel Bakış** sayfasında, aşağıdaki görüntüde gösterildiği gibi tam sunucu adını gözden geçirin. Sunucu adının üzerine gelerek **Kopyalamak için tıklayın** seçeneğini ortaya çıkarabilirsiniz. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Sunucunuzun oturum açma bilgilerini unuttuysanız SQL Veritabanı sunucu sayfasına giderek sunucu yöneticisi adını görüntüleyin ve gerekirse parolayı sıfırlayın.
5. **Veritabanı bağlantı dizelerini göster**’e tıklayın.

6. Tam **JDBC** bağlantı dizesini gözden geçirin.

    ![JDBC bağlantı dizesi](./media/sql-database-connect-query-jdbc/jdbc-connection-string.png)   

<a id="create-maven-project" class="xliff"></a>

### **Maven projesi oluşturma**
Terminalde yeni bir Maven projesi oluşturun. 
```bash
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

***pom.xml*** dosyasındaki bağımlılıklara **SQL Server için Microsoft JDBC Sürücüsü**'nü ekleyin. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.0.jre8</version>
</dependency>
```

<a id="select-data" class="xliff"></a>

## Verileri seçme

[connection](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection) sınıfıyla birlikte [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) Transact-SQL deyimi kullanarak ilk 20 ürünü kategoriye göre sorgulamak için aşağıdaki kodu kullanın. hostName, dbName, user ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                try (Statement statement = connection.createStatement();
                    ResultSet resultSet = statement.executeQuery(selectSql)) {

                        // Print results from select statement
                        System.out.println("Top 20 categories:");
                        while (resultSet.next())
                        {
                            System.out.println(resultSet.getString(1) + " "
                                + resultSet.getString(2));
                        }
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="insert-data" class="xliff"></a>

## Veri ekleme

[Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) sınıfıyla birlikte [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) Transact-SQL deyimini kullanarak SalesLT.Product tablosuna yeni ürün eklemek için aşağıdaki kodu kullanın. hostName, dbName, user ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, " 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                try (PreparedStatement prep = connection.prepareStatement(insertSql)) {
                        prep.setString(1, "BrandNewProduct");
                        prep.setInt(2, 200989);
                        prep.setString(3, "Blue");
                        prep.setDouble(4, 75);
                        prep.setDouble(5, 80);
                        prep.setTimestamp(6, sqlTimeStamp);

                        int count = prep.executeUpdate();
                        System.out.println("Inserted: " + count + " row(s)");
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
<a id="update-data" class="xliff"></a>

## Verileri güncelleştirme

[Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) sınıfıyla birlikte [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü ve Azure SQL veritabanınızdaki verileri güncelleştirmek için aşağıdaki kodu kullanın. hostName, dbName, user ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Update data example:");
                System.out.println("=========================================");

                // Prepared statement to update data
                String updateSql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(updateSql)) {
                        prep.setString(1, "500");
                        prep.setString(2, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Updated: " + count + " row(s)")
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



<a id="delete-data" class="xliff"></a>

## Verileri silme

[Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) sınıfıyla [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) Transact-SQL deyimini kullanarak daha önce eklemiş olduğunuz yeni ürünü silmek için aşağıdaki kodu kullanın. hostName, dbName, user ve password parametrelerini, AdventureWorksLT örnek verileriyle veritabanını oluştururken belirttiğiniz değerlerle değiştirin. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Delete data example:");
                System.out.println("=========================================");

                // Prepared statement to delete data
                String deleteSql = "DELETE SalesLT.Product WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(deleteSql)) {
                        prep.setString(1, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Deleted: " + count + " row(s)");
                }
        }       
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar
- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft JDBC sürücüsü](https://github.com/microsoft/mssql-jdbc)
- [Sorun bildirin/soru sorun](https://github.com/microsoft/mssql-jdbc/issues)


