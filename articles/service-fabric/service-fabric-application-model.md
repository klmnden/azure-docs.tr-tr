---
title: Azure Service Fabric uygulama modelini | Microsoft Docs
description: Nasıl model ve uygulamaları ve Hizmetleri Service fabric'te açıklar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: 750970233cbcb14d901dbb5fa94f649f6ff8ae6c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60621447"
---
# <a name="model-an-application-in-service-fabric"></a>Bir Service Fabric uygulamasında model
Bu makalede, Azure Service Fabric uygulama modelini ve bir uygulama ve hizmet bildirim dosyaları aracılığıyla nasıl tanımlanacağı hakkında genel bir bakış sağlar.

## <a name="understand-the-application-model"></a>Uygulama modelini anlama
Bir uygulama, belirli bir işlev veya işlevler gerçekleştiren bağlı Hizmetleri koleksiyonudur. Bir hizmet tam ve tek başına bir işlevi gerçekleştirir ve başlatabilir ve diğer hizmetler bağımsız olarak çalışır.  Bir hizmet, kod, yapılandırma ve veri kümesinden oluşur. Her hizmet için ikili dosyaları yürütülebilir kod oluşur, çalışma zamanında yüklenmesi gereken hizmet ayarlarını yapılandırma oluşur ve veri hizmeti tarafından kullanılacak rastgele bir statik veri oluşur. Her bir bileşende bu hiyerarşik uygulama modeli, bağımsız olarak oluşturulabilir ve yükseltilebilir tutulan olabilir.

![Service Fabric uygulama modeli][appmodel-diagram]

Uygulama türü, bir uygulamanın bir kategorisi olan ve hizmet türlerinin bir paketi oluşur. Bir hizmetin bir kategori hizmet türüdür. Farklı ayarlar ve yapılandırmalar kategorisine sahip olabilir, ancak çekirdek işlevselliği aynı kalacak. Farklı hizmet yapılandırma çeşitlemeleri aynı hizmet türünün hizmet örnekleridir.  

Sınıflar (veya "türleri") uygulamaları ve Hizmetleri XML dosyaları (uygulama bildiriminin ve hizmet bildirimleri) aracılığıyla açıklanmıştır.  Bildirimleri uygulamaları ve hizmetleri tanımlamak ve şablonları, kümenin görüntü deposundan karşı uygulamalar oluşturulabilir.  Bildirimler, ayrıntılı olarak değinilmiştir [uygulama ve hizmet bildirimleri](service-fabric-application-and-service-manifests.md). ServiceManifest.xml ve ApplicationManifest.xml dosyası için şema tanımı ile Service Fabric SDK'sı yüklü olduğundan ve araçları *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. XML Şeması belgelenen [ServiceFabricServiceModel.xsd şema belgeleri](service-fabric-service-model-schema.md).

Farklı uygulama örneklerinin kodunu aynı Service Fabric düğüm tarafından barındırıldığında bile ayrı işlem olarak çalıştırın. Ayrıca, her uygulama örneği yaşam döngüsü (örneğin yükseltme) yönetilebilir bağımsız olarak. Uygulama türleri tespiti kod, yapılandırma ve veri paketleri sırayla oluşan hizmet türleri, aşağıdaki diyagramda gösterilmiştir. Diyagram basitleştirmek için yalnızca kod/config/veri paketleri için `ServiceType4` her hizmet türü bazı verilebilir veya bunlar tüm paket türlerinin gösterilir.

![Service Fabric uygulama türleri ve hizmet türleri][cluster-imagestore-apptypes]

Kümedeki etkin bir hizmet türünün bir veya daha fazla örneğini olabilir. Örneğin, durum bilgisi olan hizmet örneği veya çoğaltmalar, yüksek güvenilirlik durumu kümedeki farklı düğümlere bulunan yinelemeler arasında çoğaltılması yoluyla elde edin. Çoğaltma, temelde hizmeti bir kümedeki bir düğümün başarısız olsa bile kullanılabilir olması artıklık sağlar. A [bölümlenmiş hizmet](service-fabric-concepts-partitioning.md) daha fazla kümedeki düğümler arasında alt durumu (ve bu durum için erişim düzenlerini) böler.

Aşağıdaki diyagramda, uygulamalar ve hizmet örnekleri, bölümler ve çoğaltmalar arasındaki ilişkiyi gösterir.

![Bölümleri ve çoğaltmalarını hizmetinden][cluster-application-instances]

> [!TIP]
> Uygulama düzenini http:// kullanılabilir Service Fabric Explorer aracını kullanarak kümedeki görüntüleyebileceğiniz&lt;yourclusteraddress&gt;: 19080/Explorer. Daha fazla bilgi için [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).
> 
> 


## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
- Hizmeti hakkında bilgi [durumu](service-fabric-concepts-state.md), [bölümleme](service-fabric-concepts-partitioning.md), ve [kullanılabilirlik](service-fabric-availability-services.md).
- Nasıl uygulamaları ve Hizmetleri içinde tanımlandığı hakkında okuyun [uygulama ve hizmet bildirimleri](service-fabric-application-and-service-manifests.md).
- [Uygulama barındırma modelleri](service-fabric-hosting-model.md) dağıtılmış hizmet ve hizmet ana bilgisayarı işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


