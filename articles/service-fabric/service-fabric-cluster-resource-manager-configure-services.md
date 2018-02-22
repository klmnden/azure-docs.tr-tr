---
title: "İçinde Azure mikro ölçümleri ve yerleştirme ayarlarını belirtme | Microsoft Docs"
description: "Service Fabric hizmeti, ölçümler, yerleştirme kısıtlamaları ve diğer yerleşim ilkeleri belirterek açıklayan."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0ae4e874d0fd0922295a4ec7ad719a0a1fb108c8
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Service Fabric Hizmetleri için küme kaynak yönetici ayarlarını yapılandırma
Service Fabric kümesi Kaynak Yöneticisi adlı hizmetin her kişi belirleyen kurallar üzerinde ayrıntılı denetim sağlar. Her adlandırılmış hizmet kümesinde nasıl ayrılmalıdır kuralları belirtebilirsiniz. Adlandırılmış her hizmet bu hizmete oldukları ne kadar önemli dahil olmak üzere raporlanacak istediği ölçümleri kümesi de tanımlayabilirsiniz. Hizmetleri yapılandırma üç farklı görevlere parçalara ayırır:

1. Yerleştirme kısıtlamalarını yapılandırma
2. Ölçümleri yapılandırma
3. Gelişmiş yerleşim ilkeleri ve diğer kurallar (daha az ortak) yapılandırma

## <a name="placement-constraints"></a>Yerleştirme kısıtlamaları
Yerleşim kısıtlaması, bir hizmeti kümedeki düğümler gerçekte çalıştırabileceğiniz denetlemek için kullanılır. Tipik olarak belirli bir hizmet örneği veya kısıtlı belirli türde bir düğüm üzerinde çalıştırmak için belirli bir türde tüm hizmetleri adlı. Kısıtlamalarından genişletilebilir. Tüm düğüm türü başına özellikler kümesini tanımlar ve sonra bunlar için kısıtlamalarıyla Hizmetleri oluştururken seçin. Çalışırken bir hizmetin kısıtlamalarından de değiştirebilirsiniz. Bu, küme veya hizmet gereksinimlerine değişiklikleri yanıt vermesini sağlar. Belirli bir düğümün özelliklerini de kümeye dinamik olarak güncelleştirilebilir. Yerleştirme kısıtlamaları ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi bulunabilir [bu makalede](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Ölçümler
Ölçümleri belirli bir adlandırılmış hizmeti gereken kaynakları kümesidir. Bir hizmet ölçüm yapılandırmasının varsayılan olarak her bir durum bilgisi olan çoğaltma veya hizmet durum bilgisiz örneğiniz tüketir bu kaynağın ne kadarının içerir. Ölçümleri gerekli bileşim durumda ne kadar önemli Bu ölçüm Dengeleme bu hizmete olan, gösteren ağırlık de içerir.

## <a name="advanced-placement-rules"></a>Gelişmiş yerleştirme kuralları
Diğer kullanışlı daha az yaygın senaryolar yerleştirme kuralı türü vardır. Bazı örnekler şunlardır:
- Coğrafi olarak dağıtılmış kümeleriyle yardım eden kısıtlamaları
- Belirli uygulama mimarisi

Diğer yerleştirme kuralları bağıntıları veya ilkeleri aracılığıyla yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar
- Service Fabric kümesi Kaynak Yöneticisi, kullanım ve kapasite kümedeki nasıl yönettiğini ölçümleridir. Ölçümleri ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi için kullanıma [bu makalede](service-fabric-cluster-resource-manager-metrics.md)
- Benzeşim hizmetlerinizi için yapılandırabileceğiniz bir moddur. Ortak değildir, ancak ihtiyacınız varsa onu hakkında bilgi edinebilirsiniz [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Hizmetinizde ilave senaryolar işlemek için yapılandırılmış birçok farklı yerleştirme kuralları vardır. Bu farklı yerleşim ilkeleri hakkında bilgi almak [burada](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- En baştan başlatın ve [bir giriş için Service Fabric kümesi Resource Manager Al](service-fabric-cluster-resource-manager-introduction.md)
- Küme Kaynak Yöneticisi'ni yönetir ve yük devretme kümesinde dengeleyen hakkında bilgi almak için makalesine kontrol [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md)
- Küme Kaynak Yöneticisi'ni küme açıklamak için birçok seçeneğiniz vardır. Bunları hakkında daha fazla bilgi için bu makalede kontrol [açıklayan bir Service Fabric kümesi](service-fabric-cluster-resource-manager-cluster-description.md)
