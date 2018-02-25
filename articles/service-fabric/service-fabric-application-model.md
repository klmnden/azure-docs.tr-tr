---
title: Azure Service Fabric uygulama modeli | Microsoft Docs
description: "Nasıl model ve uygulamaları ve Hizmetleri Service Fabric açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: ryanwi
ms.openlocfilehash: 506daa2dc0612fc49a67c5faf3c7ab51ac90126f
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="model-an-application-in-service-fabric"></a>Service Fabric uygulamada modeli
Bu makalede Azure Service Fabric uygulama modeli ve bir uygulama ve hizmet bildirim dosyaları aracılığıyla nasıl tanımlanacağı genel bakış sağlar.

## <a name="understand-the-application-model"></a>Uygulama modeli anlama
Bir uygulama, belirli bir işlevi veya işlevleri gerçekleştirmek bağlı hizmetler topluluğudur. Bir hizmet tam ve tek başına bir işlevi gerçekleştirir ve başlayın ve diğer hizmetler bağımsız olarak çalışır.  Kod, yapılandırma ve verileri bir servis oluşur. Her hizmet için kodu yürütülebilir ikili dosyaları oluşur, çalışma zamanında yüklenen hizmet ayarlarını yapılandırma oluşur ve veri hizmeti tarafından kullanılacak rasgele statik verileri oluşur. Bu hiyerarşik uygulama modeli her bileşenin bağımsız olarak sürümlü hem de yükseltilmiş olabilir.

![Service Fabric uygulama modeli][appmodel-diagram]

Uygulama türü, bir uygulamanın bir kategori değil ve hizmet türlerinin bir dizi oluşur. Bir hizmetin kategori bir hizmet türüdür. Kategori farklı ayarları ve yapılandırmaları olabilir, ancak temel işlevleri aynı kalır. Farklı hizmet yapılandırma farklılıkları aynı hizmet türünün hizmet örnekleridir.  

Sınıflar (veya "türleri") uygulamaları ve Hizmetleri XML dosyalarını (uygulama bildirimlerini ve hizmet bildirimlerini) açıklanan.  Bildirimleri göre kümenin görüntü deposundan uygulamaları oluşturulabilir şablonlar ve uygulamalar ve hizmetler açıklanmıştır.  Bildirimleri ayrıntılı olarak ele alınmıştır [uygulama ve hizmet bildirimlerini](service-fabric-application-and-service-manifests.md). ServiceManifest.xml ve ApplicationManifest.xml dosyası için şema tanımı Service Fabric SDK ile birlikte yüklenir ve araçların *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. XML Şeması belgelenen [ServiceFabricServiceModel.xsd şema belgelerine](service-fabric-service-model-schema.md).

Farklı uygulama örnekleri için kod bile aynı Service Fabric düğümüne barındırıldığında ayrı işlemler olarak çalıştırın. Ayrıca, her uygulama örneği yaşam döngüsü (örneğin yükseltilmiş) yönetilebilir bağımsız olarak. Aşağıdaki diyagramda uygulama türleri sırayla kod, yapılandırma ve veri paketleri oluşan hizmet türlerini tespiti gösterir. Diyagram basitleştirmek için yalnızca kod/config/verileri paketler için `ServiceType4` her hizmet türü bazı içerir veya bu tüm paket türlerinin de gösterilir.

![Service Fabric uygulama türleri ve hizmet türleri][cluster-imagestore-apptypes]

Kümedeki etkin bir hizmet türünün bir veya daha fazla örneğini olabilir. Örneğin, durum bilgisi olan hizmet örneği veya çoğaltmalar, yüksek güvenilirlik kümedeki farklı düğümlerde bulunan çoğaltmalar arasında durumu yineleyerek elde edin. Çoğaltma temelde hizmetinin bir kümedeki bir düğümün başarısız olsa bile kullanılabilir olması artıklık sağlar. A [bölümlenmiş hizmet](service-fabric-concepts-partitioning.md) daha fazla kümedeki düğümler arasında alt durumu (ve bu durum için erişim düzenlerini) böler.

Aşağıdaki diyagram, uygulamaları ve hizmet örnekleri, bölümler ve çoğaltmalar arasındaki ilişkiyi gösterir.

![Bölümler ve çoğaltmalar bir hizmet kapsamındaki][cluster-application-instances]

> [!TIP]
> Http:// kullanılabilir Service Fabric Explorer aracını kullanarak bir küme uygulamalarının düzeni görüntüleyebileceği&lt;yourclusteraddress&gt;: 19080/Explorer. Daha fazla bilgi için bkz: [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).
> 
> 


## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [uygulama ölçeklenebilirlik](service-fabric-concepts-scalability.md).
- Hizmeti hakkında bilgi edinin [durumu](service-fabric-concepts-state.md), [bölümleme](service-fabric-concepts-partitioning.md), ve [kullanılabilirlik](service-fabric-availability-services.md).
- Nasıl uygulamaları ve Hizmetleri içinde tanımlanan hakkında okuyun [uygulama ve hizmet bildirimlerini](service-fabric-application-and-service-manifests.md).
- [Uygulama barındırma modellerini](service-fabric-hosting-model.md) bir dağıtılan hizmet ve hizmet ana bilgisayar işlemi çoğaltmaları (veya örnekleri) arasındaki ilişkiyi açıklar.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


