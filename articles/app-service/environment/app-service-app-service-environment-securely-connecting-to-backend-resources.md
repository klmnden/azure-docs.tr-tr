---
title: Bir uygulama hizmeti ortamındaki arka uç kaynaklarına güvenli bir şekilde bağlanma
description: Bir uygulama hizmeti ortamındaki arka uç kaynaklarına güvenli bir şekilde bağlanma hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 2fb13a4dac923a19dc12910cb1b78e909b93abe1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317580"
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Bir uygulama hizmeti ortamındaki arka uç kaynaklarına güvenli bir şekilde bağlanma
## <a name="overview"></a>Genel Bakış
Uygulama hizmeti ortamı'nda her zaman oluşturulduğundan **ya da** bir Azure Resource Manager sanal ağ **veya** Klasik dağıtım modeli [sanal ağ] [ virtualnetwork], bir uygulama hizmeti ortamı giden bağlantılar diğer arka uç kaynaklarına yalnızca sanal ağ üzerinden akış.  Haziran 2016'da yapılan son bir değişiklikle ASEs ortak adres aralıkları veya RFC1918 adres alanları (özel adresleri) kullanan sanal ağları içinde dağıtılabilir.  

Örneğin, bağlantı noktası 1433 kilitli olan sanal makinelerin bir kümede çalışan bir SQL Server olabilir.  Uç nokta yalnızca aynı sanal ağda diğer kaynaklara erişime izin verecek şekilde ACLd olabilir.  

Başka bir örnek olarak, hassas uç noktaları şirket içinde çalıştırabilir ve Azure'a ya da bağlı [siteden siteye] [ SiteToSite] veya [Azure ExpressRoute] [ ExpressRoute] bağlantıları.  Sonuç olarak, siteden siteye veya ExpressRoute tüneller bağlı sanal ağlar kaynaklarında yalnızca şirket içi uç noktaları erişebilir olacaktır.

Tüm bu senaryolar için bir uygulama hizmeti ortamı üzerinde çalışan uygulamalar çeşitli sunuculara ve kaynaklara güvenli bir şekilde bağlanabilmek için olacaktır.  Uygulama hizmeti ortamı'nda özel uç noktalar aynı sanal ağda çalışan uygulamalardan giden trafik (veya aynı sanal ağa bağlı), sanal ağ üzerinden yalnızca akış olur.  Özel uç noktalarına giden trafiği genel Internet üzerinden akar değil.

Bir uyarı Uç noktalara bir sanal ağ içindeki bir uygulama hizmeti ortamı'ndan giden trafiği için geçerlidir.  Uygulama hizmeti ortamları uç noktaları bulunan sanal makinelerin erişemiyor **aynı** uygulama hizmeti ortamı olarak alt ağ.  Uygulama hizmeti ortamları yalnızca uygulama hizmeti ortamı tarafından özel kullanım için ayrılmış bir alt ağ içinde dağıtılan sürece bu normalde bir sorun olmamalıdır.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden bağlantısı ve DNS gereksinimleri
Bir uygulama hizmeti düzgün şekilde çalışabilmesi ortamı için çeşitli uç noktalarına giden erişim gerektirir. Tam bir ana tarafından kullanılan dış uç noktalar "Ağ bağlantısı gerekli" bölümünde listelenmiştir [ExpressRoute için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi.

Uygulama hizmeti ortamları sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Bir uygulama hizmeti ortamı oluşturulduktan sonra DNS yapılandırmasını herhangi bir nedenle değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını almak için bir uygulama hizmeti ortamı zorlayabilirsiniz.  Portalı'nda uygulama hizmeti ortamı yönetim dikey pencerenin üstündeki "Yeniden Başlat" simgesini kullanarak çalışırken bir ortamı yeniden başlatma tetikleme yeni DNS yapılandırmasını almak ortam neden olur.

Ayrıca, tüm özel DNS sunucuları vnet üzerinde bir uygulama hizmeti ortamı oluşturmadan önce vaktinden Kurulum olması önerilir.  Bir uygulama hizmeti ortamı oluşturulurken bir sanal ağın DNS yapılandırması değiştiyse, uygulama hizmeti ortamı oluşturma işlemi başarısız olan neden olur.  Benzer bir damarlı içinde özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki var ve DNS sunucusu erişilemez veya kullanılamaz, uygulama hizmeti ortamı oluşturma işlemi de başarısız olur.

## <a name="connecting-to-a-sql-server"></a>Bir SQL Server'a bağlanma
Yaygın olarak kullanılan SQL Server yapılandırması 1433 bağlantı noktasını dinleyen bir uç nokta vardır:

![SQL sunucusu uç noktası][SqlServerEndpoint]

Bu uç noktaya trafiği kısıtlama için iki yaklaşım vardır:

* [Ağ erişim denetim listeleri] [ NetworkAccessControlLists] (ağ ACL'leri)
* [Ağ güvenlik grupları][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Bir ağ ACL ile erişimi kısıtlama
Bağlantı noktası 1433'ü bir ağ erişim denetimi listesi kullanılarak güvenli hale getirilebilir.  Whitelists istemci aşağıdaki örnekte, bir sanal ağ içinde kaynaklanan adresleri ve diğer tüm istemciler için erişim engellenir.

![Ağ erişim denetim listesi örneği][NetworkAccessControlListExample]

SQL Server için SQL Server örneğini kullanarak bağlanmak olarak uygulama hizmeti ortamı'nda aynı sanal ağ içinde çalışan uygulamaların **VNet iç** SQL Server sanal makine için IP adresi.  

Aşağıdaki örnek bağlantı dizesi, özel IP adresini kullanarak SQL Server başvurur.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Sanal makine genel bir uç nokta da olsa da, genel IP adresi kullanılarak yapılan bağlantı girişimleri ACL ağ nedeniyle kabul edilmeyecek. 

## <a name="restricting-access-with-a-network-security-group"></a>Bir ağ güvenlik grubu ile erişimi kısıtlama
Bir ağ güvenlik grubuyla erişim güvenliğini sağlamak için alternatif bir yaklaşımdır.  Tek tek sanal makineleri veya sanal makine içeren bir alt ağ için ağ güvenlik grupları uygulanabilir.

İlk ağ güvenlik grubu oluşturulması gerekir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Yalnızca VNet iç trafiği erişimi kısıtlamak, bir ağ güvenlik grubuyla çok kolaydır.  Varsayılan kuralları bir ağ güvenlik grubu sadece erişim aynı sanal ağdaki diğer ağ istemcilerden izin verir.

Sonuç olarak SQL Server erişimi kilitleme ya da SQL Server ya da sanal makineleri içeren alt ağ çalışan sanal makineleri için ağ güvenlik grubu, varsayılan kuralları ile uygulama olarak kadar basittir.

Aşağıdaki örnek bir ağ güvenlik grubu içeren alt ağ için geçerlidir:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Nihai sonuç, VNet iç erişim sağlarken dış erişimi engelleme güvenlik kurallar kümesidir:

![Varsayılan ağ güvenlik kuralları][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamı giriş][IntroToAppServiceEnvironment]

Uygulama hizmeti ortamına gelen trafik denetleme geçici daha fazla bilgi için bkz [bir uygulama hizmeti ortamına gelen trafik denetleme][ControlInboundASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
