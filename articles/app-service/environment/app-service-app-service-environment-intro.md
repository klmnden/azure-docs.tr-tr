---
title: Giriş App Service ortamı v1 - Azure
description: Tüm uygulamalarınızı çalıştırmak için güvenli, Vnet'e katılmış, özel ölçek birimleri sağlayan App Service ortamı v1 özelliği hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 78e6d4f5-da46-4eb5-a632-b5fdc17d2394
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 2bb1a9c3922f435b6be78614aacff6e85bf475ff
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130747"
---
# <a name="introduction-to-app-service-environment-v1"></a>Giriş App Service ortamı v1

> [!NOTE]
> Bu makale, App Service ortamı v1 hakkında yöneliktir.  App Service ortamı, kullanımı daha kolay ve daha güçlü bir altyapı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm başlama hakkında daha fazla bilgi edinmek için [App Service ortamı giriş](intro.md).

## <a name="overview"></a>Genel Bakış

App Service ortamı olan bir [Premium] [ PremiumTier] hizmet planı seçeneği [Azure App Service](../overview.md) güvenli olarak çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar Azure App Service uygulamalarını yüksek ölçekte Web Apps, Mobile Apps ve API Apps dahil olmak üzere.  

App Service ortamları gerektiren uygulama iş yükleri için idealdir:

* Çok yüksek ölçek
* Yalıtım ve güvenli ağ erişimi

Müşteriler, tek bir Azure bölgesi içinde yanı sıra birden çok Azure bölgeleri arasında birden fazla App Service ortamları oluşturabilirsiniz.  Bu App Service ortamları yüksek RPS iş yüklerini desteklemek üzere durumu olmayan uygulama katmanlarını yatay yönde ölçeklendirmek için ideal hale getirir.

App Service ortamları yalnızca tek bir müşterinin uygulamalarını çalıştırmak üzere yalıtılmıştır ve her zaman bir sanal ağa dağıtılır.  Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim imajlarını ve uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz.

App Service ortamları etkinleştirme büyük ölçekli ve güvenli bir genel bakış için ağ erişimi, bkz: [AzureCon ayrıntılı Bakışında] [ AzureConDeepDive] üzerinde App Service ortamları!

Kullanarak yatay ölçeklendirme hakkında derinlemesine için birden fazla App Service ortamları Kurulumu bkz bir [coğrafi olarak dağıtılan uygulama Ayak izi][GeodistributedAppFootprint].

AzureCon ayrıntılı Bakışında gösterilen güvenlik mimarisinin nasıl yapılandırıldığını görmek için uygulamaya makaleye bakın bir [katmanlı güvenlik mimarisi](app-service-app-service-environment-layered-security.md) App Service ortamları ile.

App Service ortamlarında çalışan uygulamalar, web uygulaması güvenlik duvarları (WAF) gibi Yukarı Akış cihazları tarafından erişim sağlayabilirsiniz.  Makale [App Service ortamları için bir WAF yapılandırma](app-service-app-service-environment-web-application-firewall.md) bu senaryosuna değiniyor.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Adanmış işlem kaynakları

Tüm işlem kaynakları bir App Service Ortamı'nda yalnızca tek bir aboneliğe ayrılmıştır ve bir App Service ortamı ile en fazla elli (50) bilgi işlem kaynakları özel kullanım için tek bir uygulama tarafından yapılandırılabilir.

App Service ortamı, bir ön uç bilgi işlem kaynak havuzu yanı sıra bir ila üç çalışan işlem kaynak havuzları oluşur.

Ön uç havuzu olarak App Service ortamı içindeki uygulama isteklerinin otomatik iyi Yük Dengeleme, SSL sonlandırma sorumlu olan bilgi işlem kaynaklarını içerir.

Her çalışan havuzu için ayrılan işlem kaynaklarını içeren [App Service planları][AppServicePlan], bir veya daha fazla Azure App Service uygulamalarını sırayla içerir.  Olabileceği en fazla üç farklı çalışan havuzları bir App Service Ortamı'nda, her çalışan havuzu farklı işlem kaynaklarına seçme esnekliğine sahip olursunuz.  

Örneğin, bu, geliştirme veya test uygulamaları için hedeflenen App Service planları için daha güçlü işlem kaynaklarıyla bir çalışan havuzu oluşturmanıza olanak sağlar.  İkinci (veya hatta üçüncü) çalışan havuzu, App Service planları üretim uygulamalarını çalıştırmak için hedeflenen daha güçlü işlem kaynakları kullanabilir.

Ön uç ve çalışan havuzları için kullanılabilen işlem kaynaklarının miktarını hakkında daha fazla bilgi için bkz. [bir App Service ortamını yapılandırma][HowToConfigureanAppServiceEnvironment].  

Desteklenen bir App Service Ortamı'nda kullanılabilir bilgi işlem kaynak boyutları hakkında daha fazla ayrıntı için inceleyin [App Service fiyatlandırması] [ AppServicePricing] sayfasında ve App Service ortamlarında kullanılabilir seçenekleri gözden geçirin Premium fiyatlandırma katmanı.

## <a name="virtual-network-support"></a>Sanal ağ desteği

App Service ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağı **veya** Klasik dağıtım modeli sanal ağı ([sanalağlarhakkındadahafazlabilgi] [MoreInfoOnVirtualNetworks]).  App Service ortamı her zaman bir sanal ağ ve daha kesin bir sanal ağ alt ağı mevcut olmadığından, sanal ağların her iki gelen ve giden ağ iletişimini denetlemek için güvenlik özelliklerini yararlanabilirsiniz.  

App Service ortamı genel bir IP adresiyle veya yalnızca bir Azure iç yük dengeleyici (ILB) adresi ile karşılıklı iç ya da Internet'e yönelik olabilir.

Kullanabileceğiniz [ağ güvenlik grupları] [ NetworkSecurityGroups] yer aldığı bir App Service ortamı alt ağına gelen ağ iletişimini kısıtlamak için.  Bu, Yukarı Akış cihazlarının ve web uygulaması güvenlik duvarları ve ağ SaaS sağlayıcıları gibi hizmetler arkasındaki uygulamaları çalıştırmanıza olanak sağlar.

Uygulamalar, iç veritabanları ve web hizmetleri gibi şirket kaynaklarına da sıklıkla erişmelidir.  Bu uç noktaları, bir Azure sanal ağ içinde akan yalnızca iç ağ trafiğini kullanımına yaygın bir yaklaşımdır.  App Service ortamı iç Hizmetleri aynı sanal ağda katıldığından sonra ortamında çalışan uygulamalar üzerinden erişilebilir uç noktalar dahil erişilebilmesini [siteden siteye] [ SiteToSite] ve [Azure ExpressRoute] [ ExpressRoute] bağlantıları.

App Service ortamları, sanal ağlar ve şirket içi ağlar ile nasıl çalıştığı hakkında daha fazla ayrıntı üzerinde aşağıdaki makalelere başvurun için [ağ mimarisi][NetworkArchitectureOverview], [gelen denetleme Trafik][ControllingInboundTraffic], ve [arka uçları güvenli bir şekilde bağlanan][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Başlarken

App Service ortamları ile çalışmaya başlamak için bkz: [nasıl için oluşturma bir App Service ortamı][HowToCreateAnAppServiceEnvironment]

App Service ortamı ağ mimarisi genel bakış için bkz. [ağ mimarisine genel bakış] [ NetworkArchitectureOverview] makalesi.

ExpressRoute ile bir App Service ortamını kullanma hakkında ayrıntılar için bkz. üzerinde aşağıdaki makalede [Expressroute ve App Service ortamları][NetworkConfigDetailsForExpressRoute].

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: https://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: ../overview-hosting-plans.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[LogicApps]: https://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  app-service-app-service-environment-geo-distributed-scale.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->