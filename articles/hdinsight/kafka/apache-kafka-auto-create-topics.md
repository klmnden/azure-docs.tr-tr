---
title: Apache Kafka - Azure Hdınsight'ın otomatik konu oluşturulmasını | Microsoft Docs
description: Konuları otomatik olarak oluşturmak üzere Hdınsight üzerinde Apache Kafka yapılandırmayı öğrenin. Ambari aracılığıyla veya PowerShell veya Resource Manager şablonları ile küme oluşturma sırasında true auto.create.topics.enable ayarlayarak Kafka yapılandırabilirsiniz.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/18/2018
ms.author: larryfr
ms.openlocfilehash: fa5dd7533259c794671cd16231fd3f530173bfa3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Konuları otomatik olarak oluşturmak üzere Hdınsight üzerinde Apache Kafka yapılandırma

Varsayılan olarak, hdınsight'ta Kafka otomatik konu oluşturma etkinleştirmez. Otomatik konu oluşturma Ambari kullanarak var olan kümeleri için etkinleştirebilirsiniz. Otomatik konu oluşturma, bir Azure Resource Manager şablonu kullanarak yeni bir Kafka kümesi oluştururken de etkinleştirebilirsiniz.

## <a name="ambari-web-ui"></a>Ambari Web UI

Ambari Web kullanıcı Arabirimi aracılığıyla var olan bir kümede otomatik konu oluşturmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. Gelen [Azure portal](https://portal.azure.com), Kafka kümeyi seçin.

2. Gelen __küme genel bakış__seçin __küme Panosu__. 

    ![Seçili küme Panosu portalıyla görüntüsü](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Ardından __Hdınsight küme Panosu__. İstendiğinde, küme için oturum açma (Yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması.

    ![Hdınsight küme Panosu giriş görüntüsü](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Sayfanın sol taraftaki listeden Kafka hizmeti seçin.

    ![Hizmet listesi](./media/apache-kafka-auto-create-topics/service-list.png)

4. Yapılandırmalar ortasında sayfasını seçin.

    ![Hizmet yapılandırma sekmesi](./media/apache-kafka-auto-create-topics/service-config.png)

5. Filtre alanına değerini `auto.create`. 

    ![Filtre alanına görüntüsü](./media/apache-kafka-auto-create-topics/filter.png)

    Bu özellikler ve görüntüler listesini filtreler `auto.create.topics.enable` ayarı.

6. Değerini değiştirme `auto.create.topics.enable` için `true`ve ardından Kaydet'i seçin. Bir not ekleme ve kaydetme yeniden seçin.

    ![Auto.create.topics.enable giriş görüntüsü](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Kafka hizmetini seçin, __yeniden__ve ardından __yeniden etkilenen tüm__. İstendiğinde, seçin __Onayla tüm yeniden__.

    ![Yeniden başlatma seçimi görüntüsü](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]
> Ambari REST API aracılığıyla Ambari değerleri de ayarlayabilirsiniz. Geçerli yapılandırmayı almak, değiştirme, vb. için birden fazla REST çağrı yapmak zorunda bu genellikle daha zor, aynıdır. Daha fazla bilgi için bkz: [Ambari REST API kullanarak Hdınsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md) belge.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu kullanarak bir Kafka kümesi oluştururken, doğrudan ayarlayabileceğiniz `auto.create.topics.enable` içine ekleyerek bir `kafka-broker`. Bu değer ayarlanırsa yapmayı aşağıdaki JSON parçacığı gösteren `true`:

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

Bu belgede, hdınsight'ta Kafka için otomatik konu oluşturmayı etkinleştirmek öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Kafka kümeleri arasında verileri çoğaltma](apache-kafka-mirroring.md)