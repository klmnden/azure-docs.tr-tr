---
title: "App Service Ortamlarının Ağ Mimarisine Genel Bakış"
description: "Ağ topolojisi ofApp hizmeti ortamları mimarisine genel bakış."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 3362a55524da42914681db06b8d2c0da8df773d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="network-architecture-overview-of-app-service-environments"></a>App Service Ortamlarının Ağ Mimarisine Genel Bakış
## <a name="introduction"></a>Giriş
Uygulama hizmeti ortamları her zaman bir alt ağ içinde oluşturulmuş bir [sanal ağ] [ virtualnetwork] -uygulama hizmeti ortamı'nda çalışan uygulamalar aynı içinde sanal bulunan özel uç noktalar ile iletişim kurabilir Ağ topolojisi.  Müşteriler kendi sanal ağ altyapısı bölümlerini kilitlemek, bir uygulama hizmeti ortamı ile ortaya ağ iletişimi akışları türlerini anlamak önemlidir.

## <a name="general-network-flow"></a>Genel ağ akışı
Uygulama hizmeti ortamı (ana) uygulamaları için ortak bir sanal IP adresi (VIP) kullandığında, tüm gelen trafiği, genel VIP ulaşır.  Bu uygulamalar için HTTP ve HTTPS trafiğini yanı sıra, diğer trafik FTP, uzaktan hata ayıklama işlevselliği ve Azure yönetim işlemleri için içerir.  Ortak VIP kullanılabilen belirli bağlantı noktaları (gerekli ve isteğe bağlı) tam listesi için makaleyi bakın [gelen trafik denetleme] [ controllinginboundtraffic] bir uygulama hizmeti ortamı. 

Uygulama hizmeti ortamları, aynı zamanda bir ILB (iç yük dengeleyici) adresi olarak da adlandırılan yalnızca bir sanal ağ iç adresine bağlı çalışan uygulamaları destekler.  Uygulamalar ve bunun yanı sıra uzaktan hata ayıklama çağrıları için etkin ana, HTTP ve HTTPS trafiği üzerinde bir ILB ILB adresinde ulaşır.  ILB ana yapılandırmaların çoğu için FTP/FTPS trafiğinin de ILB adresinde ulaşırsınız.  Ancak Azure yönetim işlemlerini hala bir ILB, genel VIP bağlantı noktalarına 454/455 akar ana etkin.

Aşağıdaki diyagram, burada uygulamaları bir genel sanal IP adresine bağlı olan bir uygulama hizmeti ortamı için çeşitli gelen ve giden ağ akışına genel bir bakış gösterir:

![Genel ağ akışlar][GeneralNetworkFlows]

Bir uygulama hizmeti ortamı çeşitli özel müşteri uç noktalar ile iletişim kurabilir.  Örneğin, uygulama hizmeti ortamı'nda çalışan uygulamalar aynı sanal ağ topolojisinde Iaas sanal makinelerde çalışan veritabanı sunucuları bağlanabilir.

> [!IMPORTANT]
> Ağ Diyagramı baktığınızda, diğer işlem kaynaklarını"" uygulama hizmeti ortamı'ndan farklı bir alt ağ dağıtılır. Ana ile aynı alt ağdaki kaynakları dağıtma (dışında belirli içi ana yönlendirme) kaynaklarla ana bağlantısını engeller. Farklı bir alt ağa, bunun yerine (aynı VNET içinde) dağıtın. Uygulama hizmeti ortamı sonra bağlanabiliyor olacaktır. Hiçbir ek yapılandırma gerekli değildir.
> 
> 

Uygulama hizmeti ortamları yönetme ve bir uygulama hizmeti ortamı işletim için gerekli kaynaklar da Sql DB ve Azure Storage ile iletişim kurar.  Diğer uzak Azure bölgelerinde bulunan bir uygulama hizmeti ortamı kurduğu Sql ve depolama kaynaklarını bazıları uygulama hizmeti ortamı ile aynı bölgede yer alır.  Sonuç olarak, giden Internet bağlantısı her zaman düzgün çalışması uygulama hizmeti ortamı için gerekli değildir. 

Bir uygulama hizmeti ortamı bir alt ağda dağıtılan olduğundan, alt ağa gelen trafiği denetlemek için ağ güvenlik grupları kullanılabilir.  Bir uygulama hizmeti ortamına gelen trafiği denetlemek nasıl ilgili ayrıntılar için şu [makale][controllinginboundtraffic].

Uygulama hizmeti ortamı ', giden Internet bağlantısına izin vermek nasıl ilgili ayrıntılar için aşağıdaki makaleyi ile çalışma hakkında [hızlı rota][ExpressRoute].  Ve kullanarak zorlanan tünel siteden siteye bağlantı ile çalışırken makalesinde açıklanan aynı yaklaşımı geçerlidir.

## <a name="outbound-network-addresses"></a>Giden ağ adresleri
Bir IP adresi, her zaman bir uygulama hizmeti ortamı giden çağrıları yaptığında, giden çağrıları ile ilişkilidir.  Kullanılan belirli bir IP adresi çağrılan uç sanal ağ topolojisi içinde ya da sanal ağ topolojisi dışında bulunan olmasına bağlıdır.

Çağrılan uç nokta ise **dışında** sanal ağ topolojisini, ardından kullanılan giden (diğer adıyla giden NAT adresi) uygulama hizmeti ortamı genel VIP adresidir.  Bu adres özellikleri dikey penceresinde uygulama hizmeti ortamı için portal kullanıcı arabiriminde bulunabilir.

![Giden IP adresi][OutboundIPAddress]

Bu adres, genel VIP uygulama hizmeti ortamı'nda bir uygulama oluşturma ve ardından gerçekleştirme yeterlidir ASEs için de belirlenebilir bir *nslookup* uygulamanın adresinde. Sonuç IP adresi, hem genel VIP yanı sıra uygulama hizmeti ortamı'nın giden NAT adresi değil.

Çağrılan uç nokta ise **içinde** sanal ağ topolojisini, çağıran uygulama giden adresini uygulamayı çalıştıran tek işlem kaynağının iç IP adresi olacaktır.  Ancak yok kalıcı uygulamalara iç IP adresleri sanal ağ eşlemesi.  Uygulamaları farklı işlem kaynakları ve kaynakları bir uygulama hizmeti ortamı ölçeklendirme işlemleri nedeniyle değiştirebilirsiniz kullanılabilir işlem havuzu arasında taşıyabilirsiniz.

Ancak, bir uygulama hizmeti ortamı her zaman bir alt ağ içinde bulunduğundan, bir uygulama çalıştıran bir işlem kaynağın iç IP adresi her zaman alt ağ CIDR aralığı içinde kalan sağlanır.  Diğer uç noktaları sanal ağda güvenli şekilde hassas ACL'leri ya da ağ güvenlik grupları kullanıldığında, sonuç olarak, uygulama hizmeti ortamı içeren alt ağ aralığı erişim verilmesi gerekir.

Aşağıdaki diyagramda bu kavramları daha ayrıntılı olarak gösterilmiştir:

![Giden ağ adresleri][OutboundNetworkAddresses]

Yukarıdaki diyagramda:

* Uygulama hizmeti ortamı genel VIP 192.23.1.2 olduğundan, "Internet" Uç noktalara çağrıları yapılırken kullanılan giden IP adresidir.
* Uygulama hizmeti ortamı için içeren alt ağ CIDR aralığı 10.0.1.0/26 ' dir.  Diğer uç noktalar aynı sanal ağ altyapısı içinde çağrıları uygulamalardan herhangi bir yerde bu adres aralığı içinde kaynaklanan olarak görürsünüz.

## <a name="calls-between-app-service-environments"></a>Uygulama hizmeti ortamları arasında çağrıları
Daha karmaşık bir senaryo, aynı sanal ağda birden çok uygulama hizmeti ortamında dağıtmak ve başka bir uygulama hizmeti ortamı için bir uygulama hizmeti ortamı'ndan giden çağrıları yapma ortaya çıkabilir.  Bu tür uygulama hizmeti ortamı'nın çağrıları ayrıca "Internet" çağrısı olarak kabul edilecek arası.

(Örneğin bir uygulama hizmeti ortamı üzerinde Aşağıdaki diyagramda uygulamalarıyla katmanlı mimari örneği gösterilmektedir Web uygulamaları "Ön kapı") uygulamaları (örn. iç uç API uygulamaları amaçlanmayan Internet'ten erişilebilir) ikinci bir uygulama hizmeti ortamı çağrılması. 

![Uygulama hizmeti ortamları arasında çağrıları][CallsBetweenAppServiceEnvironments] 

Yukarıdaki örnekte, uygulama hizmeti ortamı'nı "Ana bir" 192.23.1.2 giden IP adresi vardır.  Bu bilgisayarda çalışan bir uygulama, uygulama hizmeti ortamı'nın bir ikinci uygulama hizmeti ortamı üzerinde ("ana iki") çalışan bir uygulama için giden bir çağrı aynı sanal ağda, giden çağrı bulunan yapar olacak bir "Internet" çağrısı olarak kabul.  Uygulama hizmeti ortamı ikinci gelen ağ trafiğini sonuç olarak gösterir (örn. alt ağ adres aralığı ilk uygulama hizmeti ortamı) 192.23.1.2 kaynaklanan olarak.

Her iki uygulama hizmeti ortamları aynı Azure bölgesinde bulunduğunda çağrıları farklı uygulama hizmeti ortamları arasında "Internet" çağrısı olarak kabul edilir olsa bile ağ trafiğini bölgesel Azure ağ üzerinde kalır ve fiziksel olarak akış üzerinden almayacak ortak Internet.  Sonuç olarak bir ağ güvenlik grubu ikinci uygulama hizmeti ortamı alt ağda yalnızca ortamından ilk App Service (giden IP adresini 192.23.1.2 olduğu) böylece uygulama arasında güvenli iletişim sağlamak, gelen çağrılarına izin vermek için kullanabileceğiniz Hizmeti ortamları.

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
App Service ortamları tarafından kullanılan bağlantı noktaları hakkında ayrıntılar gelen ve gelen trafiği denetlemek için ağ güvenlik gruplarını kullanarak kullanılabilir [burada][controllinginboundtraffic].

Kullanıcı kullanımıyla ilgili ayrıntılar tanımlanan yollar giden Internet erişimi App Service ortamları için bu konuda kullanılabilir verin [makale][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

