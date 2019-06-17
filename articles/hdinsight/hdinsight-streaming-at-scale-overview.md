---
title: Azure HDInsight ölçekte akış
description: Veri akışı ölçeklenebilir HDInsight kümeleri ile kullanma
author: hrasheed-msft
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: e2b6cbabc9a0c727c9eb0232bd55048493b29128
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64696918"
---
# <a name="streaming-at-scale-in-hdinsight"></a>HDInsight’ta ölçeğe göre akış

İçinde hareket olan veriler üzerinde gerçek zamanlı büyük veri çözümleri işlevi görür. Genellikle, bu verileri kendi varış sırasındaki çok değerlidir. Gelen veri akışını o an işlenebilecek miktardan daha büyük hale gelirse, kaynakları kısıtlama gerekebilir. Alternatif olarak, bir HDInsight kümesi isteğe bağlı düğümleri ekleyerek akış çözümünüzü karşılamak için ölçeği artırabilirsiniz.


Akış bir uygulamada, bir veya daha fazla veri kaynağı hızla herhangi yararlı bilgiler bırakmadan alınan gereken olayları (bazen saniye başına milyonlarca) oluşturduğunuzdan. Gelen olayları ile işlenir *akışı arabelleğe alma*ayrıca adlı *olay sıraya alma*, gibi bir hizmet tarafından [Apache Kafka](kafka/apache-kafka-introduction.md) veya [Event Hubs](https://azure.microsoft.com/services/event-hubs/). Ardından olayları topladıktan sonra gerçek zamanlı analiz sisteminde kullanarak verileri çözümleyebilir *akış işleme* gibi katman [Apache Storm](storm/apache-storm-overview.md) veya [Apache Spark akışı](spark/apache-spark-streaming-overview.md). İşlenen verilerin uzun vadeli depolama sistemlerinde gibi depolanabilir [Azure Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)ve gibi gerçek zamanlı bir iş zekası Panosu üzerinde görüntülenen [Power BI](https://powerbi.microsoft.com), Tableau veya özel bir web Sayfa.


![HDInsight akış desenleri](./media/hdinsight-streaming-at-scale-overview/HDInsight-streaming-patterns.png)

## <a name="apache-kafka"></a>Apache Kafka

Apache Kafka tarafından sağlanan yüksek performanslı, düşük gecikme süreli message Queuing hizmeti ve artık açık kaynak yazılım (OSS) Apache Suite'in bir parçası. Kafka kullanan bir Yayımla ve abone olma Mesajlaşma modeli ve depoları akışları bölümlenmiş verilerin güvenli bir şekilde dağıtılan, çoğaltılmış bir küme. Aktarım hızı arttıkça Kafka doğrusal olarak ölçeklendirir.

Daha fazla bilgi için [HDInsight üzerinde Apache Kafka'ya giriş](kafka/apache-kafka-introduction.md).

## <a name="apache-storm"></a>Apache Storm

Apache Storm verileri Hadoop ile gerçek zamanlı akışlarını işlemek için optimize edilmiş bir dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. Çekirdek veri gelen bir olay için bir demet sabit bir anahtar/değer çiftleri kümesidir birimidir. Bu demetleri formlar sınırsız bir dizi olan bir Stream bir Spout gelir. Spout (Kafka gibi) bir akış veri kaynağını saran ve diziler yayar. Bir storm topolojisi bu akışları dönüşümler dizisidir.

Daha fazla bilgi için [Azure HDInsight üzerinde Apache Storm nedir?](storm/apache-storm-overview.md).

## <a name="spark-streaming"></a>Spark akış

Spark Streaming, toplu işlem için kullandığınız aynı kodu yeniden olanak tanıyan Spark uzantısıdır. Batch ve aynı uygulamada etkileşimli sorguları birleştirebilir. Storm farklı olarak, Spark Streaming durum bilgisi olan tam olarak sağlar-bir kez semantiği işlenmesini. İle birlikte kullanıldığında [Kafka doğrudan API](https://spark.apache.org/docs/latest/streaming-kafka-integration.html), hangi tüm Kafka veri alındığında Spark akışı tarafından tam olarak bir kez sağlar, uçtan uca tam olarak elde etmek mümkündür-bir kez garanti eder. Spark akışı'nın güçlü biri kurtarma özelliklerini hataya dayanıklı hatalı düğümleri hızla birden çok düğüm kümede kullanıldığında.

Daha fazla bilgi için [Apache Spark Streaming nedir?](hdinsight-spark-streaming-overview.md).

## <a name="scaling-a-cluster"></a>Küme ölçeklendirme

Oluşturma sırasında kümedeki düğüm sayısını belirtebilseniz de, büyüyebilen veya küçülebilen iş yüküyle eşleşmesi için kümeyi isteyebilirsiniz. Tüm HDInsight kümeleri izin [kümedeki düğüm sayısını değiştirmenize](hdinsight-administer-use-portal-linux.md#scale-clusters). Tüm veriler gibi Azure Depolama'da veya Data Lake Storage Spark kümeleri veri kaybı olmadan ile bırakılabilir.

Ayırma teknolojilere avantajları vardır. Örneğin, Kafka çok g/ç kullanımı yoğun olan ve çok gerek kalmayacak şekilde teknolojisi, arabelleğe alma bir olaydır işleme gücü. Buna karşılık, akış işlemcileri akışı Spark Streaming gibi bilgi işlem açısından yoğun gerektiren daha güçlü VM'ler. Birbirinden farklı kümeler halinde bu teknolojiler sağlayarak, bunları birbirinden bağımsız olarak sanal makinelerin en iyi şekilde sırasında ölçeklendirebilirsiniz.

### <a name="scale-the-stream-buffering-layer"></a>Katman arabelleğe alma akış ölçeklendirme

Event Hubs, Kafka ve teknolojileri arabelleğe alma stream hem de bölümler kullanın ve tüketicileri bu bölümleri okuyun. Giriş aktarım hızı ölçeklendirme bölüm sayısı gerektirir ve bölümleri ekleme artan paralellik sağlar. Unutmayın hedef ölçek ile başlamanız gerekir, bu nedenle, olay hub'ları, dağıtımdan sonra bölüm sayısı değiştirilemez. Kafka ile mümkün [bölümler ekleyerek](https://kafka.apache.org/documentation.html#basic_ops_cluster_expansion), hatta veri Kafka işlerken. Kafka bölüm yeniden atamak için bir araç sağlar `kafka-reassign-partitions.sh`. HDInsight sağlayan bir [bölümün çoğaltması yeniden Dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools), `rebalance_rackaware.py`. Bu yeniden araç çağırır `kafka-reassign-partitions.sh` her çoğaltma ayrı hata etki alanı ve güncelleme etki alanı, Kafka raf uyumlu ve artan hata toleransı yapmadan olduğunu şekilde aracı.

### <a name="scale-the-stream-processing-layer"></a>Akış işleme katmanı ölçeklendirin

Hatta veri işlenirken Apache Storm ve Spark akışı, kümeler, alt düğüm eklemeyi destekler.

Storm ölçeklendirme aracılığıyla eklenen yeni düğümlerden yararlanmak için küme boyutu önce başlatılan tüm Storm topolojilerini yeniden dengelemeniz gerekir. Bu yeniden Dengeleme yapılabilir Storm kullanarak web kullanıcı arabirimini veya kendi CLI. Daha fazla bilgi için [Apache Storm belgeleri](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

Apache Spark, uygulama gereksinimlerine bağlı olarak, ortamı yapılandırmak için üç anahtar parametreleri kullanır: `spark.executor.instances`, `spark.executor.cores`, ve `spark.executor.memory`. Bir *Yürütücü* Spark uygulaması için başlatılan bir işlemdir. Bir yürütücü çalışan düğümü üzerinde çalışan ve uygulama görevleri gerçekleştirmesi için sorumludur. Varsayılan yürütücü ve her küme için Yürütücü boyutları sayısını çalışan düğümlerine ve çalışan düğümü boyutu sayısına göre hesaplanır. Bu numaraları depolanan `spark-defaults.conf`her bir küme baş düğümüne dosyası.

Şu üç parametreyi, küme üzerinde çalıştırılır ve ayrıca tek tek her uygulama için belirtilen tüm uygulamalar için küme seviyesinde yapılandırılabilir. Daha fazla bilgi için [Apache Spark kümelerine kaynaklarını yönetme](spark/apache-spark-resource-manager.md).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Storm ile çalışmaya başlama](storm/apache-storm-tutorial-get-started-linux.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](storm/apache-storm-example-topology.md)
* [HDInsight üzerinde Apache Spark'a giriş](spark/apache-spark-overview.md)
* [HDInsight üzerinde Apache kafka'yı kullanmaya başlayın](kafka/apache-kafka-get-started.md)
