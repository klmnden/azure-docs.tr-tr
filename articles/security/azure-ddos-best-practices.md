---
title: Azure DDoS koruması en iyi yöntemler ve başvuru mimarileri | Microsoft Docs
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
ms.openlocfilehash: 4fb0eb3dd3349bd901850d6b9dd0f3e33ee2e0d7
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-ddos-protection-best-practices-and-reference-architectures"></a>Azure DDoS koruması: en iyi yöntemler ve mimarileri başvuru

Bu makalede, BT karar alıcılar ve güvenlik personel içindir. Azure ile tanıdık bekliyor ağ ve güvenlik.

Hizmet (DDoS) dağıtılmış reddi tasarlama, planlama ve tasarlama modu hatası çeşitli için dayanıklılık gerektirir. Bu makale Azure uygulamalarında DDoS saldırılarına karşı dayanıklılık için tasarlamak için en iyi yöntemler sağlar.

## <a name="types-of-attacks"></a>Tür saldırılar

DDoS uygulama kaynaklarını tüketebilir dener saldırı türüdür. Uygulamanın kullanılabilirliğini ve yasal isteklerini işlemek için kendi yeteneği etkilemek için belirtilir. Saldırıları, daha karmaşık ve boyuta ve etkisi daha büyük hale gelmektedir. DDoS saldırıları, Internet üzerinden genel olarak erişilebilen herhangi bir uç noktada hedeflenebilir.

Azure DDoS saldırılara karşı sürekli koruma sağlar. Bu koruma, varsayılan olarak ve Hayır Azure platformuyla doğrudan tümleşik olduğu ekstra maliyet. 

DDoS koruması Platform çekirdek yanı sıra [Azure DDoS koruması standart](https://azure.microsoft.com/services/ddos-protection/) ağ saldırılarına karşı Gelişmiş DDoS azaltma özellikleri de sağlar. Belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanmıştır. Koruma, yeni sanal ağlar oluşturma sırasında etkinleştirmek basit bir işlemdir. Oluşturulduktan sonra da yapılabilir ve herhangi bir uygulama veya kaynak değişiklik gerektirmez.

![Bir saldırganın müşteriler ve sanal ağ korumaya rol Azure DDoS koruması](media/azure-ddos-best-practices/image1.png)

DDoS saldırıları üç kategoride sınıflandırılmış: volumetric, protokol ve kaynak.

### <a name="volumetric-attacks"></a>Volumetric saldırıları

Volumetric saldırıları DDoS saldırısının en yaygın türüdür. Volumetric saldırıları ağ ve aktarım katmanları hedef yanılma assaults ' dir. Ağ bağlantıları gibi kaynaklarını tüketebilir deneyin. 

Bu saldırıların birden çok etkilenen sistemleri gibi görünen yasal trafiği ağ katmanlarıyla bölgesini doldurmak için çoğunlukla kullanır. Internet Denetim İletisi Protokolü (ICMP), kullanıcı veri birimi Protokolü (UDP) ve İletim Denetimi Protokolü (TCP) gibi ağ katmanı protokollerini kullanırlar.

En yaygın kullanılan ağ katmanı DDoS saldırıları TCP, ICMP Yankı, UDP, DNS, taşmasını taşmasını Eşitlemeye olan ve NTP yükseltme saldırılarını. Bu tür saldırılar, yalnızca hizmet, aynı zamanda daha alınan ve hedeflenen ağ yetkisiz erişim için bir smokescreen olarak kesintiye için kullanılabilir. Son volumetric saldırı örneğidir [Memcached yararlanma](https://www.wired.com/story/github-ddos-memcached/) GitHub etkilenen. Bu saldırı, UDP bağlantı noktası 11211 hedeflenen ve 1,35 Tb/s saldırı birimin oluşturulur.

### <a name="protocol-attacks"></a>Protokol saldırıları

Protokol hedef uygulama protokolleri saldırılarından. Güvenlik duvarları, uygulama sunucuları gibi altyapı cihazlarının tüm kullanılabilir kaynakları kullanmak ve yük Dengeleyiciler deneyin. Protokol saldırıları hatalı biçimlendirilmiş ya da Protokolü anormal durumları içeren paketi kullanın. Bu tür saldırıları, çok sayıda sunucuları ve diğer iletişim aygıtları yanıt ve bir paket yanıtı bekleyin açık isteği göndererek çalışır. Hedef sonunda sistem çökmesine neden açık isteklere yanıt verecek şekilde çalışır.

En yaygın bir protokol tabanlı DDoS saldırısının TCP Eşitlemeye sel örnektir. Bu saldırı, art arda TCP Eşitlemeye istekleri bir hedef doldurmaya çalışır. Hedef hedef yanıt veremez hale getirmektir. TCP Eşitlemeye oluşmuştur uygulama katmanı saldırının olması dışında 2016 Din kesinti Din'in DNS sunucularının hedeflenen Bu bağlantı noktası 53 şişirir.

### <a name="resource-attacks"></a>Kaynak saldırıları

Kaynak saldırıları uygulama katmanı hedefleyin. Bunlar, bir sistem doldurmaya için arka uç işlemleri tetikler. Kaynak İzleme CPU-yoğun sunucuya sorgular ancak bu, normal görünen kötüye trafiği saldırıları. Kaynakları tüketmesine gerekli trafik hacmi, diğer tür saldırıların düşüktür. Bir kaynak saldırısı olarak algılamak sabit yapmadan yasal trafiğinden ayırt trafiğidir. En yaygın kaynak saldırıları HTTP/HTTPS ve DNS hizmetleri gerekir.

## <a name="shared-responsibility-in-the-cloud"></a>Bulutta paylaşılan sorumluluk

Savunma stratejisi açıdan çok yönlülük saldırıları ve artan çeşitli mücadele etmek yardımcı olur. Güvenlik, müşteri ve Microsoft arasında paylaşılan bir sorumluluğundadır. Microsoft bu çağıran bir [paylaşılan sorumluluk modeli](https://azure.microsoft.com/blog/microsoft-incident-response-and-shared-responsibility-for-cloud-computing/). Aşağıdaki şekilde bu bölme sorumluluk gösterilmektedir:

![Müşterinin ve Azure sorumlulukları](media/azure-ddos-best-practices/image2.png)

Microsoft en iyi uygulamaları gözden geçirme Azure müşterilerine yararlanabilir ve genel derleme tasarlanmış ve test için hatası uygulamalar dağıtılır.

## <a name="fundamental-best-practices"></a>Temel en iyi uygulamalar

Aşağıdaki bölümlerde Azure üzerinde DDoS dayanıklı hizmetleri oluşturmak için belirleyici rehberlik sağlar.

### <a name="design-for-security"></a>Güvenlik için tasarlama

Güvenlik tasarımı ve uygulama dağıtım ve işlemleri'ten bir uygulamanın tüm yaşam döngüsü boyunca bir öncelik olduğundan emin olun. Uygulama kaynakları, bir hizmet kesintisi kaynaklanan Ölçüsüz kullanmak için istekleri nispeten düşük hacmi izin hatalar sahip olamaz. 

Microsoft Azure üzerinde çalışan bir hizmete korunmasına yardımcı olmak için uygulama mimarisinin iyi anlamış olmanız ve gerekir odaklanmak [yazılım kalitesi beş ayaklar](https://docs.microsoft.com/azure/architecture/guide/pillars).
Tipik trafiği birimler, uygulama ve diğer uygulamalar ve ortak internet'e açık hizmet uç noktaları arasında bağlantı modeli bilmeniz gerekir.

Bir uygulama bir uygulaması, hedeflenen hizmet reddi işlemek için esnek olduğundan emin olmanın çok önemlidir. Güvenlik ve gizlilik yerleşik olarak bulunur itibaren Azure platformu [güvenlik geliştirme yaşam döngüsü (SDL)](https://www.microsoft.com/sdl/default.aspx). SDL her geliştirme aşamada güvenlik adreslerini ve Azure daha güvenli hale getirmek için sürekli olarak güncelleştirilir sağlar.

### <a name="design-for-scalability"></a>Ölçeklenebilirlik sağlamak için Tasarım

Ölçeklenebilirlik bir sistem artan yükü işlemek ne kadar iyi olur. Uygulamalarınıza tasarlamanız gerekir [yatay ölçek](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) DDoS saldırı durumunda özellikle yükseltilmiş bir yük talebi karşılamak üzere. Uygulamanızın tek bir hizmet örneği üzerinde bağımlı olması durumunda, tek bir hata noktası oluşturur. Birden çok örneği sağlama sisteminizi daha esnek ve daha ölçeklenebilir hale getirir.

İçin [Azure App Service](../app-service/app-service-value-prop-what-is.md)seçin bir [uygulama hizmeti planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) birden çok örneği sunar. Azure bulut Hizmetleri için kullanmak için rollerinin her biri yapılandırma [birden çok örneği](../cloud-services/cloud-services-choose-me.md). İçin [Azure sanal makineleri](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), sanal makine (VM) Mimarinizi birden fazla VM içerir ve her VM bulunduğundan emin olun bir [kullanılabilirlik kümesi](../virtual-machines/virtual-machines-windows-manage-availability.md). Kullanmanızı öneririz [sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) otomatik ölçeklendirmeyi özellikleri.

### <a name="defense-in-depth"></a>Derinlemesine

, Derinlemesine arkasında farklı savunma stratejileri kullanarak riskleri yönetmek için olur. Bir uygulamada güvenlik savunmaları katmanlama başarılı bir saldırı olasılığını azaltır. Azure platformu yerleşik özellikleri kullanarak, uygulamalarınız için güvenli tasarımlar uygulamanız önerilir.

Örneğin, boyutuyla saldırı riskini artırır (*yüzey alanını*) uygulamasının. Uygulamaları güvenilir listeye almayı sunulan IP adres alanı kapatmak için kullanarak ve yük dengeleyici üzerinde gerekli olmayan bağlantı noktalarını dinler yüzey alanını azaltabilirsiniz ([Azure yük dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md) ve [Azure uygulama ağ geçidi](../application-gateway/application-gateway-create-probe-portal.md)). [Ağ güvenlik grupları (Nsg'ler)](../virtual-network/security-overview.md) saldırı yüzeyini azaltmak için başka bir yoludur.
Kullanabileceğiniz [hizmet etiketleri](/virtual-network/security-overview.md#service-tags) ve [uygulama güvenlik grupları](/virtual-network/security-overview.md#application-security-groups) güvenlik kuralları oluşturma ve bir uygulamanın yapısı doğal bir uzantısı olarak ağ güvenliği yapılandırma karmaşıklık en aza indirmek için.

Azure hizmetlerinde dağıtmalısınız bir [sanal ağ](../virtual-network/virtual-networks-overview.md) mümkün olduğunda. Bu yöntem, özel IP adresleri ile iletişim kurmak hizmet kaynakları sağlar. Bir sanal ağ trafiğinden Azure hizmeti genel IP adresleri, varsayılan olarak kaynak IP adresleri olarak kullanır. Kullanarak [hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) sanal ağdan Azure hizmeti erişirken sanal ağ özel adresler kaynak IP adreslerini kullanmak için hizmeti trafiğinin geçiş yapar.

Biz, müşterilerin içi Azure kaynaklarını birlikte Saldırıya uğrayan kaynaklar genellikle bakın. Azure için bir şirket içi ortamda bağlanıyorsanız, şirket içi kaynakları riskini genel İnternet en aza öneririz. İyi bilinen ortak varlıklarınızı Azure dağıtarak ölçek ve Azure Gelişmiş DDoS koruması özelliklerini kullanabilirsiniz. Genel olarak erişilebilir varlıkları genellikle DDoS saldırıları için bir hedefi olduğundan, Azure'da koyma, şirket içi kaynaklar üzerindeki etkisini azaltır.

## <a name="azure-offerings-for-ddos-protection"></a>DDoS koruması için Azure teklifleri

Azure, ağ saldırılarına karşı (Katman 3 ve 4) koruma sağlayan iki DDoS hizmet teklifleri sahiptir: DDoS koruması temel ve DDoS koruması standart. 

### <a name="ddos-protection-basic"></a>DDoS koruması Basic

Temel koruma, varsayılan olarak hiçbir ek ücret ödemeden Azure'da tümleşiktir. Ölçek ve genel olarak dağıtılan Azure ağ kapasitesini her zaman açık trafiği izleme ve gerçek zamanlı azaltma aracılığıyla ortak ağ katmanı saldırılara karşı koruma sağlar. DDoS koruması temel yapılandırma veya uygulama herhangi bir kullanıcı değişiklik gerektirmez. DDoS koruması temel Azure DNS gibi PaaS Hizmetleri dahil olmak üzere tüm Azure hizmetlerinde korunmasına yardımcı olur.

![Azure ağı gösterimini metin "Genel DDoS azaltma varlığı" ve "DDoS azaltma kapasite baştaki" ile eşleme](media/azure-ddos-best-practices/image3.png)

Azure'da temel DDoS koruması yazılım ve donanım bileşenden oluşur. Ne zaman yazılım denetim düzlemi karar, burada, ve trafik türünü çözümlemek ve saldırı trafiği kaldıran donanım cihazları steered. Bir altyapı genelinde DDoS koruması göre bu karara denetim düzlemi yapar *İlkesi*. Bu ilke statik olarak ayarlayın ve evrensel tüm Azure müşterilerine uygulanır.

Örneğin, hangi trafik birim koruma olmalıdır DDoS koruması ilkesi belirtir *tetiklendi.* (Diğer bir deyişle, kiracının trafiği cihazları temizleme üzerinden yönlendirileceğini.) İlke sonra nasıl temizleme cihazları belirtir *azaltmak* saldırı.

Azure DDoS koruması temel Hizmeti altyapısının koruma ve Azure platformu korumasını yöneliktir. Çok kiracılı bir ortamdaki birden çok müşteri etkileyebilecek kadar önemli olduğu bir oran aştığında trafiğini azaltır. Uyarı sağlamaz veya müşteri başına ilkeleri özelleştirilebilir.

### <a name="ddos-protection-standard"></a>DDoS koruması standart

Standart koruma Gelişmiş DDoS azaltma özellikleri sağlar. Belirli Azure kaynaklarınızı sanal ağ korumaya yardımcı olmak için otomatik olarak ayarlanmıştır. Tüm yeni veya mevcut sanal ağda etkinleştirmek koruma basittir ve herhangi bir uygulama veya kaynak değişiklik gerektirmez. Günlüğe kaydetme, uyarı ve telemetri dahil olmak üzere temel hizmeti üzerinde çeşitli avantajları vardır. Aşağıdaki bölümlerde Azure DDoS koruması standart hizmeti temel özellikleri verilmiştir.

#### <a name="adaptive-real-time-tuning"></a>Uyarlamalı gerçek zamanlı ayarlama

Azure DDoS koruması temel hizmeti müşterilerin korunmasına ve diğer müşterilerin etkileri önlemek yardımcı olur. Örneğin, bir hizmet tipik bir birim değerinden küçük yasal gelen trafik için sağlanan *tetikleyici oranı* altyapı genelinde DDoS koruması İlkesi o Müşteri'nin kaynaklar üzerinde DDoS saldırı geçebilir bilgisi dışında. Daha fazla genel olarak, en son (örneğin, çok vektör DDoS) saldırıları karmaşıklığına ve kiracılar uygulamaya özgü davranışlarını başına-müşteri için özelleştirilmiş koruma ilkeleri arayın. Hizmet, iki Öngörüler kullanarak bu özelleştirme gerçekleştirir:

- Müşteri başına (her-IP) trafiği desenlerini otomatik learning için Katman 3 ve 4.

- Azure ölçeğini önemli miktarda trafiği almak için izin verdiğini dikkate hatalı pozitif sonuç en aza indirir.

![DDoS koruması standart, "İlke nesil yuvarlak içine alınmıştır" işleyişi diyagramı](media/azure-ddos-best-practices/image5.png)

#### <a name="ddos-protection-telemetry-monitoring-and-alerting"></a>DDoS koruması telemetri, izleme ve uyarma

DDoS koruması standart sunan aracılığıyla zengin telemetri [Azure İzleyici](/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) DDoS saldırısının süre. DDoS koruması kullanan Azure İzleyici ölçümleri hiçbirini uyarılarını yapılandırabilirsiniz. Azure İzleyici tanılama arabirimi aracılığıyla Gelişmiş analiz için Splunk (Azure Event Hubs), Azure günlük analizi ve Azure Storage ile günlük tümleştirebilirsiniz.

##### <a name="ddos-mitigation-policies"></a>DDoS azaltma ilkeleri

Azure portalında seçin **İzleyici** > **ölçümleri**. İçinde **ölçümleri** bölmesinde, kaynak grubunu seçin, bir kaynak türü **genel IP adresi**ve Azure genel IP adresi seçin. DDoS ölçümleri görünür **kullanılabilir ölçümler** bölmesi.

DDoS koruması standart her ortak IP korumalı kaynağının DDoS etkin olan sanal ağ için üç otomatik ayarlı azaltma ilkeleri (TCP Eşitlemeye, TCP ve UDP) uygulanır. Ölçüm seçerek ilke eşikleri görüntüleyebilirsiniz **gelen DDoS azaltma tetiklemek için paketler**.

![Kullanılabilir Ölçümler ve ölçümleri grafiği](media/azure-ddos-best-practices/image7.png)

Makine öğrenme tabanlı ağ trafiği profil aracılığıyla otomatik olarak yapılandırılan ilke eşiklere. Yalnızca ilke eşiği aşıldığında DDoS azaltma için bir IP adresi saldırıya gerçekleşir.

##### <a name="metric-for-an-ip-address-under-ddos-attack"></a>Bir IP adresi DDoS saldırıya ölçümü

Ortak IP adresi altında ise saldırısı, ölçüm değeri **altında DDoS saldırı veya** DDoS koruması saldırı trafiğini azaltma gerçekleştirirken 1 olarak değiştirir.

!["Altında DDoS veya saldırı" ölçüm ve grafik](media/azure-ddos-best-practices/image8.png)

Bu ölçüm üzerinde bir uyarı yapılandırma öneririz. Ortak IP adresi üzerinde gerçekleştirilen etkin bir DDoS azaltma olduğunda, ardından bildirilir.

Daha fazla bilgi için bkz: [yönetmek Azure DDoS koruması Azure portalını kullanarak standart](../virtual-network/ddos-protection-manage-portal.md).

#### <a name="web-application-firewall-for-resource-attacks"></a>Kaynak saldırıları için Web uygulaması güvenlik duvarı

Belirli kaynak saldırılara uygulama katmanında bir web uygulaması Güvenlik Duvarı (WAF) güvenli web uygulamaları yardımcı olmak için yapılandırmanız gerekir. Bir WAF SQL eklemelerini engellemek için siteler arası komut dosyası, DDoS ve diğer katman 7 saldırılara gelen web trafiği olup olmadığını denetler. Azure sağlar [WAF uygulama ağ geçidi, bir özellik olarak](../application-gateway/application-gateway-web-application-firewall-overview.md) web uygulamalarınızdan ortak açığından yararlanma girişimi ve güvenlik açıkları merkezi koruma. Diğer WAF teklifleri gereksinimlerinize daha uygun olabilir Azure ortaklardan kullanılabilir [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps?search=WAF&page=1).

Hatta web uygulaması güvenlik duvarı volumetric ve durum Tükenme saldırılarına açıktır. Gelen volumetric korunmasına yardımcı olmak için WAF sanal ağ üzerinde DDoS koruması standart etkinleştirilmesi önerilir ve protokolü saldırıları. Daha fazla bilgi için bkz: [DDoS koruması başvuru mimarileri](#ddos-protection-reference-architectures) bölümü.

### <a name="protection-planning"></a>Korumayı planlama

Planlama ve hazırlama bir sistem DDoS saldırı sırasında nasıl gerçekleştirecek anlamak önemli. Bir olay Yönetimi yanıt planı tasarlama bu çalışmaların parçasıdır.

DDoS koruması standart varsa, internet'e yönelik uç noktaları sanal ağda etkin olduğundan emin olun. DDoS uyarıları yapılandırma altyapınızda tüm olası saldırıları için sürekli izleme yardımcı olur. 

Uygulamalarınızı bağımsız olarak izlemeniz gerekir. Uygulamanın normal davranışını anlayın. Uygulama DDoS saldırı sırasında beklendiği gibi çalışmaz, davranacak şekilde hazırlayın.

#### <a name="testing-through-simulations"></a>Benzetimleri test etme

Dönemsel benzetimleri yürütülerek hizmetlerinizi saldırı nasıl yanıtlar hakkında varsayımları test etmek için iyi bir uygulamadır. Test sırasında hizmetlerin ve uygulamaların beklendiği gibi çalışmaya devam eder ve kullanıcı deneyimi için hiçbir bozulma olduğunu doğrulayın. Bir teknoloji ve işlem açısından çözebileceğiniz boşlukları tanımlar ve bunları DDoS yanıt stratejinize birleştirmenizi. Hazırlık ortamları veya üretim ortamına etkisini en aza indirmek için yoğun olmayan saatlerde böyle sınamalar gerçekleştirmenizi öneririz.

İle işbirliği [BreakingPoint bulut](https://www.ixiacom.com/products/breakingpoint-cloud) burada Azure müşterilerine üretebilir DDoS koruması etkinleştirilmiş için ortak uç noktaları benzetimleri göre trafiği bir arabirim oluşturmak için. Kullanabileceğiniz [BreakingPoint bulut](https://www.ixiacom.com/products/breakingpoint-cloud) benzetimi için:

- Azure DDoS Koruması'nın Azure kaynaklarınızın DDoS saldırılara karşı korunmasına nasıl yardımcı olduğunu doğrulayın.

- Olay yanıtlama işleminizi DDoS saldırıya iyileştirin.

- DDoS uyumluluk belge.

- Ağ güvenlik gruplarını eğitmek.

Siber güvenlik savunma içinde sabit yenilik gerektirir. Azure DDoS standart bir durumu,-giderek karmaşık DDoS saldırıları azaltmak için etkili bir çözüm sunan son korumadır.

## <a name="components-of-a-ddos-response-strategy"></a>Bir DDoS yanıt stratejisi bileşenleri

Azure kaynaklarını genellikle hedefleyen bir DDoS saldırı kullanıcı açısından en az müdahale gerektirir. Yine de, DDoS ekleme azaltma bir olay yanıtlama stratejisinin bir parçası olarak iş sürekliliği etkisini en aza indirmenize yardımcı olur.

### <a name="microsoft-threat-intelligence"></a>Microsoft tehdit bilgileri

Microsoft, kapsamlı tehdit Intelligence ağ sahiptir. Bu ağ, Microsoft online services, Microsoft iş ortakları ve Internet güvenlik topluluğu içinde ilişkileri destekleyen bir genişletilmiş güvenlik topluluğu toplu bilgilerini kullanır. 

Kritik altyapı sağlayıcısı olarak, Microsoft tehditler hakkında erken uyarı alır. Microsoft çevrimiçi hizmetlerini ve genel müşteri tabanı'ndan tehdit bilgileri toplar. Microsoft Azure DDoS koruması ürününde tüm bu tehdit bilgileri içerir.

Ayrıca, Microsoft dijital Suçlar birimi (DCU) botnets karşı rahatsız edici stratejileri gerçekleştirir. Botnets komut ve denetim DDoS saldırıları için ortak bir kaynaktır.

### <a name="risk-evaluation-of-your-azure-resources"></a>Azure kaynaklarınızı risk değerlendirmesi

riskini DDoS saldırılara karşı düzenli olarak kapsamı anlamak için zorunludur. Düzenli aralıklarla kendinize sorun: 
- Hangi yeni genel kullanıma açık Azure kaynaklarını koruma gerekiyor?

- Tek bir hata hizmet noktası var? 

- Nasıl hizmetleri hala Hizmetleri geçerli kullanıcılara kullanılabilir yapma çalışırken bir saldırının etkisini sınırlamak için ayrılmış olabilir mi?

- Burada DDoS koruması standart etkinleştirilmelidir sanal ağlar vardır, ancak değil mi? 

- My Hizmetleri etkin/etkin birden çok bölgeler arasında yük devretme kümelemesiyle misiniz?

### <a name="customer-ddos-response-team"></a>Müşteri DDoS yanıt ekibi

DDoS yanıt ekip oluşturma, saldırının için hızlı ve etkili bir şekilde yanıt olarak anahtar bir adımdır. Planlama ve yürütme işleten kuruluşunuzdaki kişiler tanımlayın. Bu DDoS yanıt takım Azure DDoS koruması standart hizmeti kapsamlı olarak anlamanız gerekir. Takım tanımlamak ve Microsoft destek ekibi dahil iç ve dış müşterileri ile iletişime geçerek saldırının etkisini emin olun.

DDoS yanıt ekibiniz için hizmet kullanılabilirliği ve sürekliliği planlama normal bir parçası olarak benzetimi alıştırmaları kullanmanızı öneririz. Bu alıştırmalarda ölçek testi içermesi gerekir.

### <a name="alerts-during-an-attack"></a>Bir saldırı sırasında uyarıları

Azure DDoS koruması standart tanımlar ve kullanıcı müdahalesi olmadan DDoS saldırıları azaltır. Korumalı bir genel IP için etkin bir Azaltıcı Etkenler olduğunda bildirim almak için yapabileceğiniz [bir uyarı yapılandırmak](../virtual-network/ddos-protection-manage-portal.md) ölçüm üzerinde **altında DDoS saldırı veya**. Saldırı, bırakılan trafik ve diğer ayrıntılar ölçeğini anlamak diğer DDoS ölçümler için uyarıları oluşturmayı seçebilirsiniz.

#### <a name="when-to-contact-microsoft-support"></a>Ne zaman Microsoft Destek'e başvurun

- DDoS saldırı sırasında korumalı kaynağa performansını önemli ölçüde azaltılmış veya kaynak kullanılamıyor bulun.

- DDoS koruması hizmeti beklendiği gibi çalışmaz düşünün. 

  Yalnızca azaltma DDoS koruması hizmeti başlatıldıktan ölçüm değeri **DDoS azaltma (TCP/TCP Eşitlemeye/UDP) tetiklemek için ilke** korumalı ortak IP kaynak alınan trafik düşüktür.

- Ağ trafiğinizi önemli ölçüde artırır virüslü bir olay planlamanız durumunda.

- Bir oyuncu kaynaklarınızı DDoS saldırısı başlatmak tehdit.

Kritik iş etkisine sahip saldırılar oluşturmak için önem derecesi A [destek bileti](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

### <a name="post-attack-steps"></a>Sonrası saldırı adımları

Saldırı sonrasındaki bir durum incelemesi yapın ve gerektiği gibi DDoS yanıt stratejisini ayarlamak için her zaman iyi bir stratejidir. Göz önünde bulundurmanız gerekenler:

- Edildi var. herhangi kesinti hizmetine veya kullanıcı deneyimi için ölçeklenebilir mimari yetersizliğinden?

- Hangi uygulamalar veya hizmetler en başlayamıyorsa?

- DDoS yanıt stratejisi nasıl etkili ve nasıl geliştirilebilir?

DDoS Saldırıya uğramış olduğunuz şüpheleniyorsanız, normal Azure destek kanallarınıza taşıyarak.

## <a name="ddos-protection-reference-architectures"></a>DDoS koruması başvuru mimarisi

DDoS koruması standart tasarlanmıştır [bir sanal ağda dağıtılmış hizmetlerin](../virtual-network/virtual-network-for-azure-services.md). Diğer hizmetler için varsayılan DDoS koruması temel hizmeti geçerlidir. Aşağıdaki başvuru mimarileri bir arada gruplandırılmış mimarisi desenlerle senaryolar tarafından düzenlenir.

### <a name="virtual-machine-windowslinux-workloads"></a>Sanal makine (Windows/Linux) iş yükleri

#### <a name="application-running-on-load-balanced-vms"></a>Yük dengeli VM'ler üzerinde çalıştırılan uygulama

Bu başvuru mimarisinin bir dizi birden çok Windows VM kullanılabilirliğini ve ölçeklenebilirliğini artırmak için bir yük dengeleyicinin arkasına ayarlanmış bir ölçek çalıştırmak için kanıtlanmış yöntemleri gösterir. Bu mimari, bir web sunucusu gibi durum bilgisiz tüm iş yükü için kullanılabilir.

![Yük dengeli Vm'lerinde çalışan bir uygulama için başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image9.png)

Bu mimaride, bir iş yükü birden çok VM örneğine dağıtılır. Tek bir ortak IP adresi vardır ve Internet trafiğini sanal makineleri bir yük dengeleyici aracılığıyla dağıtılır. DDoS koruması standart ilişkili genel IP sahip Azure (Internet) yük dengeleyici sanal ağda etkinleştirilir.

Yük Dengeleyici gelen Internet isteklerini VM örnekleri dağıtır. Sanal makine ölçek kümeleri el ile veya otomatik olarak önceden tanımlanmış kurallara göre giriş veya çıkış ölçeklendirilmesi VM'lerin sayısını sağlar. Bu kaynak DDoS Saldırıya uğramış durumdaysa önemlidir. Bu başvuru mimarisi hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm).

#### <a name="application-running-on-windows-n-tier"></a>Windows N katmanlı üzerinde çalışan uygulama

N katmanlı mimari uygulamanın birçok yolu vardır. Aşağıdaki diyagram tipik üç katmanlı web uygulaması gösterir. Bu mimarinin makale derlemeler [ölçeklenebilirlik ve kullanılabilirlik için yük dengeli sanal makineleri çalıştırmak](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). Web ve iş katmanları yük dengeli VM’leri kullanır.

![Windows N katmanlı üzerinde çalışan bir uygulama için başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image10.png)

Bu mimaride, sanal ağ üzerinde DDoS koruması standart etkinleştirilir. Sanal ağ içindeki tüm genel IP'ler DDoS koruması Katman 3 ve 4 alın. Katman 7 koruma için uygulama ağ geçidi WAF SKU dağıtın. Bu başvuru mimarisi hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/virtual-machines-windows/n-tier).

#### <a name="paas-web-application"></a>PaaS web uygulaması

Bu başvuru mimarisinin tek bir bölgede bir Azure uygulama hizmeti uygulaması çalıştıran gösterir. Bu mimari kanıtlanmış yöntemleri kullanan bir web uygulaması için bir dizi gösterir [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) ve [Azure SQL veritabanı](https://azure.microsoft.com/documentation/services/sql-database/).
Bekleme bölge, yük devretme senaryoları için ayarlanır.

![Bir PaaS web uygulaması için başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image11.png)

Azure trafik Yöneticisi gelen istekleri bölgelerinden uygulama ağ geçidi yönlendirir. Normal işlemler sırasında uygulama ağ geçidi etkin bölgede istekleri yönlendirir. Bu bölgede kullanılamaz duruma gelirse, trafik Yöneticisi üzerinden bekleme bölgede uygulama ağ geçidi için başarısız olur.

Web uygulamasını hedefleyen Internet'ten gelen tüm trafiği yönlendirilir [uygulama ağ geçidi genel IP adresi](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-app-overview) üzerinden trafik Yöneticisi. Bu senaryoda, uygulama hizmeti (web uygulaması) kendisine doğrudan değil dışarıdan karşılıklı ve uygulama ağ geçidi tarafından korunmaktadır. 

Uygulama ağ geçidi WAF SKU yapılandırmanızı öneririz (modunu engelleyin) katman 7 (HTTP/HTTPS/WebSocket) saldırılarına karşı korumaya yardımcı olmak için. Ayrıca, web uygulamaları için yapılandırılmış olan [yalnızca uygulama ağ geçidi gelen trafiği kabul](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/) IP adresi.

Bu başvuru mimarisi hakkında daha fazla bilgi için bkz: [bu makalede](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region).

### <a name="mitigation-for-non-web-paas-services"></a>Web olmayan PaaS Hizmetleri için azaltma

#### <a name="hdinsight-on-azure"></a>Azure Hdınsight'ta

Bu başvuru mimarisinin gösterir DDoS koruması standart için yapılandırma bir [Azure Hdınsight kümesi](https://docs.microsoft.com/azure/hdinsight/). Hdınsight kümesi bir sanal ağa bağlanır ve sanal ağ üzerinde DDoS koruması etkinleştirildiğinden emin olun.

!["Hdınsight" ve "Gelişmiş ayarları" bölmeleri, sanal ağ ayarları](media/azure-ddos-best-practices/image12.png)

![DDoS koruması etkinleştirmek için seçimi](media/azure-ddos-best-practices/image13.png)

Bu mimaride, internet'ten Hdınsight kümesine hedefleyen trafiğe Hdınsight ağ geçidi yük dengeleyici ile ilişkili genel IP yönlendirilir. Ağ geçidi yük dengeleyicinin ardından trafiği baş düğümler veya çalışan düğümleri doğrudan gönderir. DDoS koruması standart Hdınsight sanal ağda etkin olduğundan, sanal ağda tüm genel IP'ler DDoS koruması Katman 3 ve 4 alın. Bu başvuru mimarisinin bölgeli başvuru mimarisi ve N katmanlı ile birleştirilebilir.

Bu başvuru mimarisi hakkında daha fazla bilgi için bkz: [genişletmek Azure bir Azure sanal ağı kullanarak Hdınsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json) belgeleri.

### <a name="azure-api-management"></a>Azure API Management

Bu başvuru mimarisinin ortak uç noktası korur [Azure API Management](../api-management/api-management-key-concepts.md) kaynak kuruluş dışında müşterilere API'leri yayımlama. API Management DDoS koruması etkinleştirmek için bir dış sanal ağ içinde dağıtın.

![API Management başvuru mimarisi diyagramı](media/azure-ddos-best-practices/image15.png)

Dış sanal ağı yapılandırırken, API Management ağ geçidi ve Geliştirici Portalı bir genel yük dengeleyiciye aracılığıyla ortak Internet üzerinden erişilebilir. Bu mimaride DDoS koruması standart dış sanal ağ için API yönetimi etkin. Trafik, internet'ten Katman 3 ve 4 ağ saldırılarına karşı korumalı API Management genel IP adresine yönlendirilir. Katman 7 HTTP/HTTPS saldırılarına karşı korumak için uygulama ağ geçidi WAF modda yapılandırabilirsiniz.

Bir sanal ağ içinde dağıtılmıştır ve DDoS koruması standart için yapılandırılabilir ek hizmetler listesi için bkz: [bu makalede](../virtual-network/virtual-network-for-azure-services.md). DDoS koruması standart yalnızca Azure Resource Manager kaynakları destekler. 

> [!NOTE]
> Eklenen dağıtımda uygulama hizmeti ortamı PowerApps için genel IP ile bir sanal ağ yerel olarak desteklenmez. Uygulama hizmeti ortamı koruma hakkında daha fazla bilgi için bu bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DDoS koruması ürün sayfası](https://azure.microsoft.com/services/ddos-protection/)

* [Azure DDoS koruması blogu](http://aka.ms/ddosblog)

* [Azure DDoS koruması belgeleri](../virtual-network/ddos-protection-overview.md)
