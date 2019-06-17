---
title: Apache Kafka - Azure HDInsight ile yüksek kullanılabilirlik
description: Azure HDInsight’ta Apache Kafka ile nasıl yüksek kullanılabilirlik sağlayacağınız hakkında bilgi edinin. HDInsight içeren Azure bölgesindeki farklı hata etki alanlarında olmaları için bölüm çoğaltmalarını Kafka’da yeniden dengeleme hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: 70843c368b0446a7c0e09559fa759a3cd51912d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64721226"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>HDInsight’ta Apache Kafka ile verilerinizin yüksek kullanılabilirliği

Temel Donanım Rafı yapılandırmasından yararlanmak Apache Kafka konular için bölüm çoğaltmalarını yapılandırmayı öğrenin. Bu yapılandırma, HDInsight’taki Apache Kafka’da depolanmış verilerin kullanılabilir olmasını sağlar.

## <a name="fault-and-update-domains-with-apache-kafka"></a>Apache Kafka ile hata ve güncelleme etki alanları

Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

Her Azure bölgesinde belirli sayıda hata etki alanı bulunur. Etki alanlarının ve içerdikleri hata etki alanı sayısının listesi için [Kullanılabilirlik kümeleri](../../virtual-machines/windows/regions-and-availability.md#availability-sets) belgelerine bakın.

> [!IMPORTANT]  
> Kafka, hata etki alanları ile uyumlu değildir. Kafka’da bir konu oluşturduğunuzda, tüm bölüm çoğaltmaları aynı hata etki alanında depolanabilir. HDInsight, bu sorunu çözmek için [Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) kullanıma sunuyor.

## <a name="when-to-rebalance-partition-replicas"></a>Bölüm çoğaltmaları ne zaman yeniden dengelenmelidir?

Kafka verilerinizin en yüksek kullanılabilirliğe sahip olmasını istiyorsanız, konu başlığınız için bölüm çoğaltmalarını aşağıdaki durumlarda yeniden dengelemeniz gerekir:

* Yeni bir konu veya bölüm oluşturulduğunda

* Bir kümenin ölçeğini artırdığınızda

## <a name="replication-factor"></a>Çoğaltma faktörü

> [!IMPORTANT]  
> Üç hata etki alanı içeren ve çoğaltma faktörü 3 olan bir Azure bölgesi kullanmanız önerilir.

Yalnızca iki hata etki alanı içeren bir bölge kullanmanız gerekiyorsa, çoğaltmaları iki hata etki alanına eşit oranda yaymak için çoğaltma faktörü olarak 4 kullanın.

Konu oluşturma ve çoğaltma faktörü ayarlama örneği için bkz: [HDInsight üzerinde Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belge.

## <a name="how-to-rebalance-partition-replicas"></a>Bölüm çoğaltmalarını yeniden dengeleme

Kullanım [Apache Kafka bölüm yeniden Dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) seçili konuları yeniden dengelemek için. Bu araç bir SSH oturumundan Kafka kümenizin baş düğümüne doğru çalıştırılmalıdır.

SSH kullanarak HDInsight’a bağlanma hakkında daha fazla bilgi için [HDInsight ile SSH’yi kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Kafka'nın ölçeklenebilirlik](apache-kafka-scalability.md)
* [HDInsight üzerinde Apache Kafka ile yansıtma](apache-kafka-mirroring.md)
