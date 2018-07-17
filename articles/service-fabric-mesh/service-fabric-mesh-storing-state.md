---
title: Azure Service Fabric Mesh depolama seçeneklerini belirtin. | Microsoft Docs
description: Güvenilir bir şekilde durumunu Service Fabric Mesh uygulamalarda çalışan Azure Service Fabric Mesh depolama hakkında bilgi edinin.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 6aa268cf56bfb8be9c27a9e0d9e5c9f4464b0c9d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076661"
---
# <a name="state-management-with-service-fabric"></a>Service Fabric ile durum yönetimi
Service Fabric, birçok farklı seçenek için durumu depolama alanını destekler. Kavramsal genel bakış durum yönetimi düzenleri ve Service Fabric için bkz: [Service Fabric Kavramları: durum](/azure/service-fabric/service-fabric-concepts-state). Hizmetlerinizi içinde veya Service Fabric Mesh dışında çalışan tüm aynı kavramlar geçerlidir. 

## <a name="state-storage-options-in-azure-service-fabric-mesh"></a>Azure Service Fabric Mesh durumu depolama seçenekleri
Service Fabric Mesh ile kolayca yeni bir uygulama dağıtın ve Azure'da barındırılan mevcut bir veri deposuna bağlanın. Herhangi bir uzak veritabanı kullanmanın yanı sıra, hizmet yerel veya uzak depolama olup istediği bağlı olarak verileri depolamak için birkaç seçenek vardır. 

* Çoğaltılan veriler yerel olarak depolanır
  * Güvenilir koleksiyonlar (Önizleme aşamasında sağlanmaz)
    * Kuyruklar ve anahtar-değer çifti gibi veri yapıları, hizmeti uygulayan bir kitaplığı
    * Bu kolay partition Service Fabric Mesh içinde akıllı yönlendirme ile birlikte yönlendirme sağlamanın yanı sıra verilerle etkileşimde bulunmak için kolay ve hızlı bir yol sağlar
  * Service Fabric birim sürücüsü (Önizleme aşamasında sağlanmaz)
    * Bir kapsayıcıya yerel bir birimi bağlamak için bir docker birim sürücüsü
    * Bu, dosya depolama'yı destekleyen tüm API'leri verileri yerel olarak depolamak ultimate esneklik sağlar.

* Uzaktaki Depolama Birimi
  * Azure dosyaları birim sürücüsü
    * Bir kapsayıcı için bir Azure dosya paylaşımını bağlayabilmeniz için docker birim sürücüsü
    * Dosya depolama'yı destekleyen bir API aracılığıyla da daha ucuz tam esnek ve güvenilir bir veri seçeneği ancak daha az etkili sunar.
    * [Nasıl yapılır Kılavuzu: Azure dosya birimi ile uygulama dağıtma](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)
    
## <a name="next-steps"></a>Sonraki adımlar

Uygulama modeli hakkında daha fazla bilgi için bkz: [Service Fabric kaynakları](service-fabric-mesh-service-fabric-resources.md)
