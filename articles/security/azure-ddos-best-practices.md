---
title: Azure DDoS koruması, en iyi uygulamalar ve başvuru mimarileri | Microsoft Docs
description: Nasıl günlük verilerini uygulamanız hakkında ayrıntılı Öngörüler elde etmek için kullanabileceğiniz hakkında bilgi edinin.
services: security
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: barclayn
ms.openlocfilehash: 11f3dcefd283ada00e915c2d6cb8abf654590ec1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60588160"
---
# <a name="azure-ddos-protection-best-practices-and-reference-architectures"></a>Azure DDoS koruması: En iyi yöntemler ve başvuru mimarileri

Bu makalede, BT karar alma mekanizmaları ve personel güvenlik içindir. Azure ile ilgili bilgi sahibi olduğunuz bekliyor ağ ve güvenlik.

Dağıtılmış engelleme (DDoS) hizmeti için tasarlama, planlama ve tasarlama hata modlarını çeşitli için dayanıklılık gerektirir. Bu makalede, DDoS saldırılarına karşı dayanıklılık için azure'da uygulamalar tasarlamaya yönelik en iyi uygulamalar sağlanır.

## <a name="types-of-attacks"></a>Tür saldırılar

DDoS, uygulama kaynaklarını tüketebilir dener saldırı türüdür. Uygulama kullanılabilirliği ve meşru istekler işleyebilme etkileyen olmaktır. Saldırıları, daha karmaşık ve daha büyük boyuta ve etkisi gelmektedir. DDoS saldırıları internet üzerinden genel olarak erişilebilen herhangi bir uç noktasını hedefleyebilir.

Azure DDoS saldırılarına karşı sürekli koruma sağlar. Bu koruma, varsayılan ve olmaksızın Azure platformuyla doğrudan tümleşik olduğu ek bir maliyet. 

DDoS koruması platformunda çekirdek yanı sıra [standart Azure DDoS koruması](https://azure.microsoft.com/services/ddos-protection/) ağ saldırılarına karşı Gelişmiş DDoS düzeltme özellikleri sağlar. Ayrıca, belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanmıştır. Koruma sırasında yeni bir sanal ağ oluşturmayı etkinleştirmek basit bir işlemdir. Oluşturulduktan sonra yapılabilir ve herhangi bir uygulamayı veya kaynağı bir değişiklik gerektirmez.

![Müşteriler ve bir sanal ağ bir saldırıdan koruma konusunda Azure DDoS koruması rolün](media/azure-ddos-best-practices/image1.png)

DDoS saldırılarına üç kategoride sınıflandırılır: volumetric, protokolü ve kaynak.

### <a name="volumetric-attacks"></a>Volumetric saldırıları

Volumetric saldırılar, en yaygın türü DDoS saldırısının arasındadır. Volumetric saldırılar ağ ve aktarım katmanlarını hedef deneme yanılma assaults arasındadır. Ağ bağlantıları gibi kaynakları tüketebilir deneyin. 

Bu saldırıları, birden çok etkilenen sistemleri genellikle görünüşte yasal trafiği ağ katmanlarla doldurmak için kullanın. Bunlar Internet Denetim İletisi Protokolü (ICMP), kullanıcı veri birimi Protokolü (UDP) ve İletim Denetimi Protokolü (TCP) gibi ağ katmanı protokollerini kullanır.

En sık kullanılan ağ katmanı DDoS saldırıları, ICMP Yankı, UDP, DNS, taşmasını taşmasını TCP SYN olan ve NTP yükseltme saldırıları. Bu tür saldırılar, yalnızca hizmeti, aynı zamanda daha alınan ve hedeflenen ağa yetkisiz erişim için bir smokescreen olarak kesintiye kullanılabilir. Son volumetric saldırısını örneğidir [Memcached yararlanma](https://www.wired.com/story/github-ddos-memcached/) GitHub etkilenen. Bu saldırı UDP bağlantı noktası 11211 hedef ve saldırı birimin 1,35 Tb/sn üretilen.

### <a name="protocol-attacks"></a>Protokol saldırıları

Protokol, hedef uygulama protokolleri saldırıları. Yük Dengeleyiciler ve güvenlik duvarları, uygulama sunucuları gibi altyapı cihazlarındaki tüm kullanılabilir kaynakları kullanmak deneyin. Protokol saldırıları hatalı biçimlendirilmiş veya protokolü prosesler içeren paketleri kullanın. Bu tür saldırıları, sunucuları ve diğer iletişim cihazları yanıtlayın ve bir paket yanıtı bekle açık isteklerin çok sayıda göndererek çalışır. Hedef sonunda sistemin çökmesine neden açık isteklere yanıt verecek şekilde çalışır.

En yaygın protokolüne dayalı bir DDoS saldırısının TCP SYN baskın örnektir. Bu saldırısında, TCP SYN istekleri izleyenler hedef doldurmaya çalışır. Hedef yanıt vermeyen olun olmaktır. TCP SYN toplamda bir uygulama katmanı saldırı olması dışında 2016 Din kesinti Din'ın DNS sunucularının hedeflenen Bu bağlantı noktası 53 şişirir.

### <a name="resource-attacks"></a>Kaynak saldırıları

Kaynak saldırıları, uygulama katmanına hedefleyin. Bunlar, bir sistem doldurmaya çabası arka uç işlemleri tetikleyin. Kaynak CPU yoğunluklu sunucuya sorgular taşır, ancak normal görünüyor kötüye trafiği saldırıları. Trafik kaynakları tüketebilir için gereken, bir tür saldırıların düşüktür. Bir kaynak saldırı trafiği algılamak zor hale yasal akışından ayırt. En yaygın kaynak saldırıları, HTTP/HTTPS ve DNS hizmetleri gerekir.

## <a name="shared-responsibility-in-the-cloud"></a>Buluttaki paylaşılan sorumluluk

Derinlemesine savunma stratejisi, artan ve gelişmişlik düzeyi saldırıları, mücadele yardımcı olur. Güvenlik, müşteri ile Microsoft arasında paylaşılan bir sorumluluğundadır. Microsoft bu çağıran bir [paylaşılan sorumluluk modeli](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). Sorumluluk bu bölme aşağıdaki şekilde gösterilmiştir:

![Azure müşteri ve sorumlulukları](media/azure-ddos-best-practices/image2.png)

Azure müşterileri, Microsoft en iyi uygulamaları gözden geçirmeden avantajından yararlanabilir ve dağıtılmış tasarlanmış ve test hatası için uygulamalar genel olarak oluşturma.

## <a name="fundamental-best-practices"></a>Temel en iyi uygulamalar

Aşağıdaki bölümlerde, Azure üzerinde DDoS dayanıklı hizmetler oluşturmak için belirleyici rehberlik sağlar.

### <a name="design-for-security"></a>Güvenlik için tasarlama

Güvenlik, tasarım ve uygulamadan dağıtım ve işletime kadar uygulamanın tüm yaşam döngüsü boyunca bir öncelik olduğundan emin olun. Uygulamaları bir nispeten düşük bir hizmet kesintisi kaynaklanan kaynakları, Ölçüsüz kullanmak için istek hacmi izin hataları olabilir. 

Microsoft Azure'da çalışan bir hizmetin korumaya yardımcı olmak için uygulama mimarinizin iyi anlamış olmanız ve gerekir odaklanmak [, yazılım kalitesinin beş yapı taşına](https://docs.microsoft.com/azure/architecture/guide/pillars).
Normal trafik birimler, uygulama ve diğer uygulamalar ve genel internet'e bağlı hizmet uç noktaları arasında bağlantı modeli bilmeniz gerekir.

Uygulamanın bir uygulama tarafından hedeflenen hizmet reddi işlemek için dayanıklı olduğundan emin olmanın en çok önemlidir. Güvenlik ve gizlilik yerleşik olarak Azure platformu ile başlayarak, [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). SDL her geliştirme aşaması sırasında güvenlik yöneliktir ve Azure sürekli daha güvenli hale getirmek için güncelleştirilmesini sağlar.

### <a name="design-for-scalability"></a>Ölçeklenebilirlik için tasarlama

Bir sistemin artan yükü ne kadar iyi işleyebilir ölçeklenebilirlik özelliğidir. Uygulamalarınıza tasarlamanız gerekir [yatay olarak genişletmek](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) özellikle bir DDoS saldırısının olması durumunda, yükseltilmiş bir yük talebi karşılamak üzere. Uygulamanızın bağımlı hizmetinin tek bir örneği, bir tek hata noktası oluşturur. Birden fazla sağlama sisteminize karşı daha dayanıklı ve daha ölçeklenebilir hale getirir.

İçin [Azure App Service](../app-service/app-service-value-prop-what-is.md)seçin bir [App Service planı](../app-service/overview-hosting-plans.md) , birden çok örneği sunar. Azure Cloud Services için her rolünüz kullanmak için yapılandırma [birden çok örneği](../cloud-services/cloud-services-choose-me.md). İçin [Azure sanal makineler](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), sanal makine (VM) Mimarinizi birden fazla VM içerir ve her VM'nin dahil olun bir [kullanılabilirlik kümesi](../virtual-machines/virtual-machines-windows-manage-availability.md). Kullanmanızı öneririz [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) otomatik ölçeklendirme özellikleri için.

### <a name="defense-in-depth"></a>Derinlemesine savunma

Derinlemesine savunma arkasındaki farklı savunma stratejileri kullanarak riskleri yönetmek için olur. Bir uygulamada güvenlik savunmaları katmanlama başarılı bir saldırı olasılığını azaltır. Azure platformunun yerleşik özellikleri kullanarak, uygulamalarınız için güvenli tasarımlar uygulamanız önerilir.

Örneğin, saldırı riskini boyutla artar (*yüzey alanı*) uygulama. Kullanıma sunulan IP adres alanı kapatmak için beyaz listeye ekleme özelliğini kullanarak ve dinleme load balancer'ları üzerinde gerekli olmayan bağlantı noktaları'nın yüzey alanını azaltabilirsiniz ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) ve [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)). [Ağ güvenlik grupları (Nsg'ler)](../virtual-network/security-overview.md) saldırı yüzeyini azaltmak için başka bir yoludur.
Kullanabileceğiniz [hizmet etiketleri](../virtual-network/security-overview.md#service-tags) ve [uygulama güvenlik grupları](../virtual-network/security-overview.md#application-security-groups) güvenlik kuralları oluşturmadan ve ağ güvenliğini uygulamanın yapısının doğal bir uzantısı yapılandırmak için karmaşıklığını en aza indirmek için.

Azure hizmetlerini dağıtmanız gerekir bir [sanal ağ](../virtual-network/virtual-networks-overview.md) mümkün olduğunda. Bu yöntem, özel IP adresleri aracılığıyla iletişim kurmak hizmet kaynakları sağlar. Azure hizmet trafiği sanal ağınızdan genel IP adresleri, varsayılan olarak kaynak IP adresleri olarak kullanır. Kullanarak [hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) hizmet trafiği kaynak IP adresleri olarak sanal ağ özel adreslerini bir sanal ağdan Azure hizmetine erişirken kullanılacak geçer.

Genellikle müşterilerin şirket kaynakları Azure kaynaklarını birlikte Saldırıya uğrayan görüyoruz. Bir şirket içi ortamı Azure'da bağlanıyorsanız, şirket içi kaynaklara genel İnternet'e riskini en aza öneririz. İyi bilinen genel varlıklarınızı azure'da dağıtarak ölçek ve Azure'nın gelişmiş DDoS koruma özellikleri kullanabilirsiniz. Bu genel olarak erişilebilir varlıklar genellikle DDoS saldırıları için hedef olduğundan, bunları Azure'da koyarak şirket içi kaynaklarınız üzerindeki etkiyi azaltır.

## <a name="azure-offerings-for-ddos-protection"></a>DDoS koruması için Azure teklifleri

Azure (Katman 3 ve 4) ağ saldırılarına karşı koruma sağlamak için iki DDoS hizmet teklifleri sahiptir: DDoS koruması temel ve DDoS koruma standardını. 

### <a name="ddos-protection-basic"></a>DDoS koruması temel

Temel korumayı, ek ücret ödemeden varsayılan olarak Azure'a tümleşiktir. Ölçeklendirme ve küresel çapta dağıtılan Azure ağ kapasitesini her zaman açık trafik izleme ve gerçek zamanlı azaltma aracılığıyla ortak ağ katmanı saldırılarına karşı koruma sağlar. DDoS koruması temel kullanıcı yapılandırması veya uygulama değişiklikleri gerekir. DDoS koruması temel Azure DNS gibi PaaS hizmetlerine, tüm Azure Hizmetleri korumaya yardımcı olur.

![Azure ağı gösterimini metin "Genel DDoS riskini azaltma varlığı" ve "DDoS riski azaltma kapasitesi önde gelen" ile eşleme](media/azure-ddos-best-practices/image3.png)

Temel Azure DDoS koruması, yazılım ve donanım bileşenden oluşur. Bir yazılım denetim düzlemi ne zaman karar verir, burada, ve analiz edin ve saldırı trafiği kaldırmak donanım Gereçleri trafiği türüne steered. Bir altyapı genelinde DDoS koruması göre bu kararı denetim düzlemi sağlar *ilke*. Bu ilke statik olarak ayarlayın ve tüm Azure müşterilerimiz için evrensel olarak uygulanır.

Örneğin, DDoS koruma İlkesi hangi trafik hacmi koruması gerektiğini belirtir *tetiklendi.* (Diğer bir deyişle, kiracının trafiği cihazları temizleme aracılığıyla yönlendirilmesi.) İlke sonra temizleme cihazları nasıl gerektiğini belirtir *azaltmak* saldırı.

Temel Azure DDoS koruması Hizmeti altyapısının koruması ve koruma Azure platformunun yöneliktir. Bu nedenle, çok müşterili bir ortamda birden çok müşteriyi etkileyebilecek önemli bir hızı aştığında, trafik azaltır. Uyarı sağlamaz veya müşteri başına ilkeleri özelleştirilebilir.

### <a name="ddos-protection-standard"></a>DDoS koruma standardını

Standart koruma, Gelişmiş DDoS riskini azaltma özellikleri sağlar. Ayrıca, bir sanal ağdaki belirli Azure kaynaklarınızı korumaya yardımcı olmak için otomatik olarak ayarlanmıştır. Koruma, yeni veya mevcut bir sanal ağ'ı etkinleştirmek basittir ve herhangi bir uygulama veya kaynak değişiklik gerektirmez. Bu günlük, uyarı ve telemetri gibi temel hizmet üzerinde çeşitli avantajları vardır. Aşağıdaki bölümlerde, standart Azure DDoS koruması hizmeti temel özelliklerini özetler.

#### <a name="adaptive-real-time-tuning"></a>Gerçek zamanlı Uyarlamalı ayarlama

Temel Azure DDoS koruması hizmeti, müşterilere korumak ve diğer müşterilere etkileri önlemek yardımcı olur. Örneğin, daha küçük olan yasal gelen trafiğin tipik bir birim için sağlanan bir hizmet, *tetikleyici oranı* altyapı genelinde DDoS koruması İlkesi, müşterinin kaynakları bir DDoS saldırısı geçebilir. gözden kaçan. Daha genel olarak, her müşteri için özelleştirilmiş koruma ilkelerini son saldırılarına (örneğin, çok vektör DDoS) karmaşıklığına ve kiracıların uygulamaya özgü davranışları çağırın. Hizmet, iki ınsights'ı kullanarak bu özelleştirme gerçekleştirir:

- Müşteri başına (IP başına) trafiği desenlerini otomatik learning Katman 3 ve 4 için.

- Azure ölçek, önemli miktarda trafiği devralarak sağlayan dikkate alarak, hatalı pozitif sonuçları en aza indirme.

![Standart DDoS koruması, "İlke daire içinde ile oluşturma" işleyişi diyagramı](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>DDoS koruması telemetri, izleme ve uyarı

DDoS koruması standart sunan zengin telemetri aracılığıyla [Azure İzleyici](../azure-monitor/overview.md) bir DDoS saldırısının süresi. DDoS koruması kullanan Azure İzleyici ölçümleri hiçbiri için uyarıları yapılandırabilirsiniz. Azure İzleyici tanılama arabirimi aracılığıyla Gelişmiş analiz için Splunk (Azure Event Hubs), Azure İzleyici günlüklerine ve Azure depolama ile günlüğe kaydetme tümleştirebilirsiniz.

##### <a name="ddos-mitigation-policies"></a>DDoS riskini azaltma ilkeleri

Azure portalında **İzleyici** > **ölçümleri**. İçinde **ölçümleri** bölmesinde kaynak grubunu seçin, bir kaynak türünü seçin **genel IP adresi**, Azure genel IP adresi seçin. DDoS ölçümleri görünür **kullanılabilir ölçümler** bölmesi.

DDoS koruması standart DDoS etkin olan sanal ağ alanındaki korumalı kaynağın her genel IP için üç ayarı azaltma ilkeleri (TCP SYN, TCP ve UDP) uygular. Ölçüm'ı seçerek ilke eşikleri görüntüleyebilirsiniz **gelen DDoS saldırılarının tetiklemek için paket**.

![Kullanılabilir Ölçümler ve ölçüm grafiği](media/azure-ddos-best-practices/image7.png)

Machine learning tabanlı ağ trafiği profil oluşturma aracılığıyla otomatik olarak yapılandırılan ilke eşikleri var. Yalnızca ilke eşiği aşıldığında, DDoS saldırılarının saldırı altında bir IP adresi için gerçekleşir.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>DDoS saldırı altında bir IP adresi için ölçüm

Genel IP adresi altında ise saldırı, ölçüm için değer **altında DDoS saldırı veya** azaltma saldırı trafik, DDoS koruması gerçekleştirirken 1 olarak değiştirir.

!["Veya altında DDoS saldırı" ölçüm ve grafik](media/azure-ddos-best-practices/image8.png)

Bu ölçüm hakkında bir uyarı yapılandırma öneririz. Genel IP adresi üzerinde gerçekleştirilen etkin bir DDoS saldırılarının olduğunda, ardından bildirilir.

Daha fazla bilgi için [yönetme Azure DDoS koruması Azure portalını kullanarak standart](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="web-application-firewall-for-resource-attacks"></a>Web uygulaması güvenlik duvarı kaynak saldırıları için

Kaynak saldırıları uygulama katmanında belirli bir web uygulaması Güvenlik Duvarı (WAF) web uygulamalarının güvenliğini sağlayacak yapılandırmanız gerekir. Bir WAF block SQL eklemelerini, siteler arası betik, DDoS ve diğer 7. Katman saldırıları için gelen web trafiğini inceler. Azure sağlar [WAF uygulama ağ geçidi özelliği olarak](../application-gateway/application-gateway-web-application-firewall-overview.md) merkezi koruma web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları için. Kullanılabilir diğer WAF teklifleri gereksinimlerinize daha uygun olabilir Azure iş ortaklarının sunduğu [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Hatta web uygulaması güvenlik duvarları volumetric ve eyalet tükenmesi saldırılarına açıktır. Gelen volumetric korunmasına yardımcı olmak için WAF sanal ağı DDoS koruma standart etkinleştirme öneririz ve protokolü saldırıları. Daha fazla bilgi için [DDoS koruması başvuru mimarileri](#ddos-protection-reference-architectures) bölümü.

### <a name="protection-planning"></a>Korumayı planlama

Planlama ve hazırlama sırasında bir DDoS saldırısının bir sistem nasıl gerçekleştirilir anlamak önemlidir. Bir olay Yönetimi yanıt planı tasarlama bu parçasıdır.

DDoS koruması standart varsa, internet'e yönelik uç noktaları sanal ağda etkin olduğundan emin olun. DDoS uyarıları yapılandırma, tüm olası saldırıları altyapınızda için sürekli olarak izleyin yardımcı olur. 

Uygulamalarınızı birbirinden bağımsız olarak izlemeniz gerekir. Normal bir uygulama davranışını anlayın. Uygulama bir DDoS saldırısının sırasında beklendiği gibi davranmıyorsa, yapacak hazırlayın.

#### <a name="testing-through-simulations"></a>Simülasyonlar ile test etme

Dönemsel benzetimleri yürütülerek hizmetlerinizi saldırılara nasıl yanıtlar hakkında varsayımlar test etmek için iyi bir uygulamadır. Sınama sırasında hizmetlerin ve uygulamaların beklendiği gibi çalışmaya devam eder ve kullanıcı deneyiminde hiçbir kesinti olduğunu doğrulayın. Bir teknoloji ve işlem açısından boşlukları ve DDoS yanıt stratejide dahil edilip derecelendirilir. Hazırlık ortamları veya üretim ortamına etkisini en aza indirmek için yoğun olmayan saatlerde böyle testleri gerçekleştirilmesi önerilir.

İle kurdu [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) burada Azure müşterilerine oluşturabileceği DDoS koruması etkinleştirilmiş genel uç noktaları simülasyonlar için trafiği bir arabirim oluşturmak için. Kullanabileceğiniz [BreakingPoint Cloud](https://www.ixiacom.com/products/breakingpoint-cloud) benzetimi için:

- Azure DDoS Koruması'nın DDoS saldırılarına karşı Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu doğrulayın.

- Olay yanıtlama işleminize DDoS saldırıya iyileştirin.

- DDoS uygunluk belgeleyin.

- Ağ güvenlik gruplarını eğitin.

Siber güvenlik savunma sürekli yeniliğe gerektirir. Azure DDoS standart bir durumu-giderek daha karmaşık DDoS saldırıları azaltmak için maliyetli bir çözüm teklifi ürünü korumadır.

## <a name="components-of-a-ddos-response-strategy"></a>Bir DDoS yanıt stratejisi bileşenleri

Azure kaynaklarını genellikle hedefleyen bir DDoS saldırısının kullanıcı açısından en az müdahale gerektirir. Yine de ekleme DDoS azaltma bir olay yanıtı stratejisinin bir parçası olarak iş sürekliliği etkisini en aza yardımcı olur.

### <a name="microsoft-threat-intelligence"></a>Microsoft Tehdit Bilgileri

Microsoft, bir geniş tehdit zekası ağ sahiptir. Bu ağ, Microsoft Çevrimiçi Hizmetleri, Microsoft iş ortakları ve ilişkiler Internet güvenlik topluluğu içinde destekleyen bir genişletilmiş güvenlik topluluğunun kolektif bilgi kullanır. 

Kritik altyapı sağlayıcısı olarak, Microsoft tehditler hakkında erken uyarı alır. Microsoft çevrimiçi hizmetlerini ve küresel müşteri tabanına tehdit bilgileri toplar. Microsoft Azure DDoS koruması ürününde tüm bu tehdit bilgilerini içerir.

Ayrıca, Microsoft dijital Suçlar birimi (DCU), rahatsız edici stratejileri botnet karşı gerçekleştirir. Botnet komut ve denetim DDoS saldırıları için ortak bir kaynaktır.

### <a name="risk-evaluation-of-your-azure-resources"></a>Azure kaynaklarınızın risk değerlendirme

Bir DDoS saldırısının düzenli olarak, risk anlamak için zorunludur. Düzenli aralıklarla kendinize sorun: 
- Hangi yeni genel kullanıma açık Azure kaynaklarını koruması gerekir?

- Tek bir hizmetteki bir hata noktası var mı? 

- Nasıl hizmetleri hala Hizmetleri geçerli kullanıcılara kullanılabilir yapma sırasında bir saldırının etkilerini sınırlamak için yalıtılmış olabilir mi?

- Sanal ağlar nerede standart DDoS koruma etkinleştirilmelidir vardır, ancak değil mi? 

- My Hizmetleri aktif/aktif yük devretme ile birden çok bölgede misiniz?

### <a name="customer-ddos-response-team"></a>Müşteri DDoS yanıtı ekibi

DDoS yanıtı ekibi oluşturma, saldırı hızla ve etkili bir şekilde yanıt olarak önemli bir adımdır. Kuruluşunuzda planlama ve yürütme işleten kişileri tanımlayın. Bu DDoS yanıtı ekibi, standart Azure DDoS koruması hizmeti iyice anlamanız gerekir. Takım tanımlayabilir ve Microsoft destek ekibi gibi iç ve dış müşteriler ile iletişime geçerek bir saldırının etkisini emin olun.

DDoS yanıt takımınız için hizmet kullanılabilirliği ve sürekliliği planlama normal bir parçası olarak benzetimi alıştırmaları kullanmanızı öneririz. Bu alıştırmalara ölçek testi içermelidir.

### <a name="alerts-during-an-attack"></a>Bir saldırı sırasında uyarılar

Azure DDoS koruması standart tanımlar ve DDoS saldırıları kullanıcı müdahalesi olmadan azaltır. Korumalı bir genel IP için etkin bir risk azaltma sunulduğunda bildirim almak için [bir uyarı yapılandırmak](../virtual-network/ddos-protection-manage-portal.md) ölçüm üzerinde **altında DDoS saldırı veya**. Diğer DDoS ölçümleri ölçek saldırı, bırakılan trafik ve diğer ayrıntıları öğrenmek için uyarı oluşturmayı seçebilirsiniz.

#### <a name="when-to-contact-microsoft-support"></a>Ne zaman Microsoft desteğine başvurun

- Bir DDoS saldırılarının sırasında korunan kaynak performansını ciddi bir şekilde düşürülmüş veya kaynak kullanılamıyor bulun.

- DDoS koruması hizmeti beklendiği gibi davranmıyorsa düşündüğünüz. 

  DDoS koruması hizmeti yalnızca azaltma başlatır ölçüm değeri **DDoS riskini azaltma (TCP/TCP SYN/UDP) tetiklemek için ilke** korumalı genel IP kaynağı üzerinde alınan trafik düşüktür.

- Ağ trafiğinizi önemli ölçüde artıracak bir viral olay planlamanız durumunda.

- Bir aktör, kaynaklarınızın bir DDoS saldırısı başlatmak tehdit.

Bir iş açısından kritik etkisi saldırıları oluşturmak için bir önem derecesi A [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Saldırı sonrası adımlar

Bir durum incelemesi sonra bir saldırı yapmak ve DDoS yanıt stratejisi gerektiği gibi ayarlamak için her zaman iyi bir stratejidir. Göz önünde bulundurulması gerekenler:

- Olan vardır hizmet veya kullanıcı için herhangi bir kesinti deneyimi için ölçeklenebilir mimari olmaması nedeniyle?

- En çok hangi uygulamaların veya hizmetlerin çalışmaya başlayamıyorsa?

- DDoS yanıt stratejisi ne kadar verimli ve nasıl geliştirilebilir?

DDoS saldırı altında işiniz şüpheleniyorsanız, normal bir Azure destek kanalları yöneticinize iletin.

## <a name="ddos-protection-reference-architectures"></a>DDoS koruması başvuru mimarileri

DDoS koruması standart tasarlanmıştır [bir sanal ağda dağıtılmış hizmetlerin](../virtual-network/virtual-network-for-azure-services.md). Varsayılan DDoS koruması temel hizmet diğer hizmetler için geçerlidir. Aşağıdaki başvuru mimarilerine senaryoları tarafından gruplandırılmış mimarisi desenleri ile düzenlenir.

### <a name="virtual-machine-windowslinux-workloads"></a>Sanal makine (Windows/Linux) iş yükleri

#### <a name="application-running-on-load-balanced-vms"></a>Yük dengeli VM'ler üzerinde çalışan uygulama

Bu başvuru mimarisi, yük dengeleyicinin arkasındaki kullanılabilirliğini ve ölçeklenebilirliğini artırmak için bir ölçek kümesinde birden fazla Windows VM çalıştırmaya yönelik kendini kanıtlamış bir dizi gösterir. Bu mimari, bir web sunucusu gibi durum bilgisi olmayan tüm iş yükleri için kullanılabilir.

![Yük dengeli VM'ler üzerinde çalışan bir uygulama başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image9.png)

Bu mimaride, bir iş yükü birden çok VM örneğine dağıtılır. Tek bir genel IP adresi ve internet trafiği bir yük dengeleyici üzerinden vm'lere dağıtılır. DDoS koruması standart genel IP ile ilişkili olan (internet) Azure load balancer'ın sanal ağ üzerinde etkindir.

Yük Dengeleyici, gelen internet isteklerini VM örneklerine dağıtır. Sanal makine ölçek kümeleri daraltılmasına veya için sanal makinelerin sayısını elle veya otomatik olarak önceden tanımlanmış kurallara göre izin verin. Bu kaynak DDoS saldırı altında ise önemlidir. Bu başvuru mimarisi hakkında daha fazla bilgi için bkz. [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm).

#### <a name="application-running-on-windows-n-tier"></a>Windows N-katmanlı üzerinde çalışan uygulama

N katmanlı mimari uygulamanın birçok yolu vardır. Aşağıdaki diyagram tipik bir üç katmanlı web uygulaması gösterir. Bu mimari makale yapılar [ölçeklenebilirlik ve kullanılabilirlik için yük dengeli VM'ler çalıştırma](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). Web ve iş katmanları yük dengeli VM’leri kullanır.

![Windows N-katmanlı üzerinde çalışan bir uygulama başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image10.png)

Bu mimaride, DDoS koruması standart sanal ağ üzerinde etkindir. Tüm genel IP'ler sanal ağ, DDoS koruması için Katman 3 ve 4 alın. Katman 7 koruması için Application Gateway WAF SKU'suna dağıtın. Bu başvuru mimarisi hakkında daha fazla bilgi için bkz. [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier).

#### <a name="paas-web-application"></a>PaaS web uygulaması

Bu başvuru mimarisinde, tek bir bölgede bir Azure App Service uygulamasını çalıştıran gösterilmektedir. Bu mimari kullanan bir web uygulaması için kendini kanıtlamış bir dizi gösterir [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) ve [Azure SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/).
Yedek bir bölgeye yük devretme senaryoları için ayarlanır.

![PaaS web uygulaması için başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image11.png)

Azure Traffic Manager, gelen istekleri bölgelerden birine Application gateway'e yönlendirir. Normal işlemler sırasında etkin bölgeyi uygulama ağ geçidi istekleri yönlendirir. Bu bölge kullanılamaz duruma gelirse Traffic Manager Application gateway'e bekleme bölgede devreder.

Web uygulamasına giden İnternet'ten gelen tüm trafik yönlendirilir [uygulama ağ geçidi genel IP adresi](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) Traffic Manager aracılığıyla. Bu senaryoda, app service (web uygulaması) doğrudan olmayan harici olarak bakan ve uygulama ağ geçidi tarafından korunur. 

Application Gateway WAF SKU yapılandırmanızı öneririz (modu engellemek) 7. Katman (HTTP/HTTPS/WebSocket) saldırılarına karşı korumaya yardımcı olmak için. Ayrıca, web uygulamaları için yapılandırılmış olan [yalnızca uygulama ağ geçidine gelen trafiği kabul](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP adresi.

Bu başvuru mimarisi hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

### <a name="mitigation-for-non-web-paas-services"></a>Risk azaltma için web olmayan PaaS Hizmetleri

#### <a name="hdinsight-on-azure"></a>Azure üzerinde HDInsight

Bu başvuru mimarisi, DDoS koruması standart için yapılandırma gösterir bir [Azure HDInsight küme](https://docs.microsoft.com/azure/hdinsight/). DDoS koruması sanal ağ üzerinde etkin olduğunu ve HDInsight kümesi bir sanal ağa bağlı olduğundan emin olun.

!["HDInsight" ve "Gelişmiş ayarları" sanal ağ ayarları bölmesi](media/azure-ddos-best-practices/image12.png)

![DDoS koruması etkinleştirmek için seçimi](media/azure-ddos-best-practices/image13.png)

Bu mimaride, HDInsight kümesi için internet'ten hedefleyen trafiği HDInsight ağ geçidi yük dengeleyiciyle ilişkili genel IP yönlendirilir. Ağ geçidi yük dengeleyicinin ardından trafiği baş düğümleri veya çalışan düğümlerinin doğrudan gönderir. DDoS koruması standart HDInsight sanal ağ üzerinde etkin olmadığından sanal ağdaki tüm genel IP'ler Katman 3 ve 4 için DDoS koruması alın. Bu başvuru mimarisi, N katmanlı ve çok bölgeli başvuru mimarileri ile birleştirilebilir.

Bu başvuru mimarisi hakkında daha fazla bilgi için bkz. [Azure HDInsight kullanarak bir Azure sanal ağ genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) belgeleri.


> [!NOTE]
> Azure App Service ortamı için PowerApps ya da API yönetimi bir sanal ağdaki bir genel IP ile yerel olarak desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DDoS koruması ürün sayfası](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS koruması blogu](https://aka.ms/ddosblog)

* [Azure DDoS koruması belgeleri](../virtual-network/ddos-protection-overview.md)
