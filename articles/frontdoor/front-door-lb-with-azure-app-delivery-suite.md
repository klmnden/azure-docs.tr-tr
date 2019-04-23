---
title: Azure ön kapısı hizmeti - Azure'nın uygulama teslim suite ile Yük Dengeleme | Microsoft Docs
description: Bu makale Azure hakkında uygulama teslim suite ile yük dengelemeyi önerir öğrenmenize yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 3d5c0ac068a6644f3499da6c3b642a4a04408370
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790333"
---
# <a name="load-balancing-with-azures-application-delivery-suite"></a>Azure uygulama teslim paketiyle yük dengeleme

## <a name="introduction"></a>Giriş
Microsoft Azure, birden çok ağ trafiğinizi nasıl dağıtıldığını yönetmek için küresel ve bölgesel hizmetler ve Yük Dengelemesi sağlar: Traffic Manager, ön kapısı hizmeti, uygulama ağ geçidi ve yük dengeleyici.  Azure'nın çok sayıda bölge ile birlikte ve bölgesel hizmetlerin birlikte kullanarak mimarisi, sağlam, ölçeklenebilir, yüksek performanslı uygulamalar oluşturmak sağlar.

![Uygulama teslim paketi ][1]
 
Bu hizmetler, iki kategoriye ayrılır:
1. **Genel Yük Dengeleme hizmetlerini** Traffic Manager ve ön kapısı dağıtım son kullanıcılarınızdan gelen trafik, bölgesel arka uçlar arasında bulutlarda veya hibrit bile şirket içi hizmetler gibi. Genel Yük Dengeleme trafiğinizi en yakın hizmet arka ucunuza yönlendirir ve değişiklikler, hizmet güvenilirliğini veya kullanıcılarınız için her zaman açık, düzeyde performansını korumak için performans tepki verir. 
2. **Bölgesel Yük Dengeleme hizmetlerini** trafiği sanal ağları (Vnet) içinde sanal makineler (VM) veya bir bölgedeki bölgesel hizmet uç noktaları arasında dağıtma kabiliyeti gibi standart yük dengeleyici veya Application Gateway sağlayın.

Küresel ve bölgesel Services uygulamanızdaki bir uçtan uca güvenilir, yüksek performanslı ve güvenli yolu trafiği yönlendirmek için ve kullanıcılarınızın Iaas, PaaS veya şirket içi hizmetler sağlar. Sonraki bölümde, biz bu hizmetlerin her biri açıklanmaktadır.

## <a name="global-load-balancing"></a>Genel Yük Dengeleme
**Traffic Manager** Genel DNS Yük Dengeleme sağlar. Bu, gelen DNS istekleri arar ve müşterinin seçtiği yönlendirme ilkesine uygun olarak sağlıklı bir arka uç ile yanıt verir. Yönlendirme yöntemleri için Seçenekler şunlardır:
- Performans, gecikme süresi açısından en yakın olan bir arka uca istek sahibine göndermek için yönlendirme.
- Öncelik oluşturan bir arka uç arka olarak diğer arka uçları ile tüm trafiği yönlendirmek için yönlendirme.
- Ağırlıklı yönlendirme, her arka uç için atanan ağırlığı göre trafiği dağıtan hepsini.
- İstek sahipleri belirli coğrafi bölgede bulunan bu bölgelere eşlenen arka uçları yönlendirildiği emin olmak için coğrafi yönlendirme (örneğin, İspanya gelen tüm isteklerin Fransa Orta Azure bölgesiyle yönlendirileceği)
- Alt ağ IP adresini eşleştirmek izin veren yönlendirme aralıkları için arka uçlar, böylece bu gelen istekleri belirtilen arka ucuna gönderilecek (örneğin, Kurumsal HQ'ın IP adresi aralığından bağlanan tüm kullanıcıları genel değerinden farklı bir web içeriği almanız gerekir kullanıcılar için)

İstemci, doğrudan o arka ucuna bağlanır. Azure Traffic Manager, bir arka uç kötü durumda ve ardından istemcilerin sağlıklı başka bir örneğine yeniden yönlendirir algılar. Başvurmak [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) service hakkında daha fazla bilgi edinmek için belgeleri.

**Azure ön kapısı hizmet** genel HTTP Yük Dengeleme gibi dinamik Web sitesi Hızlandırma (DSA) sağlar.  Gelen HTTP isteklerinin en yakın hizmet arka uç yolları görünüyor / bölge için belirtilen ana bilgisayar adı, URL yolu ve yapılandırılmış kuralları.  
Ön kapısı, Microsoft'un ağının HTTP isteklerinin sonlandırır ve uygulamanın veya altyapının sistem durumu veya gecikme süresi değişikliklerini algılamak için etkin bir şekilde araştırmaları.  Ön kapısı sonra her zaman trafiği en hızlı ve kullanılabilir (sağlıklı) arka uç için yönlendirir. Ön kapısı'nın başvurmak [yönlendirme mimarisi](front-door-routing-architecture.md) ayrıntıları ve [trafik yönlendirme yöntemlerini](front-door-routing-methods.md) hizmeti hakkında daha fazla bilgi için.

## <a name="regional-load-balancing"></a>Bölgesel Yük Dengeleme
Application Gateway, çeşitli 7. Katman Yük Dengeleme Özellikleri uygulamanız için sunan bir hizmet olarak uygulama teslim denetleyicisi (ADC) sunar. Bu, müşterilere CPU yoğunluklu SSL sonlandırması yükünü application gateway'e boşaltarak web grubu üretkenliğini iyileştirme olanağı tanır. Hepsini bir kez deneme dağıtımını gelen trafiği, tanımlama bilgilerine dayalı oturum benzeşimi, URL yolu tabanlı Yönlendirme ve tek bir uygulama ağ geçidinin arkasında birden fazla Web sitesi barındırma olanağı diğer 7. Katman yönlendirme özelliklerini içerir. Uygulama ağ geçidi, bir Internet'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşimi yapılandırılabilir. Application Gateway tamamen Azure yönetilen, ölçeklenebilir ve yüksek oranda kullanılabilir. Daha iyi yönetilebilirlik için zengin tanılama ve günlüğe kaydetme özellikleri sağlar.
Yük Dengeleyici, yüksek performanslı, düşük gecikme süreli katman 4 Yük Dengeleme Hizmetleri için tüm UDP ve TCP protokollerini sağlayan Azure SDN yığınını bir parçası olan. Bu, gelen ve giden bağlantıları yönetir. Genel ve iç yük dengeli uç noktalarını yapılandırma ve hizmet kullanılabilirliği yönetmek için TCP ve HTTP sistem durumu yoklaması seçeneklerini kullanarak arka uç havuzu hedeflere gelen bağlantıları eşlemek için kurallar tanımlar.


## <a name="choosing-a-global-load-balancer"></a>Bir genel yük dengeleyici seçme
Genel Yönlendirme için bir genel yük dengeleyici Traffic Manager ve Azure ön kapısı arasında seçim yaparken, hizmetler, iki farklı nedir ve ne benzer düşünmelisiniz.   Her iki hizmet sağlayın
- **Çoklu coğrafi artıklık:** Tek bir bölge kalırsa, trafiği sorunsuz bir şekilde en yakın bölgeyi müdahalesi olmadan uygulama sahibinden yönlendirir.
- **En yakın bölge yönlendirme:** Trafik için en yakın bölgeyi otomatik olarak yönlendirilir

</br>Aşağıdaki tabloda, Traffic Manager ve Azure ön kapısı hizmet arasındaki farklar açıklanmaktadır:</br>

| Traffic Manager | Azure Front Door Hizmeti |
| --------------- | ------------------------ |
|**Herhangi bir protokolü:** Traffic Manager DNS katmanında çalışır çünkü her türlü ağ trafiği yönlendirebilen; HTTP, TCP, UDP, vs. | **HTTP hızlandırma:** Ön kapısı ile Edge Microsoft'un ağ proxy trafiğidir.  Bu nedenle, HTTP (S) istek gecikme süresi ve aktarım hızı geliştirmeleri SSL anlaşması için gecikme süresini azaltma ve sık erişimli AFD bağlantılarından uygulamanıza kullanarak bakın.|
|**Yönlendirme şirket içinde:** Bir DNS katmanında yönlendirme, trafiği her zaman bir noktadan noktaya gider.  Şube ofisinizde, şirket içi veri merkezine yönlendirme doğrudan bir yol alabilir; Traffic Manager kullanarak da kendi ağ üzerinde. | **Bağımsız ölçeklenebilirlik:** Ön kapısı HTTP isteği ile çalıştığı için farklı bir URL yolu için istekleri farklı arka uca yönlendirilmiş / bölgesel hizmet kuralları ve her uygulama mikro hizmet durumunu temel alan havuzları (mikro).|
|**Faturalandırma biçimi:** DNS kullanıma dayalı faturalandırma Kullanıcılarınızla ve daha fazla kullanıcısı olan hizmetleri ölçeklenen, en yüksek kullanımı maliyetini azaltmak için plateaus. |**Satır içi güvenlik:** Ön kapısı, oran sınırlandırma ve IP trafiği, uygulamanızı ulaşmadan önce uçlarınıza korumanıza olanak acl'sinin gibi kuralları etkinleştirir. 

</br>Performans, çalışabilirlik ve ön kapısı HTTP iş yükleriniz için güvenlik avantajları nedeniyle, müşterilerin ön kapısı, HTTP iş yükleri için kullanmanızı öneririz.    Traffic Manager ve ön kapısı paralel olarak tüm trafiği uygulamanız için hizmet vermek için kullanılabilir. 

## <a name="building-with-azures-application-delivery-suite"></a>Azure'nın uygulama teslim paketi ile oluşturma 
Tüm Web siteleri, API'leri, hizmetleri coğrafi olarak yedekli olmasını öneririz ve trafik, kullanıcılara en yakın (en düşük gecikme süresi) teslim konuma onları mümkün olduğunda.  Traffic Manager, ön kapısı hizmeti, uygulama ağ geçidi ve yük dengeleyici hizmetlerinden birleştirme, güvenilirlik, ölçek ve performans en üst düzeye çıkarmak için ne zonally ve coğrafi olarak yedekli oluşturmanızı sağlar.

Aşağıdaki diyagramda, genel web hizmeti oluşturmak için bu hizmetlerin tümü bir birleşimini kullanan bir örnek hizmet açıklanmaktadır.   Bu durumda, Mimarı Traffic Manager eşleşen düzeni/deposu/URL yollarına genel olarak yönlendirmek için ön kapı kullanırken, dosya ve nesne Teslimat genel arka uçları yönlendirmek kullanılan * hizmete bunlar App Service için diğer tüm yönlendirme sırasında Project.json'dan packagereference'a Bölgesel Application Gateway'ler için istek sayısı.

Bölgede, Iaas hizmet için uygulama geliştiricisi, verdi deseni/resimler /'ile eşleşen herhangi bir URL * web grubu geri kalanından farklı bir VM adanmış bir havuzdan sunulur.

Ayrıca, dinamik içerik sunan varsayılan sanal makine havuzu barındırılan bir arka uç veritabanı için yüksek kullanılabilirlik kümesinde konuşmak gerekir. Tüm dağıtım, Azure Resource Manager aracılığıyla ayarlanır.

Aşağıdaki diyagramda bu senaryonun mimarisi gösterilmektedir:

![Uygulama teslim paketi ayrıntılı mimarisi][2] 

> [!NOTE]
> Bu örnek yalnızca, Azure'un sunduğu Yük Dengeleme Hizmetleri birçok olası yapılandırmalar biridir. Traffic Manager, ön kapısı, uygulama ağ geçidi ve Load Balancer karıştırılabilir ve Yük Dengeleme ihtiyaçlarınızı en iyi karşılayacak şekilde eşleşti. Örneğin, SSL yük boşaltma ya da 7. Katman işlem gerekli değildir, Application Gateway yerine yük dengeleyici kullanılabilir.


## <a name="next-steps"></a>Sonraki Adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.

<!--Image references-->
[1]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure1.png
[2]: ./media/front-door-lb-with-azure-app-delivery-suite/application-delivery-figure2.png
