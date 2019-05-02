---
title: Azure Cosmos DB Cassandra API'sine spark'tan ile çalışma
description: Bu makale, Cosmos DB Cassandra API'sine tümleştirme spark'tan ana sayfasıdır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 75d2930363b6ad1aeace22d7529df04f31deefe5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60893652"
---
# <a name="connect-to-azure-cosmos-db-cassandra-api-from-spark"></a>Spark'tan Azure Cosmos DB Cassandra API'sine bağlanma

Bu makalede bir dizi makale Azure Cosmos DB Cassandra API'sine tümleştirme spark'tan arasında biridir. Makalelerin bağlantısı, veri tanımı Language(DDL) işlemleri, temel veri işleme Language(DML) işlemleri ve Gelişmiş Azure Cosmos DB Cassandra API'sine tümleştirme spark'tan kapsar. 

## <a name="prerequisites"></a>Önkoşullar
* [Bir Azure Cosmos DB Cassandra API hesabı sağlayın.](create-cassandra-dotnet.md#create-a-database-account)

* Spark ortam tercih ettiğiniz sağlama [[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) | [Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql) | [Diğer].

## <a name="dependencies-for-connectivity"></a>Bağlantı için bağımlılıklar
* **Cassandra için Spark Bağlayıcısı:** Spark Bağlayıcısı, Azure Cosmos DB Cassandra API'sine bağlanmak için kullanılır.  Tanımlamak ve bulunan bağlayıcı sürümünü kullanmanız [Maven central]( https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) Spark ortamınızı Spark ve Scala sürümleriyle uyumlu.

* **Cassandra API'si için Azure Cosmos DB yardımcı kitaplık:** Spark Bağlayıcısı ek olarak, adlı başka bir kitaplığı ihtiyacınız [azure-cosmos-cassandra-spark-Yardımcısı]( https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) Azure Cosmos DB'den. Bu kitaplık, özel bağlantı üretecini ve yeniden deneme ilkesi sınıfları içerir.

  Azure Cosmos DB'de yeniden deneme ilkesi, HTTP durum kodu 429 ("istek oranı büyük") özel durumları işlemek için yapılandırılır. Azure Cosmos DB Cassandra API'SİNİN bu özel durumları aşırı yüklenmiş hatalara Cassandra yerel protokolüne çeviren ve arka istenmesi ile yeniden deneyebilirsiniz. Azure Cosmos DB, sağlanan aktarım hızı modeli kullandığından, giriş/çıkış artış derecelendirir istek hızı sınırlama özel durumları ortaya çıkar. Yeniden deneme ilkesi, spark işleri kısa bir süre içinde koleksiyonunuz için ayrılan aktarım hızını aşmayı veri ani karşı korur.

  > [!NOTE] 
  > Yeniden deneme ilkesi, spark işlerinde yalnızca kısa süreli ani karşı koruyabilirsiniz. İş yükünüz çalıştırmak için gereken yeterli RU yapılandırmadıysanız, yeniden deneme ilkesi geçerli değil ve yeniden deneme ilkesi sınıfı özel durumu yeniden oluşturur.

* **Azure Cosmos DB hesabı bağlantısı ayrıntıları:** Azure Cassandra API hesap adınızı, hesabınızın uç noktası ve anahtar.
    
## <a name="spark-connector-throughput-configuration-parameters"></a>Spark Bağlayıcısı aktarım hızı yapılandırma parametreleri

Aşağıdaki tablo, Azure Cosmos DB Cassandra API'si-özel bağlayıcı tarafından sağlanan aktarım hızı yapılandırma parametreleri listeler. Tüm yapılandırma parametreleri ayrıntılı bir listesi için bkz. [yapılandırma başvurusu](https://github.com/datastax/spark-cassandra-connector/blob/master/doc/reference.md) Spark Cassandra bağlayıcı GitHub deposunun sayfası.

| **Özellik adı** | **Varsayılan değer** | **Açıklama** |
|---------|---------|---------|
| spark.cassandra.output.batch.size.rows |  1 |Tek bir toplu iş başına satır sayısı. Bu parametre, 1 olarak ayarlayın. Bu parametre, ağır iş yükleri için daha yüksek performans sağlamak için kullanılır. |
| spark.cassandra.connection.connections_per_executor_max  | None | Düğüm başına Yürütücü bağlantılarının maksimum sayısı. 10 * n, n düğümlü bir Cassandra kümesi düğümü başına 10 bağlantı eşdeğerdir. 5 düğümlü Cassandra kümesi için düğüm başına Yürütücü 5 bağlantıları gerektiriyorsa, bu nedenle, daha sonra bu yapılandırma için 25 ayarlamanız gerekir. Paralellik derecesini veya spark işleriniz için yapılandırılmış olan Yürütücü sayısına göre bu değeri değiştirin.   |
| Spark.cassandra.output.Concurrent.Writes  |  100 | Yürütücü oluşabilecek paralel yazma sayısını tanımlar. "Batch.size.rows" 1 olarak ayarlandığından, bu değeri uygun şekilde ölçeklendirir emin olun. Paralellik veya iş yükünüz için elde etmek istediğiniz işleme göre bu değeri değiştirin. |
| Spark.cassandra.Concurrent.Reads |  512 | Yürütücü oluşabilecek paralel okuma sayısını tanımlar. Paralellik veya iş yükünüz için elde etmek istediğiniz işleme göre bu değeri değiştirmek  |
| spark.cassandra.output.throughput_mb_per_sec  | None | Toplam yazma başına aktarım hızı Yürütücü tanımlar. Bu parametre bir üst sınırlamak için spark işi aktarım hızınızı ve Cosmos DB koleksiyonunuza sağlanan aktarım hızı temel olarak kullanılabilir.   |
| spark.cassandra.input.reads_per_sec| None   | Yürütücü başına toplam okuma aktarım hızı tanımlar. Bu parametre bir üst sınırlamak için spark işi aktarım hızınızı ve Cosmos DB koleksiyonunuza sağlanan aktarım hızı temel olarak kullanılabilir.  |
| Spark.cassandra.output.Batch.Grouping.Buffer.size |  1000  | Cassandra API'sine göndermeden önce belleğinde depolanabilir tek spark görev başına yığınların sayısını tanımlar |
| spark.cassandra.connection.keep_alive_ms | 60000 | Kullanılmayan bağlantıları kullanılabilir süreyi tanımlar. | 

Aktarım hızı ve spark işleriniz için beklediğiniz iş yükü ve Cosmos DB hesabınız için sağladığınız aktarım hızı temelinde Bu parametre paralellik derecesini ayarlayın.

## <a name="connecting-to-azure-cosmos-db-cassandra-api-from-spark"></a>Spark'tan Azure Cosmos DB Cassandra API'sine bağlanma

### <a name="cqlsh"></a>cqlsh
Aşağıdaki komutları cqlsh Azure CosmosDB Cassandra API'sine bağlanma ayrıntılı olarak açıklanmaktadır.  Bu örnekler, Spark üzerinde yazarken doğrulama için kullanışlıdır.<br>
**Linux/UNIX/Mac'ten:**

```bash
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false
cqlsh.py YOUR-COSMOSDB-ACCOUNT-NAME.cassandra.cosmosdb.azure.com 10350 -u YOUR-COSMOSDB-ACCOUNT-NAME -p YOUR-COSMOSDB-ACCOUNT-KEY --ssl
```

### <a name="1--azure-databricks"></a>1.  Azure Databricks
Azure Databricks kümesini sağlama, küme yapılandırması, Azure Cosmos DB Cassandra API'SİNİN ve DDL işlemleri, DML işlemleri ve daha fazlasını kapsayan birkaç örnek not defterleri için bağlanmak için aşağıdaki makaleye kapsar.<BR>
[Azure databricks'ten Azure Cosmos DB Cassandra API'si ile çalışma](cassandra-spark-databricks.md)<BR>
  
### <a name="2--azure-hdinsight-spark"></a>2.  Azure HDInsight Spark
Aşağıdaki makalede Hdınsight Spark hizmeti, sağlama, bağlama Azure Cosmos DB Cassandra API'SİNİN ve DDL işlemleri, DML işlemleri ve daha fazlasını kapsayan birkaç örnek not defterleri için küme yapılandırması içerir.<BR>
[Azure HDInsight Spark gelen Azure Cosmos DB Cassandra API'si ile çalışma](cassandra-spark-hdinsight.md)
 
### <a name="3--spark-environment-in-general"></a>3.  Genel ortam spark
Yukarıdaki bölümlerde Azure Spark tabanlı PaaS hizmetlerine belirli olmakla birlikte, bu bölüm, herhangi genel bir Spark ortam kapsar.  Bağlayıcı bağımlılıklar, içeri aktarmalar ve Spark oturum yapılandırması aşağıda açıklanmıştır. "Sonraki adımlar" bölümü, kod örnekleri DDL işlemleri, DML işlemleri ve daha fazlasını kapsamaktadır.  

#### <a name="connector-dependencies"></a>Bağlayıcı bağımlılıkları:

1. Maven koordinatları alınacak ekleme [Cassandra Spark Bağlayıcısı](cassandra-spark-generic.md#dependencies-for-connectivity)
2. Ekleme için maven koordinatları [Azure Cosmos DB yardımcı kitaplık](cassandra-spark-generic.md#dependencies-for-connectivity) Cassandra API'si

#### <a name="imports"></a>İçeri aktarmalar:

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra
```

#### <a name="spark-session-configuration"></a>Spark oturum yapılandırması:

```scala
//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related. You can adjust the values as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler Azure Cosmos DB Cassandra API ile Spark tümleştirme gösterir. 
 
* [DDL işlemleri](cassandra-spark-ddl-ops.md)
* [İşlem oluşturma/Ekle](cassandra-spark-create-ops.md)
* [okuma işlemleri](cassandra-spark-read-ops.md)
* [Upsert işlem](cassandra-spark-upsert-ops.md)
* [Silme işlemleri](cassandra-spark-delete-ops.md)
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)
