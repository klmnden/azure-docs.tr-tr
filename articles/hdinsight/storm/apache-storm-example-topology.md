---
title: Azure HDInsight, Apache Storm topolojilerini örneği
description: Örnek Storm topolojileri listesi oluşturulur ve temel C# ve Java topolojileri de dahil olmak üzere ve olay hub'larıyla çalışma HDInsight üzerinde Apache Storm ile test.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: a535510ce56147f708f1cd8219cf898e6e0df36b
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483783"
---
# <a name="example-apache-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Örnek Apache Storm topolojilerini ve HDInsight üzerinde Apache Storm bileşenleri

Bir liste oluşturulur ve ile kullanmak için Microsoft tarafından yönetilen örneklerinden verilmiştir [Apache Storm](https://storm.apache.org/) HDInsight üzerinde. Bu örneklerde temel oluşturmasını konular çeşitli kapsayan C# ve Event Hubs, Cosmos DB, SQL veritabanı gibi Azure hizmetleriyle çalışmak için Java topolojileri [Apache HBase](https://hbase.apache.org/) HDInsight ve Azure depolama. Bazı örnekler, ayrıca SignalR ve Socket.IO gibi Azure olmayan veya hatta Microsoft dışı teknolojiler ile nasıl çalışılacağını göstermektedir.

| Açıklama | Gösteriler | Dil/Framework |
|:--- |:--- |:--- |
| [Apache Storm Azure Data Lake depolamaya yazma](apache-storm-write-data-lake-store.md) |Azure Data Lake depolamaya yazma |Java |
| [Event Hubs Spout ve Bolt kaynağı](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Olay hub'ı Spout ve Bolt kaynağı |Java |
| [HDInsight üzerinde Apache Storm için Java tabanlı topolojiler geliştirme][5797064f] |Maven |Java |
| [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme][16fce2d1] |Visual Studio için HDInsight araçları |C#, Java |
| [HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme (C#)][844d1d81] |Event Hubs |C# ve Java |
| [HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) |Event Hubs |Java |
| [HDInsight üzerinde Apache Storm kullanarak Event hubs'tan vehicle sensör verisi işleme][246ee964] |Olay hub'ları, Cosmos DB, Azure depolama blobu (WASB) |C#, Java |
| [Ayıklama, dönüştürme ve yükleme (ETL) Azure Event hubs için Apache HBase, HDInsight üzerinde Apache Storm kullanma][b4b68194] |Event Hubs, HBase |C# |
| [Şablon C# HDInsight üzerinde Apache Storm Azure hizmetleriyle çalışmak için Storm topolojisi proje][ce0c02a2] |Olay hub'ları, Cosmos DB, SQL veritabanı, HBase, SignalR |C#, Java |
| [HDInsight üzerinde Apache Storm kullanan Azure Event hubs okumak için ölçeklenebilirlik değerlendirmeleri][d6c540e3] |İleti işleme hızı, Event Hubs, SQL veritabanı |C#, Java |
| [HDInsight üzerinde Apache Storm ile Python kullanma](apache-storm-develop-python-topology.md) |Python bileşenlerini Flux topolojisini |Python |
| [HDInsight üzerinde Apache Storm ile Apache kafka'yı kullanma](../hdinsight-apache-storm-with-kafka.md) | Okuma ve yazma için Apache Kafka Apache Storm | Java |

> [!WARNING]  
> C# örnekleri bu listedeki ilk olarak oluşturulan ve Windows tabanlı HDInsight ve Mayıs doğru bir şekilde Linux tabanlı HDInsight kümeleriyle çalışma test yok. Linux tabanlı kümeler, Mono .NET kodu çalıştırmak için kullanın ve örnekte kullanılan paketler ve çerçeveler uyumluluk sorunlarına sahip olabilir.
>
> Linux üzerinde HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemidir.

### <a name="next-steps"></a>Sonraki Adımlar

* [Oluşturma ve bir Azure HDInsight Apache Storm topolojisinde izleme](./apache-storm-quickstart.md)
* [HDInsight üzerinde Apache Storm ile Apache Storm topolojilerini dağıtma ve yönetme hakkında bilgi edinin][6eb0d3b8]


[6eb0d3b8]:apache-storm-deploy-monitor-topology-linux.md "Visual Studio için web tabanlı Apache Storm panosunu ve Storm kullanıcı arabirimini veya HDInsight Araçları'nı kullanarak topolojileri dağıtma ve yönetme hakkında bilgi edinin."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Visual Studio için HDInsight Araçları'nı kullanarak C# Storm topolojileri oluşturmayı öğrenin."
[5797064f]:apache-storm-develop-java-topology.md "Storm topolojileri temel wordcount topolojisini oluşturarak Maven kullanarak Java'da oluşturmayı öğrenin."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "HDInsight üzerinde Storm ile Azure Event hubs'tan veri yazma ve okuma hakkında bilgi edinin."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Storm topolojisini Azure olay hub'larından iletileri okumak, veri başvurmak için Azure Cosmos DB'den belgeleri okuyun ve verileri Azure depolamasına kaydetmek için kullanmayı öğrenin."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Azure Event Hubs okunurken ve SQL veritabanı, HDInsight üzerinde Apache Storm kullanan depolama aktarım hızı göstermek için çeşitli topolojiler."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Azure Event Hubs'dan, toplam veri okuma & verileri dönüştürün ve ardından HDInsight üzerinde HBase için depolama konusunda bilgi edinin."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Bu proje spout ve bolt Event Hubs, Cosmos DB ve SQL veritabanı gibi çeşitli Azure Hizmetleri ile etkileşim kurmak için Topolojileri için şablonlar içerir."

