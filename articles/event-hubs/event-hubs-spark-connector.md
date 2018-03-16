---
title: "Apache Spark Azure Event Hubs ile tümleştirme | Microsoft Docs"
description: "Event Hubs ile yapılandırılmış akışını etkinleştirmek için Apache Spark ile tümleştirme"
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2018
ms.author: shvija;sethm
ms.openlocfilehash: 3b44c7e7387eceb2d9ea0b2c214b13a82869110f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Apache Spark Azure Event Hubs ile tümleştirme

Azure Event Hubs ile sorunsuz şekilde tümleşir [Apache Spark](https://spark.apache.org/) akış uygulamaları kolayca yapı uçtan uca yapmak için Dağıtılmış. Bu tümleştirmeyi destekler [Spark Core](http://spark.apache.org/docs/latest/rdd-programming-guide.html), [Spark akış](http://spark.apache.org/docs/latest/streaming-programming-guide.html), [yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Olay hub'ları bağlayıcı Apache Spark için kullanılabilir [GitHub](https://github.com/Azure/azure-event-hubs-spark). Bu kitaplığı da Maven projelerden kullanmak için kullanılabilir [Maven merkezi bir depoya](http://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

Bu makalede, sürekli bir uygulamada nasıl oluşturulacağı gösterilmektedir [Azure Databrick](https://azure.microsoft.com/services/databricks/). Bu makalede kullanırken [Azure Databricks](https://azure.microsoft.com/services/databricks/), Spark kümeleri de ile kullanılabilir [Hdınsight](../hdinsight/spark/apache-spark-overview.md).

Aşağıdaki örnek, iki Scala not defterlerini kullanır: biri olayları bir event hub ve olayları yeniden göndermek için başka bir akış.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Biri, yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Bir olay hub'ları örneği. Biri, yoksa [bir tane oluşturun](event-hubs-create.md)
* Bir [Azure Databricks](https://azure.microsoft.com/services/databricks/) örneği. Biri, yoksa [bir tane oluşturun](../azure-databricks/quickstart-create-databricks-workspace-portal.md)
* [Maven koordinat kullanarak bir kitaplığı oluşturma](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.0`

Aşağıdaki kod defterinizde olay hub'ından akış olaylar için kullanın.

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()

// Select the body column and cast it to a string.
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
Aşağıdaki kod örneği, olay hub'ınıza Spark batch API'leri için olaylar gönderir. Ayrıca, olay hub'ına olayları göndermek için bir akış sorgu yazabilirsiniz.

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build

val eventHubsConf = EventHubsConf(connectionString)

// Create a column representing the partitionKey.
val partitionKeyColumn = (col("id") % 5).cast("string").as("partitionKey")
// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(partitionKeyColumn, bodyColumn)
df.write
  .format("eventhubs")
  .options(eventHubsConf.toMap)
  .save() 

```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, olay hub'ları bağlayıcı gerçek zamanlı, hataya dayanıklı akış çözümleri oluşturma için nasıl çalıştığını gösterdi. Bu bağlantıları takip ederek yapılandırılmış akış ve Event Hubs tümleşik Bağlayıcısı hakkında daha fazla bilgi:

* [Yapılandırılmış akış + Azure olay hub'ları tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Spark akış + olay hub'ları tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
