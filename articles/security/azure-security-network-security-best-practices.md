---
title: Azure ağ güvenliği için en iyi uygulamalar | Microsoft Docs
description: Bu makalede Azure özellikleri yerleşik güvenlik ağ kullanmaya yönelik en iyi yöntemler kümesi sağlar.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/17/2018
ms.author: TomSh
ms.openlocfilehash: b644a175814fb28563a2524e27f52d0285415d66
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610953"
---
# <a name="azure-network-security-best-practices"></a>Azure ağ güvenliği için en iyi uygulamalar
Bağlanabileceğiniz [Azure sanal makineleri (VM'ler)](https://azure.microsoft.com/services/virtual-machines/) ve bunları yerleştirerek tarafından diğer ağ bağlantılı cihazlar için cihazları [Azure sanal ağları](https://azure.microsoft.com/documentation/services/virtual-network/). Diğer bir deyişle, ağ bağlantısı etkin cihazlar arasında TCP tabanlı iletişime izin vermek için bir sanal ağ, sanal ağ arabirim kartları bağlanabilirsiniz. Azure sanal ağına bağlı sanal makineleri aynı sanal ağ, farklı sanal ağlar, internet veya kendi şirket içi ağlar cihazlara bağlanabilir.

Bu makalede, Azure ağ güvenliği için en iyi uygulamalar topluluğu açıklanmaktadır. Bu en iyi Azure ağı ile deneyimimizi türetilmiştir ve müşteri deneyimleri bulunun.

En iyi her uygulama için bu makalede açıklanmıştır:

* En iyi nedir
* Bu en iyi etkinleştirmek istediğiniz neden
* En iyi etkinleştirme başarısız olursa ne sonuç olabilir
* En iyi olası alternatifler
* Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure ağ güvenliği için en iyi uygulamalar makalesi bir fikir birliğine varılmış fikrim, Azure platformu özellikleri ve özellik kümeleri üzerinde temel alır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Aşağıdaki bölümlerde, ağ güvenliği için en iyi uygulamalar açıklanmaktadır.

## <a name="logically-segment-subnets"></a>Mantıksal kesim alt ağlar
Azure sanal ağları, şirket içi ağınızda LAN benzerdir. Bir Azure sanal ağı arkasında tüm Azure sanal makineleri yerleştirmek tek bir özel IP adresi alanı tabanlı ağ oluşturma olur. Sınıf A (10.0.0.0/8), sınıf B (172.16.0.0/12) özel IP adresi alanları kullanılabilir olduğunu ve aralıkları sınıfı C (192.168.0.0/16).

Mantıksal olarak alt ağlar kesimlere için en iyi yöntemler şunlardır:

**En iyi yöntem**: Büyük bir adres alanı alt ağa bölün.   
**Ayrıntı**: Kullanım [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)-, alt ağlar oluşturmak için alt ağ kurallara göre.

**En iyi yöntem**: Alt ağlar arasındaki ağ erişim denetimleri oluşturun. Alt ağlar arasında yönlendirme, otomatik olarak gerçekleşir ve yönlendirme tablolarını el ile yapılandırmanız gerekmez. Varsayılan olarak, Azure sanal ağ oluşturma alt ağlar arasındaki ağ erişimi denetim yoktur.   
**Ayrıntı**: Kullanım bir [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) (NSG). Nsg'ler basit, ağ trafiği için izin verme/reddetme kurallarını oluşturmak için 5-tanımlama grubu (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol katman 4) kullanan paket durum bilgisi olan İnceleme cihazları yaklaşımını. İzin vermeniz veya tek bir IP adresi için ve birden çok IP adresi veya için ve tüm alt ağlara gelen ve giden trafiği engelle.

Nsg'ler alt ağlar arasındaki ağ erişim denetimi için kullandığınızda, aynı güvenlik bölgesinde veya rol kendi alt ağlara ait kaynaklara yerleştirebilirsiniz.

## <a name="control-routing-behavior"></a>Yönlendirme davranışını denetleme
Bir Azure sanal ağı üzerinde bir sanal makine geçirdiğinizde, diğer VM'ler farklı alt ağlarda olsalar bile sanal Makineyi aynı sanal ağdaki herhangi bir VM bağlanabilirsiniz. Bu tür iletişimler sistem yolları varsayılan olarak etkin koleksiyonunu izin verdiğinden, bu mümkündür. Bu varsayılan yolları Vm'leri aynı sanal ağda birbiriyle ve İnternet'e (yalnızca İnternet'e giden iletişimi) yönelik bağlantıları başlatmasını sağlar.

Varsayılan sistem yollarını birçok dağıtım senaryosu için kullanışlı olsa da, dağıtımlarınız için yönlendirme yapılandırması özelleştirmek istediğiniz zaman zamanlar vardır. Belirli hedeflere ulaşmak için sonraki atlama adresi yapılandırabilirsiniz.

Yapılandırdığınız öneririz [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) bir sanal ağ için bir güvenlik Aleti dağıttığınızda. Biz başlıklı bir sonraki bölümde bu hakkında konuşmak [güvenli kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Kullanıcı tanımlı yollar gerekli değildir ve genelde varsayılan sistem yollarını çalışır.
>
>

## <a name="enable-forced-tunneling"></a>Zorlamalı tünel etkinleştir
Zorlamalı tünel daha iyi anlamak için hangi "bölünmüş tünel" olduğunu anlamak kullanışlıdır. Bölünmüş tünel en yaygın örnek, sanal özel ağ (VPN) bağlantıları ile görülür. Şirket ağınıza VPN bağlantısı, otelinden oda kurmak düşünün. Bu bağlantı, şirket kaynaklarına erişmenize olanak sağlar. Tüm iletişimi Kurumsal ağınıza VPN tüneli üzerinden geçer.

İnternet üzerindeki kaynaklara bağlanmak istediğinizde ne olur? Bölünmüş tünel etkin olduğunda, bu bağlantılar doğrudan Internet'e ve VPN tünelinden gidin. Bazı güvenlik uzmanlarından olası bir risk için göz önünde bulundurun. Bunlar, bölünmüş tünel ve tüm bağlantıları, İnternet'e giden ve kurumsal kaynaklar için hedeflenen VPN tüneli üzerinden Git sağlamaktan'devre dışı bırakılması önerilir. Bölünmüş tüneli devre dışı bırakmanın avantajı, internet bağlantıları Kurumsal ağın güvenlik cihazları daha sonra zorunlu olmasıdır. VPN istemci VPN tüneli dışında İnternet'e bağlı değilse, böyle değildir.

Şimdi şimdi VM'ler için bir Azure sanal ağ üzerinde bu geri getirin. Bir Azure sanal ağı için varsayılan yolları, VM'ler, trafiği İnternet'e başlatmak izin verin. Bu giden bağlantılar VM saldırı yüzeyini artırabilir ve saldırganlar tarafından kullanılan çünkü bu bir güvenlik riski çok temsil edebilir. Bu nedenle önerilir, [etkinleştir zorlamalı tünel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) Azure sanal ağınız ile şirket içi ağınız arasında şirketler arası bağlantı varsa, bu vm'lerde. Daha sonra ağ en iyi uygulamalar, şirket içi bağlantılar hakkında konuşun.

Şirketler arası bağlantı yoksa, Azure sanal ağ güvenlik Gereçleri (giden bağlantıları engellemek için aşağıda açıklanmıştır) internet'e Azure sanal makinelerinizden ya da Nsg'ler (daha önce açıklanmıştır) yararlanmak emin olun.

## <a name="use-virtual-network-appliances"></a>Sanal ağ gereçlerini kullanın
Nsg'leri ve kullanıcı tanımlı yönlendirme belirli bir ağ ve Aktarım katmanı güvenlik ağ ölçü sağlayabilir [OSI modeli](https://en.wikipedia.org/wiki/OSI_model). Ancak bazı durumlarda, istediğiniz veya yığın yüksek düzeyde güvenlik etkinleştirmeniz gerekir. Bu gibi durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik Gereçleri dağıtmanızı öneririz.

Azure ağ güvenlik Gereçleri ne ağ düzeyinde denetimleri sağlamak daha iyi güvenlik teslim edebilirsiniz. Sanal ağ güvenlik Gereçleri ağ güvenlik özellikleri içerir:


* Özellikleri
* Yetkisiz giriş algılama/yetkisiz erişim önleme
* Güvenlik Açığı Yönetimi
* Uygulama denetimi
* Ağ tabanlı bir anomali algılama
* Web filtreleme
* Virüsten koruma
* Botnet koruma

Kullanılabilir Azure sanal ağ güvenlik Gereçleri bulmak için Git [Azure Marketi](https://azure.microsoft.com/marketplace/) ve "güvenlik" ve "ağ güvenliği" için arama

## <a name="deploy-perimeter-networks-for-security-zones"></a>Güvenlik bölgeleri için çevre ağları dağıtma
A [çevre ağı](https://docs.microsoft.com/azure/best-practices-network-security) (DMZ olarak da bilinir) bir ek, varlıklarınızı ve internet arasında bir güvenlik katmanı sağlayan bir fiziksel veya mantıksal ağ segmenttir. Özel bir ağ erişim denetimi bir çevre ağına en önüne cihazlarda, sanal ağınızda yalnızca istenen trafiğe izin.

Çevre ağları, ağ erişim denetimi yönetim, izleme, günlüğe kaydetme ve Azure sanal ağınıza ucuna cihazlarda raporlama odaklanabilirsiniz olduğundan kullanışlıdır. Burada, genellikle hizmet (DDoS) önleme, yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (IDs/IPS), güvenlik duvarı kuralları ve ilkeleri, web filtrelemesi, ağ kötü amaçlı yazılımdan koruma ve daha fazla dağıtılmış reddi sağlar. Ağ güvenlik cihazları, Azure sanal ağınız ile internet arasında yaslanın ve her iki ağ üzerinde bir arabirim üzerinden erişir.

Bu temel tasarım, çevre bir ağdaki olsa da, vardır birçok farklı tasarım, arka arkaya üç konaklı ve birden çok girişli gibi.

Azure kaynaklarınız için ağ güvenlik düzeyini artırmak için bir çevre ağı kullanarak düşündüğünüz tüm yüksek güvenlikli dağıtımlarda öneririz.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Internet ile özel WAN bağlantıları maruz kalma riskinizi kaçının
Birçok kuruluş BT rota karma seçtiniz. Diğer şirket içi kalırken karma BT şirketinizin bilgi varlıklarını Azure'da bazılarıdır. Diğer bileşenlerini şirket içinde kalacak şekilde çoğu durumda, bazı bileşenler bir hizmetin Azure'da çalışıyor.

Karma BT senaryo genellikle bazı şirketler arası bağlantı türünü yoktur. Kendi şirket içi ağlar Azure sanal ağlarına bağlanmak şirket içi ve dışı karışık bağlantı sağlar. İki içi ve dışı karışık bağlantı çözüm mevcuttur:

* **Siteden siteye VPN**: Güvenli, güvenilir ve yerleşik bir teknoloji olan, ancak bağlantı internet üzerinden gerçekleşir. En fazla 200 MB/sn bant genişliği sınırlı. Siteden siteye VPN, bazı senaryolarda uygun bir seçenektir ve daha ayrıntılı anlatılan bölümünde [sanal makinelere RDP/SSH devre dışı erişim](#disable-rdpssh-access-to-virtual-machines).
* **Azure ExpressRoute**: Kullanmanızı öneririz [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) içi ve dışı karışık bağlantı için. ExpressRoute adanmış WAN olan şirket içi konumunuz ve Exchange barındırma sağlayıcısı arasındaki bağlantı. Bu telco bağlantı olduğundan, verilerinizi internet üzerinden yolculuk değildir ve internet iletişimi için olası riskleri gösterilmez.

## <a name="optimize-uptime-and-performance"></a>Çalışma süresi ve performansı en iyi duruma getirme
Hizmet çalışmıyorsa, bilgi erişilemez. Performans verileri kullanılamıyor kadar düşük olduğunda, veriler erişilemez göz önünde bulundurun. Güvenlik açısından bakıldığında, ne olursa olsun, hizmetlerinizi en iyi çalışma süresi ve performans sahip olduğunuzdan emin olmak için elinizden geleni yapmak gerekir.

Yük Dengeleme kullanılabilirliğini ve performansını geliştirmek için yaygın olarak kullanılan ve etkili bir yöntem var. Yük Dengeleme, bir hizmetin parçası olan sunucular arasında ağ trafiği dağıtmanın bir yöntemdir. Ön uç web sunucuları, hizmetin bir parçası varsa, örneğin, Yük Dengeleme, birden çok ön uç web sunucusu arasında trafiği dağıtmak için kullanabilirsiniz.

Yük Dengeleyici web sunuculardan biri kullanılamaz hale gelirse, bu sunucuya trafik göndermeyi durdurur ve hala çevrimiçi sunucularına yönlendiren çünkü bu trafiğin dağıtımını kullanılabilirliğini artırır. Tüm Yük Dengeleme sunucuları arasında isteklerine hizmet için işlemci, ağ ve bellek ek yükü dağıtıldığından Yük Dengeleme de performans, yardımcı olur.

Yük Dengeleme mümkün olduğunca ve hizmetleriniz için uygun olarak kullanması önerilir. İle birlikte aşağıda verilmiştir senaryoları hem Azure sanal ağ düzeyinde hem de genel düzeyde Yük Dengeleme her seçenek.

**Senaryo**: Bir uygulamaya sahip olan:

- Aynı kullanıcı/istemci oturumunun aynı arka uç sanal makinesine ulaşmaya yönelik isteklerini gerektirir. Bu örnekleri Sepeti uygulamaları ve web posta alışveriş.
- Sunucuya şifrelenmemiş iletişimin kabul edilebilir bir seçenek değil. Bu nedenle yalnızca bir güvenli bağlantı kabul eder.
- Birden fazla HTTP isteğinin yönlendirilmesini veya yüklemek için aynı uzun süreli TCP bağlantısı üzerinde farklı arka uç sunucularına dengeli gerektirir.

**Yük Dengeleme seçeneği**: Kullanım [Azure Application Gateway](../application-gateway/application-gateway-introduction.md), bir HTTP web trafiği yük dengeleyici. Application Gateway uçtan uca SSL şifrelemesini desteklemektedir ve [SSL sonlandırma](../application-gateway/application-gateway-introduction.md) Gateway. Web sunucuları sonra şifreleme ve şifre çözme ek ve arka uç sunucularına şifrelenmemiş akan trafiği kurtulmasını.

**Senaryo**: Bir Azure sanal ağı içinde bulunan, sunucular arasında internet'ten gelen bağlantıları Bakiye'ı yüklemeniz gerekir. Senaryolar verilmiştir:

- İnternet'ten gelen istekleri kabul durum bilgisi olmayan uygulamalara sahip.
- Yapışkan oturumlar gerektirmeyen veya SSL yük boşaltma. Yapışkan oturumlar uygulama yük dengelemesi ile sunucu benzeşimi elde etmek için kullanılan bir yöntemdir.

**Yük Dengeleme seçeneği**: Azure portalını kullanarak [bir dış yük dengeleyici oluşturma](../load-balancer/quickstart-create-basic-load-balancer-portal.md) yüksek seviyede kullanılabilirlik sağlamak için birden çok VM arasında gelen istekleri yayılır.

**Senaryo**: Vm'lerden internet'te olmayan Dengeleme Bağlantılar'ı yüklemeniz gerekir. Çoğu durumda, Yük Dengeleme için kabul edilen bağlantıları, SQL Server örnekleri veya dahili web sunucuları gibi bir Azure sanal ağı üzerindeki cihazlar tarafından başlatılır.   
**Yük Dengeleme seçeneği**: Azure portalını kullanarak [iç yük dengeleyici oluşturma](../load-balancer/quickstart-create-basic-load-balancer-powershell.md) yüksek seviyede kullanılabilirlik sağlamak için birden çok VM arasında gelen istekleri yayılır.

**Senaryo**: Genel Yük Dengeleme için gereksinim duyduğunuz:

- Yaygın olarak birden çok bölgede dağıtılır ve çalışma süresi (kullanılabilirlik) olası en yüksek düzeyde gerektiren bir bulut çözümü vardır.
- Çalışma süresini en üst düzey veri merkezinin tamamı kullanılamaz hale gelirse, hizmetinizin kullanılabilir olduğundan emin olmak olası gerekir.

**Yük Dengeleme seçeneği**: Azure Traffic Manager'ı kullanın. Traffic Manager, kullanıcının konumuna göre hizmetlerinizi Bakiye bağlantılarını yük mümkün kılar.

Örneğin, kullanıcı AB hizmetinize istek yaparsa, hizmetlerinizi bir AB veri Merkezi'nde bulunan bağlantı yönlendirilir. Bu bölümü Traffic Manager'a genel yüklemek için en yakın veri merkezini bağlayan uzakta olan veri merkezlerine bağlanmak daha hızlı olduğu için performansı karşı yardımcı olur.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>Sanal makinelere RDP/SSH erişimini devre dışı bırakma
Azure sanal makineleri kullanarak ulaşmak mümkündür [Uzak Masaüstü Protokolü](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ve [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokolünü. Bu protokollerin uzak konumlardan sanal makineleri yönetimini etkinleştirme ve veri merkezi bilgisayar standart.

İnternet üzerinden bu protokolleri kullanarak olası güvenlik sorunu saldırganlar kullanabilmenizdedir [yanılma](https://en.wikipedia.org/wiki/Brute-force_attack) teknikleri, Azure sanal makinelerine erişmek için. Saldırganların erişim elde ettikten sonra sanal makinenizin sanal ağınızdaki diğer makinelerin ödün için bir başlatma noktası kullanın veya hatta Azure dışındaki ağ cihazları saldırı.

Doğrudan RDP ve SSH erişimi, Azure sanal makineler için internet'ten devre dışı bırakmanızı öneririz. İnternet'ten doğrudan RDP ve SSH erişimini devre dışı bırakıldıktan sonra bu Vm'lere uzaktan yönetim için erişmek için kullanabileceğiniz diğer seçeneğiniz vardır.

**Senaryo**: Internet üzerinden Azure sanal ağına bağlanmak tek bir kullanıcı etkinleştirin.   
**Seçenek**: [Noktadan siteye VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) uzaktan erişim VPN istemci/sunucu bağlantısını için başka bir terimdir. Noktadan siteye bağlantı kurulduktan sonra öğe kullanıcı kullanıcı için noktadan siteye VPN aracılığıyla bağlanan Azure sanal ağı üzerinde bulunan herhangi bir VM bağlanmak için RDP veya SSH kullanabilirsiniz. Bu, kullanıcı bu Vm'leri ulaşmak yetkisi olduğunu varsayar.

Bir VM için iki kez bağlanmadan önce kimlik doğrulaması kullanıcının sahip olduğu noktadan siteye VPN doğrudan RDP veya SSH bağlantılarına göre daha güvenlidir. İlk olarak, kullanıcının kimlik doğrulaması (ve yetkili olması) gereken noktadan siteye VPN bağlantısı kuracak şekilde. İkinci olarak, kullanıcının kimlik doğrulaması (ve yetkili olması) gereken RDP veya SSH oturumu açmak için.

**Senaryo**: Azure sanal ağınızda Vm'lere bağlanmak şirket içi ağınızdaki kullanıcılar etkinleştirin.   
**Seçenek**: A [siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) tüm bir ağ, internet üzerinden başka bir ağa bağlanır. Siteden siteye VPN, şirket içi ağınızı bir Azure sanal ağına bağlanmak için kullanabilirsiniz. Kullanıcılar şirket içi ağınızda siteden siteye VPN bağlantısı üzerinden RDP veya SSH protokolünü kullanarak bağlanın. İnternet doğrudan RDP veya SSH erişimine izin gerekmez.

**Senaryo**: Siteden siteye VPN için benzer işlevsellik sağlamak için ayrılmış bir WAN bağlantısı kullanın.   
**Seçenek**: Kullanım [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/). Bu, siteden siteye VPN için benzer işlevsellik sağlar. Temel farklılıklar şunlardır:

- Ayrılmış bir WAN bağlantısı, İnternet'e geçiş değil.
- Adanmış WAN bağlantılarına genellikle daha kararlı ve daha iyi performans.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla güvenliğini sağlama
Sanal ağ hizmet uç noktaları, sanal ağ özel adres alanınızı ve Azure Hizmetleri için sanal ağınızın kimliğini doğrudan bağlantı üzerinden genişletmek için kullanın. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Azure hizmet trafiği sanal ağınızdan gelen her zaman Microsoft Azure omurga ağında kalır.

Hizmet uç noktaları aşağıdaki avantajları sağlar:

- **Azure hizmet kaynaklarınız için geliştirilmiş güvenlik**: Hizmet uç noktaları sayesinde Azure hizmet kaynakları sanal ağınızla sınırlandırılarak güvenli hale getirilebilir. Hizmet kaynaklarını bir sanal ağ güvenliğini sağlama, tam olarak kaynaklara genel internet erişimini kaldırarak ve yalnızca sanal ağınızdan gelen trafiğe izin veren gelişmiş güvenlik sağlar.
- **Sanal ağınızdan gelen Azure trafiği için en iyi yönlendirmeyi**: İnternet trafiği, şirket içi ve/veya zorlamalı tünel oluşturma olarak bilinen sanal gereçlerden geçmeye zorlayan tüm rotalar sanal ağınızda de Azure hizmet trafiğini internet trafiğiyle aynı rotadan geçmeye zorlar. Hizmet uç noktaları Azure trafiği için en uygun rotayı sunar.

  Uç noktaları, Azure omurga ağı üzerinde her zaman hizmet trafiğini sanal ağınızdan doğrudan hizmete yönlendirir. Trafiğin Azure omurga ağında tutulması, Denetim ve zorlamalı tünel, hizmet trafiğini etkilemeden giden internet trafiğini sanal ağlarınızdan aracılığıyla izleme devam etmenizi sağlar. Daha fazla bilgi edinin [kullanıcı tanımlı rotalar ve zorlamalı tünel](../virtual-network/virtual-networks-udr-overview.md).

- **Kolay kurulum sayesinde daha az yönetim yükü**: Artık içinde bir IP Güvenlik Duvarı üzerinden Azure kaynaklarını güvenli hale getirmek için sanal ağlarınızda ayrılmış ve genel IP adresleri gerekir. Hizmet uç noktalarını ayarlamak için herhangi bir NAT veya ağ geçidi cihazı gerekmez. Hizmet uç noktaları alt ağda tek tıklamayla yapılandırılabilir. Uç noktaları korumak için kalkar yoktur.

Hizmet uç noktaları ve Azure Hizmetleri ve hizmet uç noktaları için kullanılabilir bölgeleri hakkında daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="next-step"></a>Sonraki adım
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.
