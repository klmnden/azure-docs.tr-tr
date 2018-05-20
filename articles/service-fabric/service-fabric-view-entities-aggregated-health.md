---
title: Sistem durumu Azure Service Fabric varlıkları görüntülemek nasıl bir araya getirilir | Microsoft Docs
description: Sorgu, görüntülemek ve sistem durumu sorgularının ve genel sorgular aracılığıyla Azure Service Fabric varlıkları toplanan sistem değerlendirmek açıklar.
services: service-fabric
documentationcenter: .net
author: oanapl
manager: timlt
editor: ''
ms.assetid: fa34c52d-3a74-4b90-b045-ad67afa43fe5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 2e5d1045edbbc3c71cb0ccff34d2ba327a98a409
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="view-service-fabric-health-reports"></a>Service Fabric sistem durumu raporlarını görüntüle
Azure Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) hangi sistem bileşenleri ve watchdogs yerel koşulları rapor için sistem durumu varlıkları ile bunların izlemekte olduğunuz. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıklar sağlıklı olup olmadığını belirlemek için tüm sistem durumu verileri toplar.

Küme, sistem bileşenleri tarafından gönderilen sistem durumu raporları ile birlikte otomatik olarak doldurulur. Ayrıntılı bilgi için [sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric varlıkları toplanmış durumunu almak için birden çok yol sunar:

* [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) veya diğer görsel Araçlar
* Sistem durumu sorgularının (aracılığıyla, PowerShell, API veya REST)
* Genel sistem durumu (aracılığıyla, PowerShell, API veya REST) özelliklerinden biri olarak varlıklar, bu dönüş listesini sorgular

Bu seçenekler göstermek için yerel bir küme ile beş düğüm kullanalım ve [fabric: / WordCount uygulamasını](http://aka.ms/servicefabric-wordcountapp). **Fabric: / WordCount** iki varsayılan hizmet, bir durum bilgisi olan hizmet türü içeren uygulama `WordCountServiceType`ve durum bilgisiz hizmet türü `WordCountWebServiceType`. Değiştirdim `ApplicationManifest.xml` yedi hedeflemek için durum bilgisi olan hizmet ve bir bölüm kopyalarını gerektirecek şekilde. Kümede yalnızca beş düğümü olduğundan, sistem bileşenleri hizmet bölüme bir uyarı hedef sayısı olduğundan bildirin.

```xml
<Service Name="WordCountService">
  <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
    <UniformInt64Partition PartitionCount="[WordCountService_PartitionCount]" LowKey="1" HighKey="26" />
  </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Service Fabric Explorer'da sistem durumu
Service Fabric Explorer kümenin görsel görünümünü sağlar. Aşağıdaki resimde, görebilirsiniz:

* Uygulama **fabric: / WordCount** tarafından raporlanan bir hata olayı olduğundan (hata) kırmızıdır **MyWatchdog** özelliği için **kullanılabilirlik**.
* Kendi hizmetlerden biri **fabric: / WordCount/WordCountService** sarı (uyarı). Hizmet yedi yinelemelerle yapılandırılır ve iki repicas yerleştirilemez şekilde kümesinin beş düğümü vardır. Burada gösterilmese hizmeti sistem rapordan nedeniyle sarı bölümdür `System.FM` olduğunu belirten `Partition is below target replica or instance count`. Sarı bölüm sarı hizmet tetikler.
* Küme kırmızı nedeniyle kırmızı uygulamasıdır.

Değerlendirme varsayılan ilkelerden küme bildirimi ve uygulama bildirimi kullanır. Bunlar, katı ilkeleri ve herhangi bir hata tolerans değil.

Service Fabric Explorer ile küme görünümü:

![Service Fabric Explorer ile küme görüntüleyin.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [!NOTE]
> Daha fazla bilgi edinin [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

## <a name="health-queries"></a>Sistem durumu sorgularının sayısı
Service Fabric her desteklenen biri için sistem durumu sorgularının sunan [varlık türleri](service-fabric-health-introduction.md#health-entities-and-hierarchy). Yöntemleri kullanarak API aracılığıyla erişilebilir [FabricClient.HealthManager](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlet'leri ve REST. Bu sorguları varlık tüm sistem bilgilerini döndürür: toplanan sistem durumu, varlık sistem durumu olayları, alt sistem durumlarını (varsa), (varlık sağlıklı olmadığında) sağlıksız değerlendirmeleri ve alt sistem durumu istatistikleri (varsa).

> [!NOTE]
> Health store içinde tam olarak doldurulan bir sistem durumu varlık döndürülür. Varlık (silinmez) etkin olması ve sistem raporu olması gerekir. Hiyerarşi zincirini kendi üst varlıklarını ayrıca sistem raporları sahip olmalıdır. Bu koşulların herhangi biri değil sağlanırsa, sistem return sorgular bir [FabricException](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception) ile [FabricErrorCode](https://docs.microsoft.com/dotnet/api/system.fabric.fabricerrorcode) `FabricHealthEntityNotFound` gösterir neden varlık alınmadı.
>
>

Sistem durumu sorgularının varlık türüne bağlıdır varlık tanımlayıcısı geçmesi gerekir. Sorguları isteğe bağlı sistem durumu ilkesi parametreleri kabul eder. Sistem durumu ilkeleri belirtilirse, [sistem durumu ilkeleri](service-fabric-health-introduction.md#health-policies) küme veya uygulama bildirimden değerlendirmesi için kullanılır. Sistem durumu ilkeleri için bir tanım bildirimleri içermiyorsa, varsayılan sistem durumu ilkeleri değerlendirme için kullanılır. Varsayılan sistem durumu ilkeleri hataları tolerans değil. Sorgular yalnızca kısmi alt veya olaylardan belirtilen filtrelerle saygı olanları döndürmek için filtreler de kabul eder. Başka bir filtre alt istatistikleri hariç izin verir.

> [!NOTE]
> İleti yanıt boyutu azaltılmış şekilde çıkış filtreleri sunucu tarafında uygulanır. Döndürülen verileri sınırlamak için çıkış filtreleri kullanmak yerine istemci tarafında filtre uygulamak önerilir.
>
>

Bir varlığın durumu içerir:

* Varlık toplanan sistem durumu. Varlık sistem durumu raporları, alt sistem durumlarını (varsa) ve sistem durumu ilkeleri göre sistem durumu depoya göre hesaplanır. Daha fazla bilgi edinin [varlık sistem durumu değerlendirmesi](service-fabric-health-introduction.md#health-evaluation).  
* Varlık sistem durumu olayları.
* Sistem durumlarını alt bulunabilir varlıklar için tüm alt öğelerinin koleksiyonu. Sistem durumlarından varlık tanımlayıcıları ve toplanan sistem durumu içerir. Tüm sistem için bir alt almak için alt varlık türü için sorgu sistem çağrı ve alt tanımlayıcıda geçirin.
* Varlık sağlam değilse varlık durumunu tetiklenen raporun noktası sağlıksız değerlendirmeleri. Değerlendirmeleri özyinelemeli, geçerli sistem durumu tetiklenen alt sistem durumu değerlendirmesini içeren ' dir. Örneğin, bir izleme bir çoğaltma karşı bir hata bildirdi. Uygulama Durumu sağlıksız bir hizmet nedeniyle sağlıksız bir değerlendirme gösterir; Hizmet bir bölümünde bir hata nedeniyle sağlıksız olduğunu; bölümün bir çoğaltma hatası nedeniyle sağlıksız olduğunu; çoğaltmayı izleme hata sistem durumu raporu nedeniyle sağlam değil.
* Alt varlıklar tüm alt türleri için sistem durumu istatistikleri. Örneğin, küme durumu uygulamalar, hizmetler, bölümler, çoğaltmaları toplam sayısını gösterir ve küme varlıklarda dağıtılabilir. Hizmet durumu bölümler ve çoğaltmalar belirtilen hizmet altında toplam sayısını gösterir.

## <a name="get-cluster-health"></a>Küme durumu Al
Küme varlık durumunu döndürür ve uygulamaları ve (alt küme) düğümleri sistem durumlarını içerir. Giriş:

* [İsteğe bağlı] Düğümleri ve küme olayları değerlendirmek için kullanılan Küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama durumu ilkesi haritası, uygulama bildirim ilkeleri geçersiz kılmak için kullanılan sistem durumu ilkeleri.
* [İsteğe bağlı] Olaylar, düğümleri ve hangi girişlerin belirtin uygulamaları filtreler ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olayları, düğümleri ve uygulamaları filtre bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistikleri dışlamak için filtreleyin.
* [İsteğe bağlı] Doku içerecek şekilde filtre: / sistem durumu istatistiklerine sağlık istatistikleri. Yalnızca sistem durumu istatistikleri değil çıkarıldığında geçerlidir. Varsayılan olarak, sistem durumu İstatistikler yalnızca kullanıcı uygulamaları ve sistem uygulaması için istatistikleri içerir.

### <a name="api"></a>API
Küme durumu almak için oluşturma bir `FabricClient` ve arama [GetClusterHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthasync) yöntemi kendi **HealthManager**.

Aşağıdaki çağrıyı küme sistem alır:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Aşağıdaki kod, düğümleri ve uygulamaları için özel küme sistem durumu ilkesi ve filtreleri kullanarak küme durumu alır. Sistem durumu istatistikleri doku içerdiğini belirtir: / Sistem istatistikleri. Oluşturduğu [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthquerydescription), giriş bilgilerini içerir.

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
Küme durumu almak için cmdlet [Get-ServiceFabricClusterHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Küme beş düğümü, sistem uygulaması ve doku durumda: / WordCount açıklandığı şekilde yapılandırılmış.

Aşağıdaki cmdlet'i, varsayılan sistem durumu ilkeleri kullanarak küme durumu alır. Toplanan sistem durumu uyarı, çünkü konumunda fabric: / WordCount uygulamasını uyarı. Sağlıksız değerlendirmeleri Ayrıntıları toplanan sistem tetikleyen koşullara nasıl sağladığını unutmayın.

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

Aşağıdaki PowerShell cmdlet'ini bir özel uygulama İlkesi kullanarak küme durumunu alır. Yalnızca uygulama ve düğümler hata veya uyarı almak için sonuçları filtreler. Tüm sağlıklı olarak sonuç olarak, düğüm, döndürülür. Yalnızca fabric: / WordCount uygulamasını uygulamaları filtre dikkate alır. Özel ilke doku için uyarıları hata olarak dikkate alınması gereken belirttiğinden: / WordCount uygulama hata olduğu gibi değerlendirilir ve bu nedenle kümedir.

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
Küme durumu ile alabilirsiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-node-health"></a>Düğüm durumu Al
Bir düğüm varlık durumunu döndürür ve düğüm üzerinde bildirilen sistem durumu olayları içerir. Giriş:

* [Gerekli] Düğüm tanımlayan düğüm adı.
* [İsteğe bağlı] Sistem durumunu değerlendirmek için kullanılan Küme sistem durumu ilkesi ayarlar.
* [İsteğe bağlı] Hangi girişlerin belirten olaylar için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olaylar filtresi bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.

### <a name="api"></a>API
API aracılığıyla düğüm durumu almak için oluşturma bir `FabricClient` ve arama [GetNodeHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getnodehealthasync) kendi HealthManager yöntemi.

Aşağıdaki kod, düğüm durumu belirtilen düğümün adı için alır:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Aşağıdaki kod, düğüm durumu belirtilen düğümün adını alır ve geçirir olayları filtresi ve özel İlkesi aracılığıyla [NodeHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.nodehealthquerydescription):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Düğüm durumu almak için cmdlet [Get-ServiceFabricNodeHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnodehealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.
Aşağıdaki cmdlet'i, varsayılan sistem durumu ilkeleri kullanarak düğüm durumu alır:

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
Düğüm durumu ile alabileceğiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node) veya bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-node-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-application-health"></a>Uygulama durumunu Al
Bir uygulama varlık durumunu döndürür. Dağıtılan uygulama ve hizmet alt sistem durumlarını içerir. Giriş:

* [Gerekli] Uygulamayı tanımlar uygulaması adı (URI).
* [İsteğe bağlı] Uygulama bildirim ilkeleri geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olaylar, hizmetler ve hangi girişlerin belirtin dağıtılan uygulamalar için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olaylar, hizmetler ve dağıtılan uygulamalar filtre bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistikleri dışlamak için filtreleyin. Belirtilmezse, sistem durumu istatistikleri Tamam, uyarı ve hata sayısı için tüm uygulama alt öğeleri içerir: Hizmetler, bölümler, çoğaltmalar, dağıtılan uygulamalar ve hizmet paketleri dağıtılabilir.

### <a name="api"></a>API
Uygulama durumunu almak için oluşturma bir `FabricClient` ve arama [GetApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getapplicationhealthasync) kendi HealthManager yöntemi.

Aşağıdaki kod uygulama sistem durumu için belirtilen uygulama adı (URI) alır:

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Aşağıdaki kod filtrelerle uygulama sistem durumu için belirtilen uygulama adı (URI) alır ve özel ilkelerinde belirtilen aracılığıyla [ApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.applicationhealthquerydescription).

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
Uygulama durumunu almak için cmdlet [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i sistem durumunu verir **fabric: / WordCount** uygulama:

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

Aşağıdaki PowerShell cmdlet'ini özel ilkelerinde geçirir. Ayrıca, alt ve olayları filtreler.

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
Uygulama health ile alabilirsiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-an-application-by-using-an-application-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-service-health"></a>Hizmet durumunu Al
Bir hizmet varlık durumunu döndürür. Bölüm sistem durumlarını içerir. Giriş:

* [Gerekli] Hizmeti tanımlar hizmet adı (URI).
* [İsteğe bağlı] Uygulama bildirim ilkesi geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin belirtmek bölümleri için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olayları ve bölüm filtresi bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistikleri dışlamak için filtreleyin. Aksi takdirde hata sayısı tüm bölümler ve çoğaltmalar hizmetinin ve belirtilen sağlık istatistikleri uyarı, Tamam ' gösterilmektedir.

### <a name="api"></a>API
API aracılığıyla hizmet durumunu almak için oluşturma bir `FabricClient` ve arama [GetServiceHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getservicehealthasync) kendi HealthManager yöntemi.

Aşağıdaki örnek, belirtilen hizmet adı (URI) ile bir hizmeti alır:

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Aşağıdaki kod belirtme filtreleri ve özel ilke aracılığıyla hizmet durumu belirtilen hizmet adının (URI) alır [ServiceHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.servicehealthquerydescription):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Hizmetin sistem durumunu almak için cmdlet [Get-ServiceFabricServiceHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicehealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i, varsayılan sistem durumu ilkeleri kullanarak hizmet durumu alır:

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
Hizmet durumu ile alabileceğiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service) veya bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-partition-health"></a>Bölüm durumu Al
Bir bölüm varlık durumunu döndürür. Çoğaltma sistem durumlarını içerir. Giriş:

* [Gerekli] Bölüm bölüm tanımlayan kimlik (GUID).
* [İsteğe bağlı] Uygulama bildirim ilkesi geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin belirtin çoğaltmaları için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olayları ve çoğaltmaları filtre bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistikleri dışlamak için filtreleyin. Belirtilmezse, kaç tane çoğaltmaları Tamam, uyarı ve hata durumları olan sistem durumu istatistikleri gösterir.

### <a name="api"></a>API
API aracılığıyla bölüm durumu almak için oluşturma bir `FabricClient` ve arama [GetPartitionHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getpartitionhealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreler belirtmek için Oluştur [PartitionHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.partitionhealthquerydescription).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Bölüm sistem almak için cmdlet [Get-ServiceFabricPartitionHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricpartitionhealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i tüm bölümler, sistem alır **fabric: / WordCount/WordCountService** hizmeti ve filtreleri çoğaltma sistem durumlarını çıkışı:

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
Bölüm health ile alabileceğiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition) veya bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-partition-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-replica-health"></a>Çoğaltma durumu Al
Sistem durum bilgisi olan hizmet çoğaltma veya bir durum bilgisiz hizmet örneği durumunu döndürür. Giriş:

* [Gerekli] Çoğaltma tanımlayan bölüm kimlik (GUID) ve çoğaltma kimliği.
* [İsteğe bağlı] Uygulama bildirim ilkeleri geçersiz kılmak için kullanılan uygulama sistem durumu ilkesi parametreleri.
* [İsteğe bağlı] Hangi girişlerin belirten olaylar için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olaylar filtresi bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.

### <a name="api"></a>API
Çoğaltma durumu API aracılığıyla almak için oluşturma bir `FabricClient` ve arama [GetReplicaHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getreplicahealthasync) kendi HealthManager yöntemi. Gelişmiş parametreleri belirtmek için kullanın [ReplicaHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.replicahealthquerydescription).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Çoğaltma durumu almak için cmdlet [Get-ServiceFabricReplicaHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricreplicahealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Aşağıdaki cmdlet'i hizmetinin tüm bölümler için birincil çoğaltma durumunu alır:

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
Çoğaltma sistem durumu ile alabileceğiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica) veya bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-replica-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-deployed-application-health"></a>Dağıtılmış uygulama durumunu Al
Düğümün varlık üzerinde dağıtılan bir uygulama durumunu döndürür. Dağıtılan hizmet paketi sistem durumlarını içerir. Giriş:

* [Gerekli] Uygulama adı (URI) ve düğüm adı (dize) dağıtılmış uygulamanın tanımlayan.
* [İsteğe bağlı] Uygulama bildirim ilkeleri geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Olayları ve hangi girişlerin belirtin dağıtılmış hizmet paketleri için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olayları ve dağıtılmış hizmet paketleri, filtre bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.
* [İsteğe bağlı] Sistem durumu istatistikleri dışlamak için filtreleyin. Belirtilmezse, sistem durumu istatistikleri Tamam, uyarı ve hata sistem durumlarının dağıtılmış hizmet paketleri sayısını gösterir.

### <a name="api"></a>API
Bir düğüm API'si aracılığıyla dağıtılan bir uygulama durumunu almak için oluşturma bir `FabricClient` ve arama [GetDeployedApplicationHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreler belirtmek için kullanın [DeployedApplicationHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedapplicationhealthquerydescription).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Dağıtılmış uygulama durumunu almak için cmdlet [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i. Uygulamanın dağıtıldığı bulmak için Çalıştır [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) ve dağıtılmış uygulama alt öğeler bakın.

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
Dağıtılmış uygulama sistem durumu ile alabilirsiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-deployed-application-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="get-deployed-service-package-health"></a>Dağıtılan hizmet paketi durumu Al
Dağıtılan hizmet paketi varlık durumunu döndürür. Giriş:

* [Gerekli] Dağıtılan hizmet paketi belirlemek uygulama adı (URI), düğüm adı (dize) ve hizmet bildirim adı (dize).
* [İsteğe bağlı] Uygulama bildirim ilkesi geçersiz kılmak için kullanılan uygulama durumu ilkesi.
* [İsteğe bağlı] Hangi girişlerin belirten olaylar için filtreleri ilgilendiğiniz ve sonuçta (örneğin, yalnızca, hatalar veya uyarılar ve hatalar) döndürülmelidir. Tüm olaylar filtresi bağımsız olarak toplanan varlık durumunu değerlendirmek için kullanılır.

### <a name="api"></a>API
API aracılığıyla dağıtılan hizmet paketi durumunu almak için oluşturma bir `FabricClient` ve arama [GetDeployedServicePackageHealthAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync) kendi HealthManager yöntemi. İsteğe bağlı parametreler belirtmek için kullanın [DeployedServicePackageHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.deployedservicepackagehealthquerydescription).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Dağıtılan hizmet paketi durumu almak için cmdlet [Get-ServiceFabricDeployedServicePackageHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicepackagehealth). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i. Uygulamanın dağıtıldığı görmek için çalıştırın [Get-ServiceFabricApplicationHealth](/powershell/module/servicefabric/get-servicefabricapplicationhealth?view=azureservicefabricps) ve dağıtılan uygulamaları arayın. Dağıtılan hizmet paketi alt öğeleri bakmak paketlerdir bir uygulamada hangi hizmet görmek için [Get-ServiceFabricDeployedApplicationHealth](/powershell/module/servicefabric/get-servicefabricdeployedapplicationhealth?view=azureservicefabricps) çıktı.

Aşağıdaki cmdlet'i durumunu alır **WordCountServicePkg** hizmet paketi **fabric: / WordCount** dağıtılan uygulama **_Node_2**. Varlık **System.Hosting** başarılı hizmet paketi ve giriş noktası etkinleştirme ve başarılı hizmet türü kaydı için raporları.

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
Dağıtılan hizmet paketi durumu ile alabileceğiniz bir [GET isteğini](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package) veya bir [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-service-package-by-using-a-health-policy) gövdesinde açıklanan sistem durumu ilkeleri içerir.

## <a name="health-chunk-queries"></a>Sistem durumu öbek sorguları
Sistem durumu öbek sorgularının giriş filtreleri başına (yinelemeli olarak), çok düzeyli Küme alt öğeleri geri dönebilirsiniz. Büyük bir esnekliğe döndürülecek alt seçerken izin Gelişmiş Filtreler destekler. Filtreler alt benzersiz tanımlayıcısı veya diğer grup kimlikleri ve/veya sistem durumlarını tarafından belirtebilirsiniz. Varsayılan olarak, hiç alt öğe, her zaman birinci düzeydeki alt öğeleri dahil sistem durumu komutları aksine dahil edilir.

[Sistem durumu sorgularının](service-fabric-view-entities-aggregated-health.md#health-queries) yalnızca ilk düzeyi alt belirtilen varlık gerekli filtreleri başına döndürür. Alt öğenin alt öğelerine almak için ilgi her bir varlık için ek sistem durumu API'leri çağırmanız gerekir. Benzer şekilde, belirli varlıkların durumunu almak için istenen her varlık için bir sistem durumu API çağırmanız gerekir. Filtreleme Gelişmiş öbek sorgu ileti boyutu ve ileti sayısını en aza bir sorgu ilgi birden çok öğe istemenizi sağlar.

Öbek sorgu, sistem durumu daha fazla küme varlıklar (gerekli kök dizininde başlayan olası tüm küme varlıklar) için tek çağrıda alabilirsiniz, değeri. Karmaşık sistem durumu sorgusu gibi ifade edebilirsiniz:

* Dönüş yalnızca uygulamalar hata ve bu uygulamalar için uyarı veya hata tüm hizmetleri içerir. Döndürülen hizmetler için tüm bölümleri içerir.
* Yalnızca adlarıyla belirtilen dört uygulamaların durumunu döndürür.
* Yalnızca istenen uygulama türünde uygulamalar durumunu döndürür.
* Tüm dağıtılan varlık, bir düğümde döndür. Tüm uygulamalar döndürür tüm dağıtılmış uygulamalar belirtilen düğümde ve bu düğümdeki tüm dağıtılmış hizmet paketleri.
* Tüm çoğaltmaları hata döndürür. Tüm uygulamalar, hizmetler, bölümler ve yalnızca çoğaltmalar hata döndürür.
* Tüm uygulamaları döndür. Belirtilen bir hizmeti için tüm bölümleri içerir.

Şu anda, yalnızca küme varlık için sistem durumu öbek sorgu açıktır. İçeren bir küme durumu öbek döndürür:

* Sistem durumu kümeyi bir araya getirilir.
* Giriş filtreleri saygı düğümleri sağlık durumu öbek listesi.
* Sistem durumu öbek giriş filtreleri dikkate uygulamaların listesi. Her uygulama sistem durumu öbek giriş filtreleri ve filtreleri saygı tüm dağıtılmış uygulamaları öbek listesiyle saygı tüm hizmetleri ile öbek listesini içerir. Aynı alt hizmetler ve dağıtılan uygulamalar için. Bu şekilde, kümedeki tüm varlıklar olası istediyseniz, hiyerarşik bir biçimde döndürülebilir.

### <a name="cluster-health-chunk-query"></a>Küme durumu öbek sorgu
Küme varlık sistem durumu verir ve gerekli alt hiyerarşik sağlık durumu parçalarını içerir. Giriş:

* [İsteğe bağlı] Düğümleri ve küme olayları değerlendirmek için kullanılan Küme sistem durumu ilkesi.
* [İsteğe bağlı] Uygulama durumu ilkesi haritası, uygulama bildirim ilkeleri geçersiz kılmak için kullanılan sistem durumu ilkeleri.
* [İsteğe bağlı] Düğümler ve hangi girişlerin belirtin uygulamalar için filtreleri ilgilendiğiniz ve sonuçta döndürülmelidir. Filtreler belirli bir varlık/grubuna varlıkların veya o düzeyde tüm varlıklar için geçerlidir. Filtrelerin listesi bir genel filtre ve/veya sorgu tarafından döndürülen ayrıntılı varlıklara belirli tanımlayıcıları için filtreleri içerebilir. Alt öğe boş ise, varsayılan olarak döndürülmez.
  Filtreler hakkında daha fazla bilgiyi [NodeHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.nodehealthstatefilter) ve [ApplicationHealthStateFilter](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthstatefilter). Uygulama Filtreleri can yinelemeli çocuklar için Gelişmiş filtreler belirtin.

Öbek sonuç filtreleri saygı alt öğeleri içerir.

Şu anda, öbek sorgu sağlıksız değerlendirmeleri veya varlık olaylarını döndürmüyor. Bu ek bilgileri varolan küme sistem durumu sorgusu kullanılarak edinilebilir.

### <a name="api"></a>API
Küme durumu öbek almak için oluşturma bir `FabricClient` ve arama [GetClusterHealthChunkAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync) yöntemi kendi **HealthManager**. İçinde geçirebilirsiniz [ClusterHealthQueryDescription](https://docs.microsoft.com/dotnet/api/system.fabric.description.clusterhealthchunkquerydescription) sistem durumu ilkeleri açıklamak için ve Gelişmiş filtreleri.

Aşağıdaki kod ile Gelişmiş Filtreler küme durumu öbek alır.

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
Küme durumu almak için cmdlet [Get-ServiceFabricClusterChunkHealth](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclusterhealthchunk). İlk olarak, kullanarak kümeye bağlanın [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet'i.

Hata dışında her zaman döndürülmesi gereken belirli bir düğüm, yalnızca olmaları durumunda aşağıdaki kodu düğümleri alır.

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

Aşağıdaki cmdlet'i küme Öbek ile uygulama filtrelerini alır.

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

Aşağıdaki cmdlet'i bir düğümde dağıtılan tüm varlıkları döndürür.

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
Küme durumu Öbek ile alabileceğiniz bir [GET isteği](https://docs.microsoft.com/rest/api/servicefabric/get-the-health-of-a-cluster-using-health-chunks) veya [POST isteği](https://docs.microsoft.com/rest/api/servicefabric/health-of-cluster) sistem durumu ilkeleri ve Gelişmiş filtreleri gövdesinde açıklanan içerir.

## <a name="general-queries"></a>Genel sorgular
Genel sorguları belirtilen bir türün Service Fabric varlıkların listesini döndürür. API aracılığıyla sunulur (yöntemleri aracılığıyla **FabricClient.QueryManager**), PowerShell cmdlet'leri ve REST. Bu sorgular, alt sorgular birden çok bileşenlerini toplama. Bunlardan biri olan [sistem durumu deposu](service-fabric-health-introduction.md#health-store), toplanan sistem durumu için her bir sorgu sonucu doldurur.  

> [!NOTE]
> Genel sorgular varlık toplanan sistem durumu dönün ve zengin sistem durumu verileri içermez. Bir varlık sağlıklı durumda değilse, olaylar, alt sistem durumları ve sağlıksız değerlendirmeleri dahil olmak üzere tüm kendi sistem durumu bilgilerini almak için sistem durumu sorgularının ile izlenebilir.
>
>

Genel sorgular bir varlık için bir bilinmeyen sistem durumu döndürürse, sistem sağlığı deposunu varlık hakkında tam veri yok mümkündür. Ayrıca sistem durumu depolama için bir alt sorgu başarısız oldu mümkündür (örneğin, bir iletişim hatası oluştu veya sistem durumu deposu kısıtlanan). Varlık için bir sistem durumu sorgusu ile izle. Alt ağ sorunları gibi geçici bir hatayla karşılaştı, bu izleme sorgu başarılı olabilir. Bu aynı zamanda, daha fazla ayrıntı neden varlık sunulmaz hakkında health Store'dan verebilir.

İçeren sorgular **HealthState** varlıklar için:

* Düğüm listesi: liste düğümleri (disk belleği) kümesini döndürür.
  * API: [FabricClient.QueryClient.GetNodeListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getnodelistasync)
  * PowerShell: Get-ServiceFabricNode
* Uygulama listesi: (disk belleği) kümedeki uygulamaların listesini döndürür.
  * API: [FabricClient.QueryClient.GetApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync)
  * PowerShell: Get-ServiceFabricApplication
* Hizmet listesi: (disk belleği) bir uygulamada hizmetlerin listesini döndürür.
  * API: [FabricClient.QueryClient.GetServiceListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync)
  * PowerShell: Get-ServiceFabricService
* Bölüm listesi: (disk belleği) bir hizmet olarak bölümler listesini döndürür.
  * API: [FabricClient.QueryClient.GetPartitionListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getpartitionlistasync)
  * PowerShell: Get-ServiceFabricPartition
* Çoğaltma Listesi: (disk belleği) bölümü çoğaltmaların listesini döndürür.
  * API: [FabricClient.QueryClient.GetReplicaListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getreplicalistasync)
  * PowerShell: Get-ServiceFabricReplica
* Uygulama listesi dağıtılan: bir düğümde dağıtılan uygulamalar listesini döndürür.
  * API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication
* Hizmet paketi listesi dağıtılan: dağıtılan bir uygulama hizmeti paketlerin listesini döndürür.
  * API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync)
  * PowerShell: Get-ServiceFabricDeployedApplication

> [!NOTE]
> Bazı sorgular, disk belleğine alınan sonuçları döndürür. Bu sorguları dönüş türetilmiş bir listedir [PagedList<T>](https://docs.microsoft.com/dotnet/api/system.fabric.query.pagedlist-1). Sonuçları bir ileti uymuyorsa yalnızca bir sayfa döndürülür ve bir ContinuationToken, numaralandırma durduğu izler. Aynı sorgu çağırın ve sonraki sonuçlar elde etmek için önceki sorgudan devamlılık belirteci geçirin devam edin.
>
>

### <a name="examples"></a>Örnekler
Aşağıdaki kod, kümede sağlıksız uygulamaları alır:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Aşağıdaki cmdlet'i bir yapı için uygulama ayrıntıları alır: / WordCount uygulamasını. Sistem durumu uyarı konumunda olduğuna dikkat edin.

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

## <a name="cluster-and-application-upgrades"></a>Küme ve uygulama yükseltme
Küme ve uygulama izlenen bir yükseltme sırasında Service Fabric her şeyi sağlıklı kalmasını sağlamak için sistem durumu denetler. Bir varlık olarak yapılandırılmış sistem durumu ilkeleri kullanılarak hesaplanan sağlıksız ise, yükseltme sonraki eylemi belirlemek için yükseltme özgü ilkelerini uygular. Kullanıcı etkileşimi (örneğin, hata koşullarını düzelttikten veya ilkelerini değiştirme) izin verecek şekilde yükseltme duraklatılmış olabilir veya bu otomatik olarak önceki iyi sürüme geri.

Sırasında bir *küme* yükseltme, Küme yükseltme durumunu alma. Yükseltme durumu nedir kümede sağlıksız noktası sağlıksız değerlendirmeleri içerir. Sistem durumu sorunları nedeniyle yükseltme geri alınması durumunda, yükseltme durumu son sağlıksız nedenleri hatırlıyor. Bu bilgileri yöneticilerin yükseltme geri alındı veya durduruldu sonra nelerin yanlış gittiğini araştırmanıza yardımcı olabilir.

Benzer şekilde, sırasında bir *uygulama* yükseltme, tüm sağlıksız değerlendirmeleri uygulama yükseltme durumu da bulunur.

Aşağıdaki değiştirilmiş bir yapı için uygulama yükseltme durumunu gösterir: / WordCount uygulamasını. Bir izleme çoğaltmalarını birinde bir hata bildirdi. Sistem durumu denetimini değil geri dikkate alır çünkü yükseltme alınıyor.

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

Daha fazla bilgi edinin [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Sorun gidermek için sistem durumu değerlendirmesini kullanın
Küme veya bir uygulama ile ilgili bir sorun olduğunda, sorunun ne olduğunu saptamak için Küme veya uygulama sistem arayın. Sağlıksız değerlendirmeleri ne geçerli sağlıksız bir duruma tetiklendiği ile ilgili ayrıntılı bilgiler sağlar. Gerekiyorsa, kök nedenini belirlemek için sağlam olmayan alt varlıklarının içinde ayrıntıya girebilirsiniz.

Örneğin, bir hata raporu çoğaltmalarını birinde olduğundan bir uygulama sağlıksız düşünün. Aşağıdaki Powershell cmdlet'ini sağlıksız değerlendirmeleri gösterir:

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

Daha fazla bilgi almak için yinelemede arayabilirsiniz:

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
> İlk neden varlık için geçerli sistem durumu değerlendirilir sağlıksız değerlendirmeleri gösterir. Bu durum tetiklemek birden çok olay olabilir, ancak bunlar değil olması yansıtılır değerlendirmeleri. Daha fazla bilgi için kümedeki tüm sağlıksız raporları ekleneceğini sistem durumu varlıkları detaya.
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Özel Service Fabric sistem durumu rapor ekleme](service-fabric-report-health.md)

[Nasıl rapor ve hizmetin sistem durumunu denetle](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[İzleme ve Hizmetleri yerel olarak tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md)
