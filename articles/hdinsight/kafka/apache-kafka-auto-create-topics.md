---
title: Apache Kafka - Azure HDInsight, otomatik konu oluşturmayı etkinleştirme
description: Konular otomatik olarak oluşturmak için HDInsight üzerinde Apache Kafka yapılandırmayı öğrenin. Kafka auto.create.topics.enable Ambari aracılığıyla veya PowerShell veya Resource Manager şablonları ile küme oluşturma sırasında true olarak ayarlayarak yapılandırabilirsiniz.
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/18/2018
ms.openlocfilehash: f187991b1ff128a45845c2096928945722a9ae6a
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618278"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Konular otomatik olarak oluşturmak için HDInsight üzerinde Apache Kafka yapılandırma

Varsayılan olarak, otomatik konu oluşturmayı HDInsight üzerinde kafka'yı etkinleştirmez. Ambari kullanarak var olan kümeleri için otomatik konu oluşturmayı etkinleştirebilirsiniz. Ayrıca, bir Azure Resource Manager şablonu kullanarak yeni bir Kafka kümesi oluştururken otomatik konu oluşturmayı etkinleştirebilirsiniz.

## <a name="ambari-web-ui"></a>Ambari Web UI

Ambari Web kullanıcı Arabirimi aracılığıyla var olan bir kümede otomatik konu oluşturmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. Gelen [Azure portalında](https://portal.azure.com), Kafka kümesi seçin.

2. Gelen __kümesine genel bakış__seçin __küme Panosu__. 

    ![Seçili küme Panosu portalıyla görüntüsü](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Ardından __HDInsight küme Panosu__. İstendiğinde, küme için oturum açma (Yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması.

    ![HDInsight küme Panosu girdisinin görüntüsü](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Kafka hizmeti, sayfanın sol taraftaki listeden seçin.

    ![Hizmet listesi](./media/apache-kafka-auto-create-topics/service-list.png)

4. Sayfanın ortasında yapılandırmaları seçin.

    ![Hizmet yapılandırma sekmesi](./media/apache-kafka-auto-create-topics/service-config.png)

5. Filtre alanına bir değeri `auto.create`. 

    ![Filtre alanına görüntüsü](./media/apache-kafka-auto-create-topics/filter.png)

    Bu özellikler ve görüntüler listesini filtreler `auto.create.topics.enable` ayarı.

6. Değiştirin `auto.create.topics.enable` için `true`ve sonra da Kaydet'i seçin. Not ekleme ve kaydetme yeniden seçin.

    ![Auto.create.topics.enable girdisinin görüntüsü](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Kafka hizmeti seçin, __yeniden__ve ardından __etkilenen tüm yeniden başlatma__. Sorulduğunda, __Onayla yeniden tüm__.

    ![Yeniden başlatma seçimi görüntüsü](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]
> Ambari değerleri Ambari REST API aracılığıyla da ayarlayabilirsiniz. Geçerli yapılandırmayı almak, Değiştir, vb. için birden çok REST çağrısı yapmanız gibi genel olarak daha zor, budur. Daha fazla bilgi için [yönetme HDInsight kümeleri Ambari REST API'sini kullanarak](../hdinsight-hadoop-manage-ambari-rest-api.md) belge.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu kullanarak bir Kafka kümesi oluştururken, doğrudan ayarlayabilirsiniz `auto.create.topics.enable` içine ekleyerek bir `kafka-broker`. Aşağıdaki JSON kod parçacığında, bu değeri ayarlamak için gösterilmiştir `true`:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Sonraki Adımlar

Bu belgede, HDInsight üzerinde Kafka için otomatik konu oluşturmayı etkinleştirme öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)
