---
title: "Lambda mimarisiyle Azure Cosmos DB ve Hdınsight (Apache Spark) | Microsoft Docs"
description: "Bu makalede Azure Cosmos DB ve Hdınsight Spark kullanarak bir lambda mimarisi uygulama açıklar"
keywords: Lambda mimarisi
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: 273aeae9-e31c-4a43-b216-5751c46f212e
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 01/19/2018
ms.author: denlee
ms.openlocfilehash: 612a586a2f260221cd407ab8611596306d9e7361
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="implement-a-lambda-architecture-on-the-azure-platform"></a>Azure platformunda lambda mimarisi uygulama 

Lambda mimarileri büyük veri kümelerinin etkili bir veri işleme etkinleştirin. Lambda mimariler, büyük veri sorgulama ilgili gecikmeyi en aza indirmek için toplu işleme, akış işleme ve hizmet katmanındaki kullanın. 

Azure üzerinde bir lambda mimarisi uygulamak için gerçek zamanlı büyük veri analizi hızlandırmak için aşağıdaki teknolojileri birleştirebilirsiniz:
* [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), sektörün ilk Genel dağıtılmış, birden çok model veritabanı hizmeti. 
* [Azure Hdınsight'ta Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/), büyük ölçekli veri analizi uygulamaları çalıştıran bir işleme altyapısı
* Azure Cosmos DB [akış değiştirme](change-feed.md), Hdınsight işlemek için batch katmana yeni veri akışları
* [Spark Azure Cosmos DB Bağlayıcısı](spark-connector.md)

Bu makalede, özgün çok katmanlı tasarımı ve işlemlerini kolaylaştıran bir "rearchitected" lambda mimarisi yararları dayalı bir lambda mimarisi temelleri açıklanır.  

Lambda mimarisi ve lambda mimarisi örnek kaynaklar genel bakış için aşağıdaki videoyu izleyin:

> [!VIDEO https:///channel9.msdn.com/Events/Connect/2017/T135/player]
>

## <a name="what-is-a-lambda-architecture"></a>Lambda mimarisi nedir?
Lambda mimarisi genel, ölçeklenebilir ve hataya dayanıklı veri işleme mimarisi adresine toplu ve Gecikmeli senaryolar tarafından açıklandığı şekilde hızlandırmak [Nathan Marz](https://twitter.com/nathanmarz).

![Lambda mimarisi gösteren diyagram](./media/lambda-architecture/lambda-architecture-intro.png)

Kaynak: http://lambda-architecture.net/

Lambda mimarisinin temel ilkeleri olarak başına önceki diyagramda açıklanan [https://lambda-architecture.net](http://lambda-architecture.net/).

 1. Tüm **veri** içine gönderilen *her ikisi de* *toplu katman* ve *hızı katman*.
 2. **Toplu katman** ana bir veri kümesine (sabit, yalnızca append ham veri kümesi) sahiptir ve toplu görünümler önceden hesaplar.
 3. **Hizmet katmanı** hızlı sorgular için toplu görünümler vardır. 
 4. **Hızı katman** işleme süresi (için hizmet katmanı) dengeler ve yalnızca son veri ile ilgilidir.
 5. Tüm sorguları birleştirme sonuçlarına göre toplu görünümler ve gerçek zamanlı görünümleri ya da ayrı ayrı ping yanıtlanması.

Daha fazla okuduktan sonra biz yalnızca aşağıdakileri kullanarak bu mimarisi uygulama mümkün olacaktır:

* Azure Cosmos DB collection(s)
* Hdınsight'a (Apache Spark 2.1)
* Spark bağlayıcı [1.0](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark_2.1.0_2.11-1.0.0)

## <a name="speed-layer"></a>Hızı katmanı

İşlemler açısından bakıldığında, iki veri akışları verileri doğru durumunu sağlarken koruma karmaşık bir çaba olabilir. İşlemleri basitleştirmek üzere kullanma [Azure Cosmos DB Değiştir akışı Destek](change-feed.md) durumunu korumak için *toplu katman* aracılığıyla Azure Cosmos DB değişiklik günlüğü ortaya sırasında *değiştirmek akış API* için *hızı katman*.  
![Yeni veri, hız katman ve lambda mimarisi ana veri kümesi kısmı vurgulama diyagramı](./media/lambda-architecture/lambda-architecture-change-feed.png)

Bu katmanlar önemli yenilikler:

 1. Tüm **veri** itildiği *yalnızca* Azure Cosmos Veritabanına, bu nedenle, çok atama sorunlarını önleyebilirsiniz.
 2. **Toplu katman** ana bir veri kümesine (sabit, yalnızca append ham veri kümesi) sahiptir ve toplu görünümler önceden hesaplar.
 3. **Hizmet katmanı** sonraki bölümde açıklanmaktadır.
 4. **Hızı katman** Azure Cosmos DB Değiştir Akış okumak için Hdınsight (Apache Spark) kullanır. Bu, verilerinizi de sorgulamak ve eşzamanlı olarak işlemeye ilişkin devam ettirmek sağlar.
 5. Tüm sorguları birleştirme sonuçlarına göre toplu görünümler ve gerçek zamanlı görünümleri ya da ayrı ayrı ping yanıtlanması.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Kod örneği: Spark akış bir Azure Cosmos DB değişikliği akış yapılandırılmış
Bir parçası olarak akış Azure Cosmos DB Değiştir hızlı prototipi çalıştırmak için **hızı katman**, Twitter veri parçası olarak kullanarak test edebilirsiniz [akış işleme Azure, değiştirme DB Cosmos akışı ve Apache Sparkkullanarakdeğişiklikleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)örnek. Twitter çıkış hızla başlatmak için kod örneğinde bkz [Akış akış Twitter'dan Cosmos DB](https://github.com/tknandu/TwitterCosmosDBFeed). Önceki örnekte, Twitter verilerini Azure Cosmos Veritabanına yüklemekte olduğunuz ve akış değişiklik bağlanmak için (Apache Spark) Hdınsight kümenizi sonra ayarlayabilirsiniz. Bu yapılandırmayı ayarlama hakkında daha fazla bilgi için bkz: [Apache Spark Azure Cosmos DB bağlayıcı kurulumu için](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup).  

Aşağıdaki kod parçacığını nasıl yapılandırılacağını göstermektedir `spark-shell` , çalışan bir aralık sayısı gerçekleştirmek için gerçek zamanlı Twitter veri akışı, incelemeleri akışı, bir Azure Cosmos DB değişikliği bağlanmak için bir yapılandırılmış iş akışında çalıştırmak için.

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceConfigMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[MASTER KEY]",
"Database" -> "[DATABASE]",
"Collection" -> "[COLLECTION]",
"ConnectionMode" -> "Gateway",
"ChangeFeedCheckpointLocation" -> "checkpointlocation",
"changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Interval Count")

// Start reading change feed as a stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()

// Start streaming query to console sink
val query = streamData.withColumn("countcol", streamData.col("id").substr(0, 0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
```

Tam kod örnekleri için bkz: [azure-cosmosdb-spark/lambda/samples](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)dahil:
* [Cosmos DB değişiklik Feed.scala akış sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala)
* [Cosmos DB değişiklik Feed.scala akış etiketleri sorgudan](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala)

Bu çıktı bir `spark-shell` sürekli bir aralık sayısı Twitter veri karşı akış Azure Cosmos DB değişikliği gerçekleştiren yapılandırılmış bir akış işi çalıştıktan konsol. Aşağıdaki resimde akış işi çıktısını gösterir ve aralığı sayar.

![Akış Azure Cosmos DB değişiklik Twitter verilerden karşı aralık sayısı gösteren akış çıkışı](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Değiştirme akış Azure Cosmos DB hakkında daha fazla bilgi için bkz:

* [Destek Azure Cosmos DB'de akış değişiklik ile çalışma](change-feed.md)
* [İşlemci kitaplığı Azure CosmosDB değişiklik Giriş akışı](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [: Akış işleme Azure CosmosDB değişiklik akış + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Toplu işlem ve Katmanlar hizmet veren
Yeni veriler Azure Cosmos DB, (burada değişiklik akış kullanılıyor için hız katman) bu yüklendikten sonra yerdir **ana veri kümesi** (bir değişmez, yalnızca append ham veri kümesi) yer alıyor. Bu noktadan başlayarak, Hdınsight (Apache Spark) ön işlem işlevleri gerçekleştirmek için kullanmak **toplu katman** için **hizmet katmanı**aşağıdaki görüntüde gösterildiği gibi:

![Toplu iş katmanı ve hizmet katmanı lambda mimarisinin vurgulama diyagramı](./media/lambda-architecture/lambda-architecture-batch-serve.png)

Bu katmanlar önemli yenilikler:

 1. Tüm **veri** (çok noktaya yayın sorunlarını önlemek için) yalnızca Azure Cosmos Veritabanına gönderilir.
 2. **Toplu katman** sahip Azure Cosmos DB içinde depolanan bir ana veri kümesi (sabit, yalnızca append ham veri kümesi). HDI Spark kullanarak, hesaplanan toplu görünümlerinizi depolanması, toplamalar önceden hesaplayabilirsiniz.
 3. **Hizmet katmanı** ana veri kümesi için koleksiyonlarıyla Azure Cosmos DB veritabanıdır ve toplu görünüm hesaplanır.
 4. **Hızı katman** bu makalenin sonraki bölümlerinde ele alınmıştır.
 5. Tüm sorguların sonuçlarını toplu görünümler ve gerçek zamanlı görünümleri birleştirme veya ayrı ayrı ping yanıtlanması.

### <a name="code-example-pre-computing-batch-views"></a>Kod örneği: Toplu görünümler önceden hesaplama
Önceden hesaplanan görünümleri yürütmek nasıl göstermek için **ana veri kümesi** Azure Cosmos DB'de Apache Spark not defterlerini gelen aşağıdaki kod parçacıklarını kullanma [Lambda mimarisi Rearchitected - toplu iş katmanı ](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) ve [Lambda mimarisi Rearchitected - katman hizmet veren için toplu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb). Bu senaryoda, Azure Cosmos DB içinde depolanan Twitter verileri kullanın.

Twitter verilerini Azure Cosmos PySpark kodu kullanarak DB içinde yapılandırma bağlantısı oluşturarak başlayalım.

```
# Configuration to connect to Azure Cosmos DB
tweetsConfig = {
  "Endpoint" : "[Endpoint URL]",
  "Masterkey" : "[Master Key]",
  "Database" : "[Database]",
  "Collection" : "[Collection]", 
  "preferredRegions" : "[Preferred Regions]",
  "SamplingRatio" : "1.0",
  "schema_samplesize" : "200000",
  "query_custom" : "[Cosmos DB SQL Query]"
}

# Create DataFrame
tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()

# Create Temp View (to run Spark SQL statements)
tweets.createOrReplaceTempView("tweets")
```

Ardından, şimdi tweet'leri kümesinin ilk 10 diyez etiketlerini belirlemek için aşağıdaki Spark SQL deyimini çalıştırın. Bu Spark SQL sorgu için biz bunu Jupyter not defterinde doğrudan bu kod parçacığını aşağıdaki çıktı çubuk grafik kullanıyorsunuz.

```
%%sql
select hashtags.text, count(distinct id) as tweets
from (
  select 
    explode(hashtags) as hashtags,
    id
  from tweets
) a
group by hashtags.text
order by tweets desc
limit 10
```

![Tweet'leri diyez başına sayısını gösteren grafik](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Sorgunuz sahip olduğunuza göre şimdi onu bir koleksiyona çıktı verilerini farklı bir koleksiyona kaydetmek için Spark Bağlayıcısı'nı kullanarak kaydedin.  Bu örnekte, Scala bağlantıyı göstermek için kullanın. Önceki örneğe benzer Apache Spark DataFrame farklı bir Azure Cosmos DB koleksiyona kaydetmek için yapılandırma bağlantısı oluşturun.

```
val writeConfigMap = Map(
    "Endpoint" -> "[Endpoint URL]",
    "Masterkey" -> "[Master Key]",
    "Database" -> "[Database]",
    "Collection" -> "[New Collection]", 
    "preferredRegions" -> "[Preferred Regions]",
    "SamplingRatio" -> "1.0",
    "schema_samplesize" -> "200000"
)

// Configuration to write
val writeConfig = Config(writeConfigMap)

```

Belirttikten sonra `SaveMode` (belirten kullanılıp kullanılmayacağını `Overwrite` veya `Append` belgeleri), oluşturma bir `tweets_bytags` DataFrame benzer şekilde önceki örnekte Spark SQL sorgusu.  İle `tweets_bytags` DataFrame oluşturulmuş, kullanarak kaydedebilirsiniz `write` daha önce belirtilen kullanarak yöntemini `writeConfig`.

```
// Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
import org.apache.spark.sql.{Row, SaveMode, SparkSession}

// Create new DataFrame of tweets tags
val tweets_bytags = spark.sql("select hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc")

// Save to Cosmos DB (using Append in this case)
tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Şimdi bu son deyim, Spark DataFrame yeni bir Azure Cosmos DB koleksiyon kaydetti; lambda mimarisi açısından bakıldığında, bu olduğundan, **toplu Görünüm** içinde **hizmet katmanı**.
 
#### <a name="resources"></a>Kaynaklar

Tam kod örnekleri için bkz: [azure-cosmosdb-spark/lambda/samples](vhttps://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda) dahil olmak üzere:
* Lambda mimarisi Rearchitected - toplu katman [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb)
* Lambda mimarisi Rearchitected - hizmet veren bir katmana toplu [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb)

## <a name="speed-layer"></a>Hızı katmanı
Daha önce not ettiğiniz, kullanarak Azure Cosmos DB Değiştir Akış kitaplığı işlemleri toplu ve hız katmanlar arasında basitleştirmenize olanak sağlar. Bu mimaride, Apache Spark (aracılığıyla Hdınsight) gerçekleştirmek için kullanın. *akış yapılandırılmış* verileri sorgular. Diğer sistemleri bu verilere erişebilmesi için yapılandırılmış akış sorgularınızı sonuçlarını geçici olarak sürdürmek isteyebilirsiniz.

![Lambda mimarisinin hızı katman vurgulama diyagramı](./media/lambda-architecture/lambda-architecture-speed.png)

Bunu yapmak için yapılandırılmış akış sorguların sonuçlarını kaydetmek için ayrı bir Azure Cosmos DB koleksiyonu oluşturun.  Bu, diğer sistemler erişim sağlamak için bu bilgileri sağlar yalnızca Apache Spark. De Cosmos DB için-yaşam süresi (TTL) özelliği ile ayarlanmış bir süre sonra otomatik olarak silinecek belgelerinizi yapılandırabilirsiniz.  Azure Cosmos DB TTL özelliği hakkında daha fazla bilgi için bkz: [yaşam süresi otomatik olarak ile Azure Cosmos DB koleksiyonlarda verileri süresi dolacak](time-to-live.md)

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceCollectionName = "[SOURCE COLLECTION NAME]"
val sinkCollectionName = "[SINK COLLECTION NAME]"

val configMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[COSMOSDB MASTER KEY]",
"Database" -> "[DATABASE NAME]",
"Collection" -> sourceCollectionName,
"ChangeFeedCheckpointLocation" -> "changefeedcheckpointlocation")

val sourceConfigMap = configMap.+(("changefeedqueryname", "Structured Stream replication streaming test"))

// Start to read the stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
val sinkConfigMap = configMap.-("collection").+(("collection", sinkCollectionName))

// Start the stream writer to new collection
val streamingQueryWriter = streamData.writeStream.format(classOf[CosmosDBSinkProvider].getName).outputMode("append").options(sinkConfigMap).option("checkpointLocation", "streamingcheckpointlocation")
var streamingQuery = streamingQueryWriter.start()

```

## <a name="lambda-architecture-rearchitected"></a>Lambda mimarisi: Rearchitected
Önceki bölümlerde belirtildiği gibi aşağıdaki bileşenleri kullanarak özgün lambda mimarisi basitleştirebilirsiniz:
* Azure Cosmos DB
* Verilerinizi toplu ve hız katmanlar arasında çok noktaya yayın gerek önlemek için Azure Cosmos DB değişiklik akışı kitaplığı
* Hdınsight'ta Apache Spark
* Azure Cosmos DB için Spark Bağlayıcısı

![Azure Cosmos DB, Spark ve Azure Cosmos DB Değiştir Akış API'sini kullanarak lambda mimarisi rearchitecture gösteren diyagram](./media/lambda-architecture/lambda-architecture-re-architected.png)

Bu tasarımla, yalnızca iki yönetilen hizmetler, Azure Cosmos DB ve Hdınsight gerekir. Birlikte, bunlar toplu, hizmet ve lambda mimarisinin hızı katmanı adresi. Bu, yalnızca işlem aynı zamanda veri akışı basitleştirir. 
 1. Tüm verileri işleme için Azure Cosmos Veritabanına gönderilir
 2. Toplu iş katmanı ana bir veri kümesine (sabit, yalnızca append ham veri kümesi) sahiptir ve toplu görünümler önceden hesaplar
 3. Hizmet katmanı verilerin hızlı sorgular için toplu görünümler vardır.
 4. Hızı katman işleme süresi (için hizmet katmanı) dengeler ve yalnızca son veri ile ilgilidir.
 5. Toplu görünümler ve gerçek zamanlı görünümleri sonuçlarından birleştirerek tüm sorguları yanıtlanması.

### <a name="resources"></a>Kaynaklar

 * **Yeni veri**: [Twitter'dan CosmosDB için Akış akış](https://github.com/tknandu/TwitterCosmosDBFeed), yeni verileri Azure Cosmos Veritabanına itme mekanizması olduğu.
 * **Toplu iş katmanı:** toplu katman oluşan *ana veri kümesi* (bir değişmez, yalnızca append ham veri kümesi) ve toplu içine gönderilen veri görünümlerini önceden işlem olanağı **hizmet katmanı** .
    * **Lambda mimarisi Rearchitected - toplu katman** not defteri [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) sorguları *ana veri kümesi* Toplu görünümler kümesi.
 * **Hizmet katmanı:** **hizmet katmanı** hızlı sorgular için toplu görünümler (örneğin toplamalar, belirli dilimleyicileri, vb.) sonuçta önceden hesaplanan veri oluşur.
    * **Lambda mimarisi Rearchitected - hizmet veren bir katmana toplu** not defteri [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) toplu veri sunma katmana iter; diğer bir deyişle, Spark tweet'leri toplu iş koleksiyonu sorgular, işler ve başka bir koleksiyona (hesaplanan bir toplu iş) depolar.
* **Hızı katman:** **hızı katman** okuyup hemen hareket akış Azure Cosmos DB Değiştir kullanılarak Spark oluşur. Veriler için de kaydedilebilir *RT hesaplanan* diğer sistemler sorgulayabilmesi gerçek zamanlı bir çalışan aksine işlenen gerçek zamanlı verileri kendilerini sorgu.
    * [Akış sorgudan değiştirmek DB Cosmos akışı](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala) scala betik spark Kabuğu'ndan bir aralık sayısı işlem için akış Azure Cosmos DB Değiştir gelen bir akış sorgu yürütür.
    * [Akış etiketleri sorgudan değiştirmek DB Cosmos akışı](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala) scala komut dosyası etiketleri spark Kabuğu'ndan aralığı sayısı işlem için akış Azure Cosmos DB Değiştir gelen bir akış sorgu yürütür.
  
## <a name="next-steps"></a>Sonraki adımlar
Henüz yapmadıysanız, Spark Azure Cosmos DB bağlayıcısından karşıdan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposunu ve depodaki ek kaynakları araştırın:
* [Lambda mimarisi](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)
* [Dağıtılmış toplamalar örnekleri](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Örnek komut dosyaları ve dizüstü bilgisayarlar](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)
* [Yapılandırılmış akış gösterileri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)
* [Değişiklik gösterileri akışı](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Akış Azure Cosmos DB Değiştir akıştan geldi ve Apache Spark kullanarak değişiklikleri işleme](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)

Gözden geçirmek isteyebilirsiniz [Apache Spark SQL, DataFrames ve veri kümeleri Kılavuzu](http://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure hdınsight'ta Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) makalesi.
