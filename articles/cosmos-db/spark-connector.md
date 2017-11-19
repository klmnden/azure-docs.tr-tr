---
title: "Apache Spark Azure Cosmos DB bağlanma | Microsoft Docs"
description: "Apache Spark dağıtılmış toplanmalar ve veri sciences üzerinde çoklu kiracı genel gerçekleştirmek için Azure Cosmos DB'de dağıtılmış veritabanı sistem Microsoft'tan bağlanmanıza olanak sağlayan Azure Cosmos DB Spark Bağlayıcısı hakkında bilgi edinmek için bu öğreticiyi kullanın Bu Bulut için tasarlanmıştır."
keywords: Apache spark
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: c4f46007-2606-4273-ab16-29d0e15c0736
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: denlee
ms.openlocfilehash: ba824ed1bad49c71f8de9f2da8249945d9430222
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="accelerate-real-time-big-data-analytics-with-the-spark-to-azure-cosmos-db-connector"></a>Spark ile gerçek zamanlı büyük veri analizi Azure Cosmos DB bağlayıcıya hızlandırmak

Spark Azure Cosmos DB bağlayıcı için bir giriş kaynağı ya da Apache Spark işleri için çıkış havuzu olarak hareket edecek Azure Cosmos DB sağlar. Bağlanma [Spark](http://spark.apache.org/) için [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hızlı bir şekilde kalıcı hale getirmek ve verileri sorgulamak için Azure Cosmos DB burada kullanabileceğiniz hızlı taşıma veri bilimi sorunları yeteneğinizi hızlandırır. Spark Azure Cosmos DB bağlayıcı için verimli bir şekilde yerel Azure Cosmos yönetilen DB dizinleri kullanır. Dizinler güncelleştirilebilir sütunları analiz gerçekleştirmek ve nesnelerin interneti (IOT) arasında veri veri bilimi ve analiz senaryoları için Dağıtılmış hızlı genel değiştirme karşı itme aşağı koşul filtreleme sağlar.

Spark GraphX ve Gremlin grafik Azure Cosmos DB API'leri ile çalışmak için bkz: [Spark ve Apache TinkerPop Gremlin kullanarak grafik analytics gerçekleştirmek](spark-connector-graph.md).

## <a name="download"></a>İndir

Başlamak için Azure Cosmos DB bağlayıcısından Spark karşıdan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/) github'daki.

## <a name="connector-components"></a>Bağlayıcı bileşenleri

Bağlayıcı aşağıdaki bileşenleri kullanır:

* [Azure Cosmos DB](http://documentdb.com) sağlamak ve üretilen iş ve depolama coğrafi bölgeler arasında herhangi bir sayı özellikler esnek ölçeklendirme sağlar. Hizmeti sunar:
   * Anahtar kapatma [genel dağıtım](distribute-data-globally.md) ve yatay ölçek
   * Tek basamaklı gecikmeleri 99 adresindeki garanti
   * [Birden çok iyi tanımlanmış tutarlılık modelleri](consistency-levels.md)
   * Çok girişli özellikleriyle yüksek oranda kullanılabilirlik garanti
   * Tüm özellikleri endüstri lideri tarafından kapsamlı yedeklenen [hizmet düzeyi sözleşmelerine](https://azure.microsoft.com/support/legal/sla/cosmos-db) (SLA).

* [Apache Spark](http://spark.apache.org/) hızı, kullanımı kolay, gelişmiş analizler yerleşik bir güçlü açık kaynaklı işleme altyapısıdır.

* [Azure hdınsight'ta Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) , Apache Spark kritik dağıtımlar için bulutta kullanarak dağıtabileceğiniz [Azure Hdınsight](https://azure.microsoft.com/services/hdinsight/apache-spark/).

Resmi olarak desteklenen sürümler:

| Bileşen | Sürüm |
|---------|-------|
|Apache Spark|2.0+|
| Scala| 2.11|
| Azure DocumentDB Java SDK'sı | 1.10.0 |

Bu makalede, Python (aracılığıyla pyDocumentDB) ve Scala arabirimleri kullanarak bazı basit örneklerini çalıştırma yardımcı olur.

Apache Spark ve Azure Cosmos DB bağlanmak için iki yaklaşım vardır:
- PyDocumentDB aracılığıyla kullanmak [Azure DocumentDB Python SDK'sı](https://github.com/Azure/azure-documentdb-python).
- Java tabanlı bir Spark Azure Cosmos DB bağlayıcıya yararlanarak oluşturma [Azure DocumentDB Java SDK'sı](https://github.com/Azure/azure-documentdb-java).

## <a name="pydocumentdb-implementation"></a>pyDocumentDB uygulama
Geçerli [pyDocumentDB SDK](https://github.com/Azure/azure-documentdb-python) Spark aşağıdaki çizimde gösterildiği gibi Azure Cosmos DB bağlanmanıza olanak sağlar:

![PyDocumentDB DB aracılığıyla Azure Cosmos DB veri akışı için spark](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow-of-the-pydocumentdb-implementation"></a>Veri akışı pyDocumentDB uygulama

Veri akışı aşağıdaki gibidir:

1. Spark ana düğüm pyDocumentDB aracılığıyla Azure Cosmos DB ağ geçidi düğümü bağlanır. Bir kullanıcı yalnızca Spark ve Azure Cosmos DB bağlantıları belirtir. İlgili ana ve ağ geçidi düğümleri bağlantıları kullanıcı için saydamdır.
2. Ağ geçidi düğümü Azure Cosmos sorgu daha sonra veri düğümlerini koleksiyonunun bölümlerinde karşı çalıştığı DB sorgusu yapar. Bu sorgular için yanıt ağ geçidi düğüme geri gönderilir ve Spark ana düğüme bu sonuç kümesi döndürdü.
3. Sonraki sorguları (örneğin, bir Spark DataFrame) Spark çalışan düğümlerine işleme için gönderilir.

Spark Azure Cosmos DB arasındaki iletişimi, Spark ana düğüm ve Azure Cosmos DB ağ geçidi düğümleri ile sınırlıdır.  Bu iki düğüm arasında Aktarım katmanı sağlayan kadar hızlı sorguları gidin.

### <a name="install-pydocumentdb"></a>PyDocumentDB yükleyin
Kullanarak, sürücü düğümde pyDocumentDB yükleyebilirsiniz **PIP**, örneğin:

```
pip install pyDocumentDB
```


### <a name="connect-spark-to-azure-cosmos-db-via-pydocumentdb"></a>Spark, pyDocumentDB Azure Cosmos Veritabanına bağlanın
Basitlik iletişim taşıma bir sorgu yürütme Spark'tan Azure Cosmos DB pyDocumentDB görece basit kullanarak yapar.

Aşağıdaki kod parçacığını bir Spark bağlamında pyDocumentDB kullanmayı gösterir.

```
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]


# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)
```

Kod parçacığında belirtildiği gibi:

* Azure Cosmos DB Python SDK'sı (`pyDocumentDB`) tüm gerekli bağlantı parametrelerini içerir. Örneğin, tercih edilen konumları parametresi okuma çoğaltma ve öncelik sırasını seçer.
*  Gerekli kitaplıkları içeri aktarma ve yapılandırma, **masterKey** ve **konak** Azure Cosmos DB oluşturmak için *istemci* (**pydocumentdb.document_client** ).


### <a name="execute-spark-queries-via-pydocumentdb"></a>PyDocumentDB Spark sorgularını Yürüt
Aşağıdaki örnekler önceki kod parçacığında belirtilen salt okunur anahtarları kullanılarak oluşturulmuş Azure Cosmos DB örneği kullanın. Aşağıdaki kod parçacığını bağlandığı **airports.codes** DoctorWho hesabıyla koleksiyonunda daha önce belirtilen ve Washington durumu havaalanı şehirlerde ayıklamak için bir sorgu çalıştırır.

```
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Push into list `elements`
elements = list(query)
```

Aracılığıyla sorgu yürütüldükten sonra **sorgu**, sonuç bir **query_iterable. QueryIterable** Python listesini dönüştürülür. Python listesini, aşağıdaki kodu kullanarak bir Spark DataFrame kolayca dönüştürülebilir:

```
# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(elements)
```

### <a name="why-use-the-pydocumentdb-to-connect-spark-to-azure-cosmos-db"></a>Neden pyDocumentDB Spark Azure Cosmos Veritabanına bağlanmak için kullanılır?
PyDocumentDB kullanarak Spark Azure Cosmos Veritabanına bağlanma genellikle senaryoları için burada:

* Python kullanmak istediğiniz.
* Görece küçük sonuç Azure Cosmos DB'den için Spark kümesinde iade ettiğiniz. Temel alınan veri kümesi Azure Cosmos veritabanı oldukça büyük olabileceğini unutmayın. Koşul filtreleri Azure Cosmos DB kaynağınız karşı çalışan filtreleri, diğer bir deyişle, uyguladığınızı.  

## <a name="spark-to-azure-cosmos-db-connector"></a>Azure Cosmos DB bağlayıcıya spark

Spark Azure Cosmos DB bağlayıcısını kullanan [Azure DocumentDB Java SDK'sı](https://github.com/Azure/azure-documentdb-java) ve aşağıdaki çizimde gösterildiği gibi verileri Azure Cosmos DB ve Spark çalışan düğümleri arasında taşır:

![Azure Cosmos DB bağlayıcı için Spark veri akışı](./media/spark-connector/spark-connector.png)

Veri akışı aşağıdaki gibidir:

1. Spark ana düğüm bölüm Haritası almak için Azure Cosmos DB ağ geçidi düğümü bağlar. Bir kullanıcı yalnızca Spark ve Azure Cosmos DB bağlantıları belirtir. İlgili ana ve ağ geçidi düğümleri bağlantıları kullanıcı için saydamdır.
2. Bu bilgiler, Spark ana düğüme geri sağlanır.  Bu noktada, bölümler ve konumlarına erişmek için gereken Azure Cosmos veritabanı belirleme sorgusu ayrıştırabiliyor olması gerekir.
3. Bu bilgiler, Spark çalışan düğümlerine iletilir.
4. Spark çalışan düğümleri doğrudan veri ayıklamak ve veri Spark çalışan düğümleri Spark bölümlerinde dönmek için Azure Cosmos DB bölümleri bağlayın.

Veri taşıma Spark çalışan düğümleri ve Azure Cosmos DB veri düğümleri (bölümler) olduğundan Spark Azure Cosmos DB arasındaki iletişimi önemli ölçüde daha hızlıdır.

### <a name="build-the-spark-to-azure-cosmos-db-connector"></a>Spark Azure Cosmos DB bağlayıcısı oluşturun
Şu anda bağlayıcı proje maven kullanır. Bağımlılıklar olmadan bağlayıcısı oluşturmak için çalıştırabilirsiniz:
```
mvn clean package
```
Ayrıca JAR en son sürümlerini indirebilirsiniz *serbest* klasör.

### <a name="include-the-azure-cosmos-db-spark-jar"></a>Azure Cosmos DB Spark JAR Ekle
Herhangi bir kod çalıştırmadan önce Azure Cosmos DB Spark JAR eklemeniz gerekir.  Kullanıyorsanız **spark Kabuk**, kullanarak JAR içerebilir sonra **--Kavanoz** seçeneği.  

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3-jar-with-dependencies.jar
```

Bağımlılıklar olmadan JAR çalıştırmak istiyorsanız, aşağıdaki kodu kullanın:

```
spark-shell --master $master --jars /$location/azure-cosmosdb-spark-0.0.3.jar,/$location/azure-documentdb-1.10.0.jar
```

Azure Hdınsight Jupyter Not Defteri hizmeti gibi bir Not Defteri hizmeti kullanıyorsanız, kullanabileceğiniz **spark Sihirli** komutlar:

```
%%configure
{ "jars": ["wasb:///example/jars/azure-documentdb-1.10.0.jar","wasb:///example/jars/azure-cosmosdb-spark-0.0.3.jar"],
  "conf": {
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
}
```

**Kavanoz** komutu için gerekli olan iki Kavanoz eklemenizi sağlar **azure cosmosdb spark** (kendisi ve da Azure DocumentDB Java SDK'sı) ve çıkarma **scala-yansıtacak** Böylece ile etkilemediğinden Livy çağırır (Jupyter not defteri > Livy > Spark).

### <a name="connect-spark-to-azure-cosmos-db-using-the-connector"></a>Spark Azure Cosmos Bağlayıcısı'nı kullanarak Veritabanına bağlanın
İletişim taşıma biraz daha karmaşık olsa da, Azure Cosmos DB Spark'tan Bağlayıcısı'nı kullanarak bir sorgu yürütme önemli ölçüde daha hızlıdır.

Aşağıdaki kod parçacığını bir Spark bağlamında Bağlayıcısı'nı kullanmayı gösterir.

```
// Import Necessary Libraries
import org.joda.time._
import org.joda.time.format._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Kod parçacığında belirtildiği gibi:

- **Azure cosmosdb spark** tercih edilen konumlarını dahil tüm gerekli bağlantı parametrelerini içerir. Örneğin, okuma çoğaltma ve öncelik sırasını seçebilirsiniz.
- Yalnızca gerekli kitaplıkları içeri aktarma ve masterKey ve Azure Cosmos DB istemci oluşturmak için ana bilgisayar yapılandırın.

### <a name="execute-spark-queries-via-the-connector"></a>Bağlayıcısı aracılığıyla Spark sorgularını Yürüt

Aşağıdaki örnek önceki kod parçacığında belirtilen salt okunur anahtarları kullanılarak oluşturulmuş Azure Cosmos DB örneği kullanır. Aşağıdaki kod parçacığını DepartureDelays.flights_pcoll koleksiyonda (DoctorWho hesabı daha önce belirtildiği gibi) bağlanır ve İstanbul departing uçuşlar uçuş gecikme bilgileri ayıklamak için bir sorgu çalıştırır.

```
// Queries
var query = "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'"
val df = spark.sql(query)

// Run DF query (count)
df.count()

// Run DF query (show)
df.show()
```

### <a name="why-use-the-spark-to-azure-cosmos-db-connector-implementation"></a>Spark Azure Cosmos DB bağlayıcı uygulamaya neden kullanılır?

Bağlayıcısı'nı kullanarak Spark Azure Cosmos Veritabanına bağlanma genellikle senaryoları için burada:

* Scala kullanın ve de belirtildiği gibi bir Python sarmalayıcı içerecek şekilde güncelleştirmek istediğiniz [sorunu 3: ekleme Python sarmalayıcı ve örnekler](https://github.com/Azure/azure-cosmosdb-spark/issues/3).
* Büyük miktarda veri Apache Spark ve Azure Cosmos DB arasında aktarmak var.

Sorgu performans farkı hakkında bir fikir vermek için bkz: [sorgu Test çalışmalarını wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="distributed-aggregation-example"></a>Dağıtılmış toplama örneği
Bu bölümde bazı örnekleri nasıl dağıtılmış toplamalar ve analizi birlikte Apache Spark ve Azure Cosmos DB kullanarak bunu yapabilirsiniz. Destekleyen zaten toplamalar, Azure Cosmos DB içinde ele [Planet ölçek toplamalar Azure Cosmos DB blog ile](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/). İşte Apache Spark sonraki düzeye nasıl alabilir.

Reference için bu toplamaları olduğunu unutmayın [Spark Azure Cosmos DB Bağlayıcısı not defteri için](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark-to-CosmosDB_Connector.ipynb).

### <a name="connect-to-flights-sample-data"></a>Uçuşlar örnek verilere bağlanma
Bu toplama örnekler depolanan bazı uçuş performans veri erişim bizim **DoctorWho** Azure Cosmos DB veritabanı. Bağlanmak için aşağıdaki kod parçacığını kullanan gerekir:

```
// Import Spark to Azure Cosmos DB connector
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Connect to Azure Cosmos DB Database
val readConfig2 = Config(Map("Endpoint" -> "https://doctorwho.documents.azure.com:443/",
"Masterkey" -> "le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA==",
"Database" -> "DepartureDelays",
"preferredRegions" -> "Central US;East US 2;",
"Collection" -> "flights_pcoll",
"SamplingRatio" -> "1.0"))

// Create collection connection
val coll = spark.sqlContext.read.cosmosDB(readConfig2)
coll.createOrReplaceTempView("c")
```

Bu parçacığıyla biz de filtrelenmiş veri kümesi, burada bu ikinci dağıtılmış toplamalar gerçekleştirebilirsiniz için Spark Azure Cosmos DB'den aktarır temel bir sorgu kalmayacaktır. Bu durumda, sizden Seattle (SEA) gelen çok uçuşlar için istiyoruz.

```
// Run, get row count, and time query
val originSEA = spark.sql("SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'")
originSEA.createOrReplaceTempView("originSEA")
```

Aşağıdaki sonuçları Jupyter not defteri hizmetinden sorguları çalıştırarak üretilmiştir.  Kod parçacıkları genel ve herhangi bir hizmete özgü olmayan olduğunu unutmayın.

### <a name="running-limit-and-count-queries"></a>Çalışan sınırı ve sayısı sorguları
Tıpkı için kullandığınız SQL/Spark SQL, ile başlayalım bir **sınırı** sorgu:

![Spark sınırı sorgu](./media/spark-connector/spark-sql-query.png)

Sonraki basit ve hızlı sorgudur **sayısı** sorgu:

![Spark sayım sorgusu](./media/spark-connector/spark-count-query.png)

### <a name="group-by-query"></a>GROUP BY sorgusu
Bu sonraki kümesinde biz kolayca çalıştırabilirsiniz **GROUP BY** bizim Azure Cosmos DB veritabanı sorguları:

```
select destination, sum(delay) as TotalDelays
from originSEA
group by destination
order by sum(delay) desc limit 10
```

![Spark GROUP BY sorgu grafiği](./media/spark-connector/group-by-query-graph.png)

### <a name="distinct-order-by-query"></a>ORDER BY ayrı sorgu
Burada da bir **DISTINCT, ORDER BY** sorgu:

![Spark GROUP BY sorgu grafiği](./media/spark-connector/order-by-query.png)

### <a name="continue-the-flight-data-analysis"></a>Uçuş veri analizi devam
Aşağıdaki örnek sorgularda, uçuş veri analizi devam etmek için kullanabilirsiniz:

#### <a name="top-5-delayed-destinations-cities-departing-from-seattle"></a>İstanbul departing üst 5 Gecikmeli hedefleri (şehir)
```
select destination, sum(delay)
from originSEA
where delay < 0
group by destination
order by sum(delay) limit 5
```
![Spark üst gecikmeler grafiği](./media/spark-connector/top-delays-graph.png)


#### <a name="calculate-median-delays-by-destination-cities-departing-from-seattle"></a>Hedef Şehir İstanbul departing Medyan gecikmelerinden Hesapla
```
select destination, percentile_approx(delay, 0.5) as median_delay
from originSEA
where delay < 0
group by destination
order by percentile_approx(delay, 0.5)
```

![Spark Medyan gecikmeler grafiği](./media/spark-connector/median-delays-graph.png)

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, Spark Azure Cosmos DB bağlayıcısından karşıdan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposunu ve depodaki ek kaynakları araştırın:

* [Dağıtılmış toplamalar örnekleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Örnek komut dosyaları ve dizüstü bilgisayarlar](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

Gözden geçirmek isteyebilirsiniz [Apache Spark SQL, DataFrames ve veri kümeleri Kılavuzu](http://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure hdınsight'ta Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) makalesi.
