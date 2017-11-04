---
title: "Coğrafi olarak dağıtılan Azure SQL veritabanı çözümü | Microsoft Docs"
description: "Azure SQL Database ve bir çoğaltılmış veritabanı yük devretme için uygulamaya yapılandırmak ve yük devretme sınamasını öğrenin."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 910be8ff5f9a882c7bb8ae875b8bf5fc74d1fb9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="implement-a-geo-distributed-database"></a>Coğrafi olarak dağıtılan bir veritabanı uygulaması

Bu öğreticide bir Azure SQL veritabanı ve bir uzak bölge için yük devretme için uygulamayı yapılandırma ve yük devretme planınız sınayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Veritabanı kullanıcıları oluşturun ve bunları izinleri
> * Bir veritabanı düzeyinde güvenlik duvarı kuralı ayarlamak
> * Oluşturma bir [coğrafi çoğaltma yük devretme grubu](sql-database-geo-replication-overview.md)
> * Oluşturma ve Azure SQL veritabanını sorgulamak için bir Java uygulaması derleme
> * Bir olağanüstü durum kurtarma ayrıntıya gerçekleştirin

Bir Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.


## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son yüklenen [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Bir Azure SQL veritabanını yüklediniz. Bu öğretici AdventureWorksLT örnek veritabanı adı ile kullanır **mySampleDatabase** Bu hızlı başlangıçlar birinden:

   - [DB Oluşturma - Portal](sql-database-get-started-portal.md)
   - [DB oluşturma - CLI](sql-database-get-started-cli.md)
   - [DB Oluşturma - PowerShell](sql-database-get-started-powershell.md)

- SQL komut dosyaları, veritabanında yürütmek için bir yöntem tanımladınız aşağıdaki sorgu araçları birini kullanabilirsiniz:
   - Sorgu Düzenleyicisi'nde [Azure portal](https://portal.azure.com). Azure portalında sorgu Düzenleyicisi'ni kullanma hakkında daha fazla bilgi için bkz: [bağlanma ve sorgu Düzenleyicisi'ni kullanarak sorgu](sql-database-get-started-portal.md#query-the-sql-database).
   - En yeni sürümünü [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), Windows için Microsoft SQL veritabanı için SQL Server'dan herhangi bir SQL altyapı yönetmek için tümleşik bir ortam olduğu.
   - En yeni sürümünü [Visual Studio Code](https://code.visualstudio.com/docs), Linux, macOS, için bir grafik kod düzenleyicisini olduğu ve Windows destekleyen uzantılar da dahil olmak üzere [mssql uzantısı](https://aka.ms/mssql-marketplace) Microsoft SQL Server'ı sorgulamak için Azure SQL Database ve SQL veri ambarı. Bu aracı kullanarak Azure SQL veritabanı ile ilgili daha fazla bilgi için bkz: [Connect ve VS Code sorguyla](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Veritabanı kullanıcıları oluşturun ve izinleri

Veritabanınıza bağlanmak ve aşağıdaki sorgu araçlarından birini kullanarak kullanıcı hesapları oluşturun:

- Azure portalında sorgu Düzenleyicisi
- SQL Server Management Studio
- Visual Studio Code

Bu kullanıcı hesapları otomatik olarak ikincil sunucunuza çoğaltmak (ve eşitleme tutulması). SQL Server Management Studio veya Visual Studio Code kullanmak için kendisi için henüz bir güvenlik duvarı yapılandırdığınız olmayan bir IP adresinde bir istemciden bağlanıyorsanız, bir güvenlik duvarını yapılandırmanız gerekebilir. Ayrıntılı adımlar için bkz: [sunucu düzeyinde güvenlik duvarı kuralı oluşturma](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- Sorgu penceresinde, veritabanınızdaki iki kullanıcı hesapları oluşturmak için aşağıdaki sorguyu çalıştırın. Bu komut dosyası verir **db_owner** izinleri **app_admin** hesabı ve verir **seçin** ve **güncelleştirme** izinleri**app_user** hesabı. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Veritabanı düzeyinde güvenlik duvarı oluşturma

Oluşturma bir [veritabanı düzeyinde güvenlik duvarı kuralı](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) SQL veritabanınız için. Bu veritabanı düzeyinde güvenlik duvarı kuralı, bu öğreticide oluşturduğunuz ikincil sunucuya otomatik olarak çoğaltır. (Bu öğreticide) kolaylık sağlamak için Bu öğreticide adımları gerçekleştiriyorsanız bilgisayarın ortak IP adresi kullanın. Geçerli bilgisayarınız için sunucu düzeyinde güvenlik duvarı kuralı için kullanılan IP adresi belirlemek için bkz: [sunucu düzeyinde güvenlik duvarı oluşturma](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- Açık sorgu penceresinde, önceki sorgu ortamınız için uygun IP adreslerine sahip IP adreslerini değiştirerek aşağıdaki sorguyla değiştirin.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Aktif coğrafi çoğaltma otomatik yük devretme grubu oluşturma 

Azure PowerShell kullanarak oluşturduğunuz bir [aktif coğrafi çoğaltma otomatik yük devretme grubu](sql-database-geo-replication-overview.md) mevcut Azure SQL server'ınızı ve yeni arasında boş bir Azure bölgesi Azure SQL Server'da ve ardından örnek veritabanınızdaki yük devretme grubuna ekleyin.

> [!IMPORTANT]
> Bu cmdlet'ler Azure PowerShell 4.0 gerektirir. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Var olan sunucu ve örnek veritabanı için değerleri kullanarak, bir PowerShell komut dosyası değişkenleri doldurun ve yük devretme grup adı için bir genel benzersiz değer belirtin.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. Boş bir yedek sunucu yük devretme bölgenizde oluşturun.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. İki sunucu arasında bir yük devretme grubu oluşturun.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Veritabanınız yük devretme grubuna ekleyin.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Java yazılımı yükleme

Bu bölümdeki adımlarda, Java kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve Azure SQL Veritabanı ile çalışmaya yeni başladığınız varsayılır. 

### <a name="mac-os"></a>**Mac OS**
Terminalinizi açın ve Java projenizi oluşturmayı planladığınız dizine gidin. Aşağıdaki komutları girerek **brew** ve **Maven** araçlarını yükleyin: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Yükleme ve Java ve Maven ortam yapılandırma konusunda ayrıntılı yönergeler için Git [SQL Server'ı kullanarak bir uygulama oluşturmak](https://www.microsoft.com/sql-server/developer-get-started/)seçin **Java**seçin **MacOS**ve ardından izleyin Java ve Maven 1.2 ve 1.3 adımda yapılandırmaya yönelik ayrıntılı yönergeler için.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Terminalinizi açın ve Java projenizi oluşturmayı planladığınız dizine gidin. Aşağıdaki komutları girerek **Maven** aracını yükleyin:

```bash
sudo apt-get install maven
```

Yükleme ve Java ve Maven ortam yapılandırma konusunda ayrıntılı yönergeler için Git [SQL Server'ı kullanarak bir uygulama oluşturmak](https://www.microsoft.com/sql-server/developer-get-started/)seçin **Java**seçin **Ubuntu**ve ardından izleyin Java ve Maven 1.2, 1,3 ve 1.4 adımda yapılandırmaya yönelik ayrıntılı yönergeler.

### <a name="windows"></a>**Windows**
Resmi yükleyiciyi kullanarak [Maven](https://maven.apache.org/download.cgi) aracını yükleyin. Maven bağımlılıklarını yönetme, derleme, test ve Java projenizi çalıştırma yardımcı olması için kullanın. Yükleme ve Java ve Maven ortam yapılandırma konusunda ayrıntılı yönergeler için Git [SQL Server'ı kullanarak bir uygulama oluşturmak](https://www.microsoft.com/sql-server/developer-get-started/)seçin **Java**Windows seçin ve ardından ayrıntılı yönergeler için izleyin Java ve Maven 1.2 ve 1.3 adımda yapılandırma.

## <a name="create-sqldbsample-project"></a>SqlDbSample projesi oluşturma

1. (Örneğin, Bash) komut konsolunda bir Maven projesi oluşturun. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Tür **Y** tıklatıp **Enter**.
3. Yeni oluşturulan projenize dizinleri değiştirin.

   ```bash
   cd SqlDbSamples
   ```

4. Tercih ettiğiniz düzenleyicisi kullanarak proje klasöründe pom.xml dosyasını açın. 

5. SQL Server bağımlılığı için Microsoft JDBC sürücüsü, sık kullandığınız metin düzenleyiciyi açmak ve kopyalayıp aşağıdaki satırları pom.xml dosyanıza yapıştırarak Maven projenize ekleyin. Dosyada önceden doldurulmaz var olan değerlerin üzerine yazmaz. JDBC bağımlılık yapıştırılmasına gerekir büyük "bağımlılıkları" bölümüne (') içinde.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Aşağıdaki "Özellikler" bölümü pom.xml dosyasına sonra "bağımlılıkları" bölümünde ekleyerek karşı Projeyi derlemek için Java sürümünü belirtin. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Aşağıdaki "oluşturma" bölümünde Kavanoz içinde bildirim dosyaları desteklemek için "Özellikler" bölümüne pom.xml dosyasına ekleyin.       

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
8. pom.xml dosyasını kaydedin ve kapatın.
9. App.java dosyasını (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) açın ve aşağıdaki içeriğiyle Değiştir. Yük devretme grubu adı, yük devretme grubunun adını değiştirin. Veritabanı adı, kullanıcı veya parola değerlerini değiştirdiyseniz, bu değerleri de değiştirin.

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

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

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
      // Query the data that was previously inserted into the primary database from the geo replicated database
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
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
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
6. App.java dosyasını kaydedin ve kapatın.

## <a name="compile-and-run-the-sqldbsample-project"></a>Derleme ve SqlDbSample projesi çalıştırma

1. Komut konsolunda şu komutu yürütün.

   ```bash
   mvn package
   ```
2. Bitirdikten sonra (el ile durdurun sürece, yaklaşık 1 saat boyunca çalışır) uygulamayı çalıştırmak için aşağıdaki komutu yürütün:

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Olağanüstü durum kurtarma ayrıntıya gerçekleştirin

1. Yük devretme grubu el ile yük devretme çağırın. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Yük devretme sırasında uygulama sonuçlarını inceleyin. DNS Önbellek yenileme sırasında bazı eklemeleri başarısız.     

3. Olağanüstü durum kurtarma sunucunuzun performansının hangi rolünün ölçeğini bulun.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Yeniden çalışma.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Uygulama sonuçları yeniden çalışma sırasında gözlemleyin. DNS Önbellek yenileme sırasında bazı eklemeleri başarısız.     

6. Olağanüstü durum kurtarma sunucunuzun performansının hangi rolünün ölçeğini bulun.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [etkin coğrafi çoğaltma ve yük devretme gruplar](sql-database-geo-replication-overview.md).
