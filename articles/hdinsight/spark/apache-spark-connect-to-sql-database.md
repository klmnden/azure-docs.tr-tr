---
title: Okumak ve Azure SQL veritabanına veri yazmak için Apache Spark'ı kullanma
description: HDInsight Spark kümesi ve veri okuma, verileri ve veri akışı, bir SQL veritabanı'na yazmak için bir Azure SQL veritabanı arasında bir bağlantı kurmayı öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/21/2019
ms.openlocfilehash: 3812cf55a26a12ef110b8acf14edd0e8bfd36851
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236525"
---
# <a name="use-hdinsight-spark-cluster-to-read-and-write-data-to-azure-sql-database"></a>HDInsight Spark kümesi okumak ve Azure SQL veritabanına veri yazmak için kullanın

Azure HDInsight, Apache Spark kümesi ile Azure SQL veritabanına bağlanmak ve ardından okuma, yazma ve SQL veritabanı'na veri akışı öğrenin. Kullanım yönergeleri Bu makale bir [Jupyter not defteri](https://jupyter.org/) Scala kod parçacıklarını çalıştırmak. Ancak, Scala veya Python ile tek başına uygulama oluşturabilir ve aynı görevleri.

## <a name="prerequisites"></a>Önkoşullar

* **Azure HDInsight Spark kümesi**.  Konumundaki yönergeleri [HDInsight Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md).

* **Azure SQL veritabanı**. Konumundaki yönergeleri [bir Azure SQL veritabanı oluşturma](../../sql-database/sql-database-get-started-portal.md). Örnek ile bir veritabanı oluşturduğunuzdan emin olun **AdventureWorksLT** şema ve veri. Ayrıca, SQL veritabanı sunucusuna erişmek istemcinizin IP adresine izin vermek için bir sunucu düzeyinde güvenlik duvarı kuralı oluşturduğunuzdan emin olun. Aynı makaledeki yönergeleri güvenlik duvarı kuralı eklemek için kullanılabilir. Azure SQL veritabanınız oluşturulduktan sonra, aşağıdaki değerleri kullanışlı tutmak emin olun. Bir Spark kümelerini veritabanına bağlanmak için ihtiyaç.

    * Azure SQL veritabanını barındıran sunucu adı.
    * Azure SQL veritabanı adı.
    * Azure SQL veritabanı yönetici kullanıcı adı / parola.

* **SQL Server Management Studio**. Konumundaki yönergeleri [bağlanmak ve veri sorgulamak için SSMS kullanma](../../sql-database/sql-database-connect-query-ssms.md).

## <a name="create-a-jupyter-notebook"></a>Jupyter not defteri oluşturma 

Oluşturarak başlayın bir [Jupyter not defteri](https://jupyter.org/) Spark kümesi ile ilişkili. Bu makalede kullanılan kod parçacıklarını çalıştırmak için bu not defteri kullanırsınız. 

1. Gelen [Azure portalında](https://portal.azure.com/), kümenizi açın.
1. Seçin **Jupyter not defteri** altında **küme panoları** işlecin sağ tarafındaki.  Görmüyorsanız **küme panoları**seçin **genel bakış** sol menüden. İstenirse, küme için yönetici kimlik bilgilerini girin.

    ![Spark üzerinde Jupyter notebook](./media/apache-spark-connect-to-sql-database/hdinsight-spark-cluster-dashboard-jupyter-notebook.png "Spark üzerinde Jupyter notebook")
   
   > [!NOTE]  
   > Aşağıdaki URL'yi tarayıcınızda açarak da Spark kümesinde Jupyter not defterine erişebilirsiniz. **CLUSTERNAME** değerini kümenizin adıyla değiştirin:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

1. Sağ üst köşeden Jupyter not defteri tıklayın **yeni**ve ardından **Spark** Scala not defteri oluşturmak için. HDInsight Spark kümesinde Jupyter not defterleri de sağlaması **PySpark** Python2 uygulamalar için çekirdek ve **PySpark3** çekirdek Python3 uygulamalar için. Bu makale için bir Scala not defteri oluştururuz.
   
    ![Spark üzerinde Jupyter notebook için çekirdekler](./media/apache-spark-connect-to-sql-database/kernel-jupyter-notebook-on-spark.png "için Spark üzerinde Jupyter not defteri çekirdekleri")

    Çekirdekler hakkında daha fazla bilgi için bkz. [HDInsight’ta Apache Spark kümeleri ile Jupyter not defterleri kullanma](apache-spark-jupyter-notebook-kernels.md).

   > [!NOTE]  
   > Bu makalede, Spark akış verileri SQL veritabanına yalnızca Scala ve Java şu anda desteklemediği için Spark (Scala) çekirdek kullanırız. Okuma ve yazma SQL'e yapılabilir olsa bile bu makaledeki tutarlılık kullanarak Python, Scala üç tüm işlemler için kullanırız.

1. Bu, varsayılan bir adla yeni bir not defteri açar **adsız**. Not Defteri adına tıklayın ve tercih ettiğiniz bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-connect-to-sql-database/hdinsight-spark-jupyter-notebook-name.png "Not defteri adını belirtme")

Artık uygulamanızı oluşturmaya başlayabilirsiniz.
    
## <a name="read-data-from-azure-sql-database"></a>Azure SQL veritabanından verileri okuyamadı

Bu bölümde, bir tablodan veri okuma (örneğin, **SalesLT.Address**) AdventureWorks veritabanında mevcut.

1. Yeni bir Jupyter Not Defteri, bir kod hücresine aşağıdaki kod parçacığını yapıştırın ve yer tutucu değerlerini Azure SQL veritabanınızın değerleriyle değiştirin.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

1. API'ler oluşturur Spark dataframe geçirebileceğiniz JDBC URL oluşturmak için aşağıdaki kod parçacığında kullanmak bir `Properties` parametreleri tutacak nesne. Kod parçacığını yapıştırın bir kodu hücreyi ve ENTER tuşuna **SHIFT + ENTER** çalıştırılacak.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")         

1. Aşağıdaki kod parçacığında, bir veri çerçevesi ile Azure SQL veritabanınızda bir tablodaki verileri oluşturmak için kullanın. Bu kod parçacığında, kullandığımız bir **SalesLT.Address** olarak kullanılabilir tablo parçası **AdventureWorksLT** veritabanı. Kod parçacığını yapıştırın bir kodu hücreyi ve ENTER tuşuna **SHIFT + ENTER** çalıştırılacak.

       val sqlTableDF = spark.read.jdbc(jdbc_url, "SalesLT.Address", connectionProperties)

1. Artık veri şemasını alma gibi dataframe işlemleri gerçekleştirebilirsiniz:

       sqlTableDF.printSchema
   
    Aşağıdakine benzer bir çıktı görürsünüz:

    ![Not defteri adını belirtme](./media/apache-spark-connect-to-sql-database/read-from-sql-schema-output.png "Not defteri adını belirtme")

1. Ayrıca, ilk 10 satırı alma gibi işlemler gerçekleştirebilirsiniz.

       sqlTableDF.show(10)

1. Veya belirli sütunları veri kümesinden alabilirsiniz.

       sqlTableDF.select("AddressLine1", "City").show(10)

## <a name="write-data-into-azure-sql-database"></a>Azure SQL veritabanı'na veri yazma

Bu bölümde, örnek CSV dosyası kullanılabilir küme üzerinde Azure SQL veritabanında bir tablo oluşturma ve verilerle doldurmak için kullanırız. CSV dosyasının (**HVAC.csv**) tüm HDInsight kümelerinde kullanılabilir `HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv`.

1. Yeni bir Jupyter Not Defteri, bir kod hücresine aşağıdaki kod parçacığını yapıştırın ve yer tutucu değerlerini Azure SQL veritabanınızın değerleriyle değiştirin.

       // Declare the values for your Azure SQL database

       val jdbcUsername = "<SQL DB ADMIN USER>"
       val jdbcPassword = "<SQL DB ADMIN PWD>"
       val jdbcHostname = "<SQL SERVER NAME HOSTING SDL DB>" //typically, this is in the form or servername.database.windows.net
       val jdbcPort = 1433
       val jdbcDatabase ="<AZURE SQL DB NAME>"

    Kod hücresini çalıştırmak için **SHIFT + ENTER** tuşlarına basın.  

1. Aşağıdaki kod parçacığı API'leri oluşturur Spark dataframe geçirebileceğiniz bir JDBC URL oluşturur bir `Properties` parametreleri tutacak nesne. Kod parçacığını yapıştırın bir kodu hücreyi ve ENTER tuşuna **SHIFT + ENTER** çalıştırılacak.

       import java.util.Properties

       val jdbc_url = s"jdbc:sqlserver://${jdbcHostname}:${jdbcPort};database=${jdbcDatabase};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=60;"
       val connectionProperties = new Properties()
       connectionProperties.put("user", s"${jdbcUsername}")
       connectionProperties.put("password", s"${jdbcPassword}")

1. Aşağıdaki kod parçacığı şemasını HVAC.csv verileri ayıklamak ve bir veri çerçevesi'nde bir CSV dosyasından verileri yüklemek için şemayı kullanma `readDf`. Kod parçacığını yapıştırın bir kodu hücreyi ve ENTER tuşuna **SHIFT + ENTER** çalıştırılacak.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readDf = spark.read.format("csv").schema(userSchema).load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Kullanım `readDf` geçici bir tablo oluşturmak için veri çerçevesi `temphvactable`. Bir hive tablosu oluşturmak için geçici tablo'i kullanın `hvactable_hive`.

       readDf.createOrReplaceTempView("temphvactable")
       spark.sql("create table hvactable_hive as select * from temphvactable")

1. Son olarak, hive tablosu, Azure SQL veritabanında bir tablo oluşturmak için kullanın. Aşağıdaki kod parçacığını oluşturur `hvactable` Azure SQL veritabanı'nda.

       spark.table("hvactable_hive").write.jdbc(jdbc_url, "hvactable", connectionProperties)

1. SSMS kullanarak Azure SQL veritabanına bağlanan ve gördüğünüzü doğrulayın bir `dbo.hvactable` vardır.

    a. SSMS'yi başlatın ve aşağıdaki ekran görüntüsünde gösterildiği gibi bağlantı ayrıntıları sağlayarak Azure SQL veritabanı'na bağlanma.

    ![SSMS kullanarak SQL database'e bağlanma](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms.png "SSMS kullanarak SQL veritabanına bağlanma")

    b. Nesne Gezgini'nde, Azure SQL veritabanı ve Tablo düğümü görmek için genişletin **dbo.hvactable** oluşturulur.

    ![SSMS kullanarak SQL database'e bağlanma](./media/apache-spark-connect-to-sql-database/connect-to-sql-db-ssms-locate-table.png "SSMS kullanarak SQL veritabanına bağlanma")

1. Ssms'de, tablodaki sütunları görmek için bir sorgu çalıştırın.

        SELECT * from hvactable

## <a name="stream-data-into-azure-sql-database"></a>Azure SQL veritabanına veri Stream

Bu bölümde, biz halinde veri akışı **hvactable** zaten Azure SQL veritabanında önceki bölümde oluşturduğunuz.

1. İlk adım, hiç kayıt olmadığından emin olun **hvactable**. SSMS kullanarak, tablo üzerinde şu sorguyu çalıştırın.

       TRUNCATE TABLE [dbo].[hvactable]

1. HDInsight Spark kümesinde yeni bir Jupyter not defteri oluşturun. Bir kod hücresine aşağıdaki kod parçacığını yapıştırın ve sonra basın **SHIFT + ENTER**:

       import org.apache.spark.sql._
       import org.apache.spark.sql.types._
       import org.apache.spark.sql.functions._
       import org.apache.spark.sql.streaming._
       import java.sql.{Connection,DriverManager,ResultSet}

1. Biz verilerinden akış **HVAC.csv** hvactable içine. HVAC.csv dosyasıdır kümede kullanılabilir `/HdiSamples/HdiSamples/SensorSampleData/HVAC/`. Aşağıdaki kod parçacığında, biz ilk veri akışını şeması alın. Ardından, bu şemayı kullanarak bir akış veri çerçevesi oluşturun. Kod parçacığını yapıştırın bir kodu hücreyi ve ENTER tuşuna **SHIFT + ENTER** çalıştırılacak.

       val userSchema = spark.read.option("header", "true").csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv").schema
       val readStreamDf = spark.readStream.schema(userSchema).csv("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/") 
       readStreamDf.printSchema

1. Çıktı şemasını gösterir **HVAC.csv**. **Hvactable** de aynı şemaya sahip. Çıkış tablodaki sütunlar listeler.

    ![Tablo şemasını](./media/apache-spark-connect-to-sql-database/schema-of-table.png "tablonun şeması")

1. Son olarak, HVAC.csv veri okuyup içine akışını aşağıdaki kod parçacığını kullanın **hvactable** Azure SQL veritabanı'nda. Kod parçacığını bir kod hücresine yapıştırın, yer tutucu değerlerini Azure SQL veritabanınızın değerleriyle değiştirin ve sonra basın **SHIFT + ENTER** çalıştırılacak.

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

1. Verileri içine akıtılan olduğunu doğrulayın **hvactable** aşağıdaki sorguyu çalıştırarak SQL Server Management Studio (SSMS). Sorguyu her çalıştırdığınızda tablo artan düzende satır sayısını gösterir.

        SELECT COUNT(*) FROM hvactable

## <a name="next-steps"></a>Sonraki adımlar

* [Data Lake Storage verilerini çözümlemek için HDInsight Spark kümesi kullanın](apache-spark-use-with-data-lake-store.md)
* [EventHub kullanarak yapılandırılmış akış olayları işleyin](apache-spark-eventhub-structured-streaming.md)
* [Apache Spark yapılandırılmış akışını HDInsight üzerinde Apache Kafka ile kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
