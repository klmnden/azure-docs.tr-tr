---
title: Toplu psql - Azure Hdınsight kullanarak Apache Phoenix yükleme | Microsoft Docs
description: Toplu Yükleme veri Phoenix tablolara yüklemek için psql Aracı'nı kullanın.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ashishth
ms.openlocfilehash: 2c192707c6cf8f84d2ca1c0307770cadd5cdb8bd
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="bulk-load-data-into-phoenix-using-psql"></a>Psql kullanarak Phoenix’e toplu veri yükleme

[Apache Phoenix](http://phoenix.apache.org/) yüksek düzeyde paralel ilişkisel veritabanı üzerinde oluşturulan bir açık kaynaklı [HBase](../hbase/apache-hbase-overview.md). Phoenix, HBase üzerinde SQL benzeri sorguları sağlar. Phoenix, kullanıcıların oluşturma, silme ve SQL tablolar, dizinler, görünümler ve dizileri ve upsert satır ayrı ayrı ve toplu alter etkinleştirmek için JDBC sürücüleri kullanır. Phoenix, HBase üzerinde düşük gecikme süreli uygulamaları oluşturmak için noSQL yerel derleme sorgular derlemek için MapReduce kullanmak yerine kullanır. Phoenix, verilerle birlikte bulunan kod yürütmek sunucu adres alanında istemci tarafından sağlanan kod çalıştırılmasını desteklemek üzere birlikte işlemciler ekler. Bu istemci/sunucu veri aktarımını en aza indirir.  Phoenix Hdınsight'ta kullanarak verilerle çalışmak için ilk tablo oluşturma ve bunların içine veri yükleme.

## <a name="bulk-loading-with-phoenix"></a>Toplu Phoenix ile yükleme

İstemci API'ları, bir MapReduce işi TableOutputFormat ile kullanarak veya el ile HBase Kabuğu'nu kullanarak veri giriş yapma dahil olmak üzere HBase veri almak için birden çok yolu vardır. Phoenix CSV verileri Phoenix tablolara yüklemek için iki yöntem sunar: adlı aracı yüklenirken bir istemci `psql`ve MapReduce tabanlı toplu yükleme aracı.

`psql` Aracı tek iş parçacıklı ve megabayt veya gigabayt veri yüklemek için uygundur. Yüklenecek tüm CSV dosyaları '.csv' uzantısına sahip olmalıdır.  SQL komut dosyaları da belirtebilirsiniz `psql` '.sql' dosya uzantısına sahip komut satırı.

Çoklu iş parçacığı MapReduce kullanıyorsa olarak toplu ile MapReduce yükleme kadar büyük veri birimleri, genellikle üretim senaryoları için kullanılır.

Veri yükleme başlamadan önce Phoenix etkin olduğunu ve sorgu zaman aşımı ayarlarının beklendiği gibi olduğunu doğrulayın.  Hdınsight küme Ambari panonuz erişmek için HBase ve yapılandırma sekmesini seçin.  Apache Phoenix değerine ayarlandığını doğrulamak için aşağı kaydırmanız `enabled` gösterildiği gibi:

![Apache Phoenix Hdınsight küme ayarları](./media/apache-hbase-phoenix-psql/ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Kullanım `psql` toplu yük tablolar

1. Yeni bir tablo oluşturun, sonra sorgunuzu dosya adıyla kaydedin `createCustomersTable.sql`.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

2. CSV dosyanız (gösterilen örnek içeriği) kopyalamak `customers.csv` içine bir `/tmp/` , yeni oluşturulan tabloya yüklenmesi için dizin.  Kullanım `hdfs` CSV dosyası, istenen kaynak konumuna kopyalamak için komutu.

    ```
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,Norway
    ... 4997 more rows 
    ```

    ```bash
    hdfs dfs -copyToLocal /example/data/customers.csv /tmp/
    ```

3. Giriş verisi yüklenen düzgün bir şekilde doğrulamak için bir SQL SELECT sorgu oluşturun, sonra sorgunuzu dosya adıyla kaydedin `listCustomers.sql`. Herhangi bir SQL sorgu kullanabilirsiniz.
     ```sql
    SELECT Name, Income from Customers group by Country;
    ```

4. Toplu Yükleme veri açarak bir *yeni* Hadoop komut penceresi. İlk yürütme dizin konumu ile değiştirin `cd` komutunu ve ardından `psql` Aracı (Python `psql.py` komutu). 

    Aşağıdaki örnek, kopyaladığınız bekliyor `customers.csv` kullanarak yerel temp dizini için bir depolama hesabı dosyasından `hdfs` yukarıdaki adım 2 olduğu gibi.

    ```bash
    cd /usr/hdp/current/phoenix-client/bin

    python psql.py ZookeeperQuorum createCustomersTable.sql /tmp/customers.csv listCustomers.sql
    ```

    > [!NOTE] 
    > Belirlemek için `ZookeeperQuorum` adı, zookeeper çekirdek dize dosyasında bulun `/etc/hbase/conf/hbase-site.xml` özellik adıyla `hbase.zookeeper.quorum`.

5. Sonra `psql` işlemi tamamlandı, komut penceresinde bir ileti görürsünüz:

    ```
    CSV Upsert complete. 5000 rows upserted
    Time: 4.548 sec(s)
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>MapReduce toplu yük tablolara kullanın

Küme üzerinde dağıtılmış daha yüksek verimlilik yükleme için MapReduce Yükleme aracını kullanın. Bu yükleyici önce tüm veri HFiles dönüştürür ve ardından HBase için oluşturulan HFiles sağlar.

1. CSV MapReduce yükleyici kullanarak başlatma `hadoop` Phoenix istemci jar komutunu:

    ```bash
    hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table CUSTOMERS --input /data/customers.csv
    ```

2. Bir SQL deyimi ile yeni bir tablo oluşturmak gibi `CreateCustomersTable.sql` önceki adım 1.

3. Tablonuz şeması doğrulamak için çalıştırın `!describe inputTable`.

4. Konum yolu örnek gibi giriş verilerinize belirlemek `customers.csv` dosya. Giriş dosyaları WASB/ADLS depolama hesabınızdaki olabilir. Bu örnek senaryoda, girdi dosyaları bulunan `<storage account parent>/inputFolderBulkLoad` dizin.

5. MapReduce toplu yükleme komutu yürütme dizinine değiştirin:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

6. Bulun, `ZookeeperQuorum` değeri `/etc/hbase/conf/hbase-site.xml`, özellik adı ile `hbase.zookeeper.quorum`.

7. Sınıf ve Çalıştır ayarlama `CsvBulkLoadTool` aracı komutu:

    ```bash
    /usr/hdp/current/phoenix-client$ HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /inputFolderBulkLoad/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure
    ```

8. MapReduce ADLS ile kullanılacak olan ADLS kök dizini bulun `hbase.rootdir` değeri `hbase-site.xml`. Aşağıdaki komutta ADLS kök dizindir `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. Bu komutta ADLS giriş belirtin ve parametre olarak klasörleri çıktı:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

## <a name="recommendations"></a>Öneriler

* Aynı depolama ortamına giriş ve çıkış klasörleri için WASB veya ADLS kullanın. ADLS için WASB veri aktarımı için kullanabileceğiniz `distcp` komutu:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Daha büyük boyutu alt düğümleri kullanın. MapReduce toplu kopyalama harita işlemlerinin DFS olmayan kullanılabilir alanı dolduracak büyük miktarda geçici çıktı üretir. Toplu yükleme, büyük bir miktarını ve daha büyük boyutu daha fazla alt düğümleri kullanın. Kümeniz için doğrudan tahsis çalışan düğüm sayısı, işlemci hızı etkiler.

* Giriş dosyaları ~ 10 GB parçalara Böl. Toplu yükleme, bu nedenle daha iyi performans öbekleri sonuçlarında içine, giriş dosyalarınızın bölme depolama yoğunluklu işlemdir.

* Bölge sunucu etkin kaçının. Satır anahtarınızın düz olarak artan, HBase sıralı yazma bölge sunucu hotspotting anlamına. *Katarak* satır anahtarını sıralı yazma azaltır. Phoenix saydam aşağıda başvurulan belirli bir tablo için satır anahtarını içeren bir salting bayt salt için bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu veri Apache Phoenix ile yükleme](http://phoenix.apache.org/bulk_dataload.html)
* [Hdınsight'ta Linux tabanlı HBase kümeleriyle Apache Phoenix kullanın](../hbase/apache-hbase-phoenix-squirrel-linux.md)
* [Güvenlik tabloları](https://phoenix.apache.org/salted.html)
* [Phoenix dilbilgisi](http://phoenix.apache.org/language/index.html)
