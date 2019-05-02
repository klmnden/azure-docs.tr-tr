---
title: İzleme ve tanılama Azure Service Fabric Mesh uygulamalarında | Microsoft Docs
description: Service Fabric Mesh azure'da uygulama tanılama ve izleme hakkında bilgi edinin.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2019
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 36c9a5d75c4a72365638619ab85d451df647feb3
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939817"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Service Fabric Mesh için izleme ve Tanılama, Tanılama verileri üç temel tür olarak kategorilere:

- Uygulama günlükleri - bunlar uygulamanızı nasıl izlenen üzerinde tabanlı uygulamalarınızı kapsayıcılı günlüklerinden olarak tanımlanır (örneğin docker günlükleri)
- Platform olaylar - kafes platformu, kapsayıcı işlemine, ilgili olaylar şu anda sonlandırma kapsayıcı etkinleştirme ve devre dışı bırakma dahil.
- Kapsayıcı ölçümleri - kapsayıcılarınızı (docker istatistikleri) için kaynak kullanımı ve performans ölçümleri

Bu makalede, en son Önizleme sürümü için izleme ve tanılama seçenekleri ele alınmaktadır.

## <a name="application-logs"></a>Uygulama günlükleri

Kapsayıcı başına temelinde dağıtılan, kapsayıcılardan docker günlüklerinizi görüntüleyebilirsiniz. Service Fabric Mesh uygulama modeli, her kapsayıcı, uygulamanızdaki bir kod paketidir. Kod paketi ile ilişkili günlükleri görmek için aşağıdaki komutu kullanın:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Çoğaltma adı almak için "az mesh hizmeti çoğaltması" komutunu kullanabilirsiniz. Çoğaltma adları 0 tamsayı artırımlı sayılardır.

Oylama uygulamasını VotingWeb.Code kapsayıcısından günlüklerinden görmek için göründüğüne aşağıda verilmiştir:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="container-metrics"></a>Kapsayıcı ölçümleri 

Kafes ortam birkaç kapsayıcılarınızı performansını gösteren ölçümleri sunar. Aşağıdaki ölçümler, Azure aracılığıyla kullanılabilir portalı ve Azure CLI izleyin:

| Ölçüm | Açıklama | Birimler|
|----|----|----|
| CpuUtilization | ActualCpu/AllocatedCpu yüzdesi | % |
| MemoryUtilization | ActualMem/AllocatedMem yüzdesi | % |
| AllocatedCpu | Azure Resource Manager şablonu göre ayrılan Cpu | Millicores |
| AllocatedMemory | Azure Resource Manager şablonu göre ayrılmış bellek | MB |
| ActualCpu | CPU kullanımı | Millicores |
| ActualMemory | Bellek kullanımı | MB |
| ContainerStatus | 0 - geçersiz: Kapsayıcı durumu bilinmiyor <br> 1 - bekliyor: Kapsayıcı başlatmak için zamanlanmış <br> 2 - başlatılıyor: Başlatma işleminde kapsayıcıdır <br> 3 - başlangıç zamanı: Kapsayıcı başarıyla başlatıldı <br> 4 - durduruluyor: Kapsayıcı durduruluyor <br> 5 - durduruldu: Kapsayıcı başarıyla durduruldu | Yok |
| ApplicationStatus | 0 - bilinmeyen: Durumu alınabilir değil <br> 1 - hazır: Uygulama başarıyla çalışıyor <br> 2 - yükseltiliyor: Devam eden bir yükseltme <br> 3 - oluşturuluyor: Uygulama oluşturuluyor <br> 4 - siliniyor: Uygulama siliniyor <br> 5 - başarısız oldu: Uygulamanın dağıtımı başarısız oldu | Yok |
| ServiceStatus | 0 - geçersiz: Hizmet şu anda bir sistem durumu yok. <br> 1 - Tamam: Hizmet kötü durumda  <br> 2 - Uyarı: Olabilir bir araştırma gerektiren sorun <br> 3 - hata: Araştırma gerekiyor, yanlış bir şey yok <br> 4 - bilinmeyen: Durumu alınabilir değil | Yok |
| ServiceReplicaStatus | 0 - geçersiz: Çoğaltma sistem durumu şu anda yok <br> 1 - Tamam: Hizmet kötü durumda  <br> 2 - Uyarı: Olabilir bir araştırma gerektiren sorun <br> 3 - hata: Araştırma gerekiyor, yanlış bir şey yok <br> 4 - bilinmeyen: Durumu alınabilir değil | Yok | 
| RestartCount | Kapsayıcı yeniden başlatma sayısı | Yok |

> [!NOTE]
> ServiceStatus ve ServiceReplicaStatus aynı değerler [HealthState](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet) Service fabric'te. 

Farklı düzeylerde toplamalar görebilmeniz için her ölçüm farklı boyutlarında kullanılabilir. Geçerli boyut listesini aşağıdaki gibidir:

* ApplicationName
* ServiceName
* ServiceReplicaName
* CodePackageName

> [!NOTE]
> CodePackageName boyut Linux uygulamaları için kullanılabilir değil. 

Her boyut için farklı bileşenleri karşılık gelen [Service Fabric uygulama modeli](service-fabric-mesh-service-fabric-resources.md#applications-and-services)

### <a name="azure-monitor-cli"></a>Azure İzleyici CLI

Komutların tam listesi kullanılabilir [Azure İzleyici CLI belgeleri](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) ancak aşağıdaki bazı yararlı örnekler ekledik. 

Her örnekte kaynak kimliği şu desene uygun olmalıdır.

`"/subscriptions/<your sub ID>/resourcegroups/<your RG>/providers/Microsoft.ServiceFabricMesh/applications/<your App name>"`


* Bir uygulamada kapsayıcıların CPU kullanımı

```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization"
```
* Her hizmet çoğaltması için bellek kullanımı
```cli
    az monitor metrics list --resource <resourceId> --metric "MemoryUtilization" --dimension "ServiceReplicaName"
``` 

* 1 saat penceresinde her kapsayıcı için yeniden başlatma 
```cli
    az monitor metrics list --resource <resourceId> --metric "RestartCount" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z
``` 

* Hizmetler genelinde ortalama CPU kullanımı 1 saat penceresinde "VotingWeb" adlı
```cli
    az monitor metrics list --resource <resourceId> --metric "CpuUtilization" --start-time 2019-02-01T00:00:00Z --end-time 2019-02-01T01:00:00Z --aggregation "Average" --filter "ServiceName eq 'VotingWeb'"
``` 

### <a name="metrics-explorer"></a>Ölçüm Gezgini

Ölçüm Gezgini kafes uygulamanız için tüm ölçümleri görselleştirebilir miyim Portalı'nda bir dikey pencere ' dir. Portal ile hangi Azure İzleyicisi'ni destekleyen tüm Azure kaynaklarınız için ölçümleri görüntülemek için kullanabileceğiniz Azure İzleyici dikey penceresinde, ikinci sayfasında uygulamanın bu dikey pencereyi erişilebilir. 

![Ölçüm Gezgini](./media/service-fabric-mesh-monitoring-diagnostics/metricsexplorer.png)


<!--
### Container Insights

In addition to the metrics explorer, we also have a dashboard available out of the box that shows sample metrics over time under the Insights blade in the application's page in the portal. 

![Container Insights](./media/service-fabric-mesh-monitoring-diagnostics/containerinsights.png)
-->

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric Mesh hakkında daha fazla bilgi edinmek için [Service Fabric Mesh’e genel bakış](service-fabric-mesh-overview.md) makalesini okuyun.
* Azure İzleyici ölçümleri komutlar hakkında daha fazla bilgi için kullanıma [Azure İzleyici CLI belgeleri](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list).
