---
title: "Apache Kafka - Azure HDInsight ile yüksek kullanılabilirlik | Microsoft Docs"
description: "Azure HDInsight’ta Apache Kafka ile nasıl yüksek kullanılabilirlik sağlayacağınız hakkında bilgi edinin. HDInsight içeren Azure bölgesindeki farklı hata etki alanlarında olmaları için bölüm çoğaltmalarını Kafka’da yeniden dengeleme hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: f8164d1c3483b28e5f2abcc8035da78880daec1e
ms.contentlocale: tr-tr
ms.lasthandoff: 06/28/2017

---
# <a name="high-availability-of-your-data-with-apache-kafka-preview-on-hdinsight"></a>HDInsight’ta Apache Kafka (önizleme) ile verilerinizin yüksek kullanılabilirliği

Temel donanım rafı yapılandırmasından yararlanmak üzere Kafka için bölüm çoğaltmalarını nasıl yapılandıracağınız hakkında bilgi edinin. Bu yapılandırma, HDInsight’taki Apache Kafka’da depolanmış verilerin kullanılabilir olmasını sağlar.

## <a name="fault-and-update-domains-with-kafka"></a>Kafka ile hata ve güncelleme etki alanları

Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

Her Azure bölgesinde belirli sayıda hata etki alanı bulunur. Etki alanlarının ve içerdikleri hata etki alanı sayısının listesi için [Kullanılabilirlik kümeleri](../virtual-machines/linux/regions-and-availability.md#availability-sets) belgelerine bakın.

> [!IMPORTANT]
> Kafka, hata etki alanları ile uyumlu değildir. Kafka’da bir konu oluşturduğunuzda, tüm bölüm çoğaltmaları aynı hata etki alanında depolanabilir. Bu sorunu çözmek için [Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) kullanıma sunuyoruz.

## <a name="when-to-rebalance-partition-replicas"></a>Bölüm çoğaltmaları ne zaman yeniden dengelenmelidir?

Kafka verilerinizin en yüksek kullanılabilirliğe sahip olmasını istiyorsanız, konu başlığınız için bölüm çoğaltmalarını aşağıdaki durumlarda yeniden dengelemeniz gerekir:

* Yeni bir konu veya bölüm oluşturulduğunda

* Bir kümenin ölçeğini artırdığınızda

## <a name="replication-factor"></a>Çoğaltma faktörü

> [!IMPORTANT]
> Üç hata etki alanı içeren ve çoğaltma faktörü 3 olan bir Azure bölgesi kullanmanız önerilir.

Yalnızca iki hata etki alanı içeren bir bölge kullanmanız gerekiyorsa, çoğaltmaları iki hata etki alanına eşit oranda yaymak için çoğaltma faktörü olarak 4 kullanın.

Konu oluşturma ve çoğaltma faktörü ayarlama örneği için [HDInsight’ta Kafka kullanmaya başlama](hdinsight-apache-kafka-get-started.md) belgesine bakın.

## <a name="how-to-rebalance-partition-replicas"></a>Bölüm çoğaltmalarını yeniden dengeleme

Seçili konuları yeniden dengelemek için [Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools) kullanın. Bu araç bir SSH oturumundan Kafka kümenizin baş düğümüne doğru çalıştırılmalıdır.

SSH kullanarak HDInsight’a bağlanma hakkında daha fazla bilgi için [HDInsight ile SSH’yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md) belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight’ta Kafka ölçeklenebilirliği](hdinsight-apache-kafka-scalability.md)
* [HDInsight'ta Kafka ile yansıtma](hdinsight-apache-kafka-mirroring.md)
