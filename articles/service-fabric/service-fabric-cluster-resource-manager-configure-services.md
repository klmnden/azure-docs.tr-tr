---
title: Azure Service Fabric'te ölçümleri ve yerleştirme ayarlarını belirtme | Microsoft Docs
description: Ölçümler, yerleştirme kısıtlamaları ve diğer yerleştirme ilkeleri belirterek bir Service Fabric hizmeti açıklayan öğrenin.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 21fcac62c9335652d0c682a6ac889be82e649464
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60844151"
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Service Fabric Hizmetleri için küme kaynak yöneticisi ayarlarını yapılandırma
Service Fabric Küme Kaynak Yöneticisi, her kişi adlı hizmetin yöneten kurallar üzerinde ayrıntılı denetim sağlar. Adlandırılmış her hizmetin nasıl kümede ayrılmalıdır kuralları belirtebilirsiniz. Adlandırılmış her hizmet ne kadar önemli bu hizmete oldukları dahil olmak üzere, rapor istediği ölçüm kümesini de tanımlayabilirsiniz. Hizmetleri yapılandırma üç farklı görevlere böler:

1. Yerleştirme kısıtlamalarını yapılandırma
2. Ölçümleri yapılandırma
3. Gelişmiş yerleştirme ilkeleri ve diğer kurallar (daha az ortak) yapılandırma

## <a name="placement-constraints"></a>Yerleştirme kısıtlamaları
Yerleştirme kısıtlamaları, hizmet kümedeki düğümlere gerçekten çalıştırabileceğiniz denetlemek için kullanılır. Genellikle belirli bir hizmet örneği veya belirli bir düğüm türü üzerinde çalıştırmak için kısıtlı belirli bir türden tüm hizmetleri olarak adlandırılır. Yerleştirme kısıtlamaları genişletilebilir. Her düğüm türü başına özellikler kümesini tanımlar ve sonra bunlar için kısıtlamalarla Hizmetleri oluştururken seçin. Çalışırken, bir hizmetin yerleştirme kısıtlamaları değiştirebilirsiniz. Bu, değişiklik kümesi ya da hizmet gereksinimlerine yanıt vermesini sağlar. Belirli bir düğümün özelliklerini de kümeye dinamik olarak güncelleştirilebilir. Yerleştirme kısıtlamaları ve nasıl yapılandırılacakları hakkında daha fazla bilgi bulunabilir [bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Ölçümler
Belirli bir adlandırılmış hizmetinin gereken kaynak kümesini ölçümleridir. Bir hizmetin ölçüm yapılandırma varsayılan olarak her bir durum bilgisi olan çoğaltma ya da durum bilgisi olmayan hizmet örneğini kullanan bu kaynağın ne kadar içerir. Ölçümleri nasıl önemli bu ölçümü Dengeleme bu hizmete olduğu durumda ödün gerekli olduğu gösteren ağırlık de içerir.

## <a name="advanced-placement-rules"></a>Gelişmiş yerleştirme kuralları
Diğer türde daha az yaygın senaryoları kullanışlı olan yerleştirme kuralları vardır. Bazı örnekler şunlardır:
- Coğrafi olarak dağıtılmış kümeleriyle yardım eden kısıtlamaları
- Belirli uygulama mimarileri

Diğer yerleştirme kuralları Bağıntılar veya ilkeleri aracılığıyla yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım ve kapasite kümedeki Service Fabric Küme Kaynak Yöneticisi'ni nasıl yönettiğini ölçümleridir. Ölçümler ve nasıl yapılandırılacakları hakkında daha fazla bilgi edinmek için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
- Benzeşim hizmetlerinizi için yapılandırabileceğiniz bir moddur. Yaygın değildir, ancak ihtiyacınız varsa, hakkında bilgi edinebilirsiniz [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Hizmetinizde ek senaryoları işlemek için yapılandırılabilir birçok farklı yerleştirme kuralları vardır. Bu farklı yerleştirme ilkeleri hakkında bilgi edinin [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- En baştan başlatmak ve [için Service Fabric Küme Kaynak Yöneticisi giriş yapın](service-fabric-cluster-resource-manager-introduction.md)
- Küme Kaynak Yöneticisi yönetir ve kümedeki yük dengeleyen hakkında bilgi almak için makalesine göz atın [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- Küme Kaynak Yöneticisi kümesi tanımlamak için birçok seçenek vardır. Bunlar hakkında daha fazla bilgi için bu makalede atın [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
