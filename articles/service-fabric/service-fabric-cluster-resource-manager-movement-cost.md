---
title: 'Service Fabric kümesi Kaynak Yöneticisi: taşıma maliyeti | Microsoft Docs'
description: Taşıma maliyeti Service Fabric Hizmetleri için genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 74b61967a796fca22ab86918235f1def27a22f91
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="service-movement-cost"></a>Hizmet taşıma maliyeti
Service Fabric kümesi Kaynak Yöneticisi bir kümeye değişikliklerini ne belirlemeye çalışırken göz önünde bulundurur bir faktör bu değişiklikleri maliyetidir. "Maliyet" kavramı kapalı ne kadar küme karşı geliştirilebilir ticareti. Maliyet Dengeleme, birleştirme ve diğer gereksinimler için hizmetler taşırken hesaba katıldığında. En az kesintiye uğratan veya pahalı yolu gereksinimlerini karşılamak için belirtilir. 

Hizmetleri maliyetleri CPU süresi taşıma ve ağ bant genişliği en az. Durum bilgisi olan hizmetler için ek bellek ve disk tüketen hizmetlerin durumu kopyalama gerektiriyor. Azure Service Fabric kümesi Resource Manager ile gelir çözümleri maliyetini en aza kümenin kaynakları gereksiz yere harcanan olmayan sağlamaya yardımcı olur. Ancak, aynı zamanda kümedeki kaynaklar ayırma önemli ölçüde artırmak çözümleri yoksay istemiyorsanız.

Küme Kaynak Yöneticisi'ni maliyetleri bilgi işlem ve kümeyi yönetmek denediğinde bunları sınırlama iki yolu vardır. İlk mekanizma basit hale getirir her taşıma sayım. İki çözümleri ile aynı oluşturuluyorsa Bakiye (puan), en düşük maliyeti (taşır toplam sayısı) sahip bir küme Kaynak Yöneticisi'ni tercih sonra.

Bu strateji de çalışır. Ancak varsayılan veya statik yükleri'te olduğu gibi tüm taşır eşit herhangi karmaşık sisteminde tahmin edilemez. Bazı çok daha pahalı olması muhtemeldir.

## <a name="setting-move-costs"></a>Ayar maliyetleri taşıyın 
Varsayılan taşıma maliyeti bir hizmet oluşturulduğunda belirtebilirsiniz:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C# ' TA: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Ayrıca, belirtin veya hizmeti oluşturulduktan sonra MoveCost bir hizmet için dinamik olarak güncelleştir: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C# ' TA:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Taşıma maliyeti yineleme başına temelinde dinamik olarak belirtme

Önceki kod parçacıkları tüm hizmet dışında aynı anda tüm bir hizmetine MoveCost belirtmek için. Ancak, maliyeti en yararlı olduğu zaman taşımak, kullanım ömrü belirli hizmet nesnesi taşıma maliyeti değiştirir. Hizmetleri kendilerini büyük olasılıkla nasıl maliyetli belirli bir zaman taşımak için oldukları en iyi fikir sahip olduğundan, bir API Hizmetleri çalışma zamanı sırasında kendi tek taşıma maliyeti raporuna yoktur. 

C# ' TA:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Taşıma maliyeti etkisi
MoveCost dört düzeyi vardır: sıfır, düşük, Orta ve yüksek. MoveCosts birbirine göre dışında sıfırdır. Sıfır taşıma maliyeti hareket ücretsizdir ve çözüm puan karşı saymak değil anlamına gelir. Yüksek mu için maliyet taşınma ayarı *değil* çoğaltma tek bir yerde kalır garanti edilemez.

<center>
![Taşıma için çoğaltmalar seçerek bir etmen olarak taşıma maliyeti][Image1]
</center>

MoveCost genel en az kesintiye neden ve hala eşdeğer tarihe ulaşan çalışırken elde etmek kolay çözümler bulmanıza yardımcı olur. Bir hizmetin maliyeti kavramı pek çok göreli olabilir. Taşıma maliyeti hesaplama en yaygın unsurlar şunlardır:

- Durum veya taşımak için hizmet olan verilerin miktarı.
- İstemci bağlantısının kesilmesi maliyeti. Birincil çoğaltma taşıma genellikle daha bir ikincil çoğaltma taşıma maliyeti daha pahalıdır.
- Yürütülen bir işlem kesintiye maliyeti. Bazı işlemler veri düzeyini depolamak veya yanıt olarak bir istemci çağrısı gerçekleştirilen işlemler maliyetlidir. Belirli bir bir noktadan sonra için yoksa sonlandırmasına istemezsiniz. Bu nedenle işlemi sürerken taşıdığında olasılığını azaltmak için bu hizmeti nesnesinin taşıma maliyetini artırır. İşlemi bittiğinde, maliyet geri normal ayarlayın.

## <a name="enabling-move-cost-in-your-cluster"></a>Taşıma maliyeti kümenizdeki etkinleştirme
Dikkate alınması daha ayrıntılı MoveCosts için sırayla MoveCost kümenizdeki etkinleştirilmesi gerekir. Bu ayar olmadan taşır sayım varsayılan mod MoveCost hesaplamak için kullanılır ve MoveCost raporları göz ardı edilir.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json ya da Azure için Template.json aracılığıyla kümeleri barındırılan:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric kümesi Kaynak Yöneticisi ölçümleri kullanım ve kapasite kümedeki yönetmek için kullanır. Ölçümleri ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi için kullanıma [yönetme kaynak tüketimini ve Service Fabric yük ölçümlerle](service-fabric-cluster-resource-manager-metrics.md).
- Küme Kaynak Yöneticisi'ni nasıl yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için kullanıma [Dengeleme Service Fabric kümesi](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
