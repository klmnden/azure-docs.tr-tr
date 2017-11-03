---
title: "Service Fabric kümesi Kaynak Yöneticisi - yerleşim ilkeleri | Microsoft Docs"
description: "Ek yerleşim ilkeleri ve hizmet doku hizmetler için kuralları genel bakış"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c240643d2a7ce98ddd7f7871eeef654cced953f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="placement-policies-for-service-fabric-services"></a>Service fabric Hizmetleri için yerleşim ilkeleri
Yerleşim ilkeleri, hizmet yerleşimi bazı belirli, daha az yaygın senaryolarda yönetmek için kullanılan ek kurallardır. Bu senaryolarda bazı örnekleri şunlardır:

- Service Fabric kümesi coğrafi uzaklıkları, birden çok şirket içi veri merkezleri gibi veya Azure bölgelere yayılan
- Birden çok alanlar jeopolitik veya yasal denetim veya ilke sahip olduğu diğer bir durum ortamınızı yayılan zorlamak için ihtiyacınız sınırları
- Büyük uzaklıklar veya yavaş bir veya daha az güvenilir ağ bağlantıların kullanımı nedeniyle iletişimi performans veya gecikme noktalar vardır
- İş yükleri bir en iyi deneme ya da diğer iş yükleri ile müşteriler bir yerde konumlandırıldığında olarak birlikte bulunan belirli tutmanız gerekir

Küme hata etki alanları gösterilen, küme fiziksel düzenini ile bu gereksinimlerinin birçoğu hizalayın. 

Adres Yardım Gelişmiş yerleşim ilkeleri bu senaryolar şunlardır:

1. Geçersiz etki alanları
2. Gerekli etki alanları
3. Tercih edilen etki alanları
4. Çoğaltma paketleme izin vermeme

Aşağıdaki denetimleri çoğunu düğüm özellikleri ve kısıtlamalarından aracılığıyla yapılandırılabilir, ancak bazı daha karmaşıktır. Şeyleri daha basit hale getirmek için Service Fabric kümesi kaynak yöneticisi bu ek yerleşim ilkeleri sağlar. Yerleşim ilkeleri tek başına adlı hizmet örneği temelinde yapılandırılır. Bunlar ayrıca dinamik olarak güncelleştirilebilir.

## <a name="specifying-invalid-domains"></a>Geçersiz etki alanları belirtme
**InvalidDomain** yerleştirme ilkesi için belirli bir hizmeti belirli bir hata etki alanı geçersiz belirtmenize olanak verir. Bu ilke belirli bir hizmet hiçbir zaman jeopolitik veya şirket ilkesi nedenlerle örneğin belirli bir alandaki çalıştırmasını sağlar. Birden çok geçersiz etki alanı ayrı ilkeler belirtilebilir.

<center>
![Geçersiz etki alanı örneği][Image1]
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
## <a name="specifying-required-domains"></a>Gerekli etki alanları belirtme
Gerekli etki alanı yerleştirme İlkesi Hizmeti yalnızca belirtilen etki alanında mevcut olmasını gerektirir. Birden çok gerekli etki alanı ayrı ilkeler belirtilebilir.

<center>
![Gerekli etki alanı örneği][Image2]
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

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a>Tercih edilen bir etki alanı için bir durum bilgisi olan hizmet birincil çoğaltmalarının belirtme
Tercih edilen birincil etki alanı birincil yerleştirmek için hata etki alanını belirtir. Her şeyi iyi durumda olduğunda birincil bu etki alanında sona erer. Etki alanı veya birincil çoğaltma başarısız olur ya da kapatıldığında, birincil ideal olarak aynı etki alanındaki diğer bazı konuma taşır. Bu yeni konumu tercih edilen etki alanında değilse, Küme Kaynak Yöneticisi'ni, geri tercih edilen etki alanı için mümkün olan en kısa sürede taşır. Doğal olarak bu ayar yalnızca durum bilgisi olan hizmetler için anlamlıdır. Birden çok veri merkezi bir konumda yerleştirme tercih Hizmetleri ancak sahip veya bu ilkeyi Azure bölgeler arasında yayılmış kümelerinde en yararlı olur. Ana, kullanıcılar veya diğer hizmetler yakın tutarak yardımcı daha düşük gecikme süresi, özellikle okuma, ana tarafından varsayılan olarak işlenir sağlar.

<center>
![Tercih edilen birincil etki alanları ve yük devretme][Image3]
</center>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a>Çoğaltma dağıtım gerektiren ve paket vermemek
Çoğaltmaları _normalde_ küme iyi durumda olduğunda hata ve yükseltme etki alanları arasında dağıtılmış. Ancak, belirli bir bölüm için birden fazla çoğaltma burada anlamayabilir durumlar vardır tek bir etki alanına geçici olarak paketlenmiş. Örneğin, küme üç hata etki alanlarında, fd dokuz düğümleri olduğunu varsayalım: / 0, fd: / 1 ve fd: / 2. Ayrıca hizmetinizi üç çoğaltmaları olduğunu düşünelim. Düşünelim, fd içinde bu çoğaltmalar için kullanılmakta düğümler: / 1 ve fd: / 2 kapandı. Normalde Küme Kaynak Yöneticisi'ni bu aynı hata etki alanları içindeki diğer düğümlere tercih edebilir. Bu durumda, kapasite sorunları nedeniyle bu etki alanlarında diğer düğümlerin hiçbiri geçerli varsayalım. Küme Kaynak Yöneticisi'ni yinelemeler donanımlarının oluşturursa, bu düğümler fd seçmeniz gerekir: / 0. Ancak, bulunurken _,_ nerede hata etki alanı kısıtlaması ihlal bir durum oluşturur. Çoğaltmaları artar paketlemeyi tüm çoğaltma kümesi fırsat aşağı git veya kaybolur. 

> [!NOTE]
> Kısıtlamalar ve kısıtlama hakkında daha fazla bilgi için öncelikleri genellikle kullanıma [bu konuda](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).
>

Herhangi bir zamanda bir sistem durumu ileti gibi gördüğünüz ise "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", bu koşulu veya onu şöyle ziyaret sonra. Genellikle yalnızca bir veya iki çoğaltmaları paketlenmiş birlikte geçici olarak. Vardır sürece çoğaltmalarının belirli bir etki alanındaki bir çekirdek daha az, güvenli. Paketleme nadir, ancak olabilir ve bu gibi durumlarda düğümleri geri dönmeden bu yana genellikle geçicidir. Düğümleri kapalı kalmasını ve değişiklik oluşturmak Küme Kaynak Yöneticisi'ni gerekirse genellikle yok diğer düğümlere ideal hata etki alanı.

Bunlar daha az etki alanlarına paketlenmiş bile, bazı iş yükleri çoğaltmalar, hedef sayısını her zaman sahip tercih edebilir. Bu iş yükleri toplam eşzamanlı kalıcı etki alanı arızalarına karşı beklemek ve genellikle yerel durumu kurtarabilirsiniz. Diğer iş yüklerini yerine kapalı kalma riskini doğruluk veya veri kaybı'den önceki alır. Çoğu üretim iş yükleri üçten fazla çoğaltmalar, üçten fazla hata etki alanları ve hata etki alanı başına çok sayıda geçerli düğüm çalıştırın. Bu nedenle, varsayılan davranışı varsayılan olarak etki alanı paketleme sağlar. Geçici etki alanı paketleme anlamına olsa bile varsayılan davranış normal yük dengeleme ve bu gibi olağanüstü durumlarda, işlemek için yük devretme sağlar.

Bu tür paketleme belirli bir iş yükü için devre dışı bırakmak isterseniz, belirtebilirsiniz `RequireDomainDistribution` hizmet ilkesi. Bu ilkeyi ayarladığınızda, Küme Kaynak Yöneticisi'ni aynı hatası veya yükseltme etki alanında aynı bölüm iki hiçbir çoğaltmalardan çalıştırmak sağlar.

Kod:

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

Şimdi, bu yapılandırmalar değil coğrafi olarak dağıtılmış bir küme hizmetlerini kullanmak mümkün olur? Olabilir, ancak yok. büyük bir nedenle çok. Onları gerektiren senaryolar sürece gerekli, geçersiz ve tercih edilen etki alanı yapılandırmaları kaçınılmalıdır. Tek bir rafa çalıştırmak veya bazı segment yerel kümenizin başka bir tercih için belirli bir iş yükü zorlamak denemek için herhangi bir algılama yapmaz. Farklı donanım yapılandırmaları hata etki alanlarında yayılan ve normal kısıtlamalarından ve düğüm özellikleri aracılığıyla işlenen.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
