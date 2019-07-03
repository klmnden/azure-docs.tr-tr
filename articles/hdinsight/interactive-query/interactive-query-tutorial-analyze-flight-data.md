---
title: 'Öğretici: Ayıklama, dönüştürme ve yükleme (ETL) işlemleri kullanarak Azure HDInsight üzerinde etkileşimli sorgu gerçekleştirin'
description: Öğretici - ham CSV kümesinden verileri ayıklamak HDInsight üzerinde etkileşimli sorgu kullanarak dönüştürme hakkında bilgi edinin ve Apache Sqoop kullanarak bu dönüştürülmüş verileri Azure SQL veritabanı'na yükler.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 06/25/2019
ms.author: hrasheed
ms.custom: hdinsightactive,mvc
ms.openlocfilehash: 403e165d7ebe8365ffa0fd2f5f3779d3b4fab68f
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543639"
---
# <a name="tutorial-extract-transform-and-load-data-using-interactive-query-in-azure-hdinsight"></a>Öğretici: Ayıklama, dönüştürme ve kullanarak Azure HDInsight etkileşimli sorgu verileri yükleme

Bu öğreticide, bir ham CSV verileri dosyası herkese uçuş veri almak, HDInsight küme depolama alanına alma ve ardından Azure HDInsight etkileşimli sorgu kullanarak verileri dönüştürme. Veri dönüştürüldükten sonra bu verileri kullanarak bir Azure SQL veritabanı yük [Apache Sqoop](https://sqoop.apache.org/).

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Örnek uçuş verilerini indirme
> * Verileri bir HDInsight kümesine yükleme
> * Etkileşimli sorgu kullanarak verileri dönüştürme
> * Bir Azure SQL veritabanında bir tablo oluşturma
> * Bir Azure SQL veritabanına veri dışarı aktarmak için Sqoop kullanma

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde etkileşimli sorgu kümesi. Bkz: [Apache Hadoop kümeleri oluşturma Azure portalını kullanarak](../hdinsight-hadoop-create-linux-clusters-portal.md) seçip **etkileşimli sorgu** için **küme türü**.

* Bir Azure SQL veritabanı. Azure SQL veritabanını bir hedef veri deposu olarak kullanacaksınız. SQL veritabanınız yoksa bkz. [Azure portalında Azure SQL veritabanı oluşturma](/azure/sql-database/sql-database-single-database-get-started).

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="download-the-flight-data"></a>Uçuş verilerini indirme

1. Gözat [araştırma ve yenilikçi teknoloji yönetim, nakliye istatistikleri bürosu](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Sayfasında, tüm alanları temizleyin ve ardından aşağıdaki değerleri seçin:

   | Ad | Değer |
   | --- | --- |
   | Yıl Filtresi |2019 |
   | Dönem Filtresi |Ocak |
   | Alanlar |Yıl, FlightDate Reporting_Airline, DOT_ID_Reporting_Airline, Flight_Number_Reporting_Airline, OriginAirportID, kaynak, OriginCityName, OriginState, DestAirportID, hedef, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |

3. **Download** (İndir) seçeneğini belirleyin. Seçtiğiniz veri alanlarını içeren bir .zip dosyası alırsınız.

## <a name="upload-data-to-an-hdinsight-cluster"></a>Verileri bir HDInsight kümesine yükleme

Bir HDInsight kümesiyle ilişkili depolama birimine veri yüklemenin birçok yolu vardır. Bu bölümde, verileri karşıya yüklemek için `scp` kullanacaksınız. Verileri karşıya yüklemenin diğer yollarını öğrenmek için bkz. [Verileri HDInsight'a yükleme](../hdinsight-upload-data.md).

1. .Zip dosyasını HDInsight küme baş düğümüne yükleyin. Değiştirerek aşağıdaki komutu düzenleyin `FILENAME` .zip dosyasının adını ve `CLUSTERNAME` ile HDInsight kümesinin adı. Ardından bir komut istemi açın, çalışma dizininizin dosya konumuna ayarlayın ve ardından komutu girin.

    ```cmd
    scp FILENAME.zip sshuser@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.zip
    ```

2. Karşıya yükleme tamamlandıktan sonra SSH kullanarak kümeye bağlanın. Aşağıdaki komutta değiştirerek Düzenle `CLUSTERNAME` ile HDInsight kümesinin adı. Ardından aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Bir SSH bağlantısı kurulduktan sonra ortam değişkeni ayarlayın. Değiştirin `FILE_NAME`, `SQL_SERVERNAME`, `SQL_DATABASE`, `SQL_USER`, ve `SQL_PASWORD` uygun değerlerle. Ardından aşağıdaki komutu girin:

    ```bash
    export FILENAME=FILE_NAME
    export SQLSERVERNAME=SQL_SERVERNAME
    export DATABASE=SQL_DATABASE
    export SQLUSER=SQL_USER
    export SQLPASWORD='SQL_PASWORD'
    ```

4. .Zip dosyası, aşağıdaki komutu girerek sıkıştırmasını açın:

    ```bash
    unzip $FILENAME.zip
    ```

5. HDInsight depolama alanında bir dizin oluşturun ve ardından aşağıdaki komutu girerek .csv dosyasını dizine kopyalayın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put $FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="transform-data-using-a-hive-query"></a>Hive sorgusu kullanarak veri dönüştürme

Bir HDInsight kümesi üzerinde Hive işi çalıştırmanın çok sayıda yolu vardır. Bu bölümde, kullandığınız [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell) bir Hive işi çalıştırmak için. Bir Hive işi çalıştırma için diğer yöntemler hakkında bilgi [Apache Hive kullanma HDInsight üzerinde](../hadoop/hdinsight-use-hive.md).

Hive işinin bir parçası olarak, verileri .csv dosyasından **Delays** adlı bir Hive tablosuna aktarın.

1. Zaten sahip olduğunuz için HDInsight kümesine SSH isteminden oluşturmak için aşağıdaki komutu kullanın ve adlı yeni bir dosya Düzenle **flightdelays.hql**:

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

3. Dosyayı kaydetmek için basın **Ctrl + X**, ardından **y**, ardından girin.

4. Hive’ı başlatmak ve **flightdelays.hql** dosyasını çalıştırmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

5. **flightdelays.hql** betiği çalışmayı tamamladıktan sonra aşağıdaki komutu kullanarak etkileşimli bir Beeline oturumu açın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

6. `jdbc:hive2://localhost:10001/>` istemini aldığınızda, içeri aktarılan uçuş gecikme verilerini almak için aşağıdaki sorguyu kullanın:

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

7. Beeline’dan çıkmak için isteme `!quit` girin.

## <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

SQL Veritabanına bağlanıp tablo oluşturmanın çok sayıda yolu vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](http://www.freetds.org/) kullanılır.

1. FreeTDS yüklemek için küme açık SSH bağlantısından aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Yükleme tamamlandıktan sonra SQL Veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın.

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    Aşağıdakine benzer bir çıktı alırsınız:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to <yourdatabase>
    1>
    ```

3. `1>` isteminde aşağıdaki satırları girin:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
    ([origin_city_name] ASC))
    GO
    ```

    `GO` deyimi girildiğinde önceki deyimler değerlendirilir. Bu bildirimi adlı bir tablo oluşturur **gecikmeleri**, kümelenmiş bir dizin ile.

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

4. Tsql yardımcı programından çıkış yapmak için `1>` istemine `exit` girin.

## <a name="export-data-to-sql-database-using-apache-sqoop"></a>Apache Sqoop kullanarak SQL veritabanına veri dışarı aktarma

Önceki bölümlerde, `/tutorials/flightdelays/output` konumunda dönüştürülen verileri kopyaladınız. Bu bölümde, verileri `/tutorials/flightdelays/output` dizininden Azure SQL veritabanında oluşturduğunuz tabloya aktarmak için Sqoop kullanacaksınız.

1. Aşağıdaki komutu girerek Sqoop SQL veritabanınızı görebildiğini doğrulayın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433 --username $SQLUSER --password $SQLPASWORD
    ```

    Bu komut, oluşturduğunuz veritabanı dahil olmak üzere, veritabanlarının listesini döndürür `delays` daha önce tablo.

2. Verileri dışarı aktarma `/tutorials/flightdelays/output` için `delays` aşağıdaki komutu girerek tablosu:

    ```bash
    sqoop export --connect "jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433;database=$DATABASE" --username $SQLUSER --password $SQLPASWORD --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop içeren veritabanına bağlayan `delays` tablo ve dışarı veri `/tutorials/flightdelays/output` dizininden `delays` tablo.

3. Sqoop komut bittikten sonra aşağıdaki komutu girerek veritabanına bağlanmak için tsql yardımcı programını kullanın:

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    Verilerin delays tablosuna aktarıldığını doğrulamak için aşağıdaki ifadeleri kullanın:

    ```sql
    SELECT * FROM delays
    GO
    ```

    Tabloda verilerin listesini görürsünüz. Tablo, şehir adını ve bu şehre ait ortalama uçuş gecikme süresini içerir. 

    Tsql yardımcı programından çıkmak için `exit` yazın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir ham gerçekleştirdiğiniz CSV verileri dosyası, bir HDInsight küme depolama alanına alınan ve ardından Azure HDInsight etkileşimli sorgu kullanarak verileri dönüştürülür.  Apache Hive ambarı Bağlayıcısı hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Hive ambarı Bağlayıcısı ile Apache Hive ve Apache Spark'ı tümleştirme](./apache-hive-warehouse-connector.md)
