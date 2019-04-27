---
title: Linux tabanlı Azure HDInsight, Hadoop Oozie iş akışları kullanın
description: Linux tabanlı HDInsight, Hadoop Oozie kullanma. Bir Oozie iş akışının tanımlayın ve Oozie işi gönderme hakkında bilgi edinin.
ms.service: hdinsight
ms.custom: hdinsightactive
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 02/28/2019
ms.openlocfilehash: daee7ddd0a09d43132bbcf0f4553601846d31433
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486796"
---
# <a name="use-apache-oozie-with-apache-hadoop-to-define-and-run-a-workflow-on-linux-based-azure-hdinsight"></a>Tanımlamak ve Linux tabanlı Azure HDInsight üzerinde bir iş akışı çalıştırmak için Apache Hadoop ile Apache Oozie kullanma

Azure HDInsight üzerinde Apache Hadoop ile Apache Oozie kullanmayı öğrenin. Oozie, Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile tümleştirilir ve aşağıdaki işler destekler:

* Apache Hadoop MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanabilirsiniz.

> [!NOTE]  
> HDInsight ile iş akışlarını tanımlamak için başka bir seçenek, Azure Data Factory kullanmaktır. Data Factory hakkında daha fazla bilgi için bkz: [kullanım Apache Pig ve Apache Hive ile veri fabrikası][azure-data-factory-pig-hive]. Lütfen kurumsal güvenlik paketi ile Oozie kümelerinde bakın kullanın [HDInsight Hadoop, Apache Oozie çalıştırın, Kurumsal güvenlik paketi ile kümeleri](domain-joined/hdinsight-use-oozie-domain-joined-clusters.md).


## <a name="prerequisites"></a>Önkoşullar

* **HDInsight Hadoop kümesinde**. Bkz: [Linux'ta HDInsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Bir SSH istemcisi**. Bkz: [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](hdinsight-hadoop-linux-use-ssh-unix.md).

* **Azure SQL Database**.  Bkz: [Azure portalında bir Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md).  Bu makalede adlı bir veritabanı kullanır `oozietest`.

* **Depolama yapılandırması için olası bir değişiklik.**  Bkz: [depolama yapılandırması](#storage-configuration) depolama hesabı türü kullanılıyorsa `BlobStorage`.

## <a name="storage-configuration"></a>Depolama yapılandırması
Kullanılan depolama hesabı türü ise Eylem gerekmiyor `Storage (general purpose v1)` veya `StorageV2 (general purpose v2)`.  Makaledeki işlemi çıkış için en az üretecektir `/mapreducestaging`.  Varsayılan hadoop yapılandırma içerecek `/mapreducestaging` içinde `fs.azure.page.blob.dir` yapılandırma değişkeni `core-site.xml` hizmeti `HDFS`.  Bu yapılandırma, depolama hesabı türü için desteklenmeyen sayfa blobları olarak dizine çıkış neden olacak `BlobStorage`.  Kullanılacak `BlobStorage` kaldırmak için bu makalede, `/mapreducestaging` gelen `fs.azure.page.blob.dir` yapılandırma değişkeni.  Yapılandırma erişilebilir [Ambari UI](hdinsight-hadoop-manage-ambari.md).  Aksi takdirde hata iletisi alırsınız: `Page blob is not supported for this account type.`

> [!NOTE]  
> Bu makalede kullanılan depolama hesabında [güvenli aktarım](../storage/common/storage-require-secure-transfer.md) etkin ve bu nedenle `wasbs` yerine `wasb` makale boyunca kullanılır.

## <a name="example-workflow"></a>Örnek iş akışı

Bu belgede kullanılan iş akışı iki eylemleri içerir. Hive, Sqoop, MapReduce veya diğer işlemleri çalıştırma gibi görevler için tanımları eylemler şunlardır:

![İş akışı diyagramı][img-workflow-diagram]

1. Bir Hive eylem kayıtlarından ayıklamak için bir HiveQL betiğini çalıştırır `hivesampletable` HDInsight ile eklendi. Her veri satırının belirli bir mobil CİHAZDAN ziyaret açıklar. Kayıt biçimi şu metin gibi görünür:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Bu belgede kullanılan Hive betik, Android veya iPhone, gibi her platform için toplam ziyaret sayısı ve sayıları yeni bir Hive tablosuna depolar.

    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma Apache][hdinsight-use-hive].

2. Sqoop eylemi, Azure SQL veritabanı'nda oluşturulan bir tablo için yeni Hive tablosu içeriği dışarı aktarır. Sqoop hakkında daha fazla bilgi için bkz: [HDInsight ile Apache Sqoop'u kullanma][hdinsight-use-sqoop].

> [!NOTE]  
> HDInsight kümelerinde desteklenen Oozie sürümleri için bkz: [HDInsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler][hdinsight-versions].

## <a name="create-the-working-directory"></a>Çalışma dizini oluşturma

Oozie ile aynı dizinde bir iş için gereken tüm kaynakları depolamak için bekliyor. Bu örnekte `wasbs:///tutorials/useoozie`. Bu dizini oluşturmak için aşağıdaki adımları tamamlayın:

1. Değiştirmek için aşağıdaki kodu düzenleme `sshuser` SSH kullanıcı adı küme için ile değiştirin `clustername` ile kümesinin adı.  Kod tarafından HDInsight kümesine bağlanmak için enter [SSH kullanarak](hdinsight-hadoop-linux-use-ssh-unix.md).  

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

2. Dizin oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/useoozie/data
    ```

    > [!NOTE]  
    > `-p` Parametresi tüm dizinleri oluşturulmasını yolda neden olur. `data` Dizin tarafından kullanılan verileri tutmak için kullanılan `useooziewf.hql` betiği.

3. Değiştirmek için aşağıdaki kodu düzenleme `username` SSH kullanıcı adınızla.  Oozie kullanıcı hesabınızın bürünebileceğini emin olmak için aşağıdaki komutu kullanın:

    ```bash
    sudo adduser username users
    ```

    > [!NOTE]  
    > Kullanıcı zaten bir üye gösteren hataları yoksayabilirsiniz `users` grubu.

## <a name="add-a-database-driver"></a>Bir veritabanı sürücü Ekle

Bu iş akışı, verileri SQL veritabanına dışarı aktarmak için Sqoop kullandığından, SQL veritabanı ile etkileşim kurmak için kullanılan JDBC sürücüsü bir kopyasını sağlamanız gerekir. JDBC sürücüsü çalışma dizinine kopyalamak için SSH oturumunda aşağıdaki komutu kullanın:

```bash
hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc*.jar /tutorials/useoozie/
```

> [!IMPORTANT]  
> Var. gerçek JDBC sürücüsü doğrulayın `/usr/share/java/`.

İş akışınızı bir MapReduce uygulamasını içeren bir jar gibi diğer kaynaklar kullandıysanız, bu kaynakları da eklemeniz gerekir.

## <a name="define-the-hive-query"></a>Hive sorgusunu tanımlama

Bir sorguyu tanımlayan bir Hive sorgu dili (HiveQL) betiği oluşturmak için aşağıdaki adımları kullanın. Sorgu bir Oozie iş akışının bu belgenin sonraki bölümlerinde kullanır.

1. SSH bağlantısından adlı bir dosya oluşturmak için aşağıdaki komutu kullanın. `useooziewf.hql`:

    ```bash
    nano useooziewf.hql
    ```

3. GNU nano Düzenleyici açıldığında, dosyanın içeriğini aşağıdaki sorguyu kullanın:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Betikte kullanılan iki değişken şunlardır:

   * `${hiveTableName}`: Oluşturulacak tablonun adını içerir.

   * `${hiveDataFolder}`: Tablo için veri dosyalarının depolanacağı konumu içerir.

     İş akışı tanım dosyası, bu öğreticideki workflow.xml bu HiveQL betiğini çalışma zamanında bu değerleri geçirir.

4. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.  

5. Kopyalamak için aşağıdaki komutu kullanın `useooziewf.hql` için `wasbs:///tutorials/useoozie/useooziewf.hql`:

    ```bash
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Bu komut depolar `useooziewf.hql` küme için HDFS uyumlu depolama dosyası.

## <a name="define-the-workflow"></a>İş akışı tanımlama

Oozie iş akışı tanımları Hadoop işlem tanımı bir XML işlem tanımı dili olan dilinde (hPDL) yazılır. İş akışını tanımlamak için aşağıdaki adımları kullanın:

1. Yeni bir dosya oluşturup aşağıdaki deyimi kullanın:

    ```bash
    nano workflow.xml
    ```

2. Nano Düzenleyici açıldıktan sonra aşağıdaki XML dosyanın içeriği girin:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
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
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
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

    İş akışında tanımlanan iki eylem vardır:

   * `RunHiveScript`: Bu eylem başlangıç eylemi ve çalışan `useooziewf.hql` Hive betiği.

   * `RunSqoopExport`: Bu eylem, Sqoop kullanarak bir SQL veritabanı'na Hive komut dosyasından oluşturulan veri dışarı aktarır. Bu eylem yalnızca çalışır `RunHiveScript` eylem başarılı olur.

     İş akışı gibi birden çok girişi olan `${jobTracker}`. İş tanımında kullandığınız değerlerle bu girişlerin yerini alır. Bu belgede daha sonra iş tanımı oluşturur.

     Ayrıca unutmayın `<archive>mssql-jdbc-7.0.0.jre8.jar</archive>` Sqoop bölümünde girişi. Bu giriş, bu eylem çalıştırıldığında bu arşiv için Sqoop kullanılabilmesi için Oozie bildirir.

3. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.  

4. Kopyalamak için aşağıdaki komutu kullanın `workflow.xml` dosyasını `/tutorials/useoozie/workflow.xml`:

    ```bash
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-a-table"></a>Bir tablo oluşturma

> [!NOTE]  
> Bir tablo oluşturmak için SQL veritabanı'na bağlamak için birçok yol vardır. Aşağıdaki adımlarda HDInsight kümesinden [FreeTDS](http://www.freetds.org/) kullanılır.

1. FreeTDS HDInsight kümesine yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Değiştirmek için aşağıdaki kodu düzenleme `<serverName>` ile Azure SQL sunucunuzun adını ve `<sqlLogin>` Azure SQL server oturum açma.  Önkoşul SQL veritabanına bağlanmak için komutu girin.  İsteminde bir parola girin.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    Aşağıdaki metni gibi bir çıktı alırsınız:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. `1>` isteminde aşağıdaki satırları girin:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    `GO` deyimi girildiğinde önceki deyimler değerlendirilir. Bu deyimler adlı bir tablo oluşturma `mobiledata`, iş akışı tarafından kullanılır.

    Tablo oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Aşağıdaki metne benzer bir çıktı görürsünüz:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo             mobiledata      BASE TABLE

4. Tsql yardımcı programı girerek çıkmak `exit` adresindeki `1>` istemi.

## <a name="create-the-job-definition"></a>İş tanımı oluşturma

İş tanımı workflow.xml nerede bulacağını açıklar. Ayrıca iş akışı tarafından kullanılan diğer dosyaları nerede bulacağını açıklar `useooziewf.hql`. Ayrıca, değerleri için iş akışı ve ilişkili dosyaları içinde kullanılan özellikleri tanımlar.

1. Varsayılan depolama alanı tam adresini almak için aşağıdaki komutu kullanın. Bu adres, sonraki adımda oluşturduğunuz yapılandırma dosyası kullanılır.

    ```bash
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Bu komut, bilgi aşağıdaki XML gibi döndürür:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]  
    > HDInsight küme varsayılan depolama alanı olarak Azure depolama kullanıyorsa `<value>` öğenin içeriği ile başlayan `wasbs://`. Azure Data Lake depolama Gen1 yerine kullanılıyorsa ile başlayan `adl://`. Azure Data Lake depolama Gen2 kullanılıyorsa ile başlayan `abfs://`.

    İçeriği Kaydet `<value>` öğesi, sonraki adımda kullanılır.

2. Aşağıdaki xml gibi düzenleyin:

    |Yer tutucu değeri| Değiştirilen değer|
    |---|---|
    |wasbs://mycontainer\@mystorageaccount.blob.core.windows.net| Adım 1'den alınan değer.|
    |Yönetici| Yönetici değilse HDInsight kümesi için oturum açma adınız|
    |SunucuAdı| Azure SQL veritabanı sunucu adı.|
    |Şirket içinde sqlLogin| Azure SQL veritabanı sunucusu oturum açma.|
    |sqlPassword| Azure SQL veritabanı sunucusu oturum açma parolası.|

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>headnodehost:8050</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=sqlLogin;password=sqlPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>admin</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

    Bu dosyadaki ilgili bilgilerin çoğunu workflow.xml veya ooziewf.hql dosyaları gibi kullanılan değerlerle doldurmak için kullanılır `${nameNode}`.  Yol ise bir `wasbs` yolu, tam yolu kullanmanız gerekir. Yalnızca kısaltın değil `wasbs:///`. `oozie.wf.application.path` Girdi workflow.xml dosyasının nerede bulacağını tanımlar. Bu dosya bu iş tarafından çalıştırılan bir iş akışı içerir.

3. Oozie iş tanımı yapılandırması oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano job.xml
    ```

4. Nano Düzenleyici açıldığında, dosyanın içeriğini düzenlenen XML yapıştırın.

5. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

## <a name="submit-and-manage-the-job"></a>Gönder ve iş yönetimi

Aşağıdaki adımları Oozie komutunu gönderin ve kümede Oozie iş akışları yönetmek için kullanın. Oozie kullanıcı dostu bir arabirim üzerinden komuttur [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]  
> Oozie komutunu kullandığınızda, HDInsight baş düğüm için FQDN kullanmanız gerekir. Bu FQDN yalnızca kümeden erişilebilir veya bir küme aynı ağdaki diğer makinelerden bir Azure sanal ağı üzerinde ise.

1. Oozie hizmetin URL'sini almak için aşağıdaki komutu kullanın:

    ```bash
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Bu, aşağıdaki XML gibi bilgileri döndürür:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` Bölümüdür URL'si ile Oozie komutunu kullanın.

2. URL daha önce aldığınız değiştirmek için kodu düzenleyin. Her komut için girmek zorunda kalmaması bir ortam değişkeni için URL'yi oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

3. İşi göndermek için aşağıdakileri kullanın:

    ```bash
    oozie job -config job.xml -submit
    ```

    Bu komut iş bilgileri yükler `job.xml` ve Oozie için gönderir, ancak değil çalıştırın.

    Komut bittikten sonra işin kimliği gibi döndürmelidir `0000005-150622124850154-oozie-oozi-W`. Bu kimliği, işi yönetmek için kullanılır.

4. Değiştirmek için aşağıdaki kodu düzenleme `<JOBID>` önceki adımda döndürülen Kimliğine sahip.  İşin durumunu görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    oozie job -info <JOBID>
    ```

    Bu, aşağıdaki metni gibi bilgileri döndürür:

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasb:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Bu iş durumuna sahip `PREP`. Bu durum, iş oluşturuldu, ancak başlatılmadı olduğunu gösterir.

5. Değiştirmek için aşağıdaki kodu düzenleme `<JOBID>` daha önce döndürülen Kimliğine sahip.  İşi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    oozie job -start JOBID
    ```

    Bu komuttan sonra durumu denetleme, çalışır durumda olduğundan ve işin içinde eylemler için bilgi döndürülür.  İşin tamamlanması birkaç dakika sürer.

6. Değiştirmek için aşağıdaki kodu düzenleme `<serverName>` ile Azure SQL sunucunuzun adını ve `<sqlLogin>` Azure SQL server oturum açma.  Görev başarıyla tamamlandıktan sonra veriler oluşturulan ve aşağıdaki komutu kullanarak SQL veritabanı tablosuna dışarı olduğunu doğrulayabilirsiniz.  İsteminde bir parola girin.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -p 1433 -D oozietest
    ```

    Adresindeki `1>` isteminde, aşağıdaki sorguyu girin:

    ```sql
    SELECT * FROM mobiledata
    GO
    ```

    Döndürülen bilgileri gibi aşağıda gösterilmiştir:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Oozie komut hakkında daha fazla bilgi için bkz. [Apache Oozie komut satırı aracı](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API

Oozie REST API ile Oozie ile çalışan kendi araçları oluşturabilirsiniz. HDInsight özel Oozie REST API kullanımı hakkında bilgi verilmiştir:

* **URI**: Konumundaki kümeye dışında REST API'SİNDEN erişebileceğiniz `https://CLUSTERNAME.azurehdinsight.net/oozie`.

* **Kimlik doğrulaması**: Kimlik doğrulaması için API kümesi HTTP hesabı (Yönetici) ve parolayı kullanın. Örneğin:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Oozie REST API'SİNİN nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Apache Oozie Web Servisleri API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie web UI

Oozie web kullanıcı Arabirimi, küme üzerinde Oozie işlerin durumunu web tabanlı bir görünüm sağlar. Web kullanıcı Arabirimi ile aşağıdaki bilgileri görüntüleyebilirsiniz:

   * İş durumu
   * İş tanımı
   * Yapılandırma
   * Bir grafik iş eylemi
   * İş günlükleri

Eylemler işindeki ayrıntılarını da görüntüleyebilirsiniz.

Oozie web kullanıcı Arabirimi erişmek için aşağıdaki adımları tamamlayın:

1. HDInsight kümesine SSH tüneli oluşturma. Daha fazla bilgi için [SSH tünel oluşturmayı kullanma HDInsight ile](hdinsight-linux-ambari-ssh-tunnel.md).

2. Tünel oluşturduktan sonra Ambari web kullanıcı Arabirimi URI kullanılarak web tarayıcınızda açın `http://headnodehost:8080`.

3. Sayfanın sol taraftan seçin **Oozie** > **hızlı bağlantılar** > **Oozie Web kullanıcı arabirimini**.

    ![Menü görüntüsü](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Oozie web kullanıcı Arabirimi varsayılan olarak çalışan iş akışı işleri görüntüleyin. Tüm iş akışı işleri görmek için seçin **tüm işleri**.

    ![Görüntülenen tüm işler](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Bir iş hakkında daha fazla bilgi görüntülemek için işi seçin.

    ![İş bilgileri](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Gelen **iş bilgileri** sekmesi, temel iş bilgileri ve iş içindeki tek tek eylemleri görebilirsiniz. Görüntülemek için üst kısmındaki sekmeleri kullanabilirsiniz **iş tanımı**, **iş yapılandırması**, erişim **iş günlüğü**, ya da işin altındayönlendirilmişÇevrimsizgraf(DAG)görüntülemek**DAG iş**.

   * **İş günlüğü**: Seçin **günlükleri alma** iş için tüm günlükleri almak için düğmesine veya kullanın **girin arama filtresi** günlükleri filtrelemek için alan.

       ![İş Günlüğü](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **Proje DAG**: DAG, iş akışı gerçekleştirilen veri yolları grafik bir genel bakıştır.

       ![İş DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Eylemlerden birini seçerseniz **iş bilgileri** sekmesinde getirdiği eylemi için bilgileri. Örneğin, **RunSqoopExport** eylem.

    ![Eylem bilgileri](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Bir bağlantı gibi işlem ayrıntılarını görebilirsiniz **Konsolu URL'si**. İş için iş İzleyicisi bilgilerini görmek için bu bağlantıyı kullanın.

## <a name="schedule-jobs"></a>İşleri zamanlayın

Düzenleyici, yinelenme sıklığı işleri için bir başlangıç ve sona belirtmek için kullanabilirsiniz. İş akışı için bir zamanlama tanımlamak için aşağıdaki adımları tamamlayın:

1. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **coordinator.xml**:

    ```bash
    nano coordinator.xml
    ```

    Aşağıdaki XML dosyasının içeriği kullanın:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]  
    > `${...}` Değişkenleri, çalışma zamanında iş tanımındaki değerlerle değiştirilir. Değişkenleri şunlardır:
    >
    > * `${coordFrequency}`: Çalışan işin Örnekler arasındaki süre.
    > * `${coordStart}`: İş başlangıç zamanı.
    > * `${coordEnd}`: İşin bitiş saati.
    > * `${coordTimezone}`: Bir sabit saat dilimindeki genellikle UTC saat kullanarak tarafından temsil edilen hiçbir günışığından, düzenleyici işleri. Bu saat dilimi olarak adlandırılır *Oozie işleme saat dilimi.*
    > * `${wfPath}`: Workflow.xml yolu.

2. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

3. Bu proje için çalışma dizini dosya kopyalamak için aşağıdaki komutu kullanın:

    ```bash
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Değiştirilecek `job.xml` daha önce oluşturduğunuz dosyası, aşağıdaki komutu kullanın:

    ```bash
    nano job.xml
    ```

    Aşağıdaki değişiklikleri yapın:

   * Oozie Düzenleyicisi dosyanın yerine iş akışı çalıştırmak için açmasını sağlamak için değiştirme `<name>oozie.wf.application.path</name>` için `<name>oozie.coord.application.path</name>`.

   * Ayarlanacak `workflowPath` aşağıdaki XML'i ekleyin düzenleyici tarafından kullanılan değişkeni:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Değiştirin `wasbs://mycontainer@mystorageaccount.blob.core.windows` diğer girişler job.xml dosyasında kullanılan değerle metin.

   * Başlangıç, bitiş ve düzenleyici sıklığı tanımlamak için aşağıdaki XML'i ekleyin:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2018-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2018-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Bu değerler, 12: 00'dan 10 Mayıs 2018 için başlangıç ve bitiş zamanını 12 Mayıs 2018'e ayarlayın. Bu işi çalıştırmak için aralığı, günlük olarak ayarlanır. Dakikalar içinde sıklığıdır şekilde 1440 dakika 24 saat x 60 dakika =. Son olarak, saat dilimi UTC değerine ayarlanır.

5. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

6. Gönder ve işi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    oozie job -config job.xml -run
    ```

7. Oozie için web kullanıcı Arabirimi ve seçin giderseniz **Düzenleyicisi işleri** sekmesinde, aşağıdaki resimdeki gibi bilgileri görürsünüz:

    ![Düzenleyici işler sekmesinden](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    **Sonraki Materialization** girdisi, iş sonraki çalıştırmasında içerir.

8. Web kullanıcı Arabirimi iş girişi seçerseniz önceki iş akışı işini gibi bilgileri işinde görüntüler:

    ![İş bilgileri Düzenleyicisi](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]  
    > Bu görüntü, başarılı çalıştırmalar bireysel eylemleri zamanlanmış iş akışı içinde değil, işin yalnızca gösterir. Bireysel eylemleri görmek için aşağıdakilerden birini seçin: **eylem** girdileri.

    ![Eylem bilgileri](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Sorun giderme

Oozie UI ile Oozie günlüklerini görüntüleyebilirsiniz. Oozie kullanıcı Arabirimi de Jobtracker'a günlükleri iş akışı tarafından başlatılan MapReduce görevlerin bağlantılarını içerir. Sorun giderme için desen olmalıdır:

   1. İşi Oozie web kullanıcı arabirimini görüntüleyin.

   2. Varsa bir hata veya belirli bir eylem için hatası olmadığını görmek için bir eylem seçin **hata iletisi** alan, hata durumunda daha fazla bilgi sağlar.

   3. Varsa, eylem URL'den Jobtracker'a günlükleri, eylem için daha fazla ayrıntı görüntülemek için kullanın.

Karşılaşabileceğiniz belirli hata ve bunların nasıl çözüleceğine aşağıda verilmiştir.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Küme başlatılamıyor

**Belirtiler**: İş durumu değişikliklerini **askıya alındı**. Ayrıntılar için iş Göster `RunHiveScript` durumu olarak **START_MANUAL**. Eylemini seçerek, aşağıdaki hata iletisini görüntüler:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Neden**: İçinde kullanılan Azure Blob Depolama adresleri **job.xml** dosya depolama kapsayıcısı veya depolama hesabı adı içermiyor. Blob Depolama adresi biçimi olmalıdır `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Çözüm**: İşin kullandığı Blob Depolama adreslerini değiştirin.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltusergt"></a>JA002: ASP.NET'in kimliğine bürünmesini Oozie verilmez &lt;kullanıcı&gt;

**Belirtiler**: İş durumu değişikliklerini **askıya alındı**. Ayrıntılar için iş Göster `RunHiveScript` durumu olarak **START_MANUAL**. Eylem seçerseniz, aşağıdaki hata iletisini gösterir:

    JA002: User: oozie is not allowed to impersonate <USER>

**Neden**: Geçerli izin ayarları, Oozie belirtilen kullanıcı hesabı almasına izin vermez.

**Çözüm**: Oozie kullanıcıların kimliğine bürünmek **kullanıcılar** grubu. Kullanım `groups USERNAME` kullanıcı hesabının üyesi olduğu grupları görmek için. Kullanıcı bir üyesi değilse **kullanıcılar** grup, kullanıcı grubuna eklemek için aşağıdaki komutu kullanın:

    sudo adduser USERNAME users

> [!NOTE]  
> Bu kullanıcı grubuna eklenmiş olan HDInsight kesilmesini algılamasından önce birkaç dakika sürebilir.

### <a name="launcher-error-sqoop"></a>HATA (Sqoop) Başlatıcısı

**Belirtiler**: İş durumu değişikliklerini **KILLED**. Ayrıntılar için iş Göster `RunSqoopExport` durumu olarak **hata**. Eylem seçerseniz, aşağıdaki hata iletisini gösterir:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Neden**: Sqoop veritabanına erişmek için gereken veritabanı sürücüsünü yükleyemedi.

**Çözüm**: Sqoop, Oozie bir işten kullandığınızda, iş kullandığı workflow.xml gibi diğer kaynaklar ile veritabanı sürücüsünü içermelidir. Ayrıca, veritabanı sürücüsünü içeren arşiv başvuru `<sqoop>...</sqoop>` workflow.xml bölümü.

Örneğin, bu belgede proje için aşağıdaki adımları kullanırsınız:

1. Kopyalama `mssql-jdbc-7.0.0.jre8.jar` dosyasını **/öğreticiler/useoozie** dizini:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre8.jar /tutorials/useoozie/mssql-jdbc-7.0.0.jre8.jar
    ```

2. Değiştirme `workflow.xml` aşağıdaki XML yeni bir satıra yukarıdaki eklemek için `</sqoop>`:

    ```xml
    <archive>mssql-jdbc-7.0.0.jre8.jar</archive>
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Oozie iş akışının tanımlayın ve Oozie işinin nasıl çalıştırılacağını öğrendiniz. HDInsight ile çalışma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight, Apache Hadoop işleri için veri yükleme][hdinsight-upload-data]
* [HDInsight, Apache Hadoop ile Apache Sqoop'u kullanma][hdinsight-use-sqoop]
* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma][hdinsight-use-hive]
* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma][hdinsight-use-pig]
* [HDInsight için Java MapReduce programları geliştirme][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-linux-mac/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-linux-mac/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
