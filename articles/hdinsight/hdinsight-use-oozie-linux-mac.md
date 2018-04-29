---
title: Azure Hdınsight'ta Linux tabanlı Hadoop Oozie iş akışlarını kullanın | Microsoft Docs
description: Linux tabanlı Hdınsight'ta Hadoop Oozie kullanın. Oozie iş akışı tanımlamak ve Oozie işi göndermek öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: larryfr
ms.openlocfilehash: 8a25507ab076c4eecccea4e8a503d68ff1441ae5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-azure-hdinsight"></a>Oozie Hadoop ile tanımlamak ve Azure Hdınsight'ta Linux tabanlı bir iş akışını çalıştırmak için kullanın.

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Azure hdınsight'ta Hadoop ile Apache Oozie kullanmayı öğrenin. Oozie, Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile tümleştirilir ve aşağıdaki işleri destekler:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie, Java programları veya kabuk betikleri gibi sisteme özel işleri planlamak için de kullanabilirsiniz.

> [!NOTE]
> Hdınsight iş akışlarıyla tanımlamak için başka bir seçenek, Azure Data Factory kullanmaktır. Data Factory hakkında daha fazla bilgi için bkz: [kullanım Pig ve Hive Data Factory ile][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie etki alanına katılmış Hdınsight üzerinde etkin değil.

## <a name="prerequisites"></a>Önkoşullar

* **Hdınsight kümesi**: bkz [Linux'ta Hdınsight ile çalışmaya başlama](/hadoop/apache-hadoop-linux-tutorial-get-started.md)

> [!IMPORTANT]
> Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux üzerinde Hdınsight sürüm 3.4 veya üstü kullanılan yalnızca işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="example-workflow"></a>Örnek iş akışı

Bu belgede kullanılan iş akışı iki eylemleri içerir. Hive, Sqoop, MapReduce veya diğer işlemler çalıştırma gibi görevler için tanımları eylemler şunlardır:

![İş akışı diyagramı][img-workflow-diagram]

1. Hive eylem kayıtları ayıklamak için HiveQL betiğini çalıştırır **hivesampletable** Hdınsight ile eklendi. Her veri satırının belirli bir mobil CİHAZDAN ziyaret açıklar. Kayıt biçimi aşağıdaki metni gibi görünür:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Bu belgede kullanılan Hive betiğini Android veya iPhone, gibi her bir platform için toplam ziyaret sayar ve yeni bir Hive tablosu için sayıları depolar.

    Hive hakkında daha fazla bilgi için bkz. [HDInsight ile Hive kullanma][hdinsight-use-hive].

2. Sqoop eylem yeni Hive tablosu içeriğini Azure SQL veritabanı'nda oluşturulmuş bir tabloyu dışarı aktarır. Sqoop hakkında daha fazla bilgi için bkz: [Hdınsight ile kullanım Hadoop Sqoop][hdinsight-use-sqoop].

> [!NOTE]
> Hdınsight kümelerinde desteklenen Oozie sürümleri için bkz: [Hdınsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler][hdinsight-versions].

## <a name="create-the-working-directory"></a>Çalışma dizini oluşturma

Oozie aynı dizinde bir iş için gereken tüm kaynakları depolamak için bekliyor. Bu örnekte **wasb: / / / öğreticileri/useoozie**. Bu dizin oluşturmak için aşağıdaki adımları tamamlayın:

1. SSH kullanarak Hdınsight kümesine bağlanma:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Değiştir `sshuser` küme için SSH kullanıcı adı. Değiştir `clustername` küme adı. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Dizin oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -mkdir -p /tutorials/useoozie/data
    ```

    > [!NOTE]
    > `-p` Parametresi yolunda tüm dizinleri oluşturulmasına neden olur. **Veri** dizini tarafından kullanılan verileri depolamak için kullanılan **useooziewf.hql** komut dosyası.

3. Oozie, kullanıcı hesabınızın bürünebileceğini emin olmak için aşağıdaki komutu kullanın:

    ```bash
    sudo adduser username users
    ```

    Değiştir `username` SSH kullanıcı adı.

    > [!NOTE]
    > Kullanıcı zaten bir üyesidir hatalara yoksayabilirsiniz `users` grubu.

## <a name="add-a-database-driver"></a>Bir veritabanı sürücüsü Ekle

Bu iş akışı SQL veritabanına veri vermek için Sqoop kullandığından, SQL veritabanıyla etkileşim kurmak için kullanılan JDBC sürücüsü kopyasını sağlamanız gerekir. Çalışma dizini JDBC sürücüsü kopyalamak için SSH oturumundan aşağıdaki komutu kullanın:

```bash
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

> [!NOTE]
> Dosya zaten bir ileti alabilirsiniz.

İş akışınızı bir MapReduce uygulamayı içeren bir jar gibi diğer kaynaklar kullandıysanız, bu kaynakları de eklemeniz gerekir.

## <a name="define-the-hive-query"></a>Hive sorgusunu tanımlayın

Bir sorguyu tanımlayan bir Hive sorgu dili (HiveQL) komut dosyası oluşturmak için aşağıdaki adımları kullanın. Sorgu bir Oozie iş akışı bu belgenin sonraki bölümlerinde kullanır.

1. SSH Bağlantısı adlı bir dosya oluşturmak için aşağıdaki komutu kullanın. `useooziewf.hql`:

    ```bash
    nano useooziewf.hql
    ```

3. GNU nano Düzenleyici açıldıktan sonra dosyanın içeriğini aşağıdaki sorguyu kullanın:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Komut dosyasında kullanılan iki değişkenleri şunlardır:

    * `${hiveTableName}`: Oluşturulacak tablonun adını içerir.

    * `${hiveDataFolder}`: Tablo için veri dosyalarının depolanacağı konumu içerir.

    Bu öğreticide workflow.xml iş akışı tanımı dosyası bu değerleri bu HiveQL betiğini çalışma zamanında geçirir.

4. Düzenleyiciden çıkmak için Ctrl + X seçin. İstendiğinde, seçin `Y` dosyayı kaydetmek için girin `useooziewf.hql` dosya adı ve ardından **Enter**.

5. Kopyalamak için aşağıdaki komutları kullanın `useooziewf.hql` için `wasb:///tutorials/useoozie/useooziewf.hql`:

    ```bash
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Bu komut depolar `useooziewf.hql` küme için HDFS uyumlu depolama dosyası.

## <a name="define-the-workflow"></a>İş akışı tanımlama

Oozie iş akışı tanımları Hadoop işlem tanımı, bir XML işlem tanım dili olan dilinde (hPDL) yazılır. İş akışını tanımlamak için aşağıdaki adımları kullanın:

1. Oluşturun ve yeni bir dosya düzenlemek için şu deyimi kullanın:

    ```bash
    nano workflow.xml
    ```

2. Nano Düzenleyici açıldığında, aşağıdaki XML dosya içeriklerini girin:

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

    İş akışında tanımlanan iki eylem vardır:

   * `RunHiveScript`: Bu eylem başlangıç eylemdir ve çalışan `useooziewf.hql` Hive betiği.

   * `RunSqoopExport`: Bu eylem Sqoop kullanarak SQL veritabanına Hive komut dosyasından oluşturulan veri aktarır. Bu eylem yalnızca çalıştırır `RunHiveScript` eylem başarılı olur.

     İş akışı gibi birden çok girişi sahip `${jobTracker}`. Bu girişler iş tanımında kullandığınız değerlerle değiştirir. Bu belgede daha sonra iş tanımı oluşturur.

     Ayrıca unutmayın `<archive>sqljdbc4.jar</archive>` Sqoop bölümünde girişi. Bu giriş, bu eylem çalıştırıldığında bu arşiv Sqoop için kullanılabilmesi için Oozie bildirir.

3. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**. 

4. Kopyalamak için aşağıdaki komutu kullanın `workflow.xml` dosya `/tutorials/useoozie/workflow.xml`:

    ```bash
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir SQL veritabanı oluşturmak için adımları [bir SQL veritabanı oluşturma](../sql-database/sql-database-get-started.md) belge. Veritabanı oluşturduğunuzda, kullanmak `oozietest` veritabanı adı. Ayrıca veritabanı sunucusunun adını not edin.

### <a name="create-the-table"></a>Tablo oluşturma

> [!NOTE]
> Bir tablo oluşturmak için SQL veritabanına bağlanmak için birçok yolu vardır. Aşağıdaki adımları kullanın [ücretsiz](http://www.freetds.org/) Hdınsight kümesine ait.


1. Ücretsiz Hdınsight kümesine yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Ücretsiz yüklendikten sonra daha önce oluşturduğunuz SQL veritabanı sunucusuna bağlanmak için aşağıdaki komutu kullanın:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    Aşağıdaki metni gibi bir çıktı alırsınız:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. Konumundaki `1>` isteminde, aşağıdaki satırları girin:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Zaman `GO` deyimi girilir, önceki deyimleri değerlendirilir. Bu ifadeler adlı bir tablo oluşturmak **mobiledata**, iş akışı tarafından kullanılır.

    Tablo oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Aşağıdaki metni gibi bir çıktı görürsünüz:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

4. Tsql yardımcı programı'ndan çıkmak için girin `exit` adresindeki `1>` istemi.

## <a name="create-the-job-definition"></a>İş tanımı oluştur

İş tanımı workflow.xml nerede bulacağını açıklar. Ayrıca iş akışı tarafından kullanılan diğer dosyaları nerede bulacağını açıklanır `useooziewf.hql`. Ayrıca, iş akışı ve ilişkili dosyaları içinde kullanılan özellikler için değerler tanımlar.

1. Varsayılan depolama tam adresini almak için aşağıdaki komutu kullanın. Bu adres, sonraki adımda oluşturduğunuz yapılandırma dosyası kullanılır.

    ```bash
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Bu komut, aşağıdaki XML gibi bilgileri döndürür:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Hdınsight küme varsayılan depolama alanı olarak Azure Storage kullanıyorsa `<value>` öğenin içeriği ile başlar `wasb://`. Azure Data Lake Store yerine kullanılırsa, ile başlayan `adl://`.

    İçeriğini kaydetme `<value>` şekliyle öğe, sonraki adımlarda kullanılır.

2. Oozie iş tanımı yapılandırması oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano job.xml
    ```

3. Nano Düzenleyici açıldığında, aşağıdaki XML dosyasının içeriği kullanın:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
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
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * Tüm örneklerinin yerine `wasb://mycontainer@mystorageaccount.blob.core.windows.net` aldığınız önceki sürümleri için varsayılan depolama değerine sahip.

     > [!WARNING]
     > Yol ise bir `wasb` yolu, tam yolunu kullanmanız gerekir. Yalnızca kendisine kısaltın değil `wasb:///`.

   * Değiştir `YourName` Hdınsight kümesi için oturum açma adınızı ile.
   * Değiştir `serverName`, `adminLogin`, ve `adminPassword` SQL veritabanınız ilişkin bilgiler.

     Bu dosyadaki bilgiler çoğunu workflow.xml veya ooziewf.hql dosyalarında gibi kullanılan değerleri doldurmak için kullanılan `${nameNode}`.

     > [!NOTE]
     > `oozie.wf.application.path` Girdi workflow.xml dosya nerede bulacağını tanımlar. Bu dosya bu iş tarafından çalıştırıldığı iş akışını içerir.

5. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

## <a name="submit-and-manage-the-job"></a>Gönderme ve iş yönetimi

Aşağıdaki adımlar Oozie komutunu göndermek ve küme Oozie iş akışlarında yönetmek için kullanın. Oozie kullanıcı dostu bir arabirim üzerinden komuttur [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> Oozie komutunu kullandığınızda, Hdınsight baş düğüm için FQDN kullanmanız gerekir. Bu FQDN yalnızca kümeden erişilebilir veya küme aynı ağdaki diğer makinelerden bir Azure sanal ağda ise.


1. Oozie hizmeti URL'sini elde etmek için aşağıdaki komutu kullanın:

    ```bash
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Aşağıdaki XML gibi bilgiler verir:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` Bölümüdür Oozie komutu ile kullanılacak URL.

2. Bir ortam değişkeni URL'si oluşturmak için her komut için girmek zorunda kalmamak için aşağıdakileri kullanın:

    ```bash
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    URL, daha önce aldığınız adla değiştirin.
3. İşi göndermek için aşağıdakileri kullanın:

    ```bash
    oozie job -config job.xml -submit
    ```

    Bu komut iş bilgilerini yükler `job.xml` ve Oozie için gönderir, ancak değil çalıştırın.

    Komut bittikten sonra işin kimliği gibi döndürmelidir `0000005-150622124850154-oozie-oozi-W`. Bu kimliği iş yönetmek için kullanılır.

4. İş durumunu görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Değiştir `<JOBID>` önceki adımda döndürülen Kimliğine sahip.

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

    Bu iş durumuna sahip `PREP`. Bu durum, iş oluşturuldu, ancak başlatılmamış olduğunu gösterir.

5. İşlemi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Değiştir `<JOBID>` döndürülen Kimliğine sahip.

    Bu komutun sonraki durumunu denetlemek, çalışır durumda olduğundan ve iş içindeki eylemler için bilgi döndürülür.

6. Görev başarıyla tamamlandıktan sonra verileri oluşturulur ve aşağıdaki komutu kullanarak SQL veritabanı tablosu için dışa aktarılan olduğunu doğrulayabilirsiniz:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    Konumundaki `1>` isteminde, aşağıdaki sorguyu girin:

    ```sql
    SELECT * FROM mobiledata
    GO
    ```

    Döndürülen bilgiler gibi aşağıdaki metni şunlardır:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Oozie komutu hakkında daha fazla bilgi için bkz: [Oozie komut satırı aracı](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API

Oozie REST API'si ile Oozie ile iş kendi araçları oluşturabilirsiniz. Oozie REST API kullanımı hakkında Hdınsight özgü bilgiler aşağıdadır:

* **URI**: küme dışındaki REST API öğesinden erişebilirsiniz `https://CLUSTERNAME.azurehdinsight.net/oozie`.

* **Kimlik doğrulama**: kimlik doğrulaması için API küme HTTP hesabı (Yönetici) ve parolayı kullanın. Örneğin:

    ```bash
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Oozie REST API kullanma hakkında daha fazla bilgi için bkz: [Oozie Web Hizmetleri API'si](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie web kullanıcı Arabirimi

Oozie web kullanıcı Arabirimi kümede Oozie işlerin durumunu web tabanlı bir görünüme sağlar. Web kullanıcı Arabirimi ile aşağıdaki bilgileri görüntüleyebilirsiniz:

   * İş durumu
   * İş tanımı
   * Yapılandırma
   * İşte eylemlerin bir grafik
   * İşi için kayıtlar

Ayrıca bir işi içinde eylemler ayrıntılarını görüntüleyebilirsiniz.

Oozie web kullanıcı Arabirimi erişmek için aşağıdaki adımları tamamlayın:

1. Bir Hdınsight kümesine SSH tüneli oluşturma. Daha fazla bilgi için bkz: [kullanım SSH tünel Hdınsight ile](hdinsight-linux-ambari-ssh-tunnel.md).

2. Bir tünel oluşturduktan sonra Ambari web kullanıcı Arabirimi, web tarayıcınızda açın. Ambari site için bir URI `https://CLUSTERNAME.azurehdinsight.net`. Değiştir `CLUSTERNAME` Linux tabanlı Hdınsight kümenizin adıyla.

3. Sayfanın sol taraftan seçin **Oozie** > **hızlı bağlantılar** > **Oozie Web kullanıcı arabirimini**.

    ![görüntüsü menüler](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Oozie web kullanıcı Arabirimi varsayılan olarak çalışan iş akışı işleri görüntüleyin. Tüm iş akışı işleri görmek için seçin **tüm işleri**.

    ![Görüntülenen tüm işleri](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Bir iş hakkında daha fazla bilgi görüntülemek için işi seçin.

    ![İş bilgileri](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Gelen **iş bilgileri** sekmesi, temel iş bilgileri ve iş içindeki ayrı Eylemler görebilirsiniz. Görüntülemek için üst kısmındaki sekmeleri kullanabilirsiniz **iş tanımı**, **iş yapılandırması**, erişim **iş günlüğü**, veya iş altındayönlendirilmişÇevrimsizgrafik(DAG)görüntülemek**DAG iş**.

   * **İş günlüğü**: seçin **Get Logs** iş için tüm günlükleri almak için düğmesini veya kullanmak **girin arama filtresi** günlükleri filtrelemek için alan.

       ![İş Günlüğü](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **İş DAG**: DAG olan akışı gerçekleştirilecek veri yolları grafik bir genel bakış.

       ![İş DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Eylemlerden birini seçerseniz **iş bilgileri** sekmesi, beraberinde getirir eylemi için bilgileri. Örneğin, seçin **RunSqoopExport** eylem.

    ![Eylem bilgileri](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Bir bağlantı gibi eylemin ayrıntılarını görebilirsiniz **Konsolu URL'si**. İş için iş İzleyicisi bilgilerini görüntülemek için bu bağlantıyı kullanın.

## <a name="schedule-jobs"></a>İşleri Zamanla

Düzenleyici bir başlatma, bir son ve işleri oluşum sıklığını belirtmek için kullanabilirsiniz. İş akışı için bir zamanlama tanımlamak için aşağıdaki adımları tamamlayın:

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
    > `${...}` Değişkenleri, çalışma zamanında iş tanımında değerlere göre değiştirilir. Değişkenleri şunlardır:
    >
    > * `${coordFrequency}`: Çalışan iş örneklerini arasında saat.
    > * `${coordStart}`: İş başlangıç zamanı.
    > * `${coordEnd}`: İş bitiş saati.
    > * `${coordTimezone}`: Bir sabit saat diliminde genellikle UTC kullanarak temsil hiçbir gün ışığından yararlanma saati ile olan düzenleyici işleri. Bu saat dilimi olarak adlandırılır *Oozie işleme saat dilimi.*
    > * `${wfPath}`: Workflow.xml yolu.

2. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

3. Bu proje için çalışma dizini dosyasını kopyalamak için aşağıdaki komutu kullanın:

    ```bash
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. Değiştirilecek `job.xml` dosya, aşağıdaki komutu kullanın:

    ```
    nano job.xml
    ```

    Aşağıdaki değişiklikleri yapın:

   * İş akışı yerine Düzenleyicisi dosyasını çalıştırmak için Oozie istemek üzere değiştirme `<name>oozie.wf.application.path</name>` için `<name>oozie.coord.application.path</name>`.

   * Ayarlamak için `workflowPath` aşağıdaki XML ekleme Düzenleyicisi tarafından kullanılan değişkeni:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Değiştir `wasb://mycontainer@mystorageaccount.blob.core.windows` diğer giriş job.xml dosyası kullanılan değeri olan metin.

   * Başlangıç tanımlamak için aşağıdaki XML bitiş ve düzenleyici sıklığı ekleyin:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
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

       12:00 PM 10 May 2017'için başlangıç zamanı ve bitiş zamanı 12 Mayıs 2017 için bu değerleri ayarlayın. Bu işi çalıştırmak için aralık günlük olarak ayarlanır. Dakika cinsinden sıklığıdır 1440 dakika 24 saat x 60 dakika kadar =. Son olarak, saat dilimi UTC için ayarlanır.

5. Dosyayı kaydetmek için Ctrl + X seçip girin `Y`ve ardından **Enter**.

6. İşi çalıştırmak için aşağıdaki komutu kullanın:

    ```
    oozie job -config job.xml -run
    ```

    Bu komut gönderir ve işini başlatır.

7. Oozie web kullanıcı Arabirimi ve seçin giderseniz **Düzenleyicisi işleri** sekmesinde, aşağıdaki görüntüde gibi bilgileri görürsünüz:

    ![Düzenleyici işler sekmesi](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    **Sonraki Materialization** girişi, işin bir sonraki çalıştırmasında içeriyor.

8. Web kullanıcı Arabirimi iş girişi seçerseniz önceki iş akışı işini gibi bilgileri işinde görüntüler:

    ![Düzenleyici iş bilgileri](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Bu görüntü yalnızca başarılı çalıştırır zamanlanmış iş akışı içinde değil tek tek Eylemler işinin gösterir. Tek tek işlemleri görmek için aşağıdakilerden birini seçin **eylem** girişleri.

    ![Eylem bilgileri](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Sorun giderme

Oozie UI ile Oozie günlükleri görüntüleyebilirsiniz. Oozie UI ayrıca iş akışı tarafından başlatılan MapReduce görevler için Jobtracker'a günlüklerine bağlantılar içerir. Sorun giderme için desen olmalıdır:

   1. İşi Oozie web kullanıcı arabirimini görüntüleyin.

   2. Bir hata veya belirli bir eylemi için hatası ise, olmadığını görmek için bir eylem seçin **hata iletisi** alan hatada daha fazla bilgi sağlar.

   3. Mevcut ise, eylem URL'den eylemi için Jobtracker'a günlükleri gibi daha fazla ayrıntı görüntülemek için kullanın.

Karşılaşabileceğiniz belirli hataları ve bunların nasıl çözüleceği verilmiştir.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: küme başlatılamıyor

**Belirtiler**: İş durumu değişikliklerini **ASKIYA**. İş Göster ayrıntılarını `RunHiveScript` durumu olarak **START_MANUAL**. Eylem seçildikten aşağıdaki hata iletisini görüntüler:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Neden**: Azure Blob Depolama adresleri kullanılan **job.xml** dosya depolama kapsayıcısı veya depolama hesabı adı içermiyor. Blob Depolama adres biçimini olmalıdır `wasb://containername@storageaccountname.blob.core.windows.net`.

**Çözümleme**: iş kullanır Blob Depolama adreslerini değiştirin.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie almasına izin verilmiyor &lt;kullanıcı >

**Belirtiler**: İş durumu değişikliklerini **ASKIYA**. İş Göster ayrıntılarını `RunHiveScript` durumu olarak **START_MANUAL**. Eylem'ı seçerseniz, aşağıdaki hata iletisini gösterir:

    JA002: User: oozie is not allowed to impersonate <USER>

**Neden**: Geçerli izin ayarları, belirtilen kullanıcı hesabının kimliğine bürün Oozie izin verme.

**Çözümleme**: Oozie kullanıcıların kimliğine bürünmek **kullanıcılar** grubu. Kullanım `groups USERNAME` kullanıcı hesabının bir üyesi olduğu grupların görmek için. Kullanıcı bir üyesi değilse **kullanıcılar** grup, kullanıcı grubuna eklemek için aşağıdaki komutu kullanın:

    sudo adduser USERNAME users

> [!NOTE]
> Kullanıcı grubuna eklenmiş olan Hdınsight algılamasından önce birkaç dakika sürebilir.

### <a name="launcher-error-sqoop"></a>Başlatıcı hata (Sqoop)

**Belirtiler**: İş durumu değişikliklerini **KILLED**. İş Göster ayrıntılarını `RunSqoopExport` durumu olarak **hata**. Eylem'ı seçerseniz, aşağıdaki hata iletisini gösterir:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Neden**: Sqoop veritabanına erişmek için gerekli veritabanı sürücüsünü yükleyemiyor.

**Çözümleme**: Oozie işten Sqoop kullandığınızda, iş kullandığı workflow.xml gibi başka kaynaklar ile veritabanı sürücüsünü eklemeniz gerekir. Ayrıca, veritabanı sürücüsünü içeren arşiv başvuru `<sqoop>...</sqoop>` workflow.xml bölümü.

Örneğin, bu belgede proje için aşağıdaki adımları kullanırsınız:

1. Kopya `sqljdbc4.1.jar` dosya **/öğreticileri/useoozie** dizini:

    ```bash
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Değiştirme `workflow.xml` yeni bir satıra yukarıdaki aşağıdaki XML eklemek için `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Oozie iş akışı tanımlama ve Oozie işini çalıştır öğrendiniz. Hdınsight ile çalışma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight ile zamana dayalı Oozie düzenleyicisi kullanın][hdinsight-oozie-coordinator-time]
* [Hdınsight'ta Hadoop işleri için verileri karşıya yükleme][hdinsight-upload-data]
* [Hdınsight'ta Hadoop ile Sqoop kullanma][hdinsight-use-sqoop]
* [Hdınsight'ta Hadoop ile Hive kullanma][hdinsight-use-hive]
* [Hdınsight'ta Hadoop ile pig kullanma][hdinsight-use-pig]
* [Hdınsight için Java MapReduce programlar geliştirmek][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/transform-data.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
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

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
