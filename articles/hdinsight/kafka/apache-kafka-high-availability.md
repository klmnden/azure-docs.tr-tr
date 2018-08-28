---
title: Apache Kafka - Azure HDInsight ile yüksek kullanılabilirlik
description: Azure HDInsight’ta Apache Kafka ile nasıl yüksek kullanılabilirlik sağlayacağınız hakkında bilgi edinin. HDInsight içeren Azure bölgesindeki farklı hata etki alanlarında olmaları için bölüm çoğaltmalarını Kafka’da yeniden dengeleme hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: bd3b02d54e0a65411e45f0422a0d245645d59096
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049980"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>HDInsight’ta Apache Kafka ile verilerinizin yüksek kullanılabilirliği

Temel donanım rafı yapılandırmasından yararlanmak üzere Kafka için bölüm çoğaltmalarını nasıl yapılandıracağınız hakkında bilgi edinin. Bu yapılandırma, HDInsight’taki Apache Kafka’da depolanmış verilerin kullanılabilir olmasını sağlar.

## <a name="fault-and-update-domains-with-kafka"></a>Kafka ile hata ve güncelleme etki alanları

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

Konu oluşturma ve çoğaltma faktörü ayarlama örneği için [HDInsight’ta Kafka kullanmaya başlama](apache-kafka-get-started.md) belgesine bakın.

## <a name="how-to-rebalance-partition-replicas"></a>Bölüm çoğaltmalarını yeniden dengeleme

Seçili konuları yeniden dengelemek için [Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) kullanın. Bu araç bir SSH oturumundan Kafka kümenizin baş düğümüne doğru çalıştırılmalıdır.

SSH kullanarak HDInsight’a bağlanma hakkında daha fazla bilgi için [HDInsight ile SSH’yi kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight’ta Kafka ölçeklenebilirliği](apache-kafka-scalability.md)
* [HDInsight'ta Kafka ile yansıtma](apache-kafka-mirroring.md)
