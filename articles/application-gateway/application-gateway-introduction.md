---
title: "Azure Application Gateway’e giriş | Microsoft Docs"
description: "Bu sayfada ağ geçidi boyutları, HTTP yük dengelemesi, tanımlama bilgilerine dayalı oturum benzeşimi ve SSL yük boşaltma dahil olmak üzere 7. katman yük dengeleme için Application Gateway’e genel bakış sunulmaktadır."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: c8f54c81481ebced16ed259f07736b0a4bc80567
ms.contentlocale: tr-tr
ms.lasthandoff: 08/31/2017

---
# <a name="overview-of-application-gateway"></a>Application Gateway'e genel bakış

Microsoft Azure Application Gateway, uygulama teslim denetleyicisini (ADC) hizmet olarak sunan özel bir sanal gereçtir. Uygulamanız için çeşitli 7. katman yük dengeleme özellikleri sunar. Müşterilere, yoğun CPU kullanan SSL sonlandırması yükünü uygulama ağ geçidine boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Ayrıca, gelen trafiğin “hepsini bir kez deneme” yaklaşımıyla dağıtımı, tanımlama bilgisi tabanlı oturum benzeşimi, URL’yi yol tabanlı yönlendirme ve tek bir uygulama ağ geçidi arkasında birden fazla web sitesi barındırma gibi diğer 7. katman yönlendirme özelliklerini sağlar. Application gateway WAF SKU’su kapsamında bir web uygulaması güvenlik duvarı da (WAF) sağlanır. WAF, web uygulamaları için yaygın web güvenlik açıklarına ve açıklardan yararlanmaya karşı koruma sağlar. Application Gateway; İnternet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya bu ikisinin bir birleşimi olarak yapılandırılabilir. 

![senaryo](./media/application-gateway-introduction/scenario.png)

## <a name="features"></a>Özellikler

Application Gateway şu anda aşağıdaki özellikleri sunmaktadır:


* **[Web uygulaması güvenlik duvarı](application-gateway-webapplicationfirewall-overview.md)** - Azure Application Gateway içindeki web uygulaması güvenlik duvarı (WAF), web uygulamalarını SQL eklemesi, siteler arası komut dosyası saldırıları ve oturum ele geçirmeleri gibi yaygın web tabanlı saldırılardan korur.
* **HTTP yük dengelemesi** - Application Gateway hepsini bir kez deneme yük dengelemesi sağlar. Yük dengelemesi 7. Katmanda yapılır ve yalnızca HTTP(S) trafiği için kullanılır.
* **Tanımlama bilgilerine dayalı oturum benzeşimi** - Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı arka uçta tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı arka uca yönlendirebilir. Bu özellik, oturum durumunun bir kullanıcı oturumuna ait arka uca yerel olarak kaydedildiği durumlarda önemlidir.
* **[Güvenli Yuva Katmanı (SSL) yük boşaltması](application-gateway-ssl-arm.md)** - Bu özellik, web sunucularınızın HTTPS trafiğinin şifresini çözmeyi içeren maliyetli bir görevdir. Application Gateway üzerinde SSL bağlantısını sonlandırarak ve isteği sunucuya şifrelenmemiş olarak ileterek, web sunucusu üzerindeki şifre çözme yükü kaldırılır.  Application Gateway, yanıtı istemciye geri göndermeden önce yeniden şifreler. Bu özellik, arka ucun Azure’da Application Gateway ile aynı güvenli sanal ağda bulunduğu senaryolarda yararlıdır.
* **[Uçtan Uca SSL](application-gateway-backend-ssl.md)** - Application Gateway, trafiğin uçtan uca şifrelenmesini destekler. Application Gateway bu işlemi uygulama ağ geçidindeki SSL bağlantısını sonlandırarak yapar. Ağ geçidi bundan sonra yönlendirme kurallarını trafiğe uygular, paketi yeniden şifreler ve tanımlanan yönlendirme kurallarına göre paketi uygun arka uca iletir. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler.
* **[URL tabanlı içerik yönlendirme](application-gateway-url-route-overview.md)** - Bu özellik farklı trafikler için farklı arka uç sunucularını kullanma özelliği sağlar. Web sunucusu üzerindeki bir klasörün veya bir CDN’nin trafiği farklı bir arka uca yönlendirilebilir. Bu özellik belirli içeriklere hizmet etmeyen arka uçlardaki gereksiz yükü azaltır.
* **[Çok siteli yönlendirme](application-gateway-multi-site-overview.md)** - Application Gateway, tek bir uygulama ağ geçidi üzerinde 20’ye kadar web sitesini birleştirmenize olanak tanır.
* **[Websocket desteği](application-gateway-websocket.md)** - Application Gateway’in bir diğer harika özelliği ise Websocket’e yönelik yerel desteğidir.
* **[Sistem durumu izleme](application-gateway-probe-overview.md)** - Application Gateway, daha ayrıntılı senaryoları izlemek üzere arka uç kaynaklarına ve özel araştırmalara yönelik varsayılan sistem durumu izleme özelliğini sağlar.
* **[SSL İlkesi ve Şifreler](application-gateway-ssl-policy-overview.md)** - Bu özellik, SSL protokolü sürümlerini sınırlama imkanı sağlar ve desteklenen şifre paketleri ile bunların işlenme sırasını şifreler.
* **[İstek yönlendirme](application-gateway-redirect-overview.md)** - Bu özellik HTTP isteklerini HTTPS dinleyicisine yönlendirebilmeyi sağlar.
* **[Çok kiracılı arka uç desteği](application-gateway-web-app-overview.md)**  - Application gateway, Azure Web Apps ve API Ağ Geçidi gibi çok kiracılı arka uç hizmetlerini arka uç havuz üyeleri olarak yapılandırmayı destekler. 
* **[Gelişmiş tanılama](application-gateway-diagnostics.md)** - Application gateway tam tanılama ve erişim günlükleri sağlar. Güvenlik duvarı günlükleri, WAF’nin etkin olduğu application gateway kaynakları için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Application Gateway aşağıdakiler için yararlıdır:

* Aynı kullanıcı/istemci oturumunun aynı arka uç sanal makinesine ulaşmaya yönelik isteklerini gerektiren uygulamalar. Bu uygulamaların örnekleri alışveriş sepeti uygulamaları ve web posta sunucularıdır.
* Web sunucusu grupları için SSL sonlandırma yükünü kaldırma.
* Aynı uzun süreli TCP bağlantısı üzerinde birden fazla HTTP isteğinin yönlendirilmesini veya farklı arka uç sunucularına yük dengelemesi yapılmasını gerektiren, içerik teslim ağı gibi uygulamalar.
* Websocket trafiğini destekleyen uygulamalar
* Web uygulamalarını SQL ekleme, siteler arası komut dosyası saldırıları ve oturum ele geçirmeleri gibi yaygın web tabanlı saldırılardan koruma.
* Trafiğin url yolu veya etki alanı üst bilgileri gibi farklı yönlendirme ölçütlerine göre mantıksal dağıtımı.

Application Gateway tamamen Azure tarafından yönetilir, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar. Uygulama ağ geçidi oluşturduğunuzda, bir uç nokta (ortak VIP veya dahili ILB IP), giriş ağ trafiği için ilişkilendirilir ve kullanılır. Bu VIP veya ILB IP, taşıma düzeyinde (TCP/UDP) çalışan ve tüm gelen ağ trafiğinin yükünü uygulama ağ geçidinin çalışan örneklerinde dengeleyen Azure Load Balancer tarafından sağlanır. Ardından uygulama ağ geçidi HTTP/HTTPS trafiğini, yapılandırmasına göre (sanal makine, bulut hizmeti, iç veya dış IP adresi) yönlendirir.

Azure tarafından yönetilen bir hizmet olan Application Gateway yük dengelemesi, Azure yazılım yük dengeleyicisinin arkasında 7. katman yük dengeleyici sağlamaya olanak tanır. Aşağıdaki görüntüde gösterildiği gibi senaryoyu tamamlamak için Traffic Manager kullanılabilir; burada Traffic Manager, trafiğin farklı gruplardaki birden fazla Application Gateway kaynağına yönlendirilmesini ve bu kaynaklar tarafından kullanılmasını sağlarken, Application Gateway bölgeler arası 7. katman yük dengeleme sağlar. Bu senaryonun bir örneği şurada bulunabilir: [Azure bulutunda yük dengeleme hizmetlerini kullanma](../traffic-manager/traffic-manager-load-balancing-azure.md)

![traffic manager ve application gateway senaryosu](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Ağ geçidi boyutları ve örnekleri

Application Gateway şu anda üç büyüklükte sunulmaktadır: **Kısa**, **Orta** ve **Uzun**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

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

Uygulama ağ geçidi, uç noktası için bir genel IP, özel IP veya yapılandırıldığında her ikisine birden sahip olabilir. Application Gateway, kendi alt ağındaki bir sanal ağ içinde yapılandırılır. Uygulama ağ geçidi için oluşturulan veya kullanılan alt ağ başka türde kaynaklar içeremez; alt ağda kaynak olarak yalnızca diğer uygulama ağ geçitleri kullanılabilir. Arka uç kaynaklarınızın güvenliğini sağlamak için, arka uç sunucuları uygulama ağ geçidiyle aynı sanal ağdaki farklı bir alt ağ içinde yer alabilir. Bu alt ağ, arka uç uygulamaları için gerekli değildir. Application gateway ip adresine ulaşabildiği sürece arka uç sunucuları için ADC özellikleri sağlayabilir. 

REST API’leri, PowerShell cmdlet’leri, Azure CLI veya [Azure portalını](https://portal.azure.com/) kullanarak bir uygulama ağ geçidi oluşturup yönetebilirsiniz. Application gateway hakkında diğer sorular için, [Application Gateway SSS](application-gateway-faq.md) bölümünü ziyaret ederek sık sorulan soruların listesini görüntüleyin.

## <a name="pricing"></a>Fiyatlandırma

Fiyatlandırma, saatlik ağ geçidi örneği ücretine ve veri işleme ücretine bağlıdır. WAF SKU’su için saat başına ağ geçidi fiyatlandırması, Standart SKU ücretlerinden farklıdır. Bu fiyatlandırma bilgileri [Application Gateway fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/application-gateway/) sayfasında bulunabilir. Veri işleme ücretleri aynı kalır.

## <a name="faq"></a>SSS

Application Gateway hakkında sık sorulan sorular için bkz. [Application Gateway hakkında SSS](application-gateway-faq.md).

## <a name="next-steps"></a>Sonraki adımlar

Application Gateway hakkında bilgi aldıktan sonra [bir uygulama ağ geçidi oluşturabilir](application-gateway-create-gateway-portal.md) veya HTTPS bağlantılarının yük dengelemesini yapmak üzere [bir uygulama ağ geçidi SSL yük boşaltması oluşturabilirsiniz](application-gateway-ssl-arm.md).

URL tabanlı içerik yönlendirmeyi kullanarak bir uygulama ağ geçidi oluşturma hakkında daha fazla bilgi almak için [URL tabanlı yönlendirme kullanarak uygulama ağ geçidi oluşturma](application-gateway-create-url-route-arm-ps.md) bölümüne gidin.

Azure'un diğer önemli ağ özelliklerinden bazıları hakkında bilgi edinmek için bkz. [Azure Ağı](../networking/networking-overview.md).

