---
title: "Hdınsight küme yönetim çözümleri Azure günlük Analizi'ne ekleme | Microsoft Docs"
description: "Hdınsight kümeleri için özel görünümler oluşturmak için Azure günlük analizi kullanmayı öğrenin."
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
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: 9b2871a3dc7e8c3f36666d44e68c298f43fa6267
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Hdınsight küme yönetim çözümleri için günlük analizi Ekle

Hdınsight için Azure günlük analizi ekleyebilirsiniz kümeye özgü yönetim çözümleri sağlar. [Yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md) işlevsellik eklemek [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), ek veri ve çözümleme araçları için günlük analizi sağlar. Bu çözümler, Hdınsight kümelerinizi önemli performans ölçümleri toplamak ve ölçümleri aramak için araçlar sağlar. Bu çözümlerin de görselleştirmeleri ve panolar için Hdınsight'ta desteklenen çoğu küme türleri sağlar. Çözümle topladığınız ölçümleri kullanarak özel izleme kurallarını ve uyarıları oluşturabilirsiniz. 

Bu makalede, kümeye özgü yönetim çözümleri bir Operations Management Suite çalışma alanına eklemeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* Hdınsight kümesi Azure günlük analizi kullanmak üzere yapılandırmış olmanız gerekir. Yönergeler için bkz: [kullanım Azure günlük analizi Hdınsight kümeleri ile](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="add-cluster-specific-management-solutions"></a>Kümeye özgü yönetim çözümleri Ekle

Bu bölümde, var olan bir Operations Management Suite çalışma alanına bir HBase kümesi yönetim çözümü ekleyin.

1. Azure portalında bir HDInsigt küme Aç'ı tıklatın **izleme**ve ardından **açık OMS Pano**.

    ![Açık Operations Management Suite Pano](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "açık OMS Panosu")

1. OMS panosunda tıklatın **Çözümleri Galerisi** veya **Görünüm Tasarımcısı** sol bölmeden simgesi.

    ![Yönetim çözümü Operations Management Suite ekleme](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "yönetim çözümü Operations Management Suite ekleme")

2. Çözümleri Galerisi'nde aşağıdaki kutucuklara birini tıklatın:

    - Hdınsight Hadoop izleme
    - Hdınsight HBase izleme (Önizleme)
    - Hdınsight izleme Kafka
    - Hdınsight Storm izleme
    - Hdınsight Spark izleme

3. Sonraki ekranda, tıklatın **Ekle**.  Aşağıdaki ekran görüntüsünde, HBase izleme için Ekle düğmesini gösterir.

     ![HBase yönetim çözümü ekleyin](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "ekleme HBase yönetim çözümü")

4. HBase yönetim çözümü için OMS Panoda bir kutucuğu görebilirsiniz. (Bu makalede için önkoşul parçası) olarak Operations Management Suite ile ilişkili küme bir HBase kümesi ise, döşeme kümede küme ve düğüm sayısını adını gösterir.

    ![HBase yönetim çözümü eklenen](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "HBase yönetim çözümü eklendi")

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight kümeleri izlemek için sorgu Azure günlük analizi](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Operations Management Suite günlük analizi ile çalışma](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Günlük analizi uyarı kuralları oluşturma](../log-analytics/log-analytics-alerts-creating.md)
