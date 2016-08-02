<properties
   pageTitle="Azure Virtual Network'e (VNet) Genel Bakış"
   description="Azure'daki sanal ağlar (VNet'ler) hakkında bilgi edinin."
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="telmos" />

# Virtual Network'e Genel Bakış

Azure sanal ağ (VNet) buluttaki kendi ağınızın bir gösterimidir.  Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtımıdır. Bu ağ içindeki IP adres bloklarını, DNS ayarlarını, güvenlik ilkelerini ve yol tablolarını tam olarak denetleyebilirsiniz. Ayrıca sanal ağınızı alt ağlara ayırabilir ve Azure IaaS sanal makinelerini (VM'ler) ve/veya [Bulut hizmetlerini (PaaS rol örnekleri)](../cloud-services/cloud-services-choose-me.md) başlatabilirsiniz. Bunun yanı sıra, Azure'ın sunduğu [bağlantı seçeneklerinden](../vpn-gateway/vpn-gateway-cross-premises-options.md) birini kullanarak sanal ağı şirket içi ağınıza bağlayabilirsiniz. Özetle, IP adres blokları üzerinde tam bir kontrol sahibi olarak ve Azure'ın sunduğu kurumsal ölçek avantajıyla, ağınızı Azure'a genişletebilirsiniz.

Sanal ağları daha iyi anlayabilmek için basitleştirilmiş bir şirket içi ağını gösteren aşağıdaki şekle göz atın.

![Şirket içi ağı](./media/virtual-networks-overview/figure01.png)

Yukarıdaki şekilde, bir yönlendirici yoluyla genel İnternet'e bağlanan bir şirket içi ağı gösterilmektedir. Bir DNS sunucusunu ve bir web sunucusu grubunu barındıran bir DMZ ile yönlendirici arasında güvenlik duvarı bulunduğunu da görebilirsiniz. Web sunucusu grubunda İnternet'te kullanıma sunulan bir donanım yük dengeleyicisi kullanılarak yük dengelemesi yapılmıştır ve iç alt ağdan kaynak kullanmaktadır. İç alt ağ DMZ'den başka bir güvenlik duvarı ile ayrılmıştır ve Active Directory Etki Alanı Denetleyicileri'ni, veritabanı sunucularını ve uygulama sunucularını barındırır.

Aynı ağ aşağıdaki şekilde gösterilen biçimde Azure'da barındırılabilir.

![Azure sanal ağı](./media/virtual-networks-overview/figure02.png)

Azure altyapısının yönlendirici rolünü üstlendiğine ve herhangi bir yapılandırma gerektirmeden sanal ağınızdan genel İnternet'e erişime olanak sağladığına dikkat edin. Güvenlik duvarları yerine tek tek her alt ağa uygulanan Ağ Güvenlik Grupları (NSG'ler) kullanılabilir. Ayrıca Azure'da yük dengeleyiciler yerine internete yönelik iç yük dengeleyiciler kullanılır.

>[AZURE.NOTE] Azure'da iki dağıtım modeli vardır: klasik (Hizmet Yönetimi olarak da bilinir) ve Azure Resource Manager (ARM). Klasik sanal ağlar bir benzeşim grubuna eklenebilir veya bölgesel bir sanal ağ olarak oluşturulabilir. Benzeşim grubunda bulunan bir sanal ağınız varsa [bunu bölgesel bir sanal ağa geçirmeniz](virtual-networks-migrate-to-regional-vnet.md) önerilir.

## Virtual Network Avantajları

- **Yalıtım**. Sanal ağlar birbirlerinden tamamen yalıtılmıştır. Bu sayede geliştirme, test ve üretim için aynı CIDR adres bloklarını kullanan ayrı ağlar oluşturabilirsiniz.

- **Genel İnternet'e erişim**. Bir sanal ağda yer alan tüm IaaS VM'leri ve PaaS rol örnekleri varsayılan olarak genel İnternet'e erişebilir. Ağ Güvenlik Gruplarını (NSG'ler) kullanarak erişimi denetleyebilirsiniz.

- **Sanal ağ içinden VM'lere erişim**. PaaS rolü örnekleri ve IaaS VM'leri aynı sanal ağda başlatılabilir ve farklı alt ağlarda olsalar bile bir ağ geçidini yapılandırmayı veya genel IP adresleri kullanmayı gerektirmeden, özel IP adresleri kullanılarak birbirlerine bağlanabilirler.

- **Ad çözümlemesi**. Azure, sanal ağınızda dağıtılan IaaS VM'leri ve PaaS rolü örnekleri için iç ad çözümlemesi sağlar. Ayrıca kendi DNS sunucularınızı dağıtabilir ve sanal ağı bunları kullanmak üzere yapılandırabilirsiniz.

- **Güvenlik**. Bir sanal ağdaki sanal makinelere ve PaaS rolü örneklerine gelen ve giden trafik, Ağ Güvenliği grupları kullanılarak denetlenebilir.

- **Bağlantı**. Siteden siteye VPN bağlantısı veya ExpressRoute bağlantısı kullanılarak sanal ağlar birbirlerine ve hatta şirket içi veri merkezinize bağlanabilir. VPN ağ geçitleri hakkında daha fazla bilgi edinmek için bkz. [VPN ağ geçitleri hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md). ExpressRoute hakkında daha fazla bilgi edinmek için bkz. [ExpressRoute'a teknik genel bakış](../expressroute/expressroute-introduction.md).

    >[AZURE.NOTE] Bir IaaS VM'si veya PaaS rolü örneğini Azure ortamınıza dağıtmadan önce bir sanal ağ oluşturduğunuzdan emin olun. ARM tabanlı VM'ler için bir sanal ağ gereklidir ve var olan bir sanal ağı belirtmemeniz durumunda Azure varsayılan bir sanal ağ oluşturur, bu da şirket içi ağınızda CIDR adres bloğu çakışmasına neden olabilir. Bu durumda sanal ağınızın şirket içi ağınıza bağlanması imkansız hale gelir.

## Alt ağlar

Alt ağ, sanal ağ içindeki bir IP adresleri aralığıdır, bir sanal ağı organizasyon ve güvenlik için birden çok alt ağa bölebilirsiniz. Bir sanal ağ içindeki alt ağlara (aynı veya farklı) dağıtılan VM'ler ve PaaS rolü örnekleri, ek bir yapılandırma gerektirmeden birbirleriyle iletişim kurabilir. Ayrıca yol tablolarını ve NSG'leri bir alt ağ için yapılandırabilirsiniz.

## IP adresleri


Azure'daki kaynaklara atanan iki tür IP adresi bulunur: *genel* ve *özel*. Genel IP Adresleri, Azure kaynaklarının İnternet ile ve [Azure Redis Önbelleği](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/) gibi Azure'ın genel kullanıma yönelik diğer hizmetleriyle iletişim kurmasını sağlar. Özel IP Adresleri, İnternet'ten yönlendirilebilir IP adresleri kullanmadan, VPN yoluyla bağlı olanlar da dahil olmak üzere bir sanal ağ içindeki kaynaklar arasında iletişime olanak sağlar.

Azure'daki IP adresleri hakkında daha fazla bilgi edinmek için bkz. [sanal ağdaki IP adresleri](virtual-network-ip-addresses-overview-arm.md)

## Azure yük dengeleyicileri

Bir Sanal ağda yer alan sanal makineler ve bulut hizmetleri, Azure Yük dengeleyicileri kullanılarak İnternet'te kullanıma sunulabilir. İç kullanıma yönelik İş Kolu uygulamalarında yalnızca İç yük dengeleyici kullanılarak yük dengeleme yapılabilir.

- **Dış yük dengeleyici**. Bir dış yük dengeleyici kullanarak genel İnternet'ten erişilen IaaS VM'leri ve PaaS rol örnekleri için yüksek bir kullanılabilirlik sağlayabilirsiniz.

- **İç yük dengeleyici**. Bir iç yük dengeleyici kullanarak sanal ağınızdaki diğer hizmetlerden erişilen IaaS VM'leri ve PaaS rol örnekleri için yüksek bir kullanılabilirlik sağlayabilirsiniz.

Azure'daki yük dengeleme hakkında daha fazla bilgi edinmek için bkz. [Yük dengeleyiciye genel bakış](../load-balancer/load-balancer-overview.md).

## Ağ Güvenlik Grubu (NSG)

Ağ arabirimlerine (NIC'ler), VM'lere ve alt ağlara gelen ve giden erişimi denetlemek için NSG'ler oluşturabilirsiniz. Her NSG'de kaynak IP adresine, kaynak bağlantı noktasına, hedef IP adresine ve hedef bağlantı noktasına göre trafiğe izin verileceğini veya trafiğin reddedileceğini belirten bir veya daha fazla kural bulunur. NSG'ler hakkında daha fazla bilgi edinmek için bkz. [Ağ Güvenlik Grubu nedir?](virtual-networks-nsg.md)

## Sanal gereçler

Sanal gereç, yazılım tabanlı bir gereç işlevini (örneğin; güvenlik duvarı, WAN iyileştirmesi veya yetkisiz erişim algılama) çalıştıran sanal ağınızdaki diğer bir VM'dir. Bir sanal gerecin işlevlerini kullanmak üzere sanal ağ trafiğinizi bu sanal gerece yönlendirmek için Azure'da bir yol oluşturabilirsiniz.

Örneğin, NSG'ler sanal ağınızda güvenlik sağlamak için kullanılabilir. Ancak NSG'ler gelen ve giden paketler için katman 4 Erişim Denetimi Listesi (ACL) sağlar. Katman 7 güvenlik modelini kullanmak istiyorsanız bir güvenlik duvarı gerecini kullanmanız gerekir.

Sanal gereçler [kullanıcı tanımlı yollara ve IP iletimine](virtual-networks-udr-overview.md) bağımlıdır.

## Sınırlar
Bir abonelikte izin verilen Virtual Network'ün sayısına yönelik sınırlar vardır, daha fazla bilgi için lütfen [Azure Ağ sınırlarına](../azure-subscription-service-limits.md#networking-limits) bakın.

## Fiyatlandırma
Azure'da Virtual Network'ü kullanmanın ek bir maliyeti yoktur. Sanal ağ içinde başlatılan işlem örnekleri için [Azure VM Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/)'da açıklanan şekilde standart fiyatlar uygulanır. Sanal ağda kullanılan [Genel IP Adresleri] (https://azure.microsoft.com/pricing/details/ip-addresses/) ve [VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway/) için de standart fiyatlar uygulanır.

## Sonraki adımlar

- Alt ağlar ve [bir sanal ağ oluşturun](virtual-networks-create-vnet-arm-pportal.md).
- [Sanal ağ içinde bir VM oluşturun](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- [NSG'ler](virtual-networks-nsg.md) hakkında bilgi edinin.
- [Kullanıcı tanımlı yollar ve IP iletimi](virtual-networks-udr-overview.md) hakkında bilgi edinin.



<!---HONumber=Jun16_HO2-->


