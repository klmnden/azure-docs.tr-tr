---
title: "Apache Hive - Azure Hdınsight ile Beeline kullanma | Microsoft Docs"
description: "Hdınsight'ta Hadoop ile Hive sorguları çalıştırmak için Beeline istemci kullanmayı öğrenin. Beeline JDBC HiveServer2 ile çalışmaya yönelik bir yardımcı programdır."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: beeline hive, hive beeline
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/20/2017
ms.author: larryfr
ms.openlocfilehash: 7c582c81aac889b2b6f57777fab4531107e0fad3
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-the-beeline-client-with-apache-hive"></a>Apache Hive ile Beeline İstemcisi'ni kullanın

Nasıl kullanacağınızı öğrenin [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Hdınsight'ta Hive sorguları çalıştırmak için.

Beeline, Hdınsight kümesi baş düğümler üzerinde bulunan bir Hive istemcidir. Beeline JDBC HiveServer2, Hdınsight kümenize üzerinde barındırılan bir hizmete bağlanmak için kullanır. Beeline, hdınsight'ta Hive internet üzerinden uzaktan erişim için de kullanabilirsiniz. Aşağıdaki örnekler Hdınsight'a Beeline bağlanmak için kullanılan en yaygın bağlantı dizeleri sağlar:

* __Beeline headnode veya edge düğüm için bir SSH bağlantısı kullanarak__:`-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'`
* __Hdınsight için bir Azure sanal ağı üzerinden bağlanan bir istemci Beeline kullanarak__:`-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`
* __Hdınsight'a genel internet üzerinden bağlanan bir istemci Beeline kullanarak__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password`

> [!NOTE]
> Değiştir `admin` kümeniz için küme oturum açma hesabıyla.
>
> Değiştir `password` küme oturum açma hesabının parolası ile.
>
> `clustername` değerini HDInsight kümenizin adıyla değiştirin.
>
> Bir sanal ağ üzerinden kümeye bağlanırken Değiştir `<headnode-FQDN>` küme headnode tam etki alanı adı.

## <a id="prereq"></a>Önkoşullar

* Linux tabanlı Hadoop Hdınsight kümesi üzerinde.

  > [!IMPORTANT]
  > Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Bir SSH istemcisi veya yerel bir Beeline istemci. Bu belgede yer alan adımlar çoğunu Beeline kümeye bir SSH oturumunda kullandığınız varsayılır. Küme dışından gelen Beeline çalıştırma hakkında daha fazla bilgi için bkz: [Beeline uzaktan kullanmak](#remote) bölümü.

    SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Beeline kullanın

1. Beeline başlatırken Hdınsight kümenize HiveServer2 için bir bağlantı dizesi belirtmeniz gerekir:

    * Ortak internet üzerinden bağlanırken, küme oturum açma hesabı adı sağlamanız gerekir (varsayılan `admin`) ve parola. Örneğin, bağlanmak için bir istemci sistemden Beeline kullanarak `<clustername>.azurehdinsight.net` adresi. Bu bağlantı bağlantı noktası üzerinden yapılan `443`ve SSL kullanılarak şifrelenir:

        ```bash
        beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password
        ```

    * Bir SSH oturumunda bir küme headnode bağlanırken bağlanabileceği `headnodehost` bağlantı noktası adresi `10001`:

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    * Bir Azure sanal ağı üzerinden bağlanırken, küme baş düğümüne tam etki alanı adını (FQDN) sağlamalısınız. Bu bağlantıyı doğrudan küme düğümlerine yapıldığından bağlantı bağlantı noktası kullanan `10001`:

        ```bash
        beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
        ```

2. Beeline komutları ile başlar bir `!` karakter, örneğin `!help` Yardım gösterir. Ancak `!` bazı komutlar için atlanabilir. Örneğin, `help` de çalışır.

    Var olan bir `!sql`, HiveQL deyimlerini yürütmek için kullanılır. Ancak, önceki atlayabilirsiniz kadar sık HiveQL kullanılır `!sql`. Aşağıdaki iki ifade eşdeğerdir:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Yeni kümede, yalnızca bir tablo listelenir: **hivesampletable**.

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

    Bu bilgiler tablodaki sütunların açıklar. Biz bu verileri bazı sorgular gerçekleştirebilir olsa da, bunun yerine verileri Hive yüklemek ve bir şema uygulamak nasıl göstermek için yepyeni bir tablo oluşturalım.

4. Adlı bir tablo oluşturmak için aşağıdaki ifadeleri girin **log4jLogs** Hdınsight kümesi ile sağlanan örnek verileri kullanarak:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

    * `DROP TABLE`-Tablo varsa, bu silinir.

    * `CREATE EXTERNAL TABLE`-Oluşturur bir **dış** Hive tablo. Dış tablolara tablo tanımı kovanında yalnızca depolar. Veriler özgün konumda kalır.

    * `ROW FORMAT`-Nasıl veri biçimlendirilir. Bu durumda, her günlüğün içinde alanlar boşlukla ayrılır.

    * `STORED AS TEXTFILE LOCATION`-Verinin depolanma biçimini ve hangi dosya biçiminde.

    * `SELECT`-Tüm satırların sayımını seçer Burada sütun **t4** değeri içeren **[Hata]**. Bu sorgunun döndürdüğü değeri **3** bu değeri içeren üç satır olarak.

    * `INPUT__FILE__NAME LIKE '%.log'`-Dizindeki tüm dosyaları şema uygulamak hive çalışır. Bu durumda, dizin şeması eşleşmiyor dosyalarını içerir. Çöp veri sonuçlarında önlemek için bu bildirimi Hive biz yalnızca veri biten dosyalarından döndürmesi gerektiğini bildirir. günlük.

  > [!NOTE]
  > Dış kaynak tarafından güncelleştirilecek temel alınan veri beklediğiniz dış tablolara kullanılmalıdır. Örneğin, bir otomatik veri karşıya yükleme işlemi veya bir MapReduce işlemi.
  >
  > Bir dış tablo bırakma mu **değil** verileri, yalnızca tablo tanımını silin.

    Bu komutun çıktısı aşağıdakine benzer:

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

5. Beeline çıkmak için kullanmak `!exit`.

## <a id="file"></a>HiveQL dosyasını çalıştırmak için Beeline kullanın

Bir dosya oluşturun, sonra Beeline kullanarak çalıştırmak için aşağıdaki adımları kullanın.

1. Adlı bir dosya oluşturmak için aşağıdaki komutu kullanın **query.hql**:

    ```bash
    nano query.hql
    ```

2. Aşağıdaki metin dosyasının içeriği kullanın. Bu sorgu adlı yeni bir 'iç' tablo oluşturur **günlüklerini**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu ifadeler aşağıdaki eylemleri gerçekleştirin:

    * **Tablo IF değil var oluşturmak** -tablosu zaten mevcut değilse oluşturulur. Bu yana **dış** anahtar sözcüğü kullanılmaz, bu deyim bir iç tablosu oluşturur. İç tabloları Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir.
    * **AS ORC DEPOLANAN** -en iyi duruma getirilmiş satır sütunlu (ORC) biçiminde verileri depolar. ORC biçimi Hive verilerini depolamak için yüksek oranda en iyi duruma getirilmiş ve verimli bir biçimidir.
    * **INSERT ÜZERİNE... SEÇİN** -satırları seçer **log4jLogs** içeren tablo **[Hata]**, verileri ekler **günlüklerini** tablo.

    > [!NOTE]
    > Dış tablolara, bir iç tablosu bırakarak temel alınan veri de siler.

3. Dosyayı kaydetmek için kullanın **Ctrl**+**_X**, enter **Y**ve son olarak **Enter**.

4. Beeline kullanarak dosyayı çalıştırmak için aşağıdakileri kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > `-i` Parametre Beeline başlatır, query.hql dosyasında ifadeleri çalıştırır. Sorgu işlemi tamamlandıktan sonra gelmesini `jdbc:hive2://headnodehost:10001/>` istemi. Kullanarak bir dosyaya da çalıştırabilirsiniz `-f` sorgu tamamlandıktan sonra Beeline çıkar parametresi.

5. Doğrulamak için **günlüklerini** tablosu oluşturuldu, tüm satırların döndürmek için şu deyimi kullanın **günlüklerini**:

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

## <a id="remote"></a>Beeline uzaktan kullanın

Yerel olarak yüklenmiş Beeline varsa ve ortak internet üzerinden bağlanma, aşağıdaki parametreleri kullanın:

* __Bağlantı dizesi__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Küme oturum açma adı__:`-n admin`

* __Küme oturum açma parolasını__`-p 'password'`

Değiştir `clustername` Hdınsight kümenizin adıyla bağlantı dizesinde.

Değiştir `admin` küme oturum açma ve değiştirme adıyla `password` , küme oturum açma parolası ile.

Yerel olarak yüklenmiş Beeline varsa ve bir Azure sanal ağ üzerinden bağlanma, aşağıdaki parametreleri kullanın:

* __Bağlantı dizesi__:`-u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'`

Bir headnode tam etki alanı adını bulmak için yer alan bilgileri kullanın. [Ambari REST API kullanarak Hdınsight'ta yönetmek](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) belge.

## <a id="sparksql"></a>Spark ile Beeline kullanın

Spark Spark Thrift sunucusu olarak genellikle adlandırılır HiveServer2 kendi uygulamasını sağlar. Bu hizmet, Spark SQL sorguları Hive yerine çözümlemek için kullanır ve sorgunuzu bağlı olarak daha iyi performans sağlayabilir.

Spark Hdınsight kümesi üzerinde Spark Thrift sunucusuna bağlanmak için bağlantı noktası kullanmak `10002` yerine `10001`. Örneğin, `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.

> [!IMPORTANT]
> Spark Thrift sunucusunun internet üzerinden doğrudan erişilebilir değil. İçin bir SSH oturumundan veya Hdınsight kümesi aynı Azure sanal ağ içinde yalnızca bağlanabilir.

## <a id="summary"></a><a id="nextsteps"></a>Sonraki adımlar

Hdınsight'ta Hive hakkında daha fazla genel bilgi için aşağıdaki belgesine bakın:

* [Hdınsight'ta Hadoop ile Hive kullanma](hdinsight-use-hive.md)

Hdınsight'ta Hadoop ile çalışabilirsiniz diğer yollar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Hdınsight'ta Hadoop ile pig kullanma](hdinsight-use-pig.md)
* [Hdınsight'ta Hadoop ile MapReduce kullanma](hdinsight-use-mapreduce.md)

Tez Hive ile kullanıyorsanız, aşağıdaki belgelere bakın:

* [Windows tabanlı Hdınsight üzerinde Tez kullanıcı arabirimini kullanma](../hdinsight-debug-tez-ui.md)
* [Linux tabanlı Hdınsight üzerinde Ambari Tez görünümünü kullanın](../hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
