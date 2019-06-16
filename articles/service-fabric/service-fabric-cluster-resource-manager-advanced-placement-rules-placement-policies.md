---
title: Service Fabric Küme Kaynak Yöneticisi - yerleştirme ilkeleri | Microsoft Docs
description: Ek yerleştirme ilkeleri ve Service Fabric Hizmetleri için kuralları'na genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d5aea441f15cbf7a2a444439c06cd5f74a559d3f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60386474"
---
# <a name="placement-policies-for-service-fabric-services"></a>Service fabric Hizmetleri için yerleştirme ilkeleri
Yerleştirme ilkeleri, hizmet yerleşimi bazı belirli, daha az yaygın senaryolarda yönetmek için kullanılan ek kurallardır. Bu senaryolara bazı örnekler şunlardır:

- Service Fabric kümenizi gibi birden çok şirket içi veri merkezlerine veya Azure bölgelerinde coğrafi uzaklıkları yayılır.
- Birden fazla alana jeopolitik ya da yasal denetim veya ilke sahip olduğu diğer bir olay ortamınızı yayılan zorlamak için gereksinim duyduğunuz sınırları
- Büyük mesafeler veya yavaş ya da daha az güvenilir ağ bağlantıları kullanımı nedeniyle iletişim performans veya gecikme süresi konuları mevcuttur
- İş yükleri olarak bir en iyi çaba, yakınlık müşterilere veya diğer iş yükleri ile birlikte bulunan belirli tutmanız gerekir

Bu gereksinimlerin çoğu küme hata etki alanları temsil edilen kümenin fiziksel düzenini ile hizalar. 

Adresi yardımcı olan gelişmiş yerleştirme ilkeleri, bu senaryolar şunlardır:

1. Geçersiz etki alanları
2. Gerekli etki alanları
3. Tercih edilen etki alanları
4. Çoğaltma paketleme izin vermeme

Aşağıdaki denetimleri çoğu düğüm özellikleri ve yerleştirme kısıtlamaları yapılandırılabilir, ancak bazı daha karmaşıktır. Şeyleri daha basit hale getirmek için Service Fabric küme kaynak yöneticisi bu ek yerleştirme ilkeleri sağlar. Yerleştirme ilkeleri hizmet başına adlı örnek olarak yapılandırılır. Bunlar ayrıca dinamik olarak güncelleştirilebilir.

## <a name="specifying-invalid-domains"></a>Geçersiz etki alanı belirtme
**InvalidDomain** yerleştirme İlkesi belirli bir hata etki alanı için belirli bir hizmet geçersiz olduğunu belirtmenize olanak verir. Bu ilke, belirli bir hizmet asla jeopolitik ya da şirket ilkesi nedenlerle örneğin belirli bir alandaki çalıştığını sağlar. Birden çok geçersiz etki alanı ayrı ilkeler belirtilebilir.

<center>

![Geçersiz etki alanı örneğinde][Image1]
</center>

Kod:

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a>Gerekli etki alanı belirtme
Gerekli etki alanı yerleştirme İlkesi Hizmeti yalnızca belirtilen etki alanında olmasını gerektirir. Birden çok gerekli etki alanı ayrı ilkeler belirtilebilir.

<center>

![Gerekli etki alanı örneğinde][Image2]
</center>

Kod:

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Durum bilgisi olan hizmet, birincil çoğaltmalara için tercih edilen bir etki alanı belirtme
Tercih edilen birincil etki alanı birincil olarak yerleştirmek için hata etki alanını belirtir. Her şey iyi durumda olduğunda birincil bu etki alanında sona erer. Birincil etki alanı ya da birincil çoğaltma başarısız veya kapatıldığında, başka bir konuma, ideal olarak, aynı etki alanında taşır. Bu yeni konumu tercih edilen etki alanında değilse, Küme Kaynak Yöneticisi, geri tercih edilen etki olabildiğince çabuk taşır. Doğal olarak bu ayar yalnızca durum bilgisi olan hizmetler için anlamlıdır. Bu ilke Azure bölgeleri arasında dağıtılmış kümelerdeki en kullanışlı veya birden çok veri merkezinde, ancak belirli bir konuma yerleştirme tercih Hizmetleri sahip. Seçimlerine kullanıcıları veya diğer hizmetlere yakın tutulması yardımcı daha düşük gecikme süresi, özellikle okuma, varsayılan olarak seçimlerine göre işlenir sağlayın.

<center>

![Tercih edilen birincil etki alanları ve yük devretme][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(primaryDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Çoğaltma dağıtım gerektirme ve paketleme izin vermeme
Çoğaltmalar _normalde_ kümenin iyi durumda olduğunda, hata ve yükseltme etki alanları arasında dağıtılmış. Ancak, belirli bir bölüm için birden fazla çoğaltma burada bitirebilirsiniz durumlar vardır tek bir etki alanına geçici olarak paketlenmiş. Örneğin, küme üç hata etki alanları, fd dokuz düğümleri olduğunu varsayalım: 0, / fd: / 1 ve fd: / 2. Ayrıca, hizmetinize üç çoğaltması olduğunu varsayalım. Varsayalım, fd Bu çoğaltma için kullanılan düğümler: / 1 ve fd: / 2'e düştü. Normalde küme kaynak yöneticisi bu aynı hata etki alanları içindeki diğer düğümlere tercih edebilirsiniz. Bu durumda, kapasite sorunları nedeniyle bu etki alanları diğer düğümlere hiçbiri geçerli varsayalım. Küme Kaynak Yöneticisi yinelemeler için değiştirmeler oluşturursa, bu düğümler fd içinde seçmeniz gerekir: / 0. Ancak, yapılması _,_ nerede hata etki alanı kısıtlamasını ihlal bir durum oluşturur. Çoğaltmaları arttıkça paketleme tam çoğaltma kümesi olasılığını yüklenemedi aşağı git veya kaybolur. 

> [!NOTE]
> Kısıtlamalar ve kısıtlama hakkında daha fazla bilgi için öncelikleri genel olarak kullanıma [bu konuda](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Şimdiye kadar bir sistem durumu ileti gibi gördüğünüz varsa "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", bu koşul veya bir şeyi beğendim ziyaret sonra. Genellikle yalnızca bir veya iki çoğaltmaları paketlenmiş birlikte geçici olarak. Vardır sürece belirli bir etki alanındaki bir çekirdeği daha az güvenli. Paketleme nadirdir, ancak olabilir ve bu yana düğümlerin dönerek bu durumlar genellikle geçici olmasıdır. Düğümleri basılı kal ve değiştirmeler derlemek Küme Kaynak Yöneticisi gerekiyorsa genellikle kullanılabilir diğer düğümlere ideal hata etki alanları.

Bunlar daha az etki alanlarına paketlenir bile bazı iş yükleri çoğaltmalar, hedef sayısı her zaman sahip tercih edebilir. Bu iş yükleri, toplam eşzamanlı kalıcı etki alanını hatalarına karşı oyunlarda ve genellikle yerel durumunu kurtarabilirsiniz. Diğer iş yüklerini kapalı kalma süresi yerine risk doğruluk ya da veri kaybı'den önceki sürecektir. Üretim iş yüklerinin çoğu üçten fazla çoğaltmaları üçten fazla hata etki alanları ve hata etki alanı başına çok sayıda geçerli düğüm ile çalıştırın. Bu nedenle, varsayılan davranışı varsayılan olarak etki alanı paketleme sağlar. Geçici bir etki alanı paketleme anlamı olsa bile varsayılan davranışı normal Dengeleme ve yük devretme aşırı bu durumları idare etmek sağlar.

Belirtebileceğiniz gibi paketleme belirli bir iş yükü için devre dışı bırakmak isterseniz, `RequireDomainDistribution` hizmet ilkesi. Bu ilkeyi ayarladığınızda, Küme Kaynak Yöneticisi aynı bölüme iki hiçbir çoğaltmalardan aynı hata veya yükseltme etki alanında çalışmasını sağlar.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Şimdi, bu yapılandırmalar değil coğrafi olarak dağıtılmış bir kümede hizmetlerin kullanmak mümkün olabilir? Yapabilirsiniz, ancak yok iyi bir nedeniniz çok. Gerekli, geçersiz ve tercih edilen etki alanı yapılandırmaları, bunları senaryoları gereksinim duymadıkça kaçınılmalıdır. Bu iş yüküne verilmesi tek bir rafa çalıştırmak için veya başka bir yerel kümenizi bazı segmentini tercih etmesini zorlamak için hiçbir mantıklı değildir. Farklı donanım yapılandırmalarını hata etki alanlarına ve normal yerleştirme kısıtlamaları ve düğüm özellikleri işlenir.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
