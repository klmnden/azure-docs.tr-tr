---
title: Apache Spark - Azure olay hub'ları ile tümleştirme | Microsoft Docs
description: Event Hubs ile yapılandırılmış akış'ı etkinleştirmek için Apache Spark ile tümleştirin
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 605669a740663040ab7a167bf266fe1940123afc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60343401"
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Apache Spark Azure Event Hubs ile tümleştirme

Azure Event Hubs ile sorunsuz bir şekilde tümleştirilir [Apache Spark](https://spark.apache.org/) dağıtılmış akış uygulamalar oluşturma olanağı. Bu tümleştirmeyi desteklemektedir [Spark Core](https://spark.apache.org/docs/latest/rdd-programming-guide.html), [Spark akışı](https://spark.apache.org/docs/latest/streaming-programming-guide.html), ve [yapılandırılmış akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). Apache Spark için Event Hubs bağlayıcısını kullanılabilir [GitHub](https://github.com/Azure/azure-event-hubs-spark). Bu kitaplık ayrıca içindeki Maven projelerinde kullanılabilir [Maven Central Repository](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C).

Bu makalede, sürekli bir uygulamanın nasıl oluşturulacağını [Azure Databricks](https://azure.microsoft.com/services/databricks/). Bu makalede, Azure Databricks kullanırken, Spark kümeleri de kullanılabilen [HDInsight](../hdinsight/spark/apache-spark-overview.md).

Bu makalede bu örnek iki Scala not defterleri kullanır: biri olayları bir event hub'ı ve olayların yeniden gönderilmesi için başka bir akış.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Biri yoksa [ücretsiz bir hesap oluşturma](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Bir olay hub'ı örneği. Biri yoksa [oluşturmak](event-hubs-create.md).
* Bir [Azure Databricks](https://azure.microsoft.com/services/databricks/) örneği. Biri yoksa [oluşturmak](../azure-databricks/quickstart-create-databricks-workspace-portal.md).
* [Maven koordinatları kullanarak bir kitaplığı oluşturma](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package): `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.1`.

Aşağıdaki kodu kullanarak, olay hub'ından Stream olayları:

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
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
Aşağıdaki kod, Spark batch API'leri ile olay hub'ınıza olayları gönderir. Ayrıca, olay hub'ına olayları göndermek için bir akış sorgu yazabilirsiniz:

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build
val ehConf = EventHubsConf(connectionString)

// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(bodyColumn)
df.write
  .format("eventhubs")
  .options(ehConf.toMap)
  .save() 
```

## <a name="next-steps"></a>Sonraki adımlar

Apache Spark için Event Hubs bağlayıcısını kullanarak ölçeklenebilir, hataya dayanıklı bir akış ayarlamak nasıl biliyorsunuz. Bu bağlantıları takip ederek yapılandırılmış akış ve Spark akışı ile Event hubs'ı kullanma hakkında daha fazla bilgi edinin:

* [Yapılandırılmış akış + Azure Event Hubs tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Spark akış + Event Hubs tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
