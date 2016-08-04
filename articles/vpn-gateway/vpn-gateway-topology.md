<properties 
   pageTitle="VPN Gateway bağlantı topolojileri | Microsoft Azure"
   description="VPN Gateway bağlantı topolojilerini ve kullanılabilir yapılandırma araçları ile dağıtım modellerini görüntüleyin."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/18/2016"
   ms.author="cherylmc" />

# Azure VPN Gateway bağlantı topolojileri

Bu makalede temel VPN ağ geçidi bağlantı topolojileri gösterilmektedir. Gereksinimlerinize uygun yapılandırma topolojisini seçmenize yardımcı olmak üzere grafikleri ve açıklamaları kullanabilirsiniz. Bu makale başlıca temel topolojileri içerse de diyagramları kılavuz olarak kullanarak daha karmaşık topolojiler oluşturmak mümkündür.

Her topoloji, topolojinin kullanılabilir olduğu dağıtım modelini, her bir topolojiyi yapılandırmak için kullanabileceğiniz dağıtım araçlarını içerir ve bir makale varsa doğrudan makaleye bağlanır. Tablolar yeni makaleler ve dağıtım araçları kullanımınıza sunuldukça sık sık güncelleştirilmektedir.

VPN ağ geçitleri hakkında daha fazla bilgi istiyorsanız bkz. [VPN Gateways Hakkında](vpn-gateway-about-vpngateways.md).



## Siteden Siteye ve Çok Siteli

Siteden Siteye bağlantı IPSec/IKE (IKEv1 veya Ikev2) VPN tüneli yoluyla yapılan bir bağlantıdır. Bu bağlantı türü şirket içi bir VPN cihazı ya da Windows Server RRAS gerektirir. Siteden Siteye bağlantılar, şirket içi ve dışı karışık yapılandırmalar ve karma yapılandırmalar için kullanılabilir.   


**S2S diyagramı**

![S2S bağlantısı](./media/vpn-gateway-topology/site2site.png "site-to-site")

Azure Yol Temelli VPN’ler kullanıyorsanız şirket içi ağlarınız ile birden fazla S2S VPN bağlantısı oluşturup yapılandırabilirsiniz. Bu yapılandırma türü genellikle "çok siteli" bağlantı olarak adlandırılır.
 

**Çok Siteli diyagramı**

![Çok Siteli bağlantı](./media/vpn-gateway-topology/multisite.png "multi-site")


**Kullanılabilir dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## Sanal Ağdan Sanal Ağa

Bir sanal ağı başka bir sanal ağa bağlamak (Sanal Ağdan Sanal Ağa), bir sanal ağı şirket içi site konumuna bağlamakla neredeyse aynıdır. Her iki bağlantı türü de Azure VPN ağ geçidini kullanarak IPsec/IKE ile güvenli bir tünel sunar. Bağlandığınız Sanal Ağlar farklı bölgelerde veya farklı aboneliklerde olabilir. Hatta Sanal Ağdan Sanal Ağa iletişimini çok siteli yapılandırmalarla bile birleştirebilirsiniz. Bu özellik şirket içi ve şirket dışı bağlantıyla ağ içi bağlantıyı birleştiren ağ topolojileri kurabilmenize olanak sağlar.

Azure şu anda iki dağıtım modeline sahiptir: Azure Service Management (klasik olarak adlandırılır) ve Azure Resource Manager. Bir süredir Azure kullanıyorsanız büyük olasılıkla klasik bir VNet üzerinde çalışan Azure sanal makineleriniz ve örnek rolleriniz vardır; diğer yandan daha yeni sanal makineler ve rol örnekleri ARM’de oluşturulan bir VNet üzerinde çalışıyor olabilir. Farklı dağıtım modellerinde olsalar bile sanal ağlar arasında bir bağlantı oluşturabilirsiniz.


**VNet2VNet şeması**

![Sanal Ağdan Sanal Ağa bağlantı](./media/vpn-gateway-topology/vnet2vnet.png "vnet-to-vnet")


**Kullanılabilir dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 



## Siteden Siteye ve ExpressRoute eşzamanlı bağlantıları

ExpressRoute genel İnternet üzerinden değil WAN bağlantınızdan Azure dahil olmak üzere Microsoft Hizmetlerine doğrudan, özel olarak yapılan bir bağlantıdır. Siteden Siteye VPN trafiği genel İnternet üzerinden şifrelenmiş olarak hareket eder. Aynı sanal ağ için Siteden Siteye VPN ve ExpressRoute bağlantılarını yapılandırma yeteneğine sahip olmanın çeşitli avantajları vardır. ExressRoute için güvenli bir yük devretme yolu olarak Siteden Siteye VPN yapılandırabilir veya ağınızın parçası olmayıp ExpressRoute aracılığıyla bağlanan sitelere bağlanmak için Siteden Siteye VPN’ler kullanabilirsiniz. 


**Eşzamanlı bağlantılar diyagramı**

![Eşzamanlı bağlantı](./media/vpn-gateway-topology/expressroutes2s.png "expressroute-site2site")


**Kullanılabilir dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## Noktadan Siteye

Noktadan Siteye yapılandırma, bir istemci bilgisayardan sanal ağınıza tek başına güvenli bir bağlantı oluşturmanıza olanak sağlar. VPN bağlantısı, bağlantının istemci bilgisayardan başlatılmasıyla oluşturulur. Sanal ağınıza uzak bir konumdan (örneğin, evden veya bir konferanstan) bağlanmak istediğinizde ya da sanal bir ağa bağlanması gereken yalnızca birkaç istemciniz bulunduğunda bu ideal bir çözümdür. 

Noktadan Siteye bağlantı SSTP (Güvenli Yuva Tünel Protokolü) üzerinden yapılan bir VPN bağlantısıdır. Noktadan Siteye bağlantıların çalışması için bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. 

**P2S diyagramı**

![Noktadan siteye bağlantılar](./media/vpn-gateway-topology/point2site.png "point-to-site")

**Kullanılabilir dağıtım modelleri ve yöntemleri**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## Sonraki adımlar

Bağlantınızı planlamaya ve tasarlamaya geçmeden önce VPN ağ geçitlerini daha iyi anlamak için [VPN Gateways Hakkında](vpn-gateway-about-vpngateways.md) ve [VPN Gateway SSS](vpn-gateway-vpn-faq.md) makalelerindeki maddeleri daha iyi öğrenmeniz gerekir.





 



<!----HONumber=Jun16_HO2-->


