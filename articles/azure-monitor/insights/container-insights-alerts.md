---
title: Kapsayıcılar için Azure İzleyici'yi kullanarak performans uyarıları oluşturma | Microsoft Docs
description: Bu makalede, bellek ve CPU kullanımı için günlük sorguları dayalı özel uyarılar oluşturmak için kapsayıcılar için Azure İzleyici kullanmayı açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: magoedte
ms.openlocfilehash: ebe2c2b488e3d71597dd24f5504a14dd7ce6671e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59791550"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici'de performans sorunları için uyarılar ayarlama
Kapsayıcılar için Azure İzleyici, Azure Container Instances veya yönetilen için barındırılan Kubernetes kümeleri için Azure Kubernetes Service'teki (AKS) dağıtılan kapsayıcı iş yüklerinin performansını izler.

Bu makalede aşağıdaki durumlar için uyarılar etkinleştirme:

* Küme düğümlerinde CPU veya bellek kullanımı, tanımlı bir eşiği aştığında
* Bir denetleyici içinde herhangi bir kapsayıcı CPU veya bellek kullanımı karşılık gelen kaynak üzerinde ayarlanmış olan bir sınır göre tanımlanan bir eşiği aştığında
* *NotReady* durumu düğümünde sayar
*  *Başarısız*, *bekleyen*, *bilinmeyen*, *çalıştıran*, veya *başarılı* aşaması pod sayıları

Yüksek CPU veya bellek kullanımı küme düğümlerinde uyarmak için ölçüm uyarısı veya bir ölçüm ölçüsü uyarı oluşturmak için sağlanan sorgu kullanın. Ölçüm uyarıları günlük uyarıları daha düşük gecikme süresi vardır. Ancak, gelişmiş sorgulama ve daha gelişmiş algoritmaların günlük uyarıları sağlar. Günlük uyarıları kullanarak sorgular için geçerli bir datetime karşılaştırma *artık* işleci ve bir saat geri giderek. (Kapsayıcılar için azure İzleyici, tüm tarihleri Eşgüdümlü Evrensel Saat (UTC) biçiminde depolar.)

Azure İzleyici uyarılarla ilgili bilgi sahibi değilseniz bkz [Microsoft azure'da uyarılara genel bakış](../platform/alerts-overview.md) başlamadan önce. Günlük sorguları kullanan uyarılar hakkında daha fazla bilgi edinmek için [Azure İzleyici'de günlük uyarıları](../platform/alerts-unified-log.md). Ölçüm Uyarıları hakkında daha fazla bilgi için bkz. [Azure İzleyici ölçüm uyarıları](../platform/alerts-metric-overview.md).

## <a name="resource-utilization-log-search-queries"></a>Kaynak kullanımı günlük arama sorguları
Bu bölümdeki sorguların her uyarı senaryoyu destekler. 7. adımda alışık değilseniz [uyarı oluşturma](#create-an-alert-rule) bu makalenin.

Aşağıdaki sorgu ortalama CPU kullanımı ortalama olarak dakikada üye düğümlerinin CPU kullanımının hesaplar.  

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuCapacityNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```

Aşağıdaki sorgu ortalama bellek kullanımı ortalama olarak dakikada üye düğümlerinin bellek kullanımının hesaplar.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryCapacityBytes';
let usageCounterName = 'memoryRssBytes';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
// cluster filter would go here if multiple clusters are reporting to the same Log Analytics workspace
| distinct ClusterName, Computer
| join hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime
  | where TimeGenerated >= startDateTime
  | where ObjectName == 'K8SNode'
  | where CounterName == capacityCounterName
  | summarize LimitValue = max(CounterValue) by Computer, CounterName, bin(TimeGenerated, trendBinSize)
  | project Computer, CapacityStartTime = TimeGenerated, CapacityEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer
| join kind=inner hint.strategy=shuffle (
  Perf
  | where TimeGenerated < endDateTime + trendBinSize
  | where TimeGenerated >= startDateTime - trendBinSize
  | where ObjectName == 'K8SNode'
  | where CounterName == usageCounterName
  | project Computer, UsageValue = CounterValue, TimeGenerated
) on Computer
| where TimeGenerated >= CapacityStartTime and TimeGenerated < CapacityEndTime
| project ClusterName, Computer, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize), ClusterName
```
>[!IMPORTANT]
>Aşağıdaki sorgularda yer tutucu değerlerini kullanın \<your-kümesi-adı > ve \<Denetleyici adı Sihirbazı > Küme ve denetleyici temsil etmek için. Uyarıları ayarlama, ortamınıza özgü değerlerle değiştirin.

Aşağıdaki sorgu ortalama CPU kullanımı bir denetleyicideki tüm kapsayıcıların CPU kullanımı bir denetleyicide her kapsayıcı örneği dakikada ortalama olarak hesaplar. Ölçüm, bir kapsayıcı için ayarlanan sınırı yüzdesidir.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'cpuLimitNanoCores';
let usageCounterName = 'cpuUsageNanoCores';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

Aşağıdaki sorgu tüm kapsayıcıların bir denetleyicide ortalama bellek kullanımı her kapsayıcı örneği bir denetleyicide'nın bellek kullanımı dakikada ortalama olarak hesaplar. Ölçüm, bir kapsayıcı için ayarlanan sınırı yüzdesidir.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let capacityCounterName = 'memoryLimitBytes';
let usageCounterName = 'memoryRssBytes';
let clusterName = '<your-cluster-name>';
let controllerName = '<your-controller-name>';
KubePodInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| where ClusterName == clusterName
| where ControllerName == controllerName
| extend InstanceName = strcat(ClusterId, '/', ContainerName),
         ContainerName = strcat(controllerName, '/', tostring(split(ContainerName, '/')[1]))
| distinct Computer, InstanceName, ContainerName
| join hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ObjectName == 'K8SContainer'
    | where CounterName == capacityCounterName
    | summarize LimitValue = max(CounterValue) by Computer, InstanceName, bin(TimeGenerated, trendBinSize)
    | project Computer, InstanceName, LimitStartTime = TimeGenerated, LimitEndTime = TimeGenerated + trendBinSize, LimitValue
) on Computer, InstanceName
| join kind=inner hint.strategy=shuffle (
    Perf
    | where TimeGenerated < endDateTime + trendBinSize
    | where TimeGenerated >= startDateTime - trendBinSize
    | where ObjectName == 'K8SContainer'
    | where CounterName == usageCounterName
    | project Computer, InstanceName, UsageValue = CounterValue, TimeGenerated
) on Computer, InstanceName
| where TimeGenerated >= LimitStartTime and TimeGenerated < LimitEndTime
| project Computer, ContainerName, TimeGenerated, UsagePercent = UsageValue * 100.0 / LimitValue
| summarize AggregatedValue = avg(UsagePercent) by bin(TimeGenerated, trendBinSize) , ContainerName
```

Aşağıdaki sorgu tüm düğümleri ve durumuna sahip sayıları döndürür *hazır* ve *NotReady*.

```kusto
let endDateTime = now();
let startDateTime = ago(1h);
let trendBinSize = 1m;
let clusterName = '<your-cluster-name>';
KubeNodeInventory
| where TimeGenerated < endDateTime
| where TimeGenerated >= startDateTime
| distinct ClusterName, Computer, TimeGenerated
| summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName, Computer
| join hint.strategy=broadcast kind=inner (
    KubeNodeInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | summarize TotalCount = count(), ReadyCount = sumif(1, Status contains ('Ready'))
                by ClusterName, Computer,  bin(TimeGenerated, trendBinSize)
    | extend NotReadyCount = TotalCount - ReadyCount
) on ClusterName, Computer, TimeGenerated
| project   TimeGenerated,
            ClusterName,
            Computer,
            ReadyCount = todouble(ReadyCount) / ClusterSnapshotCount,
            NotReadyCount = todouble(NotReadyCount) / ClusterSnapshotCount
| order by ClusterName asc, Computer asc, TimeGenerated desc
```
Pod aşama sayar aşağıdaki sorgunun döndürdüğü, tüm aşamalarına temel: *Başarısız*, *bekleyen*, *bilinmeyen*, *çalıştıran*, veya *başarılı*.  

```kusto
let endDateTime = now();
    let startDateTime = ago(1h);
    let trendBinSize = 1m;
    let clusterName = '<your-cluster-name>';
    KubePodInventory
    | where TimeGenerated < endDateTime
    | where TimeGenerated >= startDateTime
    | where ClusterName == clusterName
    | distinct ClusterName, TimeGenerated
    | summarize ClusterSnapshotCount = count() by bin(TimeGenerated, trendBinSize), ClusterName
    | join hint.strategy=broadcast (
        KubePodInventory
        | where TimeGenerated < endDateTime
        | where TimeGenerated >= startDateTime
        | distinct ClusterName, Computer, PodUid, TimeGenerated, PodStatus
        | summarize TotalCount = count(),
                    PendingCount = sumif(1, PodStatus =~ 'Pending'),
                    RunningCount = sumif(1, PodStatus =~ 'Running'),
                    SucceededCount = sumif(1, PodStatus =~ 'Succeeded'),
                    FailedCount = sumif(1, PodStatus =~ 'Failed')
                 by ClusterName, bin(TimeGenerated, trendBinSize)
    ) on ClusterName, TimeGenerated
    | extend UnknownCount = TotalCount - PendingCount - RunningCount - SucceededCount - FailedCount
    | project TimeGenerated,
              TotalCount = todouble(TotalCount) / ClusterSnapshotCount,
              PendingCount = todouble(PendingCount) / ClusterSnapshotCount,
              RunningCount = todouble(RunningCount) / ClusterSnapshotCount,
              SucceededCount = todouble(SucceededCount) / ClusterSnapshotCount,
              FailedCount = todouble(FailedCount) / ClusterSnapshotCount,
              UnknownCount = todouble(UnknownCount) / ClusterSnapshotCount
| summarize AggregatedValue = avg(PendingCount) by bin(TimeGenerated, trendBinSize)
```

>[!NOTE]
>Gibi belirli pod aşamalarına uyarmak için *bekleyen*, *başarısız*, veya *bilinmeyen*, sorgunun son satırı değiştirin. Örneğin, uyarı için *FailedCount* kullanın: <br/>`| summarize AggregatedValue = avg(FailedCount) by bin(TimeGenerated, trendBinSize)`

## <a name="create-an-alert-rule"></a>Uyarı kuralı oluşturma
Daha önce sağlanan günlük arama kurallarını kullanarak günlük uyarısı Azure İzleyici'de oluşturmak için aşağıdaki adımları izleyin.  

>[!NOTE]
>Kapsayıcı kaynak kullanımı için yeni bir günlük geçiş yapmanızı gerektirir bir uyarı kuralı oluşturmak için aşağıdaki yordamı uyarıları API açıklandığı [günlük uyarıları için API anahtarı tercihi](../platform/alerts-log-api-switch.md).
>

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **İzleyici** sol tarafındaki bölmeden. Altında **Insights**seçin **kapsayıcıları**.
3. Üzerinde **izlenen kümeleri** sekmesinde, listeden bir kümeyi seçin.
4. Sol tarafındaki bölmede **izleme**seçin **günlükleri** Azure İzleyici günlüklerine sayfasını açın. Yazma ve Azure Log Analytics sorgularını yürütmek için bu sayfayı kullanın.
5. Üzerinde **günlükleri** sayfasında **+ yeni uyarı kuralı**.
6. İçinde **koşul** bölümünden **her özel günlük arama \<tanımsız mantık >** özel günlük koşulu önceden tanımlanmış. **Özel günlük araması** sinyal türü doğrudan Azure İzleyici günlüklerine sayfasından bir uyarı kuralı oluşturmakta olduğumuz için otomatik olarak seçilir.  
7. Aşağıdakilerden birini yapıştırın [sorguları](#resource-utilization-log-search-queries) halinde daha önce sağlanan **arama sorgusu** alan.
8. Uyarı aşağıdaki gibi yapılandırın:

    1. Aşağı açılan **Tetikleyici** listesinden **Metrik ölçüm**'ü seçin. Ölçüm ölçüsü, bizim belirtilen eşiğin üstünde bir değere sahip sorgudaki her nesne için bir uyarı oluşturur.
    1. İçin **koşul**seçin **büyüktür**girin **75** ilk bir temel olarak **eşiği**. Veya ölçütlerinizi karşılayan farklı bir değer girin.
    1. İçinde **tetikleyici uyarı dayalı** bölümünden **ardışık ihlaller**. Aşağı açılan listesinden **büyüktür**girin **2**.
    1. Kapsayıcı CPU veya bellek kullanımı için bir uyarı altında yapılandırmak için **bulunan**seçin **ContainerName**. 
    1. İçinde **göre Evaluated** bölümünde, **süresi** değerini **60 dakika**. Kural her 5 dakikada çalıştırın ve son bir saat geçerli saatten içinde oluşturulmuş olan kayıtları döndürür. Geniş penceresi hesaplarına olası veri gecikme süresi için süre ayarlama. Sorgu hiçbir zaman içinde uyarı tetikler false negatif önlemek için veri döndüren sağlar.

9. Seçin **Bitti** uyarı kuralını tamamlayın.
10. Bir ad girin **uyarı kuralı adı** alan. Belirtin bir **açıklama** , uyarı hakkında ayrıntılar sağlar. Ve sağlanan seçeneklerden bir uygun önem derecesini seçin.
11. Hemen uyarı kuralı etkinleştirmek için varsayılan değeri kabul **oluşturulduktan sonra kuralı etkinleştir**.
12. Mevcut bir seçin **eylem grubu** veya yeni bir grup oluşturun. Bu adım, bir uyarı tetiklenir her seferinde aynı eylemleri alınacağını sağlar. Yapılandırma bağlı BT ya da DevOps operasyon ekibinin olayları yönetir.
13. Seçin **uyarı kuralı oluştur** uyarı kuralını tamamlayın. Hemen çalıştırılmaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

* Görünüm [sorgu örnekleri oturum](container-insights-analyze.md#search-logs-to-analyze-data) önceden tanımlanmış sorgular ve değerlendirme veya uyarı diğer senaryolar için özelleştirmek için örnekler hakkında bilgi edinmek için.
* Azure İzleyici ve diğer yönleri AKS kümenizi izleme hakkında daha fazla bilgi için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
