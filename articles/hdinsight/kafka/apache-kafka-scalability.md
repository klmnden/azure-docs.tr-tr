---
title: Apache Kafka ölçek artırma - Azure HDInsight
description: Ölçeklenebilirliği artırmak için Azure HDInsight üzerinde Apache Kafka kümesi için yönetilen diskleri yapılandırmayı öğrenin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: 9ad7330960540e0bddc0130736265df402d582ff
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51009343"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için depolamayı ve ölçeklenebilirliği yapılandırma

HDInsight üzerinde Apache Kafka tarafından kullanılan yönetilen disk sayısını yapılandırmayı öğrenin.

HDInsight üzerinde Kafka, HDInsight kümesindeki sanal makinelerin yerel diskini kullanır. Kafka G/Ç açısından oldukça yoğun olduğundan yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Kafka için geleneksel sanal sabit diskler (VHD) kullanıldıysa her düğüm 1 TB ile sınırlıdır. Yönetilen disklerle kümedeki her düğüm için 16 TB elde etmek üzere birden çok disk kullanabilirsiniz.

Aşağıdaki diyagramda, yönetilen diskli HDInsight üzerinde Kafka ile yönetilen disksiz HDInsight üzerinde Kafka karşılaştırılmaktadır:

![HDInsight üzerinde Kafka'da VM başına tek bir VHD kullanımı ile VM başına birden fazla yönetilen disk kullanımının karşılaştırmasını gösteren diyagram](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Yönetilen diskleri yapılandırma: Azure portalı

1. Portalı kullanarak küme oluşturmaya yönelik genel adımları öğrenmek için [HDInsight kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md) bölümündeki adımları uygulayın. Portal oluşturma işlemini tamamlamayın.

2. __Küme boyutu__ bölümünde, disk sayısını yapılandırmak için __çalışan düğümü başına disk sayısı__ alanını kullanın.

    > [!NOTE]
    > Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Premium diskler, DS ve GS serisi VM'lerle kullanılır. Diğer tüm VM türleri standart disk kullanır.

    ![Çalışan düğümü başına disk sayısı vurgulanmış şekilde küme boyutu bölümünün görüntüsü](./media/apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Yönetilen diskleri yapılandırma: Resource Manager şablonu

Kafka kümesindeki çalışan düğümleri tarafından kullanılan disk sayısını denetlemek için şablonun şu bölümünü kullanın:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Yönetilen disklere yapılandırma işlemi gösterilmektedir eksiksiz bir şablon bulabilirsiniz [ https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json ](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Sonraki adımlar

HDInsight üzerinde Kafka ile çalışma hakkında daha fazla bilgi için şu belgelere göz atın:

* [MirrorMaker kullanarak HDInsight üzerinde Kafka kopyası oluşturma](apache-kafka-mirroring.md)
* [Apache Storm’u HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-storm-with-kafka.md)
* [Apache Spark’ı HDInsight üzerinde Kafka ile kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Azure Sanal Ağ üzerinden Kafka’ya bağlanma](apache-kafka-connect-vpn-gateway.md)

* [Kafka ile yönetilen disklerle ilgili HDInsight blogu](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
