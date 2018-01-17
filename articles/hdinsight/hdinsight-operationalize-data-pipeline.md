---
title: "Azure veri analizi ardışık - faaliyete | Microsoft Docs"
description: "Ayarlama ve yeni veri tarafından tetiklenen kısa sonuçları üreten bir örnek veri ardışık düzeni ve çalıştırın."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 7a439c9d25a470a2474b427f6b20addb6ff3b0c7
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="operationalize-a-data-analytics-pipeline"></a>Veri analizi ardışık faaliyete

*Veri ardışık* underly birçok veri analizi çözümleri. Adı da anlaşılacağı gibi veri ardışık ham verileri alır, temizler ve gerektiği gibi yeniden şekillendirir ve ardından genellikle hesaplamalar veya toplamalar işlenen veri depolamadan önce gerçekleştirir. İşlenen verilerin istemcileri, raporları veya API'ler tarafından kullanılır. Veri ardışık olup olmadığını bir zamanlamaya göre veya yeni verilerle tetiklendiğinde tekrarlanabilir sonuçlar sağlamanız gerekir.

Bu makalede, veri işlem hatlarınızı Yinelenebilirlik, Hdınsight Hadoop kümeleri üzerinde çalışan Oozie kullanarak için faaliyete açıklar. Örnek senaryo hazırlar ve uçak uçuş time series verilerini işleyen bir veri kanalı yoluyla anlatılmaktadır.

Aşağıdaki senaryoda, girdi verileri bir ay süreyle uçuş veri içeren bir düz bir dosyadır. Bu uçuş veri kaynağı ve hedef Havaalanı, beklemektedir mil, ayrılma ve varış kez ve diğerleri gibi bilgileri içerir. Bu ardışık düzen ile günlük uçak performans özetlemek için her hava yolu bir satır dakika ve o gün beklemektedir toplam mil ortalama ayrılma ve varış gecikmelerini ile her gün için sahip olduğu hedeftir.

| YIL | AY | DAY_OF_MONTH | TAŞIYICI |AVG_DEP_DELAY | AVG_ARR_DELAY |TOTAL_DISTANCE |
| --- | --- | --- | --- | --- | --- | --- |
| 2017 | 1 | 3 | AA | 10.142229 | 7.862926 | 2644539 |
| 2017 | 1 | 3 | AS | 9.435449 | 5.482143 | 572289 |
| 2017 | 1 | 3 | DL | 6.935409 | -2.1893024 | 1909696 |

Örnek potansiyel bir yeni süre 's uçuş veri ulaştığında, ardından Hive veri ambarınıza yönelik uzun vadeli çözümlemeler ayrıntılı uçuş bilgileri depolar bekler. Ardışık Düzen da yalnızca günlük uçuş verileri özetleyen bir çok daha küçük veri kümesi oluşturur. Bu günlük uçuş özet verileri için bir Web sitesi gibi raporlar sağlamak için bir SQL veritabanına gönderilir.

Aşağıdaki diyagram, örnek ardışık düzen gösterir.

![Uçuş veri ardışık](./media/hdinsight-operationalize-data-pipeline/pipeline-overview.png)

## <a name="oozie-solution-overview"></a>Oozie çözümüne genel bakış

Bu ardışık düzen Apache Oozie bir Hdınsight Hadoop kümesinde çalışan kullanır.

Oozie açıklar, ardışık düzen cinsinden *Eylemler*, *iş akışları*, ve *düzenleyiciler*. Asıl işi gerçekleştirmek, Hive sorgusu çalıştırmak gibi eylemleri belirler. İş akışları eylemlerin sırasını tanımlar. Düzenleyiciler iş akışının ne zaman çalıştırılması için zamanlamayı tanımlayın. Düzenleyiciler, iş akışı örneği başlatmadan önce yeni veri kullanılabilirliğini de bekleyebilirsiniz.

Aşağıdaki diyagramda bu örnek Oozie ardışık düzeninin üst düzey tasarımı gösterir.

![Oozie uçuş veri kanalı](./media/hdinsight-operationalize-data-pipeline/pipeline-overview-oozie.png)

### <a name="provision-azure-resources"></a>Azure kaynaklarını hazırlama

Bu ardışık düzen bir Azure SQL Database ve bir Hdınsight Hadoop kümesi ile aynı konumda gerektirir. Azure SQL veritabanı ardışık düzen ve Oozie meta veri deposu tarafından üretilen hem Özet verileri depolar.

#### <a name="provision-azure-sql-database"></a>Sağlama Azure SQL veritabanı

1. Azure portalını kullanarak oluşturduğunuz adlı yeni bir kaynak grubu `oozie` Bu örnek tarafından kullanılan tüm kaynakları içerecek şekilde.
2. İçinde `oozie` kaynak grubu, sağlama bir Azure SQL Server ve veritabanı. Bir veritabanı S1 standart fiyatlandırma katmanı büyük gerekmez.
3. Azure Portalı'nı kullanarak, yeni dağıtılan SQL veritabanınız için bölmesine gidin ve seçin **Araçları**.

    ![Araçlar düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-tools.png)

4. Seçin **sorgu Düzenleyicisi'ni**.

    ![Sorgu Düzenleyici düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-query-editor.png)

5. İçinde **sorgu Düzenleyicisi'ni** bölmesinde, **oturum açma**.

    ![Oturum açma düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-login1.png)

6. SQL veritabanı kimlik bilgilerinizi girin ve **Tamam**.

   ![Oturum açma formu](./media/hdinsight-operationalize-data-pipeline/sql-db-login2.png)

7. Sorgu Düzenleyici metin alanı oluşturmak için aşağıdaki SQL deyimlerini girin `dailyflights` her çalıştırma ardışık özetlenen verileri depolayacak tablosunu.

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

8. Seçin **çalıştırmak** SQL deyimlerini yürütmek için.

    ![Çalıştır düğmesi](./media/hdinsight-operationalize-data-pipeline/sql-db-run.png)

Azure SQL veritabanınızı artık hazırdır.

#### <a name="provision-an-hdinsight-hadoop-cluster"></a>Hdınsight Hadoop kümesi sağlama

1. Azure portalında seçin **+ yeni** ve Hdınsight arayın.
2. **Oluştur**’u seçin.
3. Temel kavramları bölmede, kümeniz için benzersiz bir ad sağlayın ve Azure aboneliğinizi seçin.

    ![Hdınsight küme adı ve abonelik](./media/hdinsight-operationalize-data-pipeline/hdi-name-sub.png)

4. İçinde **küme türü** bölmesinde, **Hadoop** küme türü, **Linux** işletim sistemi ve en son sürümünü Hdınsight kümesi. Bırakın **küme katmanı** adresindeki **standart**.

    ![HDInsight küme türü](./media/hdinsight-operationalize-data-pipeline/hdi-cluster-type.png)

5. Seçin **seçin** küme türü seçiminiz uygulamak için.
6. Tamamlamak **Temelleri** bir oturum açma parolası sağlama ve seçerek bölmesi, `oozie` kaynak grubu listeden ve sonra seçin **sonraki**.

    ![Hdınsight temelleri bölmesi](./media/hdinsight-operationalize-data-pipeline/hdi-basics.png)

7. İçinde **depolama** bölmesinde, birincil depolama türü için ayarlı bırakın **Azure Storage**seçin **Yeni Oluştur**ve yeni hesabı için bir ad sağlayın.

    ![Hdınsight depolama hesabı ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-storage.png)

8. İçin **meta depo ayarları**altında **bir SQL veritabanı için Hive seçin**, daha önce oluşturduğunuz veritabanını seçin.

    ![Hdınsight Hive meta depo ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-hive.png)

9. Seçin **SQL veritabanı kimlik doğrulaması**.

    ![Hdınsight Hive meta depo kimlik doğrulaması](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql.png)

10. SQL veritabanı kullanıcı adı ve parola girin ve seçin **seçin**. 

       ![Hdınsight Hive meta depo oturum açma kimlik doğrulaması](./media/hdinsight-operationalize-data-pipeline/hdi-authenticate-sql-login.png)

11. Geri **meta depo ayarları** veritabanınız için Oozie meta verileri depolamak ve daha önce yaptığınız gibi kimlik doğrulaması bölmesinde seçin. 

       ![Hdınsight meta depo ayarları](./media/hdinsight-operationalize-data-pipeline/hdi-metastore-settings.png)

12. Seçin **sonraki**.
13. Üzerinde **Özet** bölmesinde, **oluşturma** kümenizi dağıtmak için.

### <a name="verify-ssh-tunneling-setup"></a>Kurulum tünel SSH doğrulayın

Düzenleyici ve iş akışı örnekleri durumunu görüntülemek için Oozie Web konsolunu kullanmak için bir SSH tüneli Hdınsight kümenize ayarlayın. Daha fazla bilgi için bkz: [SSH tünel](hdinsight-linux-ambari-ssh-tunnel.md).

> [!NOTE]
> Chrome de kullanabilirsiniz [Foxy Proxy](https://getfoxyproxy.org/) uzantısı kümenizin web kaynakları SSH tüneli üzerinden göz atın. Ana bilgisayar üzerinden tüm istek için proxy yapılandırma `localhost` tünelin bağlantı noktası 9876. Bu yaklaşım, Linux, Windows 10 Bash olarak da bilinen Windows alt sistemi ile uyumludur.

1. SSH tüneli kümenize açmak için aşağıdaki komutu çalıştırın:

    ```
    ssh -C2qTnNf -D 9876 sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
    ```

2. Göz atarak baş düğümünde ambarı'na giderek tünel çalışır durumda olduğunu doğrulayın:

    http://headnodehost:8080

3. Erişim için **Oozie Web Konsolu** Ambari içinde seçin **Oozie**, **hızlı bağlantılar**ve ardından **Oozie Web Konsolu**.

### <a name="configure-hive"></a>Hive yapılandırma

1. Bir ay süreyle uçuş veri içeren bir örnek CSV dosyasını indirin. ZIP dosyası karşıdan `2017-01-FlightData.zip` gelen [Hdınsight Github deposunu](https://github.com/hdinsight/hdinsight-dev-guide) ve CSV dosyasına sıkıştırmasını `2017-01-FlightData.csv`. 

2. Hdınsight kümenize bağlı Azure Storage hesabı kadar bu CSV dosyasını kopyalayın ve yerleştirileceği `/example/data/flights` klasör.

İçinde SCP'yi kullanarak dosya kopyalama, `bash` kabuk oturumu.

1. SCP dosyaları yerel makineden Hdınsight küme baş düğümüne yerel depolama alanına kopyalamak için kullanın.

    ```bash
    scp ./2017-01-FlightData.csv sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:2017-01-FlightData.csv
    ```

2. Azure depolama birimine baş düğüm yerel depolama biriminden dosya kopyalamak için HDFS komutunu kullanın.

    ```bash
    hdfs dfs -put ./2017-01-FlightData.csv /example/data/flights/2017-01-FlightData.csv
    ```

Örnek verileri kullanıma sunulmuştur. Ancak, ardışık düzen işleme, bir gelen veriler için iki Hive tablolarını gerektirir (`rawFlights`) özetlenen veriler için bir tane (`flights`). Bu tablolar Ambari içinde aşağıdaki gibi oluşturun.

1. Ambarı'na giderek oturum açma [http://headnodehost:8080](http://headnodehost:8080).
2. Hizmetler listesinden seçin **Hive**.

    ![İçinde Ambari Hive seçme](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive.png)

3. Seçin **görünüme Git** Hive görünümü 2.0 etiket yanındaki.

    ![İçinde Ambari Hive görünümünü seçme](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-summary.png)

4. Sorgu metni alanına oluşturmak için aşağıdaki ifadeleri yapıştırın `rawFlights` tablo. `rawFlights` Tablo içinde CSV dosyaları için bir şema üzerinde-okuma sağlar `/example/data/flights` Azure Storage klasöründe. 

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

5. Seçin **yürütme** tablo oluşturmak için.

    ![Ambari Hive sorgusu](./media/hdinsight-operationalize-data-pipeline/hdi-ambari-services-hive-query.png)

6. Oluşturmak için `flights` tablo, sorgu metin alanı metni aşağıdaki deyimleri ile değiştirin. `flights` Tablodur içine yıl, ay ve ayın günü tarafından yüklenen verileri bölümler Hive yönetilen tablosu. Bu tablo, en düşük ayrıntı düzeyi içeren mevcut bir satır uçuş başına kaynak verilerdeki tüm geçmiş uçuş veri içermez.

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

7. Seçin **yürütme** tablo oluşturmak için.

### <a name="create-the-oozie-workflow"></a>Oozie iş akışı oluşturma

Ardışık Düzen genellikle belirli bir zaman aralığına göre verileri toplu işler. Bu durumda, ardışık düzen günlük uçuş verileri işler. Bu yaklaşım, günlük, haftalık, aylık veya yıllık gelmesi için giriş CSV dosyaları sağlar.

Örnek iş akışı uçuş veri gün güne göre üç ana adımda işler:

1. Tarafından temsil edilen kaynak CSV dosyasından günün tarih aralığı için veri ayıklamak için bir Hive sorgusu çalıştırmak `rawFlights` tablo ve verileri Ekle `flights` tablo.
2. Dinamik olarak gün ve taşıyıcı tarafından özetlenen uçuş verilerin bir kopyasını içeren gün için kovanında hazırlama tablosu oluşturmak için bir Hive sorgusu çalıştırmak.
3. Tüm verileri Hive günlük hazırlama tablosunda hedefe kopyalamak için Apache Sqoop'u kullanın `dailyflights` Azure SQL veritabanı tablosunda. Sqoop kaynak satırları Azure depolama alanında bulunan Hive tablosu ardındaki verileri okur ve onları bir JDBC bağlantısı kullanarak SQL veritabanına yükler.

Bu üç adımı Oozie iş akışı tarafından düzenlenir. 

1. Dosyada bir sorgu oluşturun `hive-load-flights-partition.hql`.

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

    Oozie değişkenleri sözdizimini kullanın `${variableName}`. Bu değişkenler kümesinde `job.properties` dosya bir sonraki adımda açıklandığı gibi. Oozie çalışma zamanında gerçek değerleri değiştirir.

2. Dosyada bir sorgu oluşturun `hive-create-daily-summary-table.hql`.

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

    Bu sorgu için bir gün yalnızca özetlenmiş veri depolamak, taşıyıcı tarafından güne göre beklemektedir uzaklığı toplamı ve ortalama gecikme hesaplar SELECT deyimi not edin hazırlama bir tablo oluşturur. (HiveDataFolder değişkeni tarafından belirtilen yol) bilinen bir konumda depolanan bu tabloya eklenen verilere böylece bu kaynak olarak Sqoop için sonraki adımda kullanılabilir.

3. Aşağıdaki Sqoop komutunu çalıştırın.

    ```
    sqoop export --connect ${sqlDatabaseConnectionString} --table ${sqlDatabaseTableName} --export-dir ${hiveDataFolder} -m 1 --input-fields-terminated-by "\t"
    ```

Bu üç adımı adında üç ayrı eylemler olarak aşağıdaki Oozie iş akışı dosyasında belirtilmiştir `workflow.xml`.

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

İki Hive sorguları Azure storage'da kendi yoluyla erişilir ve kalan değişken değerleri aşağıdaki tarafından sağlanan `job.properties` dosya. Bu dosya için 3 Ocak 2017 tarihi çalıştırmak için iş akışı yapılandırır.

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

Aşağıdaki tabloda her özellikleri özetler ve kendi ortamınız için değerleri bulabileceğiniz gösterir.

| Özellik | Değer kaynağı |
| --- | --- |
| nameNode | Azure depolama kapsayıcısı için tam yolu Hdınsight kümenize bağlı. |
| jobTracker | Etkin kümenizin YARN için iç hostname baş düğüm. Ambari giriş sayfasında YARN hizmetler listesinden seçin, sonra etkin Kaynak Yöneticisi'ni seçin. Ana bilgisayar adının URI, sayfanın en üstünde görüntülenir. Bağlantı noktası 8050 ekleyin. |
| queueName | Hive Eylemler zamanlarken kullanılan YARN sırasının adı. Varsayılan olarak bırakın. |
| oozie.use.system.libpath | True olarak bırakın. |
| Uygulama tabanının | Azure depolama burada Oozie iş akışı ve Destek dosyalarını dağıtmak alt klasör yolu. |
| oozie.wf.application.path | Oozie iş akışı konumunu `workflow.xml` çalıştırmak için. |
| hiveScriptLoadPartition | Azure storage'da Hive sorgusu dosyasının yolu `hive-load-flights-partition.hql`. |
| hiveScriptCreateDailyTable | Azure storage'da Hive sorgusu dosyasının yolu `hive-create-daily-summary-table.hql`. |
| hiveDailyTableName | Hazırlama tablosu için kullanılacak dinamik olarak üretilen adı. |
| hiveDataFolder | Hazırlama tablosu tarafından bulunan verileri Azure depolama yolu. |
| sqlDatabaseConnectionString | Azure SQL veritabanınıza JDBC sözdizimi bağlantı dizesi. |
| sqlDatabaseTableName | Özet satırları içine eklenen Azure SQL veritabanı tablosunun adı. Olarak bırakın `dailyflights`. |
| yıl | Yıl bileşenini günün hangi uçuş için özetleri hesaplanır. Olduğu gibi bırakın. |
| ay | Ay bileşenini günün hangi uçuş için özetleri hesaplanır. Olduğu gibi bırakın. |
| gün | Ay bileşenini için hangi uçuş özetleri hesaplanan günün günü. Olduğu gibi bırakın. |

> [!NOTE]
> Kopyanızı güncelleştirdiğinizden emin olun `job.properties` dağıtmak ve Oozie iş akışınızı çalıştırmak önce ortamınız için belirli değerleri içeren dosya.

### <a name="deploy-and-run-the-oozie-workflow"></a>Dağıtma ve Oozie iş akışını çalıştırma

Oozie akışınızı dağıtmak için bash oturumunuzda SCP kullanın (`workflow.xml`), Hive sorguları (`hive-load-flights-partition.hql` ve `hive-create-daily-summary-table.hql`) ve iş yapılandırma (`job.properties`).  Oozie, yalnızca ın `job.properties` dosya headnode yerel depolama alanında bulunabilir. Diğer tüm dosyalar bu servis talebi Azure storage'da HDFS saklanmalıdır. İş akışı tarafından kullanılan Sqoop eylem baş düğümünden HDFS için kopyalanan gerekir, SQL Database, iletişim için bir JDBC sürücüsü bağlıdır.

1. Oluşturma `load_flights_by_day` alt baş düğüm yerel depolama kullanıcının yolunda altında.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 'mkdir load_flights_by_day'

2. Geçerli dizindeki tüm dosyaları kopyalayın ( `workflow.xml` ve `job.properties` dosyaları) en fazla `load_flights_by_day` alt klasörü.

        scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:load_flights_by_day

3. Baş düğüm içine SSH gidin `load_flights_by_day` klasör.

        ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net
        cd load_flights_by_day

4. HDFS için iş akışı dosyalarını kopyalayın.

        hdfs dfs -put ./* /oozie/load_flights_by_day

5. Kopya `sqljdbc41.jar` HDFS iş akışı klasörüne yerel baş düğümünden:

        hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /oozie/load_flights_by_day

6. İş akışını çalıştırmak.

        oozie job -config job.properties -run

7. Oozie Web Konsolu kullanarak durumunu inceleyin. Ambari içinde seçin **Oozie**, **hızlı bağlantılar**ve ardından **Oozie Web Konsolu**. Altında **iş akışı işleri** sekmesine **tüm işleri**.

    ![Oozie Web konsolu iş akışları](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-workflows.png)

8. Durumu başarılı olduğunda, SQL veritabanı tablosuna eklenen satırları görüntülemek için sorgu. Azure portal'ı kullanarak, seçin, SQL veritabanınızın bölmesine gidin **Araçları**, açarak **sorgu Düzenleyicisi'ni**.

        SELECT * FROM dailyflights

İş akışı için tek bir test gün çalışıyor, bu iş akışı her gün çalışır, böylece iş akışı zamanlar düzenleyicisiyle kayabilir.

### <a name="run-the-workflow-with-a-coordinator"></a>Bir düzenleyici ile iş akışını çalıştırma

Günlük (veya bir tarih aralığındaki tüm gün) çalışır, bu iş akışı zamanlamak için bir düzenleyici kullanabilirsiniz. Bir düzenleyici örneğin bir XML dosyası tarafından tanımlanan `coordinator.xml`:

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

Gördüğünüz gibi Düzenleyici çoğunluğu yalnızca yapılandırma bilgileri için iş akışı örneği geçirme. Ancak, duyurmak için birkaç önemli öğe yok.

* Nokta 1: `start` ve `end` üzerinde öznitelikleri `coordinator-app` öğenin kendisi kontrol Düzenleyici üzerinde çalıştığı zaman aralığı.

    ```
    <coordinator-app ... start="2017-01-01T00:00Z" end="2017-01-05T00:00Z" frequency="${coord:days(1)}" ...>
    ```

    Bir düzenleyici içinde eylemler zamanlama için sorumludur `start` ve `end` tarafından belirtilen aralığa göre tarih aralığı, `frequency` özniteliği. Zamanlanmış her eylem iş akışı yapılandırıldığı gibi sırayla çalıştırır. Yukarıdaki Düzenleyicisi tanımında Düzenleyici 1 Ocak 2017 ' 5 Ocak 2017 için eylemler çalıştıracak şekilde yapılandırılır. Sıklık 1 gün ayarlanır [Oozie ifade dili](http://oozie.apache.org/docs/4.2.0/CoordinatorFunctionalSpec.html#a4.4._Frequency_and_Time-Period_Representation) sıklığı ifade `${coord:days(1)}`. Bu eylem zamanlama Düzenleyici olur (ve dolayısıyla iş akışı) günde bir kez. Bu örnekte olduğu gibi geçmişte olan tarih aralıkları için eylem gecikme olmadan çalıştırmak için zamanlanacak. İçinden bir eylem çalıştırılmak üzere zamanlandı tarih başlangıç adlı *nominal zaman*. Örneğin, 1 Ocak 2017 için verileri işlemek için düzenleyici 2017 nominal süresi olan eylem zamanlar-01-01T00:00:00 GMT.

* Nokta 2: iş akışı tarih aralığı içinde `dataset` öğesi veri belirli bir tarih aralığında HDFS'de aramak konumu belirtir ve nasıl Oozie verileri kullanılabilir olup olmadığını belirler yapılandırır henüz işleme için.

    ```
    <dataset name="ds_input1" frequency="${coord:days(1)}" initial-instance="2016-12-31T00:00Z" timezone="UTC">
        <uri-template>${sourceDataFolder}${YEAR}-${MONTH}-FlightData.csv</uri-template>
        <done-flag></done-flag>
    </dataset>
    ```

    Yolun HDFS'deki veriler için dinamik olarak sağlanan ifadeye göre yerleşik `uri-template` öğesi. Bu Düzenleyicisi'nde bir gün sıklığını ayrıca veri kümesi ile kullanılır. Eylemler zamanlanan (ve nominal zamanları tanımlar olduğunda) başlangıç ve bitiş tarihleri Düzenleyicisi öğesi denetimindeki while `initial-instance` ve `frequency` veri kümesine hesaplaması oluştururkenkullanılantarihikontrol`uri-template`. Bu durumda, emin olmak için düzenleyici başlamadan önce bir gün için ilk örneğini ayarlamak veri (1/1/2017) tutarında kullanıcının ilk gününü seçer. Veri kümesi'nin tarih hesaplaması İleri değerinden yapar `initial-instance` (31/12/2016) veri kümesi sıklığı (nominal zaman iletmez en son tarih bulana kadar 1 gün) artışlarla ilerledikten düzenleyici tarafından ayarlayın (2017-01-01T00:00:00 GMT ilk eylem için).

    Boş `done-flag` öğesi gösterir Oozie kaldırmasını aynı anda giriş veri varlığını denetlediğinde, Oozie veri kullanılabilir olup olmadığını bir dizin veya dosya varlığını tarafından belirler. Bu durumda bir csv dosyası varlığını olur. Bir csv dosyası varsa, Oozie varsayar: veri hazır ve dosyayı işlemek için bir iş akışı örneği başlatır. Mevcut herhangi bir csv dosyası varsa, Oozie değil hazır ve iş akışı çalıştıran gider henüz bekleme durumuna veri olduğunu varsayar.

* Noktası 3: `data-in` öğesi belirttiğinden nominal kullanılacak belirli zaman damgası saat değerlerini değiştirirken `uri-template` ilişkili veri kümesi için.

    ```
    <data-in name="event_input1" dataset="ds_input1">
        <instance>${coord:current(0)}</instance>
    </data-in>
    ```

    Bu durumda, ifade ayarlayın `${coord:current(0)}`, düzenleyici tarafından zamanlanan olarak özgün eylem nominal süresini kullanmaya çevirir. Düzenleyici nominal 01/01/2017 süresiyle çalıştırmak için eylem zamanlarken, diğer bir deyişle, ardından 01/01/2017 ne (2017) yıl ve ay (01) değişken URI şablonunu değiştirmek için kullanılır. Bu örnek için URI şablonunu hesaplanan sonra Oozie beklenen dizin veya dosya kullanılabilir iş akışının sonraki çalıştırma uygun şekilde zamanlar olup olmadığını denetler ve.

Burada bir gün gün şekilde kaynak verilerinin işlenmesini Düzenleyici zamanlar bir durum elde etmek üzere önceki üç nokta birleştirin. 

* Nokta 1: Düzenleyici 2017-01-01 nominal bir tarih ile başlar.

* Nokta 2: Veri bulunan Oozie arar `sourceDataFolder/2017-01-FlightData.csv`.

* Noktası 3: Oozie bu dosyayı bulduğunda, bu verileri 2017-01-01 için işleyecek iş akışı örneği zamanlar. Oozie sonra 2017-01-02 işlenmeye devam eder. Bu değerlendirme 2017-01-05 dahil değil ancak kadar tekrarlar.

İş akışları ile bir düzenleyici yapılandırmasını tanımlandığı gibi bir `job.properties` bir alt kümesi olan dosya, iş akışı tarafından kullanılan ayarlar.

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

Bu konuda sunulan yalnızca yeni özellikler `job.properties` dosyası:

| Özellik | Değer kaynağı |
| --- | --- |
| oozie.coord.application.path | Konumunu belirten `coordinator.xml` Oozie Düzenleyicisi'ni çalıştırmak için içeren dosya. |
| hiveDailyTableNamePrefix | Hazırlama tablosunun tablo adı dinamik olarak oluşturulurken kullanılan öneki. |
| hiveDataFolderPrefix | Tüm hazırlama tablolarını depolanacağı yol öneki. |

### <a name="deploy-and-run-the-oozie-coordinator"></a>Dağıtma ve Oozie Düzenleyicisi'ni çalıştırma

İş akışınızı içeren klasörün üstünde bir düzey bir klasörden çalışma dışında ardışık düzen Düzenleyicisi ile çalıştırmak için iş akışı için benzer bir şekilde devam edin. Bir düzenleyici farklı alt iş akışları ile ilişkilendirmek için bu klasör kuralı düzenleyiciler iş akışları, diskteki ayırır.

1. SCP yerel makinenizden kümenizin baş düğüm yerel depolama kadar Düzenleyicisi dosyaları kopyalamak için kullanın.

    ```bash
    scp ./* sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net:~
    ```

2. Baş düğüm içine SSH.

    ```bash
    ssh sshuser@[CLUSTERNAME]-ssh.azurehdinsight.net 
    ```

3. Düzenleyici dosyaları için HDFS kopyalayın.

    ```bash
    hdfs dfs -put ./* /oozie/
    ```

4. Düzenleyici çalıştırın.

    ```bash
    oozie job -config job.properties -run
    ```

5. Oozie Web konsolunu kullanarak durumunu doğrulamak seçerek bu zaman **Düzenleyicisi işleri** sekmesini ve ardından **tüm işleri**.

    ![Oozie Web Konsolu Düzenleyicisi işleri](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-jobs.png)

6. Zamanlanmış eylemlerin listesini görüntülemek için bir düzenleyici örneği seçin. Bu durumda, dört eylem 4/1/2017-1/1/2017 aralığında nominal sürelerine sahip görmeniz gerekir.

    ![Oozie Web Konsolu Düzenleyicisi işi](./media/hdinsight-operationalize-data-pipeline/hdi-oozie-web-console-coordinator-instance.png)

    Bu listedeki her bir eylemde bu günün başlangıcını nominal zamanına göre burada gösterilen veriler, bir günün eşitleyeceğini işler iş akışı örneği karşılık gelir.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Oozie Documentation](http://oozie.apache.org/docs/4.2.0/index.html)

<!-- * Build the same pipeline [using Azure Data Factory](tbd.md).  -->
