---
title: "Bir Web uygulaması Güvenlik Duvarı (WAF) için uygulama hizmeti ortamını yapılandırma"
description: "Uygulama hizmeti ortamınızı önünde bir web uygulaması güvenlik duvarı yapılandırmayı öğrenin."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 4c0e2d649f71d7797efbfe2c8e93ea0c844152df
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Bir Web uygulaması Güvenlik Duvarı (WAF) için uygulama hizmeti ortamını yapılandırma
## <a name="overview"></a>Genel Bakış
Web uygulaması güvenlik duvarı ister [Barracuda WAF Azure](https://www.barracuda.com/programs/azure) üzerinde kullanılabilir [Azure Marketi](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) güvenli SQL eklemelerini, siteler arası komut dosyası, kötü amaçlı yazılım yüklemeleri engellemek için gelen web trafiği incelemek tarafından web uygulamalarınızın & uygulama DDoS ve diğer saldırılara yardımcı olur. Ayrıca arka uç web sunucularından yanıtları veri kaybını önleme (DLP) inceler. Yalıtım ve App Service ortamları tarafından sağlanan ek ölçeklendirme birlikte, bu kötü amaçlı istekleri ve yüksek hacimli trafik dayanacak gereken iş kritik web uygulamalarını barındırmasını ideal bir ortam sağlar.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Kurulum
Biz yapılandırmak için bu belgenin böylece yalnızca WAF gelen trafiği uygulama hizmeti ortamı erişebilir ve çevre ağından erişilebilir olmaz Barracuda WAF örneklerini bizim uygulama hizmeti ortamı arkasında birden fazla yük dengeli. Biz de Azure Traffic Manager Azure veri merkezlerinde ve bölgeler arasında yük dengelemesi için bizim Barracuda WAF örnekler önünde sahip olur. Kurulum üst düzey bir diyagramını ne aşağıda gösterilen gibi görünür.

![Mimari][Architecture] 

> Not: girişiyle [ILB desteklemek için uygulama hizmeti ortamı](app-service-environment-with-internal-load-balancer.md), çevre ağından erişilemeyen ve yalnızca özel ağa kullanılabilir olması için ana yapılandırabilirsiniz. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Uygulama hizmeti ortamınızı yapılandırma
Bir uygulama hizmeti ortamı yapılandırmak için de başvurabilirsiniz [Belgelerimizi](app-service-web-how-to-create-an-app-service-environment.md) konu hakkında. Oluşturulan bir uygulama hizmeti ortamı olduktan sonra Web Apps, API uygulamaları oluşturabilirsiniz ve [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) Bu ortamdaki tüm biz yapılandırmak bir sonraki bölümde WAF arkasında korunur.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Barracuda WAF bulut hizmetini yapılandırma
Barracuda sahip bir [ayrıntılı makale](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) kendi WAF azure'da bir sanal makinede dağıtma. Ancak bu yönergeleri izleyerek, aynı bulut hizmetine en az 2 WAF örneği VM dağıtmak istediğiniz artıklık istediğiniz ve tek hata noktası tanıtmak değil çünkü.

### <a name="adding-endpoints-to-cloud-service"></a>Bulut hizmeti için uç noktaları ekleme
2 veya daha fazla WAF VM bulut hizmetiniz örnekleri sonra kullanabileceğiniz [Azure portal](https://portal.azure.com/) aşağıdaki resimde gösterildiği gibi uygulamanız tarafından kullanılan HTTP ve HTTPS uç noktalarını eklemek için.

![Uç noktasını yapılandırma][ConfigureEndpoint]

Uygulamalarınız diğer uç noktaları kullanıyorsa, bu da bu listeye eklemek emin olun. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF kendi Yönetim Portalı üzerinden yapılandırma
Barracuda WAF TCP bağlantı noktası 8000 kendi Yönetim Portalı aracılığıyla yapılandırması için kullanır. Biz WAF VM'ler birden çok örneğini olduğundan adımlar burada her bir VM örneği için yinelemeniz gerekecek. 

> Not: WAF yapılandırmayla tamamladıktan sonra tüm WAF, WAF güvenli tutmak için Vm'leriniz TCP/8000 endpoint kaldırın.
> 
> 

Yönetim uç nokta Barracuda WAF yapılandırmak için aşağıdaki resimde gösterildiği gibi ekleyin.

![Yönetim uç nokta ekleyin][AddManagementEndpoint]

Bulut hizmetinizi yönetim noktadaki gözatmak için bir tarayıcı kullanın. Bulut hizmetinizi test.cloudapp.net çağrılırsa, bu uç nokta için http://test.cloudapp.net:8000 göz atarak erişir. Bir oturum açma sayfası görmelisiniz aşağıdaki gibi WAF VM kurulumu aşamasında belirtilen kimlik bilgilerini kullanarak oturum açabilir.

![Yönetim oturum açma sayfası][ManagementLoginPage]

Bir kez, oturum açma WAF koruma hakkında temel istatistikleri sunacaktır aşağıdaki görüntü birinde olarak bir Pano görürsünüz.

![Yönetim Panosu][ManagementDashboard]

Hizmetler sekmesinde tıklatarak, sizin WAF koruduğu hizmetler için yapılandırma olanak tanır. Barracuda WAF yapılandırma hakkında daha fazla ayrıntı için başvurabilirsiniz [kendi belgelerine](https://techlib.barracuda.com/waf/getstarted1). Bir Azure Web uygulaması aşağıdaki örnekte HTTP ve HTTPS trafiğini hizmet veren yapılandırıldı.

![Yönetim Hizmetleri Ekle][ManagementAddServices]

> Not: uygulamalarınızı nasıl yapılandırılır ve hangi özelliklerin uygulama hizmeti ortamı'nda kullanıldığını bağlı olarak, örneğin, 80 ve 443 dışındaki bağlantı noktaları trafiği için TCP iletmek gerekir, bir Web uygulaması için IP SSL Kurulum varsa. Uygulama hizmeti ortamlarında kullanılan ağ bağlantı noktalarının listesi için lütfen bakın [denetim gelen trafiği belgelerine'nın](app-service-app-service-environment-control-inbound-traffic.md) ağ bağlantı noktaları bölümü.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Microsoft Azure trafik Yöneticisi (isteğe bağlı) yapılandırma
Yüklemek istediğiniz uygulamanız birden çok bölgede kullanılabilir ise arkasına Bakiye [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). Bir uç nokta olarak ekleyebilirsiniz Bunu yapmak için [Klasik Azure portalı](https://manage.azure.com) aşağıdaki resimde gösterildiği gibi WAF için bulut hizmeti adı trafik Yöneticisi profili kullanarak. 

![Trafik Yöneticisi uç noktası][TrafficManagerEndpoint]

Uygulamanızın kimlik doğrulaması gerektiriyorsa, herhangi bir kimlik doğrulaması ping işlemi yapmak için trafik Yöneticisi'için uygulamanızın kullanılabilirlik için gerektirmeyen bazı kaynak olduğundan emin olun. URL Yapılandır bölümünde yapılandırabilirsiniz [Klasik Azure portalı](https://manage.azure.com) aşağıda gösterildiği gibi.

![Traffic Manager'ı yapılandırma][ConfigureTrafficManager]

Trafik Yöneticisi ping uygulamanıza, WAF iletmek için Kurulum Web sitesi Çevirileri, Barracuda WAF aşağıdaki örnekte gösterildiği gibi uygulamanız için trafiği iletmek gerekir.

![Web sitesi çevirileri][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Ağ güvenlik grupları (NSG) kullanarak uygulama hizmeti ortamı için trafiğinin güvenliğini sağlama
İzleyin [denetim gelen trafiği belgelerine](app-service-app-service-environment-control-inbound-traffic.md) trafiği yalnızca bulut hizmetinizin VIP adresi kullanarak uygulama hizmeti ortamınızı WAF kısıtlama hakkında bilgi. Burada, TCP bağlantı noktası 80 bu görevi gerçekleştirmek için bir örnek Powershell komut verilmiştir.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

SourceAddressPrefix WAF'ın bulut hizmeti ile sanal IP adresi (VIP) değiştirin.

> Not: silin ve bulut hizmeti yeniden oluştururken, bulut hizmetinizin VIP değiştirin. Bunu yaptıktan sonra IP adresi ağ kaynak grubunda güncelleştirdiğinizden emin olun. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
