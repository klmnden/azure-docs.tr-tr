---
title: Sorgu günlüklerini kapsayıcılar için nasıl Azure İzleyicisi'nden | Microsoft Docs
description: Kapsayıcılar için Azure İzleyici ölçümleri ve günlük verileri toplar ve bu makalede kayıtları ve örnek sorgular içerir.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: 66fc55d8c3dbb8487d1e796d5f30b08a94f717f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60494780"
---
# <a name="how-to-query-logs-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici günlüklerinden sorgulama
Kapsayıcılar için Azure İzleyici, kapsayıcı konağında ve kapsayıcıları performans ölçümleri, Envanter verileri ve sistem durumu bilgilerini toplar ve Log Analytics çalışma alanına Azure İzleyici'de iletir. Üç dakikada bir toplanan veriler. Bu veriler için kullanılabilir [sorgu](../../azure-monitor/log-query/log-query-overview.md) Azure İzleyici'de. Geçiş planlaması kapasite analizi, bulma ve isteğe bağlı performans sorunlarını giderme senaryoları için bu verileri uygulayabilirsiniz.

## <a name="container-records"></a>Kapsayıcı kayıt

Kapsayıcılar ve günlük arama sonuçlarında görünmesini veri türleri için Azure İzleyici tarafından toplanan kayıtları örnekleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Bir Kubernetes kümesinin pod envanterini | `KubePodInventory` | TimeGenerated, bilgisayar, Lclusterıd, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, HizmetAdı, ControllerKind, ControllerName, ContainerStatus  ContainerStatusReason, Containerıd, kapsayıcı adı, ad, PodLabel, Namespace, PodStatus, ClusterName, Podıp, Analytics'teki |
| Bir Kubernetes kümesinin düğümleri bölümünün envanteri | `KubeNodeInventory` | TimeGenerated, bilgisayar, ClusterName, Lclusterıd, LastTransitionTimeReady, etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, Analytics'teki | 
| Kubernetes olayları | `KubeEvents` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, Analytics'teki | 
| Kubernetes kümesindeki Hizmetleri | `KubeServices` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, Analytics'teki | 
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;cpuAllocatableBytes, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes, memoryRssBytes, cpuUsageNanoCores, memoryWorkingsetBytes, restartTimeEpoch&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 
| Kubernetes kümesine kapsayıcıları parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SContainer" == | CounterName &#40; cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryWorkingSetBytes, restartTimeEpoch, cpuUsageNanoCores, memoryRssBytes&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 
| InsightsMetrics | Bilgisayar adı, Namespace, kaynak, Analytics'teki, etiketler, TimeGenerated, türü, değer |

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklerinde arama yapma
Azure İzleyici günlüklerine eğilimlerini arayın, performans sorunlarını tanılamanıza yardımcı olur, yardımcı olabilecek tahmin ve performanstaki veri geçerli küme yapılandırmasını en uygun şekilde çalışıp çalışmadığını belirleyin. Önceden tanımlanmış günlük aramaları, hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır. 

Etkileşimli veri analizi seçerek çalışma alanında gerçekleştirebilirsiniz **görünümü Kubernetes olay günlüklerini** veya **kapsayıcı günlüklerini görüntüleme** önizleme bölmesinde seçeneği. **Günlük araması** bulunduğunuz Azure portal sayfasının sağındaki sayfası görüntülenir.

![Log Analytics’te verileri analiz etme](./media/container-insights-analyze/container-health-log-search-example.png)   

Çalışma alanınıza iletilen kapsayıcı günlüklerini çıktısı, STDOUT ve STDERR alınır. Azure İzleyici, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "cpuUsageNanoCores" == &#124; AvgCPUUsageNanoCores özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Çizgi grafik görüntüleme seçeneğini**:<br> Perf<br> &#124;Burada ObjectName "K8SContainer" ve CounterName == "memoryRssBytes" == &#124; AvgUsedRssMemoryBytes özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |

## <a name="next-steps"></a>Sonraki adımlar
Kapsayıcılar için Azure İzleyici, önceden tanımlanmış birtakım uyarıları içermez. Gözden geçirme [performans uyarıları, kapsayıcılar için Azure İzleyici ile oluşturma](container-insights-alerts.md) DevOps veya çalışma süreçleri ve yordamları desteklemek yüksek CPU ve bellek kullanımı için önerilen uyarılar oluşturma hakkında bilgi edinmek için. 