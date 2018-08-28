---
title: Azure HDInsight kümelerinizi izlemek için log Analytics'i kullanma
description: Bir HDInsight kümesinde çalışan işleri izlemek için Azure Log Analytics kullanmayı öğrenin.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: jasonh
ms.openlocfilehash: 87d1593327ab45f064c6f6b19dfbe3882baaa446
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109525"
---
# <a name="use-azure-log-analytics-to-monitor-hdinsight-clusters"></a>Azure Log Analytics, HDInsight kümelerinizi izlemek için kullanın

Azure Log Analytics, HDInsight Hadoop küme işlemleri izlemek etkinleştirme ve bir Hdınsight izleme çözümünü ekleme öğrenin.

[Log Analytics](../log-analytics/log-analytics-overview.md) bulut izler ve şirket içi Ortamlarınızdaki kullanılabilirliği ve performansı korumak için bir hizmettir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Log Analytics çalışma alanı**. Bu çalışma alanını, kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Log Analytics ortamı olarak düşünebilirsiniz. Yönergeler için bkz. [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace).

* **Bir Azure HDInsight kümesi**. Şu anda aşağıdaki HDInsight küme türleri ile Log Analytics kullanabilirsiniz:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Bir HDInsight kümesi oluşturma hakkında yönergeler için bkz: [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).

> [!NOTE]
> HDInsight küme hem de Log Analytics çalışma alanı, daha iyi performans için aynı bölgede yerleştirmek için önerilir. Azure Log Analytics tüm Azure bölgelerinde kullanılabilir olmadığını unutmayın.

## <a name="enable-log-analytics-by-using-the-portal"></a>Log Analytics portalını kullanarak etkinleştirin.

Bu bölümde, bir Azure Log Analytics çalışma alanı işleri, hata ayıklama günlükleri izlemek üzere kullanmak için mevcut bir HDInsight Hadoop kümesi yapılandırın.

1. Bir HDInsight kümesi, Azure portalında açın.
2. Sol bölmede seçin **izleme**.
3. Sağ bölmede seçin **etkinleştirme**, mevcut bir Log Analytics çalışma alanını seçin ve ardından **Kaydet**.

    ![HDInsight kümeleri için izlemeyi etkinleştirin](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "HDInsight kümeleri için izlemeyi etkinleştir")

    Ayarı kaydetmek için birkaç dakika sürer.

## <a name="enable-log-analytics-by-using-azure-powershell"></a>Log Analytics, Azure PowerShell kullanarak etkinleştirme

Azure PowerShell kullanarak Log Analytics'e etkinleştirebilirsiniz. Cmdlet aşağıdaki gibidir:

```powershell
Enable-AzureRmHDInsightOperationsManagementSuite
      [-Name] <CLUSTER NAME>
      [-WorkspaceId] <LOG ANALYTICS WORKSPACE NAME>
      [-PrimaryKey] <LOG ANALYTICS WORKSPACE PRIMARY KEY>
      [-ResourceGroupName] <RESOURCE GROUIP NAME>
```

Bkz: [etkinleştir AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/Enable-AzureRmHDInsightOperationsManagementSuite?view=azurermps-5.0.0).

Devre dışı bırakmak için cmdlet'i aşağıdaki gibidir:

```powershell
Disable-AzureRmHDInsightOperationsManagementSuite
       [-Name] <CLUSTER NAME>
       [-ResourceGroupName] <RESOURCE GROUP NAME>
```

Bkz: [devre dışı bırak AzureRmHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/disable-azurermhdinsightoperationsmanagementsuite?view=azurermps-5.0.0).

## <a name="install-hdinsight-cluster-management-solutions"></a>HDInsight küme yönetim çözümlerini yükleme

HDInsight, Azure Log Analytics için ekleyebileceğiniz kümeye özgü yönetim çözümleri sağlıyor. [Yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md) Log Analytics'e ek veri ve analiz araçları sağlayarak, işlevsellik ekleyin. Bu çözümlerin, HDInsight kümelerinizi önemli performans ölçümlerini toplamak ve ölçümlerini arama için araçlar sağlar. Bu çözümleri ayrıca görselleştirmeler ve panolar için HDInsight içinde desteklenen çoğu küme türleri sağlar. Topladığınız ölçümleri çözümle birlikte kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz.

Var olan HDInsight çözümlerinin şunlardır:

* HDInsight Hadoop İzleme
* HDInsight HBase izleme
* HDInsight etkileşimli sorgu izleme
* HDInsight Kafka İzleme
* HDInsight Spark İzleme
* HDInsight Storm Monitoring

Bir yönetim çözümü yüklemek yönergeler için bkz. [Azure yönetim çözümlerine](../monitoring/monitoring-solutions.md#install-a-management-solution). Denemeler yapmak için bir HDInsight Hadoop Monotiring çözüm yükleyin. İşlem tamamlandığında, gördüğünüz bir **HDInsightHadoop** kutucuğu altında listelenen **özeti**. Seçin **HDInsightHadoop** Döşe. HDInsightHadoop çözüm şuna benzer:

![HDInsight OMS çözüm görünümü izleme Hadoop](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Kümeye yeni bir küme olduğundan, herhangi bir etkinlik raporu göstermez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Log Analytics, HDInsight kümelerinizi izlemek için sorgu](hdinsight-hadoop-oms-log-analytics-use-queries.md)
