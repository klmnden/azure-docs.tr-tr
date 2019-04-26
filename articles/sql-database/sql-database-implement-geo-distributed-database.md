---
title: Coğrafi olarak dağıtılmış bir Azure SQL veritabanı çözümü uygulama | Microsoft Docs
description: Azure SQL veritabanı ve çoğaltılan bir veritabanına yük devretme için uygulamanızın yapılandırma ve yük devretme testi hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 6022c016b83ffe1362db4d826a5ee4397afd4128
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60338998"
---
# <a name="tutorial-implement-a-geo-distributed-database"></a>Öğretici: Coğrafi olarak dağıtılmış bir veritabanı uygulama

Bir Azure SQL veritabanı ve uzak bir bölgeye yük devretme için uygulama yapılandırma ve test yük devretme planı. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Oluşturma bir [yük devretme grubu](sql-database-auto-failover-group.md)
> - Bir Azure SQL veritabanını sorgulamak için Java uygulaması çalıştırma
> - Yük devretme testi

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

Bu öğreticiyi tamamlamak için aşağıdaki öğeler yüklediğiniz emin olun:

- [Azure PowerShell](/powershell/azureps-cmdlets-docs)
- Bir Azure SQL veritabanı. Bir kullanım oluşturmak için
  - [Portal](sql-database-single-database-get-started.md)
  - [CLI](sql-database-cli-samples.md)
  - [PowerShell](sql-database-powershell-samples.md)

  > [!NOTE]
  > Öğreticide *AdventureWorksLT* örnek veritabanı.

- Bkz: Java ve Maven [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/sql-server/developer-get-started/), vurgulayın **Java** ve ortamınızı seçin ve ardından adımları izleyin.

> [!IMPORTANT]
> Bu öğreticideki adımları gerçekleştirdiğiniz bilgisayarın genel IP adresini kullanmak için güvenlik duvarı kurallarını ayarladığınızdan emin olun. Veritabanı düzeyinde güvenlik duvarı kuralları ikincil sunucuya otomatik olarak çoğaltır.
>
> Bilgi için [veritabanı düzeyinde güvenlik duvarı kuralı oluşturma](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) veya belirlemek için IP adresini bilgisayarınız için sunucu düzeyinde güvenlik duvarı kuralı için kullanılan bakın [sunucu düzeyinde güvenlik duvarı oluşturma](sql-database-server-level-firewall-rule.md).  

## <a name="create-a-failover-group"></a>Bir yük devretme grubu oluşturma

Azure PowerShell kullanarak oluşturma [yük devretme grupları](sql-database-auto-failover-group.md) mevcut bir Azure SQL server ve başka bir bölgede yeni bir Azure SQL sunucusu arasında. Ardından örnek veritabanını yük devretme grubuna ekleyin.

> [!IMPORTANT]
> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]

Bir yük devretme grubu oluşturmak için aşağıdaki betiği çalıştırın:

   ```powershell
    # Set variables for your server and database
    $adminlogin = "<your admin>"
    $password = "<your password>"
    $myresourcegroupname = "<your resource group name>"
    $mylocation = "<your resource group location>"
    $myservername = "<your existing server name>"
    $mydatabasename = "<your database name>"
    $mydrlocation = "<your disaster recovery location>"
    $mydrservername = "<your disaster recovery server name>"
    $myfailovergroupname = "<your globally unique failover group name>"

    # Create a backup server in the failover region
    New-AzSqlServer -ResourceGroupName $myresourcegroupname `
       -ServerName $mydrservername `
       -Location $mydrlocation `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
          -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

    # Create a failover group between the servers
    New-AzSqlDatabaseFailoverGroup `
       –ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -PartnerServerName $mydrservername  `
       –FailoverGroupName $myfailovergroupname `
       –FailoverPolicy Automatic `
       -GracePeriodWithDataLossHours 2

    # Add the database to the failover group
    Get-AzSqlDatabase `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -DatabaseName $mydatabasename | `
     Add-AzSqlDatabaseToFailoverGroup `
       -ResourceGroupName $myresourcegroupname `
       -ServerName $myservername `
       -FailoverGroupName $myfailovergroupname
   ```

Coğrafi çoğaltma ayarları da değiştirilebilir Azure portalında veritabanınızın ardından seçerek **ayarları** > **coğrafi çoğaltma**.

![Coğrafi çoğaltma ayarları](./media/sql-database-implement-geo-distributed-database/geo-replication.png)

## <a name="run-the-sample-project"></a>Örnek projeyi Çalıştır

1. Konsolunda, aşağıdaki komutla bir Maven projesi oluşturun:

   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```

1. Tür **Y** basın **Enter**.

1. Yeni Proje dizinleri.

   ```bash
   cd SqlDbSample
   ```

1. Tercih ettiğiniz düzenleyiciyi kullanarak açın *pom.xml* proje klasörünüzdeki dosya.

1. SQL Server bağımlılık için Microsoft JDBC sürücüsü aşağıdakileri ekleyerek `dependency` bölümü. Bağımlılık büyük içinde yapıştırılan gerekir `dependencies` bölümü.

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

1. Ekleyerek Java sürümü belirtmek `properties` sonra bölüm `dependencies` bölümü:

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```

1. Bildirim dosyaları ekleyerek destekler `build` sonra bölüm `properties` bölümü:

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```

1. Kaydet ve Kapat *pom.xml* dosya.

1. Açık *App.java* bulunan dosya... \SqlDbSample\src\main\java\com\sqldbsamples ve içeriğini aşağıdaki kodla değiştirin:

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "<your failover group name>";  // add failover group name
  
      private static final String DB_NAME = "<your database>";  // add database name
      private static final String USER = "<your admin>";  // add database user
      private static final String PASSWORD = "<your password>";  // add database password

      private static final String READ_WRITE_URL = String.format("jdbc:" +
         "sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:" +
         "sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;" +
         "hostNameInCertificate=*.database.windows.net;loginTimeout=30;", +
         FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println("");

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " +
                   (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " +
                   (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name so we can find the product again
      String sql = "INSERT INTO SalesLT.Product " +
         "(Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL);
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id stored in the table to be able to make unique inserts
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL);
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```

1. Kaydet ve Kapat *App.java* dosya.

1. Komut konsolunda aşağıdaki komutu çalıştırın:

   ```bash
   mvn package
   ```

1. El ile yük devretme testi çalıştırmak için size zaman izin vererek durdurulana kadar yaklaşık 1 saat boyunca çalışacak uygulamayı başlatın.

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```

   ```output
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ...
   ```

## <a name="test-failover"></a>Yük devretme testi

Bir yük devretme benzetimini gerçekleştirmek ve uygulama sonuçlarını gözlemleyin için aşağıdaki komut dosyasını çalıştırın. Veritabanı geçiş sırasında nasıl bazı ekler ve seçer bildirimi başarısız olur.

Aşağıdaki komutla test sırasında olağanüstü durum kurtarma sunucusu rolünü de göz atabilirsiniz:

   ```powershell
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername).ReplicationRole
   ```

Bir yük devretme testi için:

1. Yük devretme grubunun el ile bir yük devretme başlatın:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -FailoverGroupName $myfailovergroupname
   ```

1. Yük devretme grubu için birincil sunucuya geri dönmek:

   ```powershell
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure SQL veritabanı ve uygulamaya uzak bir bölgeye yük devretme için yapılandırılmış ve bir yük devretme planı test. Şunları öğrendiniz:

> [!div class="checklist"]
> - Coğrafi çoğaltma yük devretme grubu oluşturma
> - Bir Azure SQL veritabanını sorgulamak için Java uygulaması çalıştırma
> - Yük devretme testi

DMS kullanarak geçirme hakkında bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [DMS kullanarak Azure SQL veritabanı yönetilen örneği için SQL Server'ı geçirme](../dms/tutorial-sql-server-to-managed-instance.md)
