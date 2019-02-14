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
ms.date: 02/12/2019
ms.author: magoedte
ms.openlocfilehash: c275d50ab927895eca3aa018b71ab6989a4dde5a
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56238861"
---
# <a name="how-to-set-up-alerts-in-azure-monitor-for-container-performance-problems"></a>Azure İzleyici'de uyarılar için kapsayıcı performans sorunlarını kurma
Azure İzleyici kapsayıcıları izlemeleri için kapsayıcı iş yüklerinin performansını ya da Azure Container Instances'a dağıtılabilir veya Azure Kubernetes Service (AKS) barındırılan Kubernetes kümelerini yönetilen. 

Bu makalede, küme düğümlerinde işlemci ve bellek kullanımı, tanımlı bir eşiği aştığında uyarı vermeyi etkinleştirmek açıklar.

## <a name="create-alert-on-cluster-cpu-or-memory-utilization"></a>Küme CPU veya bellek kullanımı uyarısı oluşturma
CPU veya bellek kullanımı için bir küme yüksek olduğunda sizi uyarmak için sağlanan günlük sorguları dışına dayalı bir ölçüm ölçüsü uyarı kuralı oluşturun. Sorguları kullanarak mevcut bir datetime karşılaştırma `now` işleci ve bir saat geri gider. Kapsayıcılar için Azure İzleyici tarafından depolanan tüm tarihler UTC biçimindedir.  

Başlatmadan önce Azure İzleyici'de uyarılar alışkın değilseniz bkz [Microsoft azure'da uyarılara genel bakış](../platform/alerts-overview.md). Günlük sorguları kullanarak Uyarıları hakkında daha fazla bilgi için bkz: [Azure İzleyici'de günlük uyarıları](../platform/alerts-unified-log.md)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **İzleyici** Azure portalının sol bölmesinden. Altında **Insights** bölümünden **kapsayıcıları**.    
3. Gelen **izlenen kümeleri** sekmesinde, kümenin adına tıklayarak listeden bir küme seçin.
4. Altındaki sol bölmede **izleme** bölümünden **günlükleri** Azure İzleyicisi'ni açmak için yazma ve Azure Log Analytics sorgularını yürütmek için kullanılan sayfa günlüğe kaydeder.
5. Üzerinde **günlükleri** sayfasında **+ yeni uyarı kuralı**.
6. Altında **koşul** bölümünde, önceden tanımlanmış özel günlük koşulu tıklayın **her özel günlük arama <logic undefined>** . **Özel günlük araması** sinyal türü otomatik olarak seçilir bizim için doğrudan Azure İzleyici günlüklerine sayfasından bir uyarı kuralı oluşturma başlatılan olduğundan.  
7. Sorgulara birini yapıştırın **arama sorgusu** alan. Aşağıdaki sorgu, üye düğümler CPU kullanımı dakikada ortalama CPU kullanımı ortalama olarak hesaplar.

    ```
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

    ```
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

8. Uyarıyı aşağıdaki bilgilerle yapılandırın:

    a. Aşağı açılan **Tetikleyici** listesinden **Metrik ölçüm**'ü seçin. Bir metrik ölçümü, sorgudaki belirttiğimiz eşiği aşan her nesne için bir uyarı oluşturacaktır.  
    b. İçin **koşul**seçin **büyüktür** girin **75** ilk bir temel olarak **eşiği** veya uygun bir değer girin, ölçütleri.  
    c. Altında **tetikleyici uyarı dayalı** bölümünden **ardışık ihlaller** açılan listesini seçin ve **büyüktür** değeri girin **2**.  
    d. Altında **göre Evaluated** bölümünde, değişiklik **süresi** 60 dakika için değer. Kural her beş dakikada çalıştırın ve son bir saat geçerli saatten içinde oluşturulmuş olan kayıtları döndürür. Sürenin daha uzun tutulması veri gecikme süresini artırabilir ve uyarının hiç tetiklenmediği hatalı negatif durumlarından kaçınmak için sorgunun veri döndürmesini sağlar. 

9. **Bitti**'ye tıklayarak uyarı kuralını tamamlayın.
10. Uyarınız içinde adını sağlayın **uyarı kuralı adı** alan. Belirtin bir **açıklama** uyarı özellikleri ayrıntılı olarak açıklandığı ve sağlanan seçeneklerden uygun bir önem derecesi seçin.
11. Oluşturmadan hemen sonra uyarı kuralını etkinleştirmek için, **Oluşturulduktan sonra kuralı etkinleştir** için varsayılan değeri kabul edin.
12. Son adım için mevcut bir seçin veya yeni bir **eylem grubu**, aynı eylemleri her zaman bir uyarı tetiklenir ve tanımladığınız her kural için kullanılabilir alınır sağlar. Yapılandırma bağlı BT veya olaylar DevOps işlemlerini yönetir. 
13. Uyarı kuralını tamamlamak için **Uyarı kuralı oluştur**'a tıklayın. Hemen çalıştırılmaya başlar.

## <a name="next-steps"></a>Sonraki adımlar

* Bazı gözden [sorgu örnekleri oturum](container-insights-analyze.md#search-logs-to-analyze-data) değerlendirmek ve kullanabilir veya uyarı diğer senaryolar için özelleştirmek için örnekler ve önceden tanımlanmış sorguları hakkında bilgi edinin. 
* Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti durumu](container-insights-analyze.md)
