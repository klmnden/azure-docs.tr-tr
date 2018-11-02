---
title: Coğrafi olarak dağıtılan Azure SQL Veritabanı çözümü uygulama | Microsoft Docs
description: Çoğaltılan bir veritabanına yük devretme için Azure SQL Veritabanınızı ve uygulamanızı yapılandırmayı ve test yük devretme işlemi gerçekleştirmeyi öğreneceksiniz.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 2508d43e876a7e463d68eed1b1ca93ddf0d1e9d1
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913353"
---
# <a name="tutorial-implement-a-geo-distributed-database"></a>Öğretici: coğrafi olarak dağıtılmış bir veritabanı uygulama

Bu öğreticide, uzak bir bölgeye yük devretme için Azure SQL veritabanı ve uygulamasını yapılandırır ve sonra yük devretme planınızı test edersiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

> [!div class="checklist"]
> * Veritabanı kullanıcıları oluşturma ve bu kullanıcılara izinler verme
> * Veritabanı düzeyinde güvenlik duvarı kuralı ayarlama
> * [Coğrafi çoğaltma yük devretme grubu](sql-database-geo-replication-overview.md) oluşturma
> * Azure SQL veritabanını sorgulamak için Java uygulaması oluşturma ve derleme
> * Olağanüstü durum kurtarma tatbikatı gerçekleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs) yüklenmiş olmalıdır. 
- Bir Azure SQL veritabanı yüklenmiş olmalıdır. Bu öğreticide, şu hızlı başlangıçlardan birinde yer alan **mySampleDatabase** adıyla AdventureWorksLT örnek veritabanı kullanılmaktadır:

   - [DB Oluşturma - Portal](sql-database-get-started-portal.md)
   - [DB oluşturma - CLI](sql-database-cli-samples.md)
   - [DB Oluşturma - PowerShell](sql-database-powershell-samples.md)

- SQL betiklerini veritabanına karşı yürütme yöntemini belirledikten sonra aşağıdaki sorgu araçlarından birini kullanabilirsiniz:
   - [Azure portalındaki](https://portal.azure.com) sorgu düzenleyici. Azure portalındaki sorgu düzenleyiciyi kullanma hakkında daha fazla bilgi için bkz. [Sorgu Düzenleyiciyi kullanarak bağlanma ve sorgulama](sql-database-get-started-portal.md#query-the-sql-database).
   - Microsoft Windows için SQL Server’dan SQL Veritabanı’na tüm SQL altyapılarını yönetebileceğiniz tümleşik bir ortam olan [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)’nun en yeni sürümü.
   - Linux, macOS ve Windows’a yönelik bir grafik kod düzenleyicisi olan ve Microsoft SQL Server, Azure SQL Veritabanı ve SQL Veri Ambarı’nı sorgulamak için kullanılabilen [mssql uzantısı](https://aka.ms/mssql-marketplace) gibi uzantıları destekleyen [Visual Studio Code](https://code.visualstudio.com/docs)’un en yeni sürümü. Azure SQL Veritabanı ile bu aracı kullanma hakkında daha fazla bilgi için bkz. [VS Code ile bağlanma ve sorgulama](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Veritabanı kullanıcıları oluşturma ve izinler verme

Aşağıdaki sorgu araçlarından birini kullanarak veritabanınıza bağlanın ve kullanıcı hesapları oluşturun:

- Azure portalındaki Sorgu düzenleyici
- SQL Server Management Studio
- Visual Studio Code

Bu kullanıcı hesapları otomatik olarak ikincil sunucunuza çoğaltılır (ve eşitlenmiş şekilde tutulur). SQL Server Management Studio veya Visual Studio Code’u kullanmak için, henüz güvenlik duvarı yapılandırmadığınız bir IP adresindeki istemciden bağlanmanız durumunda bir güvenlik duvarı kuralı yapılandırmanız gerekebilir. Ayrıntılı adımlar için bkz. [Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma](sql-database-get-started-portal-firewall.md).

- Sorgu penceresinde aşağıdaki sorguyu yürüterek veritabanınızda iki kullanıcı hesabı oluşturun. Bu betik, **app_admin** hesabına **db_owner** izinleri verir ve **app_user** hesabına da **SELECT** ve **UPDATE** izinleri verir. 

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

SQL veritabanınız için [veritabanı düzeyinde güvenlik duvarı kuralı](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) oluşturun. Bu veritabanı düzeyinde güvenlik duvarı kuralı, bu öğreticide oluşturduğunuz ikincil sunucuya otomatik olarak çoğaltma yapar. (Bu öğreticide) kolaylık sağlamak için, bu öğreticideki adımları gerçekleştirdiğiniz bilgisayarın genel IP adresini kullanın. Geçerli bilgisayarınıza yönelik sunucu düzeyinde güvenlik duvarı kuralı için kullanılan IP adresini belirlemek için bkz. [Sunucu düzeyinde güvenlik duvarı oluşturma](sql-database-get-started-portal-firewall.md).  

- Açık sorgu pencerenizde, IP adreslerini ortamınız için uygun IP adresleriyle değiştirerek önceki sorguyu aşağıdaki sorguyla değiştirin.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Etkin coğrafi çoğaltma otomatik yük devretme grubu oluşturma 

Azure PowerShell kullanarak, mevcut Azure SQL sunucunuz ile bir Azure bölgesindeki yeni boş Azure SQL sunucusu arasında [etkin coğrafi çoğaltma otomatik yük devretme grubu](sql-database-geo-replication-overview.md) oluşturun ve sonra örnek veritabanınızı yük devretme grubuna ekleyin.

> [!IMPORTANT]
> Bu cmdlet’ler için Azure PowerShell 4.0 gerekir. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Mevcut sunucunuz ve örnek veritabanı için değerleri kullanarak PowerShell betikleriniz için değişkenleri doldurun ve yük devretme grubu adı için genel olarak benzersiz bir değer sağlayın.

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

2. Yük devretme bölgenizde boş bir yedekleme sunucusu oluşturun.

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

4. Veritabanınızı yük devretme grubuna ekleyin.

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

Java ve Maven ortamını yükleme ve yapılandırma konusunda ayrıntılı yönergeler için [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/sql-server/developer-get-started/) bölümüne gidin, **Java**’yı, **MacOS**’yi seçin ve sonra adım 1.2 ve 1.3’teki Java ve Maven’i yapılandırmaya yönelik ayrıntılı yönergeleri izleyin.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Terminalinizi açın ve Java projenizi oluşturmayı planladığınız dizine gidin. Aşağıdaki komutları girerek **Maven** aracını yükleyin:

```bash
sudo apt-get install maven
```

Java ve Maven ortamını yükleme ve yapılandırma konusunda ayrıntılı yönergeler için [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/sql-server/developer-get-started/) bölümüne gidin, **Java**’yı, **Ubuntu**’yu seçin ve sonra adım 1.2, 1.3 ve 1.4’teki Java ve Maven’i yapılandırmaya yönelik ayrıntılı yönergeleri izleyin.

### <a name="windows"></a>**Windows**
Resmi yükleyiciyi kullanarak [Maven](https://maven.apache.org/download.cgi) aracını yükleyin. Bağımlılıkları yönetmede, Java projenizi derlemede, test etmede ve çalıştırmada yardımcı olması için Maven'i kullanın. Java ve Maven ortamını yükleme ve yapılandırma konusunda ayrıntılı yönergeler için [SQL Server kullanarak uygulama derleme](https://www.microsoft.com/sql-server/developer-get-started/) bölümüne gidin, **Java**’yı, Windows’u seçin ve sonra adım 1.2 ve 1.3’teki Java ve Maven’i yapılandırmaya yönelik ayrıntılı yönergeleri izleyin.

## <a name="create-sqldbsample-project"></a>SqlDbSample projesi oluşturma

1. Komut konsolunda (örn. Bash) bir Maven projesi oluşturun. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. **Y** yazın ve **Enter** tuşuna basın.
3. Yeni oluşturulan projenizin dizinlerini değiştirin.

   ```bash
   cd SqlDbSamples
   ```

4. Tercih ettiğiniz düzenleyiciyi kullanarak proje klasörünüzden pom.xml dosyasını açın. 

5. Tercih ettiğiniz metin düzenleyiciyi açıp aşağıdaki satırları pom.xml dosyanıza kopyalayıp yapıştırarak Maven projenize SQL Server için Microsoft JDBC Sürücüsü bağımlılığını ekleyin. Dosyada önceden doldurulmuş olan mevcut değerlerin üzerine yazmayın. JDBC bağımlılığı, daha büyük “bağımlılıklar” bölümünün ( ) içine yapıştırılmalıdır.   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Aşağıdaki “özellikler” bölümünü, pom.xml dosyasında "bağımlılıklar" bölümünün sonrasına ekleyerek projenin derleneceği Java sürümünü belirtin. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Jar’lar içinde bildirim dosyalarını desteklemek için pom.xml dosyasında "özellikler" bölümünün sonrasına şu "derleme" bölümünü ekleyin.       

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
9. App.java dosyasını (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) açın ve içerikleri aşağıdaki içeriklerle değiştirin. Yük devretme grubu adını, yük devretme grubunuzun adıyla değiştirin. Veritabanı adı, kullanıcı veya parola değerlerini değiştirdiyseniz, bu değerleri de değiştirin.

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

## <a name="compile-and-run-the-sqldbsample-project"></a>SqlDbSample projesini derleme ve çalıştırma

1. Komut konsolunda aşağıdaki komutu yürütün.

   ```bash
   mvn package
   ```
2. İşiniz bittiğinde, uygulamayı çalıştırmak için aşağıdaki komutu yürütün (kendiniz durdurmazsanız yaklaşık 1 saat boyunca çalıştırılır):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Olağanüstü durum kurtarma tatbikatı gerçekleştirme

1. Yük devretme grubunun el ile yük devretmesini çağırın. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Yük devretme sırasında uygulama sonuçlarını gözlemleyin. DNS önbelleği yenilenirken bazı eklemeler başarısız olur.     

3. Olağanüstü durum kurtarma sunucunuzun hangi rolü gerçekleştirdiğini öğrenin.

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

5. Yeniden çalışma sırasında uygulama sonuçlarını gözlemleyin. DNS önbelleği yenilenirken bazı eklemeler başarısız olur.     

6. Olağanüstü durum kurtarma sunucunuzun hangi rolü gerçekleştirdiğini öğrenin.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uzak bir bölgeye yük devretme için Azure SQL veritabanı ve uygulamasını yapılandırmayı ve sonra yük devretme planınızı test etmeyi öğrenirsiniz.  Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veritabanı kullanıcıları oluşturma ve bu kullanıcılara izinler verme
> * Veritabanı düzeyinde güvenlik duvarı kuralı ayarlama
> * Coğrafi çoğaltma yük devretme grubu oluşturma
> * Azure SQL veritabanını sorgulamak için Java uygulaması oluşturma ve derleme
> * Olağanüstü durum kurtarma tatbikatı gerçekleştirme

Azure SQL veritabanı yönetilen DMS kullanarak örneği için SQL Server'ı geçirme için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[DMS kullanarak SQL Server’ı Azure SQL Veritabanı Yönetilen Örneği’ne geçirme](../dms/tutorial-sql-server-to-managed-instance.md)

