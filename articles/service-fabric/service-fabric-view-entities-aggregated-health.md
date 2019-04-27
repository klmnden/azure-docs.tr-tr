---
title: Görüntüleme Azure Service Fabric varlıklarının durumunu toplu olarak | Microsoft Docs
description: Sorgu, görüntülemek ve sistem durumu sorgularının sayısı ve genel sorgular aracılığıyla toplanan Azure Service Fabric varlıklarının durumunu değerlendirme açıklar.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: chackdan
editor: ''
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: e4edcc0aecfbf03aff7cf9bee764522bb1c489f3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60716389"
---
# <a name="view-service-fabric-health-reports"></a>Service Fabric sistem durumu raporlarını görüntüleme
Azure Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) hangi sistem bileşenleri ve watchdogs yerel koşulları rapor için sistem durumu varlıklarını ile bunların izleme. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıkları iyi durumda olup olmadığını belirlemek için tüm sistem durumu verileri toplar.

Küme sistem bileşenleri tarafından gönderilen sistem durumu raporlarını otomatik olarak doldurulur. Adresinde daha fazla [sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric varlıkları toplu durumunu almak için birden çok yol sağlar:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görselleştirme araçları
* Sistem durumu sorgularının sayısı (aracılığıyla, PowerShell, API veya REST)
* Genel sistem durumu özellikleri (aracılığıyla, PowerShell, API veya REST) biri olarak varlıklar listesi, dönüş sorgular

Bu seçenekler göstermek için yerel bir küme ile beş düğüm kullanalım ve [fabric: / WordCount uygulamasını](https://aka.ms/servicefabric-wordcountapp). **Fabric: / WordCount** iki varsayılan hizmet, bir durum bilgisi olan hizmet türü içeren uygulama `WordCountServiceType`ve bir durum bilgisi olmayan hizmet türü `WordCountWebServiceType`. Değiştirdim `ApplicationManifest.xml` yedi hedeflemek için durum bilgisi olan hizmet ve bir bölüm çoğaltmalarını istemek için. Kümede yalnızca beş düğüm olduğundan, hedef sayısı olduğu için sistem bileşenleri hizmet bölüme bir uyarı bildirir.

```xml
<Service Name="WordCountService">
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Service Fabric Explorer'da sistem durumu
Service Fabric Explorer, kümeye görsel görünümünü sağlar. Aşağıdaki görüntüde görebileceğiniz gibi:

* Uygulama **fabric: / WordCount** tarafından raporlanan bir hata olayı sahip olduğundan (hata) kırmızıdır **MyWatchdog** özelliği için **kullanılabilirlik**.
* Hizmetlerinin birini **fabric: / WordCount/gerçekleştirdiğiniz** sarı (uyarı). Hizmet yedi çoğaltma ile yapılandırıldığı ve yinelemeler yerleştirilemez beş düğüm kümesi bulunduğundan. Burada gösterilmese hizmeti sistem rapordan nedeniyle sarı bölümdür `System.FM` olduğunu belirten `Partition is below target replica or instance count`. Sarı bölümü, sarı hizmet tetikler.
* Küme kırmızı nedeniyle red uygulamasıdır.

Varsayılan ilkelerden küme bildirimi ve uygulama bildirimi bu değerlendirmeyi kullanır. Bunlar katı ilkeler ve tüm hatasını tolere değil.

Service Fabric Explorer ile küme görünümü:

![Service Fabric Explorer ile küme görüntüleyin.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Daha fazla bilgi edinin [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Sistem durumu sorgularının sayısı
Service Fabric gösterir sistem durumu sorgularının sayısı her desteklenen [varlık türleri](service-fabric-health-introduction.md#health-entities-and-hierarchy). Yöntemleri kullanarak, API aracılığıyla erişilebilen [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlet'leri ve REST. Bu sorguları, varlığın tüm sistem bilgilerini döndürür: toplanan sistem durumu, varlık sistem durumu olayları, alt sistem durumlarını (uygun olduğunda), (varlık iyi durumda olmadığı durumlarda) sağlıksız değerlendirmeler ve alt öğeleri sağlık istatistikleri (ne zaman uygulanabilir).

> [!NOTE]
> Health store içinde tam olarak doldurulan bir sistem durumu varlık döndürülür. Varlık (silinmedi) etkin ve sistem raporu olması gerekir. Hiyerarşi zincirini kendi üst varlıklarda sistem raporlarını da olmalıdır. Bu koşullardan herhangi biri tatmin edici değil, sistem dönüş sorgular bir [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) ile [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` gösteren neden varlık döndürülmez.
>
>

Sistem durumu sorgularının varlık türüne bağlıdır varlığı tanımlayıcısı geçmesi gerekir. Sorgular, isteğe bağlı sistem durumu ilkesi parametreleri kabul eder. Sistem durumu ilkesi yok belirtilirse, [sistem durumu ilkeleri](service-fabric-health-introduction.md#health-policies) küme veya uygulama bildiriminden değerlendirmesi için kullanılır. Sistem durumu ilkeleri için bir tanım bildirimleri içermiyorsa, varsayılan sistem durumu ilkeleri değerlendirmesi için kullanılır. Varsayılan sistem durumu ilkeleri, herhangi bir hata oluştuğunda değil. Sorgular yalnızca kısmi bir alt veya olaylardan belirtilen filtrelerle saygı olanları döndürmek için filtreler de kabul eder. Başka bir filtre alt istatistikleri hariç tutulabilmesini sağlar.

> [!NOTE]
> İleti yanıt boyutu azaltıldı şekilde çıkış filtreleri sunucu tarafında uygulanır. Size döndürülen verileri sınırlamak için çıkış filtreleri kullanın; yerine filtreleri istemci tarafında uygulama önerilir.
>
>

Bir varlığın durum içerir:

* Toplanmış sistem durumu varlık. Varlık sistem durumu raporlarının, (uygunsa) alt sistem durumlarını ve sistem durumu ilkeleri dayalı sistem durumu deposu tarafından hesaplanan. Daha fazla bilgi edinin [varlık sistem durumu değerlendirmesi](service-fabric-health-introduction.md#health-evaluation).  
* Varlık sistem durumu olayları.
* Tüm alt öğeleri alt öğeleri olan varlıklar için sağlık durumlarını koleksiyonu. Sistem durumları, varlık tanımlayıcılar ve toplanan sistem durumunu içerir. Tüm sistem için bir alt almak için alt varlık türü için sorgu sistem çağrısı ve alt tanımlayıcıda geçirin.
* Varlık iyi durumda değilse varlığın durumu tetikleyen rapora işaret sağlıksız değerlendirmeler. Değerlendirmeleri geçerli durumu tetiklenen alt sistem durumu değerlendirme içeren, yinelemelidir. Örneğin, bir izleme bir çoğaltma karşı bir hata bildirdi. Uygulama durumunu iyi durumda olmayan bir hizmet nedeniyle sağlıksız bir değerlendirme gösterir. bir bölümünde bir hata nedeniyle sağlıksız hizmetidir; Bölüm iyi durumda olmayan bir çoğaltma hatası nedeniyle; Çoğaltma nedeniyle izleme hata sistem durumu raporu sağlam değil.
* Alt varlıklar tüm alt türlerinin durumu istatistikleri. Örneğin, küme sistem durumu, uygulamalar, hizmetler, bölümler, çoğaltmaları toplam sayısını gösterir ve varlık kümesinde dağıtılan. Hizmet durumu, bölümleri ve çoğaltmalarını altında belirtilen hizmet toplam sayısını gösterir.

## <a name="get-cluster-health"></a>Küme durumunu öğrenme
Küme varlık durumunu döndürür ve uygulama ve düğümleri (alt kümesinin) sağlık durumlarını içerir. Giriş:

* [İsteğe bağlı] Düğümler ve küme olayları değerlendirmek için kullanılan Küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama sistem durumu ilkesi haritası, uygulama bildirim ilkelerini geçersiz kılmak için kullanılan sistem durumu ilkeleri ile.
* [İsteğe bağlı] Olaylar, düğümleri ve hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten uygulamalar için filtreler. Tüm olaylar, düğümleri ve uygulamaları toplu varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtreleyin.
* [İsteğe bağlı] Fabric içerecek şekilde filtre: / sistem durumu istatistiklerini sağlık istatistikleri. Yalnızca sistem durumu istatistiklerini değil dışlanmaz olduğunda geçerlidir. Varsayılan olarak, sistem durumu İstatistikler yalnızca kullanıcı uygulama ve sistem uygulaması için istatistikleri içerir.

### <a name="api"></a>API
Küme durumu almak için oluşturma bir `FabricClient` ve çağrı [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) yöntemi kendi **HealthManager**.

Küme durumu çağrısını alır:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Aşağıdaki kod, düğümleri ve uygulamalar için bir özel küme sistem durumu ilkesi ve filtreleri kullanarak küme durumu alır. Sağlık istatistikleri fabric içerdiğini belirtir: / Sistem istatistikleri. Oluşturur [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), giriş bilgileri içerir.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var healthStatisticsFilter = new ClusterHealthStatisticsFilter()
{
    ExcludeHealthStatistics = false,
    IncludeSystemApplicationHealthStatistics = true
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
    HealthStatisticsFilter = healthStatisticsFilter
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Küme durumu almak için cmdlet [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Beş düğüm, sistem uygulaması ve fabric kümesinin durumunu olduğu: / WordCount açıklandığı şekilde yapılandırılmış.

Aşağıdaki cmdlet varsayılan sistem durumu ilkeleri kullanarak küme durumu alır. Toplanmış sistem durumu uyarı, çünkü konumunda fabric: / WordCount uygulamasını uyarı. Sağlıksız değerlendirmeler Ayrıntıları toplanan sistem tetikleyen koşullara nasıl sağladığını unutmayın.

```xml
PS D:\ServiceFabric> Get-ServiceFabricClusterHealth


AggregatedHealthState   : Warning
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.


NodeHealthStates        : 
                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
HealthStatistics        : 
                          Node                  : 5 Ok, 0 Warning, 0 Error
                          Replica               : 6 Ok, 0 Warning, 0 Error
                          Partition             : 1 Ok, 1 Warning, 0 Error
                          Service               : 1 Ok, 1 Warning, 0 Error
                          DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                          DeployedApplication   : 5 Ok, 0 Warning, 0 Error
                          Application           : 0 Ok, 1 Warning, 0 Error
```

Aşağıdaki PowerShell cmdlet'i, bir özel uygulama İlkesi kullanarak küme durumunu alır. Bu, yalnızca uygulama ve düğümleri hata veya uyarı almak için sonuçları filtreler. Tüm sağlıklı olarak sonuç olarak, düğüm, döndürülür. Fabric: / WordCount uygulamasını uygulamaları filtre dikkate alır. Fabric uyarılarını hata olarak değerlendirilecek özel ilke belirttiğinden: / WordCount uygulamasını, uygulama, hata olduğu gibi değerlendirilir ve bu nedenle kümedir.

```powershell
PS D:\ServiceFabric> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics


AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                            Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                            Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                    Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None
```

### <a name="rest"></a>REST
Küme durumu ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-node-health"></a>Düğüm durumu alma
Düğümün varlık durumunu döndürür ve düğüm üzerinde bildirilen sistem durumu olaylarını içerir. Giriş:

* [Gerekli] Düğümde tanımlayan düğüm adı.
* [İsteğe bağlı] Sistem durumunu değerlendirmek için kullanılan Küme sistem durumu ilkesi ayarlar.
* [İsteğe bağlı] Hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten olaylar için filtreleri. Tüm olaylar, toplanan varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.

### <a name="api"></a>API
API aracılığıyla düğüm durumu almak için oluşturma bir `FabricClient` ve çağrı [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) kendi HealthManager yöntemi.

Aşağıdaki kod, düğüm durumu belirtilen düğümün adı alır:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Aşağıdaki kod, düğüm durumu belirtilen düğümün adını alır ve olayları filtresi ve özel İlkesi aracılığıyla geçirir [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Düğüm durumu almak için cmdlet [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.
Aşağıdaki cmdlet varsayılan sistem durumu ilkeleri kullanarak düğüm durumunun alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 7/13/2017 4:39:23 PM
                        ReceivedAt            : 7/13/2017 4:40:47 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 4:40:47 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Aşağıdaki cmdlet'i, kümedeki tüm düğümlerin durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_4                     Ok
_Node_3                     Ok
_Node_2                     Ok
_Node_1                     Ok
_Node_0                     Ok
```

### <a name="rest"></a>REST
Düğüm durumu ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-application-health"></a>Uygulama durumunu Al
Bir uygulama varlık durumunu döndürür. Bu dağıtılmış uygulama ve hizmet alt sağlık durumlarını içerir. Giriş:

* [Gerekli] Uygulamayı tanımlayan uygulama adı (URI).
* [İsteğe bağlı] Uygulama bildirim ilkelerini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olaylar, hizmetler ve hangi girişlerin belirtin dağıtılan uygulamalar için filtre ilgilendiğiniz ve (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) sonuç döndürdü. Tüm olaylar, hizmetler ve dağıtılan uygulamalar toplu varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtreleyin. Belirtilmemişse, sistem durumu istatistikleri Tamam, uyarı ve hata sayısı için tüm uygulama alt öğeleri içerir: Hizmetler, bölümler, çoğaltmalar, dağıtılan uygulamalar ve hizmet paketleri dağıtılır.

### <a name="api"></a>API
Uygulama durumunu almak için oluşturma bir `FabricClient` ve çağrı [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) kendi HealthManager yöntemi.

Aşağıdaki kod, belirtilen uygulama adı (URI) için uygulama durumunu alır:

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Aşağıdaki kod, filtrelerle belirtilen uygulama adı (URI) için uygulama durumunu alır ve belirtilen özel ilkeler aracılığıyla [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Uygulama durumunu almak için cmdlet [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet durumunu döndürür **fabric: / WordCount** uygulama:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

DeployedApplicationHealthStates : 
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/13/2017 5:57:05 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics                : 
                                  Replica               : 6 Ok, 0 Warning, 0 Error
                                  Partition             : 1 Ok, 1 Warning, 0 Error
                                  Service               : 1 Ok, 1 Warning, 0 Error
                                  DeployedServicePackage : 6 Ok, 0 Warning, 0 Error
                                  DeployedApplication   : 5 Ok, 0 Warning, 0 Error
```

Aşağıdaki PowerShell cmdlet'ini özel ilkeleri geçirir. Ayrıca, alt ve olayları filtreler.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=true.

ServiceHealthStates             : 
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>REST
Uygulama durumunu ile alabilirsiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-service-health"></a>Hizmet durumunu Al
Bir hizmet varlık durumunu döndürür. Bu bölüm sağlık durumlarını içerir. Giriş:

* [Gerekli] Hizmeti tanımlar hizmet adı (URI).
* [İsteğe bağlı] Uygulama bildirim ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten bölümlerde filtreler. Tüm olayları ve bölümler, toplu varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtreleyin. Yoksa, belirtilen uyarı, Tamam, sağlık istatistikleri göster ve tüm bölümleri ve çoğaltmalarını hizmeti için hata sayısı.

### <a name="api"></a>API
API aracılığıyla hizmet durumu almak için oluşturma bir `FabricClient` ve çağrı [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) kendi HealthManager yöntemi.

Aşağıdaki örnek, bir hizmeti ile belirtilen hizmet adı (URI) alır:

```csharp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Aşağıdaki kod, filtreler ve özel İlkesi aracılığıyla belirterek hizmet durumu belirtilen hizmet adı (URI) alır [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Hizmet durumunu almak için cmdlet [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet varsayılan sistem durumu ilkeleri kullanarak hizmet durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                        Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Warning'.

                            Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

PartitionHealthStates : 
                        PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                        AggregatedHealthState : Warning

HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 15
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
                        Partition             : 0 Ok, 1 Warning, 0 Error
```

### <a name="rest"></a>REST
Hizmet durumu ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-partition-health"></a>Bölüm durumunu öğrenme
Bir bölüm varlık durumunu döndürür. Bu çoğaltma sistem durumlarını içerir. Giriş:

* [Gerekli] Bölüm bölüm tanımlayan Kimliğini (GUID).
* [İsteğe bağlı] Uygulama bildirim ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten çoğaltmalar için filtreler. Tüm olayları ve çoğaltmaları toplanan varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtreleyin. Belirtilmezse, Tamam, uyarı ve hata durumlarını kaç çoğaltmaları olan sistem durumu istatistiklerini gösterir.

### <a name="api"></a>API
API aracılığıyla bölüm durumu almak için oluşturma bir `FabricClient` ve çağrı [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreleri belirtmek için oluşturma [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Bölüm durumu almak için cmdlet [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i tüm bölümleri için durumu alır **fabric: / WordCount/gerçekleştirdiğiniz** hizmeti ve kopya sistem durumlarının filtreler:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 72
                        SentAt                : 7/13/2017 5:57:29 PM
                        ReceivedAt            : 7/13/2017 5:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/P RD _Node_2 Up 131444422260002646
                          N/S RD _Node_4 Up 131444422293113678
                          N/S RD _Node_3 Up 131444422293113679
                          N/S RD _Node_1 Up 131444422293118720
                          N/S RD _Node_0 Up 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5.)

                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 7/13/2017 5:57:48 PM, LastError = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131444445174851664
                        SentAt                : 7/13/2017 6:35:17 PM
                        ReceivedAt            : 7/13/2017 6:35:18 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A

                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None

                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/13/2017 5:57:48 PM, LastOk = 1/1/0001 12:00:00 AM

HealthStatistics      : 
                        Replica               : 5 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Partition health ile alabileceğiniz bir [GET isteği](/rest/api/servicefabric/sfclient-api-getpartitionhealth) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-replica-health"></a>Çoğaltma sistem durumu alma
Sistem durum bilgisi olan hizmet çoğaltma ya da durum bilgisi olmayan hizmet örneği döndürür. Giriş:

* [Gerekli] Çoğaltma tanımlayan bölüm Kimliğini (GUID) ve çoğaltma kimliği.
* [İsteğe bağlı] Uygulama bildirim ilkelerini geçersiz kılmak için kullanılan uygulama sistem durumu ilkesi parametreleri.
* [İsteğe bağlı] Hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten olaylar için filtreleri. Tüm olaylar, toplanan varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.

### <a name="api"></a>API
Çoğaltma sistem API'si aracılığıyla almak için oluşturma bir `FabricClient` ve çağrı [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) kendi HealthManager yöntemi. Gelişmiş parametreleri belirtmek için kullanın [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Çoğaltma sistem durumu almak için cmdlet [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i tüm bölümleri hizmetinin birincil çoğaltmasını durumunu alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Çoğaltma sistem durumu ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-deployed-application-health"></a>Dağıtılmış uygulama durumunu Al
Düğümün varlık üzerinde dağıtılmış bir uygulamada durumunu döndürür. Bu, dağıtılan hizmet paketi sağlık durumlarını içerir. Giriş:

* [Gerekli] Uygulama adı (URI) ve düğüm adı (dize) dağıtılan uygulamayı tanımlayan.
* [İsteğe bağlı] Uygulama bildirim ilkelerini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten dağıtılmış hizmet paketleri için filtreler. Tüm olayları ve dağıtılmış hizmet paketleri, toplu varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistiklerini hariç tutmak için filtreleyin. Belirtilmemişse, sistem durumu istatistiklerini Tamam, uyarı ve hata sistem durumlarının dağıtılmış hizmet paketleri sayısını gösterir.

### <a name="api"></a>API
API aracılığıyla bir düğüme dağıtılmış bir uygulamada durumunu almak için oluşturma bir `FabricClient` ve çağrı [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreleri belirtmek için kullanın [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Dağıtılmış uygulama durumunu almak için cmdlet [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i. Bir uygulamanın nerede dağıtıldığını öğrenmek için çalıştırma [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) ve dağıtılan uygulamayı alt öğeler bakın.

Aşağıdaki cmdlet'i durumunu alır **fabric: / WordCount** dağıtılan uygulama **_Node_2**.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_0


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_0
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_0
                                     AggregatedHealthState : Ok

HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131444422261848308
                                     SentAt                : 7/13/2017 5:57:06 PM
                                     ReceivedAt            : 7/13/2017 5:57:17 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/13/2017 5:57:17 PM, LastWarning = 1/1/0001 12:00:00 AM

HealthStatistics                   : 
                                     DeployedServicePackage : 2 Ok, 0 Warning, 0 Error
```

### <a name="rest"></a>REST
Dağıtılmış uygulama sistem durumu ile alabilirsiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="get-deployed-service-package-health"></a>Dağıtılan hizmet paketi durumu alma
Dağıtılan hizmet paketi varlık durumunu döndürür. Giriş:

* [Gerekli] Dağıtılan hizmet paketi belirleyin adı (dize), uygulama adı (URI), düğüm adı (dize) ve hizmet bildirimi.
* [İsteğe bağlı] Uygulama bildirim ilkesini geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin ilgili değildir ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmesi gerektiğini belirten olaylar için filtreleri. Tüm olaylar, toplanan varlık sistem durumu, filtre bağımsız olarak değerlendirmek için kullanılır.

### <a name="api"></a>API
API aracılığıyla dağıtılan hizmet paketi durumunu almak için oluşturma bir `FabricClient` ve çağrı [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreleri belirtmek için kullanın [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Dağıtılan hizmet paketi durumu almak için cmdlet [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i. Uygulamanın dağıtıldığı görmek için şunu çalıştırın [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) ve dağıtılan uygulamaları bulun. Dağıtılan hizmet paketi alt öğeleri bakmak hangi hizmet paketlerin bir uygulamada görmek için [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) çıktı.

Aşağıdaki cmdlet'i durumunu alır **WordCountServicePkg** hizmet paketi **fabric: / WordCount** dağıtılan uygulama **_Node_2**. Varlık sahip **System.Hosting** raporlar için başarılı hizmet-paketi ve giriş noktası etkinleştirme ve başarılı hizmet türü kayıt.

```powershell
PS D:\ServiceFabric> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_2
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131444422267693359
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131444422267903345
                             SentAt                : 7/13/2017 5:57:06 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131444422272458374
                             SentAt                : 7/13/2017 5:57:07 PM
                             ReceivedAt            : 7/13/2017 5:57:18 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>REST
Dağıtılan hizmet paketi durumu ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) gövdesinde tanımlanan sistem durumu ilkeleri içerir.

## <a name="health-chunk-queries"></a>Sistem durumu öbek sorgularının sayısı
Sistem durumu öbek sorgularının giriş filtreleri başına çok düzeyli Küme alt (yinelemeli) döndürebilir. Bu, çok miktarda döndürülecek alt seçme esnekliği sağlayan gelişmiş filtreler destekler. Filtreler, benzersiz tanımlayıcı veya başka bir grup kimlikleri ve/veya sistem durumlarının alt belirtebilirsiniz. Varsayılan olarak, alt öğe, her zaman birinci düzey alt öğeleri dahil sistem durumu komutları aksine dahil edilir.

[Sistem durumu sorgularının](service-fabric-view-entities-aggregated-health.md#health-queries) yalnızca birinci düzey alt belirtilen varlık başına gerekli filtreleri döndürür. Alt çocuğunu almak için ilgilenilen her varlık için ek sistem durumu API'leri çağırmanız gerekir. Benzer şekilde, belirli varlıkların durumunu almak için istenen her varlık için bir durum API'si çağırmanız gerekir. Gelişmiş filtreleme öbek sorgu birden çok öğe ileti boyutu ve ileti sayısını en aza indirmek, bir sorgu ilgi istemenizi sağlar.

Öbek sorgu, sistem durumunu daha fazla küme varlıklar (gerekli kök dizininde başlayan potansiyel olarak tüm küme varlıklar) için tek çağrıda ulaşmak değeridir. Karmaşık bir sistem durumu sorgusu gibi ifade edebilirsiniz:

* Dönüş yalnızca uygulamaları söz konusu uygulamaların yanı sıra, hata, uyarı veya hata tüm hizmetleri içerir. Döndürülen hizmetler için tüm bölümleri içerir.
* Yalnızca belirtilen adlarıyla, dört uygulamaların durumunu döndürür.
* Yalnızca istenen uygulama türünün uygulamaların durumunu döndürür.
* Tüm dağıtılan varlıklar bir düğümde döndürür. Tüm uygulamalar döndürür tüm dağıtılan uygulamaları belirtilen düğümün ve o düğümdeki tüm dağıtılmış hizmet paketleri.
* Tüm çoğaltmalar hata döndürür. Tüm uygulamalarınıza, hizmetlerinize, bölümler ve çoğaltmalar yalnızca hata döndürür.
* Tüm uygulamaları döndürür. Belirtilen bir hizmet için tüm bölümleri içerir.

Şu anda, sistem durumu öbek sorgu yalnızca küme varlık için kullanıma sunulur. İçeren bir küme sistem durumu öbek döndürür:

* Küme sistem durumu toplanır.
* Giriş filtreler dikkate düğümleri sağlık durumu öbek listesi.
* Sistem durumu öbek giriş filtreler dikkate uygulamaların listesi. Her uygulama sistem durumu öbek giriş filtreleri ve Öbek listesini filtreler dikkate tüm dağıtılmış uygulamalar ile ilgili tüm hizmetleri ile bir öbek listesini içerir. Aynı alt hizmetler ve dağıtılmış uygulamalar için. Bu şekilde, kümedeki tüm varlıklar büyük olasılıkla istediyseniz, hiyerarşik bir biçimde döndürülebilir.

### <a name="cluster-health-chunk-query"></a>Küme sistem durumu öbek sorgu
Küme varlık sistem durumu verir ve gerekli alt öğeleri hiyerarşik bir sistem durumu parçalarını içerir. Giriş:

* [İsteğe bağlı] Düğümler ve küme olayları değerlendirmek için kullanılan Küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama sistem durumu ilkesi haritası, uygulama bildirim ilkelerini geçersiz kılmak için kullanılan sistem durumu ilkeleri ile.
* [İsteğe bağlı] Filtre düğümleri ve hangi girişlerin ilgili değildir ve sonuçta döndürülen belirtin uygulamalar için. Filtreleri özel bir varlık/grubuna varlık veya o düzeydeki tüm varlıklar için geçerlidir. Filtrelerin listesini bir genel filtre ve/veya sorgu tarafından döndürülen detaylı varlıklara belirli tanımlayıcıları için filtreler içerebilir. Boşsa, varsayılan olarak alt döndürülmez.
  Filtreler hakkında daha fazla bilgiyi [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) ve [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). Uygulama Filtreleri can yinelemeli olarak alt öğeleri için Gelişmiş filtreler belirtin.

Öbek sonuç filtreleri saygı alt öğeleri içerir.

Şu anda, öbek sorgu sağlıksız değerlendirmeler veya varlık olaylarını döndürmez. Bu ek bilgi mevcut küme sistem durumu sorgusu kullanılarak edinilebilir.

### <a name="api"></a>API
Küme sistem durumu öbek almak için oluşturma bir `FabricClient` ve çağrı [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) yöntemi kendi **HealthManager**. İçinde geçirebilirsiniz [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) sistem durumu ilkeleri tanımlamak için ve Gelişmiş filtreleri.

Aşağıdaki kod ile Gelişmiş Filtreler küme sistem durumu öbeğin alır.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Küme durumu almak için cmdlet [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki kod, hata dışında her zaman döndürülmesi gereken belirli bir düğüm, yalnızca olmaları durumunda düğümleri alır.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters


HealthState                  : Warning
NodeHealthStateChunks        : 
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Aşağıdaki cmdlet kümesi Öbek ile uygulama filtreleri alır.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks : 
                                TotalCount            : 1

                                ServiceName           : fabric:/WordCount/WordCountService
                                HealthState           : Error
                                PartitionHealthStateChunks : 
                                    TotalCount            : 1

                                    PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
                                    HealthState           : Error
                                    ReplicaHealthStateChunks : 
                                        TotalCount            : 5

                                        ReplicaOrInstanceId   : 131444422293118720
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293118721
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293113678
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422293113679
                                        HealthState           : Ok

                                        ReplicaOrInstanceId   : 131444422260002646
                                        HealthState           : Error
```

Aşağıdaki cmdlet'i, bir düğümde dağıtılan tüm varlıkları döndürür.

```xml
PS D:\ServiceFabric> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks : 
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1

                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1

                                    ServiceManifestName   : FAS
                                    ServicePackageActivationId : 
                                    HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks : 
                                TotalCount            : 1

                                NodeName              : _Node_2
                                HealthState           : Ok
                                DeployedServicePackageHealthStateChunks :
                                    TotalCount            : 1

                                    ServiceManifestName   : WordCountServicePkg
                                    ServicePackageActivationId : 
                                    HealthState           : Ok
```

### <a name="rest"></a>REST
Küme sistem durumu Öbek ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) sistem durumu ilkeleri ve Gelişmiş filtreleri gövdesinde tanımlanan içerir.

## <a name="general-queries"></a>Genel sorgular
Genel sorgular, Service Fabric varlıklarıyla belirtilen bir türün bir listesini döndürür. API aracılığıyla kullanıma sunulur (yöntemleri aracılığıyla **FabricClient.QueryManager**), PowerShell cmdlet'leri ve REST. Bu sorgular, alt sorgular birden çok bileşenlerini toplama. Bunlardan biri olan [sistem durumu deposu](service-fabric-health-introduction.md#health-store), toplanan sistem durumu için her bir sorgu sonucunun doldurur.  

> [!NOTE]
> Genel sorgular, varlık toplanan sistem durumu dönün ve zengin bir sistem durumu verilerini içermez. Bir varlık sağlıklı durumda değilse, olayları, alt sistem sağlığı durumları ve sağlıksız değerlendirmeler dahil, sağlık, tüm bilgileri almak için sistem durumu sorgularının sayısı ile izlenebilir.
>
>

Genel sorgular bir varlık için bir bilinmeyen sistem durumu dönerseniz, sistem durumu deposu eksiksiz bir veri varlığı hakkında sahip olmadığını mümkündür. Ayrıca sistem durumu deposu için bir alt sorgu başarısız oldu, mümkündür (örneğin, bir iletişim hatası oluştu veya sistem durumu deposu kısıtladı). Varlık sistem durumu sorgusu ile iletişim kurun. Alt ağ sorunları gibi geçici hatalar karşılaştıysanız izleme bu sorgu başarılı olabilir. Bu ayrıca, daha fazla ayrıntı neden varlık sunulmaz hakkında health Store'dan verebilir.

İçeren sorgular **HealthState** varlıklar için:

* Düğüm listesi: Liste düğümlerine kümenin (disk belleği) döndürür.
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Uygulama listesi: (Disk belleği) kümeye uygulamaların listesini döndürür.
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Hizmet listesi: (Disk belleği) bir uygulamanın hizmet listesini döndürür.
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Bölüm listesi: Bölümlerin (disk belleği) bir hizmet olarak döndürür.
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Çoğaltma Listesi: (Disk belleği) bölümü çoğaltmaların listesini döndürür.
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Dağıtılan uygulama listesi: Bir düğümde dağıtılan uygulamaların listesini döndürür.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* Dağıtılan hizmet paketi listesi: Dağıtılan bir uygulamada hizmet paketlerinin listesi döndürür.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Bazı sorgular, disk belleğine alınan sonuçları döndürür. Bu sorguları dönüş türetilen bir listedir [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Sonuçları bir ileti uygun değilse, yalnızca bir sayfa döndürdüğü ve bir ContinuationToken, numaralandırma durduğu izler. Aynı sorgu çağırın ve sonraki sonuçları elde etmek için önceki sorgudan devamlılık belirteci geçirmek için devam edin.

### <a name="examples"></a>Örnekler
Aşağıdaki kod, kümenin iyi durumda olmayan uygulamalar alır:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Aşağıdaki cmdlet, yapı için uygulama ayrıntıları alır: / WordCount uygulamasını. Sistem durumu uyarı olduğuna dikkat edin.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Aşağıdaki cmdlet'i hata sistem durumu hizmetleriyle alır:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Error"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Error
```

## <a name="cluster-and-application-upgrades"></a>Küme ve uygulama yükseltmeleri
Uygulama ve küme izlenen bir yükseltme sırasında Service Fabric her şey iyi durumda kalmasını sağlamak için sistem durumu denetler. Bir varlık olarak yapılandırılan bir sistem durumu ilkeleri kullanarak hesaplanan sağlıksız ise, yükseltmeden sonraki eylemi belirlemek için yükseltme özgü ilkeler geçerlidir. Kullanıcı etkileşimi (örneğin, hata durumlarını çözme veya ilkelerini değiştirme) izin verecek şekilde yükseltme duraklatılmış olabilir veya bunu otomatik olarak iyi önceki sürüme geri.

Sırasında bir *küme* yükseltme, Küme yükseltme durumunu alabilirsiniz. Yükseltme durumu nedir kümedeki iyi durumda olmayan noktası sağlıksız değerlendirmeler içerir. Yükseltme sistem durumu sorunu nedeniyle geri alınır, son iyi durumda olmayan nedenlerden yükseltme durumunu hatırlar. Bu bilgileri yöneticilerin sonra yükseltmeyi geri alındı veya durduruldu, neyin yanlış gittiğini araştırmanıza yardımcı olabilir.

Benzer şekilde, sırasında bir *uygulama* yükseltme, tüm sağlıksız değerlendirmeler uygulama yükseltme durumu da bulunur.

Aşağıdaki uygulama yükseltme durumu için değiştirilmiş bir doku gösterir: / WordCount uygulamasını. Bir izleme çoğaltmalarını birinde bir hata bildirdi. Sistem durumu denetimleri değil geri uymaya çünkü yükseltme sunulmaktadır.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2017 5:23:26 PM
FailureTimestampUtc           : 4/21/2017 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Daha fazla bilgi edinin [Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Sorun gidermek için sistem durumu değerlendirme kullanma
Kümeyi veya bir uygulama ile ilgili bir sorun olduğunda, sorunun ne olduğunu saptamak için Küme ve uygulama sağlığı arayın. Sağlıksız değerlendirmeler neyin tetiklediği geçerli iyi olmayan sistem durumu hakkında ayrıntılar sağlanmaktadır. Gerekirse, sağlam olmayan alt varlıklara kök nedenini belirlemek için detaya gidebilirsiniz.

Örneğin, bir hata raporu çoğaltmalarını birinde olduğundan bir uygulama sağlıksız düşünün. Aşağıdaki Powershell cmdlet'ini sağlıksız değerlendirmeler gösterir:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricApplicationHealth fabric:/WordCount -EventsFilter None -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            : 
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                    Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                    Unhealthy partition: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', AggregatedHealthState='Error'.

                                        Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                        Unhealthy replica: PartitionId='af2e3e44-a8f8-45ac-9f31-4093eb897600', ReplicaOrInstanceId='131444422260002646', AggregatedHealthState='Error'.

                                            Error event: SourceId='MyWatchdog', Property='Memory'.

ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : None
```

Daha fazla bilgi için çoğaltma bakabilirsiniz:

```powershell
PS D:\ServiceFabric> Get-ServiceFabricReplicaHealth -ReplicaOrInstanceId 131444422260002646 -PartitionId af2e3e44-a8f8-45ac-9f31-4093eb897600


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422260002646
AggregatedHealthState : Error
UnhealthyEvaluations  : 
                        Error event: SourceId='MyWatchdog', Property='Memory'.

HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131444422263668344
                        SentAt                : 7/13/2017 5:57:06 PM
                        ReceivedAt            : 7/13/2017 5:57:18 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_2
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : MyWatchdog
                        Property              : Memory
                        HealthState           : Error
                        SequenceNumber        : 131444451657749403
                        SentAt                : 7/13/2017 6:46:05 PM
                        ReceivedAt            : 7/13/2017 6:46:05 PM
                        TTL                   : Infinite
                        Description           : 
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 7/13/2017 6:46:05 PM, LastOk = 1/1/0001 12:00:00 AM
```

> [!NOTE]
> Sağlıksız değerlendirmeler ilk nedeni varlığın geçerli durumu değerlendirilir gösterir. Bunlar değil olması yansıtılır değerlendirmeleri ancak bu durum tetikleme birden çok olay olabilir. Daha fazla bilgi için kümedeki iyi durumda olmayan tüm raporların bulmak için sistem durumu varlıklarını detayına.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Özel Service Fabric durum raporları ekleme](service-fabric-report-health.md)

[Hizmet durumunu raporlama ve denetleme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md)
