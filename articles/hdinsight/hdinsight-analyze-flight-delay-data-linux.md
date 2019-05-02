---
title: "Öğretici: Ayıklama, dönüştürme ve yükleme (ETL) işlemleri - Azure HDInsight üzerinde Hive'ı kullanarak gerçekleştirin "
description: Ham CSV veri kümesinden veri ayıklama, HDInsight üzerinde Hive kullanarak dönüştürme ve sonra Apache Sqoop kullanarak dönüştürülmüş verileri Azure SQL veritabanına yükleme hakkında bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,mvc
ms.openlocfilehash: eb86dc8c5c3b215a2c90380b4009efd00d2a243c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64723152"
---
# <a name="tutorial-extract-transform-and-load-data-using-apache-hive-on-azure-hdinsight"></a>Öğretici: Azure HDInsight üzerinde Apache Hive kullanarak verileri ayıklama, dönüştürme ve yükleme

Bu öğreticide, bir ham CSV verileri dosyası olması, bir HDInsight küme depolama alanına alma ve ardından kullanarak verileri dönüştürme [Apache Hive](https://hive.apache.org/) Azure HDInsight üzerinde. Veri dönüştürüldükten sonra bu verileri kullanarak bir Azure SQL veritabanı yük [Apache Sqoop](https://sqoop.apache.org/). Bu makalede, genel kullanıma açık uçuş verileri kullanacaksınız.

> [!IMPORTANT]  
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, Azure HDInsight sürüm 3.4 veya üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

Bu öğretici aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * Örnek uçuş verilerini indirme
> * Verileri bir HDInsight kümesine yükleme
> * Hive kullanarak verileri dönüştürme
> * Azure SQL veritabanında tablo oluşturma
> * Sqoop kullanarak Azure SQL veritabanına veri aktarma


Aşağıdaki şekilde tipik bir ETL uygulama akışı gösterilmektedir.

![Azure HDInsight üzerinde Apache Hive kullanarak ETL işlemi](./media/hdinsight-analyze-flight-delay-data-linux/hdinsight-etl-architecture.png "Azure HDInsight üzerinde Apache Hive kullanarak ETL işlemi")

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

* **HDInsight üzerinde Linux tabanlı Hadoop kümesi**. Bkz: [HDInsight Apache Hadoop kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md) yeni Linux tabanlı HDInsight kümesi oluşturma adımları için.

* **Azure SQL Veritabanı**. Azure SQL veritabanını bir hedef veri deposu olarak kullanacaksınız. SQL veritabanınız yoksa bkz. [Azure portalında Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Azure CLI yüklemediyseniz, daha fazla adım için [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) konusuna bakın.

* **Bir SSH istemcisi**. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="download-the-flight-data"></a>Uçuş verilerini indirme

1. [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website] (Araştırma ve Yenilikçi Teknolojiler İdaresi, Ulaşım İstatistikleri Bürosu) sayfasına göz atın.

2. Sayfada aşağıdaki değerleri seçin:

   | Adı | Değer |
   | --- | --- |
   | Yıl Filtresi |2013 |
   | Dönem Filtresi |Ocak |
   | Alanlar |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   
   Diğer tüm alanları temizleyin. 

3. **Download** (İndir) seçeneğini belirleyin. Seçtiğiniz veri alanlarını içeren bir .zip dosyası alırsınız.

## <a name="upload-data-to-an-hdinsight-cluster"></a>Verileri bir HDInsight kümesine yükleme

Bir HDInsight kümesiyle ilişkili depolama birimine veri yüklemenin birçok yolu vardır. Bu bölümde, verileri karşıya yüklemek için `scp` kullanacaksınız. Verileri karşıya yüklemenin diğer yollarını öğrenmek için bkz. [Verileri HDInsight'a yükleme](hdinsight-upload-data.md).

1. Bir komut istemi açın ve aşağıdaki komutu kullanarak .zip dosyasını HDInsight kümesinin baş düğümüne yükleyin:

    ```bash
    scp <FILENAME>.zip <SSH-USERNAME>@<CLUSTERNAME>-ssh.azurehdinsight.net:<FILENAME.zip>
    ```

    *FILENAME* değerini .zip dosyasının adıyla değiştirin. *USERNAME* değerini HDInsight kümesinin SSH oturum açma adıyla değiştirin. *CLUSTERNAME* değerini HDInsight kümesinin adıyla değiştirin.

   > [!NOTE]  
   > SSH oturum açma bilgilerinizi doğrulamak için bir parola kullanıyorsanız parola girmeniz istenir. Ortak anahtar kullanıyorsanız, eşleşen özel anahtarın yolunu belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Karşıya yükleme tamamlandıktan sonra SSH kullanarak kümeye bağlanın. Komut istemine aşağıdaki komutu girin:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

3. .zip dosyasını açmak için aşağıdaki komutu kullanın:

    ```bash
    unzip FILENAME.zip
    ```

    Bu komut yaklaşık 60 MB boyutu olan bir .csv dosyasını ayıklar.

4. HDInsight deposunda bir dizin oluşturmak için aşağıdaki komutları kullanın ve sonra .csv dosyasını dizine kopyalayın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put <FILENAME>.csv /tutorials/flightdelays/data/
    ```

## <a name="transform-data-using-a-hive-query"></a>Hive sorgusu kullanarak veri dönüştürme

Bir HDInsight kümesi üzerinde Hive işi çalıştırmanın çok sayıda yolu vardır. Bu bölümde, kullandığınız [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell) bir Hive işi çalıştırmak için. Bir Hive işi çalıştırma için diğer yöntemler hakkında bilgi [Apache Hive kullanma HDInsight üzerinde](./hadoop/hdinsight-use-hive.md).

Hive işinin bir parçası olarak, verileri .csv dosyasından **Delays** adlı bir Hive tablosuna aktarın.

1. HDInsight kümesi için aldığınız SSH isteminden aşağıdaki komutu kullanarak **flightdelays.hql** adlı yeni bir dosya oluşturup düzenleyin:

    ```bash
    nano flightdelays.hql
    ```

2. Bu dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. Dosyayı kaydetmek için **Esc** tuşuna basıp `:x` girin.

3. Hive’ı başlatmak ve **flightdelays.hql** dosyasını çalıştırmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

4. __flightdelays.hql__ betiği çalışmayı tamamladıktan sonra aşağıdaki komutu kullanarak etkileşimli bir Beeline oturumu açın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. `jdbc:hive2://localhost:10001/>` istemini aldığınızda, içeri aktarılan uçuş gecikme verilerini almak için aşağıdaki sorguyu kullanın:

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Bu sorgu, hava durumundan kaynaklanan gecikmeler yaşayan şehirlerin bir listesini, ortalama gecikme süresi ile birlikte alır ve `/tutorials/flightdelays/output` konumuna kaydeder. Daha sonra, Sqoop bu konumdaki verileri okur ve Azure SQL Veritabanına aktarır.

6. Beeline’dan çıkmak için isteme `!quit` girin.

## <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

Bu bölümde, daha önce bir Azure SQL veritabanı oluşturduğunuz varsayılır. Henüz bir SQL veritabanınız yoksa, bir tane oluşturmak için [Azure portalında Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md) bölümündeki bilgileri kullanın.

Zaten bir SQL veritabanınız varsa, sunucu adını almanız gerekir. [Azure portalında](https://portal.azure.com) sunucu adını bulmak için **SQL Veritabanları**’nı seçin ve sonra kullanmayı seçtiğiniz veritabanının adıyla filtreleyin. Sunucu adı, **Sunucu adı** sütununda listelenir.

![Azure SQL server ayrıntılarını alma](./media/hdinsight-analyze-flight-delay-data-linux/get-azure-sql-server-details.png "Azure SQL server ayrıntılarını alma")

> [!NOTE]  
> SQL Veritabanına bağlanıp tablo oluşturmanın çok sayıda yolu vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](http://www.freetds.org/) kullanılır.


1. FreeTDS yüklemek için küme ile kurulan bir SSH bağlantısından aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Yükleme tamamlandıktan sonra SQL Veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın. **serverName** değerini SQL Veritabanı sunucusunun adıyla değiştirin. **adminLogin** ve **adminPassword** değerini SQL Veritabanı oturum açma bilgileriyle değiştirin. **databaseName** değerini veritabanı adıyla değiştirin.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -p 1433 -D <databaseName>
    ```

    Sorulduğunda, SQL Veritabanı yöneticisinin parolasını girin.

    Aşağıdakine benzer bir çıktı alırsınız:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. `1>` isteminde aşağıdaki satırları girin:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    `GO` deyimi girildiğinde önceki deyimler değerlendirilir. Bu sorgu, kümelenmiş bir dizin ile **delays** adlı bir tablo oluşturur.

    Tablonun oluşturulduğunu doğrulamak için aşağıdaki sorguyu kullanın:

    ```hiveql
    SELECT * FROM information_schema.tables
    GO
    ```

    Çıktı aşağıdaki metne benzer:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo             delays        BASE TABLE
    ```

5. Tsql yardımcı programından çıkış yapmak için `1>` istemine `exit` girin.

## <a name="export-data-to-sql-database-using-apache-sqoop"></a>Apache Sqoop kullanarak SQL veritabanına veri dışarı aktarma

Önceki bölümlerde, `/tutorials/flightdelays/output` konumunda dönüştürülen verileri kopyaladınız. Bu bölümde, verileri '/tutorials/flightdelays/output` dizininden Azure SQL veritabanında oluşturduğunuz tabloya aktarmak için Sqoop kullanacaksınız. 

1. Sqoop’un SQL veritabanınızı görebildiğini doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Bu komut, daha önce delays tablosunda oluşturduğunuz veritabanı dahil olmak üzere veritabanlarının bir listesini döndürür.

2. Verileri hivesampletable tablosundan delays tablosuna aktarmak için aşağıdaki komutu kullanın:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop, delays tablosunu içeren veritabanına bağlanır ve verileri `/tutorials/flightdelays/output` dizininden delays tablosuna aktarır.

3. Sqoop komutu tamamlandıktan sonra veritabanına bağlanmak için tsql yardımcı programını kullanın:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Verilerin delays tablosuna aktarıldığını doğrulamak için aşağıdaki ifadeleri kullanın:

    ```sql
    SELECT * FROM delays
    GO
    ```

    Tabloda verilerin listesini görürsünüz. Tablo, şehir adını ve bu şehre ait ortalama uçuş gecikme süresini içerir. 

    Tsql yardımcı programından çıkmak için `exit` yazın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, HDInsight üzerinde bir Apache Hadoop kümesi kullanarak veri ayıklama, dönüştürme ve yükleme işlemlerinin nasıl gerçekleştirildiğini öğrendiniz. Azure Data Factory kullanarak isteğe bağlı HDInsight Hadoop kümeleri oluşturmayı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[İsteğe bağlı Apache Hadoop kümeleri Azure Data Factory kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-adf.md)

HDInsight’ta verilerle çalışmanın diğer yollarını öğrenmek için aşağıdaki makalelere bakın:

* [Öğretici: Ayıklama, dönüştürme ve Azure HDInsight üzerinde Apache Hive kullanarak verileri yükleme](../storage/data-lake-storage/tutorial-extract-transform-load-hive.md)
* [Apache Hive, HDInsight ile kullanma][hdinsight-use-hive]
* [Apache Pig, HDInsight ile kullanma][hdinsight-use-pig]
* [HDInsight üzerinde Apache Hadoop için Java MapReduce programları geliştirme][hdinsight-develop-mapreduce]

* [HDInsight ile Apache Oozie kullanma][hdinsight-use-oozie]
* [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]



[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/


[rita-website]: https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md

[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
