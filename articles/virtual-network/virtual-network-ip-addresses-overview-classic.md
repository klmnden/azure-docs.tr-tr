---
title: IP adresi türleri azure'da (Klasik)
titlesuffix: Azure Virtual Network
description: Azure ortak ve özel IP adresleri (Klasik) hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: genli
ms.openlocfilehash: 9e7a5772dd1e10abf43eddf0548833d625ecfb24
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60742131"
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP adresi türleri ve ayırma yöntemleri (Azure'da Klasik)
Diğer Azure kaynaklarıyla, şirket içi ağınızla ve İnternet’le iletişim kurmak için Azure kaynaklarına IP adresleri atayabilirsiniz. Azure'da kullanabileceğiniz IP adreslerinin iki tür vardır: Genel ve özel.

Genel IP adresleri, genel kullanıma yönelik Azure Hizmetleri dahil olmak üzere Internet ile iletişim için kullanılır.

Ağınızı Azure'a genişletmek için bir VPN ağ geçidi veya ExpressRoute bağlantı hattı kullandığınızda bir Azure sanal ağı (VNet), bir bulut hizmeti ve şirket içi ağınız içinde iletişim kurmak için özel IP adresleri kullanılır.

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager kullanmanızı önerir. Kaynak Yöneticisi'nde IP adresleri hakkında bilgi edinmek [IP adresleri](virtual-network-ip-addresses-overview-arm.md) makalesi.

## <a name="public-ip-addresses"></a>Genel IP adresleri
Genel IP adresleri gibi İnternet'e ve Azure genel kullanıma yönelik hizmetler ile iletişim kurmak Azure kaynaklarını izin [Azure önbelleği için Redis](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL veritabanları](../sql-database/sql-database-technical-overview.md), ve [Azure depolama](../storage/common/storage-introduction.md).

Genel bir IP adresi, aşağıdaki kaynak türleriyle ilişkilendirilir:

* Bulut hizmetleri
* Iaas sanal makineleri (VM'ler)
* PaaS rolü örnekleri
* VPN ağ geçitleri
* Uygulama ağ geçitleri

### <a name="allocation-method"></a>Ayırma yöntemi
Azure kaynağına atanmış genel IP adresi gerektiğinde olduğu *dinamik olarak* kaynağın oluşturulduğu konum içinde kullanılabilir genel IP adresi havuzundan ayrılan. Bu IP adresi kaynağı durdurulduğunda serbest bırakılır. Tüm rol örneklerine, hangi kullanılarak önlenebilir durdurulduğunda böyle bulut hizmetiyle bir *statik* (ayrılmış) IP adresi (bkz [Cloud Services](#cloud-services)).

> [!NOTE]
> İçinden genel IP adresleri Azure kaynaklarına ayrılan IP aralıklarının Listesi sayfasında yayımlanır [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi
Bir bulut hizmeti ya da bir Iaas VM oluşturduğunuzda, azure'da tüm kaynaklar arasında benzersiz olan bir bulut hizmeti DNS adı sağlamanız gerekir. Bu Azure tarafından yönetilen DNS sunucuları için bir eşleme oluşturur *dnsname*. cloudapp.net kaynağın genel IP adresi için. Örneğin, oluşturduğunuzda, bir bulut hizmeti bir bulut hizmeti DNS adı ile **contoso**, tam etki alanı adı (FQDN) **contoso.cloudapp.net** bir genel IP adresine bulut (VIP) çözümler hizmeti. Bu FQDN’yi kullanarak Azure’daki genel IP adresini işaret eden özel bir etki alanı CNAME kaydı oluşturabilirsiniz.

### <a name="cloud-services"></a>Bulut hizmetleri
Bir bulut hizmeti, her zaman bir sanal IP adresi (VIP) başvurulan bir genel IP adresi vardır. VM'ler ve bulut hizmetindeki rol örneği iç bağlantı noktalarına VIP farklı bağlantı noktaları ilişkilendirmek için bir bulut hizmetinde uç noktaları oluşturabilirsiniz. 

Bir bulut hizmeti, birden çok bir Iaas Vm'si veya PaaS rolü örnekleri, aynı bulut hizmeti VIP'si sunulan tüm içerebilir. Ayrıca atayabilirsiniz [bir bulut hizmeti için birden çok VIP](../load-balancer/load-balancer-multivip.md), SSL tabanlı Web sitelerinde çok kiracılı ortam gibi çoklu VIP senaryolar sağlar.

Bir bulut hizmetinin genel IP adresi aynı kalır, hatta tüm rol örneklerine, kullanarak durdurulduğunda sağlayabilirsiniz bir *statik* olarak adlandırılır, genel IP adresi [ayrılmış IP](virtual-networks-reserved-public-ip.md). Belirli bir konumda (ayrılmış) statik bir IP kaynağı oluşturun ve bu konumda bulunan herhangi bir bulut hizmeti atayın. Ayrılmış İP'si için gerçek IP adresini belirtemezsiniz, oluşturulduğu konumda kullanılabilir IP adreslerinden oluşan bir havuzdan ayrılır. Açıkça silene kadar bu IP adresi serbest bırakılmaz.

Statik (ayrılmış) genel IP adresleri yaygın olarak, bir bulut hizmeti yerlerde senaryolarda kullanılır:

* güvenlik duvarı kuralları, son kullanıcılar tarafından kurulu olmasını gerektirir.
* dış DNS adı çözünürlüğüne bağlıdır ve dinamik IP A kayıtlarının güncelleştirilmesini gerektirir.
* IP tabanlı güvenlik modeli kullandığınız dış web hizmetlerini kullanır.
* bir IP adresine bağlı SSL sertifikaları kullanır.

> [!NOTE]
> Klasik VM'yi, kapsayıcı oluştururken *bulut hizmeti* sanal bir IP adresi (VIP) olan Azure tarafından oluşturulur. Oluşturma, varsayılan bir RDP veya SSH Portalı aracılığıyla yapılır zaman *uç nokta* VM bulut hizmeti VIP'si aracılığıyla bağlanabilmesi için portal tarafından yapılandırılır. VM'ye bağlanmak için ayrılmış bir IP adresi sağlayan etkili bir şekilde bu bulut hizmeti VIP'si ayrılabilir. Daha fazla uç noktaları yapılandırarak ek bağlantı noktalarını açabilirsiniz.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>Iaas Vm'leri ve PaaS rol örnekleri
Doğrudan bir Iaas genel bir IP adresi atayabilirsiniz [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya Bulut hizmetinden PaaS rol örneği. Bu bir örnek düzeyi genel IP adresi adlandırılır ([ILPIP](virtual-networks-instance-level-public-ip.md)). Bu genel IP adresi, yalnızca dinamik olabilir.

> [!NOTE]
> Bu bir kapsayıcıdır bir Iaas Vm'si veya PaaS rolü örnekleri, bir bulut hizmetinde birden çok Iaas Vm'leri içerebileceğinden veya PaaS rolü örnekleri, aynı bulut hizmeti VIP'si kullanıma sunulan tüm bulut hizmeti VIP'si farklıdır.
> 
> 

### <a name="vpn-gateways"></a>VPN ağ geçitleri
A [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) bir Azure sanal ağı diğer Azure sanal ağları ya da şirket içi ağlara bağlanmak için kullanılabilir. Bir VPN ağ geçidi genel IP adresi atanır *dinamik olarak*, uzak ağ ile iletişim sağlar.

### <a name="application-gateways"></a>Uygulama ağ geçitleri
Bir Azure [uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md) Layer7 Yük Dengeleme HTTP tabanlı ağ trafiği yönlendirmek için kullanılabilir. Uygulama ağ geçidi genel IP adresi atandığı *dinamik olarak*, yükü dengelenmiş bir VIP görev yapar.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda olası ayırma yöntemleri (dinamik/statik) ile birden çok genel IP adresleri atamak için özelliği her bir kaynak türü gösterir.

| Resource | Dinamik | Statik | Birden çok IP adresi |
| --- | --- | --- | --- |
| Bulut hizmeti |Evet |Evet |Evet |
| Iaas VM veya PaaS rolü örneği |Evet |Hayır |Hayır |
| VPN ağ geçidi |Evet |Hayır |Hayır |
| Uygulama ağ geçidi |Evet |Hayır |Hayır |

## <a name="private-ip-addresses"></a>Özel IP adresleri
Özel IP adresleri Azure kaynakları bir bulut hizmetinde diğer kaynaklarla iletişim kurmasına izin ver veya [sanal ağ](virtual-networks-overview.md)(VNet) veya kullanmadan şirket içi ağa (üzerinden bir VPN ağ geçidi veya ExpressRoute bağlantı hattı), bir İnternet'ten erişilebilen bir IP adresi.

Azure Klasik dağıtım modelinde, aşağıdaki Azure kaynakları için özel bir IP adresi atanabilir:

* Iaas Vm'leri ve PaaS rol örnekleri
* İç yük dengeleyici
* Uygulama ağ geçidi

### <a name="iaas-vms-and-paas-role-instances"></a>Iaas Vm'leri ve PaaS rol örnekleri
Klasik dağıtım modeliyle oluşturulan sanal makineleri (VM'ler) her zaman bir bulut hizmeti, PaaS rol örneklerine benzer yerleştirilir. Bu nedenle, bu kaynaklar için özel IP adresleri davranışını benzerdir.

Bir bulut hizmeti iki şekilde dağıtılabilir dikkat edin önemlidir:

* Olarak bir *tek başına* bulut hizmeti, onu olduğu bir sanal ağ içinde.
* Bir sanal ağın bir parçası olarak.

#### <a name="allocation-method"></a>Ayırma yöntemi
Durumunda, bir *tek başına* bulut hizmeti, özel bir IP adresi ayrılmış kaynakları get *dinamik olarak* Azure veri merkezinde özel bir IP adres aralığı. Yalnızca aynı bulut hizmetindeki diğer vm'lerle iletişim için kullanılabilir. Kaynak durduruldu ve başlatılmışsa bu IP adresi değişebilir.

Dağıtılan bir sanal ağ içinde bir bulut hizmeti olması durumunda, özel IP adresleri ilişkili alt ağı, başarılı (ağ yapılandırmasında belirtildiği şekilde) adresi aralığından ayrılan kaynakları edinin. Bu özel IP adresleri, sanal ağ içindeki tüm sanal makineler arasındaki iletişim için kullanılabilir.

Ayrıca, sanal ağ içindeki bulut Hizmetleri olması durumunda, özel bir IP adresi ayrılır *dinamik olarak* (DHCP kullanılarak) varsayılan olarak. Kaynak durduruldu ve çalışmaya değiştirebilirsiniz. IP adresinin aynı kalmasını sağlamak için ayırma yöntemini ayarlamak için gereken *statik*ve karşılık gelen adres aralığında geçerli bir IP adresi sağlayın.

Statik özel IP adresleri yaygın olarak şunlar için kullanılır:

* Etki alanı denetleyicisi veya DNS sunucusu olarak çalışan VM’ler.
* IP adresleri kullanarak güvenlik duvarı kuralları gerektiren VM'ler.
* Bir IP adresi üzerinden diğer uygulamalar tarafından erişilen Hizmetleri çalıştıran VM'ler.

#### <a name="internal-dns-hostname-resolution"></a>İç DNS ana bilgisayar adı çözümlemesi
Tüm Azure Vm'leri ve PaaS rolü örnekleri ile yapılandırılan [Azure tarafından yönetilen DNS sunucuları](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) varsayılan olarak, siz açıkça özel DNS sunucuları yapılandırmadığınız sürece. Bu DNS sunucuları, Vm'leri ve aynı VNet veya Bulut hizmetinde bulunan rol örnekleri için iç ad çözümleme sağlar.

Bir VM oluşturduğunuzda, Azure tarafından yönetilen DNS sunucularına ana bilgisayar adı için özel IP adresine yönelik bir eşleme eklenir. Multi-NIC VM ile ana bilgisayar adı birincil NIC özel IP adresine eşlenir. Ancak, bu eşleme bilgileri aynı bulut hizmetinde veya sanal ağ içindeki kaynaklara sınırlıdır.

Durumunda, bir *tek başına* bulut hizmeti, tüm VM'ler/rol örnekleri yalnızca aynı bulut hizmetinde ana bilgisayar adlarını çözmek mümkün olacaktır. Sanal ağ içinde bir bulut hizmeti olması durumunda, sanal ağ içindeki tüm VM'ler/rol örnekleri ana bilgisayar adlarını çözmek mümkün olacaktır.

### <a name="internal-load-balancers-ilb--application-gateways"></a>İç yük dengeleyiciler (ILB) ve Uygulama ağ geçitleri
Bir [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md)’ın (ILB) veya [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)’in **ön uç** yapılandırmasına özel bir IP adresi atayabilirsiniz. Özel IP adresi, yalnızca kendi sanal ağı (VNet) ve VNet’e bağlı uzak ağlar tarafından erişilebilen bir iç uç nokta olarak çalışır. Ön uç yapılandırmasına dinamik veya statik bir özel IP adresi atayabilirsiniz. Ayrıca, çoklu vip senaryoları etkinleştirmek için birden çok özel IP adresleri atayabilirsiniz.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda olası ayırma yöntemleri (dinamik/statik) ile birden çok özel IP adresleri atamak için özelliği her bir kaynak türü gösterir.

| Resource | Dinamik | Statik | Birden çok IP adresi |
| --- | --- | --- | --- |
| VM (içinde bir *tek başına* bulut hizmeti veya sanal ağ) |Evet |Evet |Evet |
| PaaS rol örneği (içinde bir *tek başına* bulut hizmeti veya sanal ağ) |Evet |Hayır |Hayır |
| İç yük dengeleyici ön ucuna |Evet |Evet |Evet |
| Uygulama ağ geçidi ön uç |Evet |Evet |Evet |

## <a name="limits"></a>Limits
Aşağıdaki tabloda, Azure'da abonelik başına adresleme IP uygulanan sınırları gösterir. [Destek ekibiyle iletişime geçerek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) varsayılan limitleri iş ihtiyaçlarınıza göre en üst düzeye çıkarabilirsiniz.

|  | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Genel IP adresleri (dinamik) |5 |desteğe başvurun |
| Ayrılmış genel IP adresleri |20 |desteğe başvurun |
| Dağıtım (bulut hizmeti) başına genel VIP |5 |desteğe başvurun |
| Özel VIP (ILB) başına dağıtım (bulut hizmeti) |1 |1 |

Eksiksiz bir listesi okuduğunuzdan emin olun [ağ limitlerinin](../azure-subscription-service-limits.md#networking-limits) azure'da.

## <a name="pricing"></a>Fiyatlandırma
Çoğu durumda, genel IP adresleri ücretsizdir. Ek ve/veya statik genel IP adresleri kullanmak için nominal bir ücret yoktur. Anladığınızdan emin olun [genel IP'lere yönelik fiyatlandırma yapısı](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Resource Manager ve klasik dağıtımlar arasındaki farklar
Resource Manager ve klasik dağıtım modeli IP adresleme özelliklerin karşılaştırması aşağıdadır.

|  | Resource | Klasik | Resource Manager |
| --- | --- | --- | --- |
| **Genel IP adresi** |***VM*** |Başvurulan bir ILPIP (yalnızca dinamik) |İçin genel IP (dinamik veya statik) adlandırılır. |
|  ||Bir Iaas VM'si veya PaaS rolü örneğini atanan |VM'nin NIC ile ilişkilendirilir |
|  |***Internet'e yönelik Yük Dengeleyici*** |VIP (dinamik) veya ayrılmış IP (statik) olarak adlandırılır |İçin genel IP (dinamik veya statik) adlandırılır. |
|  ||Bir bulut hizmetine atanan |Load balancer'ın ön uç yapılandırma için ilişkili |
|  | | | |
| **Özel IP adresi** |***VM*** |İçin bir DIP adlandırılır. |İçin özel bir IP adresi adlandırılır. |
|  ||Bir Iaas VM'si veya PaaS rolü örneğini atanan |VM'nin NIC'ye atanmış |
|  |***İç yük dengeleyici (ILB)*** |ILB (dinamik veya statik) atanmış |Atanan ILB'nin ön uç yapılandırmasına (dinamik veya statik) |

## <a name="next-steps"></a>Sonraki adımlar
* [Statik özel IP adresi ile VM dağıtma](virtual-networks-static-private-ip-classic-pportal.md) Azure portalını kullanarak.

