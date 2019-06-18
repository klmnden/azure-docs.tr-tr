---
title: Azure sanal ağ hakkında SSS
titlesuffix: Azure Virtual Network
description: Microsoft Azure sanal ağları hakkında sık sorulan sorulara yanıtlar.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2019
ms.author: kumud
ms.openlocfilehash: f4facdf8fc530c35ba02620f451a00a8da36d982
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66497104"
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure sanal ağına sık sorulan sorular (SSS)

## <a name="virtual-network-basics"></a>Sanal ağ temel bilgileri

### <a name="what-is-an-azure-virtual-network-vnet"></a>Bir Azure sanal ağı (VNet) nedir?
Bir Azure sanal ağı (VNet) buluttaki kendi ağınızın bir gösterimidir. Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtımıdır. Sanal ağlar sağlamak ve Azure'da sanal özel ağlar (VPN'ler) yönetmek ve kullanabilirsiniz, isteğe bağlı olarak, azure'daki diğer Vnet'lere veya şirket içi ile sanal ağları bağlamak, BT altyapısı, karma veya şirket içi çözümler oluşturun. Oluşturduğunuz her VNet kendi CIDR bloğu vardır ve CIDR blokları çakışmadığından sürece diğer sanal ağlar ve şirket içi ağa bağlanabilir. Ayrıca, sanal ağlar için DNS sunucusu ayarlarını denetimi ve alt ağlar ile sanal ağın segmentlere ayırma vardır.

Sanal ağlar için kullanın:

* Bir adanmış özel yalnızca bulut sanal ağ, çözümünüz için bir şirket içi yapılandırma gerektirmeyen bazen oluşturun. Bir sanal ağ oluşturduğunuzda, hizmetleri ve sanal makineler, sanal ağ içinde doğrudan ve güvenli bir şekilde birbiriyle bulutta iletişim kurabilir. Uç nokta bağlantıları, çözümünüzün bir parçası olarak Internet iletişimi gerektiren hizmetler ve VM'ler için yine de yapılandırabilirsiniz.
* Güvenli bir şekilde veri merkeziniz ile sanal ağlar'ı genişletin, veri merkezi kapasitenizi güvenli bir şekilde ölçeklendirmek için geleneksel siteden siteye (S2S) VPN oluşturabilirsiniz. S2S VPN, Kurumsal VPN ağ geçidi ve Azure arasında güvenli bir bağlantı sağlamak için IPSEC'i kullanın.
* Hibrit bulut senaryolara olanak sanal ağlar bir dizi bir hibrit bulut senaryoları desteklemek için esnekliğini sunar. Ana bilgisayarlar gibi şirket içi sistem ve Unix sistemlerinden herhangi bir türde bulut tabanlı uygulamaları güvenli bir şekilde bağlanabilirsiniz.

### <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?
Ziyaret [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/) kullanmaya başlamak için. Bu içerik, tüm sanal ağ özellikleri için genel bakış ve dağıtım bilgileri sağlar.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Sanal ağ içi ve dışı karışık bağlantı kullanabilir miyim?
Evet. Bir sanal ağ, şirket içinde bağlanmadan kullanabilirsiniz. Örneğin, Microsoft Windows Server Active Directory etki alanı denetleyicileri ve SharePoint grupları yalnızca bir Azure sanal ağınızda çalıştırabilirsiniz.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Sanal ağlar veya sanal ağ ile my şirket içi veri merkezi arasında WAN iyileştirmesi yapabilir miyim?
Evet. Dağıtabileceğiniz bir [WAN iyileştirmesi ağ sanal Gereci](https://azure.microsoft.com/marketplace/?term=wan+optimization) Azure Marketi aracılığıyla birkaç satıcılardan.

## <a name="configuration"></a>Yapılandırma

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>VNet oluşturmak için hangi araçları kullanabilir?
Oluşturun veya bir sanal ağı yapılandırmak için aşağıdaki araçları kullanabilirsiniz:

* Azure portal
* PowerShell
* Azure CLI
* Ağ yapılandırma dosyası (yalnızca klasik sanal ağlar için - netcfg). Bkz: [ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md) makalesi.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Sanal Ağlarımın içinde hangi adres aralıkları kullanabilirim?
Herhangi bir IP adresi aralığını tanımlanan [RFC 1918](https://tools.ietf.org/html/rfc1918). Örneğin: 10.0.0.0/16. Aşağıdaki adresi aralıklarını eklenemiyor:
* 224.0.0.0/4 (çok noktaya yayın)
* 255.255.255.255/32 (yayın)
* 127.0.0.0/8 (geri döngü)
* 169.254.0.0/16 (bağlantı-yerel)
* 168.63.129.16/32 (iç DNS)

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Sanal Ağlarımın içinde genel IP adresleri olabilir mi?
Evet. Genel IP adresi aralıkları hakkında daha fazla bilgi için bkz: [sanal ağ oluşturma](manage-virtual-network.md#create-a-virtual-network). Genel IP adresleri doğrudan internet'ten erişilemez.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>My vnet'te alt ağlar sayısına bir sınır var mıdır?
Evet. Bkz: [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) Ayrıntılar için. Alt ağ adres alanlarının birbiriyle çakışamaz.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?
Evet. Azure, her alt ağ içindeki 5 IP adresini ayırır. X.x.x.0 x.x.x.3 ve alt ağın son adresi şunlardır.    
- x.x.x.0 ve alt ağın son adresi protokol uyumluluğu için ayrılmıştır.
- x.x.x.1 x.x.x.3 Azure Hizmetleri için her alt ağda ayrılmıştır.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Küçük ve ne kadar büyük nasıl sanal ağlar ve alt ağları olabilir?
Desteklenen en düşük alt /29 olduğunu ve en büyük 8 uzunluğudur (CIDR alt ağ tanımlarının kullanarak).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Sanal ağlar'ı kullanarak miyim Azure'a my VLAN'ları getirebilir miyim?
Hayır. Sanal ağlar, Katman 3 katmanları olan. Azure, herhangi bir katman 2 semantiği desteklemez.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Özel yönlendirme ilkeleri my sanal ağlar ve alt ağları belirtebilir miyim?
Evet. Bir yol tablosu oluşturun ve bunu bir alt ağ ile ilişkilendirebilirsiniz. Azure'da yollar hakkında daha fazla bilgi için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Sanal ağlar, çok noktaya yayın veya çok noktaya yayın destekliyor musunuz?
Hayır. Çok noktaya yayın ve yayın desteklenmez.

### <a name="what-protocols-can-i-use-within-vnets"></a>Sanal ağlar içinde hangi protokollerin kullanabilirim?
TCP, UDP ve ICMP TCP/IP protokolleri tanımlanmasında kullanabilirsiniz. Tek noktaya yayın tanımlanmasında hariç olmak üzere dinamik konak Yapılandırma Protokolü (DHCP) tek noktaya yayın üzerinden desteklenir (bağlantı noktası UDP/68 kaynak / hedef bağlantı noktası UDP/67). Sanal ağ içinde çok noktaya yayın, yayın, IP-kapsüllenmiş IP paketleri ve Genel Yönlendirme Kapsüllemesi (GRE) paketleri engellenir. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Sanal ağ içindeki my varsayılan yönlendiriciler ping komutu gönderebilir?
Hayır.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Bağlantı tanılamak için tracert kullanabilir miyim?
Hayır.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Alt ağlar, sanal ağ oluşturulduktan sonra ekleyebilir miyim?
Evet. Alt ağları sanal ağlara herhangi bir zamanda alt ağ adres aralığı, başka bir alt ağın parçası değil ve sanal ağın adres aralığında kalan kullanılabilir alanı sürece eklenebilir.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Alt boyutu, oluşturduktan sonra değiştirebilir?
Evet. Ekle, Kaldır, genişletin veya sanal makineleri veya Hizmetleri içinde dağıtılan yoksa bir alt ağ daraltma.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Alt ağlar, bunları oluşturduktan sonra değiştirebilir?
Evet. Eklemek, kaldırmak ve sanal ağ tarafından kullanılan CIDR blokları değiştirin.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>İnternet'e, sanal ağ içinde Hizmetlerim çalıştırıyorum, bağlanabilir miyim?
Evet. Bir sanal ağ içinde dağıtılan tüm hizmetler İnternet'e giden bağlantı kurabilir. Azure'da giden internet bağlantıları hakkında daha fazla bilgi için bkz: [giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Gelen Resource Manager üzerinden dağıtılan bir kaynağa bağlanmak istiyorsanız, kaynağın genel IP adresi atanmış olmalıdır. Genel IP adresleri hakkında daha fazla bilgi için bkz: [genel IP adresleri](virtual-network-public-ip-address.md). Azure'da dağıtılan her Azure bulut hizmetine atanmış genel olarak adreslenebilir bir VIP sahiptir. PaaS rolleri için giriş uç noktaları ve İnternet'ten gelen bağlantıları kabul etmek bu hizmetlerin etkinleştirmek sanal makineler için uç noktalar siz tanımlarsınız.

### <a name="do-vnets-support-ipv6"></a>Sanal ağ, IPv6 destekliyor musunuz?
Hayır. IPv6 ile sanal ağlar şu anda kullanamazsınız. Ancak olabilir, sanal makineler Ata IPv6 adresleri yüklemek için Azure yük dengeleyicileri için Bakiye. Ayrıntılar için bkz [Azure Load Balancer için IPv6 genel bakış](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Bir VNet bölgeleri yayılabilir mi?
Hayır. Bir VNet için tek bir bölgede sınırlıdır. Bir sanal ağ, ancak kullanılabilirlik yayılma. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Sanal Ağ eşlemesi ile farklı bölgelerdeki sanal ağlara bağlayabilirsiniz. Ayrıntılar için bkz [sanal ağ eşleme genel bakış](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Bir VNet azure'da başka bir sanal ağa bağlanabilir?
Evet. Bir sanal ağ kullanarak başka bir sanal ağa bağlayabilirsiniz:
- **Sanal Ağ eşlemesi**: Ayrıntılar için bkz [VNet eşlemesi genel bakış](virtual-network-peering-overview.md)
- **Azure VPN Gateway'i**: Ayrıntılar için bkz [bir VNet-VNet bağlantısını yapılandırma](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Ad çözümleme (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Sanal ağlar için DNS seçeneklerim nelerdir?
Karar tablosu kullanmak [VM'ler ve rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md) kullanılabilir seçenekler sayfası, DNS üzerinden size yol gösterecek.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Bir sanal ağın DNS sunucuları belirtebilir miyim?
Evet. DNS sunucusu IP adresleri sanal ağ ayarlarını belirtebilirsiniz. Ayar, sanal ağ içindeki tüm sanal makineler için varsayılan DNS sunucuları uygulanır.

### <a name="how-many-dns-servers-can-i-specify"></a>Kaç tane DNS sunucuları miyim belirtebilir miyim?
Başvuru [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>My DNS sunucuları, ağ oluşturduğumuzda değişiklik yapabilirsiniz?
Evet. Sanal ağınıza ait DNS sunucusu listesine dilediğiniz zaman değiştirebilirsiniz. DNS sunucu listenizde değiştirirseniz, yeni DNS sunucusunu seçmek için Vnet'inizi için bunları sırayla VM'lerin her birinde yeniden başlatmanız gerekir.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Azure tarafından sağlanan DNS nedir ve sanal ağlar ile çalışır mı?
Azure tarafından sağlanan DNS, Microsoft tarafından sunulan çok kiracılı bir DNS hizmetidir. Azure, tüm VM'ler ve bulut Hizmeti rol örnekleri bu hizmeti kaydeder. Bu hizmet için VM'ler ve rol örnekleri aynı VNet içindeki konak adı için VM'ler ve rol örnekleri aynı bulut hizmetinde bulunan ve FQDN ad çözümlemesi sağlar. DNS hakkında daha fazla bilgi için bkz: [Vm'leri ve Cloud Services rol örnekleri için ad çözümlemesi](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Bir sanal ağ içindeki ilk 100 bulut Hizmetleri için Azure tarafından sağlanan DNS kullanılarak kiracılar arası ad çözümlemesi için bir sınırlama yoktur. Kendi DNS sunucunuzu kullanıyorsanız, bu sınırlama geçerli değildir.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>DNS ayarlarımı başına sanal makine veya Bulut hizmeti olarak geçersiz kılma?
Evet. Varsayılan ağ ayarları geçersiz kılmak için sanal makine veya Bulut hizmeti başına DNS sunucusu ayarlayabilirsiniz. Ancak, ağ çapında DNS mümkün olduğunca kullanmanız önerilir.

### <a name="can-i-bring-my-own-dns-suffix"></a>Ben kendi DNS soneki getirebilir miyim?
Hayır. Sanal ağlarınız için özel bir DNS soneki belirtemezsiniz.

## <a name="connecting-virtual-machines"></a>Sanal makineler bağlanma

### <a name="can-i-deploy-vms-to-a-vnet"></a>Vm'leri bir Vnet'e dağıtabilir miyim?
Evet. Resource Manager dağıtım modeliyle dağıtılan bir sanal makineye bağlı tüm ağ arabirimleri (NIC) bir sanal ağa bağlı olması gerekir. Klasik dağıtım modeliyle dağıtılan Vm'leri isteğe bağlı olarak bir sanal ağa bağlanabilir.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Vm'lere atamak IP adresleri farklı türleri nelerdir?
* **Özel:** Her sanal makine içindeki her bir ağ arabirimi atanır. Adresi ya da statik veya dinamik yöntem kullanılarak atanır. Özel IP adresleri, sanal ağınızın alt ayarlarında belirtilen bir aralıktan atanır. Bir sanal ağa bağlı olmasanız bile Klasik dağıtım modeliyle dağıtılan kaynakların özel IP adresleri atanır. Ayırma yöntemini davranışını bir kaynak Resource Manager veya Klasik dağıtım modeli ile dağıtılan bağlı olarak farklılık gösterir: 

  - **Kaynak Yöneticisi'ni**: Kaynak silinene kadar dinamik veya statik yöntemi ile atanan özel bir IP adresi (Resource Manager) sanal makineye atanmış olarak kalır. , Statik kullanırken atanacak adresini seçin ve dinamik kullanırken, Azure'ı seçti farktır. 
  - **Klasik**: Bir sanal makine zaman (Klasik) dinamik yöntemiyle atanmıştır özel bir IP adresi değişebilir VM durdurulmuş (serbest bırakıldı) durumda olan sonra yeniden. Özel IP adresini Klasik dağıtım modeliyle dağıtılan bir kaynak için hiç değiştirdiğinden emin olmak ihtiyacınız varsa statik bir yöntemi ile özel bir IP adresi atayın.

* **Genel:** İsteğe bağlı olarak Azure Resource Manager dağıtım modeliyle dağıtılan vm'lere bağlı ağ arabirimlerine atanmış. Adresi statik veya dinamik ayırma yöntemiyle atanabilir. Klasik dağıtım modeliyle dağıtılan tüm Vm'leri ve Cloud Services rol örnekleri atanmış bir bulut hizmetinde mevcut bir *dinamik*, genel sanal IP (VIP) adresi. Bir ortak *statik* adlı IP adresi, bir [ayrılmış IP adresi](virtual-networks-reserved-public-ip.md), isteğe bağlı olarak bir VIP atanabilir. Tek tek sanal makineleri veya Cloud Services rol örnekleri ilgili Klasik dağıtım modeliyle dağıtılan genel IP adresleri atayabilirsiniz. Bu adresler adlı [örnek düzeyi genel IP (ILPIP](virtual-networks-instance-level-public-ip.md) giderir ve dinamik olarak atanabilir.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Özel bir IP adresi daha sonraki bir zamanda oluşturma bir VM için ayırabilirsiniz?
Hayır. Özel bir IP adresi ayıramazsınız. Özel bir IP adresi varsa, DHCP sunucusu tarafından bir sanal makine veya rol örneğine atanır. VM olabilir veya istediğiniz özel IP adresi atanmış bir olabilir. Ancak, önceden oluşturulmuş bir VM'nin özel IP adresini, kullanılabilir tüm özel IP adresi değiştirebilirsiniz.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Bir vnet'teki VM'ler için özel IP adresleri değişikliği musunuz?
Duruma göre değişir. VM, Resource Manager aracılığıyla dağıtılmışsa Hayır, bağımsız olarak IP adresi statik veya dinamik atama yöntemiyle olup atanmış. VM Klasik dağıtım modeliyle dağıtılmışsa, VM durdurulmuş (serbest bırakıldı) durumda olan sonra başlatıldığında dinamik IP adresleri değiştirebilirsiniz. VM silindiğinde, her iki dağıtım modeliyle dağıtılan bir sanal makineden adresi serbest kalır.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Ben el ile IP adresleri Nıc'lere VM işletim sistemi içinde atayabilirim miyim?
Evet, ancak sürece önerilmez ne zaman bir sanal makineye birden fazla IP adresleri gibi gerekli. Ayrıntılar için bkz [adresleri bir sanal makineye birden çok IP ekleme](virtual-network-multiple-ip-addresses-portal.md#os-config). Bir Azure NIC için atanan IP adresi eklediyseniz bir VM'ye değişiklikleri ve VM'nin işletim sistemi içinde IP adresi farklı olduğundan, VM'ye bağlantısını kaybedebilir.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Benim için IP adresleri, bir bulut hizmeti dağıtım yuvası veya kapatma işletim sistemi içinde bir VM'den durdurursanız ne olur?
Bir şey yok. IP adresleri (ortak VIP, genel ve özel) bulut hizmeti dağıtım yuvası veya VM'ye atanmış kalır.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Vm'leri bir alt ağdan başka bir alt ağa bir sanal ağ içindeki yeniden dağıtmaya gerek kalmadan taşıyabilirim?
Evet. Daha fazla bilgi bulabilirsiniz [farklı bir alt ağa bir VM veya rol örneğindeki taşıma](virtual-networks-move-vm-role-to-subnet.md) makalesi.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Sanal Makinem için statik bir MAC adresi yapılandırabilir miyim?
Hayır. Bir MAC adresi statik olarak yapılandırılamaz.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>Oluşturulduktan sonra MAC adresini sanal Makinem için aynı kalır?
Evet, MAC adresini silinene kadar Resource Manager ve klasik dağıtım modelleri aracılığıyla dağıtılan bir sanal makine için aynı kalır. Daha önce MAC adresini VM durduruldu (serbest bırakıldı), ancak VM serbest bırakılmış durumda olduğunda bile artık MAC adresini korunur yayınlanmıştır. Ağ arabirimi silinmiş veya birincil ağ arabiriminin birincil IP yapılandırması için atanmış özel IP adresi değiştirildiğinde kadar MAC adresinin ağ arabirimine atanmış olarak kalır. 

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>İnternet'e bir sanal ağ içindeki bir VM'den bağlanabiliyor musunuz?
Evet. Bir sanal ağ içinde dağıtılan tüm Vm'leri ve Cloud Services rol örnekleri, Internet'e bağlanabilir.

## <a name="azure-services-that-connect-to-vnets"></a>Sanal ağlara bağlanan azure Hizmetleri

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Azure App Service Web Apps bir VNet ile birlikte kullanabilir miyim?
Evet. Bir ASE (App Service ortamı) kullanarak bir sanal ağ içindeki Web uygulamaları dağıtma, uygulamalarınızda arka uç sanal ağlarınız ile sanal ağ tümleştirmesi ve gelen trafik kilitleme için hizmet uç noktaları ile uygulamanızı bağlayın. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [App Service ağ özellikleri](../app-service/networking-features.md)
* [Bir App Service ortamında Web uygulamaları oluşturma](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [App Service erişim kısıtlamaları](../app-service/app-service-ip-restrictions.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Cloud Services web ve çalışan rolleri (PaaS) sanal ağ ile dağıtabilirsiniz?
Evet. (İsteğe bağlı olarak), Cloud Services rol örneklerini sanal ağlar içinde dağıtabilirsiniz. Bunu yapmak için sanal ağ adına ve rol/alt ağ eşlemeleri hizmet yapılandırmanıza ağ yapılandırma bölümünde belirtin. Herhangi bir ikili dosyalarınızı güncelleştirmek gerekmez.

### <a name="can-i-connect-a-virtual-machine-scale-set-to-a-vnet"></a>Sanal makine ölçek kümesi bir sanal ağa bağlayabilirim?
Evet. Sanal makine ölçek kümesi bir sanal ağa bağlanması gerekir.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Azure'nın tam bir listesi bir Vnet'e kaynaklardan dağıtabileceğiniz Hizmetleri mı?
Evet, Ayrıntılar için bkz. [Azure Hizmetleri için sanal ağ tümleştirmesi](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Hangi Azure PaaS kaynaklarına erişim için bir VNet kısıtlarım?

Bazı Azure PaaS hizmetlerinin (örneğin, Azure depolama ve Azure SQL veritabanı) aracılığıyla dağıtılan kaynaklar ağ erişimi kısıtlayabilir yalnızca sanal ağ hizmet uç noktaları kullanarak bir sanal ağ içindeki kaynaklarla. Ayrıntılar için bkz [sanal ağ hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Sanal ağlar içine ve dışına Hizmetlerim taşıyabilirim?
Hayır. Sanal ağlar hizmetlerde taşıyamazsınız. Başka bir sanal ağa bir kaynak taşıma için silin ve kaynak dağıtmanız gerekir.

## <a name="security"></a>Güvenlik

### <a name="what-is-the-security-model-for-vnets"></a>Sanal ağlar için güvenlik modeli nedir?
Sanal ağlar birbirlerine ve Azure altyapısı barındırılan diğer hizmetlerden yalıtılır. Bir sanal ağa bir güven sınırı var.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Sanal ağa bağlı kaynaklara gelen veya giden trafik akışını kısıtlayabilir mi?
Evet. Uygulayabileceğiniz [ağ güvenlik grupları](security-overview.md) VNet veya her ikisi için sanal ağ içindeki belirli alt ağlar için NIC eklenmiş.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>VNet-bağlı kaynaklar arasında bir güvenlik duvarı uygulayan miyim?
Evet. Dağıtabileceğiniz bir [güvenlik duvarı ağ sanal Gereci](https://azure.microsoft.com/marketplace/?term=firewall) Azure Marketi aracılığıyla birkaç satıcılardan.

### <a name="is-there-information-available-about-securing-vnets"></a>Var. Sanal ağları güvenli hale getirme hakkında bilgi kullanılabilir?
Evet. Ayrıntılar için bkz [Azure ağ güvenliğine genel bakış](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>API'leri, şemalar ve Araçlar

### <a name="can-i-manage-vnets-from-code"></a>Sanal ağlar koddan yönetebilir miyim?
Evet. Bulunan sanal ağlar için REST API'lerini kullanma [Azure Resource Manager](/rest/api/virtual-network) ve [Klasik](https://go.microsoft.com/fwlink/?LinkId=296833) dağıtım modelleri.

### <a name="is-there-tooling-support-for-vnets"></a>Sanal ağlar için araç desteği var mı?
Evet. Kullanma hakkında daha fazla bilgi edinin:
- Vnet üzerinden dağıtmak için Azure portalında [Azure Resource Manager](manage-virtual-network.md#create-a-virtual-network) ve [Klasik](virtual-networks-create-vnet-classic-pportal.md) dağıtım modelleri.
- Üzerinden dağıtılan sanal ağlarda yönetmek için PowerShell'i [Resource Manager](/powershell/module/az.network) ve [Klasik](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0) dağıtım modelleri.
- Azure komut satırı arabirimi (CLI) dağıtma ve yönetme ile dağıtılan sanal ağlarda [Resource Manager](/cli/azure/network/vnet) ve [Klasik](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) dağıtım modelleri.  

## <a name="vnet-peering"></a>VNet eşlemesi

### <a name="what-is-vnet-peering"></a>VNet eşlemesi nedir?
VNet eşlemesi (veya sanal ağ eşlemesi) sanal ağları bağlamanıza olanak sağlar. Sanal ağ arasında VNet eşleme bağlantısı, özel IPv4 adresleri üzerinden aralarındaki trafik yönlendirme sağlar. Eşlenmiş sanal ağlardaki sanal makineler aynı ağ içinde yoksa gibi birbiriyle iletişim kurabilir. Bu sanal ağları, aynı bölgedeki veya farklı bölgelerde (diğer adıyla genel sanal ağ eşleme) olabilir. Sanal ağ eşleme bağlantılarını da Azure abonelikleri genelinde oluşturulabilir.

### <a name="can-i-create-a-peering-connection-to-a-vnet-in-a-different-region"></a>Farklı bir bölgede bir VNet eşleme bağlantısı oluşturabilir miyim?
Evet. Küresel VNet eşlemesi, farklı bölgelerdeki sanal ağları eşlemenize olanak sağlar. Küresel VNet eşlemesi tüm genel Azure bölgeleri, Çin bulut bölgeleri ve kamu bulut bölgelerinde kullanılabilir. Ulusal bulut bölgeleri için Azure ortak bölgelerden genel eş olamaz.

### <a name="what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers"></a>Genel sanal ağ eşleme ve yük Dengeleyiciler ile ilgili sınırlamalar nelerdir?
İki sanal ağ farklı bölgede (genel sanal ağ eşleme), temel yük dengeleyici kullanan kaynaklara bağlanamıyor. Standard Load Balancer kullanan kaynaklara bağlanabilir.
Küresel VNet eşlemesi bunlara kuramayan anlamına temel yük dengeleyicileri aşağıdaki kaynakları kullanın:
- Temel yük dengeleyici arkasındaki VM'ler
- Temel yük Dengeleyiciler ile sanal makine ölçek kümeleri 
- Redis Cache 
- Uygulama ağ geçidi (v1) SKU
- Service Fabric
- SQL MI
- API Management
- Active Directory etki alanı hizmeti (ADDS)
- Logic Apps
- HDInsight
-   Azure Batch
- AKS
- App Service Ortamı

Bu kaynağı ExpressRoute veya VNet-VNet VNet ağ geçitleri aracılığıyla aracılığıyla bağlanabilirsiniz.

### <a name="can-i-enable-vnet-peering-if-my-virtual-networks-belong-to-subscriptions-within-different-azure-active-directory-tenants"></a>My sanal ağlar farklı Azure Active Directory Kiracı içindeki aboneliklere ait sanal ağ eşlemesi etkinleştirebilirim?
Evet. Farklı Azure Active Directory kiracıları için aboneliklerinizi aitse (yerel veya genel olup olmadığını) VNet eşlemesini oluşturmak mümkündür. PowerShell veya CLI bunu yapabilirsiniz. Portal henüz desteklenmiyor.

### <a name="my-vnet-peering-connection-is-in-initiated-state-why-cant-i-connect"></a>Eşleme bağlantısını konusu Ağımda *başlatılan* durum, neden bağlanamıyorum?
Eşleme bağlantınızda bir başlatılan durumda ise, bu yalnızca bir bağlantı oluşturduğunuz anlamına gelir. Başarılı bir bağlantı kurulabilmesi için çift yönlü bağlantıyı yeniden oluşturulması gerekir. Örneğin, eş VNet A ile VNet b için bir bağlantı Sanalağa için sanalağb eşlenir ve Sanalağb Sanalağa için oluşturulması gerekir. Her iki bağlantı oluşturuluyor durumuna değiştirecek *bağlandı.*

### <a name="my-vnet-peering-connection-is-in-disconnected-state-why-cant-i-create-a-peering-connection"></a>Eşleme bağlantısını konusu Ağımda *bağlantısı kesilmiş* neden olamaz oluşturabilirim eşleme bağlantı durumu?
Sanal ağ eşleme bağlantısını bağlantısı kesik durumda ise, oluşturulan bağlantılardan birini silindi anlamına gelir. Yeniden eşleme bir bağlantı kurulabilmesi için bağlantıyı silip yeniden oluşturmanız gerekir.

### <a name="can-i-peer-my-vnet-with-a-vnet-in-a-different-subscription"></a>Ağımda farklı Abonelikteki bir sanal ağ ile eşler arası?
Evet. Abonelikler arasında ve farklı bölgelerdeki sanal ağları eşleyebilirsiniz.

### <a name="can-i-peer-two-vnets-with-matching-or-overlapping-address-ranges"></a>İki sanal ağ ile eşleşen ya da çakışan adres aralıkları eşler arası?
Hayır. VNet eşlemesi etkinleştirmek için adres alanları çakışmamalıdır.

### <a name="how-much-do-vnet-peering-links-cost"></a>VNet eşlemesi bağlantı maliyeti ne kadar musunuz?
Bir VNet eşleme bağlantısı oluşturmak için ücret alınmaz. Eşleme bağlantıları arasında veri aktarımı ücretlendirilir. [Burada gördüğünüz](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="is-vnet-peering-traffic-encrypted"></a>Sanal ağ eşleme trafik şifrelenir?
Hayır. Eşlenmiş sanal ağlardaki kaynaklar arasında trafiği, özel ve ayrı tutulur. Tamamen Microsoft Backbone da kalır.

### <a name="why-is-my-peering-connection-in-a-disconnected-state"></a>Bağlantısı kesik durumda eşleme bağlantım neden oluyor?
Sanal ağ eşleme bağlantılarını gitmesi *bağlantısı kesilmiş* bir VNet eşleme bağlantısı silindiğinde durumu. Başarılı bir eşleme bağlantıyı yeniden kurmak için her iki bağlantı silmeniz gerekir.

### <a name="if-i-peer-vneta-to-vnetb-and-i-peer-vnetb-to-vnetc-does-that-mean-vneta-and-vnetc-are-peered"></a>Ben Sanalağa Sanalağb için eş ve ben Sanalağb VNetC için eş, Sanalağa ve VNetC eşlenmiş anlama geliyor?
Hayır. Geçişli eşleme desteklenmiyor. C ve VNetC Bunun gerçekleşmesi için eş gerekir.

### <a name="are-there-any-bandwidth-limitations-for-peering-connections"></a>Eşleme bağlantıları için herhangi bir bant genişliği sınırlaması var mı?
Hayır. VNet eşlemesi, yerel veya genel bant genişliği kısıtlamalar uygulamaz. Bant genişliği, VM veya işlem kaynağı tarafından yalnızca sınırlıdır.

### <a name="how-can-i-troubleshoot-vnet-peering-issues"></a>VNet eşlemesi sorunları'ilgili sorunları nasıl giderebilirim?
İşte bir [sorun giderici Kılavuzu](https://support.microsoft.com/en-us/help/4486956/troubleshooter-for-virtual-network-peering-issues) deneyebilirsiniz.

## <a name="virtual-network-tap"></a>Sanal ağ TAP

### <a name="which-azure-regions-are-available-for-virtual-network-tap"></a>Sanal ağ TAP için hangi Azure bölgeleri mevcuttur?
Sanal ağ TAP Önizleme tüm Azure bölgelerinde kullanılabilir. İzlenen ağ arabirimleri, sanal ağ TAP kaynağı ve Toplayıcı veya analiz çözümü için aynı bölgede dağıtılması gerekir.

### <a name="does-virtual-network-tap-support-any-filtering-capabilities-on-the-mirrored-packets"></a>Sanal ağ GİRİŞİ herhangi filtreleme yetenekleri yansıtılmış paketlerde destekliyor mu?
Filtreleme yetenekleri, sanal ağ TAP önizlemesi ile desteklenmez. Ne zaman DOKUNUN yapılandırma tüm giriş derin bir kopyasını bir ağ arabirimine eklenir ve çıkış trafiği Ağ arabirimindeki DOKUNUN hedefe sağlanacağına.

### <a name="can-multiple-tap-configurations-be-added-to-a-monitored-network-interface"></a>İzlenen ağ arabirimine birden çok DOKUNUN yapılandırmaları eklenebilir?
İzlenen ağ arabirimi, yalnızca bir DOKUNUN yapılandırması olabilir. Denetleme ile tek tek [iş ortağı çözümleri](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions) birden çok kopyasını analiz araçları, tercih ettiğiniz DOKUNUN trafik akışı yeteneği için.

### <a name="can-the-same-virtual-network-tap-resource-aggregate-traffic-from-monitored-network-interfaces-in-more-than-one-virtual-network"></a>Aynı sanal ağ TAP kaynağı, izlenen ağ arabirimlerinden birden fazla sanal ağ içinde trafiği toplayabilirsiniz?
Evet. Aynı sanal ağ TAP kaynak için kullanılabilir yansıtılmış trafik eşlenmiş sanal ağlarda aynı abonelikte veya farklı bir abonelikte bulunan izlenen ağ arabirimlerinden toplama. Sanal ağ TAP kaynak ve hedef yük dengeleyici veya hedef ağ arabirimi aynı abonelikte olması gerekir. Tüm abonelikler aynı Azure Active Directory kiracısı altında olması gerekir.

### <a name="are-there-any-performance-considerations-on-production-traffic-if-i-enable-a-virtual-network-tap-configuration-on-a-network-interface"></a>Ben bir ağ arabirimi bir sanal ağ TAP yapılandırmasına etkinleştirirseniz üretim trafiği üzerinde herhangi bir performans değerlendirmeleri vardır?

Sanal ağ TAP Önizleme aşamasındadır. Önizleme sırasında hizmet düzeyi anlaşması yoktur. Özelliği, üretim iş yükleri için kullanılmamalıdır. Bir sanal makinenin ağ arabirimiyle bir DOKUNUN yapılandırmasıyla etkin olduğunda, üretim trafiği göndermek için sanal makineye tahsis edilen azure ana bilgisayarda aynı kaynakları yansıtma işlevi gerçekleştirmek ve yansıtılmış paketleri göndermek için kullanılır. Doğru seçin [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yeterli kaynak üretim ve yansıtılmış trafiği göndermek sanal makine için kullanılabilir olmasını sağlamak için sanal makine boyutu.

### <a name="is-accelerated-networking-for-linuxcreate-vm-accelerated-networking-climd-or-windowscreate-vm-accelerated-networking-powershellmd-supported-with-virtual-network-tap"></a>Hızlandırılmış için ağ [Linux](create-vm-accelerated-networking-cli.md) veya [Windows](create-vm-accelerated-networking-powershell.md) desteklenen sanal ağ TAP?

Bir DOKUNUN yapılandırması ile hızlandırılmış ağ etkin bir sanal makineye bağlı ağ arabirimi eklemek mümkün olacaktır. Ancak, sanal makinede gecikme süresi ve performans yük boşaltma trafiği yansıtma için şu anda Azure tarafından desteklenmediğinden DOKUNUN yapılandırma hızlandırılmış ağ bağlantısı ekleyerek etkilenecek.

## <a name="virtual-network-service-endpoints"></a>Sanal ağ hizmet uç noktaları

### <a name="what-is-the-right-sequence-of-operations-to-set-up-service-endpoints-to-an-azure-service"></a>Bir Azure hizmeti için hizmet uç noktaları ayarlama işlemlerini doğru dizi nedir?
Bir hizmet uç noktaları aracılığıyla Azure hizmet kaynağıyla güvenliğini sağlamak için iki adımı vardır:
1. Azure hizmeti için hizmet uç noktalarını etkinleştirin.
2. Sanal ağ ACL'leri Azure hizmetini ayarlayın.

İlk adım bir ağ tarafı işlemi ve ikinci adım bir hizmet kaynak tarafı işlemi. Adımların ikisini de aynı yönetici veya yönetici rolüne RBAC izinler göre farklı yöneticiler tarafından gerçekleştirilebilir. İlk üzerinde hizmet uç noktaları Azure hizmet tarafında VNet ACL'leri ayarı önce sanal ağınız için kapatmanızı öneririz. Bu nedenle, sanal ağ hizmet uç noktalarını ayarlamak için yukarıda listelenen sırayla adımların gerçekleştirilmesi gerekir.

>[!NOTE]
> Yukarıda açıklanan iki işlem izin verilen VNet ve alt ağ için Azure hizmet erişimini sınırlayabilirsiniz önce tamamlanması gerekir. Yalnızca ağ tarafında Azure hizmeti için hizmet uç noktası açmak, sınırlı erişim sağlamaz. Ayrıca, Azure hizmet tarafında de VNet ACL'leri ayarlamanız gerekir.

Özel durumlar için yukarıdaki sırasıyla belirli hizmetler (örneğin, SQL ve CosmosDB) izin **IgnoreMissingVnetServiceEndpoint** bayrağı. Bayrağı ayarlandığında **True**, sanal ağ ACL'leri, ağ tarafında hizmet uç noktaları ayarlama önce Azure hizmet tarafında ayarlanabilir. Burada, belirli IP Güvenlik Duvarı Azure hizmetlerinde yapılandırılır ve genel IPv4 adresine kaynak IP değişir olduğundan, hizmet uç noktalarını ağ tarafında açmak için bir bağlantı bırakma neden olabilir durumlarda müşterilere yardımcı olmak için bu bayrağı Azure hizmetleri sağlama özel bir adresi. Hizmet uç noktaları ağ tarafta ayarlamadan önce Azure hizmet tarafında VNet ACL'leri ayarı, bağlantı bırakma önlemeye yardımcı olabilir.

### <a name="do-all-azure-services-reside-in-the-azure-virtual-network-provided-by-the-customer-how-does-vnet-service-endpoint-work-with-azure-services"></a>Tüm Azure Hizmetleri müşteri tarafından sağlanan Azure sanal ağ alanında bulunan? Sanal ağ hizmet uç noktası, Azure Hizmetleri ile nasıl çalışır?

Hayır, tüm Azure Hizmetleri, müşterinin sanal ağında bulunur. Azure depolama, Azure SQL ve Azure Cosmos DB, genel IP adresleri üzerinden erişilebilen çok kiracılı hizmetler gibi Azure veri çoğunluğu Hizmetleri. Azure Hizmetleri için sanal ağ tümleştirmesi hakkında daha fazla bilgi [burada](virtual-network-for-azure-services.md). 

(Sanal ağ hizmet uç noktası ağ tarafında açmak ve Azure hizmet tarafında uygun sanal ağ ACL'leri ayarlama) sanal ağ hizmet uç noktaları özelliği kullandığınızda, izin verilen bir VNet ve alt ağ bir Azure hizmetine erişimi sınırlıdır.

### <a name="how-does-vnet-service-endpoint-provide-security"></a>Sanal ağ hizmet uç noktası güvenlik nasıl sağlar?

(Sanal ağ hizmet uç noktası ağ tarafında açmak ve Azure hizmet tarafında uygun sanal ağ ACL'leri ayarlama) sanal ağ hizmet uç noktası özelliği izin verilen VNet ve alt ağ, ağ düzeyinde güvenlik ve yalıtım, bu nedenle sağlayan Azure hizmet erişimi sınırlar Azure hizmet trafiği. Tüm trafiği sanal ağ hizmet uç noktaları kullanarak, bu nedenle genel internet'ten başka bir yalıtım katmanı sağlayarak, Microsoft omurga üzerinden akar. Ayrıca, müşteriler Azure hizmet kaynaklarının genel Internet erişimini tamamen kaldırmak ve yalnızca kendi sanal ağ IP Güvenlik Duvarı ve sanal ağ ACL'leri, bu nedenle Azure hizmet kaynaklarının yetkisiz gelen koruma bir birleşimi yoluyla gelen trafiğe izin vermeyi seçebilir erişim.      

### <a name="what-does-the-vnet-service-endpoint-protect---vnet-resources-or-azure-service"></a>Ne yaptığını sanal ağ hizmet uç noktası koruma - ağ kaynakları veya Azure hizmeti mi?
Sanal ağ hizmet uç noktaları, Azure hizmet kaynaklarının korunmasına yardımcı olur. Sanal ağ kaynakları, ağ güvenlik grupları (Nsg'ler) korunur.

### <a name="is-there-any-cost-for-using-vnet-service-endpoints"></a>Sanal ağ hizmet uç noktalarının kullanımından herhangi bir maliyet var mı?

Hayır, sanal ağ hizmet uç noktalarının kullanımından ek ücret yoktur.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-subscriptions"></a>Sanal ağ hizmet uç noktaları üzerinde etkinleştirmek ve miyim sanal ağ ve Azure hizmet kaynaklarının farklı aboneliklere ait sanal ağ ACL'leri verilirse?

Evet, olabilir. Sanal ağlar ve Azure hizmet kaynakları aynı veya farklı Aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynakları aynı Active Directory (AD) kiracısı altında olması gerekir, tek gereksinim olmasıdır.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-ad-tenants"></a>Sanal ağ hizmet uç noktaları üzerinde etkinleştirmek ve miyim sanal ağ ve Azure hizmet kaynaklarının farklı AD kiracılara ait sanal ağ ACL'leri verilirse?
Hayır, sanal ağ hizmet uç noktaları ve sanal ağ ACL'leri AD kiracılarında desteklenmez.

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gateway-vpn-or-express-route-gateway-access-azure-paas-service-over-vnet-service-endpoints"></a>Azure sanal ağ geçidi (VPN) veya Express route ağ geçidi bağlanan bir şirket içi cihazın IP adresi, sanal ağ hizmet uç noktaları Azure PaaS hizmeti erişebilir miyim?
Varsayılan olarak sanal ağlara ayrılmış olan Azure hizmeti kaynaklarına şirket içi ağlardan erişmek mümkün değildir. Şirket içi gelen trafiğe izin vermek istiyorsanız, genel (genelde NAT) IP de izin vermeniz gerekir, şirket içi ya da ExpressRoute adreslerinden. Bu IP adresleri IP güvenlik duvarı yapılandırması Azure hizmet kaynakları için eklenebilir.

### <a name="can-i-use-vnet-service-endpoint-feature-to-secure-azure-service-to-multiple-subnets-with-in-a-virtual-network-or-across-multiple-virtual-networks"></a>Azure hizmeti için birden çok alt ağa sahip bir sanal ağdaki veya birden çok sanal ağ arasında güvenli hale getirmek için sanal ağ hizmet uç noktası özelliğini kullanabilir miyim?
Bir sanal ağ içindeki veya birden çok sanal ağda birden fazla alt ağdaki Azure hizmetlerinin güvenliğini sağlamak için hizmet uç noktaları her alt ağ tarafında ayrı ayrı etkinleştirebilir ve oluşturarak tüm alt ağların Azure hizmet kaynaklarını güvenli hale getirme Azure Hizmet tarafı uygun VNet ACL'ler.
 
### <a name="how-can-i-filter-outbound-traffic-from-a-virtual-network-to-azure-services-and-still-use-service-endpoints"></a>Nasıl Azure Hizmetleri için sanal ağdan giden trafiği filtrelemek ve hizmet uç noktaları kullanmaya devam?
Sanal ağdan bir Azure hizmetine giden trafiği incelemek veya filtrelemek istiyorsanız, sanal ağda ağ sanal Gereci dağıtabilirsiniz. Ardından, ağ sanal gerecinin dağıtılmış ve güvenli Azure hizmet kaynağını yalnızca bu alt ağ ile sanal ağ ACL'leri olduğu alt ağ hizmet uç noktaları uygulayabilirsiniz. Bu senaryo Azure hizmet erişimini ağ sanal Gereci filtresi kullanarak yalnızca belirli Azure kaynaklarına sanal ağınızdan kısıtlamak istiyorsanız yararlı olabilir. Daha fazla bilgi için bkz. [Ağ sanal gereçleri ile çıkış](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).

### <a name="what-happens-when-you-access-an-azure-service-account-that-has-virtual-network-access-control-list-acl-enabled-from-outside-the-vnet"></a>VNet dışından gelen etkin sanal ağ erişim denetimi listesi (ACL) sahip bir Azure hizmeti hesabına eriştiğinde ne olur?
HTTP 403 veya HTTP 404 hatası döndürülür.

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-service-account-in-another-region"></a>Başka bir bölgede bir Azure hizmeti hesabına erişmesi için izin verilen farklı bölgelerde oluşturulmuş sanal ağ alt ağları misiniz? 
Evet, Azure hizmetlerinin çoğu için farklı bölgelerde oluşturulmuş sanal ağlar başka bir bölgede Azure hizmetlerini sanal ağ hizmet uç noktaları yoluyla erişebilirsiniz. Örneğin, bir Azure Cosmos DB hesabı Batı ABD veya Doğu ABD ve sanal ağları birden çok bölgede sanal ağı Azure Cosmos DB erişebilirsiniz. Depolama ve SQL özel durumlar ve doğası gereği Bölgesel ve hem sanal ağ hem de Azure hizmetiyle aynı bölgede olması gerekir.
  
### <a name="can-an-azure-service-have-both-vnet-acl-and-an-ip-firewall"></a>Bir Azure hizmeti, VNet ACL hem bir IP Güvenlik Duvarı olabilir mi?
Evet, sanal ağ ACL'si ve bir IP Güvenlik Duvarı'nı birlikte bulunabilir. Her iki özellik, diğer yalıtım ve güvenlik sağlamak için tamamlar.
 
### <a name="what-happens-if-you-delete-a-virtual-network-or-subnet-that-has-service-endpoint-turned-on-for-azure-service"></a>Bir sanal ağ veya Azure hizmeti için açık hizmet uç noktası olan alt ağ'ı silersem ne olur?
Sanal ağlar ve alt ağları silinmesini bağımsız işlemler ve hatta hizmet uç noktaları Azure Hizmetleri için açık olduğunda desteklenir. Azure hizmetlerinin VNet, bu sanal ağlar ve alt ağlar için ayarlama ACL'leri sahip olduğu durumlarda bir VNet veya VNet Hizmeti uç noktası açık olan alt silindiğinde ile Azure hizmet devre dışı bırakılmış VNet ACL'leri bilgileri ilişkili.
 
### <a name="what-happens-if-azure-service-account-that-has-vnet-service-endpoint-enabled-is-deleted"></a>VNet Hizmeti uç noktası etkin olan Azure hizmet hesabı silinirse ne olur?
Azure hizmet hesabının silinmesini bağımsız bir işlemdir ve hizmet uç noktası ağ tarafta etkinleştirilir ve sanal ağ ACL'leri Azure hizmet tarafında ayarlanır bile desteklenir. 

### <a name="what-happens-to-the-source-ip-address-of-a-resource-like-a-vm-in-a-subnet-that-has-vnet-service-endpoint-enabled"></a>Sanal ağ hizmet uç noktası etkin olan bir kaynağın (örneğin, bir VM alt ağ) kaynak IP adresine ne olacak?
Sanal ağ hizmet uç noktaları etkin olduğunda, kaynak IP adresleri, sanal ağınızın alt ağdaki kaynaklara anahtarları Azure hizmeti trafiği için Azure sanal ağ özel IP adresleri genel IPv4 adresi kullanarak. Bu Azure Hizmetleri başarısız için genel bir IPv4 adresi için daha önce ayarlanan belirli bir IP Güvenlik Duvarı neden olabileceğini unutmayın. 

### <a name="does-service-endpoint-route-always-take-precedence"></a>Hizmet uç noktası rotası her zaman öncelikli mu?
Hizmet uç noktaları BGP yolları önceliklidir ve en uygun yönlendirme sağlamak için hizmet uç noktası trafiğini bir sistem yolu ekleyin. Hizmet uç noktaları, Microsoft Azure omurga ağı üzerinde her zaman hizmet trafiğini sanal ağınızdan doğrudan hizmete yönlendirir. Azure'nın bir yolu nasıl seçer hakkında daha fazla bilgi için bkz. [Azure sanal ağ trafiği yönlendirme](virtual-networks-udr-overview.md).
 
### <a name="how-does-nsg-on-a-subnet-work-with-service-endpoints"></a>NSG bir alt ağdaki hizmet uç noktaları ile nasıl çalışır?
Azure hizmete erişmek için Nsg'ler giden bağlantıya izin gerekir. Tüm İnternet'e giden trafik, Nsg'ler açtıysanız, hizmet uç noktası trafiğini çalışması gerekir. Yalnızca hizmet etiketleri kullanarak IP hizmetine giden trafiği de sınırlayabilirsiniz.  
 
### <a name="what-permissions-do-i-need-to-set-up-service-endpoints"></a>Hizmet uç noktalarını ayarlamak hangi izinlerin gerekiyor?
Hizmet uç noktaları sanal ağda birbirinden bağımsız olarak sanal ağda yazma erişimine sahip bir kullanıcı tarafından yapılandırılabilir. Azure hizmet kaynaklarını bir sanal ağ ile sınırlamak için kullanıcının eklenen alt ağlarda **Microsoft.Network/JoinServicetoaSubnet** iznine sahip olması gerekir. Bu izin, varsayılan olarak yerleşik Hizmet Yöneticisi rolü dahil edilir ve özel roller oluşturularak değiştirilebilir. Yerleşik rolleri hakkında daha fazla bilgi edinin ve belirli izinlerin atanması [özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles?toc=%2fazure%2fvirtual-network%2ftoc.json).
 

### <a name="can-i-filter-virtual-network-traffic-to-azure-services-allowing-only-specific-azure-service-resources-over-vnet-service-endpoints"></a>Azure Hizmetleri için sanal ağ trafiği yalnızca belirli bir azure hizmet kaynakları sanal ağ hizmet uç noktaları izin vererek filtre sağlayabilirsiniz? 

Sanal ağ (VNet) hizmet uç noktası ilkelerini yalnızca belirli bir Azure hizmet kaynaklarını hizmet uç noktaları izin vererek, Azure Hizmetleri için sanal ağ trafiğini filtreleme olanak sağlar. Uç noktası ilkeleri, Azure Hizmetleri için sanal ağ trafiğinden ayrıntılı erişim denetimi sağlar. Hizmet uç noktası ilkeleri hakkında daha fazla bilgi [burada](virtual-network-service-endpoint-policies-overview.md).
 
### <a name="are-there-any-limits-on-how-many-vnet-service-endpoints-i-can-set-up-from-my-vnet"></a>Herhangi bir sınır üzerinde kaç tane sanal ağ hizmet uç noktaları miyim my sanal ağdan ayarlayabilirsiniz vardır?
Sanal ağ hizmet uç noktaları bir sanal ağdaki toplam sayısına bir sınır yoktur. Bir Azure hizmet kaynağında (Azure Depolama hesabı gibi), hizmetler kaynağın güvenliğini sağlamak amacıyla kullanılan alt ağ sayısını sınırlandırabilir. Aşağıdaki tabloda bazı örnek sınırlar gösterilmektedir: 

|||
|---|---|
|Azure hizmeti| Sanal ağ kuralları sınırlamaları|
|Azure Storage| 100|
|Azure SQL| 128|
|Azure SQL Veri Ambarı|  128|
|Azure KeyVault|    127|
|Azure Cosmos DB|   64|
|Azure Olay Hub'ı|   128|
|Azure Service Bus| 128|
|Azure Data Lake Store V1|  100|
 
>[!NOTE]
> Sınırları, bağlı olarak Azure hizmet değişikliklerini tabi. Hizmetler ayrıntılarını ilgili hizmet belgesine bakın. 




  



