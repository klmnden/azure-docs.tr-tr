---
title: Apache Kafka - Azure HDInsight için Azure izleme günlükleri
description: Azure HDInsight üzerinde Apache Kafka kümesi günlükleri analiz etmek için Azure İzleyici günlüklerine kullanmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.openlocfilehash: 5154058bb1562b670e24f8813e979588a536f086
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58359866"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için günlüklerini çözümleme

HDInsight üzerinde Apache Kafka tarafından oluşturulan günlükleri analiz etmek için Azure İzleyici günlüklerine kullanmayı öğrenin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Azure İzleyici günlüklerine için Apache kafka'yı etkinleştir

HDInsight için Azure İzleyici günlüklerine etkinleştirme adımları tüm HDInsight kümeleri için aynıdır. Gerekli hizmetlerini oluşturup yapılandırın anlamak için aşağıdaki bağlantıları kullanın:

1. Bir Log Analytics çalışma alanı oluşturun. Daha fazla bilgi için [bir Log Analytics çalışma alanını kullanmaya başlama](https://docs.microsoft.com/azure/log-analytics) belge.

2. HDInsight kümesinde Kafka oluşturmak. Daha fazla bilgi için [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge.

3. Kafka kümesi, Azure İzleyici günlüklerine kullanmak için yapılandırın. Daha fazla bilgi için [kullanımı Azure İzleyici günlükleri HDInsight izlemek için](../hdinsight-hadoop-oms-log-analytics-tutorial.md) belge.

    > [!NOTE]  
    > Azure İzleyici günlüklerine kullanarak kümeye de yapılandırabilirsiniz `Enable-AzHDInsightOperationsManagementSuite` cmdlet'i. Bu cmdlet, aşağıdaki bilgileri gerektirir:
    >
    > * HDInsight kümesi adı.
    > * Azure İzleyici günlüklerine çalışma alanı kimliği. Çalışma alanı kimliği, Log Analytics çalışma alanında bulabilirsiniz.
    > * Log analytics bağlantısını için birincil anahtarı. Birincil anahtar, açık, Azure portalında çalışma alanını bulmak için seçin __Gelişmiş ayarlar__ sol menüden. Gelişmiş ayarları seçin __bağlı kaynaklar__>__Linux sunucuları__.


> [!IMPORTANT]  
> Bu, veri için Azure İzleyici günlüklerine kullanılabilir olmadan önce yaklaşık 20 dakika sürebilir.

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

Azure İzleyici hakkında daha fazla bilgi için bkz. [Azure İzleyiciye Genel Bakış](../../log-analytics/log-analytics-get-started.md) belge.

Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

 * [HDInsight kümeleri arasında yansıtmayı Apache Kafka](apache-kafka-mirroring.md)
 * [HDInsight üzerinde Apache Kafka'nın ölçeklenebilirliğini artırma](apache-kafka-scalability.md)
 * [Apache Spark ile Apache Kafka (DStreams) akış kullanma](../hdinsight-apache-spark-with-kafka.md)
 * [Apache Spark kullanma Apache Kafka ile structured streaming](../hdinsight-apache-kafka-spark-structured-streaming.md)
