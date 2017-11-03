---
title: "Azure uygulama hizmeti ortamları giriş"
description: "Azure uygulama hizmeti ortamları kısa genel bakış"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 322cf2ebbe83d00fcebcec618e07141d26f4f255
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-app-service-environments"></a>Uygulama hizmeti ortamları giriş #
 
## <a name="overview"></a>Genel Bakış ##

Azure uygulama hizmeti ortamı App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir Azure uygulama hizmeti özelliğidir. Bu özellik, web uygulamalarınızı barındırabilir [mobil uygulamaları][mobileapps], API uygulamaları ve [işlevleri][Functions].

Uygulama hizmeti ortamları (ASEs) gerektiren uygulama iş yükleri için uygundur:

- Çok büyük ölçekli.
- Yalıtım ve güvenli ağ erişimi.
- Yüksek bellek kullanımı.

Müşteriler, birden çok ASEs tek bir Azure bölgesi içinde veya birden çok Azure bölgeleri oluşturabilirsiniz. Bu esneklik ASEs yatay olarak ölçekleme durum bilgisiz uygulama katmanları yüksek RPS iş yüklerini desteklemek için ideal hale getirir.

ASEs yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış, her zaman bir sanal ağ içinde dağıtılır. Müşteriniz uygulama gelen ve giden ağ trafiği üzerinde hassas bir denetim yok. Uygulamaları yüksek hızlı güvenli VPN şirket içi kurumsal kaynaklara bağlantı kurabilir.

* Güvenli ağ erişimi ile yüksek ölçekli uygulama barındırma ASEs etkinleştirin. Daha fazla bilgi için bkz: [AzureCon derinlemesine](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) ASEs üzerinde.
* Birden çok ASEs yatay olarak ölçeklendirmek için kullanılabilir. Daha fazla bilgi için bkz: [coğrafi olarak dağıtılan uygulama ayak izini ayarlamak nasıl](app-service-app-service-environment-geo-distributed-scale.md).
* ASEs AzureCon derinlemesine gösterildiği gibi güvenlik mimarisi yapılandırmak için kullanılabilir. AzureCon derinlemesine içinde gösterilen güvenlik mimarisi nasıl yapılandırılan görmek için bkz: [makale katmanlı güvenlik mimarisi uygulama konusunda](app-service-app-service-environment-layered-security.md) uygulama hizmeti ortamları ile.
* ASEs üzerinde çalışan uygulamalar, web uygulaması Güvenlik Duvarı (WAFs) gibi Yukarı Akış cihazlar tarafından geçişli erişimleri olabilir. Daha fazla bilgi için bkz: [WAF uygulama hizmeti ortamları için yapılandırma](app-service-app-service-environment-web-application-firewall.md).

## <a name="dedicated-environment"></a>Ayrılmış ortamda ##

Bir ana yalnızca tek bir abonelik için ayrılmış ve 100 örnekleri barındırabilir. Aralık, 100 Tek Örnekli uygulama hizmeti planları için tek bir uygulama hizmeti planı ve arasındaki her şeyi 100 örnekleri yayılabilir.

Bir ana ön uçlar ve çalışanları oluşur. HTTP/HTTPS sonlandırma ve bir ana içinde uygulama isteklerinin otomatik Yük Dengeleme için ön uçlar sorumludur. ANA uygulama hizmeti planlarında çıkışı ölçeklenir gibi ön uçlar otomatik olarak eklenir.

Çalışanları müşteri uygulamaları barındıran rolleridir. Çalışanlar, üç sabit boyutlarında kullanılabilir:

* Bir çekirdek/3.5 GB RAM
* İki çekirdek/7 GB RAM
* Dört çekirdek/14 GB RAM

Müşteriler, ön uçlar ve çalışanları yönetmek gerekmez. Tüm altyapı, kendi uygulama hizmeti planları müşterilerin ölçeklenebilir olarak otomatik olarak eklenir. Uygulama hizmeti planları oluşturulur veya ASE'de ölçeklendirilmiş gibi gerekli altyapı eklenip uygun şekilde kaldırılabilir.

Düz aylık hızı için altyapıyı ödeyen ve ana boyutunu değiştirmez bir ana için yoktur. Ayrıca, uygulama hizmeti planı çekirdek başına bir maliyeti yoktur. ASE'de barındırılan tüm uygulamaları SKU fiyatlandırma Isolated içinde bulunur. Bir ana için fiyatlandırma hakkında daha fazla bilgi için bkz: [uygulama hizmeti fiyatlandırma] [ Pricing] sayfasında ve ASEs için kullanılabilir seçenekleri gözden geçirin.

## <a name="virtual-network-support"></a>Sanal ağ desteği ##

Bir ana yalnızca bir Azure Resource Manager sanal ağında oluşturulabilir. Azure sanal ağlar hakkında daha fazla bilgi için bkz: [Azure sanal ağları SSS](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). Bir ana sanal ağ ve daha hassas bir şekilde her zaman bir sanal ağ alt ağı içerisinde bulunmaktadır. Sanal ağların güvenlik özellikleri, uygulamalarınız için gelen ve giden ağ iletişimlerinin denetlemek için kullanabilirsiniz.

Bir ana İnternete dönük bir genel IP adresiyle veya dahili kullanıma yönelik yalnızca Azure iç yük dengeleyiciye (ILB) adresi olabilir.

[Ağ güvenlik grupları] [ NSGs] bir ana bulunduğu alt ağı için gelen ağ iletişimleri kısıtlayın. Nsg'ler, Yukarı Akış aygıtlar ve WAFs ve ağ SaaS sağlayıcıları gibi hizmetler arkasında uygulamaları çalıştırmak için kullanabilirsiniz.

Uygulamalar da sıklıkla iç veritabanları gibi şirket kaynaklarına erişmek ve web Hizmetleri gerekir. Şirket içi ağınıza bir VPN bağlantısı olan bir sanal ağdaki ana dağıtırsanız, ana uygulamalarda şirket içi kaynaklara erişebilir. Bu özellik, VPN olmasına bakılmaksızın doğrudur bir [siteden siteye](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) veya [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) VPN.

ASEs sanal ağlar ve şirket içi ağlar ile nasıl çalıştığı hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı ağ konuları][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service Ortamı v1 ##

Uygulama hizmeti ortamı iki sürümü vardır: ASEv1 ve ASEv2. Yukarıdaki bilgiler üzerinde ASEv2 dayanır. Bu bölümde ASEv1 ASEv2 arasındaki farkları gösterilmiştir. 

ASEv1 tüm kaynakları el ile yönetmesi gerekir. Ön Uçları, çalışanlar ve IP tabanlı SSL için kullanılan IP adresleri dahildir. Uygulama hizmeti planınızı ölçeklendirebilirsiniz önce ilk genişleme da barındırmak istediğiniz çalışan havuzunda yapmak gerekir.

ASEv1 ASEv2 öğesinden farklı bir fiyatlandırma modelini kullanır. ASEv1 içinde ayrılmış her çekirdek için ücret ödersiniz. Ön Uçları veya tüm iş yükleri barındıran olmayan çalışanlar için kullanılan çekirdek dahildir. ASEv1 içinde varsayılan en büyük ölçekli bir ana 55 toplam konaklar boyutudur. Bu, çalışanlar ve ön uçlar içerir. ASEv1 bir avantajı, onu bir Klasik sanal ağı ve Resource Manager sanal ağ içinde dağıtılabilir ' dir. ASEv1 hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı v1 giriş][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
