---
title: Yükleme Apache psql - Azure HDInsight kullanarak phoenix'e toplu
description: Toplu veri yükleme Phoenix tablolara yüklemek için psql aracını kullanın.
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ashishth
ms.openlocfilehash: fe6705dc3dedd521357f924dd433c7eacf88ba84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64718634"
---
# <a name="bulk-load-data-into-apache-phoenix-using-psql"></a>Psql kullanarak Apache Phoenix’e toplu veri yükleme

[Apache Phoenix](https://phoenix.apache.org/) yüksek düzeyde paralel ilişkisel veritabanı üzerinde oluşturulmuş bir açık kaynak [Apache HBase](../hbase/apache-hbase-overview.md). Phoenix, HBase üzerinde SQL benzeri sorguları sağlar. Phoenix, kullanıcıların oluşturma, silme ve ayrı ayrı ve toplu SQL tablolarını, dizinleri, görünümleri ve sıraları ve upsert satırları alter sağlamak için JDBC sürücüleri kullanır. Phoenix, HBase üzerinde düşük gecikme süreli uygulamalar oluşturmak için noSQL yerel derleme sorgular derlemek için MapReduce kullanmak yerine kullanır. Phoenix, verilerle birlikte bulunan bir kodu yürüten sunucuya adres alanında istemci tarafından sağlanan kod çalıştırılmasını desteklemek üzere ortak işlemciler ekler. Bu istemci/sunucu veri aktarımını en aza indirir.  Phoenix HDInsight kullanarak verilerle çalışmak için önce tablolar oluşturun ve bunları veri yükleme.

## <a name="bulk-loading-with-apache-phoenix"></a>Toplu Apache Phoenix ile yükleme

İçinde HBase kullanarak istemci API'leri, bir MapReduce işi TableOutputFormat ile veya el ile HBase Kabuğu'nu kullanarak veri giriş yapma gibi veri almak için birden çok yolu vardır. Phoenix, CSV verileri Phoenix tablolara yüklemek için iki yöntem sunar: bir istemci yükleme adlı aracı `psql`ve MapReduce tabanlı toplu yükleme aracı.

`psql` Aracı tek iş parçacıklı ve megabayt veya gigabayt veri yüklemek için idealdir. Yüklenen tüm CSV dosyalarını '.csv' dosya uzantısı olmalıdır.  SQL komut dosyalarında belirtebilirsiniz `psql` '.sql' dosya uzantısına sahip komut satırı.

MapReduce birden çok iş parçacığı kullanan olarak yüklenmesi ile MapReduce toplu üretim senaryoları, genellikle çok daha yüksek veri hacimleri için kullanılır.

Veri yükleme başlamadan önce Phoenix etkin olduğundan ve sorgu zaman aşımı ayarları beklendiği gibi olduğunu doğrulayın.  HDInsight kümenize erişirken [Apache Ambari](https://ambari.apache.org/) Pano, HBase seçin ve ardından yapılandırma sekmesinde.  Apache Phoenix değerine ayarlandığını doğrulamak için aşağı kaydırarak `enabled` gösterildiği gibi:

![Apache Phoenix HDInsight küme ayarları](./media/apache-hbase-phoenix-psql/ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Kullanım `psql` toplu yük tablolara

1. Yeni bir tablo oluşturun, ardından sorgunuza adıyla kaydedin `createCustomersTable.sql`.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

2. CSV dosyanız (gösterilen örnek içeriği) kopyalamak `customers.csv` içine bir `/tmp/` dizin için yeni oluşturulan tablosu yükleniyor.  Kullanım `hdfs` CSV dosyası, istenen kaynak konumuna Kopyala komutu.

    ```
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,Norway
    ... 4997 more rows 
    ```

    ```bash
    hdfs dfs -copyToLocal /example/data/customers.csv /tmp/
    ```

3. Giriş verilerini yüklenen düzgün bir şekilde doğrulamak için bir SQL SELECT sorgusu oluşturun ve ardından adıyla sorgunuzu kaydedin `listCustomers.sql`. Herhangi bir SQL sorgusu kullanabilirsiniz.
     ```sql
    SELECT Name, Income from Customers group by Country;
    ```

4. Toplu Yükleme veri açarak bir *yeni* Hadoop komut penceresi. İlk yürütme dizin konumu ile değiştirin `cd` komutunu ve ardından `psql` Aracı (Python `psql.py` komutu). 

    Aşağıdaki örnek, kopyaladığınız bekliyor `customers.csv` yerel geçici dizin kullanarak bir depolama hesabından dosya `hdfs` yukarıdaki 2. adım olduğu gibi.

    ```bash
    cd /usr/hdp/current/phoenix-client/bin

    python psql.py ZookeeperQuorum createCustomersTable.sql /tmp/customers.csv listCustomers.sql
    ```

    > [!NOTE]   
    > Belirlemek için `ZookeeperQuorum` adı, bulun [Apache ZooKeeper](https://zookeeper.apache.org/) çekirdek dizesini `/etc/hbase/conf/hbase-site.xml` özellik adı ile `hbase.zookeeper.quorum`.

5. Sonra `psql` işlemi tamamlandı, komut pencerenizde bir ileti görürsünüz:

    ```
    CSV Upsert complete. 5000 rows upserted
    Time: 4.548 sec(s)
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>Toplu yük tablolara MapReduce kullanma

Küme üzerinde dağıtılmış yüksek performanslı yükleme için MapReduce Yükleme aracını kullanın. Bu yükleyici, önce tüm verilerin HFiles dönüştürür ve ardından HBase için oluşturulan HFiles sağlar.

1. CSV MapReduce yükleyici başlatma kullanarak `hadoop` Phoenix istemci jar komutunu:

    ```bash
    hadoop jar phoenix-<version>-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table CUSTOMERS --input /data/customers.csv
    ```

2. Yeni bir tablo oluşturma bir SQL deyimi ile olduğu gibi `CreateCustomersTable.sql` önceki adım 1.

3. Tablonun şemasını doğrulamak için çalıştırın `!describe inputTable`.

4. Konumu yolu gibi örnek giriş verilerinize belirlemek `customers.csv` dosya. Giriş dosyaları WASB/ADLS depolama hesabınızdaki olabilir. Bu örnek senaryoda, giriş dosyaları bulunan `<storage account parent>/inputFolderBulkLoad` dizin.

5. MapReduce toplu yükleme komutunu yürütme dizinine geçin:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

6. Bulun, `ZookeeperQuorum` değerini `/etc/hbase/conf/hbase-site.xml`, özellik adı ile `hbase.zookeeper.quorum`.

7. Sınıf ve çalışma ayarlama `CsvBulkLoadTool` komut aracı:

    ```bash
    /usr/hdp/current/phoenix-client$ HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /inputFolderBulkLoad/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure
    ```

8. Azure Data Lake Storage ile MapReduce kullanmak için Data Lake Storage kök dizini bulun `hbase.rootdir` değerini `hbase-site.xml`. Aşağıdaki komutta, Data Lake Storage kök dizindir `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`. Bu komutta, Data Lake Storage giriş belirtin ve parametre olarak klasörleri çıktı:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

## <a name="recommendations"></a>Öneriler

* Aynı depolama ortamı, giriş ve çıkış klasörler için Azure Storage (WASB) veya Azure Data Lake Storage (ADL) kullanın. Data Lake depolamaya Azure Depolama'dan veri aktarmak için kullanabileceğiniz `distcp` komutu:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Daha büyük boyutlu çalışan düğümleri kullanın. Harita MapReduce toplu kopyalama işlemleri DFS olmayan kullanılabilir alanı dolduracak büyük miktarlarda geçici çıktı üretir. Toplu yükleme büyük miktarda için daha fazla ve daha büyük boyutlu çalışan düğümü kullanın. Doğrudan kümenize tahsis çalışan düğümlerinin sayısını işleme hızı etkilenir.

* Giriş dosyaları ~ 10 GB parçalara bölün. Toplu yükleme, bu nedenle daha iyi performans öbekleri sonuçlarında içine, giriş dosyalarınızın bölme depolama yoğunluklu işlemdir.

* Bölge sunucusu sıcak kaçının. Satır anahtarınızı tekdüze genişliyorsa, sıralı yazmalar HBase bölge sunucusu hotspotting anlamına. *Katarak* satır anahtarı, sıralı yazmalar azaltır. Phoenix, aşağıda belirtildiği gibi belirli bir tablonun satır anahtarını içeren bir güvenlik bayt şeffaf bir şekilde salt için bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu veri Apache Phoenix ile yükleniyor](https://phoenix.apache.org/bulk_dataload.html)
* [HDInsight kümelerinde Linux tabanlı Apache HBase ile Apache Phoenix kullanma](../hbase/apache-hbase-phoenix-squirrel-linux.md)
* [Salted tabloları](https://phoenix.apache.org/salted.html)
* [Apache Phoenix dilbilgisi](https://phoenix.apache.org/language/index.html)
