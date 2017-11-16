---
title: "Azure Hdınsight kümeleri izlemek için günlük analizi kullanın | Microsoft Docs"
description: "Bir Hdınsight kümesinde çalışan işleri izlemek için Azure günlük analizi kullanmayı öğrenin."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: nitinme
ms.openlocfilehash: 73c472140861a0d0d270021ab268e8c1113c23b5
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Hdınsight kümeleri izlemek için Azure günlük analizi kullanın

Hdınsight'ta Hadoop küme işlemlerini izlemek için Azure günlük analizi kullanmayı öğrenin.

[Günlük analizi](../log-analytics/log-analytics-overview.md) bir hizmetidir [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) bulut izler ve şirket içi ortamları kendi kullanılabilirliğini ve performansını korumak için. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

* **Azure Hdınsight kümesi**. Şu anda aşağıdaki Hdınsight küme türleri ile Azure OMS kullanabilirsiniz:

    * Hadoop
    * HBase
    * Interactive Query
    * Kafka
    * Spark
    * Storm

    Hdınsight kümesi oluşturma hakkında yönergeler için bkz: [Azure Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* **Günlük analizi çalışma alanı**. Bu çalışma alanında, kendi veri deposu, veri kaynakları ve çözümlerle benzersiz bir günlük analizi ortamı olarak düşünebilirsiniz. Önceden oluşturulmuş bu tür bir çalışma olmalıdır, Azure Hdınsight kümeleri ile ilişkilendirebilirsiniz. Yönergeler için bkz: [günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-log-analytics"></a>Günlük analizi kullanmak için Hdınsight kümesi yapılandırın

Bu bölümde, işler, hata ayıklama günlükleri, vb. izlemek üzere bir Azure günlük analizi çalışma alanı kullanmak için mevcut bir Hdınsight Hadoop kümesi yapılandırın.

1. Azure portalında bir Hdınsight kümesi'ni açın.
2. Sol bölmede **izleme**.
3. Sağ bölmede **etkinleştirmek**, varolan bir günlük analizi çalışma alanını seçin ve ardından **kaydetmek**.

    ![Hdınsight kümeleri için izlemeyi etkinleştir](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Hdınsight kümeleri için izlemeyi etkinleştir")

    Ayarı kaydetmek için birkaç dakika sürer.  Bunu yaptıktan sonra gördüğünüz bir **açık OMS Pano** üstteki düğmesine. 

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "açık OMS Panosu")

5. Tıklatın **açık OMS Pano**.
6. İstenirse Azure kimlik bilgilerinizi girin.

    ![Operations Management Suite portalına](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite portalına")

## <a name="next-steps"></a>Sonraki adımlar
* [Hdınsight küme yönetim çözümleri için günlük analizi Ekle](hdinsight-hadoop-oms-log-analytics-management-solutions.md)
