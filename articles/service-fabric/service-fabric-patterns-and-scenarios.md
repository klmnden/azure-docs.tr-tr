---
title: Azure Service Fabric modeller ve senaryolar | Microsoft Docs
description: En iyi adımları öğrenin ve kendini kanıtlamış, yeniden kullanılabilir düzenleri tasarlamak için geliştirme ve Service fabric'te mikro hizmetlerin çalışır.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/19/2018
ms.author: ryanwi
ms.openlocfilehash: 3bd77891cc7508eeb1fee2152d37478c654a7e37
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44294715"
---
# <a name="service-fabric-patterns-and-scenarios"></a>Service Fabric modeller ve senaryolar
Azure Service Fabric kullanarak büyük ölçekli bir mikro hizmetler oluşturmayı arıyorsanız tasarlanmıştır ve bu platform (PaaS) hizmet olarak oluşturulan oluşturan uzmanlardan bilgi edinin. Uygun bir mimariyle başlayın ve ardından kaynakları uygulamanız için en iyi duruma getirme öğrenin. [Service Fabric desenleri ve uygulamaları](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) kurs gerçek müşteriler tarafından Service Fabric senaryolarını ve uygulama alanları hakkında sık sorulan soruların yanıtları.
 
En iyi yöntemleri ve kendini kanıtlamış, yeniden kullanılabilen desenleri kullanarak mikro hizmetlerinizi Service Fabric’te tasarlama, geliştirme ve çalıştırma hakkında bilgi edinin. Service Fabric genel bir bakış edinin ve ayrıntılı küme en iyi duruma getirme ve eski uygulamaları geçirmek güvenliği, barındırma ve oyun altyapıları ölçekte IOT kapsayan konuları derinlerine. Çeşitli iş yükleri için sürekli teslim bakmak ve hatta Linux desteği ve kapsayıcılar detayları alın. 

## <a name="introduction"></a>Giriş
En iyi uygulamaları keşfedin ve platform (PaaS) hizmet olarak altyapı üzerinden hizmet (Iaas) olarak seçme hakkında bilgi edinin. Aşağıdaki kendini kanıtlamış uygulama tasarım ilkeleri hakkında ayrıntılı bilgi edinin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Service fabric'e giriş</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>Küme planlama ve yönetim
Kapasite planlaması, küme en iyi duruma getirme ve bu görünüm, Azure Service Fabric küme güvenliği hakkında bilgi edinin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Küme planlama ve yönetim</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>Hiper ölçekli web
Hiper ölçekli web, kullanılabilirlik ve güvenilirlik, Hiper ölçekli ve durum yönetimi gibi kavramlar gözden geçirin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hiper ölçekli web</a></td></tr>
</table>

## <a name="iot"></a>IoT
Nesnelerin interneti (IOT), Azure Service Fabric, Azure IOT işlem hattı, çok kiracılı ve IOT ölçekli olarak gibi bağlamında keşfedin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>Oyun
Sırayla oynadıkları oyunlar, etkileşimli oyunlar ve var olan oyun altyapıları barındırma arayın.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Oyun</a></td></tr>
</table>

## <a name="continuous-delivery"></a>Sürekli teslim
Azure işlem hatları, iş akışı derleme/paket/yayımlama, birden çok ortam kurulumu ve hizmet paketi/paylaşımı ile sürekli tümleştirme/sürekli teslim dahil olmak üzere, kavramlarını keşfedin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Sürekli teslim</a></td></tr>
</table>

## <a name="migration"></a>Geçiş
Bir bulut hizmeti, geçiş eski uygulamaların yanı sıra geçiş hakkında bilgi edinin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Geçiş</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>Kapsayıcılar ve Linux desteği
Soru, yanıt alın "neden kapsayıcıları?" Windows kapsayıcıları, Linux destekler ve Linux kapsayıcıları düzenleme önizlemesi hakkında bilgi edinin. Ayrıca, Linux için .NET Core uygulamaları geçirme hakkında bilgi edinin.

<table><tr><th>Video</th><th>Bir PowerPoint Destesi</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Kapsayıcılar ve Linux desteği</a></td></tr>
</table>

## <a name="next-steps"></a>Sonraki adımlar
Artık öğrendiğinize göre Service Fabric desenleri ve senaryoları, kullanma hakkında daha fazla okuma hakkında [küme oluşturma ve yönetme](service-fabric-deploy-anywhere.md), [Cloud Services uygulamaları Service Fabric'e geçme](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [ayarlama sürekli teslim](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), ve [kapsayıcıları dağıtma](service-fabric-containers-overview.md).
