---
title: İzleme ve tanılama Azure Service Fabric Mesh uygulamalarında | Microsoft Docs
description: Service Fabric Mesh azure'da uygulama tanılama ve izleme hakkında bilgi edinin.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2018
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 79e5622dd73e53854204675b435e99d187a3ab26
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076634"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama
Azure Service Fabric Mesh geliştiricilerin mikro hizmet uygulamaları sanal makineler, depolama, yönetme veya ağ dağıtmanıza olanak sağlayan tam olarak yönetilen bir hizmettir. Service Fabric Mesh için izleme ve Tanılama, Tanılama verileri üç temel tür olarak kategorilere:

- Uygulama günlükleri - bunlar uygulamanızı nasıl izlenen üzerinde tabanlı uygulamalarınızı kapsayıcılı günlüklerinden olarak tanımlanır (örneğin docker günlükleri)
- Platform olaylar - kafes platformu, kapsayıcı işlemine, ilgili olaylar şu anda sonlandırma kapsayıcı etkinleştirme ve devre dışı bırakma dahil.
- Kapsayıcı ölçümleri - kapsayıcılarınızı (docker istatistikleri) için kaynak kullanımı ve performans ölçümleri

Bu makalede, en son Önizleme sürümü için izleme ve tanılama seçenekleri ele alınmaktadır.

## <a name="application-logs"></a>Uygulama günlükleri

Kapsayıcı başına temelinde dağıtılan, kapsayıcılardan docker günlüklerinizi görüntüleyebilirsiniz. Service Fabric Mesh uygulama modeli, her kapsayıcı, uygulamanızdaki bir kod paketidir. Kod paketi ile ilişkili günlükleri görmek için aşağıdaki komutu kullanın:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Çoğaltma adı almak için "az kafes servicereplica" komutunu kullanabilirsiniz. Çoğaltma adları 0.* numaraları artan

Oylama uygulamasını VotingWeb.Code kapsayıcısından günlüklerinden görmek için göründüğüne aşağıda verilmiştir:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric Mesh hakkında daha fazla bilgi için genel bakış okuyun:
- [Service Fabric Mesh genel bakış](service-fabric-mesh-overview.md)
