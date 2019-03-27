---
title: Apache Hive - Azure HDInsight ile Apache Beeline kullanma
description: Beeline istemci HDInsight Hadoop ile Hive sorguları çalıştırmak için kullanmayı öğrenin. Beeline JDBC HiveServer2 ile çalışmak için bir yardımcı programdır.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: hrasheed
ms.openlocfilehash: 392c34e1896106c39b31876308084ef4fd6a7e54
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449044"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Apache Hive ile Apache Beeline istemcisini kullanma

Nasıl kullanacağınızı öğrenin [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) HDInsight üzerinde Apache Hive sorguları çalıştırmak için.

Beeline HDInsight kümenizin baş düğümlerine dahil edilmiş bir Hive istemcisidir. Beeline JDBC HiveServer2, HDInsight kümeniz üzerinde barındırılan bir hizmete bağlanmak için kullanır. Beeline HDInsight üzerindeki Hive'a internet üzerinden uzaktan erişmek için de kullanabilirsiniz. Aşağıdaki örneklerde, HDInsight için Beeline bağlanmak için kullanılan en yaygın bağlantı dizelerini sağlanır:

* __Beeline bir baş veya kenar düğümüne SSH bağlantısından kullanarak__: `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'`

* __HDInsight için bir Azure sanal ağ üzerinden bağlanan bir istemci, Beeline kullanma__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

* __Beeline bir Azure sanal ağ üzerinden bir HDInsight Kurumsal güvenlik paketi (ESP) kümesine bağlanma, bir istemci kullanarak__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-DOMAIN>;auth-kerberos;transportMode=http' -n <username>` 

* __HDInsight için genel internet üzerinden bağlanan bir istemci, Beeline kullanma__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password`

> [!NOTE]  
> Değiştirin `admin` kümeniz için küme oturum açma hesabı ile.
>
> Değiştirin `password` ile küme oturum açma hesabı için parola.
>
> `clustername` değerini HDInsight kümenizin adıyla değiştirin.
>
> Bir sanal ağ üzerinden kümeye bağlanırken değiştirin `<headnode-FQDN>` ile küme baş düğümüne tam etki alanı adı.
>
> Bir kurumsal güvenlik paketi (ESP) kümeye bağlanırken değiştirin `<AAD-DOMAIN>` adla, Azure Active Directory (küme katılmış olduğu AAD). Kullanmak için bir büyük harf dizesi `<AAD-DOMAIN>` değerini, aksi takdirde kimlik bilgisi bulunamıyor. Denetleme `/etc/krb5.conf` gerekirse bölge adları için. Değiştirin `<username>` kümeye erişmek için gerekli izinlere sahip bir etki alanı hesabı adı ile. 

## <a id="prereq"></a>Önkoşullar

* Linux tabanlı Hadoop HDInsight kümesi sürüm 3.4 üzerindeki.

  > [!IMPORTANT]  
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi veya yerel Beeline istemci. Bu belgedeki adımlarda çoğunu Beeline kümeye bir SSH oturumundan kullandığınız varsayılır. Kümenin dışından gelen Beeline çalıştırma hakkında daha fazla bilgi için bkz: [Beeline'ı uzaktan kullanma](#remote) bölümü.

    SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Bir Hive sorgusu çalıştırma

1. Beeline başlatırken, HDInsight kümenizi HiveServer2 için bir bağlantı dizesi sağlamanız gerekir:

    * Genel internet üzerinden bağlanırken, küme oturum açma hesap adı sağlamanız gerekir (varsayılan `admin`) ve parola. Örneğin, bağlanmak için bir istemci sisteminden Beeline kullanma `<clustername>.azurehdinsight.net` adresi. Bu bağlantı, bağlantı noktası üzerinden yapılan `443`ve SSL kullanarak şifrelenir:

        ```bash
        beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
        ```

    * Bir SSH oturumunda bir küme baş düğümüne bağlanırken, bağlanabileceğiniz `headnodehost` adresi üzerinde bağlantı noktası `10001`:

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    * Bir Azure sanal ağ üzerinden bağlanırken, küme baş düğümü tam etki alanı adını (FQDN) sağlamalısınız. Bu bağlantıyı doğrudan küme düğümlerinde yapıldığından, bağlantı, bağlantı noktasını kullanır. `10001`:

        ```bash
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
        ```
    * Azure Active Directory (AAD) bir kurumsal güvenlik paketi (ESP) kümesine bağlanma katıldığında, etki alanı adını da belirtmeniz gerekir `<AAD-Domain>` ve kümeye erişmek için gerekli izinlere sahip bir etki alanı kullanıcı hesabı adını `<username>`:
        
        ```bash
        kinit <username>
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
        ```

2. Beeline komutları ile başlayan bir `!` karakter, örneğin `!help` Yardımı görüntüler. Ancak `!` bazı komutlar için atlanabilir. Örneğin, `help` de çalışır.

    Var olan bir `!sql`, HiveQL ifadelerini yürütmek için kullanılır. Ancak, önceki atlayabilirsiniz kadar sık HiveQL kullanılır `!sql`. Aşağıdaki iki deyimler eşdeğerdir:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Yeni kümede, yalnızca bir tabloya listelenir: **hivesampletable**.

3. Hivesampletable şemasını görüntülemek için aşağıdaki komutu kullanın:

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

4. Adlı bir tablo oluşturmak için aşağıdaki deyimleri girin **log4jLogs** HDInsight kümesi ile sağlanan örnek verileri kullanarak:

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
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
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

5. Beeline'ndan çıkmak için kullanmak `!exit`.

### <a id="file"></a>HiveQL dosyasını çalıştırmak için Beeline kullanma

Bir dosya oluşturun ve ardından Beeline kullanarak çalıştırmak için aşağıdaki adımları kullanın.

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

## <a id="remote"></a>Beeline'ı uzaktan kullanma

Beeline yerel olarak yüklü olan ve genel internet üzerinden bağlanma, aşağıdaki parametreleri kullanın:

* __Bağlantı dizesi__: `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Küme oturum açma adı__: `-n admin`

* __Küme oturum açma parolası__ `-p 'password'`

Değiştirin `clustername` HDInsight kümenizin adıyla bağlantı dizesinde.

Değiştirin `admin` küme oturum açma ve Değiştir adıyla `password` , küme oturum açma parolası ile.

Beeline yerel olarak yüklü olması ve bir Azure sanal ağ üzerinden bağlanma, aşağıdaki parametreleri kullanın:

* __Bağlantı dizesi__: `-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

Bir baş düğüm tam etki alanı adını bulmak için yer alan bilgileri kullanın. [Apache Ambari REST API'yi kullanarak HDInsight yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belge.

## <a id="sparksql"></a>Beeline ile Apache Spark kullanma

Apache Spark, Spark Thrift sunucusu bazen denir HiveServer2 kendi uygulamasını sağlar. Bu hizmet, Spark SQL sorguları Hive yerine çözümlemek için kullanır ve sorgunuzu bağlı olarak daha iyi performans sağlayabilir.

__Bağlantı dizesi__ internet üzerinden bağlanma biraz farklı olduğunda kullanılır. İçeren yerine `httpPath=/hive2` olduğu `httpPath/sparkhive2`. İnternet üzerinden bağlanan bir örnek verilmiştir:

```bash 
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p password
```

Doğrudan küme baş düğümüne veya HDInsight kümesi olarak aynı Azure sanal ağ içindeki bir kaynağa bağlanırken, bağlantı noktası `10002` Spark Thrift sunucusu yerine kullanılması gereken `10001`. Baş düğüme bağlanma örneği verilmiştir:

```bash
beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

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
