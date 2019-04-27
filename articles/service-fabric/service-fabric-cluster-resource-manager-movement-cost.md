---
title: 'Service Fabric Küme Kaynak Yöneticisi: Taşıma maliyeti | Microsoft Docs'
description: Service Fabric Hizmetleri için taşıma maliyeti genel bakış
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 1bd049e6f929b6c3247ca1842412d5527605e643
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516590"
---
# <a name="service-movement-cost"></a>Hizmet taşıma maliyeti
Service Fabric Küme Kaynak Yöneticisi ' ne bir kümeye değişikliklerini belirlemek çalışırken göz önünde bulundurur bir faktör, bu değişiklikleri maliyetidir. "Maliyet" kavramı kapalı küme ne kadar karşı geliştirilebilir satılan. Maliyet Dengeleme, birleştirme ve diğer gereksinimler Hizmetleri taşırken katılır. En az aksatıcı veya pahalı şekilde gereksinimlerini olmaktır. 

Taşıma hizmetleri maliyetleri CPU süresi ve ağ bant genişliği en az. Durum bilgisi olan hizmetler için ek bellek ve disk tüketen hizmetlerin durumunu kopyalanmasını gerektirir. Azure Service Fabric Küme Kaynak Yöneticisi ile çıkan çözüm maliyetini en aza indirir, kümenizin kaynaklarını gereksiz yere harcanan olmayan garanti eder. Ancak, ayrıca önemli ölçüde küme içindeki kaynakların ayrılması artardı çözümleri yoksay istemezsiniz.

Küme Kaynak Yöneticisi, bilgi işlem maliyetleri ve kümeyi yönetmek çalıştığında sırasında bunları sınırlandırma iki yolu vardır. İlk mekanizma basit hale getirir her taşıma sayımı. İki çözümü ile aynı oluşturulmuşsa Bakiye (puan), Küme Kaynak Yöneticisi (hamle toplam sayısı) en düşük maliyetli bir tercih sonra.

Bu strateji de çalışır. Ancak varsayılan veya statik yükleri'te olduğu gibi tüm taşır eşitse karmaşık sistem içinde düşüktür. Bazı daha pahalı olması olasıdır.

## <a name="setting-move-costs"></a>Ayar maliyetleri taşıyın 
Varsayılan bir hizmet için ücret oluşturulduğunda taşıma belirtebilirsiniz:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C# İÇİN: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Belirtin veya hizmet oluşturulduktan sonra MoveCost bir hizmet için dinamik olarak güncelleştir: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C# İÇİN:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Taşıma maliyeti yineleme başına temelinde dinamik olarak belirtme

Önceki kod parçacıkları tüm hizmet dışında tek seferde bütün bir hizmet MoveCost belirtmek için. Ancak maliyeti, en yararlı olduğu zaman taşımak, kullanım ömrü belirli hizmet nesnesi taşıma maliyeti değiştirir. Hizmetleri, büyük olasılıkla nasıl maliyetli herhangi bir anda taşımak oldukları en iyi fikri sahip olduğundan, bir API Hizmetleri çalışma zamanı sırasında kendi tek taşıma maliyeti rapor yoktur. 

C# İÇİN:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Taşıma maliyet etkisini
MoveCost dört düzeyi vardır: Sıfır, düşük, Orta ve yüksek. MoveCosts birbirine göre sıfır dışında olan. Sıfır taşıma maliyeti taşıma ücretsizdir ve çözüm puanı karşı sayısı değil anlamına gelir. Geçişinizi yüksek mu maliyeti ayarlama *değil* tek bir yerde yineleme kalacak garantisi.

<center>

![Taşıma maliyet faktörü taşıma için çoğaltmaları seçme][Image1]
</center>

MoveCost genel en az kesintiye neden olur ve yine de eşdeğer tarihe gelen sırasında elde etmek kolay çözümler bulmanıza yardımcı olur. Bir hizmetin maliyeti kavramı göre birçok şey olabilir. Taşıma maliyeti hesaplamak en yaygın faktörler şunlardır:

- Durum veya taşımak için hizmetine sahip olan veri miktarı.
- İstemci bağlantısının kesilmesi maliyeti. Birincil çoğaltma taşıma genellikle daha fazla ikincil çoğaltma taşıma maliyeti maliyetlidir.
- Yürütülen bir işlem kesintiye maliyeti. Bazı işlemler veri düzeyi depolamak veya yanıt bir istemci çağrısı olarak gerçekleştirilen işlemleri maliyetlidir. Belirli bir noktadan sonra sahip değilseniz sonlandırmasına istemezsiniz. Bu nedenle işlemi devam ederken hareket olasılığını azaltmak için bu hizmeti nesnesi taşıma maliyeti artırın. İşlem tamamlandığında, maliyet geri normal ayarlayın.

## <a name="enabling-move-cost-in-your-cluster"></a>Taşıma maliyeti, kümenizdeki etkinleştirme
Dikkate alınması daha ayrıntılı MoveCosts için sırada MoveCost kümenizde etkinleştirilmesi gerekir. Bu ayar olmadan taşır sayım varsayılan modu MoveCost hesaplamak için kullanılır ve MoveCost raporları göz ardı edilir.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

tek başına dağıtımlarında ClusterConfig.json veya Azure için Template.json aracılığıyla kümeleri barındırılan:

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
- Service Fabric Küme Kaynak Yöneticisi, kullanım ve kapasite kümedeki yönetmek için ölçümleri kullanır. Ölçümler ve nasıl yapılandırılacakları hakkında daha fazla bilgi edinmek için kullanıma [yönetme kaynak tüketimine ve Service Fabric yük ölçümlerle](service-fabric-cluster-resource-manager-metrics.md).
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi edinmek için kullanıma [Service Fabric kümenizi Dengeleme](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
