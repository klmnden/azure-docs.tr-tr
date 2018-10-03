---
title: Apache Spark, Azure Cosmos DB'ye bağlanma | Microsoft Docs
description: Apache Spark, Azure Cosmos DB'ye bağlama olanak tanıyan Azure Cosmos DB Spark Bağlayıcısı hakkında bilgi edinin. Microsoft'un çok kiracılı, Global olarak dağıtılmış veritabanı sistemdeki dağıtılmış toplamalar gerçekleştirebilirsiniz.
keywords: Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: 4fa28e2d3f5d94d7ab47ec3b1e1e3240e5c770de
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043000"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Azure Cosmos DB Bağlayıcısı için Apache Spark'ı kullanarak büyük veri analizi hızlandırın
 
Apache Spark Bağlayıcısı Azure Cosmos DB için Azure Cosmos DB, bir giriş veya çıkış için Apache Spark işleri olmasını sağlar. Bağlanma [Spark](http://spark.apache.org/) için [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hızlı ilerleyen veri bilimi çözmekte hızlı şekilde. Azure Cosmos DB, hızlı bir şekilde kalıcı hale getirmek ve veri sorgulamak için kullanabilirsiniz. Bağlayıcı, Azure Cosmos DB tarafından yönetilen yerel dizinleri verimli bir şekilde kullanır. Analiz ve karşı hızlı biçimde değişen, küresel olarak dağıtılan veri filtreleme göndererek koşul gerçekleştirdiğinizde güncelleştirilebilir sütunlara dizinler etkinleştirin. Bu türdeki nesnelerin Internet ' (IOT) veri bilimi ve analiz senaryolarına değişebilir.

## <a name="connector-components"></a>Bağlayıcı bileşenleri

Spark Bağlayıcısı Azure Cosmos DB için aşağıdaki bileşenlere sahiptir:

* [Azure Cosmos DB](http://documentdb.com) sağlama ve hem aktarım hızı ve depolama herhangi sayıda coğrafi bölgesinde esnek bir şekilde ölçeklendirmenizi sağlar.  

* [Apache Spark](http://spark.apache.org/) hız, kullanım kolaylığı ve Gelişmiş analiz geçici olarak oluşturulmuş bir güçlü açık kaynak işleme altyapısı.  

* [Azure Databricks üzerinde Apache Spark kümesi](https://docs.azuredatabricks.net/getting-started/index.html) , Spark, Spark kümesinde işleri çalıştırma sağlar.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Apache Spark, Azure Cosmos DB'ye bağlanma

Apache Spark ve Azure Cosmos DB'ye bağlanmak için iki yaklaşım vardır:

- [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python), ayrıca olarak adlandırılan bir Python tabanlı bağlayıcısını *pyDocumentDB*.  

- [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java), Java tabanlı bir bağlayıcı.


**Desteklenen sürümler**

| Bileşen | Sürüm |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x 2.3.x |
| Scala|2.11|
| Azure Databricks çalışma zamanı sürümü | > 3.4 |
| Azure Cosmos DB SQL Java SDK'sı | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Python veya pyDocumentDB SDK'sını kullanarak bağlanma

Aşağıdaki diyagramda, pyDocumentDB SDK'sı uygulama mimarisi gösterilmektedir:

![PyDocumentDB DB üzerinden veri akışını Azure Cosmos DB için Spark'ın diyagramı](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Veri akışı

PyDocumentDB uygulamasının veri akışı aşağıdaki gibidir:

* Ana düğüm Spark'ın Azure Cosmos DB ağ geçidi düğümü pyDocumentDB aracılığıyla bağlanır. Yalnızca Spark ve Azure Cosmos DB bağlantıları belirtirsiniz. Bağlantılar ilgili ana ve ağ geçidi düğümleri için saydamdır.  

* Ağ geçidi düğümü sorguyu daha sonra veri düğümü, koleksiyonun bölümlerine karşı çalıştığı, Azure Cosmos DB sorgusu yapar. Bu sorgulara yanıt ağ geçidi düğüme geri gönderilir ve bu sonuç kümesi için Spark ana düğüm döndürülür.  

* Sonraki sorguları (örneğin, bir Spark veri çerçevesine) işleme için Spark çalışan düğümlerine gönderilir.  

Spark ve Azure Cosmos DB arasındaki iletişim Spark ana düğümü ve Azure Cosmos DB ağ geçidi düğümleri sınırlıdır. Bu iki düğüm arasında Aktarım katmanı sağladığından hızlı sorgular gidin.

Spark pyDocumentDB SDK kullanarak Azure Cosmos DB'ye bağlanmak için aşağıdaki adımları çalıştırın:

1. Oluşturma bir [Azure Databricks çalışma alanı](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) ve [Spark kümesi](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). Azure Databricks çalışma zamanı sürüm 4.0, bu çalışma alanı içinde Apache Spark 2.3.0 ve Scala 2.11 içerir.  

2. Bir küme oluşturulur ve çalışıyor, Git **çalışma** > **Oluştur** > **Kitaplığı**.  
3. Yeni Kitaplık iletişim kutusundan seçin **Python Yumurta karşıya yükleyin veya Pypı** kaynağı olarak. Sağlamak **pydocumentdb** adı ve select **kitaplık yükleme**. bulmak ve yüklemek için SDK'sı pyDocumentdb pip paketleri için zaten yayımlandı. 

   ![Yeni Kitaplık iletişim kutusunun ekran görüntüsü](./media/spark-connector/create-library.png)

4. Kitaplık yüklendikten sonra daha önce oluşturduğunuz kümeye ekleyin.  

5. Git **çalışma** > **oluşturma** > **not defteri**.  

6. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin ve seçin **Python** dili olarak. Aşağı açılan listeden, daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

7. "Doctorwho içinde" Azure Cosmos DB hesabı barındırılan veri uçuşlar kullanarak birkaç Spark sorgu örneği çalıştırın. (Bu hesap, genel olarak erişilebilir.) [Azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposunu barındıran not defterini HTML sürümü. Dosyalarını indirin ve Git `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html`. Azure Databricks hesabınıza not alın ve çalıştırın. Aşağıdaki bölümde ayrıntılı kod blokları işlevselliğini açıklar.

Aşağıdaki kod parçacığı, pyDocumentDB SDK içeri aktarma ve Spark bağlamında sorgu çalıştırma işlemi gösterilmektedir. Kod parçacığında belirtildiği gibi pyDocumentDB SDK Azure Cosmos DB hesabına bağlanmak için gereken bağlantı parametrelerini içerir. Gerekli kitaplıkları alır ve ana anahtarı hem de Azure Cosmos DB istemci (pydocumentdb.document_client) oluşturmak için konak yapılandırır.


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

Ardından, sorguları çalıştırabilirsiniz. Aşağıdaki kod parçacığı doctorwho hesabındaki airports.codes koleksiyonuna bağlanır ve Washington eyaletinde havaalanı şehirlerin ayıklamak için bir sorgu çalıştırır. 

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

Sorgu çalıştırdıktan sonra sonuç "query_iterable. olur. Bir Python listeye dönüştürülen QueryIterable". Bu liste, sırasıyla bir Spark veri çerçevesine dönüştürülür. 

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

### <a name="considerations-when-using-pydocumentdb-sdk"></a>PyDocumentDB SDK kullanmayla ilgili konular
Spark, SDK'sı, aşağıdaki senaryolarda önerilir pyDocumentDB kullanarak Azure Cosmos DB'ye bağlanma:

* Python'ı kullanmak istiyorsunuz.  

* Görece küçük sonuç Azure Cosmos DB için Spark kümesi iade ettiğiniz. Azure Cosmos DB temel alınan veri kümesindeki oldukça büyük olabilir ve koşul filtreleri, Azure Cosmos DB kaynağına karşı çalışan veya filtreler uygulayarak unutmayın.

## <a name="connect-by-using-the-java-sdk"></a>Java SDK'sını kullanarak bağlanma

Aşağıdaki diyagramda Azure Cosmos DB SQL Java SDK'sı uygulama mimarisini gösterir ve Spark çalışan düğümler arasında verileri taşır:

![Azure Cosmos DB Bağlayıcısı için Spark veri akışı diyagramı](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Veri akışı

Java SDK'sı uygulamasının veri akışı aşağıdaki gibidir:

* Spark ana düğüm bölüm eşlemesini almak için Azure Cosmos DB ağ geçidi düğümüne bağlanmış olursunuz. Yalnızca Spark ve Azure Cosmos DB bağlantıları belirtirsiniz. Bağlantılar ilgili ana ve ağ geçidi düğümleri için saydamdır.  

* Bu bilgiler, Spark ana düğüme geri sağlanır. Bu noktada, bölümler ve konumlarına erişmek için gereken bir Azure Cosmos DB belirlemek amacıyla sorgu ayrıştırılamadı görebilmeniz gerekir.  

* Bu bilgiler, Spark çalışan düğümlerine iletilir.  

* Spark çalışan düğümleri için Azure Cosmos DB bölüm ayıklayın ve Spark bölüm çalışan düğümleri olarak verileri döndürmek için doğrudan bağlanın.  

Spark çalışan düğümlerine ve Azure Cosmos DB veri düğümleri (bölümler) arasında veri taşıma olduğu için Spark ve Azure Cosmos DB arasında önemli ölçüde daha hızlı iletişimdir. Bu örnekte, bazı örnek Twitter verilerini Azure Cosmos DB hesabına gönderin ve bu verileri kullanarak Spark sorgular çalıştırın. Azure Cosmos DB için örnek Twitter verilerini yazmak için aşağıdaki adımları kullanın:

1. Oluşturma bir [Azure Cosmos DB SQL API hesabı](create-sql-api-dotnet.md#create-a-database-account) ve [bir koleksiyon Ekle](create-sql-api-dotnet.md#add-a-collection) hesabı.  

2. İndirme [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) github'dan örnek. Azure Cosmos DB için örnek Twitter verilerini yazmak için bu akışı kullanın.  

3. Bir komut istemi açın ve aşağıdaki komutları çalıştırarak modülleri Tweepy ve pyDocumentdb yükleyin:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Twitter akışındaki örnek içeriğini ayıklayın ve config.py dosyasını açın. MasterKey, konak, Databaseıd, CollectionId ve preferredLocations değerlerini güncelleştirin.  

5. Git `http://apps.twitter.com/`ve Twitter uygulama akışındaki kaydedin. Uygulamanız için bir ad seçtikten sonra birlikte sağlanacak bir **tüketici anahtarı, tüketici gizli, erişim belirteci ve erişim belirteci gizli**. Bu değerleri kopyalayın ve sağlamak için config.py dosyasını güncelleştirmeniz Twitter uygulama programlı erişim için Twitter akışı.   

6. Config.py dosyasını kaydedin. Bir komut istemi açın ve aşağıdaki komutu kullanarak Python uygulamasını çalıştırın:

   ```bash
   Python driver.py
   ```

7. Portalda Azure Cosmos DB koleksiyonu gidin ve Twitter verilerini koleksiyona yazıldığından emin olun.

### <a name="find-and-attach-the-java-sdk-to-the-spark-cluster"></a>Bul ve Spark kümesine Java SDK'sı ekleme

1. Oluşturma bir [Azure Databricks çalışma alanı](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) ve [Spark kümesi](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). Azure Databricks çalışma zamanı sürüm 4.0, bu çalışma alanı içinde Apache Spark 2.3.0 ve Scala 2.11 içerir.  

2. Bir küme oluşturulur ve çalışıyor, Git **çalışma** > **Oluştur** > **Kitaplığı**.  

3. Gelen **yeni kitaplık** iletişim kutusunda **Maven koordinatı** kaynağı olarak. Koordinat değeri sağlamak **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**seçip **Kitaplığı Oluştur**. Maven bağımlılıkları çözümlenir ve paket çalışma alanınıza eklenir. Önceki Maven koordinatı biçimde 2.3.0 Spark sürümü temsil eder, 2.11 Scala sürümünü temsil eder ve Azure Cosmos DB Bağlayıcısı sürümü 1.2.0 temsil eder. 

4. Kitaplık yüklendikten sonra daha önce oluşturduğunuz kümeye ekleyin. 

Bu makalede, aşağıdaki senaryolarda Spark Bağlayıcısı Java SDK'sı kullanımı gösterilmektedir:

* Twitter verilerini Azure Cosmos DB'den okuyun.  

* Azure Cosmos DB için akış okuma Twitter verileri.  

* Twitter verilerini Azure Cosmos DB'ye yazılmak. 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Azure Cosmos DB'den twitter verilerini okuyun
 
Bu bölümde, Spark Azure Cosmos DB'den toplu Twitter verilerini okumak için sorgular çalıştırın. [Azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposunu barındıran not defterini HTML sürümü. Dosyalarını indirin ve Git `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html`. Azure Databricks hesabınıza not defteri içeri aktarabilir ve hesabı URI'si, ana anahtar, veritabanı ve koleksiyon adları güncelleştirin. Not Defteri çalıştırmak veya şu şekilde oluşturun:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**. 

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin ve seçin **Python** dili olarak. Aşağı açılan listeden, daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, veritabanı ve hesaba bağlanmak için toplama değerleri güncelleştirin. Spark.read.format() komutunu kullanarak tweet'leri okuma.

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
|query_disableruperminuteusage  |  İstek birimi (RU) devre dışı bırakır, / sorgu normal RU/saniye sağladıysanız sunmak için dakika kapasitesinin dolması.       |
|query_emitverbosetraces   |   Araştırma için ayrıntılı izlemeleri kullanıma yaymak sorgularına izin ver seçeneğine ayarlar.      |
|query_pagesize  |   Her sorgu istek için sorgu sonuç sayfası boyutunu ayarlar. Aktarım hızını iyileştirmek için sonuçları getirilecek gidiş dönüş sayısını azaltmak için büyük sayfa boyutunu kullanın.      |
|query_custom  |  Azure Cosmos DB sorguyu varsayılan sorguyu Azure Cosmos DB'den verileri getirilirken geçersiz kılmak için ayarlar. Bu değer sağladığınızda, bunu yerine bir sorgu ile de doğrulamaları gönderilen kullanıldığını unutmayın.     |

Senaryoya bağlı olarak, aktarım hızı ve performansı iyileştirmek için farklı yapılandırma değerlerini kullanmalısınız. Yapılandırma anahtarı şu anda duyarsızdır, ve yapılandırma değeri her zaman bir dize olduğunu unutmayın.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Azure Cosmos DB için akış okuma Twitter veri

Bu bölümde, Spark bir değişiklik akışı, akış Twitter verilerini okumak için sorgular çalıştırın. Bu bölümde sorguları çalıştırırken, Twitter akışınızı uygulaması çalıştıran ve Azure Cosmos DB'ye veri Pompalama emin olun. [Azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposunu barındıran not defterini HTML sürümü. Dosyalarını indirin ve Git `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html`. Azure Databricks hesabınıza not defteri içeri aktarabilir ve hesabı URI'si, ana anahtar, veritabanı ve koleksiyon adları güncelleştirin. Not Defteri çalıştırmak veya şu şekilde oluşturun:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin ve seçin **Scala** dili olarak. Aşağı açılan listeden, daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, veritabanı ve hesaba bağlanmak için toplama değerleri güncelleştirin.

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
4. Başlangıç okumak akışı bir akış olarak spark.readStream.format() komutunu kullanarak:

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Sorgu Konsolu için akış başlatın:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

Java SDK'sı için yapılandırma eşlemesi aşağıdaki değerlerini destekler:

|Ayar  |Açıklama  |
|---------|---------|
|readchangefeed   |  Koleksiyon içeriğini CosmosDB değişiklik akışı ' alınan gösterir. Varsayılan değer false'tur.       |
|changefeedqueryname |   Sorgu tanımlamak için özel bir dize. Bağlayıcı, farklı değişiklik sorguları ayrı olarak akışı için koleksiyon devamlılık belirteci izler. Readchangefeed true ise, boş değer alamaz gerekli bir yapılandırma budur.      |
|changefeedcheckpointlocation  |   Devamlılık belirteçleri düğüm hataları durumunda kalıcı hale getirmek için yerel dosya depolama yolu.      |
|changefeedstartfromthebeginning  |  Değişiklik akışı başlangıçtan itibaren (true) veya geçerli noktasından (false) başlamalıdır olup olmadığını belirler. Varsayılan olarak, geçerli (false) başlar.       |
|rollingchangefeed  |   Değişiklik akışı olmadığını gösteren bir Boole değeri son sorgudan olmalıdır. Varsayılan değer false, değişiklikleri koleksiyonun ilk okuması sayılan anlamına gelir.      |
|changefeedusenexttoken  |   İşleme hatası senaryoları desteklemek için bir Boole değeri. Bu, geçerli değişiklik toplu iş akışı düzgün bir şekilde işlendiğini gösterir. Dağıtılmış Resiliant Dataset sonraki toplu değişiklikler almak için sonraki devamlılık belirteçleri kullanmanız gerekir.      |
| InferStreamSchema | Akış veri şeması akış başlangıcında örneğinin alınıp alınmayacağını belirten bir Boole değeri. Varsayılan olarak, bu değer ayarlanır true. Veriler örneklenir sonra bu parametreyi akış verilerinin şema değişiklikleri ve true olarak ayarlanırsa, yeni eklenen özellikler akış veri çerçevesinde bırakılır. <br/><br/> Şemadan olmasını akış veri çerçevesi istiyorsanız bu parametreyi false olarak ayarlayın. Bu modda, Azure Cosmos DB değişiklik akışı'ndan okuma belge gövdesinin bir gövde özellikte sarılır. Bu özellik, çerçevede sonuç akış veri, sistem özellik değerleri tarafından kullanılır.
 |

### <a name="connection-settings"></a>Bağlantı ayarları

Java SDK'sı aşağıdaki bağlantı ayarları destekler:

|Ayar  |Açıklama  |
|---------|---------|
|connectionmode   |  İç DocumentClient Azure Cosmos DB ile iletişim kurmak için kullanması gereken bağlantı modunu ayarlar. İzin verilen değerler **DirectHttps** (varsayılan değer) ve **ağ geçidi**. DirectHttps bağlantı modu, istekleri doğrudan CosmosDB bölümlere yönlendirir ve bazı gecikme avantajı sağlar.       |
|connectionmaxpoolsize   |  İç DocumentClient tarafından kullanılan bağlantı havuzu boyutu değerini ayarlar. Varsayılan değer 100’dür.       |
|connectionidletimeout  |  Boşta kalan bağlantıların için saniye cinsinden zaman aşımı değerini ayarlar. Varsayılan değer 60 ' dir.       |
|query_maxretryattemptsonthrottledrequests    |  En fazla yeniden deneme sayısını ayarlar. Bir istemci üzerinde hız sınırı nedeniyle istek başarısız olması durumunda bu değeri kullanın. Belirtilmezse, varsayılan değer 1000 yeniden deneme girişimleri ' dir.       |
|query_maxretrywaittimeinseconds   |  Ayarlar, en fazla'süresini saniye cinsinden yeniden deneyin. Varsayılan olarak, 1000 saniyedir.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Twitter verilerini Azure Cosmos DB'ye yazılmak 

Bu bölümde, Spark aynı veritabanında yeni bir koleksiyon için Twitter verilerini toplu yazmak için sorgular çalıştırın. [Azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposunu barındıran not defterini HTML sürümü. Dosyalarını indirin ve Git `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html`. Azure Databricks hesabınıza not defteri içeri aktarabilir ve hesabı URI'si, ana anahtar, veritabanı ve koleksiyon adları güncelleştirin. Not Defteri çalıştırmak veya şu şekilde oluşturun:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin ve seçin **Scala** dili olarak. Aşağı açılan listeden, daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, veritabanı ve Twitter veri okuma ve yazma için veritabanı koleksiyonuna bağlanmak için toplama değerleri güncelleştirin.

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

5. Tweetleri etiketlerin yeni bir veri çerçevesi oluşturun ve yeni koleksiyon verileri kaydedin. Aşağıdaki kod çalıştırdıktan sonra portala geri dönün ve veri koleksiyonuna yazıldığından emin olun. 

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
|WritingBatchSize  |   Azure Cosmos DB koleksiyonu için verileri yazarken kullanmak için toplu iş boyutu gösterir. <br/><br/> Toplu iş boyutu ' % s'importAll API Bulkexecutor'a kitaplığının giriş olarak sağlanan belgelerin WritingBatchSize parametresi gösterir BulkImport parametresi true olarak ayarlanırsa. Varsayılan olarak, bu değer için 100 bin cinsinden ayarlanır. <br/><br/> BulkImport parametresini false olarak ayarlarsanız, WritingBatchSize parametresi için Azure Cosmos DB koleksiyonu yazarken kullanmak için toplu iş boyutu gösterir. Bağlayıcı createDocument ve upsertDocument istekleri toplu işlemde zaman uyumsuz olarak gönderir. Daha büyük toplu iş boyutu, daha fazla aktarım hızı küme kaynakları mevcut olduğu sürece, elde edebilirsiniz. Öte yandan, hızı ve RU kullanımını sınırlamak için daha küçük bir sayı toplu iş boyutu belirtin. Varsayılan olarak, toplu iş boyutu yazma 500'e ayarlanır.  |
|Upsert   |  Azure Cosmos DB koleksiyonu için yazarken upsertDocument CreateDocument yerine kullanılıp kullanılmayacağını gösteren bir Boole değeri dize.   |
| WriteThroughputBudget |  Toplam üretilen iş dışında toplu alımı Spark işi ayırmak istediğiniz RU\s sayısını temsil eden bir tamsayı dize koleksiyonu için ayrılmış. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Azure Cosmos DB'ye akışa Twitter verilerini Yaz 

Bu bölümde, Spark akış Twitter verilerini aynı veritabanında yeni bir koleksiyon için bir değişiklik akış yazmak için sorgular çalıştırın. [Azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub deposunu barındıran not defterini HTML sürümü. Dosyalarını indirin ve Git `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html`. Azure Databricks hesabınıza not defteri içeri aktarabilir ve hesabı URI'si, ana anahtar, veritabanı ve koleksiyon adları güncelleştirin. Not Defteri çalıştırmak veya şu şekilde oluşturun:

1. Azure Databricks hesabınıza gidin ve seçin **çalışma** > **Oluştur** > **not defteri**.  

2. İçinde **Not Defteri Oluştur** iletişim kutusunda, kullanıcı dostu bir ad girin ve seçin **Scala** dili olarak. Aşağı açılan listeden, daha önce oluşturduğunuz kümeyi seçip **Oluştur**.  

3. Uç nokta, ana anahtar, veritabanı ve Twitter veri okuma ve yazma için veritabanı koleksiyonuna bağlanmak için toplama değerleri güncelleştirin.

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
4. Başlangıç okumak akışı bir akış olarak spark.readStream.format() komutunu kullanarak:
 
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
|Upsert   |  Azure Cosmos DB koleksiyonu için yazarken upsertDocument CreateDocument yerine kullanılıp kullanılmayacağını gösteren bir Boole değeri dize.   |
|checkpointlocation  |   Devamlılık belirteçleri düğüm hataları durumunda kalıcı hale getirmek için yerel dosya depolama yolu.   |
|WritingBatchSize  |  Azure Cosmos DB koleksiyonu için verileri yazarken kullanmak için toplu iş boyutu gösterir. Bağlayıcı createDocument ve upsertDocument istekleri toplu işlemde zaman uyumsuz olarak gönderir. Daha büyük toplu iş boyutu, daha fazla aktarım hızı küme kaynakları mevcut olduğu sürece, elde edebilirsiniz. Öte yandan, hızı ve RU kullanımını sınırlamak için daha küçük bir sayı toplu iş boyutu belirtin. Varsayılan olarak, toplu iş boyutu yazma 500'e ayarlanır.  |


### <a name="considerations-when-using-java-sdk"></a>Java SDK'sı kullanırken dikkat edilmesi gerekenler

Java SDK'sını kullanarak Azure Cosmos DB için Spark bağlanma, aşağıdaki senaryolarda önerilir:

* Python veya Scala kullanmak istiyorsunuz.  

* Büyük miktarda bir Apache Spark ve Azure Cosmos DB arasında aktarmak için veri var. Java SDK'sı pyDocumentDB daha iyi gerçekleştirir. Sorgu performansı fark hakkında daha fazla bilgi için bkz. [sorgu çalıştırmalar wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, Spark Azure Cosmos DB Bağlayıcısı'ndan indirin [azure cosmosdb spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposu. Aşağıdaki ek kaynaklara depodaki keşfedin:

* [Toplamalar örnekleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Örnek betikler ve Not Defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

İncelemek isteyebilirsiniz [Apache Spark SQL ve DataFrames veri kümeleri Kılavuzu](http://spark.apache.org/docs/latest/sql-programming-guide.html)ve [Azure HDInsight üzerinde Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) makalesi.
