---
title: Kapsayıcılar ve hizmetler için Azure Service Fabric kaynak İdaresi | Microsoft Docs
description: Azure Service Fabric, iç veya dış kapsayıcıları çalışan hizmetler için kaynak sınırları belirtmenizi sağlar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: aljo, subramar
ms.openlocfilehash: e011554e61411fddca034f024c30c2270593e07b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669257"
---
# <a name="resource-governance"></a>Kaynak idaresi

Birden fazla hizmeti aynı düğümünün veya kümenin çalıştırırken bir hizmet işlemi diğer hizmetlerde Kaynaksız daha fazla kaynak tüketebilir mümkündür. Bu sorun, "gürültülü komşu" sorununu adlandırılır. Azure Service Fabric ayırmaları ve kaynakları garanti ve kaynak kullanımını sınırlamak için hizmet başına sınırlar belirtmek Geliştirici sağlar.

> Bu makalede ile devam etmeden önce hakkında bilgi edinin öneririz [Service Fabric uygulama modelini](service-fabric-application-model.md) ve [Service Fabric barındırma modeli](service-fabric-hosting-model.md).
>

## <a name="resource-governance-metrics"></a>Kaynak İdaresi ölçümleri

Kaynak İdaresi, Service Fabric ile uyumlu olarak içerisinde desteklendiği [hizmet paketi](service-fabric-application-model.md). Hizmet paketi atanan kaynaklarını daha fazla kod paketleri arasında bölünebilir. Belirtilen kaynak sınırları Ayrıca kaynak ayırma anlamına gelir. Service Fabric destekleyen CPU ve bellek hizmet paketi başına iki yerleşik ile belirtme [ölçümleri](service-fabric-cluster-resource-manager-metrics.md):

* *CPU* (ölçüm adı `servicefabric:/_CpuCores`): Ana makinede kullanılabilir mantıksal çekirdek. Tüm düğümlerde tüm çekirdek ağırlıklı aynı.

* *Bellek* (ölçüm adı `servicefabric:/_MemoryInMB`): Bellek megabayt cinsinden ifade edilir ve makinede kullanılabilir fiziksel bellek eşlenir.

Bu iki ölçüm için [Küme Kaynak Yöneticisi](service-fabric-cluster-resource-manager-cluster-description.md) toplam küme kapasitesi, kümedeki her düğüme ve kümedeki kalan kaynaklar üzerindeki yükü izler. Bu iki ölçüm herhangi bir diğer kullanıcı veya özel ölçüm eşdeğerdir. Varolan tüm özellikler ile onları kullanılabilir:

* Küme olabilir [dengeli](service-fabric-cluster-resource-manager-balancing.md) göre bu iki ölçüm (varsayılan davranış).
* Küme olabilir [birleştirilmiş](service-fabric-cluster-resource-manager-defragmentation-metrics.md) bu iki ölçüm göre.
* Zaman [bir kümeyi açıklama](service-fabric-cluster-resource-manager-cluster-description.md), arabelleğe alınan kapasite bu iki ölçüm için ayarlanabilir.

[Dinamik yük raporlama](service-fabric-cluster-resource-manager-metrics.md) bu ölçümleri desteklenmiyor ve Bu ölçümler, oluşturma zamanında tanımlanır için yükler.

## <a name="resource-governance-mechanism"></a>Kaynak İdaresi mekanizması

Service Fabric çalışma zamanı şu anda kaynak için ayırma sağlamaz. Bir işlem veya bir kapsayıcı açıldığında, çalışma zamanı kaynak sınırları oluşturma zamanında tanımlanmış yükleri ayarlar. Ayrıca, çalışma zamanı kaynakları aşıldığında kullanılabilen yeni hizmet paketleri açılmasına reddeder. İşlemin nasıl çalıştığını daha iyi anlamak için CPU çekirdeği iki düğümle bir örneği ele alalım (bellek idare mekanizması eşdeğerdir):

1. İlk olarak, bir kapsayıcıyı tek bir CPU çekirdeği isteyen düğümde yerleştirilir. Çalışma zamanı, kapsayıcı açılır ve bir çekirdek CPU sınırı ayarlar. Kapsayıcı birden fazla çekirdek kullanmanız mümkün olmayacaktır.

2. Ardından, bir hizmetin bir çoğaltma düğümde yerleştirilir ve karşılık gelen hizmet paketi tek bir CPU çekirdeği sınırı belirtir. Çalışma zamanı, kod paketi açılır ve bir çekirdek CPU sınırına ayarlar.

Bu noktada, sınırları toplamı kapasiteyi düğümünün eşittir. Bir işlem ve kapsayıcı tek çekirdek her çalışan ve birbiriyle engellemesini değil. CPU sınırı belirtirken Service Fabric artık kapsayıcıları veya çoğaltmaları yerleştirmez.

Ancak, diğer işlemler CPU azaltması iki durum vardır. Bu gibi durumlarda, işlem ve Örneğimizdeki kapsayıcısından gürültülü komşu sorun karşılaşabilirsiniz:

* *Yönetilen ve yönetilmeyen Hizmetleri ve kapsayıcıları karıştırma*: Bir kullanıcı belirtilen tüm kaynak İdaresi bir hizmet oluşturursa, çalışma zamanı kaynak tüketen olarak görür ve örneğimizde düğümde yerleştirebilirsiniz. Bu durumda, bu yeni işlemin etkin düğümde çalışmakta olan hizmetleri çoğaltamaz bazı CPU kullanır. Bu sorunun iki çözümü vardır. Yoksa aynı küme üzerindeki yönetilen ve yönetilmeyen Hizmetleri karışık veya kullanın [yerleştirme kısıtlamaları](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) düğümleri aynı dizi üzerinde bu iki tür hizmet bitirme böylece.

* *Düğümde, Service Fabric (örneğin, bir işletim sistemi hizmet) dışında başka bir işlem başlatıldığında*: Bu durumda, Service Fabric dışında işlemi için CPU Ayrıca var olan Hizmetleri ile contends. Bu sorunun çözümü işletim sistemi yükü için düğüm kapasiteleri hesap doğru olarak ayarlamak için sonraki bölümde gösterildiği gibidir.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Kaynak İdaresi etkinleştirmek için Küme kurulumu

Bir düğüm başlar ve küme birleşimler, Service Fabric kullanılabilir bellek miktarı ve çekirdek sayısının algılar ve ardından bu iki kaynaklar için düğüm kapasiteleri ayarlar.

İşletim sistemi için ve diğer işlemler için arabellek alanı bırakmak için düğüm üzerinde çalışıyor olabilir, Service Fabric düğüm üzerinde yalnızca %80 kullanılabilir kaynakları kullanır. Bu yüzde yapılandırılabilir ve küme bildiriminde değiştirilebilir.

Söyleyin kullanılabilir CPU yüzdesi 50 ve kullanılabilir bellek yüzdesi 70 kullanmak için Service Fabric ilişkin bir örnek aşağıda verilmiştir:

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Düğüm kapasiteleri tam el ile Kurulumu gerekiyorsa, kümedeki düğümler açıklamak için normal bir mekanizma kullanabilirsiniz. 2 GB bellek ve dört çekirdek düğümle ayarlamak nasıl bir örneği aşağıda verilmiştir:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Kullanılabilir kaynaklar otomatik algılama etkindir ve düğüm kapasiteleri el ile küme bildiriminde tanımlanan, Service Fabric düğümü kullanıcı tanımlı kapasite desteklemek için yeterli kaynaklara sahip olduğunu denetler:

* Küçük veya buna eşit kullanılabilir kaynaklara düğümde bildiriminde tanımlanan düğüm kapasitesi varsa, Service Fabric bildirimde belirtilen kapasiteler kullanır.

* Bildiriminde tanımlanan düğüm kapasiteleri kullanılabilir kaynakları büyükse, Service Fabric düğüm kapasiteleri kullanılabilir kaynakları kullanır.

Gerekli değilse kullanılabilir kaynaklar otomatik algılama kapatılabilir. Devre dışı bırakmak için aşağıdaki ayarları değiştirin:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

En iyi performans için aşağıdaki ayar da küme bildiriminde açılması gerekir:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" />
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```

## <a name="specify-resource-governance"></a>Kaynak İdaresi belirtin

Kaynak İdaresi sınırları, aşağıdaki örnekte gösterildiği gibi uygulama bildiriminde (Servicemanifestımport bölümünde) belirtilir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric sums the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```

Bu örnekte, hizmet paketi olarak adlandırılan **ServicePackageA** nereye yerleştirileceğini düğümlerinde bir çekirdek alır. Bu hizmet paketi içeren iki kod paketleri (**CodeA1** ve **CodeA2**), ve her ikisini birden belirtin `CpuShares` parametresi. CpuShares 512:256 oranını çekirdek iki kod paketleri arasında böler.

Bu nedenle, bu örnekte, bir çekirdeği üçte iki CodeA1 alır ve CodeA2 üçte birinin bir çekirdek (ve aynı genel garantili ayırmayla rezervasyonu) alır. CpuShares kod paketleri belirtilmezse, Service Fabric çekirdek eşit arasında böler.

Bellek sınırları mutlak olduğundan iki kod paketi 1024 MB bellek (ve rezervasyon aynı genel garantili ayırmayla) sınırlıdır. Kod paketleri (kapsayıcılar veya işlemler) bu sınırı ve bunu denediklerinde bir bellek yetersiz özel yapma girişimi daha fazla bellek tahsis edilemiyor. Kaynak sınırı zorlamasının çalışması için, hizmet paketi içindeki tüm kod paketlerinin bellek sınırlarının belirtilmiş olması gerekir.

### <a name="using-application-parameters"></a>Uygulama parametreleri kullanma

Kaynak İdaresi belirtirken kullanmak mümkün mü [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) birden çok uygulama yapılandırmalarını yönetme. Aşağıdaki örnek, uygulama parametreleri kullanımını gösterir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="4" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="2048" />
    <Parameter Name="MemoryB" DefaultValue="2048" />
  </Parameters>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="[CpuSharesA]" MemoryInMB="[MemoryA]" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="[CpuSharesB]" MemoryInMB="[MemoryB]" />
    </Policies>
  </ServiceManifestImport>
```

Bu örnekte, her bir hizmet paketi 4 çekirdek ve 2 GB bellek nereden üretim ortamı için varsayılan parametre değerlerini ayarlanır. Varsayılan değerleri uygulama parametre dosyaları ile değiştirmek mümkündür. Bu örnekte, bir parametre dosyası uygulamayı yerel olarak test etmek için üretim ortamında daha az kaynak nereden kullanılabilir:

```xml
<!-- ApplicationParameters\Local.xml -->

<Application Name="fabric:/TestApplication1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="1024" />
    <Parameter Name="MemoryB" DefaultValue="1024" />
  </Parameters>
</Application>
```

> [!IMPORTANT]
> Uygulama parametrelerle belirten kaynak İdaresi, Service Fabric 6.1 sürümünde'den itibaren kullanılabilmektedir.<br>
>
> Uygulama parametreleri kaynak İdaresi belirtmek için kullanıldığında, Service Fabric 6.1 sürümünden önceki bir sürüme düşürülemez.

## <a name="other-resources-for-containers"></a>Kapsayıcılar için diğer kaynaklar

CPU ve bellek yanı sıra, kapsayıcılar için diğer kaynak sınırlarını belirtmek mümkündür. Bu sınırlar kod paketi düzeyinde belirtilir ve kapsayıcı başlatıldığında uygulanır. Farklı CPU ve bellek ile küme kaynak yöneticisi bu kaynakların, farkında değildir ve olmaz herhangi bir kapasite denetimleri yapmak veya bunları için Yük Dengeleme.

* *MemorySwapInMB*: Bir kapsayıcı kullanabileceğiniz swap belleği miktarı.
* *Memoryreservationınmb*: Yalnızca bellek Çekişme düğümde algılandığında zorlanan bellek idare yumuşak sınırını.
* *CpuPercent*: Kapsayıcı kullanabileceğiniz CPU yüzdesi. Bu parametre, etkili bir şekilde hizmet paketini CPU sınırları belirtilmezse, yoksayılır.
* *Maximumıops*: (Okuma ve yazma) bir kapsayıcı kullanabileceğiniz maksimum IOPS.
* *MaximumIOBytesps*: (Okuma ve yazma) bir kapsayıcı kullanabileceğiniz maksimum g/ç (bayt / saniye).
* *BlockIOWeight*: Blok için GÇ ağırlığı diğer kapsayıcılara göre.

Bu kaynaklar, CPU ve bellek ile birleştirilebilir. Kapsayıcılar için ek kaynaklar belirtmek nasıl bir örnek aşağıda verilmiştir:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuPercent="5"
            MemorySwapInMB="4084" MemoryReservationInMB="1024" MaximumIOPS="20" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="next-steps"></a>Sonraki adımlar

* Küme Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için [Service Fabric Küme Kaynak Yöneticisi ile tanışın](service-fabric-cluster-resource-manager-introduction.md).
* Uygulama modeli, hizmet paketleri ve kod paketleri--hakkında daha fazla bilgi edinin ve yinelemeler için--eşleştirebilirsiniz nasıl okuma [bir Service Fabric uygulamasında Model](service-fabric-application-model.md).
