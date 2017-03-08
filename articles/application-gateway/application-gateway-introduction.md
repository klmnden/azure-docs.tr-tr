---
title: "Application Gateway’e giriş | Microsoft Belgeleri"
description: "Bu sayfada ağ geçidi boyutları, HTTP yük dengelemesi, tanımlama bilgilerine dayalı oturum benzeşimi ve SSL yük boşaltma dahil olmak üzere 7. katman yük dengeleme için Application Gateway’e genel bakış sunulmaktadır."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 119275f335344858cd20b6a17ef87e3ef32b6e12
ms.openlocfilehash: 15db7dad6b83f6df3891aea60b308f2cf6008dd9
ms.lasthandoff: 02/28/2017


---
# <a name="overview-of-application-gateway"></a>Application Gateway'e genel bakış

## <a name="what-is-application-gateway"></a>Application Gateway Nedir?

Microsoft Azure Application Gateway, uygulamanız için çeşitli 7. katman yük dengeleme özellikleri sağlayan Application Delivery Controller'ı (ADC) bir hizmet olarak sunar. Müşterilere, yoğun CPU kullanan SSL sonlandırması yükünü Application Gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Ayrıca, gelen trafiğin dönüşümlü dağıtımı, tanımlama bilgisi tabanlı oturum benzeşimi, URL yolu tabanlı yönlendirme ve tek bir Application Gateway arkasında birden fazla web sitesi barındırma da dahil olmak üzere diğer 7. Katman yönlendirme özelliklerini sağlar. Application Gateway, uygulamanızı OWASP tarafından sunulan en yaygın 10 web güvenlik açığının çoğuna karşı koruyan bir web uygulaması güvenlik duvarına (WAF) da sahiptir. Application Gateway; İnternet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya bu ikisinin bir birleşimi olarak yapılandırılabilir. Application Gateway tamamen Azure tarafından yönetilir, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar. Application Gateway, sanal makineler, bulut hizmetleri ve dahili veya harici web uygulamalarıyla çalışır.

Application Gateway, uygulamanız için adanmış bir sanal gereç olup ölçeklenebilirlik ve yüksek kullanılabilirlik sağlamak üzere birden fazla çalışan örneğinden oluşur. Uygulama ağ geçidi oluşturduğunuzda, bir uç nokta (ortak VIP veya dahili ILB IP), giriş ağ trafiği için ilişkilendirilir ve kullanılır. Bu VIP veya ILB IP, aktarım düzeyinde (TCP/UDP) çalışan ve tüm gelen ağ trafiğinin yükünü Application Gateway'in çalışan örneklerinde dengeleyen Azure Load Balancer tarafından sağlanır. Ardından Application Gateway, HTTP/HTTPS trafiğini, yapılandırmasına göre (sanal makine, bulut hizmeti, iç veya dış IP adresi) yönlendirir. SLA ve fiyatlandırma için [SLA](https://azure.microsoft.com/support/legal/sla/) ve [Fiyatlandırma](https://azure.microsoft.com/pricing/details/application-gateway/) sayfalarına bakın.

## <a name="features"></a>Özellikler

Application Gateway şu anda aşağıdaki özelliklerle birlikte 7. katman uygulama teslimini destekler:

* **[Web Uygulaması Güvenlik Duvarı (Önizleme)](application-gateway-webapplicationfirewall-overview.md)** - Azure Application Gateway içindeki web uygulaması güvenlik duvarı (WAF), web uygulamalarını SQL eklemesi, siteler arası komut dosyası saldırıları ve oturum ele geçirmeleri gibi yaygın web tabanlı saldırılardan korur.
* **HTTP yük dengelemesi** - Application Gateway hepsini bir kez deneme yük dengelemesi sağlar. Yük dengelemesi 7. Katmanda yapılır ve yalnızca HTTP(S) trafiği için kullanılır.
* **Tanımlama bilgilerine dayalı oturum benzeşimi** - Bu özellik, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı arka uca yönlendirebilir. Bu özellik, oturum durumunun bir kullanıcı oturumuna ait arka uca yerel olarak kaydedildiği durumlarda önemlidir.
* **[Güvenli Yuva Katmanı (SSL) yük boşaltması](application-gateway-ssl-arm.md)** - Bu özellik, web sunucularınızın HTTPS trafiğinin şifresini çözmeyi içeren maliyetli bir görevdir. Application Gateway üzerinde SSL bağlantısını sonlandırarak ve isteği sunucuya şifrelenmemiş olarak ileterek, web sunucusu üzerindeki şifre çözme yükü kaldırılır.  Application Gateway, yanıtı istemciye geri göndermeden önce yeniden şifreler. Bu özellik, arka ucun Azure’da Application Gateway ile aynı güvenli sanal ağda bulunduğu senaryolarda yararlıdır.
* **[Uçtan Uca SSL](application-gateway-backend-ssl.md)** - Application Gateway, trafiğin uçtan uca şifrelenmesini destekler. Application Gateway bu işlemi uygulama ağ geçidindeki SSL bağlantısını sonlandırarak yapar. Ağ geçidi bundan sonra yönlendirme kurallarını trafiğe uygular, paketi yeniden şifreler ve tanımlanan yönlendirme kurallarına göre paketi uygun arka uca iletir. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler.
* **[URL tabanlı içerik yönlendirme](application-gateway-url-route-overview.md)** - Bu özellik farklı trafikler için farklı arka uç sunucularını kullanma özelliği sağlar. Web sunucusundaki bir klasörün veya bir CDN'nin trafiği farklı bir arka uca yönlendirilerek belirli bir içeriğe sahip olmayan arka uçlarda gerekli olmayan yükler azaltılabilir.
* **[Çok siteli yönlendirme](application-gateway-multi-site-overview.md)** - Application Gateway, tek bir uygulama ağ geçidi üzerinde 20’ye kadar web sitesini birleştirmenize olanak tanır.
* **[Websocket desteği](application-gateway-websocket.md)** - Application Gateway’in bir diğer harika özelliği ise Websocket’e yönelik yerel desteğidir.
* **[Sistem durumu izleme](application-gateway-probe-overview.md)** - Application Gateway, daha ayrıntılı senaryoları izlemek üzere arka uç kaynaklarına ve özel araştırmalara yönelik varsayılan sistem durumu izleme özelliğini sağlar.
* **[Gelişmiş tanılama](application-gateway-diagnostics.md)** - Application gateway tam tanılama ve erişim günlükleri sağlar. Güvenlik duvarı günlükleri, WAF’nin etkin olduğu application gateway kaynakları için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Application Gateway aşağıdakiler için yararlıdır:

* Aynı kullanıcı/istemci oturumunun aynı arka uç sanal makinesine ulaşmaya yönelik isteklerini gerektiren uygulamalar. Bu uygulamaların örnekleri alışveriş sepeti uygulamaları ve web posta sunucularıdır.
* Web sunucusu gruplarından SSL sonlandırma yükünü kaldırmak isteyen uygulamalar.
* Aynı uzun süreli TCP bağlantısı üzerinde birden fazla HTTP isteğinin yönlendirilmesini veya farklı arka uç sunucularına yük dengelemesi yapılmasını gerektiren, içerik teslim ağı gibi uygulamalar.
* Websocket trafiğini destekleyen uygulamalar
* Web uygulamalarını SQL ekleme, siteler arası komut dosyası saldırıları ve oturum ele geçirmeleri gibi yaygın web tabanlı saldırılardan koruma.

Azure tarafından yönetilen bir hizmet olan Application Gateway yük dengelemesi, Azure yazılım yük dengeleyicisinin arkasında 7. katman yük dengeleyici sağlamaya olanak tanır. Aşağıdaki görüntüde gösterildiği gibi senaryoyu tamamlamak için Traffic Manager kullanılabilir; burada Traffic Manager, trafiğin farklı gruplardaki birden fazla Application Gateway kaynağına yönlendirilmesini ve bu kaynaklar tarafından kullanılmasını sağlarken, Application Gateway bölgeler arası 7. katman yük dengeleme sağlar. Bu senaryonun bir örneği şurada bulunabilir: [Azure bulutunda yük dengeleme hizmetlerini kullanma](../traffic-manager/traffic-manager-load-balancing-azure.md)

![traffic manager ve application gateway senaryosu](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Ağ geçidi boyutları ve örnekleri

Application Gateway şu anda üç büyüklükte sunulmaktadır: **Kısa**, **Orta** ve **Uzun**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Application Gateway için şu anda iki SKU sunulmaktadır: **WAF** ve **Standart**.

Bir abonelik için en fazla 50 uygulama ağ geçidi oluşturabilirsiniz ve her uygulama ağ geçidi en fazla 10 örnek içerebilir. Her uygulama ağ geçidi 20 http dinleyicisinden oluşabilir. Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Aşağıdaki tabloda, SSL boşaltmasının etkin olduğu her bir Application Gateway örneği için ortalama performans aktarım hızı gösterilmiştir:

| Arka uç sayfa yanıtı | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6K |7,5 Mbps |13 Mbps |50 Mbps |
| 100K |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

## <a name="health-monitoring"></a>Sistem durumunu izleme

Azure Application Gateway, temel veya özel sistem durumu araştırmaları aracılığıyla arka uç örneklerinin sistem durumunu otomatik olarak izler. Sistem durumu araştırmalarını kullanan bu işlem yalnızca sağlıklı konakların trafiğe yanıt vermesini sağlar. Daha fazla bilgi için bkz. [Application Gateway sistem durumunu izlemeye genel bakış](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Yapılandırma ve yönetme

Uygulama ağ geçidi, uç noktası için bir genel IP, özel IP veya yapılandırıldığında her ikisine birden sahip olabilir. Application Gateway, kendi alt ağındaki bir sanal ağ içinde yapılandırılır. Uygulama ağ geçidi için oluşturulan veya kullanılan alt ağ başka türde kaynaklar içeremez; alt ağda kaynak olarak yalnızca diğer uygulama ağ geçitleri kullanılabilir. Arka uç kaynaklarınızın güvenliğini sağlamak için, arka uç sunucuları uygulama ağ geçidiyle aynı sanal ağdaki farklı bir alt ağ içinde yer alabilir. Bu ek alt ağ, arka uç uygulamaları için gerekli değildir; uygulama ağ geçidi ip adresine ulaşabildiği sürece arka uç sunucuları için ADC özellikleri sağlayabilir.

REST API’leri, PowerShell cmdlet’leri, Azure CLI veya [Azure portalını](https://portal.azure.com/) kullanarak bir uygulama ağ geçidi oluşturup yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway hakkında bilgi aldıktan sonra [bir uygulama ağ geçidi oluşturabilir](application-gateway-create-gateway-portal.md) veya HTTPS bağlantılarının yük dengelemesini yapmak üzere [bir uygulama ağ geçidi SSL yük boşaltması oluşturabilirsiniz](application-gateway-ssl-arm.md).

URL tabanlı içerik yönlendirmeyi kullanarak bir uygulama ağ geçidi oluşturma hakkında daha fazla bilgi almak için [URL tabanlı yönlendirme kullanarak uygulama ağ geçidi oluşturma](application-gateway-create-url-route-arm-ps.md) bölümüne gidin.

