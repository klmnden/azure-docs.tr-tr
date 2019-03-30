---
title: Azure'da bir Service Fabric kümesini izleme | Microsoft Docs
description: Bu öğreticide, Service Fabric olayları görüntüleme, EventStore API'leri sorgulama, performans sayaçlarını izleme ve sistem durumu raporlarını görüntüleme bir kümesini izleme konusunda bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/13/2019
ms.author: srrengar
ms.custom: mvc
ms.openlocfilehash: 62ab98d2279380df33657967c55bf7fb4d36da43
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662457"
---
# <a name="tutorial-monitor-a-service-fabric-cluster-in-azure"></a>Öğretici: Azure'da bir Service Fabric kümesini izleme

İzleme ve tanılama geliştirme, test ve herhangi bir bulut ortamında iş yüklerini dağıtma için kritik öneme sahiptir. Bu öğretici bir serinin ikinci kısmı parçasıdır ve izleme ve tanılama olayları, performans sayaçları ve sağlık raporları kullanarak bir Service Fabric kümesi gösterilmektedir.   Daha fazla bilgi için genel bakış okuyun [küme izleme](service-fabric-diagnostics-overview.md#platform-cluster-monitoring) ve [altyapı izleme](service-fabric-diagnostics-overview.md#infrastructure-performance-monitoring).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Service Fabric olaylarını görüntüle
> * Küme olayları için sorgu EventStore API'leri
> * Altyapı/Topla performans sayacı izleme
> * Küme sistem durumu raporlarını görüntüleme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Güvenli oluşturma [Windows Küme](service-fabric-tutorial-create-vnet-and-windows-cluster.md) bir şablonu kullanarak azure'da
> * Bir kümesini izleme
> * [Bir kümenin ölçeğini daraltma veya genişletme](service-fabric-tutorial-scale-cluster.md)
> * [Bir kümenin çalışma zamanını yükseltme](service-fabric-tutorial-upgrade-cluster.md)
> * [Küme silme](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Azure Powershell modülü sürüm 4.1 veya üzerini](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) ya da [Azure CLI](/cli/azure/install-azure-cli)'yı yükleyin.
* Güvenli oluşturma [Windows kümesi](service-fabric-tutorial-create-vnet-and-windows-cluster.md) 
* Kurulum [tanılama koleksiyonu](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configurediagnostics_anchor) küme için
* Etkinleştirme [Eventstore'a hizmet](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureeventstore_anchor) kümedeki
* Yapılandırma [Azure İzleyici günlüklerine ve Log Analytics aracısını](service-fabric-tutorial-create-vnet-and-windows-cluster.md#configureloganalytics_anchor) küme için

## <a name="view-service-fabric-events-using-azure-monitor-logs"></a>Azure İzleyici günlüklerine kullanarak Service Fabric olaylarını görüntüle

Azure izleme günlükleri toplar ve uygulamalardan ve bulut üzerinde barındırılan hizmetlerden daha fazla telemetri analiz eder ve kullanılabilirliği ve performansı en üst düzeye çıkarmanıza yardımcı olması için analiz araçları sağlar. Azure İzleyici günlüklerine Öngörüler edinin ve neler kümenizde sorun giderme için sorguları çalıştırabilirsiniz.

Service Fabric analizi çözümü erişmek için Git [Azure portalında](https://portal.azure.com) ve Service Fabric analizi çözümü oluşturduğunuz kaynak grubunu seçin.

Kaynak seçin **ServiceFabric(mysfomsworkspace)**.

İçinde **genel bakış** her bir Service Fabric için de dahil olmak üzere etkin çözüm için bir grafik biçiminde kutucuklar görürsünüz. Tıklayın **Service Fabric** Service Fabric analizi çözümü devam etmek için bir grafik.

![Service Fabric çözümü](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-summary.png)

Service Fabric analizi çözümü giriş sayfası aşağıdaki resimde gösterilmektedir. Bu giriş sayfası, kümenizin neler olduğunu bir anlık görüntü görünümü sağlar.

![Service Fabric çözümü](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-solution.png)

 Küme oluşturma sırasında tanılama etkinleştirilirse, olayları görebilirsiniz. 

* [Service Fabric küme olayları](service-fabric-diagnostics-event-generation-operational.md)
* [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programlama modeline olayları](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Ayrıntılı sistem olaylarını yanı sıra Service Fabric olayları hazır tarafından toplanabilir [tanılama uzantınızın yapılandırmayı güncelleştirerek](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Düğümlerde Eylemler dahil olmak üzere görünümü Service Fabric olayları

Grafik için Service Fabric analizi sayfasında tıklayarak **küme olayları**.  Toplanmış sistem olayları için günlüklere görünür. Bunlar, başvuru için gelen **WADServiceFabricSystemEventsTable** Azure depolama hesabı ve benzer şekilde sonraki gördüğünüz güvenilir hizmetler ve actors olayları ilgili bu tablolardaki'dır.
    
![Sorgu işlevsel kanal](media/service-fabric-tutorial-monitor-cluster/oms-service-fabric-events.png)

Sorgu, aradığınızı iyileştirmek için değiştirebileceğiniz Kusto sorgu dilini kullanır. Örneğin, kümedeki düğümler üzerinde gerçekleştirilen tüm eylemler bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay kimlikleri bulunan [işlevsel kanal etkinlik başvurusu](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Kusto sorgu güçlü bir dildir. Diğer bir kullanışlı sorgular şunlardır.

Oluşturma bir *ServiceFabricEvent* ServiceFabricEvent takma ad ile bir işlev olarak sorgu kaydederek kullanıcı tanımlı işlev olarak arama tablosu:

```kusto
let ServiceFabricEvent = datatable(EventId: int, EventName: string)
[
    ...
    18603, 'NodeUpOperational',
    18604, 'NodeDownOperational',
    ...
];
ServiceFabricEvent
```

Kaydedilen son bir saat içinde dönüş işletimsel olayları:
```kusto
ServiceFabricOperationalEvent
| where TimeGenerated > ago(1h)
| join kind=leftouter ServiceFabricEvent on EventId
| project EventId, EventName, TaskName, Computer, ApplicationName, EventMessage, TimeGenerated
| sort by TimeGenerated
```

İşletimsel olaylar EventID ile iade 18604 ve EventName == 'NodeDownOperational' ==:
```kusto
ServiceFabricOperationalEvent
| where EventId == 18604
| project EventId, EventName = 'NodeDownOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
```

İşletimsel olaylar EventID ile iade 18604 ve EventName == 'NodeUpOperational' ==:
```kusto
ServiceFabricOperationalEvent
| where EventId == 18603
| project EventId, EventName = 'NodeUpOperational', TaskName, Computer, EventMessage, TimeGenerated
| sort by TimeGenerated 
``` 
 
Durum raporlarıyla HealthState döndürür (hata) 3 == ve ek özellikler EventMessage alanından ayıklayın:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| extend HealthStateId = extract(@"HealthState=(\S+) ", 1, EventMessage, typeof(int))
| where TaskName == 'HM' and HealthStateId == 3
| extend SourceId = extract(@"SourceId=(\S+) ", 1, EventMessage, typeof(string)),
         Property = extract(@"Property=(\S+) ", 1, EventMessage, typeof(string)),
         HealthState = case(HealthStateId == 0, 'Invalid', HealthStateId == 1, 'Ok', HealthStateId == 2, 'Warning', HealthStateId == 3, 'Error', 'Unknown'),
         TTL = extract(@"TTL=(\S+) ", 1, EventMessage, typeof(string)),
         SequenceNumber = extract(@"SequenceNumber=(\S+) ", 1, EventMessage, typeof(string)),
         Description = extract(@"Description='([\S\s, ^']+)' ", 1, EventMessage, typeof(string)),
         RemoveWhenExpired = extract(@"RemoveWhenExpired=(\S+) ", 1, EventMessage, typeof(bool)),
         SourceUTCTimestamp = extract(@"SourceUTCTimestamp=(\S+)", 1, EventMessage, typeof(datetime)),
         ApplicationName = extract(@"ApplicationName=(\S+) ", 1, EventMessage, typeof(string)),
         ServiceManifest = extract(@"ServiceManifest=(\S+) ", 1, EventMessage, typeof(string)),
         InstanceId = extract(@"InstanceId=(\S+) ", 1, EventMessage, typeof(string)),
         ServicePackageActivationId = extract(@"ServicePackageActivationId=(\S+) ", 1, EventMessage, typeof(string)),
         NodeName = extract(@"NodeName=(\S+) ", 1, EventMessage, typeof(string)),
         Partition = extract(@"Partition=(\S+) ", 1, EventMessage, typeof(string)),
         StatelessInstance = extract(@"StatelessInstance=(\S+) ", 1, EventMessage, typeof(string)),
         StatefulReplica = extract(@"StatefulReplica=(\S+) ", 1, EventMessage, typeof(string))
```

Bir zaman grafiği EventID içeren olayların sayısını döndürmek! 17523 =:

```kusto
ServiceFabricOperationalEvent
| join kind=leftouter ServiceFabricEvent on EventId
| where EventId != 17523
| summarize Count = count() by Timestamp = bin(TimeGenerated, 1h), strcat(tostring(EventId), " - ", case(EventName != "", EventName, "Unknown"))
| render timechart 
```

Service Fabric düğüm ve belirli bir hizmeti ile işletimsel olaylar toplanan alın:

```kusto
ServiceFabricOperationalEvent
| where ApplicationName  != "" and ServiceName != ""
| summarize AggregatedValue = count() by ApplicationName, ServiceName, Computer 
```

Service Fabric EventID olayların sayısı işleme / EventName kaynaklar arası sorgu kullanarak:

```kusto
app('PlunkoServiceFabricCluster').traces
| where customDimensions.ProviderName == 'Microsoft-ServiceFabric'
| extend EventId = toint(customDimensions.EventId), TaskName = tostring(customDimensions.TaskName)
| where EventId != 17523
| join kind=leftouter ServiceFabricEvent on EventId
| extend EventName = case(EventName != '', EventName, 'Undocumented')
| summarize ["Event Count"]= count() by bin(timestamp, 30m), EventName = strcat(tostring(EventId), " - ", EventName)
| render timechart
```

### <a name="view-service-fabric-application-events"></a>Service Fabric uygulama olaylarını görüntüle

Reliable services ve reliable actors olayları görüntüleyebileceğiniz bir kümede dağıtılan uygulamalar.  Service Fabric analizi sayfasında için grafiğe tıklayın **uygulama olayları**.

Reliable services uygulamalarınızdan olayları görüntülemek için aşağıdaki sorguyu çalıştırın:
```kusto
ServiceFabricReliableServiceEvent
| sort by TimeGenerated desc
```

Hizmet runasync başlatıldığında ve hangi dağıtımları ve yükseltmeleri genellikle gerçekleşir tamamlandı farklı olaylara görebilirsiniz.

![Service Fabric güvenilir hizmetler çözümü](media/service-fabric-tutorial-monitor-cluster/oms-reliable-services-events-selection.png)

ServiceName güvenilir hizmeti için olayları bulabilirsiniz == "fabric: / izleme/WatchdogService":

```kusto
ServiceFabricReliableServiceEvent
| where ServiceName == "fabric:/Watchdog/WatchdogService"
| project TimeGenerated, EventMessage
| order by TimeGenerated desc  
```
 
Güvenilir aktör olayları, benzer şekilde görüntülenebilir:

```kusto
ServiceFabricReliableActorEvent
| sort by TimeGenerated desc
```
Reliable actors için Ayrıntılı olayları yapılandırmak için değiştirebileceğiniz `scheduledTransferKeywordFilter` yapılandırmasını küme şablondaki tanılama uzantısı. Değerleri içinde bunlar için ayrıntıları [reliable actors olayları başvurusu](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

## <a name="view-performance-counters-with-azure-monitor-logs"></a>Azure İzleyici günlükleri ile görünümü performans sayaçları
Performans sayaçlarını görüntülemek için Git [Azure portalında](https://portal.azure.com) ve Service Fabric analizi çözümü oluşturduğunuz kaynak grubunda. 

Kaynak seçin **ServiceFabric(mysfomsworkspace)**, ardından **Log Analytics çalışma alanı**, ardından **Gelişmiş ayarlar**.

Tıklayın **veri**, ardından **Windows performans sayaçları**. Varsayılan sayaçları etkinleştirmek için seçebileceğiniz bir listesi bulunur ve çok koleksiyon aralığını ayarlayabilirsiniz. Ayrıca ekleyebilirsiniz [ek performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) toplanacak. Bu doğru biçimde başvurulan [makale](/windows/desktop/PerfCtrs/specifying-a-counter-path). Tıklayın **Kaydet**, ardından **Tamam**.

Gelişmiş ayarlar dikey pencereyi kapatın ve seçin **çalışma özeti** altında **genel** başlığı. Her etkinleştirilirse çözümleri için Service Fabric için bir tane de dahil olmak üzere, grafik bir kutucuk var. Tıklayın **Service Fabric** Service Fabric analizi çözümü devam etmek için bir grafik.

İşlevsel kanal ve güvenilir hizmetler olayları için grafik kutucukları vardır. Grafik gösterimi için seçtiğiniz sayaç altında görünür içeriye veri **düğüm ölçümlerini**. 

Seçin **kapsayıcı ölçüm** grafiğine bakın. Ayrıca küme olayları ve düğümler, performans sayaç adı ve Kusto sorgu dilini kullanarak değerleri bir filtreye benzer şekilde, performans sayacı verileri sorgulayabilirsiniz.

## <a name="query-the-eventstore-service"></a>Sorgu EventStore hizmeti
[Eventstore'a hizmet](service-fabric-diagnostics-eventstore.md) zaman küme veya iş yüklerini belirli bir noktada durumunu öğrenmek için bir yol sağlar. Eventstore'a kümeden olayları tutar durum bilgisi olan bir Service Fabric hizmeti ' dir. Olayları aracılığıyla sunulur [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), REST ve API'ler. Tanılama Veri almanın Eventstore'a içinde kullanılabilir olayların tam listesi görmek için kümedeki herhangi bir varlık bkz doğrudan küme Eventstore'a sorgular [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

EventStore API'leri kullanarak programlı olarak sorgulanabilir [Service Fabric istemci Kitaplığı](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

İşte bir örnek istek tüm olaylar arasında 2018 kümesi için-04-03T18:00:00Z ve 2018-04-GetClusterEventListAsync işlev aracılığıyla bir 04T18:00:00Z.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

Eylül 2018'de küme sistem durumu ve tüm düğüm olayları sorgular ve bunları yazdırır başka bir örnek aşağıda verilmiştir.

```csharp
const int timeoutSecs = 60;
var clusterUrl = new Uri(@"http://localhost:19080"); // This example is for a Local cluster
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl);

var clusterHealth = sfhttpClient.Cluster.GetClusterHealthAsync().GetAwaiter().GetResult();
Console.WriteLine("Cluster Health: {0}", clusterHealth.AggregatedHealthState.Value.ToString());


Console.WriteLine("Querying for node events...");
var nodesEvents = sfhttpClient.EventsStore.GetNodesEventListAsync(
    "2018-09-01T00:00:00Z",
    "2018-09-30T23:59:59Z",
    timeoutSecs,
    "NodeDown,NodeUp")
    .GetAwaiter()
    .GetResult()
    .ToList();
Console.WriteLine("Result Count: {0}", nodesEvents.Count());

foreach (var nodeEvent in nodesEvents)
{
    Console.Write("Node event happened at {0}, Node name: {1} ", nodeEvent.TimeStamp, nodeEvent.NodeName);
    if (nodeEvent is NodeDownEvent)
    {
        var nodeDownEvent = nodeEvent as NodeDownEvent;
        Console.WriteLine("(Node is down, and it was last up at {0})", nodeDownEvent.LastNodeUpAt);
    }
    else if (nodeEvent is NodeUpEvent)
    {
        var nodeUpEvent = nodeEvent as NodeUpEvent;
        Console.WriteLine("(Node is up, and it was last down at {0})", nodeUpEvent.LastNodeDownAt);
    }
}
```


## <a name="monitor-cluster-health"></a>Küme durumunu izleme
Service Fabric tanıtır bir [sistem durumu modeli](service-fabric-health-introduction.md) hangi sistem bileşenleri ve watchdogs yerel koşulları rapor için sistem durumu varlıklarını ile bunların izleme. [Sistem durumu deposu](service-fabric-health-introduction.md#health-store) varlıkları iyi durumda olup olmadığını belirlemek için tüm sistem durumu verileri toplar.

Küme sistem bileşenleri tarafından gönderilen sistem durumu raporlarını otomatik olarak doldurulur. Adresinde daha fazla [sorun gidermek için sistem durumu raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Service Fabric gösterir sistem durumu sorgularının sayısı her desteklenen [varlık türleri](service-fabric-health-introduction.md#health-entities-and-hierarchy). Yöntemleri kullanarak, API aracılığıyla erişilebilen [FabricClient.HealthManager](/dotnet/api/system.fabric.fabricclient.healthmanager?view=azure-dotnet), PowerShell cmdlet'leri ve REST. Bu sorguları, varlığın tüm sistem bilgilerini döndürür: toplanan sistem durumu, varlık sistem durumu olayları, alt sistem durumlarını (uygun olduğunda), (varlık iyi durumda olmadığı durumlarda) sağlıksız değerlendirmeler ve alt öğeleri sağlık istatistikleri (ne zaman uygulanabilir).

### <a name="get-cluster-health"></a>Küme durumunu öğrenme
[Cmdlet Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) küme varlık durumunu döndürür ve uygulama ve düğümleri (alt kümesinin) sağlık durumlarını içerir.  İlk olarak kullanarak kümeye bağlanma [Connect-ServiceFabricCluster cmdlet'i](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps).

11 düğümleri, sistem uygulaması ve fabric kümesinin durumunu olduğu: / Voting açıklandığı şekilde yapılandırılmış.

Aşağıdaki örnek, varsayılan sistem durumu ilkeleri kullanarak küme durumu alır. 11 düğümleri sağlıklı olduğunu ancak toplanan küme sistem durumu hata çünkü fabric: / oylama uygulaması hatası. Sağlıksız değerlendirmeler Ayrıntıları toplanan sistem tetikleyen koşullara nasıl sağladığını unutmayın.

```powershell
Get-ServiceFabricClusterHealth

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            33% (1/3) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                          
NodeHealthStates        : 
                          NodeName              : _nt2vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_4
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_3
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_2
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_1
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt1vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt3vm_0
                          AggregatedHealthState : Ok
                          
                          NodeName              : _nt2vm_4
                          AggregatedHealthState : Ok
                          
ApplicationHealthStates : 
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok
                          
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
HealthStatistics        : 
                          Node                  : 11 Ok, 0 Warning, 0 Error
                          Replica               : 4 Ok, 0 Warning, 0 Error
                          Partition             : 2 Ok, 0 Warning, 0 Error
                          Service               : 2 Ok, 0 Warning, 0 Error
                          DeployedServicePackage : 3 Ok, 1 Warning, 1 Error
                          DeployedApplication   : 1 Ok, 1 Warning, 1 Error
                          Application           : 0 Ok, 0 Warning, 1 Error
```

Aşağıdaki örnek, bir özel uygulama İlkesi kullanarak küme durumunu alır. Bu, yalnızca uygulama ve düğümleri hata veya uyarı almak için sonuçları filtreler. Tüm sağlıklı oldukları gibi bu örnekte, düğüm, döndürülür. Fabric: / oylama uygulaması uygulamaları filtre dikkate alır. Fabric uyarılarını hata olarak değerlendirilecek özel ilke belirttiğinden: / oylama uygulaması, uygulama hata olduğu gibi değerlendirilir ve bu nedenle kümedir.

```powershell
$appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/Voting"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error" -ExcludeHealthStatistics

AggregatedHealthState   : Error
UnhealthyEvaluations    : 
                          100% (1/1) applications are unhealthy. The evaluation tolerates 0% unhealthy applications.
                          
                          Application 'fabric:/Voting' is in Error.
                          
                            100% (5/5) deployed applications are unhealthy. The evaluation tolerates 0% unhealthy deployed applications.
                          
                            Deployed application on node '_nt2vm_3' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '8723eb73-9b83-406b-9de3-172142ba15f3' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376195593305'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_2' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2466f2f9-d5fd-410c-a6a4-5b1e00630cca' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376486201388'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_4' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '5faa5201-eede-400a-865f-07f7f886aa32' is in Error.
                          
                                    'System.Hosting' reported Warning for property 'CodePackageActivation:Code:SetupEntryPoint:131959376207396204'. The evaluation treats 
                          Warning as Error.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt2vm_0' is in Error.
                          
                                100% (1/1) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '204f1783-f774-4f3a-b371-d9983afaf059' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959375885791093'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1
                          
                            Deployed application on node '_nt3vm_0' is in Error.
                          
                                50% (1/2) deployed service packages are unhealthy.
                          
                                Service package for manifest 'VotingWebPkg' and service package activation ID '2533ae95-2d2a-4f8b-beef-41e13e4c0081' is in Error.
                          
                                    'System.Hosting' reported Error for property 'CodePackageActivation:Code:SetupEntryPoint:131959376108346272'.
                                    There was an error during CodePackage activation.The service host terminated with exit code:1                         
                          
NodeHealthStates        : None
ApplicationHealthStates : 
                          ApplicationName       : fabric:/Voting
                          AggregatedHealthState : Error
                          
HealthEvents            : None
```

### <a name="get-node-health"></a>Düğüm durumu alma
[Cmdlet Get-ServiceFabricNodeHealth](/powershell/module/servicefabric/get-servicefabricnodehealth) düğüm varlık durumunu döndürür ve düğüm üzerinde bildirilen sistem durumu olaylarını içerir. İlk olarak kullanarak kümeye bağlanın [Connect-ServiceFabricCluster cmdlet'i](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps). Aşağıdaki örnek, varsayılan sistem durumu ilkeleri kullanarak belirli bir düğümün durumunu alır:

```powershell
Get-ServiceFabricNodeHealth _nt1vm_3
```

Aşağıdaki örnek, kümedeki tüm düğümlerin durumunu alır:
```powershell
Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize
```

### <a name="get-system-service-health"></a>Sistem hizmet durumunu Al 

Toplanmış sistem hizmetlerin durumunu alın:

```powershell
Get-ServiceFabricService -ApplicationName fabric:/System | Get-ServiceFabricServiceHealth | select ServiceName, AggregatedHealthState | ft -AutoSize
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Service Fabric olaylarını görüntüle
> * Küme olayları için sorgu EventStore API'leri
> * Altyapı/Topla performans sayacı izleme
> * Küme sistem durumu raporlarını görüntüleme

Ardından, bir kümenin ölçeğini öğrenmek için aşağıdaki öğreticiye geçin.
> [!div class="nextstepaction"]
> [Küme ölçeklendirme](service-fabric-tutorial-scale-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[template]: https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json