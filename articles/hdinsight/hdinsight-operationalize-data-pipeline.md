---
title: Veri analizi işlem hattı - Azure'ı kullanıma hazır hale getirme
description: Ayarlama ve yeni verilerle tetiklenir ve kısa sonuçları üreten bir örnek veri işlem hattı çalıştırma.
ms.service: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/11/2018
ms.openlocfilehash: 524386c046534b0ef0050e15d326118b84822822
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64718035"
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Veri analizi işlem hattını kullanıma hazır hale getirme

*Veri komut zincirlerini* underly birçok veri analizi çözümleri. Adından da anlaşılacağı gibi bir veri işlem hattı ham verileri alır, temizler ve gerektiği gibi yeniden şekillendirir ve sonra genellikle hesaplamaları veya toplama işlenen verileri depolamadan önce gerçekleştirir. İşlenen veri, istemciler, raporlara veya API'leri tarafından kullanılır. Veri işlem hattı, bir zamanlama olup olmadığını veya yeni veriler tarafından tetiklendiğinde tekrarlanabilir sonuçlar sağlamanız gerekir.

Bu makalede, veri işlem hatlarınızı yinelenebilirliği, HDInsight Hadoop kümelerini çalıştıran Oozie kullanma için hazır hale getirmek açıklar. Örnek senaryo hazırlar ve Havayolu uçuş zaman serisi verilerini işleyen bir veri işlem hattı size yol gösterir.

Aşağıdaki senaryoda, girdi verilerini bir ay için bir dizi uçuş verileri içeren bir düz bir dosyadır. Bu uçuş verileri kaynak ve hedef airport, akışı mil, kalkış ve varış kez ve benzeri gibi bilgileri içerir. Bu işlem hattı ile günlük Havayolu performansı özetlemek için burada her Havayolu dakika ve o gün akışı toplam mil ortalama kalkış ve varış gecikmeleri ile her gün için bir satır var. hedeftir.

| YEAR | MONTH | DAY_OF_MONTH | TAŞIYICI |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | AA | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | DL | 6.935409 | -2.1893024 | 1909696 |

Örnek işlem hattı, bir yeni dönemin uçuş verileri ulaşır ve uzun vadeli analizleri için Apache Hive, veri ambarı'na, ayrıntılı uçuş bilgilerini depolar kadar bekler. İşlem hattı yalnızca günlük uçuş verileri özetleyen bir çok daha küçük veri kümesi de oluşturur. Bu günlük uçuş Özet veriler, raporlar gibi bir Web sitesi için sağlamak için bir SQL veritabanı'na gönderilir.

Aşağıdaki diyagramda, örnek işlem hattı gösterilmektedir.

![Uçuş veri işlem hattı](./media/hdinsight-operationalize-data-pipeline/pipeline-overview.png)

## <a name="apache-oozie-solution-overview"></a>Apache Oozie çözümüne genel bakış

Bu işlem hattı, bir HDInsight Hadoop küme üzerinde çalışan Apache Oozie kullanır.

Oozie açıklar de işlem hatları *eylemleri*, *iş akışları*, ve *düzenleyicileri*. Bir Hive sorgusu çalıştırmak gibi gerçekleştirmek için asıl işi eylemleri belirler. İş akışları, eylemlerin sıralamasını tanımlarsınız. Düzenleyicileri, iş akışı çalıştırmak için zamanlamayı tanımlar. Düzenleyicileri, iş akışı örneği başlatmadan önce yeni veri kullanılabilirliğine de bekleyebilirsiniz.

Aşağıdaki diyagramda, bu örnek Oozie ardışık düzeninin üst düzey tasarım gösterilmektedir.

![Oozie uçuş veri işlem hattı](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

### <a name="provision-azure-resources"></a>Azure kaynaklarını hazırlama

Bu işlem hattı, bir Azure SQL veritabanı ile aynı konumda bir HDInsight Hadoop kümesi gerektirir. Azure SQL veritabanı, işlem hattı ve Oozie meta veri deposu tarafından oluşturulan hem Özet verilerini depolar.

#### <a name="provision-azure-sql-database"></a>Azure SQL veritabanı sağlama

1. Adlı yeni bir kaynak grubu oluşturma Azure portalını kullanarak `oozie` Bu örnek tarafından kullanılan tüm kaynakları içerecek.
2. İçinde `oozie` kaynak grubu, sağlama bir Azure SQL sunucusu ve veritabanı. Bir veritabanının standart S1 fiyatlandırma katmanını daha büyük ihtiyacınız yoktur.
3. Azure portalını kullanarak, yeni dağıtılan SQL veritabanınızın bölmesine gidin ve seçin **Araçları**.

    ![Araçlar düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-tools.png)

4. Seçin **sorgu Düzenleyicisi**.

    ![Sorgu Düzenleyici düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-query-editor.png)

5. İçinde **sorgu Düzenleyicisi** bölmesinde **oturum açma**.

    ![Oturum açma düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-login1.png)

6. SQL veritabanı kimlik bilgilerinizi girin ve seçin **Tamam**.

   ![Oturum açma formu](./media/hdinsight-operationalize-data-pipeline/sql-db-login2.png)

7. Sorgu Düzenleyicisi metin alanında oluşturmak için aşağıdaki SQL deyimlerini girin `dailyflights` her çalıştırma Ardışık düzenin özetlenmiş veri depolayan bir tablo.

    ```
    CREATE TABLE dailyflights
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER CHAR(2),
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    GO

    CREATE CLUSTERED INDEX dailyflights_clustered_index on dailyflights(YEAR,MONTH,DAY_OF_MONTH,CARRIER)
    GO
    ```

8. Seçin **çalıştırma** SQL deyimlerini yürütmek için.

    ![Çalıştırma düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-run.png)

Azure SQL veritabanı artık hazırdır.

#### <a name="provision-an-hdinsight-hadoop-cluster"></a>Bir HDInsight Hadoop kümesi sağlayın

1. Azure portalında **+ yeni** ve HDInsight arayın.
2. **Oluştur**’u seçin.
3. Temel bölmede, kümeniz için benzersiz bir ad belirtin ve Azure aboneliğinizi seçin.

    ![HDInsight küme adı ve abonelik](./media/hdinsight-operationalize-data-pipeline/hdi-name-sub.png)

4. İçinde **küme türü** bölmesinde **Hadoop** küme türü, **Linux** HDInsight küme en son sürümü ve işletim sistemi. Bırakın **küme katmanı** adresindeki **standart**.

    ![HDInsight küme türü](./media/hdinsight-operationalize-data-pipeline/hdi-cluster-type.png)

5. Seçin **seçin** küme türü seçiminizi uygulamak için.
6. Tamamlamak **Temelleri** oturum açma parolası sağlayarak ve seçme bölmesi, `oozie` kaynak grubunda listeden ve ardından **sonraki**.

    ![HDInsight temel bilgileri bölmesi](./media/hdinsight-operationalize-data-pipeline/hdi-basics.png)

7. İçinde **depolama** bölmesinde, birincil depolama türü için ayarlı bırakın **Azure depolama**seçin **Yeni Oluştur**, yeni hesap için bir ad sağlayın.

    ![HDInsight depolama hesabı ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-storage.png)

8. İçin **meta depo ayarları**altında **Hive için bir SQL veritabanı seçin**, daha önce oluşturduğunuz veritabanını seçin.

    ![HDInsight Hive meta depo ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-hive.png)

9. Seçin **SQL veritabanı kimlik doğrulaması**.

    ![HDInsight Hive meta veri deposu kimlik doğrulaması](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql.png)

10. SQL veritabanı kullanıcı adı ve parola girin ve seçin **seçin**. 

       ![HDInsight Hive meta veri deposu kimlik doğrulaması, oturum açma](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql-login.png)

11. Yeniden **meta depo ayarları** veritabanınızın Oozie meta verileri depolamak ve daha önce yaptığınız gibi kimlik doğrulaması bölmesinde seçin. 

       ![HDInsight meta depo ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-settings.png)

12. **İleri**’yi seçin.
13. Üzerinde **özeti** bölmesinde **Oluştur** kümenize dağıtmak için.

### <a name="verify-ssh-tunneling-setup"></a>SSH tünel oluşturma Kurulum doğrulayın

Düzenleyici ve iş akışı örnekleri durumunu görüntülemek için Oozie Web konsolunu kullanmak için HDInsight kümenize SSH tüneli ayarlayın. Daha fazla bilgi için [SSH tüneli](hdinsight-linux-ambari-ssh-tunnel.md).

> [!NOTE]  
> Chrome ile de kullanabileceğiniz [Foxy Proxy](https://getfoxyproxy.org/) SSH tüneli üzerinden kümenizin web kaynaklarına göz atmak için uzantı. Proxy konağı üzerinden tüm istekleri yapılandırmak `localhost` tünelin bağlantı 9876. Bu yaklaşım, Windows 10 üzerinde Bash olarak da bilinen, Linux için Windows alt sistemi ile uyumludur.

1. SSH tüneli kümenize açmak için aşağıdaki komutu çalıştırın:

    ```
    ssh -C2qTnNf -D 9876 sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
    ```

2. Tünel göz atarak, baş düğümünde ambarı'na giderek çalışır durumda olduğunu doğrulayın:

    http:\//headnodehost:8080

3. Erişim için **Oozie Web Konsolu** Ambari içinde arasından **Oozie**, **hızlı bağlantılar**ve ardından **Oozie Web Konsolu**.

### <a name="configure-hive"></a>Hive'ı yapılandırma

1. Bir ay için uçuş verileri içeren örnek bir CSV dosyası indirin. Kendi ZIP dosyasını indirin `2017-01-FlightData.zip` gelen [HDInsight GitHub deposu](https://github.com/hdinsight/hdinsight-dev-guide) ve CSV dosyasına sıkıştırmasını `2017-01-FlightData.csv`. 

2. HDInsight kümenize bağlı Azure depolama hesabına kadar bu CSV dosyasını kopyalayın ve yerleştirebilir `/example/data/flights` klasör.

SCP içinde kullanarak dosya kopyalamanız, `bash` kabuk oturumu.

1. SCP dosyaları yerel makinenizden HDInsight küme baş düğümünün yerel depolama alanına kopyalamak için kullanın.

    ```bash
    scp ./2017-01-FlightData.csv sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:2017-01-FlightData.csv
    ```

2. Baş düğüm yerel depolama alanınızdan Azure Depolama'ya dosya kopyalamak HDFS komutunu kullanın.

    ```bash
    hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
    ```

Örnek veriler artık kullanılabilir. Ancak, işlem hattı işlemek, bir gelen veriler için iki Hive tablolarını gerektirir (`rawFlights`) ve biri de özetlenmiş veri (`flights`). Bu tablo Ambari gibi oluşturun.

1. Ambari için http giderek oturum açın:\//headnodehost:8080.
2. Hizmetler listesinden seçin **Hive**.

    ![Ambari Hive'ı seçme](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Seçin **görünüme Git** yanında Hive görünümü 2.0 etiketi.

    ![Ambari Hive görünümünü seçme](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Oluşturmak için aşağıdaki deyimleri sorgu metin alanına yapıştırın `rawFlights` tablo. `rawFlights` Tablo içindeki CSV dosyaları için bir şema okuma sağlar `/example/data/flights` Azure depolama alanında bir klasör. 

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS rawflights (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    ) 
    LOCATION '/example/data/flights'
    ```

5. Seçin **yürütme** tablo oluşturun.

    ![Ambari, Hive sorgusu](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-query.png)

6. Oluşturulacak `flights` tablo, sorgu metin alanı içindeki metni aşağıdaki deyimleri ile değiştirin. `flights` İçine yıl, ay ve ayın günü tarafından yüklenen verileri bölümler bir yönetilen Hive tablosu bir tablodur. Bu tabloda bir satır uçuş başına kaynak verilerde mevcut en düşük ayrıntı düzeyi ile tüm geçmiş uçuş verileri içerir.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;

    CREATE TABLE flights
    (
        FL_DATE STRING,
        CARRIER STRING,
        FL_NUM STRING,
        ORIGIN STRING,
        DEST STRING,
        DEP_DELAY FLOAT,
        ARR_DELAY FLOAT,
        ACTUAL_ELAPSED_TIME FLOAT,
        DISTANCE FLOAT
    )
    PARTITIONED BY (YEAR INT, MONTH INT, DAY_OF_MONTH INT)
    ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
    WITH SERDEPROPERTIES 
    (
        "separatorChar" = ",",
        "quoteChar"     = "\""
    );
    ```

7. Seçin **yürütme** tablo oluşturun.

### <a name="create-the-oozie-workflow"></a>Oozie iş akışı oluşturma

İşlem hatları, verileri toplu işler halinde genellikle belirli bir zaman aralığına göre işleyin. Bu durumda, işlem hattı uçuş verileri günlük işler. Bu yaklaşım, günlük, haftalık, aylık veya yıllık gelmesi için giriş CSV dosyaları sağlar.

Örnek iş akışı, uçuş verileri gün güne göre başlıca üç adımda işler:

1. Tarafından temsil edilen kaynak CSV dosyasından o güne ait tarih aralığı için verileri ayıklamak için bir Hive sorgusu çalıştırmayı `rawFlights` tablo ve verilerin Ekle `flights` tablo.
2. Dinamik olarak bir hazırlama tablosuna gün ve operatör tarafından özetlenen uçuş verilerin bir kopyasını içerir gün için Hive oluşturmak için bir Hive sorgusu çalıştırın.
3. Tüm verileri Hive günlük hazırlama tablosunda hedefe kopyalamak için Apache Sqoop'u kullanma `dailyflights` Azure SQL veritabanı tablosunda. Sqoop kullanarak kaynak satırları Azure depolamada bulunan Hive tablosu verileri okur ve bunları JDBC bağlantı kullanarak SQL veritabanına yükler.

Bu üç adımı bir Oozie iş akışının tarafından düzenlenir. 

1. Bir sorgu dosyasını içinde oluşturmak `hive-load-flights-partition.hql`.

    ```
    SET hive.exec.dynamic.partition.mode=nonstrict;
    
    INSERT OVERWRITE TABLE flights
    PARTITION (YEAR, MONTH, DAY_OF_MONTH)
    SELECT  
        FL_DATE,
        CARRIER,
        FL_NUM,
        ORIGIN,
        DEST,
        DEP_DELAY,
        ARR_DELAY,
        ACTUAL_ELAPSED_TIME,
        DISTANCE,
        YEAR,
        MONTH,
        DAY_OF_MONTH
    FROM rawflights
    WHERE year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Oozie değişkenleri sözdizimini kullanın `${variableName}`. Bu değişkenler kümesinde `job.properties` dosya bir sonraki adımda açıklandığı gibi. Oozie zamanında gerçek değerlerle değiştirir.

2. Bir sorgu dosyasını içinde oluşturmak `hive-create-daily-summary-table.hql`.

    ```
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}
    (
        YEAR INT,
        MONTH INT,
        DAY_OF_MONTH INT,
        CARRIER STRING,
        AVG_DEP_DELAY FLOAT,
        AVG_ARR_DELAY FLOAT,
        TOTAL_DISTANCE FLOAT
    )
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName}
    SELECT  year, month, day_of_month, carrier, avg(dep_delay) avg_dep_delay, 
            avg(arr_delay) avg_arr_delay, sum(distance) total_distance 
    FROM flights
    GROUP BY year, month, day_of_month, carrier 
    HAVING year = ${year} AND month = ${month} AND day_of_month = ${day};
    ```

    Bu sorgu, bir hazırlama tablosuna yalnızca özet verileri depolamak için bir gün, operatörünüz tarafından göre günlük akışı uzaklık toplam ve ortalama gecikmeler hesaplar SELECT deyiminin not oluşturur. (HiveDataFolder değişkeni tarafından belirtilen yol) bilinen bir konumda depolanan bu tabloya eklenen verilere böylece kaynak olarak Sqoop için sonraki adımda kullanılabilir.

3. Sqoop aşağıdaki komutu çalıştırın.

    ```
    sqoop export --connect ${sqlDatabaseConnectionString} --table ${sqlDatabaseTableName} --export-dir ${hiveDataFolder} -m 1 --input-fields-terminated-by "\t"
    ```

Bu üç adımı adlı üç ayrı eylem aşağıdaki Oozie iş akışının dosyasında belirtilir `workflow.xml`.

```
<workflow-app name="loadflightstable" xmlns="uri:oozie:workflow:0.5">
    <start to = "RunHiveLoadFlightsScript"/>
    <action name="RunHiveLoadFlightsScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptLoadPartition}</script>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
        </hive>
        <ok to="RunHiveCreateDailyFlightTableScript"/>
        <error to="fail"/>
    </action>

    <action name="RunHiveCreateDailyFlightTableScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScriptCreateDailyTable}</script>
            <param>hiveTableName=${hiveDailyTableName}</param>
            <param>year=${year}</param>
            <param>month=${month}</param>
            <param>day=${day}</param>
            <param>hiveDataFolder=${hiveDataFolder}/${year}/${month}/${day}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
    </action>

    <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}/${year}/${month}/${day}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
    </action>
    <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
    </kill>
    <end name="end"/>
</workflow-app>
```

İki Hive sorguları, kendi yolundaki bir Azure Depolama tarafından erişilen ve kalan değişken değerleri aşağıdaki tarafından sağlanan `job.properties` dosya. Bu dosya iş akışının çalıştırılmasını 3 Ocak 2017 tarihinden için yapılandırır.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.wf.application.path=${appBase}/load_flights_by_day
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableName=dailyflights${year}${month}${day}
hiveDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/${year}/${month}/${day}
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights
year=2017
month=01
day=03
```

Aşağıdaki tabloda, özelliklerin her birini özetler ve kendi ortamınızı değerleri bulabileceğiniz gösterir.

| Özellik | Değer kaynağı |
| --- | --- |
| NameNode | Azure depolama kapsayıcısı için tam yolu, HDInsight kümenize eklenebilecek. |
| Jobtracker'a | İç ana bilgisayar adı active kümenin YARN için baş düğüm. Ambari giriş sayfasında, YARN hizmetler listesinden seçin, sonra etkin Kaynak Yöneticisi'ni seçin. URI ana bilgisayar adı, sayfanın en üstünde görüntülenir. Bağlantı noktası 8050 ekleyin. |
| queueName | Hive eylemleri zamanlarken kullanılan YARN Kuyruğun adı. Varsayılan olarak bırakın. |
| oozie.use.system.libpath | True olarak bırakın. |
| appBase | Oozie iş akışının ve destekleyen dosyaları burada dağıttığınız Azure Depolama'daki bir alt klasör yolu. |
| oozie.wf.application.path | Oozie iş akışının konumunu `workflow.xml` çalıştırılacak. |
| hiveScriptLoadPartition | Hive sorgu dosyasını Azure Depolama'daki yolu `hive-load-flights-partition.hql`. |
| hiveScriptCreateDailyTable | Hive sorgu dosyasını Azure Depolama'daki yolu `hive-create-daily-summary-table.hql`. |
| hiveDailyTableName | Hazırlama tablosu için kullanılacak dinamik olarak oluşturulan adı. |
| hiveDataFolder | Azure Depolama'da verileri hazırlama tablosu tarafından giden yol. |
| sqlDatabaseConnectionString | Azure SQL veritabanınızı JDBC söz dizimi bağlantı dizesi. |
| sqlDatabaseTableName | Azure SQL veritabanı'nın Özet satır içine eklenen tablonun adı. Olarak bırakın `dailyflights`. |
| yıl | Yıl bileşenini günün hangi uçuş için özetleri hesaplanır. Olduğu gibi bırakın. |
| ay | Ay bileşeninin günün hangi uçuş için özetleri hesaplanır. Olduğu gibi bırakın. |
| gün | Ayın günü bileşenini günün hangi uçuş için özetleri hesaplanır. Olduğu gibi bırakın. |

> [!NOTE]  
> Kopyanızı güncelleştirdiğinizden emin olun `job.properties` dağıtabilir ve Oozie iş akışınızı çalıştırma önce sizin ortamınıza özgü değerlerle dosya.

### <a name="deploy-and-run-the-oozie-workflow"></a>Dağıtma ve Oozie iş akışı çalıştırma

SCP bash oturumunuzdan, Oozie iş akışının dağıtmak için kullanın. (`workflow.xml`), Hive sorguları (`hive-load-flights-partition.hql` ve `hive-create-daily-summary-table.hql`) ve iş yapılandırması (`job.properties`).  Oozie, yalnızca içinde `job.properties` dosya baş düğümüne yerel depolama alanında bulunabilir. Diğer tüm dosyalar bu örneği Azure depolama, HDFS saklanmalıdır. İş akışı tarafından kullanılan Sqoop eylem baş düğümünden HDFS'ye kopyalanmalıdır, SQL veritabanı ile iletişim kurmak için JDBC sürücüsü bağlıdır.

1. Oluşturma `load_flights_by_day` baş düğümün yerel depolamadaki kullanıcının yol altındaki alt.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 'mkdir load_flights_by_day'

2. Geçerli dizindeki tüm dosyaları kopyalayın ( `workflow.xml` ve `job.properties` dosyaları) en fazla `load_flights_by_day` alt.

        scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:load_flights_by_day

3. SSH, baş düğümüne gidin `load_flights_by_day` klasör.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
        cd load_flights_by_day

4. HDFS için iş akışı dosyalarını kopyalayın.

        hdfs dfs -put ./* /oozie/load_flights_by_day

5. Kopyalama `sqljdbc41.jar` HDFS iş akışı klasörüne yerel baş düğümünden:

        hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /oozie/load_flights_by_day

6. İş akışı çalıştırın.

        oozie job -config job.properties -run

7. Oozie Web konsolunu kullanarak durumunu gözlemleyin. Ambari içinde arasından **Oozie**, **hızlı bağlantılar**, ardından **Oozie Web Konsolu**. Altında **iş akışı işleri** sekmesinde **tüm işleri**.

    ![Oozie Web konsolu iş akışları](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

8. Durum başarılı olduğunda, eklenen satırları görüntülemek için SQL veritabanı tablosunu sorgulayın. Azure portalını kullanarak, select, SQL veritabanı için bölmesine gidin **Araçları**açın **sorgu Düzenleyicisi**.

        SELECT * FROM dailyflights

Tek bir test güne ait iş akışını çalıştıran, bu iş akışı her gün çalışır, böylece iş akışını zamanlar bir düzenleyici ile sarabilirsiniz.

### <a name="run-the-workflow-with-a-coordinator"></a>Bir düzenleyici ile iş akışı çalıştırma

Günlük (veya bir tarih aralığındaki tüm gün) çalışır, böylece bu iş akışı zamanlamak için bir düzenleyici kullanabilirsiniz. Bir düzenleyici gibi bir XML dosyası tarafından tanımlanan `coordinator.xml`:

```
<coordinator-app name="daily_export" start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" timezone="UTC" xmlns="uri:oozie:coordinator:0.4">
    <datasets>
        <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
            <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
            <done-flag></done-flag>
        </dataset>
    </datasets>
    <input-events>
        <data-in name="event_input1" dataset="ds_input1">
            <instance>${coord:current(0)}</instance>
        </data-in>
    </input-events>
    <action>
        <workflow>
            <app-path>${appBase}/load_flights_by_day</app-path>
            <configuration>
                <property>
                    <name>year</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'yyyy')}</value>
                </property>
                <property>
                    <name>month</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'MM')}</value>
                </property>
                <property>
                    <name>day</name>
                    <value>${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveScriptLoadPartition</name>
                    <value>${hiveScriptLoadPartition}</value>
                </property>
                <property>
                    <name>hiveScriptCreateDailyTable</name>
                    <value>${hiveScriptCreateDailyTable}</value>
                </property>
                <property>
                    <name>hiveDailyTableNamePrefix</name>
                    <value>${hiveDailyTableNamePrefix}</value>
                </property>
                <property>
                    <name>hiveDailyTableName</name>
                    <value>${hiveDailyTableNamePrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}${coord:formatTime(coord:nominalTime(), 'MM')}${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>hiveDataFolderPrefix</name>
                    <value>${hiveDataFolderPrefix}</value>
                </property>
                <property>
                    <name>hiveDataFolder</name>
                    <value>${hiveDataFolderPrefix}${coord:formatTime(coord:nominalTime(), 'yyyy')}/${coord:formatTime(coord:nominalTime(), 'MM')}/${coord:formatTime(coord:nominalTime(), 'dd')}</value>
                </property>
                <property>
                    <name>sqlDatabaseConnectionString</name>
                    <value>${sqlDatabaseConnectionString}</value>
                </property>
                <property>
                    <name>sqlDatabaseTableName</name>
                    <value>${sqlDatabaseTableName}</value>
                </property>
            </configuration>
        </workflow>
    </action>
</coordinator-app>
```

Gördüğünüz gibi Düzenleyici çoğunu yalnızca yapılandırma bilgileri için iş akışı örneği geçirme. Ancak, duyurmak için birkaç önemli öğe yok.

* 1\. noktası: `start` Ve `end` üzerinde öznitelikleri `coordinator-app` öğenin kendisinin denetim Düzenleyici üzerinde çalıştığı zaman aralığı.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Bir düzenleyici içinde eylemleri zamanlama için sorumlu `start` ve `end` tarafından belirtilen aralığa göre tarih aralığı, `frequency` özniteliği. Zamanlanmış her eylem iş akışı yapılandırılmış gibi sırayla çalışır. Yukarıdaki Düzenleyicisi tanımında Düzenleyici Eylemler 1 Ocak 2017'den 5 Ocak 2017'ye çalıştıracak şekilde yapılandırılır. Sıklığı tarafından 1 güne ayarlanır [Oozie ifade dili](https://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) sıklığı ifade `${coord:days(1)}`. Bu eylem zamanlama Düzenleyicisi'nde sonuçlanır (ve bu nedenle iş akışı) günde bir kez. Bu örnekte olduğu gibi daha önce olan tarih aralıkları için eylem gecikme olmadan çalıştırmak için zamanlanır. İçinden bir eylem çalışmak üzere zamanlandığı tarih başlangıcı adlı *nominal zaman*. Örneğin, 1 Ocak 2017'ye genel bakış için verileri işlemek için düzenleyici 2017 nominal süresine sahip eylem zamanlayacak-01-01T00:00:00 GMT.

* Noktası 2: İş akışı tarih aralığı içinde `dataset` öğesi HDFS'deki belirli bir tarih aralığı için veri aramak konumu belirtir ve nasıl Oozie verilerin kullanılabilir olup olmadığını belirler. yapılandırır henüz işleme için.

    ```
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    HDFS'deki veriler yolunu sağlanan ifadeye göre dinamik olarak oluşturulan `uri-template` öğesi. Bu Düzenleyici içinde bir gün sıklığını da veri kümesiyle birlikte kullanılır. Eylemler, zamanlanmış (ve nominal süreleri tanımlar olduğunda) Düzenleyici öğesi denetiminde başlangıç ve bitiş tarihlerini while `initial-instance` ve `frequency` hesaplaması oluşturulmasındakullanılantarihiverikümesiüzerindedenetim`uri-template`. Emin olmak için düzenleyici başlamadan önce bir gün için ayarlanan ilk örneği bu durumda kullanıcının (1/1/2017) değerinde veri ilk gününü seçer. Veri kümesinin tarih hesaplama İleri değerinden yapar `initial-instance` (12/31/2016) veri kümesi sıklığı (nominal saati geçmez en son tarihi bulana kadar 1 gün) artışlarla ilerledikten düzenleyici tarafından ayarlayın (2017-01-01T00:00:00 GMT ilk eylemi için).

    Boş `done-flag` öğesi Oozie kaldırmasını anda giriş veri varlığını denetlediğinde, Oozie veri kullanılabilir olup olmadığını bir dizin veya dosya varlığını tarafından belirlediğini gösterir. Bu durumda bir csv dosyasının varlığını olur. Bir csv dosyası varsa, Oozie varsayar: veri hazırdır ve dosyasını işlemek için bir iş akışı örneği başlatır. Mevcut herhangi bir csv dosyası varsa, Oozie değil hazır ve iş akışı çalıştıran gider henüz bir bekleme durumuna veri olduğunu varsayar.

* 3\. noktası: `data-in` Öğesi belirtir nominal kullanılacak belirli zaman damgası saat değerleri değiştirirken `uri-template` ilişkili veri kümesi için.

    ```
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    Bu durumda, ifade kümesi örneği `${coord:current(0)}`, özgün olarak düzenleyici tarafından zamanlanmış eylemin nominal zaman kullanmaya çevirir. Düzenleyici ile 01/01/2017'in nominal bir kez çalıştırılacak eylemi zamanlarken, diğer bir deyişle, ardından 01/01/2017 ne (2017) yıl ve ay (01) değişkenlerinde URI şablonu değiştirmek için kullanılır. URI şablonu, bu örneği için hesaplanan sonra Oozie beklenen dizin veya dosya kullanılabilir iş akışı sonraki çalıştırma uygun şekilde zamanlar olup olmadığını denetler ve.

Düzenleyici kaynak verileri güne göre günlük bir biçimde işlenmesini olduğu zamanlar bir durum elde etmek üzere önceki üç noktanın birleştirin. 

* 1\. noktası: Düzenleyici, 2017-01-01 nominal tarihi ile başlar.

* Noktası 2: Veri arar Oozie `sourceDataFolder/2017-01-FlightData.csv`.

* 3\. noktası: Oozie bu dosyayı bulduğunda, 2017-01-01 için veri işleme iş akışı örneği zamanlar. Oozie ardından 2017-01-02 işlenmeye devam eder. Bu değerlendirme kadar ancak 2017-01-05 içermeyen tekrarlar.

İş akışları ile bir düzenleyici yapılandırmasını sınıfında tanımlandığı gibi bir `job.properties` bir üst kümesi olan dosya, iş akışı tarafından kullanılan ayarları.

```
nameNode=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net
jobTracker=hn0-[CLUSTERNAME].[UNIQUESTRING].dx.internal.cloudapp.net:8050
queueName=default
oozie.use.system.libpath=true
appBase=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie
oozie.coord.application.path=${appBase}
sourceDataFolder=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/
hiveScriptLoadPartition=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-load-flights-partition.hql
hiveScriptCreateDailyTable=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/oozie/load_flights_by_day/hive-create-daily-summary-table.hql
hiveDailyTableNamePrefix=dailyflights
hiveDataFolderPrefix=wasbs://[CONTAINERNAME]@[ACCOUNTNAME].blob.core.windows.net/example/data/flights/day/
sqlDatabaseConnectionString="jdbc:sqlserver://[SERVERNAME].database.windows.net;user=[USERNAME];password=[PASSWORD];database=[DATABASENAME]"
sqlDatabaseTableName=dailyflights

```

Bu yalnızca yeni özellikleri `job.properties` dosyası:

| Özellik | Değer kaynağı |
| --- | --- |
| oozie.coord.application.path | Konumunu gösteren `coordinator.xml` çalıştırılacak Oozie Düzenleyici içeren dosya. |
| hiveDailyTableNamePrefix | Hazırlama tablosunun tablo adını dinamik olarak oluştururken kullanılan önek. |
| hiveDataFolderPrefix | Tüm hazırlama tablolarını depolanacağı yol ön eki. |

### <a name="deploy-and-run-the-oozie-coordinator"></a>Dağıtın ve Oozie düzenleyicisini çalıştırın

İş akışınızı içeren klasöre bir düzeyin bir klasörden iş dışında bir düzenleyici ile işlem hattı çalıştırma, iş akışı için benzer bir şekilde devam edin. Bir düzenleyici farklı alt iş akışlarıyla ilişkilendirebilmeniz için bu klasörü kuralı düzenleyicileri diskte, iş akışlarını ayırır.

1. SCP yerel makinenizden yerel depolama kümenizin baş düğümün kadar Düzenleyicisi dosyaları kopyalamak için kullanın.

    ```bash
    scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:~
    ```

2. Baş düğümüne SSH uygulayın.

    ```bash
    ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 
    ```

3. HDFS'ye Düzenleyicisi dosyaları kopyalayın.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. Düzenleyici çalıştırın.

    ```bash
    oozie job -config job.properties -run
    ```

5. Oozie Web konsolunu kullanarak durumunu doğrulamak seçerek bu zaman **Düzenleyicisi işleri** sekmesini ve ardından **tüm işleri**.

    ![Oozie Web Konsolu Düzenleyicisi işleri](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Zamanlanmış Eylemler listesini görüntülemek için bir düzenleyici örneği seçin. Bu durumda, 1/4/2017-1/1/2017 aralığında nominal sürelerine sahip dört eylemleri görmeniz gerekir.

    ![Oozie Web Konsolu Düzenleyicisi işi](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Bu listedeki her eylem, bir günün değerinde veri, o günün başlangıcını nominal zaman burada gösterilen işleyen iş akışı örneğine karşılık gelir.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Oozie belgeleri](https://oozie.apache.org/docs/4.2.0/index.html)

<!-- * Build the same pipeline [using Azure Data Factory](tbd.md).  -->
