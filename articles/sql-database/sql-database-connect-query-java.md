---
title: Azure SQL Veritabanı sorgulamak için Java kullanma | Microsoft Docs
description: Bir Azure SQL veritabanına bağlanan bir program oluşturmak için Java kullanın ve T-SQL deyimlerini kullanarak sorgu işlemi gösterilmektedir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: java
ms.topic: quickstart
author: ajlam
ms.author: andrela
ms.reviewer: v-masebo
manager: craigg
ms.date: 12/01/2018
ms.openlocfilehash: 3a036ac1260923a5030b8b0c3345482346c183fe
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563141"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Java kullanarak Azure SQL veritabanı sorgulama

Bu makalede nasıl yapılacağı açıklanır [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) bir Azure SQL veritabanına bağlanmak için. Ardından, T-SQL deyimleriyle veri kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

- Yüklü işletim sisteminiz için Java ile ilgili yazılım:

  - **MacOS**Homebrew ve Java yükleyin ve ardından Maven'i yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu**, Java, Java Development Kit yükleyin, ardından Maven'i yükleyin. Bkz: [1.2, 1.3 ve 1.4 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows**Java yükleyin ve ardından Maven'i yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-database-connection"></a>Veritabanı bağlantı Al

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]

## <a name="create-the-project"></a>Proje oluşturma

1. Komut İstemi'nden adlı yeni bir Maven projesi oluşturun *sqltest*.

    ```bash
    mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=sqltest" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0" --batch-mode
    ```

1. Klasör olarak değiştirmek *sqltest* açın *pom.xml* sık kullandığınız metin düzenleyicisinde. Ekleme **SQL Server için Microsoft JDBC sürücüsü** için aşağıdaki kodu kullanarak proje bağımlılıklarınızı.

    ```xml
    <dependency>
        <groupId>com.microsoft.sqlserver</groupId>
        <artifactId>mssql-jdbc</artifactId>
        <version>7.0.0.jre8</version>
    </dependency>
    ```

1. Yine *pom.xml* dosyasında, aşağıdaki özellikleri projenize ekleyin. Özellikler bölümünüz yoksa, bağımlılıklardan sonra ekleyebilirsiniz.

   ```xml
   <properties>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. *Pom.xml* dosyasını kaydedin ve kapatın.

## <a name="add-code-to-query-database"></a>Veritabanını sorgula için kod ekleyin

1. Adlı bir dosya zaten olmalıdır *App.java* Maven projenize adresinde yer alan:

   *..\sqltest\src\main\java\com\sqldbsamples\App.java*

1. Dosyayı açıp içeriğini aşağıdaki kodla değiştirin. Ardından, sunucu, veritabanı, kullanıcı ve parola için uygun değerleri ekleyin.

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
            String hostName = "your_server.database.windows.net"; // update me
            String dbName = "your_database"; // update me
            String user = "your_username"; // update me
            String password = "your_password"; // update me
            String url = String.format("jdbc:sqlserver://%s:1433;database=%s;user=%s;password=%s;encrypt=true;"
                + "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
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
                    connection.close();
                }
            }
            catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```

   > [!NOTE]
   > Kod örneği kullanan **AdventureWorksLT** Azure SQL veritabanı örneği.

## <a name="run-the-code"></a>Kodu çalıştırma

1. Komut isteminde, uygulamayı çalıştırın.

    ```bash
    mvn package -DskipTests
    mvn -q exec:java "-Dexec.mainClass=com.sqldbsamples.App"
    ```

1. En çok 20 satırlar döndürülür ve uygulama penceresini kapatın doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)  

- [SQL Server için Microsoft JDBC sürücüsü](https://github.com/microsoft/mssql-jdbc)  

- [Sorun bildirin/soru sorun](https://github.com/microsoft/mssql-jdbc/issues)  
