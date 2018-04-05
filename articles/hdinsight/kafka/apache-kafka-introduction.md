---
title: HDInsight üzerinde Apache Kafka'ya giriş | Microsoft Docs
description: 'HDInsight üzerinde Apache Kafka hakkında bilgi edinin: Nedir, ne işe yarar, örneklere ve başlangıç bilgilerine nereden ulaşılabilir?'
services: hdinsight
documentationcenter: ''
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
ms.date: 02/27/2018
ms.author: larryfr
ms.openlocfilehash: 4a4f2c6734de211cd20ee4b9f6815bdefefb25bc
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="introducing-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka’ya giriş

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Kafka ayrıca, adlandırılmış veri akışları yayımlayıp abone olabileceğiniz bir ileti kuyruğuna benzer aracı işlevselliği sağlar. HDInsight üzerinde Kafka, Microsoft Azure bulutunda yönetilen, yüksek oranda ölçeklenebilir ve yüksek oranda kullanılabilir bir hizmet sağlar.

## <a name="why-use-kafka-on-hdinsight"></a>HDInsight üzerinde Kafka neden kullanılmalıdır?

HDInsight üzerinde Kafka aşağıdaki özellikleri sunar:

* __Kafka çalışma süresiyle ilgili %99,9 Hizmet Düzeyi Sözleşmesi (SLA)__ : Daha fazla bilgi için [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) belgesine göz atın.

* __Hataya dayanıklılık__: Kafka, tek boyutlu bir raf düşünülerek tasarlanmıştır ve bu yaklaşım bazı ortamlar için çok uygundur. Ancak, Azure gibi bazı ortamlarda raf iki boyuta ayrılmıştır: Güncelleştirme Etki Alanları (UD) ve Hata Etki Alanları (FD). Microsoft, UD ve FD’ler genelinde Kafka bölümleri ve çoğaltmalarını yeniden dengelemeyi sağlayan araçlar sunar. 

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md).

* **Azure Yönetilen Diskler ile tümleştirme**: Yönetilen diskler, kümede HDInsight üzerinde Kafka tarafından düğüm başına 16 TB’a kadar kullanılabilen diskler için daha yüksek ölçek ve aktarım hızı sağlar.

    Yönetilen disklerin HDInsight üzerinde Kafka ile yapılandırılması hakkında bilgi edinmek için bkz. [HDInsight üzerinde Kafka'nın ölçeklenebilirliğini artırma](apache-kafka-scalability.md).

    Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md).

* **Uyarı, izleme ve tahmine dayalı bakım**: Azure Log Analytics, HDInsights üzerinde Kafka’yı izlemek için kullanılabilir. Log Analytics, Kafka’dan sanal disk, NIC ölçüleri ve JMX ölçüleri gibi makine düzeyinde bilgi açığa çıkarır.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka için günlük çözümleme](apache-kafka-log-analytics-operations-management.md).

* **Kafka verilerini çoğaltma**: Kafka, Kafka kümeleri arasında verileri çoğaltan MirrorMaker yardımcı programını sağlar.

    MirrorMaker hakkında bilgi için bkz. [HDInsight üzerinde Kafka ile Kafka konularını çoğaltma](apache-kafka-mirroring.md).

* **Küme ölçekleme**: HDInsight, küme oluşturulduktan sonra çalışan düğümlerinizin sayısını (Kafka aracısını barındıran) değiştirebilmenizi sağlar. İş yükleri arttıkça kümenin ölçeğini artırın veya maliyeti azaltmak için ölçeği azaltın. Ölçeklendirme Azure portalı, Azure PowerShell ve diğer Azure yönetim arabirimleri üzerinde gerçekleştirilebilir. Kafka için, bölüm çoğaltmalarını ölçeklendirme işlemlerinden sonra yeniden dengelemeniz gerekir. Bölümleri yeniden dengelemek, Kafka’nın yeni çalışan düğüm sayısından yararlanabilmesini sağlar.

    Daha fazla bilgi için bkz. [HDInsight üzerinde Kafka ile yüksek kullanılabilirlik](apache-kafka-high-availability.md).

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

## <a name="architecture"></a>Mimari

![Kafka kümesi yapılandırması](./media/apache-kafka-introduction/kafka-cluster.png)

Bu şemada olayların hata dayanıklılığı ile paralel olarak okunması için tüketici gruplarını, bölümlemeyi ve çoğaltmayı kullanan tipik Kafka yapılandırması gösterilmektedir. Kafka kümesinin durumunu yöneten Apache ZooKeeper eş zamanlı, esnek ve düşük gecikmeli işlemler için derlenmiştir. Kafka, kayıtları *başlıklar* halinde depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler, kayıtları Kafka *aracılarından* alır. HDInsight kümenizdeki her çalışan düğümü bir Kafka aracısıdır. Her tüketici için bir bölüm oluşturulduğundan akış verileri paralel olarak işlenebilir. Çoğaltma, bölmeleri düğümlere yaymak ve düğüm (aracı) kesintilerine karşı koruma sağlamak için kullanılır. *(L)* harfi bulunan bölüm, verilen bölümün lideridir. Üretici trafiği ZooKeeper tarafından yönetilen durum kullanılarak her düğümün liderine yönlendirilir.

Her Kafka aracısı, Azure Yönetilen Diskler’i kullanır. Disk sayısı kullanıcı tarafından tanımlanmıştır ve diskler aracı başına 16 TB’a varan depolama alanı sunabilir.

> [!IMPORTANT]
> Kafka, Azure veri merkezindeki temel donanımın (raf) farkında değildir. Bölümlerin temel donanım üzerinde doğru şekilde dengelendiğinden emin olmak için [Yüksek kullanılabilirliği yapılandırma (Kafka)](apache-kafka-high-availability.md) belgesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Kafka kullanmaya başlama](apache-kafka-get-started.md)

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](apache-kafka-mirroring.md)

* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)