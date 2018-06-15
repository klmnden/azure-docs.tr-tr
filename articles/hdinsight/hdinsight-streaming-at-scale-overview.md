---
title: Azure hdınsight'ta ölçekte akış | Microsoft Docs
description: Ölçeklenebilir Hdınsight kümeleri ile akış verileri kullanma
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: raghavmohan
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: ramoha
ms.openlocfilehash: 4f1a0873ccdffde7e3567d7e3c50336b20749116
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31408679"
---
# <a name="streaming-at-scale-in-hdinsight"></a>HDInsight’ta ölçeğe göre akış

İçinde hareket halinde olan veriler üzerinde gerçek zamanlı büyük veri çözümleri davranır. Genellikle, bu veriler, Varış aynı anda en faydalıdır. Gelen veri akışının o anda işlenen büyük olursa, kaynakları kısıtlama gerekebilir. Alternatif olarak, Hdınsight kümesi isteğe bağlı düğüm ekleyerek akış çözümünüzü karşılamak üzere ölçeği artırabilirsiniz.

Bir akış uygulamasında, bir veya daha fazla veri kaynağı hızla herhangi yararlı bilgiler bırakmadan alınan gereken olayları (bazen saniye başına milyonlarca) oluşturduğunu. Gelen olaylar ile işlenmektedir *akışı arabelleğe alma*, olarak da bilinir *olay queuing*, gibi bir hizmet tarafından [Kafka](kafka/apache-kafka-introduction.md) veya [olay hub'ları](https://azure.microsoft.com/services/event-hubs/). Olayları topladıktan sonra gerçek zamanlı analiz sistemi içinde kullanarak verileri sonra çözümleyebilirsiniz *akış işleme* gibi katman [Storm](storm/apache-storm-overview.md) veya [Spark akış](spark/apache-spark-streaming-overview.md). İşlenen verilerin uzun vadeli depolama sistemlerinde gibi depolanabilir [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)ve gerçek zamanlı bir iş zekası Panoda gibi görüntülenen [Power BI](https://powerbi.microsoft.com), Tableau ya da özel bir web sayfası .

![Hdınsight akış desenleri](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka yüksek verimlilik, düşük gecikme süreli message Queuing hizmeti sağlar ve Apache suite açık kaynak yazılım (OSS) artık parçasıdır. Kafka yayımlamayı kullanır ve güvenli bir şekilde dağıtılmış, çoğaltılmış bir küme bölümlenmiş veri Mesajlaşma modeli ve depoları akışları abone olun. Üretilen iş arttıkça Kafka doğrusal olarak ölçeklendirir.

Daha fazla bilgi için bkz: [Hdınsight üzerinde Apache Kafka Tanıtımı](kafka/apache-kafka-introduction.md).

## <a name="apache-storm"></a>Apache Storm

Apache Storm verileri Hadoop ile gerçek zamanlı Akışlar işlemek için iyileştirilmiş bir dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. Çekirdek veri gelen olayı için sabit bir anahtar/değer çiftleri kümesidir bir tanımlama grubu birimidir. Bu diziler formlar sınırsız bir dizi olan bir akış Spout gelir. Spout akış veri kaynağı (örneğin, Kafka) sarmalar ve diziler yayar. Storm topolojisini bu akışlar dönüşümler dizisidir.

Daha fazla bilgi için bkz: [Azure hdınsight'ta Apache Storm nedir?](storm/apache-storm-overview.md).

## <a name="spark-streaming"></a>Spark akış

Spark akış toplu işleme için kullandığınız aynı kodunu yeniden olanak tanıyan Spark uzantısıdır. Batch ve aynı uygulamada etkileşimli sorgular birleştirebilirsiniz. Storm farklı olarak, Spark akış durum bilgisi olan tam olarak sağlar-semantiği işleme sonra. İle birlikte kullanıldığında [Kafka doğrudan API](http://spark.apache.org/docs/latest/streaming-kafka-integration.html), hangi tüm Kafka veri alındığında Spark akış tarafından tam olarak bir kez sağlar, uçtan uca tam olarak elde etmek mümkündür-kez güvence altına alır. Spark akış'ın gücü biri kurtarma yeteneklerini hataya dayanıklı hatalı düğümleri hızlı bir şekilde birden çok düğüm kümede kullanıldığında.

Daha fazla bilgi için bkz: [Spark Akış nedir?](hdinsight-spark-streaming-overview.md).

## <a name="scaling-a-cluster"></a>Küme ölçeklendirme

Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, büyütür veya küçültür iş yüküyle eşleşmesi için kümeyi isteyebilirsiniz. Tüm Hdınsight kümeleri izin [kümedeki düğüm sayısını değiştirmenize](hdinsight-administer-use-management-portal.md#scale-clusters). Tüm verilerin depolandığı gibi Azure Storage veya Data Lake Store Spark kümeleri veri kaybı olmaksızın, ile bırakılabilir.

Ayırma teknolojileri faydası vardır. Örneğin, Kafka çok g/ç yoğun olan ve çok gerekli değildir, teknolojisi arabelleğe alma bir olaydır işlemci gücü. Buna karşılık, akış işlemciler Spark akış gibi işlem yoğunluklu, daha güçlü VM'ler gerektirme. Farklı kümeler halinde ayrılmış bu teknolojiler sağlayarak, bunları bağımsız olarak en iyi VM'ler kullanılarak ölçeklendirebilirsiniz.

### <a name="scale-the-stream-buffering-layer"></a>Katman arabelleğe alma akış ölçeklendirme

Olay hub'ları ve Kafka teknolojileri arabelleğe alma akış her ikisi de bölümleri kullanın ve tüketicilerin bu bölümleri okuyun. Giriş işleme ölçeklendirme bölüm sayısı ölçeklendirme gerektirir ve bölümleri ekleme artan paralellik sağlar. Hedef ölçek aklınızda başlayın önemlidir olay hub ' dağıtımdan sonra bölüm sayısı değiştirilemez. Kafka ile mümkün [bölümleri eklemek](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion), hatta Kafka verileri işlerken. Kafka, bölümler yeniden atamak için bir araç sağlar `kafka-reassign-partitions.sh`. Hdınsight sağlayan bir [aracı dengelenmesi bölüm çoğaltma](https://github.com/hdinsight/hdinsight-kafka-tools), `rebalance_rackaware.py`. Bu yeniden dengelenemiyor aracını çağıran `kafka-reassign-partitions.sh` her çoğaltma ayrı hata etki alanı ve güncelleştirme etki alanı, Kafka raf uyumlu ve artan hata toleransı yapmadan olduğunu şekilde aracı.

### <a name="scale-the-stream-processing-layer"></a>Akış işleme katmanı ölçeklendirme

Apache Storm ve Spark akış bile veri gerçekleştirilirken kendi kümeler, alt düğüm eklemeyi destekler.

Storm ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu artırılmadan önce herhangi bir Storm topolojileri yeniden dengelemeniz gerekir. Bu yeniden dengelenmesi yapılabilir Storm kullanarak web kullanıcı Arabirimi veya kendi CLI. Daha fazla bilgi için bkz: [Apache Storm belgelerine](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

Apache Spark uygulama gereksinimlerine bağlı olarak, ortamı yapılandırmak için üç anahtar parametreleri kullanır: `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir *Yürütücü* Spark uygulama için başlatılan bir işlemdir. Bir yürütücü çalışan düğümünde çalışır ve uygulamanın görevleri gerçekleştirmesi için sorumludur. Yürütücüler ve her küme için Yürütücü boyutları varsayılan sayısı çalışan düğümleri ve alt düğüm boyutu sayısına göre hesaplanır. Bu sayı depolanmış `spark-defaults.conf`her küme baş düğümünde dosya.

Şu üç parametreyi kümede çalışan ve ayrıca tek tek her uygulama için belirtilen tüm uygulamalar için küme düzeyinde yapılandırılabilir. Daha fazla bilgi için bkz: [Apache Spark kümeleri için kaynakları yönetme](spark/apache-spark-resource-manager.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight üzerinde Apache Storm kullanmaya başlama](storm/apache-storm-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](storm/apache-storm-example-topology.md)
* [Hdınsight'ta Spark giriş](spark/apache-spark-overview.md)
* [Hdınsight üzerinde Apache Kafka başlayın](kafka/apache-kafka-get-started.md)
