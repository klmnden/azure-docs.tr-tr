---
title: Giriş App Service ortamı v1
description: Tüm uygulamalarınızın çalıştırmak için güvenli, VNet katılmış, ayrılmış ölçek birimleri sağlayan uygulama hizmeti ortamı v1 özelliği hakkında bilgi edinin.
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
ms.openlocfilehash: ca30818b015e95594d3b2c9861d98f24174c0aea
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318172"
---
# <a name="introduction-to-app-service-environment-v1"></a>Giriş App Service ortamı v1

> [!NOTE]
> Bu makale hakkında uygulama hizmeti ortamı v1 yazılmıştır.  Uygulama hizmeti ortamı kullanmak daha kolay ve daha güçlü altyapısı üzerinde çalışan daha yeni bir sürümü var. Yeni sürüm Başlarken hakkında daha fazla bilgi edinmek için [uygulama hizmeti ortamı giriş](intro.md).
> 

## <a name="overview"></a>Genel Bakış
Bir uygulama hizmeti ortamını bir [Premium] [ PremiumTier] hizmet planı seçeneği [Azure App Service](../app-service-web-overview.md) güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar Azure App Service uygulamalarının Web Apps, Mobile Apps ve API uygulamaları gibi yüksek ölçekte.  

Uygulama hizmeti ortamları gerektiren uygulama iş yükleri için idealdir:

* Çok yüksek düzeyde ölçekleme
* Yalıtım ve güvenli ağ erişimi

Müşterilerin, tek bir Azure bölge içinde ve aynı zamanda birden çok Azure bölgeler arasında birden fazla App Service ortamları oluşturabilirsiniz.  Bu uygulama hizmeti ortamları yatay olarak ölçekleme durumu daha az uygulama katmanları yüksek RPS iş yüklerini desteklemek için ideal hale getirir.

Uygulama hizmeti ortamları yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış, her zaman bir sanal ağ içinde dağıtılır.  Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim müşterilerin sahip ve uygulamaları şirket içi kurumsal kaynaklara sanal ağları üzerinden yüksek hızlı güvenli bağlantı kurabilirsiniz.

Uygulama hizmeti ortamları nasıl yüksek ölçekli etkinleştirmek ve güvenli bir genel bakış için ağ erişimi, bkz: [AzureCon derinlemesine] [ AzureConDeepDive] App Service ortamları üzerinde!

Yatay olarak kullanarak ölçekleme üzerinde bir derin Dalış için kurulumu hakkında makaleyi birden fazla App Service ortamları bakın bir [coğrafi olarak dağıtılan uygulama ayak izini][GeodistributedAppFootprint].

AzureCon derinlemesine içinde gösterilen güvenlik mimarisi nasıl yapılandırılan görmek için makaleyi uygulanmasına bakın bir [güvenlik mimarisine katmanlı](app-service-app-service-environment-layered-security.md) App Service ortamları ile.

Uygulama hizmeti ortamları üzerinde çalışan uygulamalar, web uygulaması Güvenlik Duvarı (WAF) gibi Yukarı Akış cihazlar tarafından geçişli erişimleri olabilir.  Makale [App Service ortamları için bir WAF yapılandırma](app-service-app-service-environment-web-application-firewall.md) bu senaryo, kapsar. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="dedicated-compute-resources"></a>Adanmış bir işlem kaynakları
Uygulama hizmeti ortamı'nda işlem kaynakların tümünü yalnızca tek bir abonelik için ayrılmış ve bir uygulama hizmeti ortamı ile en fazla elli (50) işlem kaynaklarını özel kullanım için tek bir uygulama tarafından yapılandırılabilir.

Bir uygulama hizmeti ortamı bir ön uç işlem kaynak havuzu, hem de bir ile üç çalışan işlem kaynak havuzları oluşur. 

Ön uç havuzu, bir uygulama hizmeti ortamı içindeki uygulama isteklerinin iyi Otomatik Yük Dengeleme olarak SSL sonlandırma sorumlu işlem kaynaklarını içerir. 

Her bir çalışan havuzu için ayrılan işlem kaynaklarını içeren [App Service planları][AppServicePlan], bir veya daha fazla Azure App Service uygulama sırayla içerir.  Bir uygulama hizmeti ortamında en çok üç farklı çalışan havuzlarında olabilir olduğundan, her bir çalışan havuzu farklı işlem kaynakları seçme esnekliğine sahip.  

Örneğin, bu uygulama geliştirme veya test uygulamaları için hedeflenen hizmet planları için daha az güçlü işlem kaynakları ile bir çalışan havuzu oluşturmanıza olanak sağlar.  İkinci (veya hatta üçüncü) çalışan havuzunda App Service planları çalışan üretim uygulamalar için hedeflenen daha güçlü işlem kaynakları kullanabilir.

İşlem kaynakları için ön uç ve çalışan havuzları kullanılabilir miktarı hakkında daha fazla bilgi için bkz: [bir uygulama hizmeti ortamını yapılandırma][HowToConfigureanAppServiceEnvironment].  

Uygulama hizmeti ortamı'nda desteklenen kullanılabilir işlem kaynak boyutları hakkında daha fazla bilgi için başvurun [App Service fiyatlandırması] [ AppServicePricing] sayfasında ve App Service ortamları için kullanılabilir seçenekleri gözden geçir Premium fiyatlandırma katmanı.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Bir uygulama hizmeti ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağ **veya** Klasik dağıtım modeli sanal ağı ([sanal ağlarhakkındadahafazlabilgi] [MoreInfoOnVirtualNetworks]).  Bir uygulama hizmeti ortamı her zaman bir sanal ağ ve daha hassas bir şekilde bir sanal ağ alt ağı mevcut olmadığından, hem gelen ve giden ağ iletişimlerinin denetlemek için sanal ağlar'ın güvenlik özelliklerine yararlanabilirsiniz.  

Uygulama hizmeti ortamı'ortak bir IP adresiyle veya iç yalnızca bir Azure iç yük dengeleyici (ILB) adresi ile karşılıklı ya da Internet'e olabilir.

Kullanabileceğiniz [ağ güvenlik grubu] [ NetworkSecurityGroups] bir uygulama hizmeti ortamı bulunduğu alt ağı için gelen ağ iletişimleri kısıtlamak için.  Bu uygulamalar Yukarı Akış aygıtlar ve web uygulaması güvenlik duvarı ve ağ SaaS sağlayıcıları gibi hizmetler arkasında çalıştırmanıza olanak sağlar.

Uygulamalar, iç veritabanları ve web hizmetleri gibi şirket kaynaklarına da sıklıkla erişmelidir.  Bu uç noktaları, bir Azure sanal ağı içinde akan yalnızca iç ağ trafiğini kullanımına ortak bir yaklaşımdır.  Bir uygulama hizmeti ortamı iç Hizmetleri aynı sanal ağ katıldığı sonra ortamda çalışan uygulamalar, aracılığıyla erişilebilir uç noktalar dahil erişebilmeleri [siteden siteye] [ SiteToSite] ve [Azure ExpressRoute] [ ExpressRoute] bağlantıları.

Uygulama hizmeti ortamları sanal ağlar ve şirket içi ağlar ile nasıl çalıştığı hakkında daha fazla ayrıntı üzerinde aşağıdaki makalelere bakın için [ağ mimarisi][NetworkArchitectureOverview], [denetleme gelen Trafik][ControllingInboundTraffic], ve [arka uçlarını bağlanırken güvenli bir şekilde][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [nasıl için oluşturma bir uygulama hizmeti ortamı][HowToCreateAnAppServiceEnvironment]

Uygulama hizmeti ortamı ağ mimarisine genel bakış için bkz: [ağ mimarisine genel bakış] [ NetworkArchitectureOverview] makalesi.

ExpressRoute ile bir uygulama hizmeti ortamı kullanımıyla ilgili ayrıntılar görmek için aşağıdaki makaleye [hızlı rota ve App Service ortamları][NetworkConfigDetailsForExpressRoute].

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: ../azure-web-sites-web-hosting-plans-in-depth-overview.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  app-service-app-service-environment-geo-distributed-scale.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  app-service-web-configure-an-app-service-environment.md
[ControllingInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SecurelyConnectingToBackends]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NetworkArchitectureOverview]:  app-service-app-service-environment-network-architecture-overview.md
[NetworkConfigDetailsForExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->


