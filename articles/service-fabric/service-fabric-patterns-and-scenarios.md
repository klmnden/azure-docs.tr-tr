---
title: "Azure Service Fabric modelleri ve senaryoları | Microsoft Docs"
description: "En iyi yöntemleri öğrenin ve, tasarlamak için yeniden kullanılabilir desenleri kanıtlanmış geliştirmek ve, mikro Service Fabric üzerinde çalışır."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: d5aa75ff-98b9-4573-a2e5-7f5ab288157a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/04/2017
ms.author: ryanwi
ms.openlocfilehash: 7808493ca984277a939f04098799dbbd8287cc0c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="service-fabric-patterns-and-scenarios"></a>Service Fabric modelleri ve senaryoları
Azure Service Fabric kullanarak büyük ölçekli mikro derlemeye arıyorsanız, kimin tasarlanmış ve bu platform (PaaS) hizmet olarak yerleşik uzmanlarından öğrenin. Uygun mimarisi ile başlayın ve uygulamanız için kaynakları en iyi duruma getirme hakkında bilgi edinin. [Service Fabric desenleri ve uygulamalar](https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344) indirmelere gerçek müşteriler tarafından Service Fabric senaryoları ve uygulama alanları hakkında sık sorulan sorular yanıtlanmaktadır.
 
En iyi yöntemleri ve kendini kanıtlamış, yeniden kullanılabilen desenleri kullanarak mikro hizmetlerinizi Service Fabric’te tasarlama, geliştirme ve çalıştırma hakkında bilgi edinin. Service Fabric özetini almak ve derin küme en iyi duruma getirme ve eski uygulamalar geçirme güvenlik, IOT ve oyun motorları barındırma ölçekte kapsayan konularını içine yakından inceleyin. Çeşitli iş yükleri için kesintisiz teslim bakın ve hatta Linux desteği ve kapsayıcıları ayrıntıları alın. 

## <a name="introduction"></a>Tanıtım
En iyi uygulamaları inceleyin ve platform (PaaS) hizmet olarak altyapı üzerinden bir hizmet (Iaas) olarak seçme hakkında bilgi edinin. Ayrıntılar aşağıdaki kanıtlanmış uygulama tasarım kurallara göre alın.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=N2KwbbSGD_6405167344">
<img src="./media/service-fabric-patterns-and-scenarios/intro.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mudwqISGD_6005167344">Service Fabric giriş</a></td></tr>
</table>

## <a name="cluster-planning-and-management"></a>Küme planlama ve Yönetimi
Kapasite planlaması, küme en iyi duruma getirme ve bu göz Azure Service Fabric kümesi güvenliği hakkında bilgi edinin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=cyDYZcSGD_2805167344">
<img src="./media/service-fabric-patterns-and-scenarios/cluster.png" WIDTH="360" HEIGHT="244">
</a></td><td> <a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=E5B3nJSGD_805167344">Küme planlama ve Yönetimi</a></td></tr>
</table>

## <a name="hyper-scale-web"></a>Hiper ölçekli web
Kullanılabilirlik ve güvenilirlik, Hiper ölçekli ve durum yönetimi dahil olmak üzere hiper ölçekli web geçici kavramlarını gözden geçirin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=NgldAdSGD_405167344">
<img src="./media/service-fabric-patterns-and-scenarios/hyperscaleweb.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=CPMLBLSGD_7705167344">Hiper ölçekli web</a></td></tr>
</table>

## <a name="iot"></a>IoT
Azure IOT ardışık düzen, çok kiracılı ve IOT ölçekte da dahil olmak üzere Azure Service Fabric bağlamında nesnelerin interneti (IOT) keşfedin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=naFUVeSGD_1505167344">
<img src="./media/service-fabric-patterns-and-scenarios/iot.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">IoT</a></td></tr>
</table>

## <a name="gaming"></a>Oyun
Bırakma tabanlı oyunlar, etkileşimli oyunlar ve varolan oyun altyapıları barındırma arayın.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=6wECzeSGD_3805167344">
<img src="./media/service-fabric-patterns-and-scenarios/gaming.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=kfqFWMSGD_6205167344">Oyun</a></td></tr>
</table>

## <a name="continuous-delivery"></a>Sürekli teslim
Visual Studio Team Services, iş akışı derleme/paket/yayımlama, çoklu ortam kurulumu ve hizmet paketi/paylaşımı sürekli tümleştirme/kesintisiz teslim de dahil olmak üzere kavramları keşfedin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=78h5ofSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/cd.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=VlENvOSGD_105167344">Kesintisiz teslim</a></td></tr>
</table>

## <a name="migration"></a>Geçiş
Eski uygulamalar geçişini yanı sıra bir bulut hizmeti geçiş hakkında bilgi edinin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=hd1cMgSGD_5605167344">
<img src="./media/service-fabric-patterns-and-scenarios/migration.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=GQAq4QSGD_8305167344">Geçiş</a></td></tr>
</table>

## <a name="containers-and-linux-support"></a>Kapsayıcılar ve Linux desteği
Sorusunun yanıtını alma "neden kapsayıcıları?" Önizleme için Windows kapsayıcıları, Linux destekler ve Linux kapsayıcıları düzenleme hakkında bilgi edinin. Ayrıca, .NET Core uygulamaları için Linux geçirme hakkında bilgi edinin.

<table><tr><th>Video</th><th>PowerPoint deste</th></tr>
<tr><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=V1ERJhSGD_305167344">
<img src="./media/service-fabric-patterns-and-scenarios/containers.png" WIDTH="360" HEIGHT="244">
</a></td><td><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/service-fabric-patterns-and-practices-16925?l=mlYsZRSGD_2105167344">Kapsayıcılar ve Linux desteği</a></td></tr>
</table>

## <a name="next-steps"></a>Sonraki adımlar
Artık öğrendiğinize göre Service Fabric modelleri ve senaryoları, nasıl hakkında daha fazla bilgi edinin hakkındaki [oluşturun ve kümelerini Yönet](service-fabric-deploy-anywhere.md), [bulut Hizmetleri uygulamaları için Service Fabric geçirme](service-fabric-cloud-services-migration-worker-role-stateless-service.md), [sürekli teslimat ayarlamak](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), ve [kapsayıcıları dağıtın](service-fabric-containers-overview.md).
