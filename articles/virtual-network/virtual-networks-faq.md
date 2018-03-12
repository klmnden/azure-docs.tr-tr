---
title: Azure Virtual Network SSS | Microsoft Docs
description: "Microsoft Azure sanal ağlar hakkında sık sorulan soruların yanıtları."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.openlocfilehash: a5b4bac9e0d8bc10defaff251557129a70d8a022
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure sanal ağı sık sorulan sorular (SSS)

## <a name="virtual-network-basics"></a>Sanal ağ temelleri

### <a name="what-is-an-azure-virtual-network-vnet"></a>Bir Azure sanal ağı (VNet) nedir?
Bir Azure sanal ağı (VNet), kendi ağ bulutta gösterimidir. Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtımıdır. Sanal ağlar sağlamak ve sanal özel ağları (VPN'ler) Azure'da yönetin ve kullanabilirsiniz, isteğe bağlı olarak, diğer sanal ağlar Azure veya şirket içi sanal ağlara bağlantı karma veya şirket içi çözümler oluşturmak için BT altyapısı. Oluşturduğunuz her bir Vnet'teki kendi CIDR bloğu var ve CIDR blokları çakışmaması sürece diğer sanal ağlar ve şirket içi ağlara bağlanabilir. Ayrıca denetim sanal ağlar için DNS sunucu ayarları ve sanal ağ kesimleme alt ağlar içine vardır.

Sanal ağlar için kullanın:

* Bir adanmış özel yalnızca bulut VNet çözümünüz için bir şirket içi yapılandırma gerektirmeyen bazen oluşturun. Bir sanal ağ oluşturduğunuzda, hizmetleri ve sanal makineleri sanal ağınızın içinde doğrudan ve güvenli bir şekilde birbirleri ile bulutta iletişim kurabilir. Uç noktası bağlantıları VM'ler, çözümün parçası olarak Internet iletişimi gerektiren ve Hizmetleri için de yapılandırabilirsiniz.
* Güvenli bir şekilde, veri merkezinizdeki sanal ağlar ile genişletmek, güvenli bir şekilde datacenter kapasitenizi ölçeklendirmek için geleneksel siteden siteye (S2S) VPN oluşturabilirsiniz. S2S VPN, şirket VPN ağ geçidi ve Azure arasında güvenli bir bağlantı sağlamak için IPSec kullanın.
* Etkinleştirme karma bulut senaryolarında sanal ağlar karma bulut senaryolarında bir dizi desteklemek için esneklik sağlar. Ana bilgisayarlar gibi şirket içi sistem ve UNIX sistemleri herhangi bir türde bulut tabanlı uygulamaları güvenli bir şekilde bağlanabilir.

### <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?
Ziyaret [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/) başlamak için. Bu içerik, tüm sanal ağ özellikleri için genel bakış ve dağıtım bilgileri sağlar.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Sanal ağlar arası bağlantı kullanabilir miyim?
Evet. Bir sanal ağ, şirket bağlanmadan kullanabilirsiniz. Örneğin, Microsoft Windows Server Active Directory etki alanı denetleyicileri ve SharePoint grupları yalnızca bir Azure VNet içinde çalıştırabilirsiniz.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Sanal ağlar veya bir VNet ile my şirket içi veri merkezi arasında WAN iyileştirmesi gerçekleştirebilir miyim?

Evet. Dağıtabilmeniz için bir [WAN iyileştirmesi ağ sanal gereç](https://azure.microsoft.com/marketplace/?term=wan+optimization) Azure Market üzerinden birkaç satıcılardan.

## <a name="configuration"></a>Yapılandırma

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Bir VNet oluşturmak için hangi Araçlar kullanıyor?
Oluşturma veya bir sanal ağı yapılandırmak için aşağıdaki araçları kullanabilirsiniz:

* Azure portalına
* PowerShell
* Azure CLI
* Ağ yapılandırma dosyası (netcfg - yalnızca klasik sanal ağlar için). Bkz: [bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md) makalesi.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Hangi adres aralıklarını my Vnet'lerde kullanabilir miyim?
Herhangi bir IP adresi aralığını tanımlanan [RFC 1918](http://tools.ietf.org/html/rfc1918). Örneğin, 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>My Vnet'ler genel IP adresleri olabilir mi?
Evet. Ortak IP adresi aralıkları hakkında daha fazla bilgi için bkz: [bir sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network). Genel IP adresleri, internet üzerinden doğrudan erişilemez değildir.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>My Vnet'te alt ağlar sayısına bir sınır var mıdır?
Evet. Bkz: [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) Ayrıntılar için. Alt ağ adres alanları birbirine gelemez.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Bu alt ağ içindeki IP adresleri kullanma kısıtlamaları var mı?
Evet. Azure bazı IP adreslerini her alt ağ içindeki ayırır. Her alt ağ ilk ve son IP adreslerini Azure Hizmetleri için kullanılan her alt ağ x.x.x.1 x.x.x.3 adreslerini birlikte Protokolü uyum için ayrılmıştır.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Küçük ve ne kadar büyük nasıl sanal ağlar ve alt ağları olabilir?
Desteklenen en küçük alt /29 yer ve en büyük 8 uzunluğudur (CIDR alt ağ tanımlarının kullanarak).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Sanal ağlar kullanarak ı Azure'a my VLAN'lar getirebilir miyim?
Hayır. Sanal ağlar Katman 3 yer paylaşımları ' dir. Azure, Katman-2 semantiği desteklemez.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Özel yönlendirme ilkeleri my sanal ağlar ve alt ağları belirtebilir miyim?
Evet. Bir yol tablosu oluşturmanız ve bir alt ağa ilişkilendirebilirsiniz. Azure'da yönlendirme hakkında daha fazla bilgi için bkz: [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Sanal ağlar, çok noktaya yayın veya çok noktaya yayın destekliyor musunuz?
Hayır. Çok noktaya yayın ve yayın desteklenmez.

### <a name="what-protocols-can-i-use-within-vnets"></a>Hangi protokolleri içinde sanal ağlar kullanabilir miyim?
Sanal ağlar içinde TCP, UDP ve TCP/IP'yi ICMP protokoller kullanabilir. Tek noktaya yayın içinde sanal ağlar, hariç olmak üzere Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) tek noktaya yayın üzerinden desteklenir (bağlantı noktası UDP/68 kaynak / hedef bağlantı noktası UDP/67). Çok noktaya yayın, yayın, IP-kapsüllenmiş IP paketlerinin ve Genel Yönlendirme Kapsüllemesi (GRE) paketleri Vnet'ler engellenir. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Bir sanal ağ içindeki my varsayılan yönlendiricileri ping?
Hayır.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Bağlantı tanılamak için tracert kullanabilir miyim?
Hayır.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Alt ağlar, sanal ağ oluşturulduktan sonra ekleyebilir miyim?
Evet. Alt ağın herhangi bir zamanda alt ağ adresi aralığı başka bir alt ağının parçası değil ve sanal ağın adres aralığındaki sol kullanılabilir alanı sürece eklenebilir.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Alt boyutu, onu oluşturduktan sonra değişiklik yapabilirsiniz?
Evet. Ekle, Kaldır, genişletin veya sanal makineleri veya Hizmetleri içinde dağıtılan yoksa bir alt ağ küçültür.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Alt ağlar, bunları oluşturduktan sonra değişiklik yapabilirsiniz?
Evet. Ekleme, kaldırma ve sanal ağ tarafından kullanılan CIDR blokları değiştirme.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>İnternet'e, sanal ağ içinde Hizmetlerim çalıştırdığım, bağlanabilir miyim?
Evet. Bir sanal ağ içinde dağıtılan tüm hizmetlerin giden internet bağlanabilir. Azure'a giden internet bağlantıları hakkında daha fazla bilgi için bkz: [giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Resource Manager aracılığıyla dağıtılan bir kaynağa gelen bağlanmak istiyorsanız, kaynak kendisine atanmış bir ortak IP adresi olmalıdır. Genel IP adresleri hakkında daha fazla bilgi için bkz: [ortak IP adresleri](virtual-network-public-ip-address.md). Azure üzerinde dağıtılan her Azure bulut hizmeti kendisine atanmış bir genel olarak adreslenebilir VIP sahiptir. PaaS rol için giriş uç noktaları ve Internet'ten gelen bağlantıları kabul etmek bu hizmetleri sağlamak sanal makineler için uç noktaları tanımlayın.

### <a name="do-vnets-support-ipv6"></a>Sanal ağlar IPv6 destekliyor musunuz?
Hayır. Şu anda sanal ağlar ile IPv6 kullanamazsınız. Ancak, sanal makineleri yüklemek için Azure yük dengeleyicileri Ata IPv6 adreslerine bakiye. Ayrıntılar için bkz [Azure yük dengeleyici için IPv6 genel bakış](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Bir sanal ağ bölgeleri yayılabilir mi?
Hayır. Tek bir bölge için bir VNet sınırlıdır. Ancak, bir sanal ağ kullanılabilirlik bölgeleri span. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sanal Ağ eşlemesi ile sanal ağlar farklı bölgelerde bağlanabilir. Ayrıntılar için bkz [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Bir VNet Azure başka bir sanal ağa bağlanabiliyor musunuz?
Evet. Bir VNet kullanarak başka bir Vnet'e bağlanabilir:
- **Sanal Ağ eşlemesi**: ayrıntılı bilgi için bkz: [VNet eşleme genel bakış](virtual-network-peering-overview.md)
- **Bir Azure VPN ağ geçidi**: ayrıntılı bilgi için bkz: [VNet-VNet bağlantı yapılandırma](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Ad çözümlemesi (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Sanal ağlar için DNS seçeneklerim nelerdir?
Karar tablosu kullanılacağı [VM'ler ve rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md) tüm DNS yol göstermesi için sayfa seçenekleri kullanılabilir.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>DNS sunucuları için bir VNet belirtebilir miyim?
Evet. DNS sunucusu IP adresleri sanal ağ ayarları belirtebilirsiniz. Ayar, sanal ağ içindeki tüm VM'ler için varsayılan DNS sunucuları uygulanır.

### <a name="how-many-dns-servers-can-i-specify"></a>Kaç tane DNS sunucuları ı belirtebilir miyim?
Başvuru [Azure sınırlar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>My DNS sunucuları, ağ oluşturmuş olduğunuz sonra değişiklik yapabilirsiniz?
Evet. DNS sunucusu listesinde ağınız için dilediğiniz zaman değiştirebilirsiniz. DNS sunucusu listesinde değiştirirseniz, yeni DNS sunucusunu seçmek için Vnet'inizi için bunları sırayla VM'lerin her birinde yeniden başlatmanız gerekir.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Azure tarafından sağlanan DNS nedir ve sanal ağlar ile çalışır mı?
Azure tarafından sağlanan DNS, Microsoft tarafından sunulan çok kiracılı bir DNS hizmetidir. Azure VM'ler ve bulut hizmet rolü örneklerinin tümünde bu hizmetinde kaydeder. Bu hizmet VM'ler ve rol örnekleri aynı sanal ağda için sanal makineleri ve aynı bulut hizmetinde bulunan rol örnekleri için ana bilgisayar adı ve FQDN tarafından ad çözümlemesi sağlar. DNS hakkında daha fazla bilgi için bkz: [Vm'leri ve bulut Hizmetleri rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md).

VNet ilk 100 bulut hizmetlerine Azure tarafından sağlanan DNS kullanarak çapraz Kiracı adı çözümlemesi için bir sınırlama yoktur. Kendi DNS sunucusu kullanıyorsanız, bu kısıtlama geçerli değildir.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>DNS ayarlarımı VM başına veya Bulut hizmeti olarak geçersiz kılabilirsiniz?
Evet. Varsayılan ağ ayarlarını geçersiz kılmak için VM veya Bulut hizmeti başına DNS sunucuları kurabilirsiniz. Ancak, ağ çapında DNS mümkün olduğunca kullanmanız önerilir.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kendi DNS soneki getirebilir miyim?
Hayır. Özel bir DNS soneki, sanal ağlar için belirtilemez.

## <a name="connecting-virtual-machines"></a>Sanal makineler bağlanma

### <a name="can-i-deploy-vms-to-a-vnet"></a>Bir sanal ağa VM'ler dağıtabilir miyim?
Evet. Resource Manager dağıtım modeli aracılığıyla dağıtılan bir VM'ye bağlı tüm ağ arabirimleri (NIC) bir sanal ağa bağlanması gerekir. Klasik dağıtım modeli aracılığıyla dağıtılan VM'ler isteğe bağlı olarak bir sanal ağa bağlanabilir.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>VM atamak IP adresleri farklı türleri nelerdir?
* **Özel:** her VM dahilinde her NIC atanmış. Adresi ya da statik veya dinamik yöntemi kullanılarak atanır. Özel IP adresleri, sanal ağınızın alt ayarlarında belirtilen aralıkta atanır. Bir sanal ağa bağlı değilseniz olsa bile kaynaklara Klasik dağıtım modeli aracılığıyla dağıtılan özel IP adresleri atanır. Ayırma yöntemi davranışını bir kaynak Resource Manager veya Klasik dağıtım modeli ile dağıtılmış olan bağlı olarak farklılık gösterir: 

  - **Resource Manager**: kaynak silinene kadar dinamik veya statik yöntemiyle atanan özel bir IP adresi bir sanal makineye (Resource Manager) atanmış olarak kalır. Statik kullanırken atamak için adresi seçin ve Azure dinamik kullanırken seçer farktır. 
  - **Klasik**: dinamik yöntemiyle atanan özel bir IP adresi, bir sanal makine olduğunda (Klasik) değişebilir VM durduruldu (serbest bırakıldığında) durumda bırakıldı sonra yeniden. Özel IP adresi Klasik dağıtım modeli aracılığıyla dağıtılan bir kaynak için hiç değiştirdiğinden emin olmak gerekiyorsa, özel bir IP adresi statik yöntemiyle atayın.

* **Genel:** Azure Resource Manager dağıtım modeli aracılığıyla dağıtılan VM'ler bağlı NIC'ler için isteğe bağlı olarak atanmış. Adres statik veya dinamik ayırma yöntemiyle atanabilir. Klasik dağıtım modeli aracılığıyla dağıtılan tüm sanal makineleri ve bulut Hizmetleri rol örnekleri atanmış bir bulut hizmeti içinde mevcut bir *dinamik*, genel sanal IP (VIP) adresi. Ortak bir *statik* IP adresi olarak adlandırılan bir [ayrılmış IP adresi](virtual-networks-reserved-public-ip.md), isteğe bağlı bir VIP atanabilir. Tek tek sanal makineleri veya Bulut Hizmetleri rolü örneklerine ilgili Klasik dağıtım modeli aracılığıyla dağıtılan ortak IP adresleri atayın. Bu adresler adlı [örnek düzeyinde ortak IP (ILPIP](virtual-networks-instance-level-public-ip.md) adresleri ve dinamik olarak atanabilir.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Özel bir IP adresi daha sonraki bir zamanda oluşturmak bir VM için ayırabilirsiniz?
Hayır. Özel bir IP adresi rezerve edemezsiniz. Özel IP adresi varsa, bir VM veya rol örneğine DHCP sunucusu tarafından atanır. VM olabilir veya özel bir IP adresi atanmış istediğiniz bir olmayabilir. Ancak, özel IP adresi zaten oluşturulmuş bir VM'nin kullanılabilir tüm özel IP adresi değiştirebilirsiniz.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Bir sanal ağ içindeki VM'ler için özel IP adresleri değişiklik yapmak?
Duruma göre değişir. VM Resource Manager aracılığıyla dağıttıysanız Hayır, bağımsız olarak IP adresini statik veya dinamik ayırma yöntemiyle olup atandı. VM Klasik dağıtım modeli aracılığıyla dağıtılmışsa, bir VM durduruldu (serbest bırakıldığında) durumda bırakıldı sonra başlatıldığında dinamik IP adreslerini değiştirebilirsiniz. Adres VM silindiğinde, her iki dağıtım modeli aracılığıyla dağıtılan bir sanal makineden yayımlanır.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>I el ile IP adresleri için NIC VM işletim sistemi içinde atayabilirsiniz?
Evet, ancak sürece önerilmez zaman birden çok IP bir sanal makineye adresleri atama gibi gerekli. Ayrıntılar için bkz [ekleme birden çok IP adresleri bir sanal makineye](virtual-network-multiple-ip-addresses-portal.md#os-config). Bir Azure NIC'ye atanan IP adresi bağlıysa bir VM değişiklikleri ve IP adresi VM işletim sistemi içinde farklı olduğundan, VM bağlantısını kaybedebilir.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Bir bulut hizmeti dağıtım yuvası veya kapatma işletim sistemi içinde bir VM'den durdurursanız my IP adreslerine ne olur?
Bir şey yok. IP adresleri (ortak VIP, ortak ve özel) bulut hizmeti dağıtım yuvası ya da VM atanmış kalır.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>I VM'ler bir alt ağdan başka bir alt ağa bir VNet içinde yeniden dağıtmadan taşıyabilir miyim?
Evet. Daha fazla bilgi bulabilirsiniz [farklı bir alt ağa bir VM veya rol örneğine taşıma](virtual-networks-move-vm-role-to-subnet.md) makalesi.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>VM'im için statik bir MAC adresi yapılandırabilir miyim?
Hayır. Bir MAC adresi statik olarak yapılandırılamaz.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>MAC adresi oluşturulduktan sonra aynı VM'im için kalacak?
Evet, MAC adresi silinene kadar Resource Manager ve klasik dağıtım modeli dağıtılan bir VM için aynı kalır. Daha önce MAC adresi VM (serbest bırakıldığında) durduruldu ancak VM deallocated durumda olduğunda bile MAC adresi şimdi korunur yayımlanmıştır.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>İnternet'e bir VNet içindeki bir VM'den bağlanabiliyor musunuz?
Evet. Bir sanal ağ içinde dağıtılan tüm sanal makineleri ve bulut Hizmetleri rol örnekleri Internet'e bağlanabilir.

## <a name="azure-services-that-connect-to-vnets"></a>Sanal ağlara bağlanma azure Hizmetleri

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Azure App Service Web Apps bir VNet ile birlikte kullanabilir miyim?
Evet. Bir ana (uygulama hizmeti ortamı) kullanarak VNet içindeki Web uygulamaları dağıtabilirsiniz. Ağınız için yapılandırılmış bir noktadan siteye bağlantı varsa, tüm Web uygulamaları güvenli bir şekilde bağlanmak ve VNet kaynaklara erişim. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Bir uygulama hizmeti ortamı'nda Web uygulamaları oluşturma](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uygulamanızı Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [VNet tümleştirme ve karma bağlantılar Web uygulamaları ile kullanma](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Bulut Hizmetleri web ve çalışan rolleri (PaaS) bir VNet ile dağıtabilirsiniz?
Evet. Sanal ağlar içindeki bulut Hizmetleri rol örnekleri (isteğe bağlı) dağıtabilirsiniz. Bunu yapmak için sanal ağ adı ve rol/alt eşlemeleri hizmetinizi ağ yapılandırma bölümünde belirtin. Herhangi biri, ikili dosyaları güncelleştirmek gerekmez.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Bir sanal makine ölçek kümesi (VMSS) bir Vnet'e bağlayabilir miyim?
Evet. Bir sanal ağa bir VMSS bağlanmanız gerekir.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Bir VNet kaynaklardan dağıtabileceğiniz Hizmetleri Azure tam bir listesi vardır?

Evet, Ayrıntılar için bkz [Azure Hizmetleri için sanal ağ tümleştirmesinin](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Hangi Azure PaaS kaynaklarına erişim için bir VNet kısıtlayabilir miyiz?

Bazı Azure PaaS Hizmetleri (örneğin, Azure Storage ve Azure SQL veritabanı) üzerinden dağıtılan kaynak ağ erişimi kısıtlayabilir yalnızca sanal ağ hizmeti uç noktalarını kullanarak bir sanal ağ kaynaklarına. Ayrıntılar için bkz [sanal ağ hizmet uç noktaları genel bakış](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Sanal ağlar ve Hizmetlerim taşıyabilir miyim?
Hayır. Sanal ağlar hizmetlerde taşınamıyor. Bir kaynağı başka bir Vnet'e taşımak için silin ve kaynak dağıtmanız gerekir.

## <a name="security"></a>Güvenlik

### <a name="what-is-the-security-model-for-vnets"></a>Sanal ağlar için güvenlik modeli nedir?
Sanal ağlar birbirinden ve Azure altyapısı için barındırılan diğer hizmetlerin yalıtılır. Bir VNet bir güven sınırı ' dir.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>VNet bağlı kaynaklara gelen veya giden trafik akışı kısıtlayabilir miyiz?
Evet. Uygulayabileceğiniz [ağ güvenlik grupları](security-overview.md) bir VNet veya her ikisi için bir sanal ağ içindeki tek tek alt ağlara NIC'ler bağlı.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>VNet bağlı kaynaklar arasında bir güvenlik duvarı uygulamak?
Evet. Dağıtabilmeniz için bir [güvenlik duvarı ağ sanal gereç](https://azure.microsoft.com/marketplace/?term=firewall) Azure Market üzerinden birkaç satıcılardan.

### <a name="is-there-information-available-about-securing-vnets"></a>Var. bilgi sanal ağlar güvenliğini sağlama hakkında var mı?
Evet. Ayrıntılar için bkz [Azure ağ güvenliğine genel bakış](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>API'leri, şemalar ve araçları

### <a name="can-i-manage-vnets-from-code"></a>Sanal ağlar koddan yönetebilir miyim?
Evet. İçinde sanal ağlar için REST API'ler kullanabilirsiniz [Azure Resource Manager](/rest/api/virtual-network) ve [Klasik (Hizmet Yönetimi)](http://go.microsoft.com/fwlink/?LinkId=296833) dağıtım modeli.

### <a name="is-there-tooling-support-for-vnets"></a>Sanal ağlar için araç desteği vardır?
Evet. Kullanma hakkında daha fazla bilgi edinin:
- Sanal ağlar üzerinden dağıtmak için Azure portal [Azure Resource Manager](manage-virtual-network.md#create-a-virtual-network) ve [Klasik](virtual-networks-create-vnet-classic-pportal.md) dağıtım modeli.
- Sanal ağlar üzerinden yönetmek için PowerShell [Resource Manager](/powershell/module/azurerm.network) ve [Klasik](/powershell/module/azure/?view=azuresmps-3.7.0) dağıtım modeli.
- Azure komut satırı arabirimi (CLI) dağıtmak ve sanal ağlar üzerinden yönetmek için [Resource Manager](/cli/azure/network/vnet) ve [Klasik](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) dağıtım modeli.  
