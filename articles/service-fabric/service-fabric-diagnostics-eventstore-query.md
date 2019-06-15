---
title: Sorgu EventStore API'leri kullanarak Azure Service Fabric kümelerinde küme olayları için | Microsoft Docs
description: Azure Service Fabric EventStore API'leri sorgulamak için platform olaylarını kullanmayı öğrenin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: facbcd6def7451ca83bdf00fe9b7c7cac2c74945
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60392883"
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Küme olayları için sorgu EventStore API'leri

Bu makalede sorgu EventStore API'leri, Service Fabric sürüm 6.2 kullanılabilir ve daha sonra - Eventstore'a hizmeti hakkında daha fazla bilgi edinmek istiyorsanız, bkz. nasıl etkinleştireceğinizi de açıklar [Eventstore'a hizmetine genel bakış](service-fabric-diagnostics-eventstore.md). Şu anda Eventstore'a hizmeti (Bu, kümenin tanılama veri bekletme ilkesi temel alır) son 7 gün için yalnızca verilere erişebilir.

>[!NOTE]
>Service Fabric sürümü 6.4 Azure üzerinde çalışan Windows kümeleri için'ten itibaren genel kullanım EventStore API'leri var.

EventStore API'leri, REST uç noktası aracılığıyla doğrudan veya program aracılığıyla erişilebilir. Sorguya bağlı olarak doğru verileri toplamak için gereken birkaç parametre yok. Genellikle bu parametreleri içerir:
* `api-version`: kullanmakta olduğunuz EventStore API'leri sürümü
* `StartTimeUtc`: göz atan ilgilenen dönemi başlangıcını tanımlar
* `EndTimeUtc`: süre sonu

Bu parametrelerin yanı sıra de olduğu gibi kullanılabilen isteğe bağlı parametreler vardır:
* `timeout`: İstek işlemini gerçekleştirmek için varsayılan 60 ikinci zaman aşımını geçersiz kıl
* `eventstypesfilter`: Bu belirli olay türleri için filtre seçeneği sunar
* `ExcludeAnalysisEvents`: 'Çözümleme' olayları döndürmüyor. Varsayılan olarak, mümkün olduğunda, "analiz" olaylarla Eventstore'a sorguları döndürür. Çözümleme olayları ek bağlam ve normal bir Service Fabric olay ötesinde bilgiler içerir ve daha fazla ayrıntı sağlayan daha zengin işlevsel kanal olaylardır.
* `SkipCorrelationLookup`: kümedeki olası bağıntılı olaylar için arama. Varsayılan olarak, bir küme genelinde olayları ilişkilendirmenize ve mümkün olduğunda olaylarınızı birbirine bağlamak Eventstore'a dener. 

Bir kümedeki her varlık, olaylar için sorgular olabilir. Türün tüm varlıklar için olaylar için sorgulayabilirsiniz. Örneğin, belirli bir düğümün veya tüm düğümler için olayları kümenizde sorgulayabilirsiniz. Geçerli varlıklar için olaylar için sorgulayabilirsiniz (nasıl sorgu yapılandırılmış ile) kümesidir:
* Küme: `/EventsStore/Cluster/Events`
* Düğüm: `/EventsStore/Nodes/Events`
* Düğüm: `/EventsStore/Nodes/<NodeName>/$/Events`
* Uygulamalar: `/EventsStore/Applications/Events`
* Uygulama: `/EventsStore/Applications/<AppName>/$/Events`
* Hizmetler: `/EventsStore/Services/Events`
* Hizmet: `/EventsStore/Services/<ServiceName>/$/Events`
* Bölümler: `/EventsStore/Partitions/Events`
* Bölüm: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Yineleme: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Çoğaltma: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Bir uygulama veya hizmet adı başvururken sorgu içerecek şekilde gerekmiyor "fabric: /" öneki. Ayrıca, uygulama veya hizmet adları varsa, bunlara "/", kendisine geçiş bir "~" sorgu çalışma korumak için. Örneğin, uygulamanızın olarak gösterilip gösterilmeyeceğini "fabric: / App1/FrontendApp", uygulama belirli sorgularınızı olarak yapılandırılacağını `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>İçin sorgulama için Ayrıca, hizmetleri için sistem durumu raporlarının bugün karşılık gelen uygulama altında görünmesi `DeployedServiceHealthReportCreated` doğru uygulama varlık için olayları. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Sorgu EventStore REST API uç noktaları aracılığıyla

Hale getirerek doğrudan bir REST uç noktası aracılığıyla Eventstore'a sorgulayabilirsiniz `GET` ister: `<your cluster address>/EventsStore/<entity>/Events/`.

Örneğin, tüm küme olayları sorgulamak için `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, isteğiniz şöyle görünmelidir:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.4&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Bu olayı yok veya döndürülen json'da olayların listesini döndürebilir:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Burada arasında görebiliriz `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, öncelikle, komutla zaman bu kümedeki ilk yükseltmesi başarıyla tamamlandı. gelen `"CurrentClusterVersion": "0.0.0.0:"` için `"TargetClusterVersion": "6.2:1.0"`, `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>Eventstore'a programlı bir şekilde sorgulama

Ayrıca, Eventstore'a sorgulayabilirsiniz programlama yoluyla [Service Fabric istemci Kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Service Fabric ayarlanan istemci aldıktan sonra bu gibi Eventstore'a erişerek olayları için sorgulama yapabilirsiniz: `sfhttpClient.EventStore.<request>`

İşte bir örnek istek için tüm olayları arasındaki küme `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, aracılığıyla `GetClusterEventListAsync` işlevi.

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

## <a name="sample-scenarios-and-queries"></a>Örnek senaryolar ve sorguları

Kümenizi durumunu anlamak için olay Store REST API'lerini nasıl çağırabilirsiniz üzerinde bazı örnekler aşağıdadır.

*Küme yükseltme:*

Kümeniz başarıyla veya geçen hafta yükseltilecek çalıştı son kez görmek için Eventstore'a "ClusterUpgradeCompleted" olayları için sorgulayarak kümenize, kısa süre önce tamamlanan yükseltme için API'leri sorgulayabilirsiniz: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeCompleted`

*Küme yükseltme sorunlar:*

Benzer şekilde, yeni bir küme yükseltmesi ile ilgili sorunlar varsa, tüm olaylar kümeyi varlık için sorgulayabilir. Yükseltmeler, yükseltme ile başarıyla alındı her UD ve başlatma gibi çeşitli etkinlikler görürsünüz. Geri alma başlatıldı ve sistem durumu olaylarını karşılık gelen olaylar için hangi noktada de görürsünüz. Bunun için kullanacağınız sorgu aşağıda verilmiştir: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Düğüm durumu değişiklikleri:*

Geçtiğimiz, düğüm durumu değişikliği görmek üzere - ne zaman düğümleri yukarı veya aşağı gittiği etkin veya (chaos hizmeti, platform tarafından veya kullanıcı girişinden) devre dışı - birkaç gün aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Uygulama olayları:*

Ayrıca, yükseltmeleri ve son uygulama dağıtımını izleyebilirsiniz. Kümenizdeki tüm uygulama olaylarını görmek için aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Bir uygulama için sistem durumu geçmiş:*

Yalnızca uygulama yaşam döngüsü olaylarını görmenin yanı sıra, ayrıca geçmiş verileri belirli bir uygulamanın durumunu görmek istediğiniz. Verileri toplamak istediğiniz uygulama adını belirterek bunu yapabilirsiniz. Tüm uygulama sistem durumu olaylarını almak için bu sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.4&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ApplicationNewHealthReport`. Süresi dolmuş olan sistem durumu olayları dahil etmek istiyorsanız (Canlı (TTL) zamanlarının geçirilen gitti), ekleme `,ApplicationHealthReportExpired` iki tür olay filtrelemek için sorguyu, sonuna.

*"MyApp" tüm hizmetler için sistem durumu geçmiş:*

Şu anda Hizmetleri için sistem durumu raporu olayları olarak görünmesi `DeployedServicePackageNewHealthReport` olayları karşılık gelen uygulama varlığı altında. Nasıl hizmetlerinizi "App1 için" yapılması görmek için aşağıdaki sorguyu kullanın: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServicePackageNewHealthReport`

*Bölümü yeniden yapılandırma işlemi:*

Tümünü görmek için kümenizin gerçekleşen bölüm hareketleri sorgulamak için `PartitionReconfigured` olay. Bu, yardımcı şekil hangi iş yüklerini hangi düğümde belirli zamanlarda ne zaman tanılama izin ver sorunları kümenizde bitmiştir. Burada, yapan bir örnek sorgu verilmiştir: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.4&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigured`

*Kaos hizmeti:*

Zaman hizmeti başlatılır ya da diğer bir deyişle durdurulur Chaos kümesi düzeyinde kullanıma sunulan bir etkinliği yoktur. Son Chaos hizmet kullanımınızı görmek için aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.4&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

