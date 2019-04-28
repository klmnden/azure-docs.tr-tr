---
title: HDInsight - Azure HDInsight üzerinde Apache Phoenix
description: ''
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 7d9aafeb920eab7f6a87061a135bf2e464add436
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763647"
---
# <a name="apache-phoenix-in-hdinsight"></a>HDInsight üzerinde Apache Phoenix

[Apache Phoenix](https://phoenix.apache.org/) yüksek düzeyde paralel ilişkisel veritabanı katmanı üzerinde oluşturulan bir açık kaynak [Apache HBase](hbase/apache-hbase-overview.md). Phoenix, HBase üzerinde SQL benzeri sorguları kullanmanıza olanak sağlar. Phoenix, oluştur, Sil, SQL tabloları, dizinleri, görünümleri ve dizileri ve upsert satırları ayrı ayrı ve toplu değiştirme olanağı altında JDBC sürücüleri kullanır. Phoenix, HBase üzerinde düşük gecikme süreli uygulamaları oluşturmayı etkinleştirme sorgular derlemek için MapReduce kullanmak yerine noSQL yerel derleme kullanır. Phoenix, verilerle birlikte kod yürüten sunucu adres alanında istemci tarafından sağlanan kod çalıştırılmasını desteklemek üzere coprocessors ekler. Bu yaklaşım istemci/sunucu veri aktarımını en aza indirir.

Apache Phoenix, olmayan-SQL benzeri bir sözdizimi kullanan geliştiricilerin büyük veri sorgularını açılır programlama yerine. Phoenix yüksek oranda optimize HBase için diğer araçları gibi [Apache Hive](hadoop/hdinsight-use-hive.md) ve Apache Spark SQL. Geliştiriciler için en büyük avantajı yüksek performansa sahip sorgular çok daha az kod yazıyor.
<!-- [Spark SQL](spark/apache-spark-sql-with-hdinsight.md)  -->

Bir SQL sorgusu gönderdiğiniz zaman, Phoenix sorgu HBase yerel çağrıları derler ve iyileştirme paralel tarama (veya planı) çalıştırır. Bu Soyutlama Katmanı iş mantığını ve iş akışını kendi uygulamasına Phoenix'ın büyük veri depolama ve bunun yerine odaklanmak için MapReduce işleri yazmasını Geliştirici serbest bırakır.

## <a name="query-performance-optimization-and-other-features"></a>Sorgu performans iyileştirme ve diğer özellikler

Apache Phoenix, HBase sorguları çeşitli performans iyileştirmeleri ve özellikler ekler.

### <a name="secondary-indexes"></a>İkincil dizinler

HBase birincil satır anahtarı lexicographically sıralanmış tek bir dizinde sahip. Bu kayıtlar, yalnızca satır anahtarı erişilebilir. Kayıtları satır anahtarı dışında herhangi bir sütun aracılığıyla erişen tüm verilerin gerekli filtre uygulanırken tarama gerektirir. İkincil bir dizinde sütunları veya dizini oluşturulmuş bir form ifadeler bu dizini aramaları ve aralığı izin vererek bir diğer satır anahtarı tarar.

İle ikincil bir dizin oluşturma `CREATE INDEX` komutu:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Bu yaklaşım, tek dizinli sorgular yürütme üzerinden önemli ölçüde performans artışı sağlayabilir. İkincil dizin bu tür bir **dizin kapsayan**, sorguda bulunan sütunların tümünü içeren. Bu nedenle, tablo arama gerekli değildir ve tüm sorgu dizini karşılar.

### <a name="views"></a>Görünümler

Phoenix görünümleri performans yaklaşık 100'den fazla fiziksel tablo oluşturduğunuzda düşmeye başladığı bir HBase sınırlamanın üstesinden gelmek için bir yol sağlar. Phoenix görünümlerini etkinleştirmek birden çok *sanal tablolar* temel alınan fiziksel HBase tablo paylaşmak için.

Phoenix görünümü oluşturma, standart SQL görünümü sözdizimini kullanarak benzer. Devralınan kendi temel tablodan sütun yanı sıra görünümünüz için sütunları tanımlayabilirsiniz bir farktır. Ayrıca yeni ekleyebilirsiniz `KeyValue` sütunları.

Örneğin, adında bir fiziksel tablo işte `product_metrics` aşağıdaki tanımıyla:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR, 
    created_date DATE, 
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Ek sütunlar içeren bu tablo üzerine bir görünümü tanımlayın:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

Daha fazla sütun daha eklemek için `ALTER VIEW` deyimi.

### <a name="skip-scan"></a>Skip tarama

Skip tarama bir bileşik dizin, bir veya daha fazla sütun benzersiz değerleri bulmak için kullanır. Aralık tarama, tarama, sonuçlanmıyor satır içi Atla tarama uygulayan [geliştirilmiş performans](https://phoenix.apache.org/performance.html#Skip-Scan). Tarama sırasında sonraki değer bulunana kadar ilk eşleşen değerin yanı sıra dizin atlanır.

Skip tarama kullanan `SEEK_NEXT_USING_HINT` HBase filtre numaralandırması. Kullanarak `SEEK_NEXT_USING_HINT`, Atla tarama hangi anahtar kümesini veya anahtarlarının aralıkları izler, için her bir sütunun Aranmakta olan. Skip tarama ardından filtre değerlendirmesi sırasında geçilen ve bileşimlerinden olup olmadığını belirler bir anahtarı alır. Aksi halde, atlamak için bir sonraki en yüksek anahtar Atla tarama değerlendirir.

### <a name="transactions"></a>İşlemler

HBase satır düzeyinde işlemler sağlasa da, Phoenix ile tümleşir [Tephra](https://tephra.io/) tam satır içi ve geçici tablo işlem desteği eklemek için [ACID](https://en.wikipedia.org/wiki/ACID) semantiği.

Olarak ile geleneksel SQL işlemleri, Phoenix işlem yöneticisi sağlanan işlemleri başarıyla atomik bir veri birimi üzerinde herhangi bir işlem etkin tabloda upsert işlem başarısız olursa işlemi geri alınıyor upserted, olduğundan emin olmak sağlar.

Phoenix hareketleri etkinleştirmek için bkz: [Apache Phoenix işlem belgeleri](https://phoenix.apache.org/transactions.html).

Etkin işlemleri yeni bir tablo oluşturmak için `TRANSACTIONAL` özelliğini `true` içinde bir `CREATE` deyimi:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

İşlem var olan bir tabloyu değiştirmek için aynı özelliği kullanan bir `ALTER` deyimi:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]
> İşlem olmayan olan geri işlemsel bir tabloda geçiş yapamazsınız.

### <a name="salted-tables"></a>Salted tabloları

*Bölge sunucusu hotspotting* kayıtları için HBase ile sıralı anahtarları yazılırken ortaya çıkabilir. Kümenizde birden çok bölge sunucuları olsanız, okumalarınızın tüm yalnızca birinde oluşan. Bu yoğunluk, tüm kullanılabilir bölge sunucuları arasında dağıtılmasını yazma iş yükünüzü yerine, yalnızca bir yük işlediği hotspotting sorun oluşturur. Her bölge, bölge söz konusu boyut sınırına ulaştığında önceden tanımlanmış bir maksimum boyut olduğundan, iki küçük bölgelere ayrılmış durumdadır. Bu durum oluştuğunda, bu yeni bölgeler biri yeni etkin olma, tüm yeni kayıtlar alır.

Bu sorunu çözmek ve daha iyi performans elde etmek için böylece tüm bölge sunucuları eşit olarak kullanılan tablolar önceden bölün. Phoenix sağlar *salted tabloları*, saydam bir şekilde belirli bir tablonun satır anahtarı güvenlik bayt ekleme. Tablo salt bayt sınırlarına eşit yük dağıtımı bölge sunucuları arasında ilk aşaması sırasında tablonun emin olmak için önceden bölme. Bu yaklaşım, tüm yazma iyileştirme kullanılabilir bölge sunucuları arasında yazma iş yükü dağıtır ve performans okuyun. Tablo salt için belirtin `SALT_BUCKETS` tablo oluşturulduğunda özellik tablosu:

```sql
CREATE TABLE Saltedweblogs (
    transactionid varchar(500) Primary Key,
    transactiondate Date NULL,
    customerid varchar(50) NULL,
    bookid varchar(50) NULL,
    purchasetype varchar(50) NULL,
    orderid varchar(50) NULL,
    bookname varchar(50) NULL,
    categoryname varchar(50) NULL,
    invoicenumber varchar(50) NULL,
    invoicestatus varchar(50) NULL,
    city varchar(50) NULL,
    state varchar(50) NULL,
    paymentamount DOUBLE NULL,
    quantity INTEGER NULL,
    shippingamount DOUBLE NULL) SALT_BUCKETS=4;
```

## <a name="enable-and-tune-phoenix-with-apache-ambari"></a>Etkinleştirme ve Phoenix Apache Ambari ile ayarlama

HDInsight HBase kümesi içeren [Ambari UI](hdinsight-hadoop-manage-ambari.md) yapılandırma değişiklikleri yapmak için.

1. Etkinleştirme veya devre dışı Phoenix ve Phoenix'ın sorgu zaman aşımı ayarlarını denetlemek için oturum açmak için Ambari Web kullanıcı arabirimini (`https://YOUR_CLUSTER_NAME.azurehdinsight.net`) Hadoop kullanıcı kimlik bilgilerinizi kullanarak.

2. Seçin **HBase** sol menüdeki hizmetler listesinden seçip **yapılandırmaları** sekmesi.

    ![Ambari HBase yapılandırma](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config.png)

3. Bulma **Phoenix SQL** etkinleştirmek veya devre dışı phoenix ve sorgu zaman aşımı ayarlamak için yapılandırma bölümü.

    ![Ambari Phoenix SQL yapılandırma bölümü](./media/hdinsight-phoenix-in-hdinsight/ambari-phoenix.png)

## <a name="see-also"></a>Ayrıca bkz.

* [HDInsight Linux tabanlı HBase kümeleriyle Apache Phoenix kullanma](hbase/apache-hbase-phoenix-squirrel-linux.md)
