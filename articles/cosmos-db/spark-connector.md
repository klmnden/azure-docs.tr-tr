---
title: Apache Spark, Azure Cosmos DB'ye bağlanma
description: Apache Spark, Azure Cosmos DB'ye bağlama olanak tanıyan Azure Cosmos DB Spark Bağlayıcısı hakkında bilgi edinin.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: ramkris
ms.openlocfilehash: bc0f2044f70c674177f9c9786f56f0441db2e282
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65978899"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Azure Cosmos DB Bağlayıcısı için Apache Spark'ı kullanarak büyük veri analizi hızlandırın

Çalıştırabileceğiniz [Spark](https://spark.apache.org/) Cosmos DB Spark Bağlayıcısı'nı kullanarak Azure Cosmos DB içinde depolanan verilerle işler. Cosmos düşük gecikme süreli erişim için batch ve akış işleme için ve bir hizmet katmanı olarak kullanılabilir.

Bağlayıcı ile kullanabileceğiniz [Azure Databricks](https://azure.microsoft.com/services/databricks) veya [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/), Azure üzerinde yönetilen Spark kümeleri sağlayın. Aşağıdaki tabloda desteklenen Spark sürümler gösterilmektedir.

| Bileşen | Sürüm |
|---------|-------|
| Apache Spark | 2.4.x, 2.3.x 2.2.x ve 2.1.x |
| Scala | 2.11 |
| Azure Databricks çalışma zamanı sürümü | > 3.4 |

> [!WARNING]
> Bu bağlayıcı, Azure Cosmos DB, çekirdek (SQL) API destekler.
> Cosmos DB için MongoDB API'si için kullanmak [MongoDB Spark Bağlayıcısı](https://docs.mongodb.com/spark-connector/master/).
> Cosmos DB Cassandra API'si için kullanmak [Cassandra Spark Bağlayıcısı](https://github.com/datastax/spark-cassandra-connector).
>

## <a name="quickstart"></a>Hızlı Başlangıç

* Bölümündeki adımları [Java SDK'sı ile çalışmaya başlama](sql-api-async-java-get-started.md) bir Cosmos DB hesabı ayarlamanız ve bazı verileri doldurmasına.
* Bölümündeki adımları [Azure Databricks kullanmaya başlama](https://docs.azuredatabricks.net/getting-started/index.html) bir Azure Databricks çalışma alanı ve küme ayarlamak için.
* Şimdi yeni bir not defterleri oluşturma ve Cosmos DB Bağlayıcısı kitaplığı içeri aktarabilirsiniz. Atla [Cosmos DB Bağlayıcısı ile çalışma](#bk_working_with_connector) çalışma alanınızı ayarlama konusunda ayrıntılı bilgi için.
* Aşağıdaki bölümde, okuma ve yazma bağlayıcı kullanarak nasıl parçacıkları sahiptir.

### <a name="reading-from-cosmos-db"></a>Cosmos DB veri okuma

Aşağıdaki kod parçacığı, PySpark Cosmos DB'de okunacak bir Spark DataFrame oluşturma işlemi gösterilmektedir.

```python
# Read Configuration
readConfig = {
  "Endpoint" : "https://doctorwho.documents.azure.com:443/",
  "Masterkey" : "YOUR-KEY-HERE",
  "Database" : "DepartureDelays",
  "Collection" : "flights_pcoll",
  "query_custom" : "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
}

# Connect via azure-cosmosdb-spark to create Spark DataFrame
flights = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**readConfig).load()
flights.count()
```

Ve Scala aynı kod parçacığında:

```scala
// Import Necessary Libraries
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Configure connection to your collection
val readConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_pcoll",
  "query_custom" -> "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
))

// Connect via azure-cosmosdb-spark to create Spark DataFrame
val flights = spark.read.cosmosDB(readConfig)
flights.count()
```

### <a name="writing-to-cosmos-db"></a>Cosmos DB'ye yazma

Aşağıdaki kod parçacığında, bir veri çerçevesi PySpark Cosmos DB için yazma işlemi gösterilmektedir.

```python
# Write configuration
writeConfig = {
 "Endpoint" : "https://doctorwho.documents.azure.com:443/",
 "Masterkey" : "YOUR-KEY-HERE",
 "Database" : "DepartureDelays",
 "Collection" : "flights_fromsea",
 "Upsert" : "true"
}

# Write to Cosmos DB from the flights DataFrame
flights.write.format("com.microsoft.azure.cosmosdb.spark").options(**writeConfig).save()
```

Ve Scala aynı kod parçacığında:

```scala
// Configure connection to the sink collection
val writeConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_fromsea",
  "Upsert" : "true"
))

// Upsert the dataframe to Cosmos DB
import org.apache.spark.sql.SaveMode
flights.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Daha fazla kod parçacıkları ve uçtan uca örnekler görmek [Jupyter](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/notebooks).

## <a name="bk_working_with_connector"></a> Bağlayıcı ile çalışma

Github'da kaynağı bağlayıcısından derleme veya uber jar dosyaları dışındaki Maven'dan aşağıdaki bağlantılarda indirin.

| Spark | Scala | En son sürümü |
|---|---|---|
| 2.4.0 | 2.11 | [azure-cosmosdb-spark_2.4.0_2.11_1.3.5](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar)
| 2.3.0 | 2.11 | [azure-cosmosdb-spark_2.3.0_2.11_1.3.3](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.3.0_2.11/1.3.3/jar)
| 2.2.0 | 2.11 | [azure-cosmosdb-spark_2.2.0_2.11_1.1.1](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-cosmosdb-spark_2.2.0_2.11%7C1.1.1%7Cjar)
| 2.1.0 | 2.11 | [azure-cosmosdb-spark_2.1.0_2.11_1.2.2](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.2/jar)

### <a name="using-databricks-notebooks"></a>Databricks not defterlerini kullanma

Azure Databricks Kılavuzu'ndaki yönergeleri takip ederek, Databricks çalışma alanı kullanarak bir kitaplığı oluşturma > [Azure Cosmos DB Spark Bağlayıcısı kullanma](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

> [!NOTE]
> Not **Azure Cosmos DB Spark Bağlayıcısı kullanma** sayfa şu anda güncel değil. Altı ayrı jar dosyaları dışındaki altı farklı kitaplıklara indirmek yerine, maven uber jar indirebilirsiniz https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.5/jar) ve bu bir jar/kitaplığını yükleyin.
> 

### <a name="using-spark-cli"></a>Spark-cli kullanarak

Spark-cli kullanarak Bağlayıcısı ile çalışma (diğer bir deyişle, `spark-shell`, `pyspark`, `spark-submit`), kullanabileceğiniz `--packages` bağlayıcının parametresiyle [maven koordinatları](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11).

```sh
spark-shell --master yarn --packages "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5"

```

### <a name="using-jupyter-notebooks"></a>Jupyter not defterlerini kullanma

Jupyter not defterleri HDInsight içinde kullanıyorsanız, spark Sihirli kullanabileceğiniz `%%configure` bağlayıcının maven koordinatlarını belirtmek için hücre.

```python
{ "name":"Spark-to-Cosmos_DB_Connector",
  "conf": {
    "spark.jars.packages": "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.3.5",
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
   ...
}
```

> Not, eklenmesi, `spark.jars.excludes` Bağlayıcısı, Apache Spark ve Livy arasındaki olası çakışmaları kaldırmak için özeldir.

### <a name="build-the-connector"></a>Bağlayıcı oluşturma

Şu anda bu bağlayıcı proje kullanan `maven` bağımlılıkları oluşturmak için yapmanız çalıştırabilirsiniz:

```sh
mvn clean package
```

## <a name="working-with-our-samples"></a>Örneklerimizi ile çalışma

[Cosmos DB Spark GitHub deposu](https://github.com/Azure/azure-cosmosdb-spark) aşağıdaki örnek not defterleri ve betikleriniz deneyebilirsiniz.

* **Spark ve Cosmos DB (Seattle) zamanında uçuş performans** [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.html): Spark, Spark SQL, GraphFrames ve ML komut zincirlerini kullanarak tahmin uçuş gecikme göstermek için HDInsight Jupyter notebook hizmeti kullanarak Cosmos DB'ye bağlanın.
* **[Cosmos DB değişiklik akışı ile Spark bağlanma](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Spark%2Band%2BCosmos%2BDB%2BChange%2BFeed.ipynb)** : Spark, Cosmos DB değişiklik akışı'na bağlanma hakkında hızlı bir gösterimi.
* **Apache Spark ve Azure Cosmos DB değişiklik akışı ile kaynak twitter**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.html)
* **Cosmos DB grafik sorgu için Apache Spark ile**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.html)
* **[Azure Databricks, Azure Cosmos DB'ye bağlanmanın](https://docs.databricks.com/spark/latest/data-sources/azure/cosmosdb-connector.html)**  kullanarak `azure-cosmosdb-spark`.  Bağlı burada da bir Azure Databricks sürümüdür [zamanında uçuş performans not defteri](https://github.com/dennyglee/databricks/tree/master/notebooks/Users/denny%40databricks.com/azure-databricks).
* **[Azure Cosmos DB ile lambda mimarisi ve HDInsight (Apache Spark)](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/readme.md)** : Cosmos DB ve Spark'ı kullanarak büyük veri işlem hatları koruma işlemsel ek yükü azaltabilir.

## <a name="more-information"></a>Daha Fazla Bilgi

Daha fazla bilgi sahibiz `azure-cosmosdb-spark` [wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki) dahil olmak üzere:

* [Azure Cosmos DB Spark Bağlayıcısı Kullanıcı Kılavuzu](https://github.com/Azure/azure-documentdb-spark/wiki/Azure-Cosmos-DB-Spark-Connector-User-Guide)
* [Toplamalar örnekleri](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)

### <a name="configuration-and-setup"></a>Yapılandırma ve Kurulum

* [Spark Bağlayıcısı yapılandırması](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuration-references)
* [Cosmos DB Bağlayıcısı Kurulum Spark'a](https://github.com/Azure/azure-documentdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup) (Sürüyor)
* [Azure cosmos DB Apache Spark (HDI) aracılığıyla Power BI doğrudan sorgu yapılandırma](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuring-Power-BI-Direct-Query-to-Azure-Cosmos-DB-via-Apache-Spark-(HDI))

### <a name="troubleshooting"></a>Sorun giderme

* [Cosmos DB toplamaları kullanma](https://github.com/Azure/azure-documentdb-spark/wiki/Troubleshooting:-Using-Cosmos-DB-Aggregates)
* [Bilinen Sorunlar](https://github.com/Azure/azure-cosmosdb-spark/wiki/Known-Issues)

### <a name="performance"></a>Performans

* [Performans İpuçları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Performance-tips)
* [Sorgu Test çalıştırmaları](https://github.com/Azure/azure-documentdb-spark/wiki/Query-Test-Runs)
* [Yazma Test çalıştırmaları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Writing-Test-Runs)

### <a name="change-feed"></a>Değişiklik Akışı

* [Stream Azure Cosmos DB değişiklik akışı ile Apache Spark'ı kullanarak değişiklikleri işleme](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)
* [Değişiklik akışı tanıtımları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Yapılandırılmış Stream tanıtımları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)

### <a name="monitoring"></a>İzleme

* [Spark işlerinde application ınsights ile izleme](https://github.com/Azure/azure-cosmosdb-spark/tree/2.3/samples/monitoring)

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, Spark Azure Cosmos DB Bağlayıcısı'ndan indirin [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposu. Aşağıdaki ek kaynaklara depodaki keşfedin:

* [Toplamalar örnekleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Örnek betikler ve Not Defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

İncelemek isteyebilirsiniz [Apache Spark SQL ve DataFrames veri kümeleri Kılavuzu](https://spark.apache.org/docs/latest/sql-programming-guide.html)ve [Azure HDInsight üzerinde Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) makalesi.
