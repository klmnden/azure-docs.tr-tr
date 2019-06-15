---
title: Azure Traffic Manager | Microsoft Docs
description: Bu makalede Azure Traffic Manager'a genel bir bakış sağlanmıştır. Uygulamanızın kullanıcı trafiğine yük dengeleme uygulamak için doğru seçenek olup olmadığına karar verin.
services: traffic-manager
author: asudbring
manager: twooley
ms.service: traffic-manager
customer intent: As an IT admin, I want to learn about Traffic Manager and what I can use it for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/05/2019
ms.author: allensu
ms.openlocfilehash: 95ef493ed44da8fd8372c11f15c3ab712961783e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071017"
---
# <a name="what-is-traffic-manager"></a>Traffic Manager nedir?
Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir.

Traffic Manager, trafik yönlendirme yöntemine ve uç noktaların sistem durumuna bağlı olarak istemci isteklerini en uygun hizmet uç noktasına yönlendirmek için DNS hizmetini kullanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](traffic-manager-monitoring.md) sunar. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. Aktarım Katmanı Güvenliği (TLS) protokolü sonlandırma ("SSL yük boşaltma") veya HTTP/HTTPS isteği başına uygulama katmanı işleme özellikleri istiyorsanız [Application Gateway](../application-gateway/application-gateway-introduction.md)'i inceleyin. Bölgesel Yük Dengeleme için arıyorsanız, gözden [Load Balancer](../load-balancer/load-balancer-overview.md). Uçtan uca senaryolarınızda bu çözümleri bir arada da kullanabilirsiniz.

Traffic Manager, özellikleri sunar:

## <a name="increase-application-availability"></a>Uygulama kullanılabilirliğini artırma

Traffic Manager, uç noktalarınızı izleyerek ve uç nokta kullanım dışı kaldığında otomatik yük devretme sağlayarak kritik uygulamalarını için yüksek kullanılabilirlik sağlar.
    
## <a name="improve-application-performance"></a>Uygulama performansını geliştirme

Azure, dünyanın dört bir yanında bulunan veri merkezlerinde bulut hizmeti veya web sitesi çalıştırmanıza olanak tanır. Traffic Manager, trafiği istemci için en düşük ağ gecikme süresine sahip trafiğe yönlendirerek uygulama yanıt süresini kısaltır.

## <a name="perform-service-maintenance-without-downtime"></a>Kesinti süresi olmadan hizmet bakımı gerçekleştirme

Kesinti süresi yaşamadan uygulamalarınızda planlı bakım çalışmaları yürütebilirsiniz. Bakım işlemi devam ederken, traffic Manager alternatif uç noktaları için trafiği yönlendirebilir.

## <a name="combine-hybrid-applications"></a>Hibrit uygulamaları birleştirme

Traffic Manager harici, Azure dışı uç noktalar için sunduğu destek sayesinde "[buluta veri bloğu aktarma](https://azure.microsoft.com/overview/what-is-cloud-bursting/)," "buluta geçiş" ve "buluta devretme" senaryoları gibi hibrit bulut ve şirket içi dağıtımlarda kullanılabilir.

## <a name="distribute-traffic-for-complex-deployments"></a>Karmaşık dağıtımlar için trafiği dağıtma

Kullanarak [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md), birden çok trafik yönlendirme yöntemleri, karmaşık ve esnek ölçeklendirme ihtiyaçlarını daha büyük, daha karmaşık dağıtımlar için kurallar oluşturmak için birleştirilebilir.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma bilgileri için bkz. [Traffic Manager Fiyatlandırması](https://azure.microsoft.com/pricing/details/traffic-manager/).


## <a name="next-steps"></a>Sonraki adımlar

- [Traffic Manager profili oluşturmayı](traffic-manager-create-profile.md) öğrenin.
- [Traffic Manager'ın nasıl çalıştığını](traffic-manager-how-it-works.md) öğrenin.
- Traffic Manager hakkında [sık sorulan soruları](traffic-manager-FAQs.md) görüntüleyin.




