---
title: Azure HDInsight kümelerinizi izlemek için Azure İzleyici'yi kullanma oturumu
description: Bir HDInsight kümesinde çalışan işleri izlemek için Azure İzleyici günlüklerine kullanmayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: hrasheed
ms.openlocfilehash: 610843d325744aec8ad944075f06c63c90b6fe4d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203675"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Azure İzleyici'yi kullanın, HDInsight kümelerinizi izlemek için günlükleri

HDInsight Hadoop küme işlemleri izlemek Azure İzleyici günlüklerini etkinleştirme ve bir HDInsight izleme çözümünü ekleme konusunda bilgi edinin.

[Azure İzleyici günlüklerine](../log-analytics/log-analytics-overview.md) bulut izler ve şirket içi Ortamlarınızdaki kullanılabilirliği ve performansı korumak için Azure İzleyici'de bir hizmettir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

* **Bir Log Analytics çalışma alanı**. Bu çalışma alanını, kendi veri deposu, veri kaynakları ve çözümleri olan benzersiz bir Azure İzleyici günlüklerine ortamı olarak düşünebilirsiniz. Yönergeler için bkz. [Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* **Bir Azure HDInsight kümesi**. Şu anda aşağıdaki HDInsight küme türleri ile Azure İzleyici günlüklerine kullanabilirsiniz:

  * Hadoop
  * HBase
  * Interactive Query
  * Kafka
  * Spark
  * Storm

  Bir HDInsight kümesi oluşturma hakkında yönergeler için bkz: [Azure HDInsight ile çalışmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* **Azure PowerShell Az modül**.  Bkz: [Karşınızda yeni Azure PowerShell Az modül](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

> [!NOTE]  
> HDInsight küme hem de Log Analytics çalışma alanı, daha iyi performans için aynı bölgede yerleştirmek için önerilir. Azure İzleyici günlüklerine kullanılabilir değil tüm Azure bölgelerinde.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Portalı kullanarak Azure İzleyici günlüklerini etkinleştirme

Bu bölümde, bir Azure Log Analytics çalışma alanı işleri, hata ayıklama günlükleri izlemek üzere kullanmak için mevcut bir HDInsight Hadoop kümesi yapılandırın.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol menüden **tüm hizmetleri**.

3. Altında **ANALYTICS**seçin **HDInsight kümeleri**.

4. Kümenizi listeden seçin.

5. Soldan altında **izleme**seçin **Operations Management Suite**.

6. Ana görünümünde altında **OMS izleme**seçin **etkinleştirme**.

7. Gelen **bir çalışma alanı seçin** aşağı açılan listesinde, mevcut bir Log Analytics çalışma alanını seçin.

8. **Kaydet**’i seçin.  Ayarı kaydetmek için birkaç dakika sürer.

    ![HDInsight kümeleri için izlemeyi etkinleştirin](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "HDInsight kümeleri için izlemeyi etkinleştir")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Azure PowerShell kullanarak Azure İzleyici günlüklerini etkinleştirme

Azure İzleyici günlüklerine Az Azure PowerShell modülünü kullanarak etkinleştirebilirsiniz [etkinleştir AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightoperationsmanagementsuite) cmdlet'i.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables Operations Management Suite
Enable-AzHDInsightOperationsManagementSuite -ResourceGroupName $resourceGroup -Name $cluster -WorkspaceId $WorkspaceId -PrimaryKey $PrimaryKey
```

Devre dışı bırakmak için kullanımı [devre dışı bırak AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightoperationsmanagementsuite) cmdlet:

```powershell
Disable-AzHDInsightOperationsManagementSuite -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>HDInsight küme yönetim çözümlerini yükleme

HDInsight için Azure İzleyici günlüklerine ekleyebileceğiniz kümeye özgü yönetim çözümleri sağlıyor. [Yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md) ek veri ve analiz araçları sağlayarak Azure İzleyici günlüklerine, işlevsellik ekleyin. Bu çözümlerin, HDInsight kümelerinizi önemli performans ölçümlerini toplamak ve ölçümlerini arama için araçlar sağlar. Bu çözümleri ayrıca görselleştirmeler ve panolar için HDInsight içinde desteklenen çoğu küme türleri sağlar. Topladığınız ölçümleri çözümle birlikte kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz.

Var olan HDInsight çözümlerinin şunlardır:

* HDInsight Hadoop İzleme
* HDInsight HBase İzleme
* HDInsight etkileşimli sorgu izleme
* HDInsight Kafka İzleme
* HDInsight Spark İzleme
* HDInsight Storm Monitoring

Bir yönetim çözümü yüklemek yönergeler için bkz. [Azure yönetim çözümlerine](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Denemeler yapmak için bir HDInsight Hadoop izleme çözümü yükleyin. İşlem tamamlandığında, gördüğünüz bir **HDInsightHadoop** kutucuğu altında listelenen **özeti**. Seçin **HDInsightHadoop** Döşe. HDInsightHadoop çözüm şuna benzer:

![HDInsight izleme çözüm görünümü](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Kümeye yeni bir küme olduğundan, herhangi bir etkinlik raporu göstermez.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerinizi izlemek için sorgu Azure izleme günlükleri](hdinsight-hadoop-oms-log-analytics-use-queries.md)
