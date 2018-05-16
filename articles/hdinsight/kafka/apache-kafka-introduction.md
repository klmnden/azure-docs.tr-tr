---
title: HDInsight üzerinde Apache Kafka'ya giriş | Microsoft Docs
description: 'HDInsight üzerinde Apache Kafka hakkında bilgi edinin: Nedir, ne işe yarar, örneklere ve başlangıç bilgilerine nereden ulaşılabilir?'
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/11/2018
ms.author: larryfr
ms.openlocfilehash: 51b4e4dea0f0c4da739f9e40beb74931060dd22b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka nedir?

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar. 

Aşağıda, HDInsight üzerinde Kafka’ya özgü özellikler verilmiştir:

* Bu, basitleştirilmiş bir yapılandırma işlemi sağlayan yönetilen bir hizmettir. Sonuçta, Microsoft tarafından test edilen ve desteklenen bir yapılandırma elde edilir.

* Microsoft, Kafka çalışma süresinde %99,9 Hizmet Düzeyi Sözleşmesi (SLA) sağlar. Daha fazla bilgi için [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) belgesine bakın.

* Kafka için yedekleme deposu olarak Azure Yönetilen Diskler’i kullanır. Yönetilen Diskler, her Kafka aracısı için 16 TB’a kadar depolama alanı sağlayabilir. Yönetilen disklerin HDInsight üzerinde Kafka ile yapılandırılması hakkında bilgi edinmek için bkz. [HDInsight üzerinde Kafka'nın ölçeklenebilirliğini artırma](apache-kafka-scalability.md).

    Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md).

* Kafka, bir rafın tek bir boyutlu görünümüyle tasarlanmıştır. Azure, bir rafı iki boyuta ayırır: Güncelleştirme Etki Alanları (UD) ve Hata Etki Alanları (FD). Microsoft, UD ve FD’ler genelinde Kafka bölümleri ve çoğaltmalarını yeniden dengeleyen araçlar sağlar. 

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md).

* HDInsight, küme oluşturulduktan sonra çalışan düğümlerinin sayısını (Kafka aracısını barındıran) değiştirmenize olanak sağlar. Ölçeklendirme Azure portalı, Azure PowerShell ve diğer Azure yönetim arabirimleri üzerinde gerçekleştirilebilir. Kafka için, bölüm çoğaltmalarını ölçeklendirme işlemlerinden sonra yeniden dengelemeniz gerekir. Bölümleri yeniden dengelemek, Kafka’nın yeni çalışan düğüm sayısından yararlanabilmesini sağlar.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md).

* HDInsight üzerinde Kafka’yı izlemek için Azure Log Analytics kullanılabilir. Log Analytics, Kafka’dan sanal disk, NIC ölçüleri ve JMX ölçüleri gibi makine düzeyinde bilgi açığa çıkarır.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka için günlük çözümleme](apache-kafka-log-analytics-operations-management.md).

### <a name="kafka-on-hdinsight-architecture"></a>HDInsight mimarisi üzerinde Kafka

Aşağıdaki diyagram, olayların hata dayanıklılığı ile paralel olarak okunması için tüketici gruplarını, bölümlemeyi ve çoğaltmayı kullanan tipik Kafka yapılandırmasını göstermektedir:

![Kafka kümesi yapılandırması diyagramı](./media/apache-kafka-introduction/kafka-cluster.png)

Apache ZooKeeper, Kafka kümesinin durumunu yönetir. Zookeeper, eşzamanlı, esnek ve düşük gecikme süreli işlemler için derlenmiştir. 

Kafka, kayıtları (verileri) **konular** içinde depolar. Kayıtlar, **Üreticiler** tarafından oluşturulur ve **tüketiciler** tarafından kullanılır. Üreticiler, Kafka **aracılarına** kayıtlar gönderir. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır. 

Aracılar arasında konuların bölüm kayıtları. Kayıtları tüketirken, verilerin paralel işlemesini elde etmek için bölüm başına en fazla bir tüketici kullanabilirsiniz.

Çoğaltma, düğümler arasında bölmeleri çoğaltmak ve düğüm (aracı) kesintilerine karşı koruma sağlamak için kullanılır. Diyagramda *(L)* harfi bulunan bölüm, verilen bölümün lideridir. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

## <a name="why-use-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka neden kullanılmalıdır?

Aşağıda, HDInsight üzerinde Kafka kullanılarak gerçekleştirilebilen yaygın görevler ve desenler verilmiştir:

* **Kafka verilerini çoğaltma**: Kafka, Kafka kümeleri arasında verileri çoğaltan MirrorMaker yardımcı programını sağlar.

    MirrorMaker hakkında bilgi için bkz. [HDInsight üzerinde Kafka ile Kafka konularını çoğaltma](apache-kafka-mirroring.md).

* **Yayımla-abone ol mesajlaşma modeli**: Kafka, bir Kafka konu başlığında kayıt yayımlamaya yönelik bir Producer API (Üretici API’si) sağlar. Bir konu başlığına abone olurken Consumer API (Tüketici API’si) kullanılır.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md).

* **Akış işleme**: Kafka genellikle gerçek zamanlı akış işleme için Apache Storm veya Spark ile birlikte kullanılır. Kafka 0.10.0.0 (HDInsight sürüm 3.5 ve 3.6), Storm ya da Spark gerektirmeden akış çözümleri oluşturmanızı sağlayan bir akış API’sini kullanıma sunmuştur.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md).

* **Yatay ölçek**: Kafka bölümleri, HDInsight kümesindeki düğümler arasında akış yapar. Kayıtlar kullanılırken yük dengeleme sağlamak üzere tüketici işlemleri, tek bölümlerle ilişkilendirilebilir.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md).

* **Sıralı teslim**: Her bölüm için kayıtlar alındıkları sırayla akışa depolanır. Bölüm başına bir tüketici işlemi ile ilişkilendirerek, kayıtların sırayla işlenmesini garanti edebilirsiniz.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka kullanmaya başlama](apache-kafka-get-started.md).

## <a name="use-cases"></a>Uygulama alanları

* **Mesajlaşma**: Yayımla-abone ol ileti modelini desteklediğinden, Kafka genellikle bir ileti aracısı olarak kullanılır.

* **Etkinlik izleme**: Kafka, kayıtların sıralı olarak günlüğe kaydedilmesini sağladığından, etkinlikleri izlemek ve yeniden oluşturmak için kullanılabilir. Örneğin, bir web sitesindeki veya uygulamadaki kullanıcı işlemleri.

* **Toplama**: Akış işlemeyi kullanarak, bilgileri işlem verileriyle birleştirmek ve merkezi hale getirmek üzere farklı akışlardan gelen bilgileri toplayabilirsiniz.

* **Dönüştürme**: Akış işlemeyi kullanarak, birden fazla girdi konu başlığındaki verileri bir veya daha fazla çıktı konu başlığında birleştirerek verileri zenginleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [Hızlı Başlangıç: HDInsight üzerinde Kafka oluşturma](apache-kafka-get-started.md)

* [Öğretici: HDInsight üzerinde Kafka ile Apache Spark kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Öğretici: HDInsight üzerinde Kafka ile Apache Storm kullanma](../hdinsight-apache-storm-with-kafka.md)