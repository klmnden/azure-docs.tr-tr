---
title: Azure Service Fabric kaynak İdaresi kapsayıcıları ve hizmetler için | Microsoft Docs
description: Azure Service Fabric iç veya dış kapsayıcıları çalışan hizmetler için kaynak sınırları belirtmenize olanak tanır.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 49c7e2c99cce13880781a67806543b1cde0c12b6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208021"
---
# <a name="resource-governance"></a>Kaynak idaresi 

Birden fazla hizmeti aynı düğümü veya küme çalıştırırken bir hizmet işlemdeki diğer hizmetler starving daha fazla kaynak tüketebilir mümkündür. Bu sorunu "gürültülü komşu" sorun olarak adlandırılır. Azure Service Fabric ayırmaları ve kaynakları garanti ve kaynak kullanımını sınırla hizmeti başına sınırlar belirtmek Geliştirici sağlar.

> Bu makale ile devam etmeden önce hakkında bilgi edinin öneririz [Service Fabric uygulama modeli](service-fabric-application-model.md) ve [Service Fabric barındırma modeli](service-fabric-hosting-model.md).
>

## <a name="resource-governance-metrics"></a>Kaynak İdaresi ölçümleri 

Kaynak İdaresi ile uyumlu olarak Service Fabric desteklenir [hizmet paketi](service-fabric-application-model.md). Hizmet paketi atanmış olan kaynakları daha fazla kod paketler arasında bölünebilir. Belirtilen kaynak sınırları ayrıca kaynakları ayırma anlamına gelir. Service Fabric destekler CPU ve bellek hizmet paketi, her iki yerleşik ile belirtme [ölçümleri](service-fabric-cluster-resource-manager-metrics.md):

* *CPU* (ölçüm adı `servicefabric:/_CpuCores`): ana makinede kullanılabilir mantıksal bir çekirdek. Tüm çekirdek tüm düğümler arasında ağırlıklı aynı.

* *Bellek* (ölçüm adı `servicefabric:/_MemoryInMB`): bellek megabayt cinsinden ifade edilir ve makinede kullanılabilir fiziksel bellek eşler.

Bu iki ölçümler için [küme Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) toplam küme kapasite, kümedeki her düğümde ve kümedeki kalan kaynaklar üzerindeki yükü izler. Bu iki ölçümleri, herhangi bir diğer kullanıcı veya özel bir ölçü eşdeğerdir. Varolan tüm özellikler bunlarla kullanılabilir:
* Küme olabilir [dengeli](service-fabric-cluster-resource-manager-balancing.md) göre bu iki ölçümleri (varsayılan davranıştır).
* Küme olabilir [birleştirilmiş](service-fabric-cluster-resource-manager-defragmentation-metrics.md) bu iki ölçümleri göre.
* Zaman [bir küme açıklayan](service-fabric-cluster-resource-manager-cluster-description.md), arabelleğe alınan kapasite iki bu ölçümleri ayarlanabilir.

[Dinamik yük raporlama](service-fabric-cluster-resource-manager-metrics.md) bu ölçümleri desteklenmiyor ve bu ölçümleri oluşturma zamanında tanımlanır için yükler.

## <a name="resource-governance-mechanism"></a>Kaynak İdaresi mekanizması

Service Fabric çalışma zamanı ayırma kaynaklar için şu anda sağlamaz. Bir işlem veya bir kapsayıcı açıldığında, çalışma zamanı kaynak sınırları oluşturma sırasında tanımlanan yükleri ayarlar. Ayrıca, çalışma zamanı kaynakları aşıldığında kullanılabilen yeni hizmet paketleri açılmasını reddeder. İşlemin nasıl çalıştığını daha iyi anlamak için iki CPU çekirdekleri düğümle örneği atalım (bellek idare mekanizma eşdeğerdir):

1. İlk olarak, kapsayıcı bir CPU çekirdeği isteyen düğümde yerleştirilir. Çalışma zamanı kapsayıcısı açar ve CPU sınır için bir çekirdek ayarlar. Kapsayıcı birden fazla çekirdek kullanmanız mümkün olmayacaktır.

2. Ardından, bir hizmetin bir çoğaltma düğüme yerleştirilen ve karşılık gelen hizmet paketi bir CPU çekirdeği sınırı belirtir. Çalışma zamanı kod paketi açar ve bir çekirdek, CPU sınırına ayarlar.

Bu noktada, sınırları toplamını düğüm kapasitesi eşittir. Bir işlem ve bir kapsayıcı bir çekirdek ile her çalıştıran ve birbirlerini engellemeyen değil. CPU sınırı belirtirken Service Fabric başka kapsayıcıları veya çoğaltmaları yerleştirmez.

Ancak, diğer işlemler için CPU yüklüyorsa iki durum vardır. Bu durumlarda, işlem ve örneğimizde kapsayıcıdan gürültülü komşu sorun karşılaşabilirsiniz:

* *Yönetilen ve yönetilen olmayan hizmetleri ve kapsayıcıları karıştırma*: bir kullanıcı belirtilen tüm kaynak İdaresi bir hizmet oluşturursa, çalışma zamanının hiçbir kaynak tüketen olarak görür ve örneğimizde düğümde yerleştirebilirsiniz. Bu durumda, bu yeni işlem etkin düğüm üzerinde çalışmakta olan hizmetleri ödün verme pahasına bazı CPU kullanır. Bu sorunun iki çözümü vardır. Verme kapsamındadır ve yönetilen olmayan hizmetleri aynı kümede karışık ya da kullanın [kısıtlamalarından](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) böylece bu iki tür hizmet düğümleri aynı kümesi şunun yok.

* *Düğümde, Service Fabric (örneğin, bir işletim sistemi hizmeti) dışında başka bir işlem başlatıldığında*: Bu durumda, Service Fabric dışında işlemi ayrıca CPU için varolan hizmetleriyle contends. Sonraki bölümde gösterildiği gibi işletim sistemi yükü için hesap için doğru düğümü kapasiteler ayarlamak için bu sorun için çözüm olur.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Kaynak İdaresi etkinleştirmek için Küme kurulumu

Bir düğüm başlar ve küme birleştirir, Service Fabric kullanılabilir bellek ve çekirdek kullanılabilir sayısı algılar ve bu iki kaynakları için düğüm kapasiteler ayarlar. 

İşletim sistemi için ve diğer işlemler için arabellek boşluk bırakmak için düğüm üzerinde çalıştırıyor olabilirsiniz, Service Fabric düğümde yalnızca % 80 kullanılabilir kaynakları kullanır. Bu yüzde yapılandırılabilir ve küme bildiriminde değiştirilebilir. 

Aşağıda, kullanılabilir CPU % 50'si ve kullanılabilir belleğin % 70 kullanmak için Service Fabric istemek üzere nasıl bir örnek verilmiştir: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Düğüm kapasitesi tam el ile Kurulumu gerekiyorsa, kümedeki düğümler açıklamak için normal mekanizması kullanabilirsiniz. 2 GB bellek ve dört çekirdek düğümle ayarlama konusunda bir örnek aşağıda verilmiştir: 

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Kullanılabilir kaynaklar otomatik algılamayı etkinleştirdiğinizde ve düğüm kapasiteleri küme bildiriminde el ile tanımlanan, Service Fabric düğümü kullanıcı tanımlı kapasite desteklemek için yeterli kaynak olup olmadığını denetler:
* Küçük veya bu düğümde kullanılabilir kaynaklar eşit bildiriminde tanımlanan düğüm kapasiteler: Service Fabric bildiriminde belirtilen kapasiteleri kullanır.

* Bildiriminde tanımlanan düğüm kapasiteleri kullanılabilir kaynakları büyük olan Service Fabric düğümü kapasiteleri kullanılabilir kaynakları kullanır.

Gerekli değilse, kullanılabilir kaynakları bilgisayarının otomatik algılanmasını devre dışı. Devre dışı bırakmak için aşağıdaki ayarını değiştirin:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

En iyi performans için aşağıdaki ayar da küme bildiriminde açık olmalıdır: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specify-resource-governance"></a>Kaynak İdaresi belirtin 

Kaynak İdaresi sınırları uygulama bildirimi (ServiceManifestImport bölümüne) aşağıdaki örnekte gösterildiği gibi belirtilir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>

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
  
Bu örnekte, hizmet paketi adlı **ServicePackageA** nereye yerleştirileceğini düğümlerinde bir temel alır. Bu hizmet paketi iki farklı kod paketi içerir (**CodeA1** ve **CodeA2**), ve her ikisini de belirtin `CpuShares` parametresi. CpuShares 512:256 oranını çekirdek iki kod paket arasında böler. 

Bu nedenle, bu örnekte, bir çekirdek iki üç CodeA1 alır ve CodeA2 üçte bir çekirdek (ve aynı yazılım garantisi ayırma) alır. CpuShares kod paketler için belirtilmezse Service Fabric çekirdek bunları arasında eşit olarak böler.

Her iki kod paketi için 1024 MB bellek (ve aynı yazılım garantisi ayırma) sınırlı olacak şekilde bellek sınırları kesin. Kod paketleri (kapsayıcıları veya işlemler), bu sınır ve bir bellek yetersiz özel durum bunu sonuçlarında denenirse daha fazla bellek ayrılamıyor. Kaynak sınırı zorlamasının çalışması için, hizmet paketi içindeki tüm kod paketlerinin bellek sınırlarının belirtilmiş olması gerekir.

### <a name="using-application-parameters"></a>Uygulama parametreleri kullanma

Kaynak İdaresi belirtirken kullanmak mümkün [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) birden çok uygulama yapılandırmalarını yönetme. Aşağıdaki örnek, uygulama parametreleri kullanımını gösterir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>

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

Bu örnekte, her bir hizmet paketi 4 çekirdek ve 2 GB bellek nereden üretim ortamı için varsayılan parametre değerlerini ayarlanır. Uygulama parametreleri dosyalarını ile varsayılan değerleri değiştirmek mümkündür. Bu örnekte, bir parametre dosyası uygulamayı yerel olarak test etmek için üretim daha az kaynak nereden kullanılabilir: 

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
> Uygulama parametrelerle belirten kaynak İdaresi Service Fabric sürüm 6.1 itibaren kullanılabilir.<br> 
>
> Kaynak İdaresi belirtmek için uygulama parametresiz kullanıldığında Service Fabric sürüm 6.1 önce bir sürüm indirgenemez. 


## <a name="other-resources-for-containers"></a>Kapsayıcıları için diğer kaynaklar
CPU ve bellek yanı sıra kapsayıcıları için diğer kaynak sınırları belirtin mümkündür. Bu sınırlar kod paketi düzeyinde belirtilen ve kapsayıcı başlatıldığında uygulanır. Aksine CPU ve bellek Küme Kaynak Yöneticisi'ni bu kaynaklar farkında değildir ve olmaz herhangi kapasite denetimleri yapmak veya bunları için Yük Dengeleme. 

* *MemorySwapInMB*: bir kapsayıcı kullanabilirsiniz swap bellek miktarı.
* *MemoryReservationInMB*: yalnızca bellek Çekişme düğümde algılandığında zorlanır bellek idare yumuşak sınırı.
* *CpuPercent*: kapsayıcı kullanabilirsiniz CPU yüzdesi. CPU sınırları hizmet paketini belirtilirse, bu parametre etkili bir şekilde yoksayılır.
* *Maximumıops*: (okuma ve yazma) bir kapsayıcı kullanabileceğiniz maksimum IOPS.
* *MaximumIOBytesps*: (okuma ve yazma) bir kapsayıcı kullanabileceğiniz maksimum g/ç (Saniyedeki bayt cinsinden).
* *BlockIOWeight*: GÇ blok göre diğer kapsayıcıları için ağırlık.

Bu kaynaklar CPU ve bellek ile birleştirilebilir. Kapsayıcıları için ek kaynaklar belirleme konusunda bir örneği burada verilmiştir:

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
* Küme Kaynak Yöneticisi hakkında daha fazla bilgi için okuma [Service Fabric kümesi Kaynak Yöneticisi'ni Tanıtımı](service-fabric-cluster-resource-manager-introduction.md).
* Uygulama modeli, hizmet paketleri ve kod paketleri--hakkında daha fazla bilgi ve çoğaltmalar için--eşleştirebilirsiniz nasıl okumak için [Service Fabric uygulamada Model](service-fabric-application-model.md).
