---
title: "Öğretici: Azure HDInsight üzerinde Apache Hive'ı kullanarak ayıklama, dönüştürme ve yükleme (ETL) işlemleri gerçekleştirin."
description: Bu öğreticide, ham CSV kümesinden verileri ayıklamak için Azure HDInsight üzerinde Apache Hive'ı kullanarak verileri dönüştürebilirsiniz öğrenin ve ardından dönüştürülmüş verileri Sqoop kullanarak Azure SQL veritabanı'na yükleyin.
services: storage
author: jamesbak
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jamesbak
ms.openlocfilehash: a5e7fd200617661c38b65ebbd4473a1a729de457
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682364"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-apache-hive-on-azure-hdinsight"></a>Öğretici: Ayıklama, dönüştürme ve Azure HDInsight üzerinde Apache Hive'ı kullanarak veri yükleme

Bu öğreticide, bir ETL işlemi gerçekleştirin: ayıklama, dönüştürme ve veri yükleyebilir. Bir ham CSV verileri dosyası olması, bir Azure HDInsight kümesinde almak, Apache Hive ile dönüştürme ve Apache Sqoop ile bir Azure SQL veritabanına yükleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Ayıklayın ve bir HDInsight kümesi için verileri karşıya yükleyin.
> * Apache Hive'ı kullanarak verileri dönüştürme.
> * Sqoop kullanarak bir Azure SQL veritabanına veri yükleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **HDInsight için yapılandırılmış bir Azure Data Lake depolama Gen2'ye depolama hesabı**

    Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2).

* **HDInsight üzerinde Linux tabanlı Hadoop kümesi**

    Bkz: [hızlı başlangıç: Apache Hadoop ve Apache Hive, Azure portalını kullanarak Azure HDInsight ile çalışmaya başlama](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-linux-create-cluster-get-started-portal).

* **Azure SQL veritabanı**: Azure SQL veritabanını bir hedef veri deposu olarak kullanacaksınız. SQL veritabanınız yoksa bkz. [Azure portalında Azure SQL veritabanı oluşturma](../../sql-database/sql-database-get-started.md).

* **Azure CLI**: Azure CLI'yi yüklemediyseniz, bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Güvenli Kabuk (SSH) istemcisi**: Daha fazla bilgi için [SSH kullanarak HDInsight (Hadoop) bağlanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Bu makaledeki adımlarda Linux kullanan bir HDInsight kümesi gerektirir. Linux üzerinde Azure HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemi ' dir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="download-the-flight-data"></a>Uçuş verilerini indirme

1. Gözat [araştırma ve yenilikçi teknoloji yönetim, nakliye istatistikleri bürosu](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Sayfada aşağıdaki değerleri seçin:

   | Adı | Değer |
   | --- | --- |
   | Yıl Filtresi |2013 |
   | Dönem Filtresi |Ocak |
   | Alanlar |Yıl, FlightDate Reporting_Airline, IATA_CODE_Reporting_Airline, Flight_Number_Reporting_Airline, OriginAirportID, kaynak, OriginCityName, OriginState, DestAirportID, hedef, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   
   Diğer tüm alanları temizleyin.

3. **Download** (İndir) seçeneğini belirleyin. Seçtiğiniz veri alanlarını içeren bir .zip dosyası alırsınız.

## <a name="extract-and-upload-the-data"></a>Ayıklama ve verileri yükleme

Bu bölümde, HDInsight kümenize karşıya yüklemek ve ardından verileri Data Lake depolama Gen2 hesabınıza kopyalayın.

1. Bir komut istemi açın ve HDInsight küme baş düğümüne .zip dosyasını karşıya yüklemek için aşağıdaki güvenli kopya (Scp) komutunu kullanın:

   ```bash
   scp <file-name>.zip <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net:<file-name.zip>
   ```

   * Değiştirin `<file-name>` yer tutucu içeren .zip dosyasının adı.
   * Değiştirin `<ssh-user-name>` SSH oturum açma HDInsight kümesi için yer tutucu.
   * Değiştirin `<cluster-name>` yer tutucu ile HDInsight kümesinin adı.

   SSH oturum açma bilgilerinizi doğrulamak için bir parola kullanıyorsanız parola girmeniz istenir.

   Ortak anahtar kullanıyorsanız, eşleşen özel anahtarın yolunu belirtmek için `-i` parametresini kullanmanız gerekebilir. Örneğin, `scp -i ~/.ssh/id_rsa <file_name>.zip <user-name>@<cluster-name>-ssh.azurehdinsight.net:`.

2. Karşıya yükleme tamamlandıktan sonra SSH kullanarak kümeye bağlanın. Komut istemine aşağıdaki komutu girin:

   ```bash
   ssh <ssh-user-name>@<cluster-name>-ssh.azurehdinsight.net
   ```

3. .zip dosyasını açmak için aşağıdaki komutu kullanın:

   ```bash
   unzip <file-name>.zip
   ```

   Komut ayıklar bir **.csv** dosya.

4. Data Lake depolama Gen2'ye dosya sistemi oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/
   ```

   Değiştirin `<file-system-name>` dosya sisteminize vermek istediğiniz adla yer tutucu.

   Değiştirin `<storage-account-name>` depolama hesabınızın adıyla yer tutucu.

5. Bir dizin oluşturmak için aşağıdaki komutu kullanın.

   ```bash
   hdfs dfs -mkdir -p abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data
   ```

6. Kopyalamak için aşağıdaki komutu kullanın *.csv* dosyayı dizine:

   ```bash
   hdfs dfs -put "<file-name>.csv" abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data/
   ```

   Dosya adı boşluk veya özel karakterler içeriyorsa dosya adını tırnak kullanın.

## <a name="transform-the-data"></a>Verileri dönüştürme

Bu bölümde, bir Apache Hive işi çalıştırmak için Beeline kullanma.

Apache Hive işi bir parçası olarak, verileri .csv dosyasından adlı bir Apache Hive tablosuna içeri **gecikmeleri**.

1. Zaten sahip olduğunuz için HDInsight kümesine SSH isteminden adlı yeni bir dosya oluşturup aşağıdaki komutu kullanın. **flightdelays.hql**:

   ```bash
   nano flightdelays.hql
   ```

2. Aşağıdaki metni tarafından değiştir değiştir `<file-system-name>` ve `<storage-account-name>` yer tutucuları olan dosya sistemi ve depolama hesabı adı. Daha sonra kopyalayın ve birlikte sağ fare düğmesine SHIFT tuşuna basarak kullanarak nano konsola metni yapıştırın.

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
    LOCATION 'abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays
    LOCATION 'abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/processed'
    AS
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

3. Kullanarak dosyayı kaydedin, CTRL + X kullanın ve ardından yazın `Y` istendiğinde.

4. Hive’ı başlatmak ve **flightdelays.hql** dosyasını çalıştırmak için aşağıdaki komutu kullanın:

   ```bash
   beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
   ```

5. __flightdelays.hql__ betiği çalışmayı tamamladıktan sonra aşağıdaki komutu kullanarak etkileşimli bir Beeline oturumu açın:

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

   Bu sorgu, hava durumundan kaynaklanan gecikmeler yaşayan şehirlerin bir listesini, ortalama gecikme süresi ile birlikte alır ve `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output` konumuna kaydeder. Daha sonra, Sqoop bu konumdaki verileri okur ve Azure SQL Veritabanına aktarır.

7. Beeline’dan çıkmak için isteme `!quit` girin.

## <a name="create-a-sql-database-table"></a>SQL veritabanı tablosu oluşturma

Bu işlem için SQL veritabanı, sunucu adı gerekir. Sunucu adını bulmak için şu adımları tamamlayın.

1. [Azure Portal](https://portal.azure.com) gidin.

2. Seçin **SQL veritabanları**.

3. Kullanmayı seçerseniz veritabanının adına filtreleyin. Sunucu adı, **Sunucu adı** sütununda listelenir.

4. Kullanmak istediğiniz veritabanı adına filtreleyin. Sunucu adı, **Sunucu adı** sütununda listelenir.

    ![Azure SQL server ayrıntılarını alma](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Azure SQL server ayrıntılarını alma")

    SQL Veritabanına bağlanıp tablo oluşturmanın çok sayıda yolu vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](http://www.freetds.org/) kullanılır.

5. FreeTDS yüklemek için küme ile kurulan bir SSH bağlantısından aşağıdaki komutu kullanın:

   ```bash
   sudo apt-get --assume-yes install freetds-dev freetds-bin
   ```

6. Yükleme tamamlandıktan sonra SQL veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın.

   ```bash
   TDSVER=8.0 tsql -H '<server-name>.database.windows.net' -U '<admin-login>' -p 1433 -D '<database-name>'
    ```
   * Değiştirin `<server-name>` yer tutucusunu SQL veritabanı sunucu adı ile.

   * Değiştirin `<admin-login>` yer tutucusunu SQL veritabanı için yönetici oturum açma ile.

   * Değiştirin `<database-name>` yer tutucu ile veritabanı adı

   İstendiğinde, SQL veritabanı Yöneticisi oturum açma bilgileri için parolayı girin.

   Aşağıdakine benzer bir çıktı alırsınız:

   ```
   locale is "en_US.UTF-8"
   locale charset is "UTF-8"
   using default charset "UTF-8"
   Default database being set to sqooptest
   1>
   ```

7. Adresindeki `1>` isteminde, aşağıdaki deyimleri girin:

   ```hiveql
   CREATE TABLE [dbo].[delays](
   [OriginCityName] [nvarchar](50) NOT NULL,
   [WeatherDelay] float,
   CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
   ([OriginCityName] ASC))
   GO
   ```

8. `GO` deyimi girildiğinde önceki deyimler değerlendirilir.

   Sorgu adlı bir tablo oluşturur **gecikmeleri**, kümelenmiş dizinine sahip.

9. Tablo oluşturulduğunu doğrulamak için aşağıdaki sorguyu kullanın:

   ```hiveql
   SELECT * FROM information_schema.tables
   GO
   ```

   Çıktı aşağıdaki metne benzer:

   ```
   TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
   databaseName       dbo             delays        BASE TABLE
   ```

10. Tsql yardımcı programından çıkış yapmak için `1>` istemine `exit` girin.

## <a name="export-and-load-the-data"></a>Dışarı aktarma ve verileri yükleme

Önceki bölümde kopyaladığınız konumda dönüştürülmüş verileri `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output`. Bu bölümde, verileri dışarı aktarmak için Sqoop kullanma `abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/tutorials/flightdelays/output` Azure SQL veritabanı'nda, oluşturduğunuz tabloya.

1. Sqoop’un SQL veritabanınızı görebildiğini doğrulamak için aşağıdaki komutu kullanın:

   ```bash
   sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
   ```

   Komut, oluşturduğunuz veritabanı dahil olmak üzere, veritabanlarının listesini döndürür **gecikmeleri** tablo.

2. Verileri dışarı aktarmak için aşağıdaki komutu kullanın **hivesampletable** tablo **gecikmeleri** tablosu:

   ```bash
   sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<file-system-name>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
   ```

   Sqoop içeren veritabanına bağlayan **gecikmeleri** tablo ve dışarı veri `/tutorials/flightdelays/output` dizininden **gecikmeleri** tablo.

3. Sonra `sqoop` komut tamamlandığında, veritabanına bağlanmak için tsql yardımcı programını kullanın:

   ```bash
   TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
   ```

4. Verileri dışarı aktarılan doğrulamak için aşağıdaki deyimleri kullanın **gecikmeleri** tablosu:

   ```sql
   SELECT * FROM delays
   GO
   ```

   Tabloda verilerin listesini görürsünüz. Tablo, şehir adını ve bu şehre ait ortalama uçuş gecikme süresini içerir.

5. Girin `exit` tsql yardımcı programı'ndan çıkmak için.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide kullanılan tüm kaynakları önceden var olan. Temizleme gerekli değildir.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight verilerle çalışma hakkında daha fazla bilgi için şu makaleye bakın:

> [!div class="nextstepaction"]
> [Azure Data Lake depolama Gen2 Azure HDInsight kümeleri ile kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
