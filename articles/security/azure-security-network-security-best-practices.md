---
title: Ağ güvenliği - Microsoft Azure için en iyi uygulamalar
description: Bu makalede Azure özellikleri yerleşik güvenlik ağ kullanmaya yönelik en iyi yöntemler kümesi sağlar.
services: security
author: TerryLanfear
manager: barbkess
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/05/2019
ms.author: TomSh
ms.openlocfilehash: 78402d3e388f08eae6652859a71c93ff408a5b0d
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65152985"
---
# <a name="azure-best-practices-for-network-security"></a>Azure, ağ güvenliği için en iyi uygulamaları
Bu makale koleksiyonu, ağ güvenliğini artırmak için Azure en iyi uygulamaları açıklar. Bu en iyi Azure ağı ile deneyimimizi türetilmiştir ve müşteri deneyimleri bulunun.

En iyi her uygulama için bu makalede açıklanmıştır:

* En iyi nedir
* Bu en iyi etkinleştirmek istediğiniz neden
* En iyi etkinleştirme başarısız olursa ne sonuç olabilir
* En iyi olası alternatifler
* Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu iyi bir fikir birliğine varılmış fikrim, Azure platformu özellikleri ve özellik kümeleri üzerinde temel alır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="use-strong-network-controls"></a>Güçlü ağ denetimleri kullanın
Bağlanabileceğiniz [Azure sanal makineleri (VM'ler)](https://azure.microsoft.com/services/virtual-machines/) ve bunları yerleştirerek tarafından diğer ağ bağlantılı cihazlar için cihazları [Azure sanal ağları](https://docs.microsoft.com/azure/virtual-network/). Diğer bir deyişle, ağ bağlantısı etkin cihazlar arasında TCP tabanlı iletişime izin vermek için bir sanal ağ, sanal ağ arabirim kartları bağlanabilirsiniz. Azure sanal ağına bağlı sanal makineleri aynı sanal ağ, farklı sanal ağlar, internet veya kendi şirket içi ağlar cihazlara bağlanabilir.

Ağınız ile ağınızın güvenliğini planlarken, merkezileştirme öneririz:

- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresleme gibi ağ işlevlerini çekirdek yönetimi.
- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresleme gibi ağ sanal Gereci işlevleri gibi ağ güvenlik öğelerini İdaresi.

Ağınız ile ağınızın güvenliğini izlemek için ortak bir dizi Yönetim Aracı'nı kullanın, her ikisini de kopyalayıp Temizle görünürlük alırsınız. Basit ve birleştirilmiş güvenlik stratejisi, İnsan anlama ve Otomasyon güvenilirliğini artırır çünkü hataları azaltır.

## <a name="logically-segment-subnets"></a>Mantıksal kesim alt ağlar
Azure sanal ağları, şirket içi ağınızda LAN benzerdir. Bir Azure sanal ağı arkasındaki bir ağ, tüm Azure sanal makineleri yerleştirmek tek özel bir IP adres alanı üzerinde oluşturduğunuz olur. Sınıf A (10.0.0.0/8), sınıf B (172.16.0.0/12) özel IP adresi alanları kullanılabilir olduğunu ve aralıkları sınıfı C (192.168.0.0/16).

Mantıksal olarak alt ağlar kesimlere için en iyi yöntemler şunlardır:

**En iyi yöntem**: Atamayın geniş aralıklarıyla kuralları izin verir (örneğin, izin ver 0.0.0.0 255.255.255.255 aracılığıyla).  
**Ayrıntı**: Sorun giderme yordamları engelleyin veya bu tür bir kurallar ayarlama yasaklamak emin olun. Bu yanlış bir fikir güvenlik kuralları müşteri adayına izin ve sık bulundu ve kırmızı ekipleri tarafından kötüye.

**En iyi yöntem**: Büyük bir adres alanı alt ağa bölün.   
**Ayrıntı**: Kullanım [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)-, alt ağlar oluşturmak için alt ağ kurallara göre.

**En iyi yöntem**: Alt ağlar arasındaki ağ erişim denetimleri oluşturun. Alt ağlar arasında yönlendirme, otomatik olarak gerçekleşir ve yönlendirme tablolarını el ile yapılandırmanız gerekmez. Varsayılan olarak, bir Azure sanal ağı oluşturma alt ağlar arasındaki ağ erişimi denetim yoktur.   
**Ayrıntı**: Kullanım bir [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) Azure alt ağına istekte bulunulmamış trafik karşı korumak için. Ağ güvenlik grupları ağ trafiği için izin verme/reddetme kurallarını oluşturmak için 5 tanımlama grubu yaklaşım (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol katman 4) kullanan basit, durum bilgisi olan paket incelemesi cihazlardır. İzin vermeniz veya tek bir IP adresi için ve birden çok IP adresi veya için ve tüm alt ağlara gelen ve giden trafiği engelle.

Alt ağlar arasındaki ağ erişim denetimi için ağ güvenlik grupları'nı kullandığınızda aynı güvenlik bölgesinde veya rol kendi alt ağlara ait kaynaklara yerleştirebilirsiniz.

**En iyi yöntem**: Küçük sanal ağlar ve alt ağlar, Basitlik ve esneklik sağlamak için kaçının.   
**Ayrıntı**: Çoğu kuruluş başlangıçta planlanandan daha fazla kaynak ekleyin ve adresleri yeniden ayırma işçilik yoğun olur. Küçük bir alt ağları kullanarak sınırlı güvenlik değeri ekler ve her alt ağa bir ağ güvenlik grubu eşleme yükü ekler. Alt ağlar esneklik büyüme için sahip olmasını sağlamak için genel olarak tanımlayın.

**En iyi yöntem**: Ağ güvenlik grubu kuralı Yönetimi tanımlayarak basitleştirmek [uygulama güvenlik grupları](https://docs.microsoft.com/rest/api/virtualnetwork/applicationsecuritygroups).  
**Ayrıntı**: Düşündüğünüz IP adresleri listesi gelecekte değişebilir veya çok sayıda ağ güvenlik grupları arasında kullanılması için bir uygulama güvenlik grubu tanımlayın. Emin olmak adına uygulama güvenliği grupları açıkça olması şekilde diğer kendi içerik ve amacını anlayabilir.

## <a name="adopt-a-zero-trust-approach"></a>Sıfır güven bir yaklaşım benimseyin
Çevre tabanlı ağlarda bir ağ içindeki tüm sistemler güvenilir olabilir varsayımına çalışır. Ancak günümüzde çalışanlar yerden kullanıcıların, kuruluş kaynaklarına erişim cihazları ve uygulamaları çeşitli üzerinde getiren çevre güvenlik denetimleri ilgisiz. Kimin bir kaynağa erişmek için yalnızca odaklanan erişim denetimi ilkeleri yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi Yöneticisi için güvenlik yöneticileri ayrıca etmeni gerekir *nasıl* kaynak erişilir.

Ağları ağları ihlallerine karşı savunmasız olabilir çünkü geleneksel savunmaları evrim Geçiren gerekir: bir saldırgan, güvenilen sınırları içinde tek bir uç nokta tehlikeye ve bir köprübaşı tamamını ağ üzerinden hızlı bir şekilde genişletin. [Sıfır güven](https://www.microsoft.com/security/blog/2018/06/14/building-zero-trust-networks-with-microsoft-365/) ağları içinde bir çevre ağ konumuna dayalı güven kavramını ortadan kaldırın. Bunun yerine, kuruluş verilerine ve kaynaklarına erişimi yöneten cihaz ve kullanıcı güven talepleri sıfır güven mimarileri kullanın. Yeni girişim için erişim zamanında güven doğrulama sıfır güven yaklaşım benimseyin.

En iyi uygulamalar şunlardır:

**En iyi yöntem**: Cihaz, kimlik, Güvencesi, ağ konumu ve diğer kaynaklarına koşullu erişim verin.  
**Ayrıntı**: [Azure AD koşullu erişim](../active-directory/conditional-access/overview.md) gerekli koşullara göre otomatik erişim denetimi kararları uygulayarak doğru erişim denetimlerini uygulamak sağlar. Daha fazla bilgi için [koşullu erişim ile Azure yönetim erişimi yönetme](../role-based-access-control/conditional-access-azure-management.md).

**En iyi yöntem**: Yalnızca iş akışı onaydan sonra bağlantı noktası erişimi etkinleştirin.  
**Ayrıntı**: Kullanabileceğiniz [tam zamanında VM erişimi, Azure Güvenlik Merkezi'nde](../security-center/security-center-just-in-time.md) kilidi gelen trafiği Azure vm'lerinize, Vm'lere gerektiğinde bağlanılabilmesi için kolay erişim sağlamanın yanı sıra saldırılara maruz kalma riskinizi azaltır.

**En iyi yöntem**: Kötü amaçlı veya yetkisiz kullanıcıların izinleri süresi dolduktan sonra erişmesini engeller ayrıcalıklı görevlerin gerçekleştirilmesi için geçici izinleri verin. Yalnızca kullanıcıların gerektiğinde erişim izni verilir.  
**Ayrıntı**: Tam zamanında erişim, Azure AD Privileged Identity Management veya bir üçüncü taraf çözümde ayrıcalıklı görevleri gerçekleştirmek için izinleri vermek için kullanın.

Ağ güvenliği bir sonraki aşamasıdır sıfır güven olur. "İhlal varsayımı" anlayış gerçekleştirilecek kuruluşlar sızdırdığında durumunu sürücüleri, ancak bu yaklaşım sınırlama olmamalıdır. Dilediğiniz zaman ve dilediğiniz yerde, herhangi bir şekilde üretken olmak için çalışanlarınıza güç katın teknolojilerini kullanarak kuruluşlar modern şirketlerin oluşturabilir sağlarken şirket verilerine ve kaynaklarına sıfır güven ağları koruyun.

## <a name="control-routing-behavior"></a>Yönlendirme davranışını denetleme
Bir Azure sanal ağı üzerinde bir sanal makine geçirdiğinizde, diğer VM'ler farklı alt ağlarda olsalar bile sanal Makineyi aynı sanal ağdaki herhangi bir VM bağlanabilirsiniz. Bu tür iletişimler sistem yolları varsayılan olarak etkin koleksiyonunu izin verdiğinden, bu mümkündür. Bu varsayılan yolları Vm'leri aynı sanal ağda birbiriyle ve İnternet'e (yalnızca İnternet'e giden iletişimi) yönelik bağlantıları başlatmasını sağlar.

Varsayılan sistem yollarını birçok dağıtım senaryosu için kullanışlı olsa da, dağıtımlarınız için yönlendirme yapılandırması özelleştirmek istediğiniz zaman zamanlar vardır. Belirli hedeflere ulaşmak için sonraki atlama adresi yapılandırabilirsiniz.

Yapılandırdığınız öneririz [kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md) bir sanal ağ için bir güvenlik Aleti dağıttığınızda. Biz başlıklı bir sonraki bölümde bu hakkında konuşmak [güvenli kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla](azure-security-network-security-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks).

> [!NOTE]
> Kullanıcı tanımlı yollar gerekli değildir ve genelde varsayılan sistem yollarını çalışır.
>
>

## <a name="use-virtual-network-appliances"></a>Sanal ağ gereçlerini kullanın
Ağ güvenlik gruplarını ve kullanıcı tanımlı yönlendirme belirli bir ağ ve Aktarım katmanı güvenlik ağ ölçü sağlayabilir [OSI modeli](https://en.wikipedia.org/wiki/OSI_model). Ancak bazı durumlarda, istediğiniz veya yığın yüksek düzeyde güvenlik etkinleştirmeniz gerekir. Bu gibi durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik Gereçleri dağıtmanızı öneririz.

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

Çevre ağları, ağ erişim denetimi yönetim, izleme, günlüğe kaydetme ve Azure sanal ağınıza ucuna cihazlarda raporlama odaklanabilirsiniz olduğundan kullanışlıdır. Bir çevre ağında genellikle burada engelleme (DDoS) hizmeti, yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (IDs/IPS), güvenlik duvarı kuralları ve ilkeleri, web filtrelemesi, ağ kötü amaçlı yazılımdan koruma ve daha fazla dağıtılmış reddi Etkinleştir ' dir. Ağ güvenlik cihazları, Azure sanal ağınız ile internet arasında yaslanın ve her iki ağ üzerinde bir arabirim üzerinden erişir.

Bu temel tasarım, çevre bir ağdaki olsa da, vardır birçok farklı tasarım, arka arkaya üç konaklı ve birden çok girişli gibi.

Sıfır güven kavramını daha önce bahsedilen bağlı olarak, Azure kaynaklarınız için ağ güvenlik ve erişim denetimi düzeyini artırmak için tüm yüksek güvenlikli dağıtımlarda çevre ağı kullanarak geçirmeniz önerilir. Azure veya üçüncü taraf çözümü, varlıklarınızı ve internet arasında ek bir güvenlik katmanı sağlamak için kullanabilirsiniz:

- Azure yerel denetler. [Azure Güvenlik Duvarı](../firewall/overview.md) ve [Application Gateway web uygulaması güvenlik duvarı](../application-gateway/overview.md#web-application-firewall) tam durum bilgisi olan bir güvenlik duvarı ile temel güvenlik hizmeti, yerleşik yüksek kullanılabilirlik, sınırsız bulut ölçeklenebilirlik sunar FQDN'sini filtreleme , OWASP çekirdek kural kümeleri ve basit kurulum ve yapılandırma desteği.
- Üçüncü taraf teklifleri. Arama [Azure Marketi](https://azuremarketplace.microsoft.com/) için tasarlanan yeni nesil Güvenlik Duvarı (NGFW) ve bilinen güvenlik araçları ve önemli ölçüde Gelişmiş ağ güvenlik düzeyi sağlayan diğer üçüncü taraf teklifleri. Yapılandırma daha karmaşık olabilir ancak bir üçüncü taraf teklifini, mevcut özelliklerinize ve becerilerini kullanmaya izin verebilir.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>İnternet ile özel WAN bağlantıları maruz kalma riskinizi kaçının
Birçok kuruluş BT rota karma seçtiniz. Hibrit BT, şirketinizin bilgi varlıklarını Azure'da bazıları ve başkalarının şirket içinde kalır. Diğer bileşenlerini şirket içinde kalacak şekilde çoğu durumda, bazı bileşenler bir hizmetin Azure'da çalışıyor.

Karma bir BT senaryo, genellikle bazı şirketler arası bağlantı türünü yoktur. Kendi şirket içi ağlar Azure sanal ağlarına bağlanmak şirket içi ve dışı karışık bağlantı sağlar. İki içi ve dışı karışık bağlantı çözüm mevcuttur:

* [Siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md). Güvenli, güvenilir ve yerleşik bir teknoloji olan, ancak bağlantı internet üzerinden gerçekleşir. En fazla 200 MB/sn bant genişliği sınırlı. Siteden siteye VPN, bazı senaryolarda uygun bir seçenektir.
* **Azure ExpressRoute**. Kullanmanızı öneririz [ExpressRoute](../expressroute/expressroute-introduction.md) içi ve dışı karışık bağlantı için. ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute sayesinde, Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. ExpressRoute adanmış WAN olan şirket içi konumunuza ya da Microsoft Exchange barındırma sağlayıcısına arasındaki bağlantı. Bu telco bağlantı olduğundan, internet iletişimi için olası riskleri ortaya olmayan şekilde verilerinizi internet üzerinden yolculuk değil.

ExpressRoute bağlantınızı konumunu, güvenlik duvarı kapasite, ölçeklenebilirlik, güvenilirlik ve ağ trafiğini görünürlüğünü etkileyebilir. ExpressRoute, mevcut (şirket içi) ağlarda sonlandırmak nereye tanımlamak gerekir. Şunları yapabilirsiniz:

- Güvenlik Duvarı (çevre ağı paradigma) dışında veri merkezleri yalıtma var olan bir uygulama olarak devam etmeniz gerekiyorsa trafiğinde görünürlüğe ihtiyacınız varsa ya da Azure'da yalnızca extranet kaynakları yerleştiriyorsanız sonlandırın.
- Güvenlik Duvarı (ağ uzantısı paradigma) içinde sonlandırır. Bu varsayılan önerilir. Diğer tüm durumlarda Azure n. bir veri merkezi davranılması öneririz.

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

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.
