---
title: Azure DDoS koruması en iyi yöntemler & referans mimarisi | Microsoft Docs
description: Nasıl verileri günlüğe kaydetmeye uygulamanız hakkında ayrıntılı Öngörüler elde etmek için kullanabileceğiniz hakkında bilgi edinin.
services: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/20/2018
ms.author: barclayn
ms.openlocfilehash: 4b2d785f5b9095a2decfc65ec46808ff6f65c38e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architecture"></a>Azure DDoS koruması: en iyi yöntemler ve mimarisi referans

Bu belge, BT karar alıcılar ve güvenlik personel yöneliktir. Benzerlik Azure ile ağ ve güvenlik beklenir.

Hizmet (DDoS) dağıtılmış reddi tasarlama, planlama ve tasarlama modu hatası çeşitli için dayanıklılık gerektirir. Bu belge, Azure uygulamalarında DDoS saldırılarına karşı dayanıklılık için tasarlamak için en iyi yöntemler sağlar.

## <a name="types-of-attacks"></a>Tür saldırılar

DDoS uygulama kaynakları tüketmesine çaba içinde kullanılan saldırı türüdür. Uygulamanızın kullanılabilirlik ve yasal istekleri işleme yeteneği etkilemek için belirtilir. Saldırıları, daha karmaşık ve boyuta ve etkisi daha büyük hale gelmektedir. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

Azure DDoS saldırılara karşı sürekli koruma sağlar. Bu koruma, varsayılan olarak ve herhangi bir ek ücret ödemeden Azure platformuyla doğrudan tümleşik olduğu. DDoS koruması platform sağlanan çekirdek yanı sıra da adlı yeni bir sunum sahibiz "[Azure DDoS koruması standart](https://azure.microsoft.com/services/ddos-protection/)", ağ saldırılarına karşı Gelişmiş DDoS azaltma özellikleri sağlar ve olduğu belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanmış. Koruma, yeni sanal ağlar oluşturma sırasında etkinleştirmek basit bir işlemdir. İlk oluşturulduktan sonra da yapılabilir ve herhangi bir uygulama veya kaynak değişiklik gerektirmez.

![](media/azure-ddos-best-practices/image1.png)

DDoS saldırıları kapsamlı üç (3) farklı kategoride sınıflandırılabilir

### <a name="volumetric-attacks"></a>Volumetric saldırıları

Volumetric saldırıları DDoS saldırısının en yaygın türleridir. Volumetric saldırıları ağ ve aktarım katmanları hedef yanılma assaults ' dir. Bu tür ağ bağlantıları kaynaklarını tüketebilir deneyin. Bu saldırıların birden çok etkilenen sistemleri görünen meşru trafiğin önemli miktarda ağ katmanlarıyla bölgesini doldurmak için yaygın olarak kullanır. Internet Denetim İletisi Protokolü (ICMP), kullanıcı veri birimi Protokolü (UDP) ve İletim Denetimi Protokolü (TCP) gibi farklı ağ katmanı protokollerini bu tür saldırılar kullanılır.

Ağ katmanı saldırıları: TCP taşmasını Eşitlemeye, ICMP Yankı, UDP taşmasını, DNS ve NTP DDoS'en yaygın olarak kullanılan yükseltme saldırısı. Bu tür saldırılar, yalnızca hizmet, aynı zamanda daha alınan ve hedeflenen ağ yetkisiz erişim için bir smokescreen olarak kesintiye için kullanılabilir. Son volumetric saldırı örneğidir [Memcached yararlanma](https://www.wired.com/story/github-ddos-memcached/) GitHub etkilenebilir. Bu saldırı, UDP bağlantı noktası 11211 hedeflenen ve 1,35 Tb/s saldırı birimin oluşturulur.

### <a name="protocol-attacks"></a>Protokol saldırıları

Protokol hedef uygulama protokolleri saldırılarından. Altyapısı aygıtlarındaki güvenlik duvarları, uygulama sunucuları ve yük Dengeleyiciler gibi tüm kullanılabilir kaynakları kullanmayı deneyin. Protokol saldırıları hatalı biçimlendirilmiş ya da Protokolü anormal durumları içeren paketi kullanın. Bu tür saldırıları, çok sayıda açık isteklerin, sunucuları ve diğer iletişim aygıtları yanıt ve bir paket yanıt bekle göndererek çalışır. Hedef, sonunda hedeflenen sistem çökmesine neden açık isteklere yanıt verecek şekilde çalışır.

En yaygın bir protokol tabanlı DDoS saldırısının TCP Eşitlemeye sel örnektir. Bu saldırı art arda TCP Eşitlemeye isteklerinin doğru hedef yönlendirilir ve onu doldurmaya kullanılabilir. Hedef hedef yanıt veremez hale getirmektir. Bir uygulama katmanı saldırısı olması dışında 2016 Din kesinti, ayrıca bağlantı noktası 53 Din'in DNS sunucularının hedefleme TCP Eşitlemeye sel içermektedir.

### <a name="resource-attacks"></a>Kaynak saldırıları

Kaynak saldırıları uygulama katmanı hedefleyin. Bunlar, hedef sistem doldurmaya için arka uç işlemleri tetikler. Kaynak İzleme CPU-yoğun sunucuya sorgular ancak bu, normal görünen kötüye trafiği saldırıları. Kaynakları tüketmesine gerekli trafik hacmi oldukça, diğer tür saldırıların düşüktür. Bir kaynak saldırısı olarak algılamak zorlaşır yasal trafiğinden ayırt trafiğidir. En yaygın kaynak saldırıları HTTP/HTTPS ve DNS hizmetleri gerekir.

## <a name="shared-responsibility-in-the-cloud"></a>Bulutta paylaşılan sorumluluk

Savunma kapsamlı strateji artan türleri ve açıdan çok yönlülük saldırıları mücadele etmek için gereklidir. Güvenlik, müşteri ve Microsoft arasında paylaşılan bir sorumluluğundadır. Microsoft başvurduğu şunun gibi bir [paylaşılan sorumluluk modeli](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). Aşağıdaki şekilde bu bölme sorumluluk gösterilmektedir.

![](media/azure-ddos-best-practices/image2.png)

Bizim en iyi uygulamaları gözden geçirme Azure müşterilerine yararlanabilir ve genel derleme tasarlanmış ve test için hatası uygulamalar dağıtılır.

## <a name="fundamental-best-practices"></a>Temel en iyi uygulamalar

DDoS saldırılar ve artarken. Aşağıdaki bölümde, Azure üzerinde DDoS dayanıklı hizmetleri oluşturmak için belirleyici rehberlik sağlar.

### <a name="design-for-security"></a>Güvenlik için tasarlama

Müşteriler, güvenlik tasarımı ve uygulama dağıtım ve işlemleri'ten bir uygulamanın tüm yaşam döngüsü boyunca bir önceliktir emin olmalısınız. Uygulama kaynakları, bir hizmet kesintisi kaynaklanan Ölçüsüz kullanmak için hazırlanmış istekleri nispeten düşük hacmi izin hatalar sahip olamaz. Microsoft Azure üzerinde çalışan bir hizmete korumak için müşteriler kendi uygulama mimarisinin iyi anlamış olmanız gerekir ve odaklanmak gerekir [yazılım kalitesi 5 ayaklar](https://docs.microsoft.com/azure/architecture/guide/pillars).
Müşteriler tipik trafiği birimler, uygulama ve diğer uygulamalar ve ortak Internet'e açık hizmet uç noktaları arasında bağlantı modeli bilmeniz gerekir.

Ayrıca, bir uygulama bir uygulaması, hedeflenen hizmet reddi işlemek için esnek olduğundan olmanın en önemli çok önemlidir. Güvenlik ve gizlilik yerleşik olarak bulunur itibaren sağ Azure platformu, [güvenlik geliştirme yaşam döngüsü (SDL)](https://www.microsoft.com/sdl/default.aspx). SDL her geliştirme aşamada güvenlik adreslerini ve Azure daha güvenli hale getirmek için sürekli olarak güncelleştirilir sağlar.

### <a name="design-for-scalability"></a>Ölçeklenebilirlik sağlamak için Tasarım

Ölçeklenebilirlik, sistemin artan yükü idare edebilme özelliğidir. Müşterilerin kendi uygulamalarında tasarım gerekir [yatay ölçek](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) yükseltilmiş yük, özellikle bir olay DDoS saldırısının talebi karşılamak üzere. Uygulamanızın tek bir hizmet örneği üzerinde bağımlı olması durumunda, tek bir hata noktası oluşturur. Birden çok örneği sağlama, esneklik ve ölçeklenebilirlik artırır.

İçin [Azure App Service](../app-service/app-service-value-prop-what-is.md)seçin bir [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) birden çok örneği sunar. Azure bulut Hizmetleri için kullanmak için rollerinin her biri yapılandırma [birden çok örneği](../cloud-services/cloud-services-choose-me.md). İçin [Azure sanal makineleri (VM'ler)](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), VM Mimarinizi birden fazla VM içerir ve her VM bulunduğundan emin olun bir [kullanılabilirlik kümesi](../virtual-machines/virtual-machines-windows-manage-availability.md). Kullanmanızı öneririz [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) otomatik ölçeklendirmeyi özellikleri.

### <a name="defense-in-depth"></a>Derinlemesine

, Derinlemesine arkasında farklı savunma stratejileri ile risk yönetme için olur. Bir uygulamada güvenlik savunmaları katmanlama başarılı bir saldırı olasılığını azaltır. Müşteriler yerleşik özellikleri aracılığıyla uygulamalarını Azure platformu için güvenli tasarımlar uygulamak öneririz.

Örneğin, boyut veya uygulamanın yüzey alanını saldırı riskini artırır. Yüzey alanını azaltma önerilir uygulamaları güvenilir listeye almayı sunulan IP Kapat aşağı adres alanı & üzerinde yük dengeleyici gerekli olmayan dinleme bağlantı noktaları ([ALB](../load-balancer/load-balancer-get-started-internet-portal.md)/[uygulama ağ geçidi](../application-gateway/application-gateway-create-probe-portal.md)) veya aracılığıyla [ağ güvenlik grubu (NSG).](../virtual-network/virtual-networks-nsg.md)
Kullanabileceğiniz [hizmet etiketleri](/virtual-network/security-overview.md) ve [uygulama güvenlik grupları](/virtual-network/security-overview.md) karmaşıklık güvenlik kuralı oluşturma en aza indirmek ve ağ güvenliği uygulamanın yapısı doğal bir uzantısı olarak yapılandırmak için.

Müşteriler, Azure hizmetlerinde dağıtmalısınız bir [sanal ağ](../virtual-network/virtual-networks-overview.md) mümkün olduğunda. Bu hizmet kaynakları özel IP adresleri iletişim kurmasına izin verir. Bir sanal ağ trafiğinden Azure hizmeti genel IP adresleri, varsayılan olarak kaynak IP adresleri olarak kullanır. Kullanarak [hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) sanal ağ özel adresler kaynak IP adresleri olarak Azure hizmeti sanal ağdan erişirken kullanılacak hizmeti trafiğinin geçiş yapar.

Biz, müşterinin içi Azure kaynaklarını yanında Saldırıya uğrayan kaynaklar genellikle bakın. Azure için bir şirket içi ortamda bağlanıyorsanız, şirket içi kaynakları riskini genel İnternet en aza öneririz. İyi bilinen ortak varlıklarınızı Azure dağıtarak ölçek ve Azure Gelişmiş DDoS koruması özelliklerini kullanabilirsiniz. Bu genel olarak erişilebilir varlıklar genellikle DDoS saldırıları için bir hedefi olduğundan, Azure'da koyma, şirket içi kaynaklar üzerindeki etkisini azaltır.

## <a name="azure-ddos-protection"></a>Azure DDoS koruması

Azure, ağ saldırılarına (Katman 3 ve 4) - DDoS temel koruma ve DDoS standart koruma karşı koruma sağlayan iki DDoS hizmet teklifleri sahiptir. 

### <a name="azure-ddos-basic-protection"></a>Azure DDoS temel koruma

Temel koruma, varsayılan olarak hiçbir ek ücret ödemeden Azure'da tümleşiktir. Azure'nın genel olarak dağıtılan ağ kapasitesini ve tam ölçekli ortak ağ katmanı saldırılar her zaman trafiği izleme ve gerçek zamanlı azaltma karşı koruma sağlar. DDoS Basic koruma kullanıcı yapılandırma veya uygulama değişiklikleri gerektirir. Azure DNS gibi PaaS Hizmetleri dahil olmak üzere tüm Azure hizmetlerinin temel DDoS Koruması tarafından korunur.

![](media/azure-ddos-best-practices/image3.png)

Azure'nın temel DDoS koruması yazılım ve donanım bileşenden oluşur. Ne zaman yazılım denetim düzlemi karar, burada, ve trafik türünü çözümlemek ve saldırı trafiği kaldıran donanım cihazları steered. Bir altyapı genelinde DDoS koruması göre bu karara denetim düzlemi yapar *İlkesi*, hangi statik olarak ayarlayın ve evrensel tüm Azure müşterilerine uygulanması.

Örneğin, hangi trafik birim koruma olmalıdır DDoS koruması ilkesi belirtir *tetiklenen* (diğer bir deyişle, kiracının trafiği cihazları temizleme üzerinden yönlendirileceğini) ve daha sonra nasıl temizleme cihazları gereken *azaltmak* saldırı.

Azure'nın DDoS koruması temel hizmeti altyapı koruma ve Azure platformu korumasını yöneliktir. Çok kiracılı ortamında birden çok müşteri etkileyebilir kadar önemli olduğu bir oran aştığında trafiğini azaltır. Uyarı sağlamaz veya müşteri başına ilkeleri özelleştirilebilir.

### <a name="azure-ddos-standard-protection"></a>Azure DDoS standart koruma

Standart koruması Gelişmiş DDoS azaltma özellikleri sağlar ve otomatik olarak bir sanal ağdaki belirli Azure kaynaklarınızı korumak için özel olarak ayarlanmış. Koruma, tüm yeni veya mevcut sanal ağda etkinleştirmek basit bir işlemdir ve hiçbir uygulamanın veya kaynak değişiklikleri gerektirir. Günlüğe kaydetme, uyarı ve telemetri dahil olmak üzere temel hizmeti üzerinde çeşitli avantajları vardır. Aşağıda açıklanan Azure DDoS koruması standart hizmetinin anahtar differentiators var.

#### <a name="adaptive-realtime-tuning"></a>Uyarlamalı gerçek zamanlı ayarlama

Azure DDoS koruması temel hizmeti müşterilerin korunmasına yardımcı olur, ancak yalnızca diğer müşteriler etkileyen önlemek için korur. Örneğin, bir hizmet tipik bir birim değerinden küçük yasal gelen trafik için sağlanan *tetikleyici oranı* altyapı genelinde DDoS koruması İlkesi o Müşteri'nin kaynaklar üzerinde DDoS saldırı gidebilir bilgisi dışında. Daha fazla genel olarak, kiracılar uygulamaya özgü davranışlarını yanı sıra, en son (örneğin, çok vektör DDoS) saldırıları karmaşık yapısını başına-müşteri için özelleştirilmiş koruma ilkeleri arayın.
Bu, iki kullanarak gerçekleştirilir:

1. Müşterinin başına (her-IP) otomatik öğrenme Katman 3/4 trafik modelleri ve
2. Azure'nın ölçek önemli miktarda trafiği almak amacıyla verir o hatalı pozitif sonuç en aza indirir.

![](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring--alerting"></a>DDoS koruması telemetri, izleme ve uyarı

DDoS koruması standart ile biz zengin telemetri aracılığıyla kullanıma [Azure İzleyici](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) DDoS saldırısının süresi sırasında. Uyarı herhangi bir DDoS Koruması tarafından kullanıma sunulan Azure İzleyici ölçümleri için yapılandırılabilir. Günlüğe kaydetme Azure İzleyici tanılama arabirimi aracılığıyla Gelişmiş analiz için Splunk (Azure Event Hubs), Azure günlük analizi ve Azure Storage ile tümleştirilebilir.

##### <a name="ddos-mitigation-policies"></a>DDoS azaltma ilkeleri

Azure portalında tıklatın **İzleyici** \> **ölçümleri**. Üzerinde **ölçümleri** ekranında, kaynak grubu, ortak IP adresinin kaynak türü ve Azure genel IP adresi seçin. DDoS ölçümleri kullanılabilir ölçümler bölmesi altında görünür.

DDoS koruması standart geçerlidir **üç otomatik ayarlı azaltma ilkeleri (TCP Eşitlemeye, TCP ve UDP)** her ortak IP korumalı kaynağının DDoS etkin olan sanal ağ için. Ölçüm seçerek ilke eşikleri görüntüleyebilirsiniz **'Gelen DDoS azaltma tetiklemek için paketler'**.

![](media/azure-ddos-best-practices/image7.png)

İlke tabanlı ağ trafiği profil oluşturmayı öğrenme bizim makine aracılığıyla otomatik olarak yapılandırılan eşiklere. Yalnızca ilke eşiği aşıldığında DDoS azaltma için bir IP adresi saldırıya gerçekleşir.

##### <a name="under-ddos-attack"></a>DDoS Saldırıya uğramış

Genel IP adresi Saldırıya uğramış ise, biz saldırı trafiğini azaltma gerçekleştirirken ölçüm 'Altında DDoS veya saldırı' değeri 1 olarak değiştirir.

![](media/azure-ddos-best-practices/image8.png)

Ortak IP adresi üzerinde gerçekleştirilen etkin bir DDoS azaltma olduğunda bildirim almak için bu ölçüm üzerinde bir uyarı yapılandırma öneririz.

Daha fazla bilgi için bkz: [yönetmek Azure DDoS koruması Azure portalını kullanarak standart](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="resource-attacks"></a>Kaynak saldırıları

Belirli kaynak saldırılara uygulama katmanında, müşterilerin güvenli web uygulamaları yardımcı olmak için Web uygulaması Güvenlik Duvarı (WAF) yapılandırmanız gerekir. WAF SQL eklemelerini, siteler arası komut dosyası, DDoS ve diğer katman 7 saldırılarını engellemek için gelen web trafiği olup olmadığını denetler. Azure sağlar [WAF uygulama ağ geçidi, bir özellik olarak](../application-gateway/application-gateway-web-application-firewall-overview.md) web uygulamalarınızdan ortak açığından yararlanma girişimi ve güvenlik açıkları merkezi koruma. WAF teklifleri çeşitleri gereksinimlerinize daha uygun olabilir Azure ortaklardan kullanılabilir [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Hatta Web uygulaması güvenlik duvarı volumetric & Durum Tükenme saldırılarına açıktır. Gelen volumetric korumak için WAF sanal ağ üzerinde DDoS standart korumayı kapatma önerilir & Protokolü saldırıları. Daha fazla bilgi için başvuru mimarisi bölümüne bakın.

### <a name="protection-planning"></a>Korumayı planlama

Planlama ve hazırlama bir sistem DDoS saldırı sırasında nasıl gerçekleştirecek anlamak önemli. Ayrıca bu planlama ve hazırlık bir olay Yönetimi yanıt planı tasarlamanıza yardımcı olur.

Müşteriler DDoS koruması standart Internet sanal ağ üzerinde etkin olduğunu e yönelik uç noktalar emin olmalısınız. DDoS uyarıları yapılandırma üzerinde tüm olası saldırılara karşı altyapınızda sabit watchful göz tutmaya yardımcı olur. Müşterilerin kendi uygulamalarında bağımsız olarak izlemeniz gerekir. Uygulamanın normal davranışlarını anlamak gerekir. Uygulama DDoS saldırı sırasında beklendiği gibi çalışmaz, adımlar izlenmelidir.

#### <a name="ddos-attacks-orchestration"></a>DDoS saldırıları düzenleme

Dönemsel benzetimleri yürütülerek bile gerçekleşmeden önce bir saldırıya karşı hizmetlerinizi nasıl yanıt hakkında varsayımları test etmek için iyi bir uygulamadır. Test sırasında hizmetlerin ve uygulamaların beklendiği gibi çalışmaya devam eder ve son kullanıcı deneyimi için hiçbir bozulma olduğunu doğrulayın. Bir teknoloji & işlem açısından çözebileceğiniz boşlukları tanımlar ve DDoS yanıt stratejinize içerecek. Genel hazırlama ortamları veya üretim ortamına etkisini en aza indirmek için yoğun olmayan saatlerde böyle sınamalar gerçekleştirmek için önerilir.

İle işbirliği [BreakingPoint bulut](https://www.ixiacom.com/products/breakingpoint-cloud) bir arabirim oluşturmak için burada Azure müşterilerine DDoS koruması karşı trafiği oluşmasına neden olabilir benzetimleri için ortak uç noktaları etkin. [Kesme noktası bulut](https://www.ixiacom.com/products/breakingpoint-cloud) benzetimi etmenizi sağlar:

- Microsoft Azure DDoS koruması Azure kaynaklarınızı DDoS saldırılara karşı nasıl koruduğunu doğrula

- Olay yanıtlama işleminizi DDoS saldırıya en iyi duruma getirme

- Belge DDoS uyumluluk

- Ağ güvenlik gruplarını eğitme

Siber güvenlik savunma içinde sabit yenilik gerektiren zorlu bir var olma Savaşının ' dir. Azure'nın DDoS standart bir giderek karmaşık DDoS saldırıları azaltmak olan müşteriler için etkin bir çözüm sunan harikası sunumu harikası korumadır.

## <a name="components-of-a-ddos-response-strategy"></a>Bir DDoS yanıt stratejisi bileşenleri

Çalışmalarının çoğu DDoS saldırı Azure kaynaklarınızı hedeflendiğinde yoktur bir son kullanıcı açısından gerekli en az müdahale. Yine de, iş sürekliliği için en az etki DDoS ekleme azaltma kuruluşunuzun olay yanıtlama stratejisinin bir parçası olarak sağlanır.

### <a name="microsoft-threat-intelligence"></a>Microsoft tehdit bilgileri

Microsoft, Microsoft online services, Microsoft iş ortakları ve Internet güvenlik topluluğu içinde ilişkileri destekleyen bir Genişletilmiş Güvenlik Topluluğu'nun toplu bilgi kullanan bir kapsamlı tehdit Intelligence ağ sahiptir. Bir kritik altyapı sağlayıcısı gibi Microsoft tehditler hakkında erken uyarı alır, diğer Microsoft online services ve Microsoft'un Genel Müşteri öğrenilen tehdit bilgileri temel alır. Tüm Microsoft tehdit bilgileri birleştirilmiş geri Azure DDoS koruması ürünlerinden.

Buna ek olarak, Microsoft dijital Suçlar birimi (DCU) botnets, ortak bir kaynaktan komut ve denetim karşı rahatsız edici stratejileri için DDoS saldırıları gerçekleştirir.

### <a name="perform-a-risk-evaluation-of-your-azure-resources"></a>Azure kaynaklarınızı bir risk değerlendirmesi gerçekleştirmek

riskini DDoS saldırılara karşı düzenli olarak kapsamı anlamak için zorunludur. Müşteriler düzenli aralıklarla isteyin kendilerini: hangi yeni genel kullanıma açık Azure kaynaklarını koruma gerekir? Hizmet hataları tek noktası var? Nasıl hizmetleri hala Hizmetleri geçerli kullanıcılara kullanılabilir yapma çalışırken bir saldırının etkisini sınırlamak için ayrılmış olabilir mi? Burada DDoS koruması standart etkinleştirilmesi gerekir, ancak değil sanal ağlar var mı? My Hizmetleri etkin/etkin birden çok bölgeler arasında yük devretme kümelemesiyle misiniz?

### <a name="customer-ddos-response-team"></a>Müşteri DDoS yanıt ekibi

DDoS yanıt ekip oluşturma, saldırının etkili ve hızlı yanıta sağlamak anahtar bir adımdır. Müşteriler, planlama ve yürütme işleten kuruluşunuzdaki çeşitli kişiler tanımlamanız gerekir. DDoS yanıt takım Azure DDoS koruması standart hizmeti kapsamlı olarak anlamayı olmalıdır ve tanımlama ve Microsoft destek ekibi dahil olmak üzere çeşitli dahili ve harici olan müşteriler, Eşgüdümleme bir saldırının Azaltıcı apt olmalıdır gerektiğinde.

Microsoft, Ölçek, hizmet kullanılabilirliğini ve sürekliliği planlama normal bir parçası olarak test de dahil olmak üzere DDoS yanıt takım planlama ve benzetimi alıştırmaları ekleme önerir. 

### <a name="during-an-attack"></a>Saldırının sırasında

Azure DDoS koruması standart tanımlayabilir ve kullanıcı müdahalesi olmadan DDoS saldırıları azaltmak. Korumalı bir genel IP için etkin bir Azaltıcı Etkenler olduğunda bildirim almak için yapabileceğiniz [bir uyarı yapılandırmak](../virtual-network/ddos-protection-manage-portal.md) ölçüm üzerinde "Altında DDoS saldırı veya değil". Daha ayrıntılı olarak gözden geçirme ve saldırı, bırakılan trafik vb. ölçeğini anlamak diğer DDoS ölçümler için istediğiniz gibi uyarılar oluşturabilir.

#### <a name="when-to-contact-microsoft-support"></a>Ne zaman Microsoft Destek'e başvurun

- DDoS saldırı sırasında görürseniz korumalı kaynağa performansını ciddi bir şekilde düşürülmüş veya kaynak mevcut değil.

- Düşünüyorsanız DDoS koruması hizmeti beklendiği gibi çalışmaz. DDoS koruması hizmetini yalnızca başlatmak azaltma, ölçütleri karşılar:

    - Ölçü değerini ' DDoS azaltma (TCP/TCP Eşitlemeye/UDP) tetiklemek için ilke korumalı genel IP kaynağı alınan trafik değerinden daha düşük.

- Biliyorsanız, ağ trafiğinizi önemli artışa neden planlı bir virüslü olay alınacaktır.

- Bir oyuncu kaynaklarınızı DDoS saldırısı başlatmak tehdit değilse.

Sorunları etkileyen kritik iş için bir önem derecesi-Oluştur [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>POST saldırı adımları

Bir durum incelemesi saldırı sonrasındaki yapmak ve DDoS yanıt stratejisi gerektiği gibi yeniden ayarlama gereksinimini her zaman iyi bir stratejidir. Göz önünde bulundurmanız gerekenler:

- Edildi var. herhangi kesinti hizmetine veya son kullanıcı deneyimi için ölçeklenebilir mimari yetersizliğinden?

- Hangi uygulamalar veya hizmetler en başlayamıyorsa?

- DDoS yanıt stratejisi nasıl etkili ve nasıl, daha fazla iyileşme?

Lütfen bir DDoS Saldırıya uğramış olduğundan şüpheleniyorsanız, normal Azure destek kanalları taşıyarak.

## <a name="ddos-protection-reference-architectures"></a>DDoS koruması başvuru mimarisi

DDoS koruması standart tasarlanmıştır [bir sanal ağda dağıtılmış hizmetleri için.](../virtual-network/virtual-network-for-azure-services.md) Diğer hizmetler için varsayılan DDoS temel koruması geçerli olur. Aşağıda açıklanan belirli başvuru mimarileri bir arada gruplandırılmış mimarisi desenlerle senaryolar tarafından düzenlenir.

### <a name="virtual-machine-windowslinux-workloads"></a>Sanal makine (Windows/Linux) iş yükleri

#### <a name="application-running-on-load-balanced-vms"></a>VM'ler üzerindeki yük uygulama çalıştıran dengeli

Bu başvuru mimarisi, bir yük dengeleyicinin arkasındaki bir ölçek kümesinde birden çok Windows sanal makinesi (VM) çalıştırmaya yönelik kendini kanıtlamış bir dizi uygulamayı göstermektedir. Bu mimari, bir web sunucusu gibi durum bilgisiz tüm iş yükü için kullanılabilir.

![](media/azure-ddos-best-practices/image9.png)

Bu mimaride, bir iş yükü birden çok VM örneğine dağıtılır. Tek bir genel IP adresi vardır ve İnternet trafiği bir yük dengeleyici kullanılarak VM’lere dağıtılır. DDoS koruması standart ilişkili genel IP sahip Azure (Internet) yük dengeleyici sanal ağda etkinleştirilir.

Yük Dengeleyici gelen Internet isteklerini VM örnekleri dağıtır. VM ölçek kümesi, el ile veya otomatik olarak önceden tanımlanmış kurallara göre giriş veya çıkış ölçeklendirilmesi VM'ler izin verir. Bu kaynak DDoS Saldırıya uğramış durumdaysa önemlidir. Bu başvuruda [makale](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm), bu başvuru mimarisi hakkında daha fazla bilgi için.

#### <a name="applications-running-on-windows-n-tier"></a>Windows N katmanlı üzerinde çalışan uygulamalar

N katmanlı mimari uygulamanın birçok yolu vardır. Diyagramda tipik 3 katmanlı web uygulaması gösterilmektedir. Bu mimari derlemeler [ölçeklenebilirlik ve kullanılabilirlik için yük dengeli sanal makineleri çalıştırmak](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). Web ve iş katmanları yük dengeli VM’leri kullanır.

![](media/azure-ddos-best-practices/image10.png)

Yukarıdaki mimarisinde, sanal ağ üzerinde DDoS koruması standart etkinleştirilir. Sanal ağ içindeki tüm genel IP'ler Layer3/Layer4 DDoS koruması alırsınız. Katman 7 koruma için uygulama ağ geçidi WAF SKU dağıtılmalıdır. Başvurmak [bu](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier) makalede, bu başvuru mimarisi hakkında daha fazla bilgi için.

#### <a name="paas-web-application"></a>PaaS web uygulaması

Bu başvuru mimarisinin tek bir bölgede bir Azure uygulama hizmeti uygulaması çalıştıran gösterir. Bu mimari kullanan bir web uygulaması için kanıtlanmış uygulamalar kümesidir gösterir [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) ve [Azure SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/).
Bekleme bölge, yük devretme senaryoları için ayarlanır.

![](media/azure-ddos-best-practices/image11.png)

Trafik Yöneticisi gelen istekleri bölgelerinden uygulama ağ geçidi yönlendirir. Normal işlemler sırasında uygulama ağ geçidi etkin bölgede istekleri yönlendirir. Bu bölgede kullanılamaz duruma gelirse, trafik Yöneticisi üzerinden bekleme bölgede uygulama ağ geçidi için başarısız olur.

Web uygulamasını hedefleyen Internet'ten gelen tüm trafiği yönlendirilir [uygulama ağ geçidi genel IP adresi](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) üzerinden trafik Yöneticisi. Bu senaryoda, uygulama hizmeti (web uygulamaları) kendisine doğrudan değil dışarıdan karşılıklı ve uygulama ağ geçidi tarafından korunmaktadır. Uygulama ağ geçidi WAF SKU yapılandırmak için önerilen (modunu engelleyin) katman 7 (HTTP/HTTPS/Web yuvasını) saldırılarına karşı korumak için. Ayrıca, Web uygulamaları için yapılandırılmış olan [yalnızca uygulama ağ geçidi gelen trafiği kabul](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP adresi.

Başvurmak [bu](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region) makalede, bu başvuru mimarisi hakkında daha fazla bilgi için.

### <a name="mitigation-for-non-web-paas-services"></a>Web olmayan PaaS Hizmetleri için azaltma

#### <a name="hdinsight-on-azure"></a>Azure Hdınsight'ta

Bu başvuru mimarisinin gösterir DDoS koruması standart yapılandırma [Hdınsight kümesi](https://docs.microsoft.com/azure/hdinsight/) Azure üzerinde. Hdınsight kümesi bir sanal ağa bağlanır ve bu sanal ağ üzerinde DDoS koruması etkinleştirildiğinden emin olmanız gerekir.

![](media/azure-ddos-best-practices/image12.png)

![](media/azure-ddos-best-practices/image13.png)

Bu mimaride, internet'ten Hdınsight kümesine hedeflenen trafiği, Hdınsight ağ geçidi yük dengeleyici ile ilişkili genel IP yönlendirilir. Ağ geçidi yük dengeleyicinin ardından trafiği baş düğümler veya çalışan düğümleri doğrudan gönderir. Verilen DDoS koruması standart Hdınsight sanal ağ etkinleştirildiğinde, sanal ağda tüm genel IP'ler Layer3/Layer4 DDoS koruması alırsınız. Yukarıdaki referans mimarisi N-katmanı/çok-Region başvuru mimarileri ile birleştirilebilir.

Bu başvuru mimarisi hakkında daha fazla ayrıntı için başvurmak [genişletmek Azure bir Azure sanal ağı kullanarak Hdınsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) belgeleri.

### <a name="azure-api-management"></a>Azure API Yönetimi

Bu başvuru mimarisinin ortak uç noktası korur [API management](../api-management/api-management-key-concepts.md) kaynak kuruluş dış müşterilere API'leri yayımlama. Bir dış sanal DDoS koruması etkinleştirmek için ağ içinde APIM dağıtılmalıdır.

![](media/azure-ddos-best-practices/image15.png)

Dış sanal ağ yapılandırarak, API Management ağ geçidi & Geliştirici Portalı bir genel yük dengeleyiciye aracılığıyla ortak Internet üzerinden erişilebilir. Bu mimaride APIM sanal ağ dış sanal ağ DDoS koruması standart etkinleştirilir. Trafik, internet'ten Layer3/Layer4 ağ saldırılarına karşı korumalı APIM ortak IP adresine yönlendirilir. Katman 7 HTTP/HTTPs saldırılarına karşı korumak için bir uygulama ağ geçidi WAF modda yapılandırabilirsiniz.

Bir sanal ağ içinde dağıtılmıştır ve DDoS koruması standart korunur için yapılandırılan ek hizmetlerin listesi [burada](../virtual-network/virtual-network-for-azure-services.md). DDoS koruması standart yalnızca Azure Resource Manager kaynaklarını destekler. *Genel IP sahip bir VNET eklenen uygulama hizmeti ortamı (ana) dağıtımında yerel olarak desteklenmez.* ANA ortamı koruma hakkında daha fazla bilgi için bu bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DDoS koruması ürün sayfası](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS koruması blogu](http://aka.ms/ddosblog)

* [Azure DDoS koruması belgeleri ](../virtual-network/ddos-protection-overview.md)
