---
title: Apache Spark okumak ve Azure SQL veritabanına veri yazmak için kullanın | Microsoft Docs
description: Hdınsight Spark kümesi ve veri okuma, yazma bir SQL veritabanına veri ve veri akışı için bir Azure SQL veritabanı arasında bir bağlantı kurmayı öğrenin
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: nitinme
ms.openlocfilehash: 6ef0b1ce589bd19693d45a9e4f579ef260530a40
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>Hdınsight Spark kümesi okumak ve Azure SQL veritabanına veri yazmak için kullanın

Azure hdınsight'ta Apache Spark kümesi ile Azure SQL veritabanına bağlanma ve ardından okuma, yazma ve SQL veritabanına veri akışı öğrenin. Bu makaledeki yönergeleri Jupyter not defteri Scala kod parçacıklarını çalıştırmak için kullanın. Ancak, Scala veya Python bağımsız uygulama oluşturma ve aynı görevleri gerçekleştirin. 

## <a name="prerequisites"></a>Önkoşullar

* **Azure Hdınsight Spark kümesi**.  Bölümündeki yönergeleri izleyin [Hdınsight'ta bir Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md).

* **Azure SQL veritabanı**. Bölümündeki yönergeleri izleyin [bir Azure SQL veritabanı oluşturma](../../sql-database/sql-database-get-started-portal.md). Örnek bir veritabanı oluşturduğunuzdan emin olun **AdventureWorksLT** şeması ve verisi. Ayrıca, sunucudaki SQL veritabanına erişmek, istemcinin IP adresi izin veren sunucu düzeyinde güvenlik duvarı kuralı oluşturduğunuzdan emin olun. Güvenlik duvarı kuralı eklemek için yönergeleri aynı makalesinde mevcut değil. Azure SQL veritabanınızı kez oluşturdunuz, aşağıdaki değerleri elinizin altında tutun emin olun. Bir Spark kümeden veritabanına bağlanmak için gereksinim duyarsınız.

    * Azure SQL veritabanını barındıran sunucu adı
    * Azure SQL veritabanı adı
    * Azure SQL veritabanı yönetici kullanıcı adı / parola

* **SQL Server Management Studio**. Bölümündeki yönergeleri izleyin [SSMS bağlanma ve veri sorgulama için kullanım](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma

Spark kümesi ile ilişkili bir Jupyter not defteri oluşturarak başlayın. Bu makalede kullanılan kod parçacıklarını çalıştırmak için bu dizüstü bilgisayar kullanın. 

1. Gelen [Azure portal](https://portal.azure.com/), kümenizi açın. 

2. Gelen **hızlı bağlantılar** 'yi tıklatın **küme panolar** açmak için **küme panolar** görünümü.  Görmüyorsanız, **hızlı bağlantılar**, tıklatın **genel bakış** dikey sol menüden.

    ![Küme Panosu Spark üzerinde](./media/apache-spark-connect-to-sql-database/hdinsight-cluster-dashboard-on-spark.png "küme Panosu Spark üzerinde") 

3. Tıklatın **Jupyter not defteri**. İstenirse, küme için yönetici kimlik bilgilerini girin.

    ![Jupyter not defteri Spark üzerinde](./media/apache-spark-connect-to-sql-database/hdinsight-jupyter-notebook-on-spark.png "Spark üzerinde Jupyter not defteri")
   
   > [!NOTE]
   > Tarayıcınızda aşağıdaki URL'yi açarak Spark kümesinde Jupyter Not Defteri de erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

4. Sağ üst köşesinden Jupyter not defteri tıklatın **yeni**ve ardından **Spark** Scala dizüstü bilgisayar oluşturmak için. Hdınsight Spark kümesinde Jupyter not defterleri de sağlaması **PySpark** Python2 uygulamalar için çekirdek ve **PySpark3** çekirdek Python3 uygulamalar için. Bu makalede, bir Scala not defteri oluşturun.
   
    ![Spark üzerinde Jupyter not defteri için tekrar](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "için Spark Jupyter not defterlerinde çekirdekler")

    Çekirdekler hakkında daha fazla bilgi için bkz. [HDInsight’ta Apache Spark kümeleri ile Jupyter not defterleri kullanma](apache-spark-jupyter-notebook-kernels.md).

   > [!NOTE]
   > Bu makalede, Spark akış verilerinden SQL veritabanına yalnızca Scala ve Java şu anda desteklenmediği için Spark (Scala) çekirdek kullanırız. Okuma ve yazma SQL'e yapılabilir olsa da bu makalede tutarlılık için Python kullanarak Scala üç tüm işlemler için kullanırız.
   >

5. Bu varsayılan adıyla yeni bir not defteri açar **adsız**. Dizüstü bilgisayar adına tıklayın ve tercih ettiğiniz bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Not defteri adını belirtme")

Uygulamanızı oluşturma şimdi başlayabilirsiniz.
    
## <a name="read-data-from-azure-sql-database"></a>Azure SQL veritabanından verileri okuyamadı

Bu bölümde, bir tablodan veri okuma (örneğin, **SalesLT.Address**) AdventureWorks veritabanında yok.

1. Yeni bir Jupyter Not Defteri, bir kod hücreye aşağıdaki kod parçacığını yapıştırın ve Azure SQL veritabanınızın değerlerle yer tutucu değerlerini değiştirin.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

2. Aşağıdaki kod parçacığında API'ler oluşturur Spark dataframe geçirebilirsiniz bir JDBC URL derlemeler bir `Properties` parametreleri tutacak nesne. Bir kod hücresini ve tuşuna parçacığını yapıştırın **SHIFT + ENTER** çalıştırmak için.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

3. Aşağıdaki kod parçacığını bir dataframe verileriyle, Azure SQL veritabanındaki bir tablo oluşturur. Kullandığımız bu parçacığında, bir **SalesLT.Address** olarak kullanılabilir tablo parçası **AdventureWorksLT** veritabanı. Bir kod hücresini ve tuşuna parçacığını yapıştırın **SHIFT + ENTER** çalıştırmak için.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

4. Şimdi veri şeması alma gibi dataframe işlemlerini de gerçekleştirebilirsiniz:

       sqlTableDF.printSchema
   
    Aşağıdakine benzer bir çıktı görürsünüz:

    ![Not defteri adını belirtme](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "Not defteri adını belirtme")

5. İlk 10 satır alma gibi işlemleri de gerçekleştirebilirsiniz.

       sqlTableDF.show(10)

6. Veya belirli sütunlardaki kümesinden alınamıyor.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Azure SQL veritabanına veri yazma

Bu bölümde, örnek bir CSV dosyası kullanılabilir küme üzerinde Azure SQL veritabanında bir tablo oluşturmak ve verilerle doldurmak için kullanırız. Örnek CSV dosyası (**HVAC.csv**) tüm Hdınsight kümeleri üzerinde kullanılabilir `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Yeni bir Jupyter Not Defteri, bir kod hücreye aşağıdaki kod parçacığını yapıştırın ve Azure SQL veritabanınızın değerlerle yer tutucu değerlerini değiştirin.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

2. Aşağıdaki kod parçacığında API'ler oluşturur Spark dataframe geçirebilirsiniz bir JDBC URL derlemeler bir `Properties` parametreleri tutacak nesne. Bir kod hücresini ve tuşuna parçacığını yapıştırın **SHIFT + ENTER** çalıştırmak için.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

3. Aşağıdaki kod parçacığında HVAC.csv verilerde şeması ayıklar ve verileri bir dataframe CSV'ye yüklemek üzere şemasını kullanan `readDf`. Bir kod hücresini ve tuşuna parçacığını yapıştırın **SHIFT + ENTER** çalıştırmak için.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

4. Kullanım `readDf` geçici bir tablo oluşturmak için dataframe `temphvactable`. Bir hive tablosu oluşturmak için geçici tablo kullanın `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

5. Son olarak, hive tablosu Azure SQL veritabanında bir tablo oluşturmak için kullanın. Aşağıdaki kod parçacığında oluşturur `hvactable` Azure SQL veritabanında.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

6. SSMS kullanarak Azure SQL veritabanına bağlama ve gördüğünüzü doğrulayın bir `dbo.hvactable` vardır.

    a. SSMS başlatın ve aşağıdaki ekran görüntüsünde gösterildiği gibi bağlantı ayrıntılarını sağlayarak Azure SQL veritabanına bağlanın.

    ![SSMS kullanarak SQL veritabanına bağlanma](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "SSMS kullanarak SQL veritabanına bağlan")

    b. Nesne Gezgini'nde, Azure SQL database ve görmek için Tablo düğümü genişletin **dbo.hvactable** oluşturuldu.

    ![SSMS kullanarak SQL veritabanına bağlanma](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "SSMS kullanarak SQL veritabanına bağlan")

## <a name="stream-data-into-azure-sql-database"></a>Azure SQL veritabanına veri akışı

Bu bölümde, biz halinde veri akışı **hvactable** zaten Azure SQL veritabanı önceki bölümde oluşturduğunuz.

1. İlk adım olarak, hiç kayıt olduğundan emin olun **hvactable**. SSMS kullanarak, tablo üzerinde aşağıdaki sorguyu çalıştırın.

       DELETE FROM [dbo].[hvactable]

2. Hdınsight Spark kümesinde yeni bir Jupyter not defteri oluşturun. Bir kod hücreye aşağıdaki kod parçacığını yapıştırın ve sonra basın **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

3. Biz veri akışı **HVAC.csv** hvactable içine. Konumundaki küme üzerinde HVAC.csv dosya kullanılabilir */HdiSamples/HdiSamples/SensorSampleData/HVAC/*. Aşağıdaki kod parçacığında, biz öncelikle veri şeması ve akışını alın. Ardından, o Şeması'nı kullanarak bir akış dataframe oluşturun. Bir kod hücresini ve tuşuna parçacığını yapıştırın **SHIFT + ENTER** çalıştırmak için.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema1).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

4. Çıktı şeması gösterir **HVAC.csv**. **Hvactable** de aynı şeması vardır. Çıkış tablosundaki sütunlar listeler.

    ![Tablonun şeması](./media/apache-spark-connect-to-sql-database/schema-of-table.png "tablonun şeması")

5. Son olarak, aşağıdaki kod parçacığında HVAC.csv veri okumak ve içine akış kullanmasını **hvactable** Azure SQL veritabanında. Kod parçacığını bir kod hücreye yapıştırın, Azure SQL veritabanınızın değerlerle yer tutucu değerlerini değiştirin ve tuşuna **SHIFT + ENTER** çalıştırmak için.

       val WriteToSQLQuery  = readStreamDf.writeStream.foreach(new ForeachWriter[Row] {
          var connection:java.sql.Connection = _
          var statement:java.sql.Statement = _
          
          val jdbcUsername = "<SQL DB ADMIN USER>"
          val jdbcPassword = "<SQL DB ADMIN PWD>"
          val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
          val jdbcPort = 1433
          val jdbcDatabase ="<AZURE SQL DB NAME>"
          val driver = "com.microsoft.sqlserver.jdbc.SQLServerDriver"
          val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
  
         def open(partitionId: Long, version: Long):Boolean = {
           Class.forName(driver)
           connection = DriverManager.getConnection(jdbc_url, jdbcUsername, jdbcPassword)
           statement = connection.createStatement
           true
         }
  
         def process(value: Row): Unit = {
           val Date  = value(0)
           val Time = value(1)
           val TargetTemp = value(2)
           val ActualTemp = value(3)
           val System = value(4)
           val SystemAge = value(5)
           val BuildingID = value(6)  
    
           val valueStr = "'" + Date + "'," + "'" + Time + "'," + "'" + TargetTemp + "'," + "'" + ActualTemp + "'," + "'" + System + "'," + "'" + SystemAge + "'," + "'" + BuildingID + "'"
           statement.execute("INSERT INTO " + "dbo.hvactable" + " VALUES (" + valueStr + ")")   
           }

         def close(errorOrNull: Throwable): Unit = {
            connection.close
          }
         })
        
         var streamingQuery = WriteToSQLQuery.start()

6. Verilerin içine akıtılan doğrulayın **hvactable** SQL Server Management Studio (SSMS) aşağıdaki sorguyu çalıştırarak. Her sorguyu çalıştırmak, artan tablo satır sayısını gösterir.

        SELECT COUNT(*) FROM hvactable

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Store'da verileri çözümlemek üzere Hdınsight Spark kümesi kullanın](apache-spark-use-with-data-lake-store.md)
* [EventHub kullanarak, işlem, yapılandırılmış akış olayları](apache-spark-eventhub-structured-streaming.md)
* [Hdınsight üzerinde Kafka ile akış yapılandırılmış Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
