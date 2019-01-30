---
title: include dosyası
description: include dosyası
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/19/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 98ea4d78a473123708be6e371587252acad6ffcd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55205115"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Azure sanal ağ geçidi (VPN Gateway) ile Azure Sanal WAN vpngateway arasında ne fark vardır?

Sanal WAN geniş ölçekli Siteden Siteye bağlantı sağlar; aktarım hızı, ölçeklendirilebilirlik ve kullanım kolaylığı için oluşturulmuştur. ExpressRoute ve noktadan siteye bağlantı işlevleri şu anda Önizlemededir. CPE dal cihazları otomatik olarak sağlanır ve Azure Sanal WAN'a bağlanır. Bu cihazlar büyüyen bir SD-WAN ve VPN iş ortakları ekosisteminden sağlanır. Bkz. [Tercih Edilen İş Ortağı Listesi](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Başlangıçta hangi cihaz sağlayıcıları (Sanal WAN iş ortakları) desteklenir? 

Şu anda birçok iş ortağı tümüyle otomatik Sanal WAN deneyimini desteklemektedir. Daha fazla bilgi için bkz. [Sanal WAN iş ortakları](https://go.microsoft.com/fwlink/p/?linkid=2019615). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>Sanal WAN iş ortağı otomasyonu adımları nedir?

İş ortağı otomasyonu adımları için bkz. [Sanal WAN iş ortağı otomasyonu](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Tercih edilen bir iş ortağı cihazını kullanmam gerekiyor mu?

Hayır. IKEv2/IKEv1 IPsec desteğinin Azure gereksinimlerini karşılayan, VPN özellikli herhangi bir cihaz kullanabilirsiniz.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Sanal WAN iş ortakları Azure Virtual WAN ile bağlantıyı nasıl otomatik hale getirir?

Yazılım tanımlı bağlantı çözümleri normalde dal cihazlarını yönetmek için bir denetleyici veya cihaz sağlama merkezi kullanır. Denetleyici, Azure Sanal WAN'a otomatik bağlantı sağlamak için Azure API'lerini kullanabilir. Daha fazla bilgi için bkz. Sanal WAN iş ortağı otomasyonu.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>Sanal WAN mevcut bağlantı özelliklerinden herhangi birini değiştiriyor mu?   

Mevcut Azure bağlantı özelliklerinde hiçbir değişiklik yoktur.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Sanal WAN için yeni Resource Manager kaynakları sağlanıyor mu?
  
Evet, Sanal WAN yeni Resource Manager kaynakları getirir. Daha fazla bilgi için bkz. [Genel Bakış](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>Tek bir Hub’a kaç adet VPN cihazı bağlanabilir?

Sanal hub başına en çok 1000 bağlantı desteklenir. Her bağlantı, etkin-etkin yapılandırmasına sahip iki tünelden oluşur. Tüneller bir Azure Sanal Hub vpngateway'de sonlandırılır.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>Şirket içi VPN cihazı birden çok Hub’a bağlanabilir mi?

Evet. Başlarken trafik akışı şirket içi cihazdan en yakın Microsoft edge’e ve ardından Sanal Hub’a doğru gerçekleştirilir.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>Azure Sanal WAN ile Genel Sanal Ağ Eşleme destekleniyor mu? 

 Hayır.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>Sanal hub'a bağlı uç sanal ağları birbiriyle iletişim kurabiliyor mu?

Evet. Uç sanal ağları, doğrudan sanal ağ eşlemesi kurabilir. Ancak, sanal ağlar, geçişli hub'ı ile iletişim kurulurken desteklemez. Daha fazla bilgi için bkz. [Sanal Ağ Eşlemesi](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Azure Sanal WAN ile tercih ettiğim ağ sanal cihazını (bir NVA sanal ağı içinde) dağıtabilir ve kullanabilir miyim?

Evet, en sevdiğiniz ağ sanal gereci(NVA) VNet’ini Azure Sanal WAN’a bağlayabilirsiniz. İlk olarak ağ sanal gereci VNet’ini Hub Sanal Ağ bağlantısının olduğu hub’a bağlayın. Ardından sonraki atlamanın Sanal Gerece işaret ettiği bir Sana Hub yönlendirmesi oluşturun. Sanal Hub Rota Tablosuna birden fazla rota uygulayabilirsiniz. Uç VNet yollarının şirket içi sistemlere yayıldığından emin olunması için NVA VNet’e bağlı olan tüm uçların ek olarak sanal hub’a bağlı olması gerekmektedir.

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Bir NVA sanal ağının sanal ağ geçidi olabilir mi?

Hayır. NVA sanal ağı sanal hub'a bağlıysa, bunun sanal ağ geçidi olamaz. 

### <a name="is-there-support-for-bgp"></a>BGP için destek var mı?

Evet, BGP desteklenir. NVA sanal ağından gelen yolların düzgün tanıtıldığından emin olmak için, NVA sanal ağına ve dolayısıyla da bir sanal hub'a bağlı olan uçların BGP'yi devre dışı bırakması gerekir. Ek olarak uç VNet yollarının şirket içi sistemlere yayıldığından emin olmak için uç VNetlerini sanal hub’a bağlayın.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Sanal hub'da UDR kullanarak trafiği yönlendirebilir miyim?

Evet, Sanal Hub Yol Tablosunu kullanarak trafiği bir VNet’e yönlendirebilirsiniz.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Sanal WAN ile ilgili lisans ve fiyatlandırma bilgileri var mı?
 
Evet. Bkz. [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-wan/) sayfası.

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>İlk iş ortağı listenizde yer almayan yeni iş ortakları nasıl eklenir?

azurevirtualwan@microsoft.com adresine e-posta gönderin. İdeal bir iş ortağı, IKEv1 veya IKEv2 IPsec bağlantısına yönelik sağlanabilen bir cihaza sahip olandır.

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>Azure Sanal WAN’ı bir Resource Manager şablonu ile yapılandırmak mümkün mü?

Tek bir hub ve vpnsite’ı olan tek bir Sanal WAN’ın basit bir yapılandırması bir [Azure Hızlı Başlangıç Şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network) kullanılarak oluşturulabilir. Sanal WAN birincil olarak bir REST veya Portal odaklı bir hizmettir.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>Sanal WAN’da daldan dala bağlantıya izin verilir mi?

Evet, daldan dala bağlantı VPN için Sanal WAN’da ve VPN ile ExpressRoute arasında kullanılabilir. VPN siteden siteye GA iken ExpressRoute ve noktadan siteye şu anda önizlemededir.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>Daldan dala trafik Azure Sanal WAN üzerinden geçiş yapar mı?

Evet.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>Sanal WAN, mevcut Azure Sanal Ağ’dan nasıl farklıdır?

Sanal Ağ Geçidi VPN’i 30 tünelle sınırlıdır. Bağlantılar için, büyük ölçekli VPN’lere yönelik Sanal WAN kullanmanız gerekir. Orta Batı ABD bölgesi dışındaki tüm bölgelerde hub'da 2 Gb/sn hıza sahip 1000 dal bağlantısı kurabilirsiniz. Orta Batı ABD bölgesinde 20 Gb/sn hızdan faydalanabilirsiniz. 20 Gb/sn desteğini ileride başka bölgelerde de kullanıma sunacağız. Bağlantı şirket içi VPN cihazından sanal hub’a giden bir etkin-etkin tüneldir. Bölge başına bir hub’ınız olabilir. Yani hub’lar arasında 1000’den fazla dal bağlayabilirsiniz.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>Bu Sanal WAN her siteden ExpressRoute alınmasını gerektirir mi?

Hayır, Sanal WAN her siteden ExpressRoute alınmasını gerektirmez. Cihazdan Azure Sanal WAN merkezine kurulan İnternet bağlantıları aracılığıyla standart IPsec siteden siteye bağlantı kullanır. Siteleriniz ExpressRoute bağlantı hattı kullanılarak bir sağlayıcı ağına bağlanabilir. Sanal Hub’da ExpressRoute kullanılarak bağlanan Siteler için (Önizlemede) VPN ile ExpressRoute arasında daldan dala trafik akışı olabilir. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>Azure Sanal WAN kullanırken bir ağ aktarım hızı sınırı var mı?

Dal sayısı merkez/bölge başına 1000 bağlantı ve merkezde toplam 2 G ile sınırlıdır. Yalnızca Orta Batı ABD bölgesi toplam 20 GB/sn hız sunmaktadır. 20 Gb/sn hızı ileride daha fazla bölgede sunuyor olacağız.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>Sanal WAN, şirket içi cihazın paralel olarak birden fazla ISS’den yararlanmasına izin verir mi, yoksa her zaman tek bir VPN tüneli mi var?

Evet, dal cihazına bağlı olarak tek bir daldan etkin-etkin tünelleriniz (2 tünel = 1 Azure Sanal WAN bağlantısı) olabilir.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>Azure temelinde trafik nasıl yönlendirilir?

Trafik şu düzeni izler: dal cihazı ->ISS->Microsoft Edge->Microsoft DC->Microsoft Edge->ISS->dal cihazı

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>Bu modelde her sitede neye ihtiyacınız var? Yalnızca interneti bağlantısına mı?

Evet. İnternet bağlantısı ve tercihen tümleşik [iş ortaklarımızdan](https://go.microsoft.com/fwlink/p/?linkid=2019615) bir fiziksel cihaz. İsteğe bağlı olarak Azure bağlantısını ve yapılandırmasını tercih ettiğiniz cihazdan el ile yönetebilirsiniz.
