---
title: Hdınsight'ta - Azure Hdınsight Apache Phoenix | Microsoft Docs
description: ''
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 5d96b5656881815a82c89e0d159ba2bf556946b9
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="apache-phoenix-in-hdinsight"></a>HDInsight üzerinde Apache Phoenix

[Apache Phoenix](http://phoenix.apache.org/) yüksek düzeyde paralel ilişkisel veritabanı katmanı üzerinde oluşturulan bir açık kaynaklı [HBase](hbase/apache-hbase-overview.md). Phoenix, HBase üzerinde SQL benzeri sorguları kullanmanıza olanak sağlar. Phoenix, kullanıcıların oluştur, Sil, ayrı ayrı ve toplu SQL tablolar, dizinler, görünümler ve dizileri ve upsert satırları değiştirmek altındaki JDBC sürücüleri kullanır. Phoenix, HBase düşük gecikme süreli uygulamalarınızı oluşturulmasına etkinleştirme sorgular derlemek için MapReduce kullanmak yerine noSQL yerel derleme kullanır. Phoenix, verilerle birlikte bulunan kod yürütmek sunucu adres alanında istemci tarafından sağlanan kod çalıştırılmasını desteklemek üzere coprocessors ekler. Bu yaklaşım istemci/sunucu veri aktarımını en aza indirir.

Apache Phoenix, büyük veri sorguları olmayan-SQL benzeri bir sözdizimi kullanan geliştiriciler için açılan programlama yerine. Phoenix getirilmiş HBase için diğer araçları gibi [Hive](hadoop/hdinsight-use-hive.md) ve Spark SQL. Geliştiricilere avantajı yüksek oranda daha az kod ile kullanıcı sorguları yazma.
<!-- [Spark SQL](spark/apache-spark-sql-with-hdinsight.md)  -->

Bir SQL sorgusu gönderdiğiniz zaman Phoenix HBase yerel çağrıları sorguya derler ve en iyi duruma getirme paralel tarama (veya plan) çalıştırır. Bu soyutlama katmanını iş mantığı ve iş akışını kendi uygulamasına Phoenix'ın büyük veri depolama ve bunun yerine odaklanmak için MapReduce işleri yazmasını Geliştirici boşaltır.

## <a name="query-performance-optimization-and-other-features"></a>Sorgu performansı en iyi duruma getirme ve diğer özellikleri

Apache Phoenix, HBase sorgular için birkaç performans geliştirmeleri ve özellikler ekler.

### <a name="secondary-indexes"></a>İkincil dizinler

HBase birincil satır anahtarı lexicographically sıralanmış tek bir dizini içeriyor. Bu kayıtları yalnızca satır anahtarı erişilebilir. Satır anahtarını dışındaki herhangi bir sütun aracılığıyla kayıtları erişen tüm verileri gerekli filtre uygulanırken tarama gerektirir. İkincil bir dizinde sütunları veya dizinli biçimindedir deyimleri o dizini aramaları ve aralığı sağlayan bir alternatif satır anahtarını tarar.

İle ikincil bir dizin oluşturun `CREATE INDEX` komutu:

```sql
CREATE INDEX ix_purchasetype on SALTEDWEBLOGS (purchasetype, transactiondate) INCLUDE (bookname, quantity);
```

Bu yaklaşım, tek dizine sorguları yürütme üzerinden önemli ölçüde performans artışı yol açabilir. İkincil dizin bu tür bir **dizin kapsayan**, tüm sorguda dahil sütunları içeren. Bu nedenle, tablo arama gerekli değildir ve tüm sorgu dizini karşılar.

### <a name="views"></a>Görünümler

Phoenix görünümleri performans yaklaşık 100'den fazla fiziksel tablolar oluşturduğunuzda düşmeye başladığı bir HBase sınırlamanın üstesinden gelmek için bir yol sağlar. Phoenix görünümleri etkinleştirmek birden çok *sanal tablolar* bir temel alınan fiziksel HBase tablo paylaşmak için.

Phoenix görünümü oluşturma, standart SQL görünümü sözdizimi kullanmaya benzer. Temel tablodan devralınan sütunları yanı sıra, görünüm için sütunları tanımlayabilirsiniz bir farktır. Ayrıca yeni ekleyebilirsiniz `KeyValue` sütun.

Örneğin, adlandırılmış bir fiziksel tablo işte `product_metrics` aşağıdaki tanımıyla:

```sql
CREATE  TABLE product_metrics (
    metric_type CHAR(1),
    created_by VARCHAR, 
    created_date DATE, 
    metric_id INTEGER
    CONSTRAINT pk PRIMARY KEY (metric_type, created_by, created_date, metric_id));
```

Bu tablo ek sütunlar ile üzerinden bir görünüm tanımlayın:

```sql
CREATE VIEW mobile_product_metrics (carrier VARCHAR, dropped_calls BIGINT) AS
SELECT * FROM product_metrics
WHERE metric_type = 'm';
```

Daha sonra daha fazla sütun eklemek için kullanın `ALTER VIEW` deyimi.

### <a name="skip-scan"></a>Atla tarama

Atla tarama birleşik bir dizin, bir veya daha fazla sütun farklı değerleri bulmak için kullanır. Bir aralık tarama Atla tarama içi tarama, oluşturan satır uygulayan [geliştirilmiş performans](http://phoenix.apache.org/performance.html#Skip-Scan). Tarama sırasında sonraki değeri bulunana kadar ilk eşleşen değeri yanı sıra dizin atlanır.

Atla tarama kullanan `SEEK_NEXT_USING_HINT` HBase filtre numaralandırması. Kullanarak `SEEK_NEXT_USING_HINT`, Atla tarama anahtarları hangi kümesi veya anahtarları aralıklarına izler, için her bir sütunun Aranmakta olan. Atla tarama ardından ona filtre değerlendirme sırasında geçildi ve birleşimlerinden biri olup olmadığını belirler bir anahtarı alır. Aksi halde, sonraki en yüksek anahtarını atlamak için Atla tarama değerlendirir.

### <a name="transactions"></a>İşlemler

Phoenix HBase satır düzeyi işlemleri sağlarken tümleşir [Tephra](http://tephra.io/) tam desteğiyle çapraz satır ve çapraz tabloda işlem eklemek için [ACID](https://en.wikipedia.org/wiki/ACID) semantiği.

Olarak geleneksel SQL hareketlerle Phoenix işlem yöneticisi sağlanan işlemleri atomik bir veri birimi başarıyla upserted, üzerinde herhangi bir işlem etkin tabloyu upsert işlem başarısız olursa işlemi geri alınıyor olduğundan emin olmak sağlar.

Phoenix hareketleri etkinleştirmek için bkz: [Apache Phoenix işlem belgeleri](http://phoenix.apache.org/transactions.html).

Etkin işlemler ile yeni bir tablo oluşturmak için `TRANSACTIONAL` özelliğine `true` içinde bir `CREATE` deyimi:

```sql
CREATE TABLE my_table (k BIGINT PRIMARY KEY, v VARCHAR) TRANSACTIONAL=true;
```

İşlem için var olan bir tabloyu değiştirmek için aynı özelliğinde kullanmak bir `ALTER` deyimi:

```sql
ALTER TABLE my_other_table SET TRANSACTIONAL=true;
```

> [!NOTE]
> İşlem olmayan duruma geri işlem tablo geçemezsiniz.

### <a name="salted-tables"></a>Güvenlik tabloları

*Bölge sunucu hotspotting* kayıtları HBase için sıralı anahtarları ile yazarken oluşabilir. Kümedeki birden çok bölgede sunucuları olabilir ancak, yazma tüm yalnızca birinde yaşanan. Bu yoğunluğu nerede, tüm kullanılabilir bölge sunucuları arasında dağıtılmakta yazma yükünüzü yerine yalnızca bir yük işliyor hotspotting sorun oluşturur. Bir bölge bu boyut sınırına ulaştığında her bölge önceden tanımlanmış en büyük boyutu olduğundan, iki küçük bölgelere ayrılır. Bu durum oluştuğunda, bu yeni bölgelerinden yeni etkin nokta olmadan tüm yeni kayıtları alır.

Bu sorunu azaltmak ve daha iyi performans elde etmek için böylece tüm bölge sunucuları eşit olarak kullanılan tablo önceden bölün. Phoenix sağlar *güvenlik tabloları*, saydam belirli bir tablo satır anahtarını salting bayt ekleme. Tablo salt bayt sınırları eşit yük dağıtımı bölge sunucuları arasında ilk aşamasında tablosunun emin olmak için önceden Böl. Bu yaklaşım yazma iş yükü tüm yazma iyileştirme kullanılabilir bölge sunucular arasında dağıtır ve performans okuyun. Tablo salt için belirtmeniz `SALT_BUCKETS` tablo oluşturulduğunda tablo özelliği:

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

## <a name="enable-and-tune-phoenix-with-ambari"></a>Etkinleştirme ve Phoenix Ambari ile ayarlama

Bir Hdınsight HBase kümesi içeren [Ambari UI](hdinsight-hadoop-manage-ambari.md) yapılandırma değişiklikleri yapmak için.

1. Etkinleştirmek veya devre dışı Phoenix ve Phoenix'ın sorgu zaman aşımı ayarlarını denetlemek için oturum için Ambari Web kullanıcı arabirimini (`https://YOUR_CLUSTER_NAME.azurehdinsight.net`) Hadoop kullanıcı kimlik bilgilerinizi kullanarak.

2. Seçin **HBase** sol menüde hizmetler listesinden seçip **yapılandırmalar** sekmesi.

    ![Ambari HBase yapılandırma](./media/hdinsight-phoenix-in-hdinsight/ambari-hbase-config.png)

3. Bul **Phoenix SQL** etkinleştirmek veya phoenix devre dışı bırakın ve sorgu zaman aşımı ayarlamak için yapılandırma bölümü.

    ![Ambari Phoenix SQL yapılandırma bölümü](./media/hdinsight-phoenix-in-hdinsight/ambari-phoenix.png)

## <a name="see-also"></a>Ayrıca bkz.

* [Hdınsight'ta Linux tabanlı HBase kümeleriyle Apache Phoenix kullanın](hbase/apache-hbase-phoenix-squirrel-linux.md)
