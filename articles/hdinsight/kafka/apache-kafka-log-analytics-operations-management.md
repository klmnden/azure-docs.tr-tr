---
title: Günlük analizi Apache Kafka - Azure Hdınsight | Microsoft Docs
description: Azure hdınsight'ta Apache Kafka kümeden günlüklerini çözümlemek için günlük analizi kullanmayı öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2018
ms.author: larryfr
ms.openlocfilehash: 9f366631ced4392831ad9ed97898a88b3290cd22
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>Hdınsight üzerinde Apache Kafka günlüklerini analiz edin

Hdınsight üzerinde Apache Kafka tarafından oluşturulan günlüklerini çözümlemek için günlük analizi kullanmayı öğrenin.

## <a name="enable-log-analytics-for-kafka"></a>Günlük analizi için Kafka etkinleştir

Hdınsight için günlük analizi etkinleştirme adımları tüm Hdınsight kümeleri için aynıdır. Oluşturma ve gerekli hizmetleri yapılandırma anlamak için aşağıdaki bağlantıları kullanın:

1. Günlük analizi çalışma alanı oluşturun. Daha fazla bilgi için bkz: [günlük analizi çalışma alanı ile çalışmaya başlama](https://docs.microsoft.com/azure/log-analytics) belge.

2. Hdınsight kümesinde bir Kafka oluşturun. Daha fazla bilgi için bkz: [Hdınsight üzerinde Apache Kafka başlayarak](apache-kafka-get-started.md) belge.

3. Günlük analizi kullanmak için Kafka küme yapılandırın. Daha fazla bilgi için bkz: [Hdınsight izlemek için kullanım günlük analizi](../hdinsight-hadoop-oms-log-analytics-tutorial.md) belge.

    > [!NOTE]
    > Günlük analizi kullanarak kümeye de yapılandırabilirsiniz `Enable-AzureRmHDInsightOperationsManagementSuite` cmdlet'i. Bu cmdlet, aşağıdaki bilgileri gerektirir:
    >
    > * Hdınsight küme adı.
    > * Günlük analizi çalışma alanı kimliği. Günlük analizi çalışma alanınızda çalışma alanı kimliği bulabilirsiniz.
    > * Günlük analizi bağlantının birincil anahtarı. Birincil anahtarı bulmak için günlük analizi örneğinizi seçin ve ardından __OMS portalı__. OMS Portalı'ndan seçin __ayarları__, __bağlı kaynakları__ve ardından __Linux sunucuları__.


> [!IMPORTANT]
> Veri günlük analizi için kullanılabilir olmadan önce yaklaşık 20 dakika sürebilir.

## <a name="query-logs"></a>Sorgu günlükleri

1. Gelen [Azure portal](https://portal.azure.com), günlük analizi çalışma alanınızı seçin.

2. Seçin __oturum arama__. Buradan, Kafka toplanan verileri arayabilirsiniz. Bazı örnek aramalar şunlardır:

    * Disk kullanımı: `Perf | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * CPU kullanımı: `Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)`

    * Saniye başına gelen iletileri: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)`

    * Saniye başına gelen bayt sayısı: `metrics_kafka_CL | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    * Saniye başına Giden bayt sayısı: `metrics_kafka_CL | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)`

    > [!IMPORTANT]
    > Sorgu değerleri kümeye özel bilgileri ile değiştirin. Örneğin, `ClusterName_s` kümenizin adını ayarlamanız gerekir. `HostName_s` Kümedeki çalışan düğümü etki alanı adını ayarlamanız gerekir.

    Ayrıca girebilirsiniz `*` kaydedilen tüm türleri aramak için. Şu anda günlükleri sorgularında kullanılabilir:

    | Günlük türü | Açıklama |
    | ---- | ---- |
    | Günlük\_kafkaserver\_Temizle | Kafka Aracısı server.log |
    | Günlük\_kafkacontroller\_Temizle | Kafka Aracısı controller.log |
    | Ölçümleri\_kafka\_Temizle | Kafka JMX ölçümleri |

    ![CPU kullanımı arama görüntüsü](./media/apache-kafka-log-analytics-operations-management/kafka-cpu-usage.png)
 
 ## <a name="next-steps"></a>Sonraki adımlar

Günlük analizi hakkında daha fazla bilgi için bkz: [günlük analizi çalışma alanı ile çalışmaya başlama](../../log-analytics/log-analytics-get-started.md) belge.

Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

 * [Hdınsight kümeleri arasında yansıtma Kafka](apache-kafka-mirroring.md)
 * [Hdınsight üzerinde Kafka ölçeklenebilirliği artırmak](apache-kafka-scalability.md)
 * [(DStreams) ile Kafka Spark akış kullanın](../hdinsight-apache-spark-with-kafka.md)
 * [Kafka ile akış yapılandırılmış Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
