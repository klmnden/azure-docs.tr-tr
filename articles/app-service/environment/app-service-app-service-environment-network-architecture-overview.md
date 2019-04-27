---
title: Ağ mimarisine genel App Service ortamları - Azure
description: Ağ topolojisi ofApp Service ortamları mimari genel bakış.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 0d7d4af46e54ad89e0d084cb15af13e56115e996
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60765320"
---
# <a name="network-architecture-overview-of-app-service-environments"></a>App Service Ortamlarının Ağ Mimarisine Genel Bakış
## <a name="introduction"></a>Giriş
App Service ortamları her zaman bir alt ağ içinde oluşturulmuş bir [sanal ağ] [ virtualnetwork] -App Service ortamında çalışan uygulamalar aynı içinde sanal bulunan özel uç noktalar ile iletişim kurabilir Ağ topolojisi.  Müşterilerin kendi sanal ağ altyapısı bölümlerini kilitleme olduğundan, bir App Service ortamı ile ortaya çıkan ağ iletişimi akışlar türlerini anlamak önemlidir.

## <a name="general-network-flow"></a>Genel ağ akışı
App Service ortamı (ASE) uygulamaları için bir genel sanal IP adresi (VIP) kullandığında, tüm gelen trafiği üzerinde genel VIP ulaşır.  Bu uygulamalar için HTTP ve HTTPS trafiğini yanı sıra, diğer trafik FTP, uzaktan hata ayıklama işlevselliği ve Azure yönetim işlemlerini içerir.  Genel VIP üzerinde kullanılabilir olan belirli bağlantı noktalarını (gerekli ve isteğe bağlı) tam listesi için makaleye bakın [gelen trafiği denetleme] [ controllinginboundtraffic] bir App Service ortamı için. 

App Service ortamları, ayrıca bir ILB (iç yük dengeleyici) adresi da adlandırılan yalnızca bir sanal ağ iç adresi, ilişkili çalışan uygulamaları destekler.  ILB etkin ASE, HTTP ve HTTPS trafiğini uzaktan hata ayıklama çağrıları yanı sıra, uygulamalar için ILB adresini temel ulaşır.  En yaygın ILB ASE yapılandırmaları için FTP/FTPS trafiğinin ILB adresini temel de ulaşır.  Ancak Azure yönetim işlemleri hala bir ILB, genel VIP üzerindeki bağlantı noktalarına 454/455 akar ASE etkin.

Aşağıdaki diyagramda, uygulamaları burada bir genel sanal IP adresine bağlı olan bir App Service ortamı için çeşitli gelen ve giden ağ akışlarına genel bakış gösterilmektedir:

![Genel ağ akışlar][GeneralNetworkFlows]

App Service ortamı, çeşitli özel müşteri uç noktaları ile iletişim kurabilir.  Örneğin, App Service ortamında çalışan uygulamalar, aynı sanal ağ topolojisinde Iaas sanal makinelerde çalışan veritabanı sunucuları için bağlanabilirsiniz.

> [!IMPORTANT]
> Ağ diyagramın bakarak, "diğer işlem kaynakları" App Service Ortamı'ndan farklı bir alt ağda dağıtılır. ASE ile aynı alt ağdaki kaynaklara dağıtma ASE bağlantısı bu kaynaklara (dışında belirli içi ASE yönlendirme) engeller. Farklı bir alt ağa (aynı sanal ağda) bunun yerine dağıtın. App Service ortamı, ardından bağlanmak mümkün olacaktır. Hiçbir ek yapılandırma gerekli değildir.
> 
> 

App Service ortamları yönetme ve App Service ortamı işletme için gerekli kaynaklar ayrıca Azure depolama ve Sql DB ile iletişim kurar.  Diğer uzak Azure bölgelerinde bulunan App Service ortamı ile iletişim kuran Sql ve depolama kaynakları bazıları App Service ortamı ile aynı bölgede yer alır.  Sonuç olarak, İnternet'e giden bağlantı her zaman düzgün çalışması App Service ortamı için gerekli değildir. 

App Service ortamı, bir alt ağda dağıtılmış olduğundan, alt ağına gelen trafiği denetlemek için ağ güvenlik grupları kullanılabilir.  Bir App Service ortamına gelen trafiği denetleme konusunda bilgi için şu [makale][controllinginboundtraffic].

Bir App Service ortamından giden Internet bağlantısına izin hakkında ayrıntılı bilgi görmek için aşağıdaki makaleyi ile çalışma hakkında [Express Route][ExpressRoute].  Siteden siteye bağlantı ile çalışırken aynı yaklaşımı adlı makalede açıklanan uygular ve Kullanarak zorlamalı tünel.

## <a name="outbound-network-addresses"></a>Giden ağ adresleri
Bir IP adresi, her zaman bir App Service ortamı giden bir çağrı yaptığında, giden çağrıları ile ilişkilidir.  Kullanılan belirli bir IP adresi, çağrılan uç sanal ağ topolojisi içinde veya sanal ağ topolojisini dışında bulunan olmasına göre değişir.

Çağrılan uç noktaysa **dışında** sanal ağ topolojisi, ardından kullanılan çıkış (giden NAT adresi olarak da bilinir) App Service ortamı, genel VIP adresidir.  Bu adres, Özellikler dikey penceresinde App Service ortamı için portal kullanıcı arabiriminde bulunabilir.

![Giden IP adresi][OutboundIPAddress]

Bu adres, yalnızca App Service ortamında uygulama oluşturma ve ardından gerçekleştirerek bir genel VIP sahip Ase'ler için de belirlenebilir bir *nslookup* uygulamanın adresinde. Sonuç IP adresi, hem genel VIP hem de App Service ortamının giden NAT adresi ' dir.

Çağrılan uç noktaysa **içinde** sanal ağ topolojisi, çağıran uygulama giden adresini uygulamayı çalıştıran tek bir işlem kaynağı iç IP adresi olur.  Ancak yok kalıcı uygulamalara iç IP adreslerini sanal ağ eşlemesi.  Uygulamaları, farklı işlem kaynaklarına ve havuzu kullanılabilir bilgi işlem kaynakları bir App Service ortamı ölçeklendirme işlemleri nedeniyle değiştirebilirsiniz arasında taşıyabilirsiniz.

Ancak, bir App Service ortamı her zaman bir alt ağ içinde bulunduğundan, iç IP adresi bir uygulama çalıştıran bir işlem kaynağı her zaman alt ağın CIDR aralığında kalan garanti edilir.  Ayrıntılı ACL veya ağ güvenlik grupları sanal ağ içindeki diğer Uç noktalara erişimi güvenli hale getirmek için kullanıldığında, sonuç olarak, App Service ortamını içeren alt ağ aralığı erişim verilmesi gerekir.

Aşağıdaki diyagram bu kavramları daha ayrıntılı olarak gösterilmiştir:

![Giden ağ adresleri][OutboundNetworkAddresses]

Yukarıdaki diyagramda:

* App Service ortamı, genel VIP 192.23.1.2 olduğundan, "Internet" uç noktalarına çağrı yapılırken kullanılan giden IP adresidir.
* 10.0.1.0/26 için App Service ortamını içeren alt ağın CIDR aralığı.  Diğer uç noktalar aynı sanal ağ alt yapısında uygulamalardan gelen çağrıları yere bu adres aralığında kaynaklanan olarak görürsünüz.

## <a name="calls-between-app-service-environments"></a>App Service ortamları arasında çağrıları
Daha karmaşık bir senaryo, aynı sanal ağda birden fazla App Service ortamlarında dağıtmak ve başka bir App Service ortamı için bir App Service ortamından giden çağrıları yapmak ortaya çıkabilir.  Bu tür alanları arası App Service ortamı çağrılar da "Internet" çağrısı olarak kabul edilir.

Bir App Service ortamında uygulamaları ile katmanlı mimari örneği aşağıdaki diyagramda (örn gösterilmektedir Web uygulamaları "Ön kapı") uygulamaları (örneğin iç arka uç API apps hazırlanmamış Internet'ten erişilebilir olmasını) ikinci bir App Service ortamı çağırma. 

![App Service ortamları arasında çağrıları][CallsBetweenAppServiceEnvironments] 

Yukarıdaki örnekte, App Service ortamı "ASE bir" 192.23.1.2 giden IP adresi vardır.  App Service ortamı giden bir çağrı bir ikinci App Service ortamında ("ASE iki") çalışan bir uygulamanın aynı sanal ağdaki giden çağrı bulunan yapar olur, bu üzerinde çalışan bir uygulamanın, bir "Internet" çağrısı olarak kabul.  Sonuç olarak, ikinci gelen ağ trafiğini App Service ortamı gösterilir (yani olmayan alt ağ adres aralığı ilk App Service ortamı) 192.23.1.2 kaynaklanan olarak.

Aynı Azure bölgesinde iki App Service ortamları bulunduğunda, farklı App Service ortamları arasında çağrıları "Internet" çağrısı olarak kabul edilir olsa bile ağ trafiğini bölgesel Azure ağı üzerinde kalır ve fiziksel olarak akış üzerinden sağlamaz Genel İnternet'e.  Bir ağ güvenlik grubu ikinci App Service ortamı alt ağda yalnızca ilk App Service (giden IP adresleri 192.23.1.2 değer) bu nedenle uygulama arasında güvenli iletişim sağlama ortamı, gelen çağrıların izin vermek için sonuç olarak kullanabilirsiniz Service ortamları.

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
App Service ortamları tarafından kullanılan bağlantı noktaları hakkında ayrıntılı bilgi gelen ve gelen trafiği denetlemek için ağ güvenlik gruplarını kullanarak kullanılabilir [burada][controllinginboundtraffic].

Kullanıcı kullanımıyla ilgili ayrıntılar tanımlı bu mevcut App Service ortamları için giden Internet erişimi vermek için yollar [makale][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[ExpressRoute]:  app-service-app-service-environment-network-configuration-expressroute.md

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

