---
title: Azure Service Fabric kapsayıcıları günlüklerini görüntülemek | Microsoft Docs
description: Service Fabric Explorer kullanarak çalışan bir Service Fabric kapsayıcı hizmetler için kapsayıcı günlüklerini görüntülemeyi açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: 48ee54460454368deef44c8f84624e32856efafa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Service Fabric kapsayıcı Hizmeti günlüklerini görüntüle
Azure Service Fabric bir kapsayıcı orchestrator ve her ikisi de destekler [Linux ve Windows kapsayıcıları](service-fabric-containers-overview.md).  Bu makalede, tanılama ve sorun giderme için çalışan bir kapsayıcı hizmeti kapsayıcı günlüklerini görünümü açıklar.

## <a name="access-container-logs"></a>Erişim kapsayıcı günlükleri
Kapsayıcı günlükleri kullanarak erişilebilir [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Kümenin yönetim uç noktasından Service Fabric Explorer giderek bir web tarayıcısında açın [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Kapsayıcı günlükleri kapsayıcı hizmeti örneğini çalıştıran küme düğümünde bulunur. Örnek olarak, web ön uç kapsayıcısının günlükler elde [Linux oylama örnek uygulama](service-fabric-quickstart-containers-linux.md). Ağaç görünümünde genişletin **küme**>**uygulamaları**>**VotingType**>**fabric: / oylama / azurevotefront**.  Sonra (d1aa737e-f22a-e347-be16-eec90be24bc1, bu örnekte) bölümünü genişletin ve kapsayıcı küme düğümü üzerinde çalışıp çalışmadığını *_lnxvm_0*.

Kod paketi bulunamadı ağaç görünümünde *_lnxvm_0* genişleterek düğümü **düğümleri**>**_lnxvm_0**>**fabric: / oylama**  > **azurevotfrontPkg**>**kod paketleri**>**kod**.  Ardından **kapsayıcı günlükleri** kapsayıcı günlükleri görüntülemek için seçeneği.

![Service Fabric platformu][Image1]


## <a name="next-steps"></a>Sonraki adımlar
- Aracılığıyla iş [Linux kapsayıcı uygulama öğretici oluşturma](service-fabric-tutorial-create-container-images.md).
- Daha fazla bilgi edinmek [Service Fabric ve kapsayıcıları](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
