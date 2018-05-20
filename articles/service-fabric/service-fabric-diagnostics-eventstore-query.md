---
title: Azure Service Fabric kümeleri EventStore API'lerini kullanarak küme olayları için sorgu | Microsoft Docs
description: Azure hizmet doku EventStore API'leri sorgulamak için platform olayları kullanmayı öğrenin
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 5c184841602f269555ce2196ef660faba14dbf8a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="query-eventstore-apis-for-cluster-events"></a>Küme olayları için sorgu EventStore API'leri

Bu makale, Service Fabric sürüm 6.2 yüklenebilir ve daha sonra - EventStore hizmeti hakkında daha fazla bilgi edinmek istiyorsanız bkz EventStore API'ları sorgu alınmaktadır [EventStore hizmetine genel bakış](service-fabric-diagnostics-eventstore.md). Şu anda EventStore service (Bu, kümenin tanılama veri bekletme ilkesi temel dayanır) son 7 gün için yalnızca verilere erişebilirsiniz.

>[!NOTE]
>Service Fabric sürüm 6.2 itibariyle. EventStore apı'leridir şu anda yalnızca Azure üzerinde çalışan Windows kümeleri için önizlemede. Bu işlevsellik, tek başına kümelerinin yanı sıra Linux taşıma üzerinde çalışıyoruz.

EventStore API'lerini doğrudan bir REST uç noktası aracılığıyla veya program aracılığıyla erişilebilir. Sorgu bağlı olarak doğru verileri toplamak için gereken birkaç parametre vardır. Bunlar genellikle şunlardır:
* `api-version`: kullanmakta olduğunuz EventStore API sürümü
* `StartTimeUtc`: bakarak ilginç dönem başlangıcı tanımlar
* `EndTimeUtc`: süre sonu

Bunlara ek olarak, kullanılabilir İsteğe bağlı parametreler de gibi:
* `timeout`: İstek işlemini gerçekleştirmek için varsayılan 60 saniye zaman aşımı geçersiz kıl
* `eventstypesfilter`: Bu, belirli olay türleri için filtre uygulama seçeneği sunar
* `ExcludeAnalysisEvents`: 'Analiz' olayları döndürmüyor. Varsayılan olarak, mümkün olduğunda EventStore sorguları "Çözümleme" olaylarla döndürür - daha kapsamlı sağlamak ve ek bağlam veya normal bir Service Fabric olay ötesinde bilgileri içeren daha zengin işletimsel kanal olayları analiz olaylardır.
* `SkipCorrelationLookup`: olası bağıntılı olaylar kümedeki arayın değil. Varsayılan olarak, bir küme üzerinde olayları ilişkilendirmenize ve mümkün olduğunda, olayları birbirine bağlamak EventStore deneyecek. 

Bir kümedeki her varlık olayları için sorgular olabilir. Türündeki tüm varlıkları için olaylar için sorgulayabilirsiniz. Örneğin, olayları belirli bir düğümün veya tüm düğümler için kümenizdeki sorgulayabilirsiniz. Geçerli olaylar için Sorgulayabileceğiniz varlıkları (nasıl sorgu yapılandırılmış ile) kümesidir:
* Küme: `/EventsStore/Cluster/Events`
* Düğümleri: `/EventsStore/Nodes/Events`
* Düğüm: `/EventsStore/Nodes/<NodeName>/$/Events`
* Uygulamalar: `/EventsStore/Applications/Events`
* Uygulama: `/EventsStore/Applications/<AppName>/$/Events`
* Hizmetler: `/EventsStore/Services/Events`
* Hizmet: `/EventsStore/Services/<ServiceName>/$/Events`
* Bölümler: `/EventsStore/Partitions/Events`
* Bölümü: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Çoğaltmaları: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Çoğaltma: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Bir uygulama veya hizmet adı başvururken sorgu dahil gerekmez "fabric: /" öneki. Uygulama veya hizmet adlarına sahip, bir "/" bunlara ek olarak, kendisine geçiş bir "~" sorgu çalışma tutmak için. Örneğin, uygulamanızın olarak görünüyorsa "fabric: / App1/FrontendApp", uygulama belirli sorgularınızı olarak yapılandırılacağını `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>İçin sorgulama için ek olarak, sistem durumu raporlarının Hizmetleri için bugün karşılık gelen uygulamanın altında görünmesini `DeployedServiceHealthReportCreated` olayları için doğru uygulama varlığı. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>REST API uç noktaları aracılığıyla EventStore sorgulama

Bir REST uç noktası aracılığıyla doğrudan EventStore yaparak Sorgulayabileceğiniz `GET` ister: `<your cluster address>/EventsStore/<entity>/Events/`.

Örneğin, tüm küme olayları arasında sorgulamak için `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, isteğiniz aşağıdaki gibidir:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.2-preview&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Bu ya da bir hata olayı yok kullanılabilir veya sorgu başarılı olduysa, json'da döndürülen olay göreceksiniz döndürebilirsiniz:

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

Arasında burada görebiliriz `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, önce stood zaman bu küme, ilk Yükseltme başarıyla tamamlandı. gelen `"CurrentClusterVersion": "0.0.0.0:"` için `"TargetClusterVersion": "6.2:1.0"`, `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>EventStore program aracılığıyla sorgu

EventStore sorgu da programlı olarak aracılığıyla [Service Fabric istemci Kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Service Fabric ayarlanan istemciniz olduktan sonra böyle EventStore erişerek olaylar için sorgulama yapabilirsiniz: ` sfhttpClient.EventStore.<request>`

İşte bir örnek isteği tüm olaylar arasındaki kümesi için `2018-04-03T18:00:00Z` ve `2018-04-04T18:00:00Z`, aracılığıyla `GetClusterEventListAsync` işlevi.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

## <a name="sample-scenarios-and-queries"></a>Örnek senaryolar ve sorguları

İşte birkaç örnekler, küme durumunu anlamak için olay Store REST API'lerini nasıl çağırabilirsiniz üzerinde.

*Küme yükseltme:*

Kümeniz başarıyla veya geçen hafta yükseltilecek deneyen son zamanı görmek için EventStore "ClusterUpgradeComplete" olayları için sorgulayarak kümeniz için son tamamlanan yükseltmeler için API'leri sorgulayabilirsiniz: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeComplete`

*Küme yükseltme sorunlar:*

Benzer şekilde, yeni bir küme yükseltme ile ilgili sorunlar varsa, küme varlığın tüm olaylar için sorgulayabilir. Yükseltmeleri ve kendisi için yükseltme aracılığıyla başarıyla alındı her UD başlatma dahil olmak üzere çeşitli olayları görürsünüz. Ayrıca, hangi noktada olaylarını başlatıldı ve sistem durumu olayları karşılık gelen geri alma görürsünüz. Bunun için kullanacağınız sorgu şöyledir: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Düğüm durumu değişiklikler:*

Düğüm durumunuzu en son değişiklikleri görmek için aşağıdaki sorguyu - zaman düğümleri yukarı veya aşağı oluştu veya etkinleştirilmiş veya (ya da chaos hizmeti, platform veya kullanıcı girişi) devre dışı - birkaç gün kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Uygulama olayları:*

Ayrıca, yeni uygulama dağıtımları ve yükseltmeleri de izleyebilirsiniz. Tüm uygulama görmek için aşağıdaki sorguyu kullanın ilgili olayları kümenizdeki: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

*Bir uygulama için geçmiş sistem durumu:*

Uygulama yaşam döngüsü olayları yalnızca görme ek olarak, ayrıca belirli bir uygulama sistem durumu hakkında geçmiş verileri görmek isteyebilirsiniz. Bu, veri toplamak istediğiniz uygulama adı belirterek yapabilirsiniz. Tüm uygulama sistem durumu olayları almak için bu sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.2-preview&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ProcessApplicationReport`. Süresi sona ermiş olabilir sistem durumu olayları dahil etmek istediğiniz (kendi zaman canlı (TTL) geçirilen gitti), ekleme `,ExpiredDeployedApplicationEvent` iki tür olay filtrelemek için sorgu sonuna.

*Geçmiş durumu "Uygulamam" tüm hizmetler için:*

Şu anda Hizmetleri için sistem durumu raporu olayları gösterme `DeployedServiceHealthReportCreated` olayları karşılık gelen uygulama varlığı altında. Nasıl hizmetlerinizi "App1 için" bulunurken görmek için aşağıdaki sorguyu kullanın: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServiceHealthReportCreated`

*Bölümü yeniden yapılandırma:*

Tümünü görmek için kümedeki oldu bölüm hareketleri sorgulamak için `ReconfigurationCompleted` olay. Bu yardımcı olabilir hangi iş yüklerini hangi düğümde zaman tanılama izin ver sorunları kümenizdeki belirli zamanlarda bitmiştir şekil. Aşağıda, yapan bir örnek sorgu verilmiştir: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.2-preview&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigurationCompleted`

*Chaos hizmeti:*

Ne zaman hizmeti başlatılır ya da başka bir deyişle durdurulur karmaşası küme düzeyde gösterilen için bir olay yok. Chaos hizmetin son kullanımını görmek için aşağıdaki sorguyu kullanın: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

