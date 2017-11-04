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
ms.date: 09/12/2017
ms.author: nitinme
ms.openlocfilehash: dbd3d0ed4337d4fe86465c5c59bf20c0a50a87b4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Hdınsight kümeleri (Önizleme) izlemek için Azure günlük analizi kullanın

Bu makalede, Azure günlük analizi Hdınsight Hadoop kümeleri işlemleri izlemek için nasıl kullanılacağını öğrenin.

Günlük analizi olan bir hizmet olarak [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) bulut izler ve şirket içi ortamları kendi kullanılabilirliğini ve performansını korumak için. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar. 

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bu öğreticiye başlamadan önce bir Azure aboneliğinizin olması gerekir. Bkz. [Ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free).

* **Azure Hdınsight kümesi**. Şu anda aşağıdaki Hdınsight küme türleri ile Azure OMS kullanabilirsiniz:
    * Hadoop
    * Spark
    * HBase
    * Storm
    * Kafka
    * Etkileşimli Hive

    Hdınsight kümesi oluşturma hakkında yönergeler için bkz: [Azure Hdınsight kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).


* **Günlük analizi çalışma alanı**. Bu çalışma alanında, kendi veri deposu, veri kaynakları ve çözümlerle benzersiz bir günlük analizi ortamı olarak düşünebilirsiniz. Önceden oluşturulmuş bu tür bir çalışma olmalıdır, Azure Hdınsight kümeleri ile ilişkilendirebilirsiniz. Yönergeler için bkz: [günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

## <a name="configure-hdinsight-cluster-to-use-azure-log-analytics"></a>Hdınsight kümesi Azure günlük analizi kullanacak şekilde yapılandırma

Bu bölümde, işler, hata ayıklama günlükleri, vb. izlemek üzere bir Azure günlük analizi çalışma alanı kullanmak için mevcut bir Hdınsight Hadoop kümesi yapılandırın.

1. Azure portalında, sol bölmeden tıklatın **Hdınsight kümeleri**ve ardından Azure günlük analizi ile yapılandırmak istediğiniz kümenin adını tıklatın.

2. Sol bölmeden ve küme dikey tıklatın **izleme**.

3. Sağ bölmede **etkinleştirmek**, varolan bir günlük analizi çalışma alanını seçin. **Kaydet** düğmesine tıklayın.

    ![Hdınsight kümeleri için izlemeyi etkinleştir](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Hdınsight kümeleri için izlemeyi etkinleştir")

4. Küme izlemek için günlük analizi kullanmak üzere yapılandırıldıktan sonra gördüğünüz bir **açık OMS Pano** sekmenin üstünde seçeneği. Düğmesini tıklatın.

    ![Açık OMS Pano](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-open-workspace.png "açık OMS Panosu")

5. İstenirse Azure kimlik bilgilerinizi girin. Microsoft OMS panosuna yönlendirilir.

    ![Operations Management Suite portalına](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring-oms-portal.png "Operations Management Suite portalına")

## <a name="next-steps"></a>Sonraki adımlar
* [Hdınsight küme yönetim çözümleri için günlük analizi Ekle](hdinsight-hadoop-oms-log-analytics-management-solutions.md)
