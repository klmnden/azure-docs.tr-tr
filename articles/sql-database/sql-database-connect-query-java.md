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
ms.date: 03/25/2019
ms.openlocfilehash: 2d9ce34d52d08b4dd38caaadfab48b7a69870e9a
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447914"
---
# <a name="quickstart-use-java-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Java kullanarak Azure SQL veritabanı sorgulama

Bu makalede nasıl yapılacağı açıklanır [Java](/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server) bir Azure SQL veritabanına bağlanmak için. Ardından, T-SQL deyimleriyle veri kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

- Bir Azure SQL veritabanı. Şu hızlı başlangıçlardan biriyle oluşturmak ve ardından bir veritabanını Azure SQL veritabanı'nda yapılandırmak için kullanabilirsiniz:

  || Tek veritabanı | Yönetilen örnek |
  |:--- |:--- |:---|
  | Oluştur| [Portal](sql-database-single-database-get-started.md) | [Portal](sql-database-managed-instance-get-started.md) |
  || [CLI](scripts/sql-database-create-and-configure-database-cli.md) | [CLI](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44) |
  || [PowerShell](scripts/sql-database-create-and-configure-database-powershell.md) | [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md) |
  | Yapılandırma | [sunucu düzeyinde IP güvenlik duvarı kuralı](sql-database-server-level-firewall-rule.md)| [Bir VM bağlantısı](sql-database-managed-instance-configure-vm.md)|
  |||[Şirket içi bağlantısı](sql-database-managed-instance-configure-p2s.md)
  |Veri yükleme|Adventure Works hızlı başlangıç yüklendi|[Wide World Importers geri yükleme](sql-database-managed-instance-get-started-restore.md)
  |||Geri yükleme ya da Adventure Works'den içe [BACPAC](sql-database-import.md) dosya [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works)|
  |||

  > [!IMPORTANT]
  > Komut bu makalede, Adventure Works veritabanı kullanmak için yazılır. Yönetilen örnek sayesinde, Adventure Works veritabanı bir örneği veritabanına aktarmak veya betiklerde Wide World Importers veritabanını kullanmak için bu makaleyi değiştirin.

- Yüklü işletim sisteminiz için Java ile ilgili yazılım:

  - **MacOS**Homebrew ve Java yükleyin ve ardından Maven'i yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/mac/).

  - **Ubuntu**, Java, Java Development Kit yükleyin, ardından Maven'i yükleyin. Bkz: [1.2, 1.3 ve 1.4 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/).

  - **Windows**Java yükleyin ve ardından Maven'i yükleyin. Bkz: [1.2 ve 1.3 adımları](https://www.microsoft.com/sql-server/developer-get-started/java/windows/).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

Azure SQL veritabanına bağlanmak için gereken bağlantı bilgilerini alın. Yaklaşan yordamlar için tam sunucu adını veya ana bilgisayar adı, veritabanı adını ve oturum açma bilgileri gerekir.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. Gidin **SQL veritabanları** veya **SQL yönetilen örnekler** sayfası.

3. Üzerinde **genel bakış** sayfasında, tam sunucu adını gözden **sunucu adı** tek bir veritabanı veya tam sunucu adı yanındaki **konak** yönetilen bir örneği. Sunucu adı veya ana bilgisayar adı kopyalamak için üzerine gelin ve seçin **kopyalama** simgesi. 

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
