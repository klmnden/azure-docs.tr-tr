---
title: Hdınsight üzerinde Apache Storm topolojilerini örnek | Microsoft Docs
description: Örnek Storm topolojileri listesi oluşturulur ve olay hub'larıyla çalışma ve temel C# ve Java topolojileri dahil olmak üzere Hdınsight üzerinde Apache Storm ile test.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: larryfr
ms.openlocfilehash: 429373a27ad9be23b986116182a4eda80bace7f7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626896"
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Örnek Storm topolojileri ve Hdınsight üzerinde Apache Storm bileşenleri

Oluşturulan ve Hdınsight üzerinde Apache Storm ile kullanılmak üzere Microsoft tarafından yönetilen örnekleri listesi verilmiştir. Bu örneklerde, temel C# ve Event Hubs, Cosmos DB, SQL Database, Hdınsight ve Azure Storage HBase gibi Azure hizmetleriyle çalışmaya Java topolojileri oluşturmasına konular, çeşitli kapsar. Bazı örnekler ayrıca SignalR ve Socket.IO gibi Azure olmayan veya hatta Microsoft dışı teknolojileri ile çalışmaya nasıl ekleyebileceğiniz gösterilmektedir.

| Açıklama | Gösterir | Dil/Framework |
|:--- |:--- |:--- |
| [Azure Data Lake Store için Apache Storm yazma](apache-storm-write-data-lake-store.md) |Azure Data Lake Store'a yazma |Java |
| [Event Hubs Spout ve Cıvata kaynağı](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Olay hub'ı Spout ve Cıvata için kaynağı |Java |
| [Hdınsight üzerinde Apache Storm için Java tabanlı topolojiler geliştirme][5797064f] |Maven |Java |
| [Visual Studio kullanarak Hdınsight üzerinde Apache Storm için C# topolojileri geliştirme][16fce2d1] |Visual Studio için HDInsight Araçları |C#, Java |
| [Azure Event hubs'tan (C#) hdınsight'ta Storm işlem olayları][844d1d81] |Event Hubs |C# ve Java |
| [HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (Java)](https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/) |Event Hubs |Java |
| [Event Hubs Hdınsight üzerinde Storm kullanmaya aracın algılayıcı verilerini işlemek][246ee964] |Olay hub'ları, Cosmos DB Azure depolama blobunu (WASB) |C#, Java |
| [Ayıklama, dönüştürme ve yükleme (ETL) Azure Event hubs'tan Hdınsight üzerinde Storm kullanarak HBase için][b4b68194] |Olay hub'ları, HBase |C# |
| [Hdınsight üzerinde Storm Azure hizmetlerinden ile çalışmak için şablon C# Storm topolojisini proje][ce0c02a2] |Olay hub'ları, Cosmos DB, SQL veritabanı, HBase, SignalR |C#, Java |
| [Hdınsight üzerinde Storm kullanarak Azure Event hubs'tan okumak için ölçeklenebilirlik değerlendirmeleri][d6c540e3] |İleti işleme, olay hub'ları, SQL veritabanı |C#, Java |
| [Hdınsight üzerinde Storm ile Python kullanma](apache-storm-develop-python-topology.md) |Bir Flux topolojisi ile Python bileşenleri |Python |
| [Hdınsight üzerinde Storm ile Kafka kullanın](../hdinsight-apache-storm-with-kafka.md) | Apache Storm okuma ve Apache Kafka yazma | Java |

> [!WARNING]
> C# örnekleri bu listedeki ilk olarak oluşturulan ve Windows tabanlı Hdınsight ve Mayıs doğru Linux tabanlı Hdınsight kümeleri ile çalışma test değil. Linux tabanlı kümelerde Mono .NET kodu çalıştırmak için kullanın ve örnekte kullanılan paketleri ve çerçeveleri uyumluluk sorunlarına sahip olabilir.
>
> Linux üzerinde Hdınsight sürüm 3.4 veya üstü kullanılan yalnızca işletim sistemidir.

### <a name="next-steps"></a>Sonraki Adımlar

* [HDInsight üzerinde Apache Storm kullanmaya başlama][2b8c3488]
* [Dağıtma ve Storm topolojileri Hdınsight üzerinde Storm ile yönetme hakkında bilgi edinin][6eb0d3b8]

[2b8c3488]:apache-storm-tutorial-get-started-linux.md "Hdınsight kümesinde bir Storm oluşturmak ve örnek topolojileri dağıtmak için Storm panosunu kullanma hakkında bilgi edinin."
[6eb0d3b8]:apache-storm-deploy-monitor-topology.md "Dağıtmak ve yönetmek için Visual Studio web tabanlı Storm panosunu ve Storm kullanıcı Arabirimi veya Hdınsight araçları kullanarak topolojiler öğrenin."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Visual Studio için Hdınsight araçları kullanarak C# Storm topolojileri oluşturmayı öğrenin."
[5797064f]:apache-storm-develop-java-topology.md "Temel wordcount topoloji oluşturarak Maven kullanarak Java Storm topolojilerini oluşturmayı öğrenin."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "Okuma ve Hdınsight üzerinde Storm ile Azure Event hubs'tan veri yazma öğrenin."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Bir Storm topolojisinin Azure olay hub'larından iletileri okumak, veri başvurmak için Azure Cosmos DB'den belgeleri okuyun ve verileri Azure depolamasına kaydetmek için nasıl kullanılacağını öğrenin."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Azure Event Hubs okunurken ve Hdınsight üzerinde Apache Storm kullanarak SQL veritabanına depolanmasını verimlilik göstermek için birkaç topoloji."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Azure Event Hubs'tan gelen, toplam veri okuma & verileri dönüştürmek ve ardından hdınsight'ta HBase için depolamak öğrenin."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Bu proje spout'lar, Cıvatalar ve Event Hubs, Cosmos DB ve SQL veritabanı gibi çeşitli Azure hizmetleriyle etkileşime Topolojileri için şablonlar içerir."

