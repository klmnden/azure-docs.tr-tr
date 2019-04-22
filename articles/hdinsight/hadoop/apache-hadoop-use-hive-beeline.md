---
title: Apache Hive - Azure HDInsight ile Apache Beeline kullanma
description: Beeline istemci HDInsight Hadoop ile Hive sorguları çalıştırmak için kullanmayı öğrenin. Beeline JDBC HiveServer2 ile çalışmak için bir yardımcı programdır.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: hrasheed
ms.openlocfilehash: 89303e5c827fc24540d345a9a2b9a0743e453a4d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59257136"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Apache Hive ile Apache Beeline istemcisini kullanma

Nasıl kullanacağınızı öğrenin [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) HDInsight üzerinde Apache Hive sorguları çalıştırmak için.

Beeline HDInsight kümenizin baş düğümlerine dahil edilmiş bir Hive istemcisidir. Beeline JDBC HiveServer2, HDInsight kümeniz üzerinde barındırılan bir hizmete bağlanmak için kullanır. Beeline HDInsight üzerindeki Hive'a internet üzerinden uzaktan erişmek için de kullanabilirsiniz. Aşağıdaki örneklerde, HDInsight için Beeline bağlanmak için kullanılan en yaygın bağlantı dizelerini sağlanır:

## <a name="types-of-connections"></a>Bağlantı türleri

### <a name="from-an-ssh-session"></a>Bir SSH oturumundan

Bir SSH oturumunda bir küme baş düğümüne bağlanırken, ardından bağlanabilirsiniz `headnodehost` adresi üzerinde bağlantı noktası `10001`:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

---

### <a name="over-an-azure-virtual-network"></a>Bir Azure sanal ağ üzerinden

Bir istemciden HDInsight için bir Azure sanal ağ üzerinden bağlanırken, küme baş düğümü tam etki alanı adını (FQDN) sağlamalısınız. Bu bağlantıyı doğrudan küme düğümlerinde yapıldığından, bağlantı, bağlantı noktasını kullanır. `10001`:

```bash
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Değiştirin `<headnode-FQDN>` ile küme baş düğümüne tam etki alanı adı. Bir baş düğüm tam etki alanı adını bulmak için yer alan bilgileri kullanın. [Apache Ambari REST API'yi kullanarak HDInsight yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belge.

---

### <a name="to-hdinsight-enterprise-security-package-esp-cluster"></a>HDInsight Kurumsal güvenlik paketi (ESP) kümeye

Azure Active Directory (AAD) bir istemciden bir kurumsal güvenlik paketi (ESP) kümesine bağlanma katıldığında, etki alanı adını da belirtmeniz gerekir `<AAD-Domain>` ve kümeye erişmek için gerekli izinlere sahip bir etki alanı kullanıcı hesabı adını `<username>`:

```bash
kinit <username>
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
```

Değiştirin `<username>` kümeye erişmek için gerekli izinlere sahip bir etki alanı hesabı adı ile. Değiştirin `<AAD-DOMAIN>` adla, Azure Active Directory (küme katılmış olduğu AAD). Kullanmak için bir büyük harf dizesi `<AAD-DOMAIN>` değerini, aksi takdirde kimlik bilgisi olmaz bulunabilir. Denetleme `/etc/krb5.conf` gerekirse bölge adları için.

---

### <a name="over-public-internet"></a>Genel internet üzerinden

Genel internet üzerinden bağlanırken, küme oturum açma hesap adı sağlamanız gerekir (varsayılan `admin`) ve parola. Örneğin, bağlanmak için bir istemci sisteminden Beeline kullanma `<clustername>.azurehdinsight.net` adresi. Bu bağlantı, bağlantı noktası üzerinden yapılan `443`ve SSL kullanarak şifrelenir:

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
```

`clustername` değerini HDInsight kümenizin adıyla değiştirin. Değiştirin `admin` kümeniz için küme oturum açma hesabı ile. Değiştirin `password` ile küme oturum açma hesabı için parola.

---

### <a id="sparksql"></a>Beeline ile Apache Spark kullanma

Apache Spark, Spark Thrift sunucusu bazen denir HiveServer2 kendi uygulamasını sağlar. Bu hizmet, Spark SQL sorguları Hive yerine çözümlemek için kullanır ve sorgunuzu bağlı olarak daha iyi performans sağlayabilir.

#### <a name="over-public-internet-with-apache-spark"></a>Apache Spark ile genel internet üzerinden

İnternet üzerinden bağlanırken kullanılan bağlantı dizesi biraz farklıdır. İçeren yerine `httpPath=/hive2` olduğu `httpPath/sparkhive2`:

```bash 
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p password
```

---

#### <a name="from-cluster-head-or-inside-azure-virtual-network-with-apache-spark"></a>Gelen baş veya iç Azure sanal ağ ile Apache Spark küme

Doğrudan küme baş düğümüne veya HDInsight kümesi olarak aynı Azure sanal ağ içindeki bir kaynağa bağlanırken, bağlantı noktası `10002` Spark Thrift sunucusu yerine kullanılması gereken `10001`. Aşağıdaki örnek, doğrudan baş düğümüne bağlanmak gösterilmektedir:

```bash
beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

---

## <a id="prereq"></a>Önkoşullar

* HDInsight Hadoop kümesinde. Bkz: [Linux'ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).

* Bildirim [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kümenizin birincil depolama. Örneğin, `wasb://` Azure depolama için `abfs://` için Azure Data Lake depolama Gen2 veya `adl://` Azure Data Lake depolama Gen1 için. Güvenli aktarım, Azure Depolama'da veya Data Lake depolama Gen2 için etkinse URI'dir `wasbs://` veya `abfss://`sırasıyla. Daha fazla bilgi için [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).


* 1. seçenek: Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md). Bu belgedeki adımlarda çoğunu Beeline kümeye bir SSH oturumundan kullandığınız varsayılır.

* 2. seçenek:  Yerel Beeline istemci.


## <a id="beeline"></a>Bir Hive sorgusu çalıştırma

Bu örnek, bir SSH bağlantısı Beeline istemciden kullanarak temel alır.

1. Aşağıdaki kod ile küme için bir SSH bağlantısı açın. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabı için parolayı girin.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. HiveServer2 ile Beeline istemciniz, açık SSH oturumunda aşağıdaki komutu girerek bağlantı:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Beeline komutları ile başlayan bir `!` karakter, örneğin `!help` Yardımı görüntüler. Ancak `!` bazı komutlar için atlanabilir. Örneğin, `help` de çalışır.

    Var olan bir `!sql`, HiveQL ifadelerini yürütmek için kullanılır. Ancak, önceki atlayabilirsiniz kadar sık HiveQL kullanılır `!sql`. Aşağıdaki iki deyimler eşdeğerdir:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Yeni kümede, yalnızca bir tabloya listelenir: **hivesampletable**.

4. Hivesampletable şemasını görüntülemek için aşağıdaki komutu kullanın:

    ```hiveql
    describe hivesampletable;
    ```

    Bu komut, aşağıdaki bilgileri döndürür:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Bu bilgiler, tablodaki sütun açıklar.

5. Adlı bir tablo oluşturmak için aşağıdaki deyimleri girin **log4jLogs** HDInsight kümesi ile sağlanan örnek verileri kullanarak: (Gerektiğinde gözden geçirme temel alarak, [URI şeması](../hdinsight-hadoop-linux-information.md#URI-and-scheme).)

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs 
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' 
        GROUP BY t4;
    ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

    * `DROP TABLE` -Tablo varsa, silinir.

    * `CREATE EXTERNAL TABLE` -Oluşturur bir **dış** Hive tablosunda. Dış tablolar, yalnızca Hive tablo tanımı depolayın. Verileri özgün konumunda bırakılır.

    * `ROW FORMAT` -Nasıl veri biçimlendirilir. Bu durumda, her günlük alanlar boşlukla ayrılır.

    * `STORED AS TEXTFILE LOCATION` -Veri depolandığı ve hangi dosya biçimi.

    * `SELECT` -Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu sorgunun döndürdüğü değeri **3** olarak bu değeri içeren üç satır.

    * `INPUT__FILE__NAME LIKE '%.log'` -Hive, dizindeki tüm dosyaları şema uygulamak çalışır. Bu durumda, dizin şemasını eşleşmeyen dosyalarını içerir. Çöp veri sonuçları engellemek için bu bildirimi Hive, yalnızca veri sonu dosyalarından dönmesi gerektiğini söyler. günlük.

   > [!NOTE]  
   > Dış tablolar, temel alınan veriler dış bir kaynak tarafından güncelleştirilmesi beklediğiniz kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya MapReduce işlemi.
   >
   > Bir dış tablo bırakılırken mu **değil** verileri, yalnızca tablo tanımını silin.

    Bu komutun çıktısı şu metne benzer:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

6. Beeline'ndan çıkmak için kullanmak `!exit`.

## <a id="file"></a>HiveQL dosyasını çalıştırın

Önceki örnekte bir devamlılık budur. Bir dosya oluşturun ve ardından Beeline kullanarak çalıştırmak için aşağıdaki adımları kullanın.

1. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **query.hql**:

    ```bash
    nano query.hql
    ```

2. Aşağıdaki metin dosyasının içeriğini kullanın. Adlı yeni bir 'internal' tablo bu sorgu oluşturur **günlüklerini**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu deyimler, aşağıdaki eylemleri gerçekleştirin:

   * **OLUŞTURMA IF NOT TABLOSUNUN VAROLDUĞUNU** -tablo zaten mevcut değilse oluşturulur. Bu yana **dış** anahtar sözcüğü kullanılmazsa, bu deyimi iç tablo oluşturur. İç tablolar Hive veri ambarı'nda depolanır ve yığın tarafından tamamen yönetilir.
   * **DEPOLANAN AS ORC** -en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde veri depolar. ORC biçimi, Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.
   * **INSERT ÜZERİNE... SEÇİN** -satırları seçer **log4jLogs** içeren tablo **[Hata]**, ardından verileri ekler **günlüklerini** tablo.

    > [!NOTE]  
    > Dış tablolar, iç tablo bırakılırken, temel alınan verileri de siler.

3. Dosyayı kaydetmek için kullanın **Ctrl**+**_X**, enter **Y**ve son olarak **Enter**.

4. Beeline'ı kullanarak dosyayı çalıştırmak için aşağıdakileri kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > `-i` Parametre Beeline başlar ve ifadeleri çalıştırır `query.hql` dosya. Sorgu tamamlandıktan sonra ulaşırsınız `jdbc:hive2://headnodehost:10001/>` istemi. Kullanarak bir dosyaya da çalıştırabilirsiniz `-f` sorgu tamamlandıktan sonra Beeline çıkar parametresi.

5. Doğrulamak için **günlüklerini** tablonun oluşturulma, bulunan tüm satırlar döndürülecek aşağıdaki deyimini **günlüklerini**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Üç veri satırı döndürülmesi, tüm içeren **[Hata]** sütun t4 içinde:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)




## <a id="summary"></a><a id="nextsteps"></a>Sonraki adımlar

HDInsight Hive hakkında daha fazla genel bilgi için aşağıdaki belgesine bakın:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive'ı kullanma](hdinsight-use-hive.md)

HDInsight üzerinde Hadoop kullanmaya çalışabilir diğer yollar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight üzerinde Apache Hadoop ile Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[apache-tez]: https://tez.apache.org
[apache-hive]: https://hive.apache.org/
[apache-log4j]: https://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: https://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md

[putty]: https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: https://technet.microsoft.com/library/ee692792.aspx
