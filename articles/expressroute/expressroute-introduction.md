<properties 
   pageTitle="ExpressRoute’a Giriş | Microsoft Azure"
   description="Bu sayfa ExpressRoute bağlantısının nasıl çalıştığı dahil olmak üzere ExpressRoute hizmetlerine genel bir bakış sağlar."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="05/02/2016"
   ms.author="cherylmc"/>

# ExpressRoute teknik genel bakış

Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir.  ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.  

![](./media/expressroute-introduction/expressroute-basic.png)

**Başlıca yararları şunlardır:**

- Bağlantı sağlayıcı üzerinden şirket içi ağınız ve Microsoft Cloud arasındaki Katman 3 bağlantısı. Herhangi bir ağdan herhangi bir (IPVPN) ağa, noktadan noktaya Ethernet bağlantısı veya Ethernet değişimi aracığıyla sanal çapraz bağlantısı üzerinden bağlantı olabilir.
- Coğrafi bölgedeki tüm bölgeler arasında Microsoft bulut hizmetlerine erişim.
- ExpressRoute premium eklentisine sahip tüm bölgelere arasında Microsoft hizmetlerine genel bağlantı.
- Endüstri standardı protokolleri (BGP) üzerinden ağınız ve Microsoft arasında dinamik yönlendirme.
- Yüksek güvenilirlik için her eşleme konumunda yerleşik yedeklilik.
- Bağlantı çalışma süresi [SLA](https://azure.microsoft.com/support/legal/sla/).
- QoS ve Skype Kurumsal gibi özel uygulamalar için hizmetlerin çoklu sınıflarına yönelik destek.

Daha fazla ayrıntı için bkz. [ExpressRoute SSS](expressroute-faqs.md).

## <a name="howtoconnect"></a>ExpressRoute kullanarak ağımı Microsoft’a nasıl bağlarım?

Şirket içi ağlarınız ve Microsoft bulutu arasında üç farklı yolla bağlantı oluşturabilirsiniz:

### Bulut değişiminde ortak yerleşim

Bulut değişimine sahip bir tesiste ortak yerleşime sahipseniz ortak yerleşim sağlayıcının Ethernet değişimi üzerinden sanal çapraz bağlantıları Microsoft ağına sıralayabilirsiniz. Ortak yerleşim sağlayıcıları, ortak yerleşim tesisi ve Microsoft bulutu altyapısı arasında Katman 2 çapraz bağlantıları veya yönetilen Katman 3 çapraz bağlantılarını sağlayabilir. 

### Noktadan noktaya Ethernet bağlantıları 

Noktadan noktaya Ethernet bağlantıları üzerinden şirket içi veri merkezleri/ofislerinizi Microsoft bulutuna bağlayabilirsiniz. Noktadan noktaya Ethernet sağlayıcıları, siteniz ve Microsoft bulutu arasında Katman 2 bağlantıları veya yönetilen Katman 3 bağlantılarını sağlayabilir.

### Herhangi bir ağdan herhangi bir (IPVPN) ağa

WAN’ınızı Microsoft bulutu ile tümleştirebilirsiniz. IPVPN sağlayıcıları (genellikle MPLS VPN) şubeleriniz ve veri merkezleriniz arasında herhangi bir yerden herhangi bir yere bağlantı sunabilir. Herhangi diğer bir şube gibi görünmesi sağlamak için Microsoft bulutu ile WAN’ınız birbirine bağlanabilir.  WAN sağlayıcıları genel olarak yönetilen Katman 3 bağlantısı sağlar. ExpressRoute yetenekleri ve özellikleri yukarıdaki tüm bağlantı modelleri arasında aynıdır. 

Bağlantı sağlayıcıları bir veya daha fazla bağlantı modeli sunabilir. Sizin için en iyi modeli seçmek için bağlantı sağlayıcınız ile çalışabilirsiniz.

![](./media/expressroute-introduction/expressroute-connectivitymodels.png)



## ExpressRoute özellikleri

ExpressRoute aşağıdaki özellikleri ve yetenekleri destekler: 

### Katman 3 bağlantısı

Microsoft, şirket içi ağınız ile Azure ve Microsoft ortak adreslerinde bulunan örnekleriniz arasındaki yolları değiştirmek için endüstri standardı dinamik yönlendirme protokolünü (BGP) kullanır.  Farklı trafik profilleri için ağınızda çoklu BGP oturumları kuruyoruz. Daha fazla bilgi [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md) makalesinde bulunabilir. 

### Yedeklilik

Her ExpressRoute bağlantı hattı, bağlantı sağlayıcısından veya ağınızın kenarından Microsoft Kurumsal kenar yönlendiricilerine (MSEEs) yapılan iki bağlantıdan oluşur. Microsoft, her bir MSEE için bir adet olmak üzere bağlantı sağlayıcısından veya sizin tarafınızdan ikili BGP bağlantısı gerektirecektir. Kendi tarafınızdaki yedekli cihazlara veya Ethernet bağlantı hattına dağıtmamayı seçebilirsiniz. Ancak, bağlantı sağlayıcılar bağlantılarınızın yedekli olarak Microsoft’a devredildiğinden emin olmak için yedekli cihazlar kullanır. Yedekli Layer 3 bağlantı yapılandırması [SLA](https://azure.microsoft.com/support/legal/sla/)’mızın geçerli olması için bir gereksinimdir. 

### Microsoft bulut hizmetlerine bağlantı

ExpressRoute bağlantıları aşağıdaki hizmetlere erişim sağlar:

- Microsoft Azure hizmetleri
- Microsoft Office 365 hizmetleri
- Microsoft CRM Online hizmetleri 
 
ExpressRoute üzerinde desteklenen hizmetlerin detaylı listesi için [ExpressRoute SSS](expressroute-faqs.md) sayfasını ziyaret edebilirsiniz.

### Coğrafi konumdaki tüm bölgelere bağlantı

[Eşleme konumlarımızın](expressroute-locations.md) biriyle Microsoft’a bağlanabilir ve coğrafi konum içindeki tüm bölgelere erişebilirsiniz.  

Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlandıysanız Kuzey Avrupa ve Batı Avrupa’da barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. Jeopolitik bölgeler, ilişkili Microsoft bulut bölgeleri ve karşılık gelen ExpressRoute eşleme konumlarına genel bakış için [ExpressRoute iş ortakları ve eşleme konumları](expressroute-locations.md) makalesine bakın.

### ExpressRoute premium eklentisine ile genel bağlantı

Coğrafi sınırlar arasındaki bağlantıyı genişletmek için ExpressRoute premium eklenti özelliğini etkinleştirebilirsiniz. Örneğin, Microsoft’a Amsterdam’da ExpressRoute aracılığıyla bağlanırsanız dünyadaki tüm bölgelerde (ulusal bulutlar dışında) barındırılan tüm Microsoft bulut hizmetlerine erişiminiz olur. Güney Amerika ve Avustralya’da dağıtılan hizmetlere Kuzey ve Batı Avrupa bölgeleriyle aynı şekilde erişebilirsiniz.

### Zengin bağlantı iş ortağı ekosistemi

ExpressRoute sürekli büyüyen bağlantı sağlayıcıları ve SI ortakları ekosistemine sahiptir. En yeni bilgiler için [ExpressRoute sağlayıcıları ve konumları](expressroute-locations.md) makalesine başvurabilirsiniz.

### Ulusal bulutlara bağlantı

Microsoft, özel coğrafi bölgeler ve müşteri kesimine yönelik yalıtılmış bulut ortamlarını çalıştırır. Ulusal bulutlar ve sağlayıcılar listesi için [ExpressRoute sağlayıcıları ve konumları](expressroute-locations.md) sayfasına başvurun.

### Desteklenen bant genişliği seçenekleri

ExpressRoute bağlantı hattını çeşitli sayıda bant genişlikleriyle satın alabilirsiniz. Desteklenen bant genişlikleri listesi aşağıda listelenmektedir. Sağladıkları desteklenen bant genişlikleri listesini belirlemek için bağlantı sağlayıcınıza danıştığınızdan emin olun.

- 50 Mbps
- 100 Mbps
- 200 Mbps
- 500 Mbps
- 1 Gbps
- 2 Gbps
- 5 Gbps
- 10 Gbps

### Bant genişliğini dinamik ölçeklendirme

Bağlantınızı kesmeden ExpressRoute bağlantı hattı bant genişliğini (en iyi çaba ilkesine göre) artırma olanağına sahipsiniz. 

### Esnek faturalama modelleri

Size en uygun faturalama modelini seçin. Aşağıda listelenen faturalama modelleri listesinden seçin. Daha fazla ayrıntı için [ExpressRoute SSS](expressroute-faqs.md) sayfasına başvurun. 

- **Sınırsız veri**. ExpressRoute bağlantı hattı aylık ücrete dayalı olarak ücretlendirilir ve tüm gelen, giden veri aktarımı ücretsiz olarak yer alır. 
- **Tarifeli veri**. ExpressRoute bağlantı hattı aylık ücrete dayalı olarak ücretlendirilir. Tüm gelen veri aktarımı ücretsizdir. Giden veri aktarımı, her GB veri aktarımı için ücretlendirilir. Veri aktarımı bölgelere göre farklılık gösterir.
- **ExpressRoute premium eklentisi**. ExpressRoute premium ExpressRoute bağlantı hattı üzerinde bir eklentidir. ExpressRoute premium eklentisi aşağıdaki yetenekleri sağlar: 
    - Azure ortak ve Azure özel eşleme için 4,000 yoldan 10,000 yola artırılmış yol sınırları.
    - Hizmetler için genel bağlantı. Herhangi bir bölgede oluşturulan ExpressRoute bağlantı hattı dünyada bulunan tüm diğer bölgelerdeki kaynaklara erişime sahip olur. Örneğin, Batı Avrupa’da oluşturulan bir sana ağa Silikon Vadisi’nde sağlanan bir ExpressRoute bağlantı hattı üzerinden erişilebilir.
    - Bağlantı hattı bağlantı genişliğine bağlı olarak 10’dan daha yüksek bir sınıra kadar artırılmış ExpressRoute bağlantı hattı başına VNet bağlantı sayısı.

## Sonraki adımlar

- ExpressRoute bağlantıları ve yönlendirme etki alanları hakkında bilgi edinin. Bkz. [ExpressRoute bağlantı hatları ve etki alanlarını yönlendirme](expressroute-circuit-peerings.md).
- Bir hizmet sağlayıcı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
- Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
- [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
- ExpressRoute bağlantınızı yapılandırın.
    - [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-classic.md)
    - [Yönlendirmeyi yapılandırma](expressroute-howto-routing-classic.md)
    - [ExpressRoute bağlantı hattına bir VNet bağlama](expressroute-howto-linkvnet-classic.md)



<!--HONumber=Jun16_HO2-->


