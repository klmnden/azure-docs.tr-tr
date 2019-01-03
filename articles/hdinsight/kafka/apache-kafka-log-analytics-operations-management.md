---
title: Apache Kafka - Azure HDInsight için log Analytics
description: Log Analytics, Azure HDInsight üzerinde Apache Kafka kümesi günlükleri analiz etmek için kullanmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 69eaa0028f1115cafbd1ed28b66940d7faaed062
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53608554"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için günlüklerini çözümleme

HDInsight üzerinde Apache Kafka tarafından oluşturulan günlükleri analiz etmek için log Analytics kullanmayı öğrenin.

## <a name="enable-log-analytics-for-apache-kafka"></a>Apache Kafka için log Analytics etkinleştir

HDInsight için log Analytics etkinleştirme adımları tüm HDInsight kümeleri için aynıdır. Gerekli hizmetlerini oluşturup yapılandırın anlamak için aşağıdaki bağlantıları kullanın:

1. Bir Log Analytics çalışma alanı oluşturun. Daha fazla bilgi için [bir Log Analytics çalışma alanını kullanmaya başlama](https://docs.microsoft.com/azure/log-analytics) belge.

2. HDInsight kümesinde Kafka oluşturmak. Daha fazla bilgi için [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge.

3. Log Analytics'i kullanmak için Kafka kümesi yapılandırın. Daha fazla bilgi için [HDInsight izlemek için Log Analytics'i kullanmak](../hdinsight-hadoop-oms-log-analytics-tutorial.md) belge.

    > [!NOTE]  
    > Log Analytics kullanarak kümeye de yapılandırabilirsiniz `Enable-AzureRmHDInsightOperationsManagementSuite` cmdlet'i. Bu cmdlet, aşağıdaki bilgileri gerektirir:
    >
    > * HDInsight kümesi adı.
    > * Log Analytics çalışma alanı kimliği. Çalışma alanı kimliği, Log Analytics çalışma alanında bulabilirsiniz.
    > * Log Analytics bağlantısı için birincil anahtar. Birincil anahtar, açık, Azure portalında çalışma alanını bulmak için seçin __Gelişmiş ayarlar__ sol menüden. Gelişmiş ayarları seçin __bağlı kaynaklar__>__Linux sunucuları__.


> [!IMPORTANT]  
> Bu veriler, Log Analytics için kullanılabilir olmadan önce yaklaşık 20 dakika sürebilir.

## <a name="query-logs"></a>Sorgu günlükleri

1. Gelen [Azure portalında](https://portal.azure.com), Log Analytics çalışma alanınızı seçin.

2. Seçin __günlük arama__. Buradan, Kafka'dan toplanan verileri arayabilirsiniz. Bazı örnek aramalar şunlardır:

    * Disk kullanımı: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * CPU kullanımı: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Saniye başına gelen iletileri: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Saniye başına gelen bayt sayısı: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Saniye başına Giden bayt sayısı: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]  
    > Sorgu değerleri kümeye özel bilgileri ile değiştirin. Örneğin, `ClusterName_s` kümenizin adı olarak ayarlanması gerekir. `HostName_s` Kümedeki çalışan düğümü etki alanı adı olarak ayarlanmalıdır.

    Ayrıca girebilirsiniz `*` kaydedilen tüm türleri aranacak. Şu anda aşağıdaki günlükleri sorgular için kullanılabilir:

    | Günlük türü | Açıklama |
    | ---- | ---- |
    | Günlük\_kafkaserver\_Temizle | Kafka Aracısı server.log |
    | Günlük\_kafkacontroller\_Temizle | Kafka Aracısı controller.log |
    | Ölçümleri\_kafka\_Temizle | Kafka JMX ölçümlerini |

    ![CPU kullanım arama görüntüsü](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Sonraki adımlar

Log Analytics hakkında daha fazla bilgi için bkz. [bir Log Analytics çalışma alanını kullanmaya başlama](../../log-analytics/log-analytics-get-started.md) belge.

Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

 * [HDInsight kümeleri arasında yansıtmayı Apache Kafka](apache-kafka-mirroring.md)
 * [HDInsight üzerinde Apache Kafka'nın ölçeklenebilirliğini artırma](apache-kafka-scalability.md)
 * [Apache Spark ile Apache Kafka (DStreams) akış kullanma](../hdinsight-apache-spark-with-kafka.md)
 * [Apache Spark kullanma Apache Kafka ile structured streaming](../hdinsight-apache-kafka-spark-structured-streaming.md)
