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
ms.date: 6/6/2019
ms.author: srrengar
ms.openlocfilehash: f1e7428bc0665cdd3f981bb9c2e7b1f564598f40
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074248"
---
# <a name="eventstore-overview"></a>Eventstore'a genel bakış

>[!NOTE]
>Service Fabric sürümü 6.4 itibaren. EventStore API'leri yalnızca yalnızca Azure üzerinde çalışan Windows kümeleri için kullanılabilir. Bu işlev, tek başına kümeler yanı sıra Linux taşıma üzerinde çalışıyoruz.

## <a name="overview"></a>Genel Bakış

6\.2 sürümünde sunulan Eventstore'a Service fabric'te izleme bir seçenek hizmetidir. Eventstore'a zaman küme veya iş yüklerini belirli bir noktada durumunu anlamak için bir yol sağlar.
Eventstore'a kümeden olayları tutar durum bilgisi olan bir Service Fabric hizmeti ' dir. Olayı Service Fabric Explorer, REST ve API'ler aracılığıyla sunulur. Eventstore'a doğrudan herhangi bir varlık, kümenizdeki ilgili tanılama verilerini almak için küme sorgular ve yardımcı olmak için kullanılmalıdır:

* Geliştirme veya test sorunları tanılayın ve burada izleme bir işlem hattı kullanıyor olabilir
* Yönetim eylemleri kümenizde sürüp işlenen doğru onaylayın
* "Service Fabric belirli bir varlık ile nasıl etkileşim kurmanın anlık" Al

![EventStore](media/service-fabric-diagnostics-eventstore/eventstore.png)

Eventstore'a içinde kullanılabilir olayların tam listesi için bkz [Service Fabric olayları](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>Service Fabric sürümü 6.4 itibaren. UX ve EventStore API'leri Azure Windows kümeleri için genel kullanıma sunulmuştur. Bu işlev, tek başına kümeler yanı sıra Linux taşıma üzerinde çalışıyoruz.

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

### <a name="azure-cluster-version-65"></a>Azure kümesine sürüm 6.5 +
Azure kümenize 6.5 veya üzeri sürüme yükseltilmesi, Eventstore'a kümenizde otomatik olarak etkinleştirilecektir. Geri çevirmek için küme şablonunuza aşağıdakiyle güncelleştirin gerekir:

* Bir API sürümünü kullanın `2019-03-01` ya da daha yeni 
* Kümenizi, özellikleri bölümünde aşağıdaki kodu ekleyin
  ```json  
    "fabricSettings": [
      …
    ],
    "eventStoreEnabled": false
  ```

### <a name="azure-cluster-version-64"></a>Azure kümesine sürüm 6.4

6\.4 sürümü kullanıyorsanız, Azure Resource Manager şablonunuzu Eventstore'a Hizmeti'ni başlatmak için düzenleyebilirsiniz. Gerçekleştirerek bu yapılır bir [küme yapılandırma yükseltmesi](service-fabric-cluster-config-upgrade-azure.md) ve aşağıdaki kodu ekleyerek, çoğaltmaları Eventstore'a hizmetinin belirli bir NodeType örn sistem hizmetler için ayrılmış bir NodeType yerleştirmek için PlacementConstraints kullanabilirsiniz . `upgradeDescription` Bölümü yeniden başlatma düğümlerinde tetiklemek için yapılandırma yükseltme yapılandırır. Başka bir güncelleştirmede bölüm kaldırabilirsiniz.

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
