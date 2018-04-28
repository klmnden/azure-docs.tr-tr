---
title: Hive hdınsight'ta - Azure ile uçuş gecikme veri çözümleme | Microsoft Docs
description: Hive Linux tabanlı Hdınsight üzerinde uçuş verileri çözümlemek ve Sqoop kullanarak bu verileri SQL veritabanına vermek için nasıl kullanılacağını öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: fd0daae8289839b64e7b54d97c78719587c18e7d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>Linux tabanlı Hdınsight'ta Hive kullanarak uçuş gecikme verilerini çözümleme

Linux tabanlı Hdınsight'ta Hive kullanarak uçuş gecikme verilerini analiz etme ve Sqoop kullanarak Azure SQL veritabanına verileri dışarı aktarma öğrenin.

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux Azure Hdınsight sürüm 3.4 veya üstü kullanılan yalnızca işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight kümesi**. Bkz: [Hdınsight'ta Hadoop kullanmaya başlamanıza](hadoop/apache-hadoop-linux-tutorial-get-started.md) yeni bir Linux tabanlı Hdınsight kümesi oluşturma adımları için.

* **Azure SQL Veritabanı**. Hedef veri deposu olarak Azure SQL veritabanını kullanın. Bir SQL veritabanı yoksa bkz [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Azure CLI yüklemediyseniz, bkz: [Azure CLI 1.0 yüklemek](../cli-install-nodejs.md) daha fazla adım için.

* **Bir SSH istemcisi**. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight'a (Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="download-the-flight-data"></a>Uçuş veri indirin

1. Gözat [araştırma ve yenilikçi teknoloji yönetim, taşıma İstatistik kuruluşu][rita-website].

2. Sayfasında, aşağıdaki değerleri seçin:

   | Ad | Değer |
   | --- | --- |
   | Filtre yıl |2013 |
   | Dönem filtre |Ocak |
   | Alanlar |Yıl, FlightDate, UniqueCarrier, taşıyıcı, FlightNum, OriginAirportID, kaynak, OriginCityName, OriginState, DestAirportID, hedef, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   Diğer tüm alanlar temizleyin. 

3. Seçin **karşıdan**.

## <a name="upload-the-data"></a>Veri yükleme

1. Hdınsight küme baş düğümüne .zip dosyasını karşıya yüklemek için aşağıdaki komutu kullanın:

    ```bash
    scp FILENAME.zip sshuser@clustername-ssh.azurehdinsight.net:
    ```

    Değiştir `FILENAME` .zip dosya adı. Değiştir `sshuser` SSH oturum açma için Hdınsight kümesine sahip. Değiştir `clustername` Hdınsight kümesi adı.

2. Karşıya yükleme tamamlandıktan sonra SSH kullanarak kümeye bağlanın:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

3. .Zip dosyasını sıkıştırmasını açmak için aşağıdaki komutu kullanın:

    ```bash
    unzip FILENAME.zip
    ```

    Bu komut, kabaca 60 MB boyutunda bir .csv dosyası ayıklar.

4. Hdınsight depolama biriminde bir dizin oluşturmak için aşağıdaki komutu kullanın ve ardından dosyayı dizinine kopyalayın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a>Oluşturma ve HiveQL çalıştırma

Adlı bir Hive tabloya .csv dosyasından veri almak için aşağıdaki adımları kullanın **gecikmeler**.

1. Oluşturma ve düzenleme adlı yeni bir dosya için aşağıdaki komutu kullanın **flightdelays.hql**:

    ```bash
    nano flightdelays.hql
    ```

    Aşağıdaki metni bu dosyanın içeriğini kullanın:

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

2. Dosyayı kaydetmek için Ctrl + X, Y kullanın.

3. Hive başlatmak ve çalıştırmak için **flightdelays.hql** dosya, aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

4. Sonra __flightdelays.hql__ betik çalıştıran bitirdiğinde, etkileşimli Beeline oturum açmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Aldığınızda `jdbc:hive2://localhost:10001/>` isteminde, verileri içeri aktarılan uçuş gecikme verilerini almak için aşağıdaki sorguyu kullanın:

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Bu sorgu deneyimli hava durumu, ortalama gecikme süresi ile birlikte, gecikmeler ve kendisine kaydeden Şehir listesini alır `/tutorials/flightdelays/output`. Daha sonra Sqoop bu konumdan veri okuyan ve bunu Azure SQL veritabanına aktarır.

6. Beeline çıkmak için girin `!quit` isteminde.

## <a name="create-a-sql-database"></a>SQL veritabanı oluşturma

Bir SQL veritabanı zaten varsa, sunucu adını edinmeniz gerekir. Sunucu adını bulmak için [Azure portal](https://portal.azure.com)seçin **SQL veritabanları**ve daha sonra kullanmak için seçtiğiniz veritabanı adına filtre. Sunucu adı listelenir **SERVER** sütun.

Bir SQL veritabanı zaten sahip değilseniz, bilgileri kullanmak [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md) oluşturmak için. Veritabanı için kullanılan sunucu adını kaydedin.

## <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

> [!NOTE]
> SQL veritabanına bağlanmak ve bir tablo oluşturmak için birçok yolu vardır. Aşağıdaki adımları kullanın [ücretsiz](http://www.freetds.org/) Hdınsight kümesine ait.


1. Ücretsiz yüklemek için bir SSH bağlantısı küme aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Yükleme tamamlandıktan sonra SQL veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın. Değiştir **serverName** SQL veritabanı sunucu adı. Değiştir **adminLogin** ve **Admınpassword** SQL veritabanı için oturum açma ile. Değiştir **databaseName** veritabanı adında.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -p 1433 -D <databaseName>
    ```

    İstendiğinde, SQL veritabanı yönetici oturum açma için parola girin.

    Aşağıdakine benzer bir çıktı alırsınız:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. Konumundaki `1>` isteminde, aşağıdaki satırları girin:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Zaman `GO` deyimi girilir, önceki deyimleri değerlendirilir. Bu sorgu adlı bir tablo oluşturur **gecikmeler**, kümelenmiş bir dizin ile.

    Tablo oluşturulduğunu doğrulamak için aşağıdaki sorguyu kullanın:

    ```hiveql
    SELECT * FROM information_schema.tables
    GO
    ```

    Çıktı aşağıdaki metne benzer:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Girin `exit` adresindeki `1>` tsql yardımcı programı'ndan çıkmak komut istemi.

## <a name="export-data-with-sqoop"></a>Sqoop ile verileri dışarı aktarma

1. Sqoop SQL veritabanınız görebildiğini doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Bu komut, daha önce gecikmeler tablo oluşturulan veritabanı dahil olmak üzere veritabanlarının listesini döndürür.

2. Veri hivesampletable gecikmeler tabloya dışarı aktarmak için aşağıdaki komutu kullanın:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop bağlayan gecikmeler tablo içeren ve verileri dışa aktarır veritabanına `/tutorials/flightdelays/output` gecikmeler tabloya dizin.

3. Sqoop komut bittikten sonra veritabanına bağlanmak için tsql yardımcı programını kullanın:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Veri gecikmeler tablosuna aktarılmış doğrulamak için aşağıdaki ifadeleri kullanın:

    ```sql
    SELECT * FROM delays
    GO
    ```

    Tablosunda veri listesini görmelisiniz. Tür `exit` tsql yardımcı programı'ndan çıkmak için.

## <a name="next-steps"></a>Sonraki adımlar

Hdınsight'ta verilerle çalışma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight ile Hive kullanma][hdinsight-use-hive]
* [Oozie Hdınsight ile kullanma][hdinsight-use-oozie]
* [Hdınsight ile Sqoop kullanma][hdinsight-use-sqoop]
* [HDInsight ile Pig kullanma][hdinsight-use-pig]
* [Hdınsight'ta Hadoop için Java MapReduce programlar geliştirmek][hdinsight-develop-mapreduce]
* [MapReduce programları Hdınsight için akış Python geliştirme][hdinsight-develop-streaming]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-develop-streaming]:hadoop/apache-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
