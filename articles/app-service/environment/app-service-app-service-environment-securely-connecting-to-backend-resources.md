---
title: Güvenli bir şekilde yeniden bağlanma end - App Service ortamından Azure kaynakları
description: Bir App Service ortamından arka uç kaynaklarına güvenli bir şekilde bağlanma hakkında bilgi edinin.
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
ms.custom: seodec18
ms.openlocfilehash: aea51234d26e5dbaef836419c2a13a12f8083e6f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130713"
---
# <a name="connect-securely-to-back-end-resources-from-an-app-service-environment"></a>Güvenli bir şekilde yeniden bağlanma son kaynaklardan bir App Service ortamı
## <a name="overview"></a>Genel Bakış
App Service ortamı, her zaman oluşturulduğundan **ya da** bir Azure Resource Manager sanal ağı **veya** Klasik dağıtım modeli [sanal ağ] [ virtualnetwork], App Service ortamı giden bağlantılar diğer arka uç kaynaklarına yalnızca sanal ağ üzerinden flow.  Haziran 2016'da yapılan son değişikliği ile ase genel adres aralıkları ya da RFC1918 adres alanları (yani özel adresler) kullanan sanal ağlara dağıtılabilir.  

Örneğin, bağlantı noktası 1433'ü kilitli ile sanal makine bir kümede çalışan bir SQL Server olabilir.  Uç nokta yalnızca aynı sanal ağ diğer kaynaklara erişime izin verecek şekilde ACLd olabilir.  

Başka bir örnek olarak, hassas uç noktalar şirket içi çalışabilir ve ya da Azure'a bağlanması [siteden siteye] [ SiteToSite] veya [Azure ExpressRoute] [ ExpressRoute] bağlantıları.  Sonuç olarak, yalnızca kaynak siteden siteye veya ExpressRoute tüneller bağlı sanal ağlarda bulunan şirket içi Uç noktalara erişebilir olacaktır.

Tüm bu senaryolar için bir App Service ortamında çalışan uygulamalar çeşitli sunucuları ve kaynakları güvenli bir şekilde bağlamak mümkün olacaktır.  Bir App Service Ortamı'nda özel uç noktalar aynı sanal ağda çalışan uygulamalardan giden trafik (veya aynı sanal ağa bağlı), sanal ağ üzerinden yalnızca akış olacaktır.  Özel uç noktalarına giden trafiği genel Internet üzerinden akışı değil.

Bir uyarı, uç noktalarına bir sanal ağ içindeki App Service Ortamı'ndan giden trafiği için geçerlidir.  App Service ortamları bulunan sanal makinelerin uç noktalarına ulaşamıyor **aynı** App Service ortamı alt.  App Service ortamları yalnızca App Service ortamı tarafından özel kullanım için ayrılmış bir alt ağa dağıtılır sürece bu normalde bir sorunu olmamalıdır.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden bağlantı ve DNS gereksinimleri
Bir App Service düzgün çalışması ortamı için çeşitli uç noktalarına giden erişim gerektirir. Tam bir ASE tarafından kullanılan dış uç noktaları "Ağ bağlantısı gerekli" bölümünde listesidir [ExpressRoute için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi.

App Service ortamları sanal ağ için yapılandırılmış geçerli bir DNS altyapısına gerektirir.  App Service ortamı oluşturulduktan sonra herhangi bir nedenle DNS yapılandırmasını değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını seçmek için bir App Service ortamı zorlayabilirsiniz.  Portalda App Service ortamı yönetim dikey penceresinin üstünde bulunan "Restart" simgesini kullanarak sıralı bir ortamı yeniden harekete yeni DNS yapılandırmasını seçmek için ortamı neden olur.

Özel DNS sunucuları vnet üzerinde App Service ortamı oluşturmadan önce önceden kurulum olması önerilir.  App Service ortamı oluşturma sırasında bir sanal ağın DNS yapılandırması değiştiyse, App Service ortamı oluşturma işlemi başarısız olmasıyla sonuçlanır.  Benzer bir damarlı içinde özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki var ve DNS sunucusu erişilemez veya kullanılamaz, App Service ortamı oluşturma işlemi de başarısız olur.

## <a name="connecting-to-a-sql-server"></a>Bir SQL Server'a bağlanma
Yaygın olarak kullanılan SQL Server yapılandırması 1433 numaralı bağlantı noktasını dinleyen bir uç nokta vardır:

![SQL Server uç noktası][SqlServerEndpoint]

Bu uç noktaya trafiği kısıtlamak için iki yaklaşım vardır:

* [Ağ erişim denetimi listelerini] [ NetworkAccessControlLists] (ağ ACL'leri)
* [Ağ güvenlik grupları][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Bir ağ ACL ile erişimi kısıtlama
1433 numaralı bağlantı noktasını kullanarak bir ağ erişim denetimi listesi güvenliği sağlanabilir.  Beyaz istemci aşağıdaki örnekte, bir sanal ağın içinde kaynaklanan adresleri ve diğer tüm istemciler için erişim engellenir.

![Ağ erişim denetimi listesi örneği][NetworkAccessControlListExample]

SQL Server için SQL Server örneği kullanarak bağlanabilir olarak App Service Ortamı'nda aynı sanal ağda çalışan uygulamalar **VNet iç** SQL Server sanal makine için IP adresi.  

Aşağıdaki örnek bağlantı dizesi, özel IP adresini kullanarak SQL Server başvuruyor.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Sanal makine ortak uç nokta da olsa da, ağ ACL nedeniyle genel IP adresini kullanarak bağlantı denemeleri reddedilir. 

## <a name="restricting-access-with-a-network-security-group"></a>Bir ağ güvenlik grubu ile erişimi kısıtlama
Bir ağ güvenlik grubuyla erişim güvenliğini sağlamak için alternatif bir yaklaşımdır.  Ayrı sanal makinelere veya sanal makinesi içeren bir alt ağ için ağ güvenlik grupları uygulanabilir.

İlk kez bir ağ güvenlik grubu oluşturulması gerekir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Erişimini yalnızca VNet iç trafik bir ağ güvenlik grubuyla çok basit bir işlemdir.  Varsayılan bir ağ güvenlik grubu kuralları yalnızca aynı sanal ağdaki diğer ağ istemcilerden gelen erişime izin.

SQL Server'a erişim sonucunda kilitlemek, kendi varsayılan kurallarla bir ağ güvenlik grubu ya da SQL Server ya da sanal makineleri içeren alt ağın çalışan sanal makineler için uygulanan kadar kolaydır.

Aşağıdaki örnekte, bir ağ güvenlik grubu içeren alt ağa uygulanır:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

Sonuç bir VNet iç erişim verirken dış erişimi engelleyen güvenlik kurallar kümesidir:

![Varsayılan ağ güvenlik kuralları][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [App Service Ortamı'na giriş][IntroToAppServiceEnvironment]

App Service ortamınıza gelen trafiği denetleme etrafında daha fazla bilgi için bkz [bir App Service ortamına gelen trafiği denetleme][ControlInboundASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md
[SiteToSite]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[ControlInboundASE]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
