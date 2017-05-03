---
title: "HDInsight Üzerinde Apache Kafka’ya Giriş | Microsoft Docs"
description: "HDInsight üzerinde Apache Kafka hakkında bilgi edinin. Ne olduğu, ne işe yaradığı ve örnekler ile başlangıç bilgilerinin nerede bulunacağı."
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
ms.date: 02/09/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: a3ceca6cd0f470a5cd6849c345867f094b870a85
ms.lasthandoff: 04/19/2017

---
# <a name="introducing-apache-kafka-on-hdinsight-preview"></a>HDInsight üzerinde Apache Kafka’ya giriş (önizleme)

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar. HDInsight üzerinde Kafka, Microsoft Azure bulutunda yönetilen, yüksek oranda ölçeklenebilir ve yüksek oranda kullanılabilir bir hizmet sağlar.

## <a name="why-use-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka neden kullanılmalıdır?

Kafka aşağıdaki özellikleri sağlar:

* Yayımla-abone ol mesajlaşma modeli: Kafka, bir Kafka konu başlığında kayıt yayımlamaya yönelik bir Producer API (Üretici API’si) sağlar. Bir konu başlığına abone olurken Consumer API (Tüketici API’si) kullanılır.

* Akış işleme: Kafka genellikle gerçek zamanlı akış işleme için Apache Storm veya Spark ile birlikte kullanılır. Kafka 0.10.0.0 (HDInsight sürüm 3.5), Storm ya da Spark gerektirmeden akış çözümleri oluşturmanızı sağlayan bir akış API’sini kullanıma sunmuştur.

* Yatay ölçek: Kafka bölümleri, HDInsight kümesindeki düğümler arasında akış yapar. Kayıtlar kullanılırken yük dengeleme sağlamak üzere tüketici işlemleri, tek bölümlerle ilişkilendirilebilir.

* Sıralı teslim: Her bölüm için kayıtlar alındıkları sırayla akışa depolanır. Bölüm başına bir tüketici işlemi ile ilişkilendirerek, kayıtların sırayla işlenmesini garanti edebilirsiniz.

* Hataya dayanıklı: Bölümler, hataya dayanıklılığı sağlamak üzere düğümler arasında çoğaltılabilir.

## <a name="use-cases"></a>Uygulama alanları

* **Mesajlaşma**: Yayımla-abone ol ileti modelini desteklediğinden, Kafka genellikle bir ileti aracısı olarak kullanılır.

* **Etkinlik izleme**: Kafka, kayıtların sıralı olarak günlüğe kaydedilmesini sağladığından, etkinlikleri izlemek ve yeniden oluşturmak için kullanılabilir. Örneğin, bir web sitesindeki veya uygulamadaki kullanıcı işlemleri.

* **Toplama**: Akış işlemeyi kullanarak, bilgileri işlem verileriyle birleştirmek ve merkezi hale getirmek üzere farklı akışlardan gelen bilgileri toplayabilirsiniz.

* **Dönüştürme**: Akış işlemeyi kullanarak, birden fazla girdi konu başlığındaki verileri bir veya daha fazla çıktı konu başlığında birleştirerek verileri zenginleştirebilirsiniz.

## <a name="where-do-i-start"></a>Nereden başlamalıyım?

Kafka kümesi oluşturma adımları ve üretici, tüketici ve akış API’leri kullanmaya yönelik Java tabanlı örnekler dahil Kafka kullanma hakkında bilgi için bkz. [HDInsight’ta Kafka kullanmaya başlama](hdinsight-apache-kafka-get-started.md)

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Kafka kullanmaya başlama](hdinsight-apache-kafka-get-started.md)

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](hdinsight-apache-kafka-mirroring.md)

* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](hdinsight-apache-storm-with-kafka.md)

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](hdinsight-apache-kafka-connect-vpn-gateway.md)
