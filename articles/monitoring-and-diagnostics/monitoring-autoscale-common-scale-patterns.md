---
title: Ortak otomatik ölçeklendirme desenleri genel bakış
description: Azure kaynağınıza otomatik olarak için ortak desenler öğrenin.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: c7084a10aceafcdd1039893b810fcbd8b74b874b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967413"
---
# <a name="overview-of-common-autoscale-patterns"></a>Ortak otomatik ölçeklendirme desenleri genel bakış
Bu makalede, Azure'da kaynağınızı ölçeklendirmek için ortak desenler bazılarını açıklar.

Azure İzleyici otomatik ölçeklendirme için yalnızca geçerlidir [sanal makine ölçek kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/), ve [APIManagementHizmetleri](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

# <a name="lets-get-started"></a>Sağlar kullanmaya başlayın

Bu makalede, otomatik ölçeklendirme ile ilgili bilgi sahibi olduğunuz varsayılır. Yapabilecekleriniz [kaynağınızı ölçeklendirmek için buradan başlayın][1]. Ortak ölçek desenleri bazıları aşağıda verilmiştir.

## <a name="scale-based-on-cpu"></a>CPU üzerinde göre ölçeklendirin

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve

- İstediğiniz ölçek dışı/ölçek dayalı olarak CPU.
- Ayrıca, en az bir örnek sayısı olduğundan emin olmak istersiniz.
- Ayrıca, örnek için ölçeklendirilebilir sayısı en fazla bir sınır koymak sağlamak istiyorsunuz.

![CPU üzerinde göre ölçeklendirin][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Haftanın günlerini vs sonralı farklı ölçeklendirme

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve

- Varsayılan olarak (hafta içi) 3 örnek istiyor
- Hafta sonu trafiği beklemeyen ve bu nedenle sonralı 1 örnek aşağı ölçeklendirin istiyorsunuz.

![Haftanın günlerini vs sonralı farklı ölçeklendirme][3]

## <a name="scale-differently-during-holidays"></a>Farklı tatilleri ölçeklendirin

Bir web uygulaması (/ VMSS/bulut hizmeti rolü) sahip ve

- Varsayılan olarak CPU kullanımına göre ölçeğini artırmanızı/azaltmanızı istiyorsunuz
- Ancak, mağazalarımızdaki ürünler (ya da işletmeniz için önemli olan, belirli gün) boyunca Varsayılanları geçersiz kılmak ve elinizin daha fazla kapasiteye sahip olmak istiyorsanız.

![Ölçek farklı üzerinde tatiller][4]

## <a name="scale-based-on-custom-metric"></a>Özel bir ölçüme göre ölçeklendirin

Bir web ön uç ve arka ucunuzla iletişim kuran bir API katmanı vardır.

- Ön uç özel olaylar göre API katmanı ölçeklendirmek istediğiniz (örnek: kullanıma alma işleminizi, öğelerin alışveriş sepetine göre ölçeklendirmek istediğiniz)

![Özel bir ölçüme göre ölçeklendirin][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png
