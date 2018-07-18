---
title: Apache Spark, Azure Cosmos DB'ye bağlanma | Microsoft Docs
description: Apache Spark üzerinde çok kiracılı, Global olarak dağıtılmış toplanmalar ve veri Bilimleri gerçekleştirmek için Azure Cosmos DB veritabanı sistemi Microsoft Dağıtılmış bağlanmanıza olanak sağlayan Azure Cosmos DB Spark Bağlayıcısı hakkında bilgi edinmek için bu öğreticiyi kullanın. Bu Bulut için tasarlandı.
keywords: Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: cae66a40882231f7762af29cdeaaf658dd2aa968
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113956"
---
# <a name="accelerate-real-time-big-data-analytics-by-using-the-spark-to-azure-cosmos-db-connector"></a>Azure Cosmos DB Bağlayıcısı için Spark'ı kullanarak gerçek zamanlı büyük veri analizi hızlandırın
 
Spark Bağlayıcısı Azure Cosmos DB için Azure Cosmos DB, girdi olarak davranan veya Apache Spark işleri için çıkış sağlar. Bağlanma [Spark](http://spark.apache.org/) için [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) yeteneğinizi hızlı ilerleyen veri bilimi sorunları çözmek için hızlı bir şekilde kalıcı hale getirmek ve veri sorgulamak için Azure Cosmos DB kullanabileceğiniz hızlandırır. Spark Bağlayıcısı Azure Cosmos DB için Azure Cosmos DB tarafından yönetilen yerel dizinleri verimli bir şekilde yararlanır. Analizler gerçekleştirin ve nesnelerin Internet ' (IOT) arasında veri, veri bilimi ve analiz senaryolarına dağıtılmış göndererek hızla küresel olarak değişen karşı koşul filtreleme güncelleştirilebilir sütunlara dizinler etkinleştirin.

Bu videoda Bağlayıcısı Azure Cosmos DB için Spark hakkında daha fazla bilgi edinin:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Bağlayıcı bileşenleri

Azure Cosmos DB Bağlayıcısı için Spark aşağıdaki bileşenleri kullanır:

* [Azure Cosmos DB](http://documentdb.com) sağlayın ve hem aktarım hızını ve depolamayı herhangi sayıda coğrafi bölgesinde esnek ölçeklendirme müşterilerin sağlar.  

* [Apache Spark](http://spark.apache.org/) hız, kullanım kolaylığı ve Gelişmiş analiz geçici olarak oluşturulmuş bir güçlü açık kaynak işleme altyapısıdır.  

* [Azure Databricks üzerinde Apache Spark kümesi](https://docs.azuredatabricks.net/getting-started/index.html) kullanarak spark kümesi üzerinde spark işleri çalıştırma için.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Apache Spark, Azure Cosmos DB'ye bağlanma

Apache Spark ve Azure Cosmos DB'ye bağlanmak için iki yaklaşım vardır:

1. Kullanarak [Azure Cosmos DB SQL Python SDK'sı](https://github.com/Azure/azure-documentdb-python), Python tabanlı bir spark "pyDocumentDB" da adlandırılır ve Cosmos DB Bağlayıcısı.  

2. Kullanarak [Azure Cosmos DB SQL Java SDK'sı](https://github.com/Azure/azure-documentdb-java) Cosmos DB Bağlayıcısı için Java tabanlı bir spark.


**Desteklenen sürümler**

| Bileşen | Sürüm |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x 2.3.x |
| Scala|2.11|
| Databricks çalışma zamanı sürümü | > 3.4 |
| Azure Cosmos DB SQL Java SDK'sı | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Python veya pyDocumentDB SDK'sını kullanarak bağlanma

Aşağıdaki görüntüde, pyDocumentDB SDK'sı uygulama mimarisi gösterilmektedir:

![Azure Cosmos DB veri akışı aracılığıyla pyDocumentDB DB için spark](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Veri akışı

PyDocumentDB uygulamasının veri akışı aşağıdaki gibidir:

* Ana düğüm spark'ın Azure Cosmos DB ağ geçidi düğümü pyDocumentDB aracılığıyla bağlanır. Bir kullanıcı, yalnızca Azure Cosmos DB bağlantıları ve spark belirtir. İlgili ana ve ağ geçidi düğümleri bağlantıları kullanıcı için saydamdır.  

* Ağ geçidi düğümü, Azure Cosmos DB sorguyu daha sonra veri düğümü, koleksiyonun bölümlerine karşı çalıştığı sorgusu yapar. Bu sorgulara yanıt ağ geçidi düğüme geri gönderilir ve bu sonuç kümesi için spark ana düğüm döndürülür.  

* Sonraki sorguları (örneğin, bir spark veri çerçevesine) işleme için Spark çalışan düğümlerine gönderilir.  

Spark ve Azure Cosmos DB arasındaki iletişim spark ana düğümü ve Azure Cosmos DB ağ geçidi düğümleri sınırlıdır. Bu iki düğüm arasında Aktarım katmanı sağladığından hızlı sorgular gidin.

Spark pyDocumentDB SDK kullanarak Azure Cosmos DB'ye bağlanmak için aşağıdaki adımları çalıştırın:

1. Oluşturma bir [Azure Databricks çalışma alanı](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) ve [spark kümesi](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) ((Apache Spark 2.3.0, Scala 2.11 içerir) Databricks çalışma zamanı sürüm 4.0, çalışma alanı içinde.  

2. Bir küme oluşturulur ve çalışan sonra gidin **çalışma** > **Oluştur** > **Kitaplığı**.  
3. Yeni Kitaplık iletişim kutusundan seçin **Python Yumurta karşıya yükleyin veya Pypı** kaynağı **pydocumentdb** ad olarak **kitaplık yükleme**. Bulmak ve yüklemek için PyDocumentdb SDK'sı için pip paketleri zaten yayımlandı. 

   ![Oluşturma ve kitaplığı ekleme](./media/spark-connector/create-library.png)

4. Kitaplık yüklendikten sonra daha önce oluşturduğunuz kümeye ekleyin.  

5. İleri gidin **çalışma** > **Oluştur** > **not defteri**.  

6. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin, **Python** dili olarak. Daha önce oluşturduğunuz kümeyi seçin ve seçin ve açılan **Oluştur**.  

7. İletişim taşıma basitliğinin pyDocumentDB görece basit kullanarak Azure Cosmos DB için spark sorgudan yürütülmesini sağlar. Daha az sayıda çalışacak uçuşlar örnek verileri kullanarak spark sorguları barındırılan "doctorwho" içinde genel olarak erişilebilir olan Cosmos DB hesabı. Not defterini HTML sürümünü barındırılan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposu. Dosyalarını indirin ve gidin `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html` hesabınıza Azure Databricks not defteri içeri aktarabilir ve çalıştırın. Aşağıdaki bölümde ayrıntılı kod bloklarında işlevselliğini açıklar.

Aşağıdaki kod parçacığı, pyDocumentDB SDK içeri aktarma ve spark bağlamında sorgu çalıştırma işlemi gösterilmektedir. Kod parçacığında belirtildiği gibi pyDocumentDB SDK Azure Cosmos DB hesabına bağlanmak için gereken bağlantı parametrelerini içerir. Bu, gerekli kitaplıkları alır, ana anahtarı hem de Azure Cosmos DB istemci (pydocumentdb.document_client) oluşturmak için konak yapılandırır.


```python
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

Ardından sorguları çalıştırabilirsiniz; aşağıdaki kod parçacığı DoctorWho hesabındaki airports.codes koleksiyonuna bağlanır ve Washington eyaletinde havaalanı şehirlerin ayıklamak için bir sorgu çalıştırır. 

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

```

Sorgu yürütüldükten sonra "query_iterable. sonuç olur. Daha sonra bir spark veri çerçevesine dönüştürülür Python listesini dönüştürülen QueryIterable". 

```python
# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(list(query))

# Show data
df.show()
```

## <a name="considerations-when-using-pydocumentdb-sdk"></a>PyDocumentDB SDK kullanmayla ilgili konular
Spark, SDK'sı, aşağıdaki senaryolarda önerilir pyDocumentDB kullanarak Azure Cosmos DB'ye bağlanma:

* Python'ı kullanmak istiyorsunuz.  

* Görece küçük sonuç almak için Azure Cosmos DB'den kümesi iade ettiğiniz. Azure Cosmos DB temel alınan veri kümesindeki oldukça büyük olabilir ve koşul filtreleri, Azure Cosmos DB kaynağına karşı çalışan veya filtreler uygulayarak unutmayın.

## <a name="connect-by-using-the-java-sdk"></a>Java SDK'sını kullanarak bağlanma

Aşağıdaki görüntüde Azure Cosmos DB SQL Java SDK'sı uygulama mimarisini gösterir ve spark çalışan düğümler arasında verileri taşır:

![Azure Cosmos DB Bağlayıcısı için Spark veri akışı](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Veri akışı

Java SDK'sı uygulamasının veri akışı aşağıdaki gibidir:

* Spark ana düğüm bölüm eşlemesini almak için Azure Cosmos DB ağ geçidi düğümüne bağlanmış olursunuz. Bir kullanıcı yalnızca spark ve Azure Cosmos DB bağlantıları belirtir. İlgili ana ve ağ geçidi düğümleri bağlantıları kullanıcı için saydamdır.  

* Bu bilgiler, spark ana düğüme geri sağlanır. Bu noktada, bölümler ve konumlarına erişmek için gereken bir Azure Cosmos DB belirlemek amacıyla sorgu ayrıştırılamadı görebilmeniz gerekir.  

* Bu bilgiler, spark çalışan düğümlerine iletilir.  

* Spark çalışan düğümlerine doğrudan verileri ayıklamak ve spark bölüm çalışan düğümleri olarak verileri döndürmek için Azure Cosmos DB bölüm bağlanın.  

Spark ve Azure Cosmos DB arasındaki iletişimi önemli ölçüde daha hızlı nedeniyle veri taşıma, spark çalışan düğümleri ve Azure Cosmos DB veri düğümleri (bölümler) arasında. Bu belgede, Azure Cosmos DB hesabı için bazı örnek twitter verilerini gönderin ve bu verileri kullanarak spark sorgular çalıştırın. Azure Cosmos DB için örnek Twitter verilerini yazmak için aşağıdaki adımları kullanın:

1. Oluşturma bir [Azure Cosmos DB SQL API hesabı](create-sql-api-dotnet.md#create-a-database-account) ve [bir koleksiyon Ekle](create-sql-api-dotnet.md#add-a-collection) hesabı.  

2. İndirme [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) örnek Twitter verilerini Azure Cosmos DB'ye yazılmak için kullanılan github örnek.  

3. Bir komut istemi açın ve aşağıdaki komutları çalıştırarak modülleri Tweepy ve pyDocumentdb yükleyin:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Twitter akış örneği içeriğini ayıklayın ve config.py dosyasını açın. MasterKey, konak, Databaseıd, CollectionId ve preferredLocations değerlerini güncelleştirin.  

5. Gidin `http://apps.twitter.com/` ve betik yeni bir uygulama olarak akış Twitter kaydedin. Bir ad ve uygulamanız için uygulama seçtikten sonra birlikte sağlanacak bir **tüketici anahtarı, tüketici gizli, erişim belirteci ve erişim belirteci gizli**. Bu değerleri kopyalayın ve Twitter için programlı erişim sağlamak için config.py dosyasını güncelleştirin.   

6. Config.py dosyasını kaydedin. Komut istemini açın ve aşağıdaki komutu kullanarak python uygulamasını çalıştırın:

   ```bash
   Python driver.py
   ```

7. Portalda Azure Cosmos DB koleksiyonu gidin ve twitter verilerini koleksiyona yazıldığından emin olun.

### <a name="find-and-attach-java-sdk-to-the-spark-cluster"></a>Bul ve spark kümesine Java SDK'sı ekleme

1. Oluşturma bir [Azure Databricks çalışma alanı](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) ve [spark kümesi](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) ((Apache Spark 2.3.0, Scala 2.11 içerir) Databricks çalışma zamanı sürüm 4.0, çalışma alanı içinde.  

2. Bir küme oluşturulur ve çalışan sonra gidin **çalışma** > **Oluştur** > **Kitaplığı**.  

3. Yeni Kitaplık iletişim kutusundan seçin **Maven koordinatı** koordinat değeri, kaynak olarak sağlamak **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**seçip  **Kitaplığı oluşturma**. Maven bağımlılıkları çözümlenir ve paket çalışma alanınıza eklenir. Yukarıdaki maven koordinatı biçimde 2.3.0 spark sürümü temsil eder, 2.11 scala sürümünü temsil eder ve Azure Cosmos DB Bağlayıcısı sürümü 1.2.0 temsil eder. 

4. Kitaplık yüklendikten sonra daha önce oluşturduğunuz kümeye ekleyin. 

Bu makalede, aşağıdaki senaryolarda, Java SDK'sı spark Bağlayıcısı kullanımı gösterilmektedir:

* Azure Cosmos DB'den twitter verilerini okuyun  

* Azure Cosmos DB için akış twitter verilerini okuma  

* Twitter verilerini Azure Cosmos DB'ye yazılmak 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Azure Cosmos DB'den twitter verilerini okuyun
 
Bu bölümde, Azure Cosmos DB'den toplu Twitter verilerini okumak için spark sorgular çalıştırın. Not defterini HTML sürümünü barındırılan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposu. Dosyalarını indirin ve gidin `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html` hesabınıza Azure Databricks not defteri alma, URI ana anahtar, veritabanı, koleksiyon adları hesabı güncelleştirebilir ve çalıştırın veya Not Defteri gibi oluşturabilirsiniz:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**. 

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin, **Python** dili olarak açılan daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtarı hesabına bağlanın ve spark.read.format() komutunu kullanarak tweetleri okumak için veritabanı ve koleksiyon değerlerini güncelleştirin.

   ```python
   # Configuration Map
   tweetsConfig = {
   "Endpoint" : "<Your Azure Cosmos DB endpoint>",
   "Masterkey" : "<Primary key of your Azure Cosmos DB account>",
   "Database" : "<Your Azure Cosmos DB database name>",
   "Collection" : "<Your Azure Cosmos DB collection name>", 
   "preferredRegions" : "East US",
   "SamplingRatio" : "1.0",
   "schema_samplesize" : "200000",
   "query_custom" : "SELECT c.id, c.created_at, c.user.screen_name, c.user.lang, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   }
   # Read Tweets
   tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()
   tweets.createOrReplaceTempView("tweets")
   #tweets.cache()

   ```

4. Önbelleğe alınan verileri farklı diyez etiketlerini tarafından tweetleri sayısını almak için sorguyu çalıştırın. 

   ```python
   %sql
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

Java SDK'sı için yapılandırma eşlemesi aşağıdaki değerlerini destekler: 

|Ayar  |Açıklama  |
|---------|---------|
|query_maxdegreeofparallelism  | Paralel sorgu yürütme işlemi sırasında istemci tarafında çalışma eşzamanlı işlemlerin sayısını ayarlar. 0'dan büyük bir değere ayarlanırsa, atanan değer için eşzamanlı işlemlerin sayısını sınırlar. 0'dan düşük bir değere ayarlanırsa, sistem çalıştırmak için eşzamanlı işlemlerin sayısını otomatik olarak karar verir. Her bir yürütücü koleksiyon bölümüyle bağlayıcı eşlemeleri gibi bu değer okuma işlemi üzerinde hiçbir etkisi olmaz.        |
|query_maxbuffereditemcount     |    Arabelleğe alınıp öğe maksimum sayısı, paralel sorgu yürütme işlemi sırasında istemci tarafında ayarlar. 0'dan büyük bir değere ayarlanırsa, atanan değer için arabelleğe alınan öğe sayısını sınırlar. 0'dan düşük bir değere ayarlanırsa, sistem arabelleğe öğe sayısını otomatik olarak karar verir.     |
|query_enablescan    |   Dizin oluşturma için sunulan uygulanamadı sorguları taramalar etkinleştirme seçeneği olan ayarlar istenen yollarında vazgeçti.       |
|query_disableruperminuteusage  |  İstek birimi (RU) devre dışı bırakır / dakika normal sağlanan RU/saniye aşıldı sorgu kulla kapasitesi.       |
|query_emitverbosetraces   |   Araştırma için ayrıntılı izlemeleri kullanıma yaymak sorgularına izin ver seçeneğine ayarlar.      |
|query_pagesize  |   Her sorgu istek için sorgu sonuç sayfası boyutunu ayarlar. Aktarım hızını iyileştirmek için sorgu sonuçları getirilecek gidiş dönüş sayısını azaltmak için büyük sayfa boyutunu kullanın.      |
|query_custom  |  Azure Cosmos DB sorguyu varsayılan sorguyu Azure Cosmos DB'den verileri getirilirken geçersiz kılmak için ayarlar. Bu değeri sağlandığında, bir sorgu ile yerine de doğrulamaları gönderilen kullanılacağını unutmayın.     |

Senaryoya bağlı olarak farklı yapılandırma değerleri, aktarım hızı ve performansı iyileştirmek için kullanılmalıdır. Yapılandırma anahtarı şu anda duyarsızdır, ve yapılandırma değeri her zaman bir dize olduğunu unutmayın.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Azure Cosmos DB için akış twitter verilerini okuma

Bu bölümde, bir değişiklik akışı, akış twitter verilerini okumak için spark sorgular çalıştırın. Bu bölümde sorguları çalıştırırken, Twitter akışınızı uygulaması çalıştıran ve Azure Cosmos DB'ye veri Pompalama emin olun. Not defterini HTML sürümünü barındırılan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposu. Dosyalarını indirin ve gidin `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html` hesabınıza Azure Databricks not defteri alma, URI ana anahtar, veritabanı, koleksiyon adları hesabı güncelleştirebilir ve çalıştırın veya Not Defteri gibi oluşturabilirsiniz:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin, **Scala** dili olarak açılan daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtarı hesabına bağlanmak için veritabanı ve koleksiyon değerlerini güncelleştirin.

   ```scala
   // This script does the following:
   // - creates a structured stream from a Twitter feed CosmosDB collection (on top of change feed)
   // - get the count of tweets
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import org.codehaus.jackson.map.ObjectMapper
   import com.microsoft.azure.cosmosdb.spark.streaming._
   import java.time._

   val sourceConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB collection name>", 
   "ConnectionMode" -> "Gateway",
   "ChangeFeedCheckpointLocation" -> "/tmp",
   "changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Internal Count")
   ```
4. Başlangıç spark.readStream.format() komutunu kullanarak bir akış olarak akış okuma Değiştir:

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Sorgu konsoluna akış başlatın:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

Java SDK'sı için yapılandırma eşlemesi aşağıdaki değerlerini destekler:

|Ayar  |Açıklama  |
|---------|---------|
|readchangefeed   |  Koleksiyon içeriğini CosmosDB değişiklik akışı ' alınan gösterir. Varsayılan değer false'tur.       |
|changefeedqueryname |   Sorgu tanımlamak için özel bir dize. Bağlayıcı, farklı değişiklik sorguları ayrı olarak akışı için koleksiyon devamlılık belirteci izler. Readchangefeedis true, bu boş değer alamaz, gerekli bir yapılandırma olup olmadığını.      |
|changefeedcheckpointlocation  |   Devamlılık belirteçleri düğüm hataları durumunda kalıcı hale getirmek için yerel dosya depolama yolu.      |
|changefeedstartfromthebeginning  |  Değişiklik akışı başlangıçtan itibaren (true) veya geçerli noktasından (false) başlamalıdır olup olmadığını belirler. Varsayılan olarak, geçerli (false) başlar.       |
|rollingchangefeed  |   Değişiklik akışı olmadığını gösteren bir Boole değeri son sorgudan olmalıdır. Varsayılan değer false, değişiklikleri koleksiyonun ilk okuması sayılır anlamına gelir.      |
|changefeedusenexttoken  |   İşleme hatası senaryoları desteklemek için bir Boole değeri. Geçerli değişiklik toplu iş akışı düzgün bir şekilde işlendiğini ve RDD sonraki toplu değişiklikler almak için sonraki devamlılık belirteçleri kullanmalısınız belirtmek için kullanılır.      |
| InferStreamSchema | Akış veri şeması akış başlangıcında örneğinin alınıp alınmayacağını belirtilen bir Boole değeri. Varsayılan olarak, bu değer ayarlanır true. Veriler örneklenir sonra bu parametreyi akış verilerinin şema değişiklikleri ve true olarak ayarlanırsa, yeni eklenen özellikler akış veri çerçevesinde bırakılır. <br/><br/> Şemadan olmasını akış veri çerçevesi istiyorsanız bu parametreyi false olarak ayarlayın. Bu modda, Azure Cosmos DB değişiklik akışı'ndan okuma belge gövdesinin sistem özellik değerleri tarafından sonuç akış veri çerçevesi 'Gövde' özelliğinde içine sarılır.
 |

### <a name="connection-settings"></a>Bağlantı ayarları

Java SDK'sı aşağıdaki bağlantı ayarları destekler:

|Ayar  |Açıklama  |
|---------|---------|
|connectionmode   |  İç DocumentClient Azure Cosmos DB ile iletişim kurmak için kullanması gereken bağlantı modunu ayarlar. İzin verilen değerler **DirectHttps** (varsayılan değer) ve **ağ geçidi**. DirectHttps bağlantı modu, istekleri doğrudan CosmosDB bölümlere yönlendirir ve bazı gecikme avantajı sağlar.       |
|connectionmaxpoolsize   |  İç DocumentClient tarafından kullanılan bağlantı havuzu boyutu değerini ayarlar. Varsayılan değer 100’dür.       |
|connectionidletimeout  |  Boşta kalan bağlantıların için zaman aşımı değeri saniye cinsinden ayarlar. Varsayılan değer 60 ' dir.       |
|query_maxretryattemptsonthrottledrequests    |  En fazla yeniden deneme sayısını ayarlar. Bu değer, bir istemci üzerinde hız sınırı nedeniyle istek başarısız olması durumunda kullanılır. Belirtilmezse, varsayılan değer 1000 yeniden deneme girişimleri ' dir.       |
|query_maxretrywaittimeinseconds   |  Ayarlar, en fazla'süresini saniye cinsinden yeniden deneyin. Varsayılan olarak, 1000 saniyedir.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Twitter verilerini Azure Cosmos DB'ye yazılmak 

Bu bölümde, aynı veritabanında yeni bir koleksiyon için twitter verilerini toplu yazmak için spark sorgular çalıştırın. Not defterini HTML sürümünü barındırılan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposu. Dosyalarını indirin ve gidin `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html` hesabınıza Azure Databricks not defteri alma, URI ana anahtar, veritabanı, koleksiyon adları hesabı güncelleştirebilir ve çalıştırın veya Not Defteri gibi oluşturabilirsiniz:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin, **Scala** dili olarak açılan daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, twitter veri okuma ve yazma için veritabanı koleksiyonuna bağlanmak için veritabanı ve koleksiyon değerlerini güncelleştirin.

   ```scala
   %scala
   // Import Necessary Libraries
   import org.joda.time._
   import org.joda.time.format._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.config.Config

   // Maps
   val readConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000",
   "query_custom" -> "SELECT c.id, c.created_at, c.user.screen_name, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   )
   val writeConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000"
   ) 

   // Configs
   // get read
   val readConfig = Config(readConfigMap)
   val tweets = spark.read.cosmosDB(readConfig)
   tweets.createOrReplaceTempView("tweets")
   tweets.cache()

   // get write
   val writeConfig = Config(writeConfigMap)
   ```
4. Önbelleğe alınan verileri farklı diyez etiketlerini tarafından tweetleri sayısını almak için sorguyu çalıştırın. 

   ```scala
   %sql
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

5. Tweetleri etiketlerin yeni veri çerçevesi oluşturun ve yeni koleksiyon verileri kaydedin. Aşağıdaki kod çalıştırdıktan sonra portala geri dönün ve veri koleksiyonuna yazıldığından emin olun. 

   ```scala
   %scala
   // Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
   import org.apache.spark.sql.{Row, SaveMode, SparkSession}

   // Create new DataFrame of tweets tags
   val tweets_bytags = spark.sql("select '2018-06-12' as currdt, hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc limit 10")

   // Save to Cosmos DB (using Append in this case)
   // Ensure the baseConfig contains a Read-Write Key
   // The key provided in our examples is a Read-Only Key

   tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
   ```

Java SDK'sı için yapılandırma eşlemesi aşağıdaki değerlerini destekler:

|Ayar  |Açıklama  |
|---------|---------|
| BulkImport | Bulkexecutor'a kitaplığını kullanarak veri alınması gerekip gerekmediğini gösteren bir Boole değeri. Varsayılan olarak, bu değer ayarlanır true. |
|WritingBatchSize  |   Azure Cosmos DB koleksiyonu için verileri yazarken kullanmak için toplu iş boyutu gösterir. <br/><br/> Toplu iş boyutu ' % s'importAll API Bulkexecutor'a kitaplığının giriş olarak sağlanan belgelerin WritingBatchSize parametresi gösterir BulkImport parametresi true olarak ayarlanırsa. Varsayılan olarak, bu değer için 100 bin cinsinden ayarlanır. <br/><br/> BulkImport parametresini false olarak ayarlarsanız, WritingBatchSize parametresi için Azure Cosmos DB koleksiyonu yazarken kullanmak için toplu iş boyutu gösterir. Bağlayıcı createDocument/upsertDocument istekleri toplu işlemde zaman uyumsuz olarak gönderir. Küme kaynaklarını kullanılabilir olduğu sürece biz ulaşabileceği daha fazla aktarım hızı toplu iş boyutu daha büyük. Öte yandan, hızı ve RU kullanımını sınırlamak için daha küçük bir sayı toplu iş boyutu belirtin. Varsayılan olarak, toplu iş boyutu yazma 500'e ayarlanır.  |
|Upsert   |  UpsertDocument yerine CreateDocument CosmosDB koleksiyonuna yazılırken kullanılıp kullanılmayacağını gösteren bir Boole değeri dize.   |
| WriteThroughputBudget |  Toplam üretilen iş dışında toplu alımı spark işi ayırmak istediğiniz RU\s sayısını temsil eden bir tamsayı dize koleksiyonu için ayrılmış. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Azure Cosmos DB'ye akışa twitter verilerini Yaz 

Bu bölümde, değişiklik akışı twitter verilerini aynı veritabanında yeni bir koleksiyon için akış yazmak için spark sorgular çalıştırın. Not defterini HTML sürümünü barındırılan [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposu. Dosyalarını indirin ve gidin `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html` hesabınıza Azure Databricks not defteri alma, URI ana anahtar, veritabanı, koleksiyon adları hesabı güncelleştirebilir ve çalıştırın veya Not Defteri gibi oluşturabilirsiniz:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin, **Scala** dili olarak açılan daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, twitter veri okuma ve yazma için veritabanı koleksiyonuna bağlanmak için veritabanı ve koleksiyon değerlerini güncelleştirin.

   ```scala
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import com.microsoft.azure.cosmosdb.spark.streaming._

   // Configure connection to Azure Cosmos DB Change Feed (Trades)
   val ConfigMap = Map(
   // Account settings
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   // Change feed settings
   "ReadChangeFeed" -> "true",
   "ChangeFeedStartFromTheBeginning" -> "true",
   "ChangeFeedCheckpointLocation" -> "dbfs:/cosmos-feed",
   "ChangeFeedQueryName" -> "Structured Stream Read",
   "InferStreamSchema" -> "true"
   )
   ```
4. Başlangıç spark.readStream.format() komutunu kullanarak bir akış olarak akış okuma Değiştir:
 
   ```scala
   // Start reading change feed of trades as a stream
   var streamdata = spark
     .readStream
     .format(classOf[CosmosDBSourceProvider].getName)
     .options(ConfigMap)
     .load()
   ```

5. Hedef koleksiyondaki yapılandırmasını tanımlamak ve iş akışında writeStream.format() yöntemi kullanarak başlayın:

   ```scala
   val sinkConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "checkpointLocation" -> "streamingcheckpointlocation6",
   "WritingBatchSize" -> "100",
   "Upsert" -> "true")

   // Start the stream writer
   val streamingQueryWriter = streamdata
    .writeStream
    .format(classOf[CosmosDBSinkProvider].getName)
    .outputMode("append")
    .options(sinkConfigMap)
    .start()
 ```

Java SDK'sı için yapılandırma eşlemesi aşağıdaki değerlerini destekler:

|Ayar  |Açıklama  |
|---------|---------|
|Upsert   |  UpsertDocument yerine CreateDocument CosmosDB koleksiyonuna yazılırken kullanılıp kullanılmayacağını gösteren bir Boole değeri dize.   |
|checkpointlocation  |   Devamlılık belirteçleri düğüm hataları durumunda kalıcı hale getirmek için yerel dosya depolama yolu.   |
|WritingBatchSize  |  Azure Cosmos DB koleksiyonu için verileri yazarken kullanmak için toplu iş boyutu gösterir. Bağlayıcı createDocument/upsertDocument istekleri toplu işlemde zaman uyumsuz olarak gönderir. Küme kaynaklarını kullanılabilir olduğu sürece biz ulaşabileceği daha fazla aktarım hızı toplu iş boyutu daha büyük. Öte yandan, hızı ve RU kullanımını sınırlamak için daha küçük bir sayı toplu iş boyutu belirtin. Varsayılan olarak, toplu iş boyutu yazma 500'e ayarlanır.  |


## <a name="considerations-when-using-java-sdk"></a>Java SDK'sı kullanırken dikkat edilmesi gerekenler

Java SDK'sını kullanarak Azure Cosmos DB'ye bağlanan spark aşağıdaki senaryolarda önerilir:

* Python ve/veya Scala kullanmak istiyorsunuz.  

* Apache Spark ve Azure Cosmos DB arasında aktarmak için veri büyük miktarda sahip, pyDocumentDB için karşılaştırıldığında daha yüksek performans Java SDK'sı yoktur. Sorgu performans farkı hakkında bir fikir vermek için bkz: [sorgu Test çalıştırmalarını wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, Spark Azure Cosmos DB Bağlayıcısı'ndan indirin [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposu ve depodaki ek kaynakları keşfedin:

* [Dağıtılmış toplamalar örnekleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Örnek betikler ve Not Defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

İncelemek isteyebilirsiniz [Apache Spark SQL ve DataFrames veri kümeleri Kılavuzu](http://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure HDInsight üzerinde Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) makalesi.
