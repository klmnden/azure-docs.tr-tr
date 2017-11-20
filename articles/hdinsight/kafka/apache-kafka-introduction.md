---
title: "HDInsight üzerinde Apache Kafka'ya giriş | Microsoft Docs"
description: "HDInsight üzerinde Apache Kafka hakkında bilgi edinin: Nedir, ne işe yarar, örneklere ve başlangıç bilgilerine nereden ulaşılabilir?"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: f284b6e3-5f3b-4a50-b455-917e77588069
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: c4e0d792ae8f4c17d53430f49d81d179e56b9722
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="introducing-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka’ya giriş

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar. HDInsight üzerinde Kafka, Microsoft Azure bulutunda yönetilen, yüksek oranda ölçeklenebilir ve yüksek oranda kullanılabilir bir hizmet sağlar.

## <a name="why-use-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka neden kullanılmalıdır?

HDInsight üzerinde Kafka aşağıdaki özellikleri sunar:

* Hizmet Düzeyi Sözleşmesi (SLA): [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/).

* Yayımla-abone ol mesajlaşma modeli: Kafka, bir Kafka konu başlığında kayıt yayımlamaya yönelik bir Producer API (Üretici API’si) sağlar. Bir konu başlığına abone olurken Consumer API (Tüketici API’si) kullanılır.

* Akış işleme: Kafka genellikle gerçek zamanlı akış işleme için Apache Storm veya Spark ile birlikte kullanılır. Kafka 0.10.0.0 (HDInsight sürüm 3.5 ve 3.6), Storm ya da Spark gerektirmeden akış çözümleri oluşturmanızı sağlayan bir akış API’sini kullanıma sunmuştur.

* Yatay ölçek: Kafka bölümleri, HDInsight kümesindeki düğümler arasında akış yapar. Kayıtlar kullanılırken yük dengeleme sağlamak üzere tüketici işlemleri, tek bölümlerle ilişkilendirilebilir.

* Sıralı teslim: Her bölüm için kayıtlar alındıkları sırayla akışa depolanır. Bölüm başına bir tüketici işlemi ile ilişkilendirerek, kayıtların sırayla işlenmesini garanti edebilirsiniz.

* Hataya dayanıklı: Bölümler, hataya dayanıklılığı sağlamak üzere düğümler arasında çoğaltılabilir.

* Azure Yönetilen Diskler ile tümleştirme: Yönetilen diskler, HDInsight kümesinde sanal makineler tarafından kullanılan diskler için daha yüksek ölçek ve aktarım hızı sağlar.

    Yönetilen diskler HDInsight üzerinde Kafka için varsayılan olarak etkindir. Düğüm başına kullanılan disk sayısı HDInsight oluşturma işlemi sırasında yapılandırılabilir. Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md).

    Yönetilen disklerin HDInsight üzerinde Kafka ile yapılandırılması hakkında bilgi edinmek için bkz. [HDInsight üzerinde Kafka'nın ölçeklenebilirliğini artırma](apache-kafka-scalability.md).

## <a name="use-cases"></a>Uygulama alanları

* **Mesajlaşma**: Yayımla-abone ol ileti modelini desteklediğinden, Kafka genellikle bir ileti aracısı olarak kullanılır.

* **Etkinlik izleme**: Kafka, kayıtların sıralı olarak günlüğe kaydedilmesini sağladığından, etkinlikleri izlemek ve yeniden oluşturmak için kullanılabilir. Örneğin, bir web sitesindeki veya uygulamadaki kullanıcı işlemleri.

* **Toplama**: Akış işlemeyi kullanarak, bilgileri işlem verileriyle birleştirmek ve merkezi hale getirmek üzere farklı akışlardan gelen bilgileri toplayabilirsiniz.

* **Dönüştürme**: Akış işlemeyi kullanarak, birden fazla girdi konu başlığındaki verileri bir veya daha fazla çıktı konu başlığında birleştirerek verileri zenginleştirebilirsiniz.

## <a name="architecture"></a>Mimari

![Kafka kümesi yapılandırması](./media/apache-kafka-introduction/kafka-cluster.png)

Bu şemada olayların hata dayanıklılığı ile paralel olarak okunması için tüketici gruplarını, bölümlemeyi ve çoğaltmayı kullanan tipik Kafka yapılandırması gösterilmektedir. Kafka kümesinin durumunu yöneten Apache ZooKeeper eş zamanlı, esnek ve düşük gecikmeli işlemler için derlenmiştir. Kafka, kayıtları *başlıklar* halinde depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler, kayıtları Kafka *aracılarından* alır. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır. Her tüketici için bir bölüm oluşturulduğundan akış verileri paralel olarak işlenebilir. Çoğaltma, bölmeleri düğümlere yaymak ve düğüm (aracı) kesintilerine karşı koruma sağlamak için kullanılır. *(L)* harfi bulunan bölüm, verilen bölümün lideridir. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

> [!IMPORTANT]
> Kafka, Azure veri merkezindeki temel donanımın (raf) farkında değildir. Bölümlerin temel donanım üzerinde doğru şekilde dengelendiğinden emin olmak için bkz. [yüksek kullanılabilirliği yapılandırma (Kafka)](apache-kafka-high-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Kafka kullanmaya başlama](apache-kafka-get-started.md)

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](apache-kafka-mirroring.md)

* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)