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
ms.topic: tutorial
ms.date: 08/17/2016
ms.author: naziml
ms.custom: mvc
ms.openlocfilehash: bfe36ee5365e71db4280e8e2ccff6db8e552dd39
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Bir Web uygulaması Güvenlik Duvarı (WAF) için uygulama hizmeti ortamını yapılandırma
## <a name="overview"></a>Genel Bakış
Web uygulaması güvenlik duvarı ister [Barracuda WAF Azure](https://www.barracuda.com/programs/azure) üzerinde kullanılabilir [Azure Marketi](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) güvenli SQL eklemelerini, siteler arası komut dosyası, kötü amaçlı yazılım yüklemeleri engellemek için gelen web trafiği incelemek tarafından web uygulamalarınızın & uygulama DDoS ve diğer saldırılara yardımcı olur. Ayrıca arka uç web sunucularından yanıtları veri kaybını önleme (DLP) inceler. Yalıtım ve App Service ortamları tarafından sağlanan ek ölçeklendirme birlikte, bu kötü amaçlı istekleri ve yüksek hacimli trafik dayanacak gereken iş kritik web uygulamalarını barındırmasını ideal bir ortam sağlar.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Kurulum
Bu belge için biz Barracuda WAF birden çok yük dengeli örneğini arkasında uygulama hizmeti ortamı WAF yalnızca trafiğinden uygulama hizmeti ortamı erişebilir ve çevre ağından erişilebilir değil şekilde yapılandırın. Biz de Azure Traffic Manager Azure veri merkezlerinde ve bölgeler arasında yük dengelemesi için Barracuda WAF örnekler önünde vardır. Yüksek düzey bir diyagramını Kurulumu aşağıdaki görüntü gibi görünür:

![Mimari][Architecture] 

> [!NOTE]
> Girişiyle [ILB desteklemek için uygulama hizmeti ortamı](app-service-environment-with-internal-load-balancer.md), çevre ağından erişilemeyen ve yalnızca özel ağa kullanılabilir olması için ana yapılandırabilirsiniz. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Uygulama hizmeti ortamınızı yapılandırma
Bir uygulama hizmeti ortamı yapılandırmak için bkz [Belgelerimizi](app-service-web-how-to-create-an-app-service-environment.md) konu hakkında. Oluşturulan bir uygulama hizmeti ortamı olduktan sonra Web Apps, API uygulamaları oluşturabilirsiniz ve [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) Bu ortamdaki tüm biz yapılandırmak bir sonraki bölümde WAF arkasında korunur.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Barracuda WAF bulut hizmetini yapılandırma
Barracuda sahip bir [ayrıntılı makale](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) kendi WAF azure'da bir sanal makinede dağıtma. Ancak bu yönergeleri izleyerek, aynı bulut hizmetine en az iki WAF örneği VM dağıtmak istediğiniz artıklık istediğiniz ve tek hata noktası tanıtmak değil çünkü.

### <a name="adding-endpoints-to-cloud-service"></a>Bulut hizmeti için uç noktaları ekleme
2 veya daha fazla WAF VM bulut hizmetiniz örnekleri sonra kullanabileceğiniz [Azure portal](https://portal.azure.com/) aşağıdaki görüntüde gösterildiği gibi uygulamanız tarafından kullanılan HTTP ve HTTPS uç noktalarını eklemek için:

![Uç noktasını yapılandırma][ConfigureEndpoint]

Uygulamalarınız diğer uç noktaları kullanıyorsa, bu listeye eklediğinizden emin olun. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF kendi Yönetim Portalı üzerinden yapılandırma
Barracuda WAF TCP bağlantı noktası 8000 kendi Yönetim Portalı aracılığıyla yapılandırması için kullanır. WAF VM'ler birden çok örneği varsa, her bir VM örneği için adımlar burada tekrarlamanız gerekir. 

> [!NOTE]
> WAF yapılandırmayla tamamladıktan sonra TCP/8000 endpoint tüm WAF, WAF güvenli tutmak için Vm'leriniz kaldırın.
> 
> 

Yönetim uç nokta Barracuda WAF yapılandırmak için aşağıdaki görüntüde gösterildiği gibi ekleyin.

![Yönetim uç nokta ekleyin][AddManagementEndpoint]

Bulut hizmetinizi yönetim noktadaki gözatmak için bir tarayıcı kullanın. Bulut hizmetinizi test.cloudapp.net çağrılırsa, bu uç nokta için http://test.cloudapp.net:8000 göz atarak erişir. Oturum açma sayfası WAF VM kurulumu aşamasında belirtilen kimlik bilgilerini kullanarak oturum aşağıdaki görüntü gibi görmeniz gerekir.

![Yönetim oturum açma sayfası][ManagementLoginPage]

Oturum açtığında bir Pano WAF koruma hakkında temel istatistikleri gösterir aşağıdaki resimde bir gibi görmeniz gerekir.

![Yönetim Panosu][ManagementDashboard]

Tıklayarak **Hizmetleri** sekmesi, WAF koruma hizmetleri için yapılandırmanıza olanak sağlar. Barracuda WAF yapılandırma hakkında daha fazla ayrıntı için bkz: [kendi belgelerine](https://techlib.barracuda.com/waf/getstarted1). Aşağıdaki örnekte, HTTP ve HTTPS trafiğini hizmet veren bir Azure Web uygulaması yapılandırıldı.

![Yönetim Hizmetleri Ekle][ManagementAddServices]

> [!NOTE]
> Bir Web uygulaması için IP SSL Kurulum varsa uygulamalarınızı nasıl yapılandırılır ve hangi özelliklerin uygulama hizmeti ortamı'nda kullanıldığını bağlı olarak, trafiği için TCP bağlantı noktaları 80 ve 443, örneğin, dışındaki iletmek gerekir. App Service ortamlarda kullanılan ağ bağlantı noktalarının listesi için bkz: [denetim gelen trafiği belgelerine'nın](app-service-app-service-environment-control-inbound-traffic.md) ağ bağlantı noktaları bölümü.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Microsoft Azure trafik Yöneticisi (isteğe bağlı) yapılandırma
Yüklemek istediğiniz uygulamanız birden çok bölgede kullanılabilir ise arkasına Bakiye [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). Bunu yapmak için bir uç nokta olarak ekleyebilirsiniz [Azure portal](https://portal.azure.com) aşağıdaki görüntüde gösterildiği gibi WAF için bulut hizmeti adı trafik Yöneticisi profili kullanarak. 

![Trafik Yöneticisi uç noktası][TrafficManagerEndpoint]

Uygulamanızın kimlik doğrulaması gerektiriyorsa, herhangi bir kimlik doğrulaması ping işlemi yapmak için trafik Yöneticisi'için uygulamanızın kullanılabilirlik için gerektirmeyen bazı kaynak olduğundan emin olun. URL yapılandırabilirsiniz **yapılandırma** sayfasındaki [Azure portal](https://portal.azure.com) aşağıdaki görüntüde gösterildiği gibi:

![Traffic Manager'ı yapılandırma][ConfigureTrafficManager]

Trafik Yöneticisi ping uygulamanıza, WAF iletmek için aşağıdaki örnekte gösterildiği gibi uygulamanızın trafiği iletmek için Web sitesi çevirileri Barracuda WAF üzerinde ayarlamanız gerekir:

![Web sitesi çevirileri][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Ağ güvenlik grupları (NSG) kullanarak uygulama hizmeti ortamı için trafiğinin güvenliğini sağlama
İzleyin [denetim gelen trafiği belgelerine](app-service-app-service-environment-control-inbound-traffic.md) trafiği yalnızca bulut hizmetinizin VIP adresi kullanarak uygulama hizmeti ortamınızı WAF kısıtlama hakkında bilgi. Burada, TCP bağlantı noktası 80 bu görevi gerçekleştirmek için bir örnek Powershell komut verilmiştir.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

SourceAddressPrefix WAF'ın bulut hizmeti ile sanal IP adresi (VIP) değiştirin.

> [!NOTE]
> VIP bulut hizmeti değişikliklerinizi sildiğinizde ve bulut hizmeti yeniden oluşturun. Bunu yaptıktan sonra IP adresi ağ kaynak grubunda güncelleştirdiğinizden emin olun. 
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
