---
title: Performans uyarıları kapsayıcılar için Azure İzleyici ile oluşturun. | Microsoft Docs
description: Bu makalede, bellek ve kapsayıcılar için Azure İzleyici'den CPU kullanımı için günlük sorguları dayalı özel Azure uyarıları nasıl oluşturabileceğiniz açıklanır.
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
ms.date: 02/26/2019
ms.author: magoedte
ms.openlocfilehash: e6fdb0d57a44578647c1f16dc76c557296f20ddb
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886794"
---
# <a name="how-to-set-up-alerts-for-performance-problems-in-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici'de performans sorunları için uyarılar ayarlama
Azure İzleyici kapsayıcıları izlemeleri için kapsayıcı iş yüklerinin performansını ya da Azure Container Instances'a dağıtılabilir veya Azure Kubernetes Service (AKS) barındırılan Kubernetes kümelerini yönetilen. 

Bu makalede aşağıdaki durumlar için uyarı vermeyi etkinleştirmek açıklar:

* Zaman küme düğümlerinde CPU ve bellek kullanımı veya tanımlı, eşiği aşıyor.
* Ne zaman herhangi bir denetleyici kapsayıcılara CPU veya bellek kullanımı, karşılık gelen kaynağın sınırını göre tanımlanan, eşiği aşıyor.

CPU veya bellek kullanımı için bir küme ya da denetleyiciye yüksek olduğunda sizi uyarmak için sağlanan günlük sorguları dışına dayalı bir ölçüm ölçüsü uyarı kuralı oluşturun. Sorgular için şimdi işlecini kullanarak geçerli bir datetime karşılaştırın ve bir saat döner. Kapsayıcılar için Azure İzleyici tarafından depolanan tüm tarihler UTC biçimindedir.

Başlatmadan önce Azure İzleyici'de uyarılar alışkın değilseniz bkz [Microsoft azure'da uyarılara genel bakış](../platform/alerts-overview.md). Günlük sorguları kullanarak Uyarıları hakkında daha fazla bilgi için bkz: [Azure İzleyici'de günlük uyarıları](../platform/alerts-unified-log.md)

## <a name="resource-utilization-log-search-queries"></a>Kaynak kullanımı günlük arama sorguları
Sorgular, bu bölümde, her uyarı senaryoyu desteklemek için sağlanır. 7. adım altında için gerekli sorguların [uyarı oluşturma](#create-alert-rule) bölümüne bakın.  

Aşağıdaki sorgu, üye düğümler CPU kullanımı dakikada ortalama CPU kullanımı ortalama olarak hesaplar.  

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

Aşağıdaki sorgu üye düğümlerinin bellek kullanımı dakikada ortalama olarak ortalama bellek kullanımını hesaplar.

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
>Sorgularda aşağıdaki yer tutucu dize değerleri, küme ve denetleyicisi adları - var. < uygulamanızın kümesi-adı > ve < uygulamanızın denetleyicisi-adı >. Uyarıları Ayarlama önce ortamınıza özgü değerlerle yer tutucularını değiştirin. 


Aşağıdaki sorgu ortalama CPU kullanımı bir denetleyicideki tüm kapsayıcıların ortalama CPU kullanımı için bir kapsayıcı ayarlanan sınırı yüzdesi olarak dakikada bir denetleyicide her kapsayıcı örneği olarak hesaplar.

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

Aşağıdaki sorgu ortalama bellek kullanımı bir denetleyicideki tüm kapsayıcıların her kapsayıcı örneği bir denetleyicide'nın bellek kullanımı için bir kapsayıcı ayarlanan sınırı yüzdesi olarak dakikada ortalama olarak hesaplar.

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

## <a name="create-alert-rule"></a>Uyarı kuralı oluştur
Azure İzleyicisi'nde daha önce sağlanan günlük arama kurallarını kullanarak günlük uyarı oluşturmak için aşağıdaki adımları gerçekleştirin.  

>[!NOTE]
>Aşağıdaki yordamda açıklandığı gibi yeni günlük uyarıları API'sine geçiş yapmanızı gerektirir [günlük uyarıları için API anahtarı tercihi](../platform/alerts-log-api-switch.md) kapsayıcı kaynak kullanımı için uyarı kuralı oluşturuyorsanız. 
>

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **İzleyici** Azure portalının sol bölmesinden. Altında **Insights** bölümünden **kapsayıcıları**.    
3. Gelen **izlenen kümeleri** sekmesinde, kümenin adına tıklayarak listeden bir küme seçin.
4. Altındaki sol bölmede **izleme** bölümünden **günlükleri** Azure İzleyicisi'ni açmak için yazma ve Azure Log Analytics sorgularını yürütmek için kullanılan sayfa günlüğe kaydeder.
5. Üzerinde **günlükleri** sayfasında **+ yeni uyarı kuralı**.
6. Altında **koşul** bölümünde, önceden tanımlanmış özel günlük koşulu tıklayın **her özel günlük arama <logic undefined>** . **Özel günlük araması** sinyal türü otomatik olarak seçilir bizim için doğrudan Azure İzleyici günlüklerine sayfasından bir uyarı kuralı oluşturma başlatılan olduğundan.  
7. Aşağıdakilerden birini yapıştırın [sorguları](#resource-utilization-log-search-queries) halinde daha önce sağlanan **arama sorgusu** alan. 

8. Uyarıyı aşağıdaki bilgilerle yapılandırın:

    a. Aşağı açılan **Tetikleyici** listesinden **Metrik ölçüm**'ü seçin. Bir metrik ölçümü, sorgudaki belirttiğimiz eşiği aşan her nesne için bir uyarı oluşturacaktır.  
    b. İçin **koşul**seçin **büyüktür** girin **75** ilk bir temel olarak **eşiği** veya uygun bir değer girin, ölçütleri.  
    c. Altında **tetikleyici uyarı dayalı** bölümünden **ardışık ihlaller** açılan listesini seçin ve **büyüktür** değeri girin **2**.  
    d. Kapsayıcı CPU veya bellek kullanımı için bir uyarı altında yapılandırıyorsanız **bulunan** seçin **ContainerName** aşağı açılan listeden.  
    e. Altında **göre Evaluated** bölümünde, değişiklik **süresi** 60 dakika için değer. Kural her beş dakikada çalıştırın ve son bir saat geçerli saatten içinde oluşturulmuş olan kayıtları döndürür. Sürenin daha uzun tutulması veri gecikme süresini artırabilir ve uyarının hiç tetiklenmediği hatalı negatif durumlarından kaçınmak için sorgunun veri döndürmesini sağlar. 

9. **Bitti**'ye tıklayarak uyarı kuralını tamamlayın.
10. Uyarınız içinde adını sağlayın **uyarı kuralı adı** alan. Belirtin bir **açıklama** uyarı özellikleri ayrıntılı olarak açıklandığı ve sağlanan seçeneklerden uygun bir önem derecesi seçin.
11. Oluşturmadan hemen sonra uyarı kuralını etkinleştirmek için, **Oluşturulduktan sonra kuralı etkinleştir** için varsayılan değeri kabul edin.
12. Son adım için mevcut bir seçin veya yeni bir **eylem grubu**, aynı eylemleri her zaman bir uyarı tetiklenir ve tanımladığınız her kural için kullanılabilir alınır sağlar. Yapılandırma bağlı BT veya olaylar DevOps işlemlerini yönetir. 
13. Uyarı kuralını tamamlamak için **Uyarı kuralı oluştur**'a tıklayın. Hemen çalıştırılmaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

* Bazı gözden [sorgu örnekleri oturum](container-insights-analyze.md#search-logs-to-analyze-data) değerlendirmek ve kullanabilir veya uyarı diğer senaryolar için özelleştirmek için örnekler ve önceden tanımlanmış sorguları hakkında bilgi edinin. 
* Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti durumu](container-insights-analyze.md)
