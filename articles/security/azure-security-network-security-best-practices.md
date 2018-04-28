---
title: En iyi güvenlik uygulamaları, Azure ağı | Microsoft Docs
description: Bu makalede Azure özellikleri yerleşik güvenlik ağ kullanmaya yönelik en iyi yöntemler kümesi sağlar.
services: security
documentationcenter: na
author: TomShinder
manager: swadhwa
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: d6d723f40cdc0382fa41a51eb32e7b59f0798627
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-network-security-best-practices"></a>Azure ağı en iyi güvenlik uygulamaları
Microsoft Azure sanal makineleri ve cihazları diğer ağ bağlantılı cihazlar için Azure sanal ağlarda koyarak bağlamanıza olanak sağlar. Bir Azure sanal ağı etkin ağ aygıtları arasındaki TCP tabanlı iletişime izin vermek için sanal bir ağa sanal ağ arabirim kartları bağlanmanıza olanak sağlayan bir yapıdır. Bir Azure sanal ağına bağlı Azure sanal makineleri aynı Azure sanal ağı, farklı Azure sanal ağlar, Internet'te veya kendi şirket içi ağlarda bile cihazlarda bağlanabilir.

Bu makalede, en iyi güvenlik uygulamaları, Azure ağ topluluğu açıklanmaktadır. Bu en iyi uygulamaları Azure ağ ile deneyimi bizim türetilmiş ve müşteri deneyimleri bulunun.

En iyi her uygulama için bu makalede açıklanmaktadır:

* En iyi uygulama nedir
* Bu en iyi uygulama etkinleştirmek istediğiniz neden
* En iyi uygulama olarak etkinleştirmek başarısız olursa ne sonucu olabilir
* En iyi uygulama için olası alternatifler
* Nasıl en iyi uygulama olarak etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure ağ güvenlik en iyi yöntemler makalesi anlaşma fikir ve Azure platformu özellikleri ve özellik kümeleri dayanır. Zaman içinde görüşlerini ve teknolojileri değiştirebilirsiniz ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure ağ en iyi yöntemler şunlardır:

* Mantıksal kesim alt ağlar
* Yönlendirme davranışını denetlemek
* Zorlanan tünel etkinleştir
* Sanal ağ cihazları kullanın
* Güvenlik bölgesi için DMZ'ler dağıtma
* Adanmış WAN bağlantılarını Internet'le maruz kaçının
* Açık kalma süresi ve performansı en iyi duruma getirme
* Genel Yük Dengelemesi
* Azure sanal makinelere RDP erişimini devre dışı bırakma
* Etkinleştirme Azure Güvenlik Merkezi
* Veri merkeziniz Azure genişletme

## <a name="logically-segment-subnets"></a>Mantıksal kesim alt ağlar
[Azure sanal ağlar](https://azure.microsoft.com/documentation/services/virtual-network/) , şirket içi ağınızda LAN benzerdir. Üzerinde yerleştirebileceğiniz tüm tek bir özel IP adresi alanı tabanlı ağı oluşturma fikirdir bir Azure sanal ağı arkasında, [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/). Sınıf A (10.0.0.0/8), sınıf B (172.16.0.0/12), özel IP adres alanlarının kullanılabilir olduğundan ve C sınıfı (192.168.0.0/16) aralıkları.

Benzer şekilde ne şirket içi yapmak için büyük bir adres alanı alt ağlara ayırabilir. Kullanabileceğiniz [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) , alt ağlar oluşturmak için alt ağları kurallara göre.

Alt ağlar arasında yönlendirme otomatik olarak gerçekleşir ve yönlendirme tablolarını el ile yapılandırmanız gerekmez. Ancak, varsayılan ayarı vardır hiçbir ağ erişim denetimlerini Azure sanal ağ oluşturma alt ağlar arasında olmasıdır. Alt ağlar arasında ağ erişim denetimleri oluşturmak için alt ağlar arasında bir şey put gerekir.

Bu görevi gerçekleştirmek için kullanabileceğiniz özelliklerinden biri olan bir [ağ güvenlik grubu](../virtual-network/virtual-networks-nsg.md) (NSG). Nsg'ler olan 5-tanımlama grubu (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve katman 4 Protokolü) kullanan basit durum bilgisi olan paket incelemesi cihazlara izin verme/reddetme oluşturmak için bir yaklaşım ağ trafiği için kuralları. İzin vermek veya tek IP adresi için ve birden çok IP adresi veya hatta ve tüm alt ağlar gelen ve giden trafiği reddetmek.

Nsg'ler alt ağlar arasında ağ erişim denetimi için kullanarak, aynı güvenlik bölgesi ya da kendi alt rolünde ait kaynakları yerleştirilmesine olanak sağlar. Örneğin bir web katmanı, bir uygulama mantığı katmanından ve veritabanı katmanından olan bir basit bir 3 katmanlı uygulama düşünün. Her bu katmanların kendi alt ait sanal makineler yerleştirin. Daha sonra alt ağlar arasında trafiği denetlemek için Nsg'ler kullanın:

* Web Katmanı sanal makineler yalnızca uygulama mantığını makinelere bağlantılar başlatabilir ve yalnızca Internet'ten gelen bağlantıları kabul edebilir
* Uygulama mantığı sanal makineler yalnızca veritabanı katmanı ile bağlantılar başlatabilir ve yalnızca web katmanı gelen bağlantıları kabul edebilir
* Veritabanı katmanı sanal makineleri, kendi alt ağı dışında herhangi bir şeyle bağlantı başlatılamıyor ve yalnızca uygulama mantığı katmanı gelen bağlantıları kabul edebilir

Ağ güvenlik grupları ve mantıksal olarak Azure sanal ağlarınıza segmentlere ayırmak için bunları nasıl kullanabileceğiniz hakkında daha fazla bilgi için bkz: [bir ağ güvenlik grubu nedir](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Yönlendirme davranışını denetlemek
Bir Azure sanal ağ üzerinde bir sanal makine geçirdiğinizde, diğer sanal makineler farklı alt ağlarda olsa bile, sanal makine aynı Azure sanal ağ üzerindeki diğer sanal makineye bağlanabildiğinizi görürsünüz. Bu tür iletişim izin varsayılan olarak etkin olan sistem yolları koleksiyonunu olduğundan, bu mümkündür. Bu varsayılan yolları sanal makineleri aynı Azure sanal ağda birbirleriyle ve Internet (için yalnızca Internet giden iletişimler) ile bağlantı başlatmasına izin verin.

Varsayılan sistem yolları birçok dağıtım senaryoları için faydalı olsa da, dağıtımlarınız için yönlendirme yapılandırması özelleştirmek istediğiniz zaman zamanlar vardır. Bu özelleştirmeler belirli hedeflere ulaşmak için sonraki atlama adresi yapılandırmanıza izin verir.

Sonraki en iyi uygulama ele alınan bir sanal ağ güvenlik uygulaması dağıttığınızda, kullanıcı tanımlı rotaları yapılandırmanız önerilir.

> [!NOTE]
> Kullanıcı tanımlı yollar gerekli değildir ve çoğu durumda varsayılan sistem yolları çalışır.
>
>

Kullanıcı tanımlanan yollar ve bunların makalesini okuyarak nasıl yapılandırılacağı hakkında daha fazla bilgiyi [kullanıcı tanımlı yollar ve IP iletimi nedir](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Zorlanan tünel etkinleştir
Zorlamalı tünel daha iyi anlamak için hangi "bölünmüş tünel" olduğunu anlamak kullanışlıdır.
Bölünmüş tünel en yaygın örnek VPN bağlantıları ile görülür. Şirket ağınıza, otel oda VPN bağlantısı kuracak düşünün. Bu bağlantı, şirket kaynaklarına erişmek için şirket ağınıza yapılan tüm bağlantılar VPN tünelinden gidip verir.

Internet üzerindeki kaynaklara bağlanmak istediğiniz ne olur? Bölünmüş tünel etkinleştirildiğinde, bu bağlantılar doğrudan Internet'e ve VPN tünelinden gidin. Bazı güvenlik Uzmanları bu potansiyel bir risk olmasını ve bu nedenle bölünmüş tüneli devre dışı bırakılır önerilir ve tüm bağlantıları göz önünde bulundurun. Internet'e ve şirket kaynakları için hedefleyen bağlantıları için hedeflenen bağlantılar VPN tünelinden gitmeniz gerekir. Bunu yapmanın avantajı, Internet bağlantısı VPN istemcisi VPN tüneli dışında Internet'e bağlı olduğu durumda olmayacaktır şirket ağı güvenlik aygıtları aracılığıyla sonra zorlanır ' dir.

Şimdi bir Azure sanal ağdaki sanal makineler için şimdi bu geri getirin. Bir Azure sanal ağı için varsayılan yolların Internet trafiği başlatmak için sanal makineleri izin verin. Bu giden bağlantılar bir sanal makineyi saldırı yüzeyini artırabilir ve saldırganlar tarafından işlevden bu bir güvenlik riski çok temsil edebilir.
Bu nedenle, şirket içi ağınızı Azure sanal ağınız arasında şirketler arası bağlantı varsa, sanal makinelere zorlamalı tünel etkinleştirmenizi öneririz. Şirket içi bağlantı, bu belgede daha sonra Azure ağ en iyi yöntemler ele alınmıştır.

Bir şirket içi ve dışı bağlantı yoksa, avantajından ağ güvenlik grupları (daha önce bahsedilen) ya da Azure sanal emin olun Internet'e giden bağlantılar, Azure sanal engellemek için güvenlik Gereçleri (ele yanında) ağ Makineler.

Zorlamalı tünel ve bunun nasıl etkinleştirileceğini hakkında daha fazla bilgi için bkz: [yapılandırma PowerShell ve Azure Resource Manager kullanarak zorlamalı tüneli](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Sanal ağ cihazları kullanın
Ağ güvenlik grupları ve kullanıcı tanımlı yönlendirme ağ güvenlik ağ ve Aktarım katmanı en belirli bir ölçü sağlayabilir sırada [OSI modeli](https://en.wikipedia.org/wiki/OSI_model), var olan geçmeye, istediğiniz veya olduğu etkinleştirmek gereken durumlar olabilir Güvenlik, yüksek düzeyde yığınının. Böyle durumlarda, Azure iş ortakları tarafından sağlanan sanal ağ güvenlik Gereçleri dağıtmanızı öneririz.

Azure ağı güvenlik Gereçleri gelişmiş düzeyde güvenlik, ağ düzeyinde denetimler tarafından sağlanan üzerinden sunabilir. Sanal ağ güvenlik Gereçleri tarafından sağlanan ağ güvenlik özelliklerden bazıları şunlardır:

* Saldırısından
* Yetkisiz erişim algılama/izinsiz giriş önleme
* Güvenlik Açığı Yönetimi
* Uygulama denetimi
* Ağ tabanlı anomali algılama
* Web filtreleme
* Virüsten koruma
* Botnet koruma

Ağ düzeyinde erişim denetimleriyle edinebilirsiniz olandan daha yüksek düzeyde ağ güvenlik gerektiriyorsa, ardından araştırın ve Azure sanal ağı güvenlik Gereçleri dağıtmak öneririz.

Hangi Azure sanal ağ ile ilgili güvenlik Gereçleri kullanılabilir ve yeteneklerini hakkında ziyaret öğrenmek için [Azure Marketi](https://azure.microsoft.com/marketplace/) ve "güvenlik" ve "ağ güvenliği" için arama

## <a name="deploy-dmzs-for-security-zoning"></a>Güvenlik bölgesi için DMZ'ler dağıtma
DMZ veya "çevre ağ" ek bir varlıklarınızı ve Internet arasındaki güvenlik katmanı sağlamak üzere tasarlanmış bir fiziksel veya mantıksal ağ kesimi. Ağ güvenlik aygıtı geçmiş ve Azure sanal ağınızda yalnızca istenen trafiğe izin DMZ amacı özel bir ağ erişim denetimi aygıtları DMZ ağ kenarına yerleştirmektir.

Ağ erişim denetimi Yönetimi izleme, günlüğe kaydetme ve cihazlarda Azure sanal ağınızda kenarında rapor odaklanabilirsiniz sağladığından DMZ'ler kullanışlıdır. Burada genellikle DDoS önleme, izinsiz giriş algılama/izinsiz giriş önleme sistemleri (Kimlikleri/IP), güvenlik duvarı kuralları ve ilkeleri, web filtreleme, ağ kötü amaçlı yazılımdan koruma ve daha fazlasını etkinleştirmeniz. Ağ güvenlik cihazlar Azure sanal ağınızda ve Internet arasında yaslanın ve her iki ağda bir arabirim.

DMZ, temel tasarım ederken vardır birçok farklı DMZ tasarımı, aşağıdaki gibi uçtan uca, üç konaklı, çok konaklı ve diğerleri.

Azure kaynaklarınız için ağ güvenlik düzeyini artırmak için DMZ dağıtmayı göz önünde bulundurun tüm yüksek güvenlikli dağıtımlarda öneririz.

DMZ'ler ve Azure'da dağıtma hakkında daha fazla bilgi için bkz: [Microsoft bulut Hizmetleri ve ağ güvenliği](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Adanmış WAN bağlantılarını Internet'le maruz kaçının
Birçok kuruluş karma BT rota seçtiniz. Başkalarının şirket içi kalırken karma BT şirketinizin bilgi varlıklarını bazıları Azure içinde değişir. Diğer bileşenler şirket içi kalırken çoğu durumda, bir hizmetin bazı bileşenleri Azure'da çalıştırırsınız.

Karma BT senaryo genellikle şirket içi bağlantılar tür yok. Bu şirket içi ağlarını Azure sanal ağlara bağlanmak üzere olanak tanıyan şirketler arası. İki şirket içi bağlantısı çözümlerini vardır:

* Konumdan konuma VPN
* ExpressRoute

[Siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) şirket içi ağınız ve Azure sanal ağ arasında özel bir sanal bağlantı temsil eder. Bu bağlantının Internet üzerinden gerçekleşir ve ağınız ve Azure arasında şifrelenmiş bir bağlantısı içinde "tünel" bilgileri sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. Tünel şifreleme kullanarak gerçekleştirilir [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx).

Siteden siteye VPN güvenilir, güvenilir ve yerleşik bir teknoloji olsa da, tünel içindeki trafiği Internet çapraz geçiş. Ayrıca, bant genişliği en fazla 200 MB/sn için görece sınırlı değildir.

Şirket içi bağlantılarınız için güvenlik ve performans olağanüstü bir düzeyi gerekiyorsa, şirket içi bağlantılar için Azure ExpressRoute kullanmanızı öneririz. Expressroute'dur ayrılmış WAN şirket içi konumunuz veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. Bu telco bağlantı olduğundan, verilerinizi Internet üzerinden yolculuk değil ve bu nedenle Internet iletişimlerde devralınmış olası riskleri maruz kalmaz.

Azure ExpressRoute nasıl çalıştığını ve nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Açık kalma süresi ve performansı en iyi duruma getirme
Gizlilik, bütünlük ve kullanılabilirlik (CIA) bugünün en etkili güvenlik modeli Üçlü kapsar. Gizliliği şifreleme hakkında ve gizlilik, bütünlük verileri yetkisiz personeli tarafından değiştirilmez ve yetkili kişiler erişmeye yetkili bilgi erişimi mümkün olduğundan emin olmanızı hedefler kullanılabilirliği olduğundan emin olmanızı hedefler. Bu alanlar herhangi birinde başarısızlığı olası bir ihlali güvenlik temsil eder.

Kullanılabilirlik, açık kalma süresi ve performans hakkında olduğu düşünülebilir. Bir hizmeti çalışmıyorsa, bilgi erişilemiyor. Performans verileri kullanılamaz hale konusunda kadar düşük olduğunda, biz verilerin erişilemez düşünebilirsiniz. Bu nedenle, güvenlik açısından bakıldığında, biz hizmetlerimizle en iyi çalışır durumda kalma süresi ve performans emin olmak için ne olursa olsun kuruluşunuzun yöneticisiyle yapmanız gerekir.
Kullanılabilirlik ve performansı artırmak için kullanılan popüler ve etkili bir yöntemi, Yük Dengeleme kullanmaktır. Yük Dengeleme, bir hizmetin parçası olan sunucular arasında ağ trafiğini dağıtmanın bir yöntemdir. Ön uç web sunucuları, hizmetin bir parçası varsa, örneğin, Yük Dengeleme, birden çok ön uç web sunucusu arasında trafiği dağıtmak için kullanabilirsiniz.

Web sunuculardan biri kullanılamaz duruma gelirse, yük dengeleyici bu sunucuya trafik göndermeyi durdurur ve hala çevrimiçi olduğu sunucuya yeniden çünkü bu dağıtım trafik kullanılabilirliğini artırır. İşlemci ve ağ tüm yük yükünü isteklere hizmet vermek için Dağıtılmış bellek sunucuları dengeli çünkü Yük Dengeleme de performans, yardımcı olur.

Yük Dengeleme yapabilecekleriniz olduğunda ve hizmetleriniz için uygun şekilde kullanması önerilir. Aşağıdaki bölümlerde site adres: Azure sanal ağ düzeyinde Azure üç birincil sizinle Yük Dengeleme seçenekleri sağlar:

* HTTP tabanlı Yük Dengeleme
* Dış Yük Dengeleme
* İç yük dengeleyici

## <a name="http-based-load-balancing"></a>HTTP tabanlı Yük Dengeleme
HTTP tabanlı Yük Dengeleme özelliklerini HTTP protokolünü kullanarak bağlantı göndermek için hangi sunucu hakkında kararlar alır. Azure uygulama ağ geçidi adı tarafından gelecek bir HTTP yük dengeleyici sahiptir.

Azure uygulama ağ geçidi kullanmanızı öneririz zaman:

* Aynı kullanıcı/istemci oturumunun aynı arka uç sanal makinesine ulaşmaya yönelik isteklerini gerektiren uygulamalar. Bu örnekleri Sepeti uygulamaları ve web posta sunucuları alışveriş.
* Web sunucusu gruplarında SSL sonlandırma gelen ek yükü uygulama ağ geçidi yararlanarak serbest bırakmak istediğiniz uygulamaları [SSL boşaltma](https://f5.com/glossary/ssl-offloading) özelliği.
* Aynı uzun süreli TCP bağlantısı üzerinde birden fazla HTTP isteğinin yönlendirilmesini veya farklı arka uç sunucularına yük dengelemesi yapılmasını gerektiren, içerik teslim ağı gibi uygulamalar.

Azure uygulama ağ geçidi nasıl çalıştığı ve nasıl dağıtımlarınızda kullanabileceğiniz hakkında daha fazla bilgi için bkz: [uygulama ağ geçidi'ne genel bakış](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Dış Yük Dengeleme
Internet'ten gelen bağlantıları bir Azure sanal ağınızda bulunan, sunucular arasında dengeli olduğunda dış Yük Dengeleme gerçekleşir. Azure dış yük dengeleyici, bu özellik sağlayabilirsiniz ve gerektiğinde bunu Yapışkan oturumları gerektirmeyen ya da SSL boşaltma kullanmanızı öneririz.

HTTP tabanlı Yük Dengeleme aksine, dış yük dengeleyici bilgileri OSI Ağ modelinin ağ ve Aktarım katmanı Yük Dengeleme bağlantısı için hangi sunucusunda kararlar almak için kullanır.

Sahip olduğunuz her durumda, dış Yük Dengeleme kullanmanızı öneririz [durum bilgisiz uygulamaların](http://whatis.techtarget.com/definition/stateless-app) Internet'ten gelen istekleri kabul.

Azure dış yük dengeleyici çalışır ve onu nasıl dağıtabileceğiniz nasıl görürüm hakkında daha fazla bilgi edinmek için [PowerShell kullanarak Kaynak Yöneticisi'nde bir Internet'e yönelik Yük dengeleyicinin oluşturmaya başlamak](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>İç Yük Dengeleme
İç Yük Dengeleme dış Yük Dengeleme için benzer ve Bakiye bağlantıları arkasına sunuculara yüklemek için aynı mekanizmayı kullanır. Yük Dengeleyici, bu durumda Internet'te olmayan sanal makineleri gelen bağlantıları kabul yalnızca farktır. Çoğu durumda, Yük Dengeleme için kabul edilen bağlantılarda bir Azure sanal ağı üzerindeki cihazlar tarafından başlatılır.

İç Yük Dengeleme Bakiye bağlantıları SQL veya iç web sunucularındaki yük gerektiğinde gibi bu özelliği, yararlı senaryoları için kullanmanızı öneririz.

Azure iç Yük Dengeleme nasıl çalıştığını ve nasıl dağıtabileceğiniz hakkında daha fazla bilgi için bkz: [PowerShell kullanarak bir iç yük dengeleyici oluşturmaya başlamak](../load-balancer/load-balancer-get-started-ilb-arm-ps.md).

## <a name="use-global-load-balancing"></a>Genel Yük Dengelemesi
Genel bulut bilgi işlem yapar genel olarak dağıtmak mümkün dağıtılmış tüm dünyadaki veri merkezlerinde bulunan bileşenleriniz uygulamalar. Bu Azure küresel veri merkezi varlığı nedeniyle Microsoft Azure üzerinde mümkün olur. Yük Dengeleme daha önce bahsedilen teknolojilerini aksine genel Yük Dengeleme, hatta tüm veri merkezlerinde kullanılamaz hale gelebilir kullanılabilir hizmetler yapmak mümkün kılar.

Bu tür Azure'daki yararlanarak genel Yük Dengeleme alabilirsiniz [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/). Kullanıcının konumuna göre Services Bakiye bağlantıları yüklemek, trafik Yöneticisi yapar mümkündür.

Örneğin, kullanıcının AB hizmetiniz için bir istek değiştirirken, bir AB veri merkezinde bulunan hizmetlerinize bağlantı yönlendirilir. Bu bölümü trafik Yöneticisi'nin genel yüklemek için en yakın veri merkezini bağlayan uzakta olan veri merkezleri bağlayan daha hızlı olduğu için performansı artırmak için karşı yardımcı olur.

Kullanılabilirlik tarafında genel Yük Dengeleme veri merkezinin tamamı kullanılamaz hale olsa bile hizmetinizi kullanılabilir emin olur.

Azure veri merkezinde ortam nedenlerden dolayı veya kesintiler (örneğin, bölgesel ağ hataları) nedeniyle kullanılamaz duruma gelirse, örneğin, hizmetinize bağlantılar için yönlendirilmesi çevrimiçi datacenter en yakın. Bu genel yük dengelemesi, trafik Yöneticisi'nde oluşturabilirsiniz DNS ilkeleri yararlanarak elde edilir.

Trafik Yöneticisi, birçok bölgeye yaygın olarak dağıtılan bir kapsama sahip ve çalışır durumda kalma süresi olası en yüksek düzeyde gerektiren geliştirme tüm bulut çözümü için kullanmanızı öneririz.

Azure Traffic Manager ve nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [trafik Yöneticisi nedir](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Azure sanal makinelere RDP/SSH erişimini devre dışı bırakma
Kullanarak Azure sanal makineleri ulaşması mümkündür [Uzak Masaüstü Protokolü](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ve [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokoller. Bu protokollerin uzak konumlardaki sanal makineleri yönetmek mümkün kılar ve veri merkezi bilgi standart.

Bu protokoller Internet üzerinden kullanarak olası güvenlik sorunu saldırganlar çeşitli kullanabilmenizdir [yanılma](https://en.wikipedia.org/wiki/Brute-force_attack) erişmek için Azure sanal makineler için teknikleri. Saldırganlar erişim sonra sanal makinenizi Azure sanal ağınızdaki diğer makinelerin tehlikeye için bir başlatma noktası kullanın veya hatta ağa bağlı aygıtları Azure dışında saldırı.

Bu nedenle, doğrudan RDP ve SSH erişim Azure sanal makineleri için Internet'ten devre dışı bırakmanızı öneririz. Internet'ten doğrudan RDP ve SSH erişimi devre dışı bırakıldıktan sonra Uzaktan Yönetim için bu sanal makinelere erişmek için kullanabileceğiniz diğer seçeneğiniz vardır:

* Noktadan siteye VPN
* Konumdan konuma VPN
* ExpressRoute

[Noktadan siteye VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) uzaktan erişim VPN istemci/sunucu bağlantısı için başka bir terimdir. Bir noktadan siteye VPN Internet üzerinden bir Azure sanal ağa bağlanmak tek bir kullanıcı sağlar. Noktadan siteye bağlantı kurulduktan sonra kullanıcı kullanıcı için noktadan siteye VPN aracılığıyla bağlanan Azure sanal ağda bulunan tüm sanal makinelere bağlanmak için RDP veya SSH kullanmanız mümkün olacaktır. Bu kullanıcının bu sanal makinelere erişmek için yetkili varsayar.

Bir sanal makine için iki kez bağlanmadan önce kimliğini doğrulamak kullanıcının sahip olduğu noktadan siteye VPN doğrudan RDP veya SSH bağlantılarına göre daha güvenlidir. İlk olarak, kullanıcı kimlik doğrulaması (ve yetkili olması) gerekir; noktadan siteye VPN bağlantısı kurmak için İkinci olarak, kullanıcının kimlik doğrulaması (ve yetkilendirilmesi) gerektiği RDP veya SSH oturumu.

A [siteden siteye VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) tüm ağ Internet üzerinden başka bir ağa bağlanır. Şirket içi ağınızı Azure sanal ağa bağlanmak için siteden siteye VPN kullanabilirsiniz. Şirket içi ağınız üzerinde siteden siteye VPN, kullanıcıların dağıtırsanız siteden siteye VPN bağlantısı üzerinden RDP veya SSH protokolünü kullanarak sanal makinelere Azure sanal ağınıza bağlanmak oluşturabilecek ve doğrudan RDP veya SSH erişime izin verecek şekilde gerektirmez Internet üzerinden.

Ayrılmış bir WAN bağlantısı, siteden siteye VPN benzer işlevsellik sağlamak için de kullanabilirsiniz. 1 temel farklılıklar şunlardır. ayrılmış bir WAN bağlantısı Internet ve 2 çapraz geçiş değil. ayrılmış WAN genellikle daha fazla kararlı ve kullanıcı bağlantılardır. Azure, ayrılmış bir WAN bağlantısı çözümü biçiminde sağlar [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Etkinleştirme Azure Güvenlik Merkezi
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve artan, görünürlük ve denetim üzerinden, Azure kaynaklarınızın güvenliğini sağlar. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Azure Güvenlik Merkezi en iyi duruma getirme ve ağ güvenliği tarafından izlemenize yardımcı olur:

* Ağ güvenlik önerileri sağlama
* Ağ güvenliği yapılandırması durumunu izleme
* Temel ağ tehditlerinden her iki uç ve ağ düzeyinde konusunda sizi uyarmayı

Azure Güvenlik Merkezi tüm Azure dağıtımları için etkinleştirmenizi öneririz.

Azure Güvenlik Merkezi ve dağıtımlarınızda etkinleştirme hakkında daha fazla bilgi için bkz: [Azure Güvenlik Merkezi'ne giriş](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Güvenli bir şekilde veri merkeziniz Azure genişletme
Birçok kuruluş BT kuruluşları, şirket içi veri merkezlerini büyüyen yerine buluta genişletmek için arama. Bu genişleme uzantı var olan BT altyapısının genel buluta temsil eder. Şirketler arası bağlantı seçenekleri yararlanarak, şirket içi ağ altyapısında yalnızca başka bir alt ağ olarak Azure sanal ağlarınıza işlemek mümkündür.

Ancak, ilk önce ele alınması gereken planlama ve tasarım sorunları vardır. Bu ağ güvenliği alanında özellikle önemlidir. Bu tür bir tasarım yaklaşımını nasıl anlamak için en iyi yolu bir örnek görmek için biridir.

Microsoft oluşturdu [Datacenter uzantısı başvuru mimarisi diyagramı](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) ve böyle bir veri merkezi uzantısı aşağıdaki gibi görünür anlamanıza yardımcı olması için malzemeleri destekleme. Bu planlama ve bir güvenli kurumsal veri merkezi uzantısı buluta tasarlamak için kullanabileceğiniz örnek başvuru uygulaması sağlar. Güvenli bir çözümdür anahtar bileşenleri hakkında bir fikir edinmek için bu belgeyi gözden geçirmenizi öneririz.

Güvenli bir şekilde veri merkeziniz Azure genişletme hakkında daha fazla bilgi için videosunu görüntüleyin [genişletme bilgisayarınızı Datacenter Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA).
