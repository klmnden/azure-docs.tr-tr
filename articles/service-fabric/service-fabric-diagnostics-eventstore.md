---
title: Azure Service Fabric olay Store | Microsoft Docs
description: Azure Service Fabric'in Eventstore'a hakkında bilgi edinin
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
ms.date: 1/17/2019
ms.author: srrengar
ms.openlocfilehash: 36d01a9e6e55ae54377ba3f983f779dbc692c49a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392929"
---
# <a name="eventstore-service-overview"></a>Eventstore'a hizmetine genel bakış

>[!NOTE]
>Service Fabric sürümü 6.4 itibaren. EventStore API'leri yalnızca yalnızca Azure üzerinde çalışan Windows kümeleri için kullanılabilir. Bu işlev, tek başına kümeler yanı sıra Linux taşıma üzerinde çalışıyoruz.

## <a name="overview"></a>Genel Bakış

6.2 sürümünde sunulan Eventstore'a Service fabric'te izleme bir seçenek hizmetidir. Eventstore'a zaman küme veya iş yüklerini belirli bir noktada durumunu anlamak için bir yol sağlar. Eventstore'a kümeden olayları tutar durum bilgisi olan bir Service Fabric hizmeti ' dir. Olayı Service Fabric Explorer, REST ve API'ler aracılığıyla sunulur. Eventstore'a doğrudan herhangi bir varlık, kümenizdeki ilgili tanılama verilerini almak için küme sorgular ve yardımcı olmak için kullanılmalıdır:

* Geliştirme veya test sorunları tanılayın ve burada izleme bir işlem hattı kullanıyor olabilir
* Yönetim eylemleri kümenizde sürüp işlenen doğru onaylayın
* "Service Fabric belirli bir varlık ile nasıl etkileşim kurmanın anlık" Al

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

Eventstore'a içinde kullanılabilir olayların tam listesi için bkz [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Service Fabric 6.2 sürümü itibarıyla. EventStore API'leri şu anda yalnızca Azure üzerinde çalışan Windows kümeleri için Önizleme. Bu işlev, tek başına kümeler yanı sıra Linux taşıma üzerinde çalışıyoruz.

Eventstore'a hizmet, her varlık ve kümenizde varlık türü için kullanılabilir olan olaylar için sorgulanabilir. Başka bir deyişle, olayları aşağıdaki düzeylerde sorgulayabilirsiniz:
* Küme: kümenin kendisine (örneğin, Küme yükseltme) belirli olayları
* Düğüm: tüm düğüm düzeyinde olaylar
* Düğüm: bir düğüm tarafından tanımlanan, belirli olayları `nodeName`
* Uygulamalar: tüm uygulama düzeyinde olaylar
* Uygulama: tarafından tanımlanan bir uygulama için belirli olayları `applicationId`
* Hizmetleri: tüm kümelerinizi Hizmetleri'nde olaylardan
* Hizmeti: belirli bir hizmet tarafından tanımlanan olayları `serviceId`
* Bölümler: tüm bölümleri olaylardan
* Bölüm: olayları ile tanımlanan belirli bir bölüm `partitionId`
* Bölüm çoğaltmalarını: tüm çoğaltmaların olaylar / örnekleri tarafından tanımlanan belirli bir bölüm içinde `partitionId`
* Bölümü çoğaltması: belirli bir çoğaltma olaylardan / örneği ile tanımlanan `replicaId` ve `partitionId`

Bilgi edinmek için API hakkında daha fazla kullanıma [Eventstore'a API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore).

Eventstore'a hizmeti, ayrıca, kümenizdeki olayları ilişkilendirmenize özelliğine sahiptir. Aynı anda birbirine etkilemiş olabilecek farklı varlıklardan yazılmış olayları bakarak, Eventstore'a hizmet bu olaylar, etkinlikler, kümenizdeki nedenleri tanımlamaya yardımcı olmak için bağlantı kuramıyor. Uygulamalarınızdan biri uyarılmış hiçbir değişiklik yapmadan sistem durumu ortaya çıkarsa gibi Eventstore'a ayrıca diğer olayları göz platform tarafından sunulan ve bununla ilişkilendirin mı bir `Error` veya `Warning` olay. Bu, daha hızlı hata algılama ile yardımcı olur ve kök neden analizi.

## <a name="enable-eventstore-on-your-cluster"></a>Kümenizde Eventstore'a etkinleştir

### <a name="local-cluster"></a>Yerel küme

İçinde [kümenizdeki fabricSettings.json](service-fabric-cluster-fabric-settings.md)EventStoreService bir eklenti özelliği olarak ekleyin ve bir küme yükseltmesi gerçekleştirin.

```json
    "addOnFeatures": [
        "EventStoreService"
    ],
```

### <a name="azure-cluster"></a>Azure kümesine

Kümenizin Azure Resource Manager şablonunda gerçekleştirerek Eventstore'a hizmette kapatabilirsiniz bir [küme yapılandırma yükseltmesi](service-fabric-cluster-config-upgrade-azure.md) ve aşağıdaki kodu ekleyerek, Eventstore'a çoğaltmalarının koymak PlacementConstraints kullanabilirsiniz belirli bir NodeType örn sistem hizmetler için ayrılmış bir NodeType hizmeti. `upgradeDescription` Bölümü yeniden başlatma düğümlerinde tetiklemek için yapılandırma yükseltme yapılandırır. Başka bir güncelleştirmede bölüm kaldırabilirsiniz.

```json
    "fabricSettings": [
          …
          …
          …,
         {
            "name": "EventStoreService",
            "parameters": [
              {
                "name": "TargetReplicaSetSize",
                "value": "3"
              },
              {
                "name": "MinReplicaSetSize",
                "value": "1"
              },
              {
                "name": "PlacementConstraints",
                "value": "(NodeType==<node_type_name_here>)"
              }
            ]
          }
        ],
        "upgradeDescription": {
          "forceRestart": true,
          "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807",
          "healthCheckWaitDuration": "00:01:00",
          "healthCheckStableDuration": "00:01:00",
          "healthCheckRetryTimeout": "00:5:00",
          "upgradeTimeout": "1:00:00",
          "upgradeDomainTimeout": "00:10:00",
          "healthPolicy": {
            "maxPercentUnhealthyNodes": 100,
            "maxPercentUnhealthyApplications": 100
          },
          "deltaHealthPolicy": {
            "maxPercentDeltaUnhealthyNodes": 0,
            "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0,
            "maxPercentDeltaUnhealthyApplications": 0
          }
        }
```


## <a name="next-steps"></a>Sonraki adımlar
* -Eventstore'a API'si ile çalışmaya başlama [EventStore API'leri kullanarak Azure Service Fabric kümeleri](service-fabric-diagnostics-eventstore-query.md)
* -Eventstore'a tarafından sunulan olayların listesi hakkında daha fazla bilgi [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md)
* İzleme ve tanılama Service fabric'te - genel bakış [Service Fabric için izleme ve tanılama](service-fabric-diagnostics-overview.md)
* API çağrıları - tam listesini görüntüleyin [Eventstore'a REST API Başvurusu](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-eventsstore)
* Kümenizi izleme hakkında daha fazla bilgi- [platform ve küme izleme](service-fabric-diagnostics-event-generation-infra.md).
