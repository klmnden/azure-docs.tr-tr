---
title: IP adresi türleri Azure (Klasik) | Microsoft Docs
description: Azure ortak ve özel IP adresleri (Klasik) hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: 2f8664ab-2daf-43fa-bbeb-be9773efc978
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: genli
ms.openlocfilehash: 6a63099bf2a8bc818c88ccec1d5f44bb9ffc32de
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="ip-address-types-and-allocation-methods-classic-in-azure"></a>IP adresi türleri ve ayırma yöntemleri (Azure'da Klasik)
Diğer Azure kaynaklarıyla, şirket içi ağınızla ve İnternet’le iletişim kurmak için Azure kaynaklarına IP adresleri atayabilirsiniz. Azure'da kullanabileceğiniz IP adreslerinin iki tür vardır: ortak ve özel.

Genel IP adresleri, Azure genel kullanıma yönelik Hizmetleri ile birlikte Internet ile iletişim için kullanılır.

Ağınızı Azure'a genişletmek için bir VPN ağ geçidi veya expressroute bağlantı hattı kullandığınızda özel IP adresleri bir Azure sanal ağı (VNet), bir bulut hizmeti ve şirket içi ağınız içinde iletişim için kullanılır.

> [!IMPORTANT]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md).  Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager kullanmanızı önerir. Okuyarak IP adreslerinin Kaynak Yöneticisi'nde öğrenin [IP adreslerini](virtual-network-ip-addresses-overview-arm.md) makalesi.

## <a name="public-ip-addresses"></a>Genel IP adresleri
Genel IP adresleri, Azure kaynaklarının İnternet ile ve [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Olay Hub’ları](https://azure.microsoft.com/services/event-hubs/), [SQL veritabanları](../sql-database/sql-database-technical-overview.md) ve [Azure depolama alanı](../storage/common/storage-introduction.md) gibi genel kullanıma yönelik Azure hizmetleriyle iletişim kurmasını sağlar.

Bir ortak IP adresi aşağıdaki kaynak türleri ile ilişkilidir:

* Bulut hizmetleri
* Iaas sanal makineleri (VM'ler)
* PaaS rolü örnekleri
* VPN ağ geçitleri
* Uygulama ağ geçitleri

### <a name="allocation-method"></a>Ayırma yöntemi
Bir ortak IP adresi için bir Azure kaynağı atanması gerektiğinde olur *dinamik olarak* kaynak oluşturulduğunda konum içinde kullanılabilir ortak IP adresi havuzundan ayrılmış. Kaynak durdurulduğunda bu IP adresi serbest bırakılır. Tüm rol örneklerini durduğunda böyle bir bulut hizmeti olan durumunda, hangi kaçınılabilir kullanarak bir *statik* (ayrılmış) IP adresi (bkz [bulut Hizmetleri](#Cloud-services)).

> [!NOTE]
> İçinden ortak IP adresleri için Azure kaynaklarını ayrılır IP aralıkları listesi, yayımlanan [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).
> 
> 

### <a name="dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi
Bir bulut hizmeti veya bir Iaas VM oluşturduğunuzda, tüm Azure kaynakları arasında benzersiz olan bir bulut hizmeti DNS adı sağlamanız gerekir. Bu Azure tarafından yönetilen DNS sunucuları için bir eşleme oluşturur *dnsname*. genel IP adresi kaynağının cloudapp.net. Örneğin, oluşturduğunuzda, bir bulut hizmeti bir bulut hizmeti DNS adı ile **contoso**, tam etki alanı adı (FQDN) **contoso.cloudapp.net** bir ortak IP adresine bulut hizmetinin (VIP) çözer. Bu FQDN’yi kullanarak Azure’daki genel IP adresini işaret eden özel bir etki alanı CNAME kaydı oluşturabilirsiniz.

### <a name="cloud-services"></a>Bulut hizmetleri
Bir bulut hizmeti için bir sanal IP adresi (VIP) adlandırılan bir ortak IP adresi her zaman vardır. Uç noktaları VM'ler ve rol örnekleri bulut hizmeti içindeki iç bağlantı noktalarına VIP farklı bağlantı noktaları ilişkilendirmek için bir bulut hizmeti oluşturun. 

Bir bulut hizmeti birden çok Iaas Vm'si veya PaaS rolü örnekleri, aynı bulut hizmeti VIP sunulan tüm içerebilir. Ayrıca atayabilirsiniz [bir bulut hizmeti için birden çok Vip](../load-balancer/load-balancer-multivip.md), çok kiracılı ortamı SSL tabanlı Web sitelerinde gibi çoklu VIP senaryoları etkinleştirir.

Bir bulut hizmeti genel IP adresi aynı kalır, hatta tüm rol örnekleri, kullanarak durdurulduğunda sağlayabilirsiniz bir *statik* olarak adlandırılır, genel IP adresi [ayrılmış IP](virtual-networks-reserved-public-ip.md). Belirli bir konumda bir statik (ayrılmış) IP kaynağı oluşturun ve bu konumda bulunan herhangi bir bulut hizmetine atayın. Ayrılmış IP'si için gerçek IP adresi belirtemezsiniz, oluşturulduğu konumda kullanılabilir IP adresi havuzundan ayrılır. Açıkça silene kadar bu IP adresi serbest bırakılmaz.

Statik (ayrılmış) ortak IP adresleri, genellikle bir bulut hizmeti olduğu senaryolarda kullanılır:

* Son kullanıcılar tarafından kurulması için güvenlik duvarı kurallarını gerektirir.
* dış DNS ad çözümlemesi bağlıdır ve dinamik IP A kayıtlarını güncelleştirme gerektirir.
* IP tabanlı güvenlik modeli kullanan dış web hizmetlerini kullanır.
* bir IP adresine bağlı SSL sertifikaları kullanır.

> [!NOTE]
> Bir kapsayıcı klasik bir VM oluştururken *bulut hizmeti* bir sanal IP adresi (VIP) olan Azure tarafından oluşturulur. Oluşturma RDP veya SSH varsayılan portal yapılır zaman *endpoint* VM bulut hizmeti VIP aracılığıyla bağlanabilmesi için portal tarafından yapılandırılır. Etkili bir şekilde VM'e bağlanmak için ayrılmış bir IP adresi sağlayan bu bulut hizmeti VIP ayrılabilir. Daha fazla uç noktaları yapılandırarak ek bağlantı noktalarını açabilirsiniz.
> 
> 

### <a name="iaas-vms-and-paas-role-instances"></a>Iaas Vm'leri ve PaaS rol örnekleri
Doğrudan bir Iaas genel bir IP adresi atayabilirsiniz [VM](../virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya PaaS rolü örneğini bir bulut hizmeti içinde. Bu bir örnek düzeyinde ortak IP adresi adlandırılır ([ILPIP](virtual-networks-instance-level-public-ip.md)). Bu genel IP adresi yalnızca dinamik olabilir.

> [!NOTE]
> Bu VIP Iaas Vm'si veya PaaS rolü örnekleri, bir bulut hizmeti birden çok Iaas Vm'leri içerebileceğinden veya PaaS rolü örnekleri, tümü aynı bulut hizmeti VIP kullanıma sunulan ilişkin bir kapsayıcıdır bulut hizmeti farklıdır.
> 
> 

### <a name="vpn-gateways"></a>VPN ağ geçitleri
A [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) Azure VNet diğer Azure sanal ağlar veya şirket içi ağlara bağlanmak için kullanılabilir. Bir VPN ağ geçidi genel bir IP adresi atanır *dinamik olarak*, uzak ağ ile iletişim sağlar.

### <a name="application-gateways"></a>Uygulama ağ geçitleri
Bir Azure [uygulama ağ geçidi](../application-gateway/application-gateway-introduction.md) Layer7 Yük Dengeleme HTTP tabanlı ağ trafiğini yönlendirmek için kullanılabilir. Uygulama ağ geçidi, bir ortak IP adresi atanır *dinamik olarak*, yük dengeli VIP görev yapar.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda, her kaynak türündeki ayırma yöntemlerini (dinamik/statik) ve birden çok ortak IP adresleri atamak için özelliği gösterilmektedir.

| Kaynak | Dinamik | Statik | Birden çok IP adresi |
| --- | --- | --- | --- |
| Bulut hizmeti |Evet |Evet |Evet |
| Iaas VM veya PaaS rol örneği |Evet |Hayır |Hayır |
| VPN ağ geçidi |Evet |Hayır |Hayır |
| Uygulama ağ geçidi |Evet |Hayır |Hayır |

## <a name="private-ip-addresses"></a>Özel IP adresleri
Özel IP adresleri diğer kaynaklar bir bulut hizmeti ile iletişim kurmak Azure kaynaklarını izin ver veya [sanal ağ](virtual-networks-overview.md)(VNet) veya bir Internet erişilebilir IP adresi kullanarak olmadan şirket ağından (bir VPN ağ geçidi veya expressroute bağlantı hattı).

Azure Klasik dağıtım modelinde, özel bir IP adresi için aşağıdaki Azure kaynakları atanabilir:

* Iaas Vm'leri ve PaaS rol örnekleri
* İç yük dengeleyici
* Uygulama ağ geçidi

### <a name="iaas-vms-and-paas-role-instances"></a>Iaas Vm'leri ve PaaS rol örnekleri
Klasik dağıtım modeliyle oluşturulan sanal makineleri (VM'ler) her zaman PaaS rolü örnekleri için benzer bir bulut hizmeti yerleştirilir. Özel IP adresleri davranışını böylece bu kaynaklar için benzer.

Bir bulut hizmeti iki yolla dağıtılabilir dikkate almak önemlidir:

* Farklı bir *tek başına* bulut olduğu olmayan bir sanal ağ içindeki hizmeti.
* Sanal ağın bir parçası olarak.

#### <a name="allocation-method"></a>Ayırma yöntemi
Durumunda, bir *tek başına* bulut hizmeti, özel bir IP adresi ayrılan kaynakları get *dinamik olarak* Azure veri merkezinde özel bir IP adresi aralığı. Yalnızca diğer VM'ler içinde aynı bulut hizmeti ile iletişim için kullanılabilir. Kaynak durduruldu ve başlatılmışsa bu IP adresi değiştirebilirsiniz.

Bir sanal ağ içinde dağıtılan bir bulut hizmeti durumunda özel IP adresleri (belirtildiği gibi ağ yapılandırmasıyla) ilişkili alt ağı adres aralığındaki ayrılan kaynakları alın. Bu özel IP adresleri sanal ağ içindeki tüm sanal makineler arasındaki iletişim için kullanılabilir.

Ayrıca, bir sanal ağ içindeki bulut Hizmetleri durumunda özel bir IP adresi ayrılır *dinamik olarak* (DHCP kullanarak) varsayılan olarak. Kaynak durduruldu ve başlatılmışsa değiştirebilirsiniz. IP adresinin aynı kaldığından emin olmak için ayırma yöntemi ayarlamak gereken *statik*ve karşılık gelen adres aralığı içinde geçerli bir IP adresi sağlayın.

Statik özel IP adresleri yaygın olarak şunlar için kullanılır:

* Etki alanı denetleyicisi veya DNS sunucusu olarak çalışan VM’ler.
* IP adresleri kullanarak güvenlik duvarı kuralları gerektiren VM'ler.
* IP adresi üzerinden diğer uygulamalar tarafından erişilen Hizmetleri çalıştıran VM'ler.

#### <a name="internal-dns-hostname-resolution"></a>İç DNS ana bilgisayar adı çözümlemesi
Tüm Azure Vm'leri ve PaaS rol örnekleri ile yapılandırılan [Azure tarafından yönetilen DNS sunucuları](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) varsayılan olarak, özel DNS sunucularını açıkça yapılandırmadığınız sürece. Bu DNS sunucuları, sanal makineleri ve aynı sanal veya Bulut hizmetinde bulunan rol örnekleri için dahili ad çözümlemesi sağlar.

Bir VM oluşturduğunuzda, Azure tarafından yönetilen DNS sunucularına ana bilgisayar adı için özel IP adresine yönelik bir eşleme eklenir. Birincil NIC özel IP adresi eşlenmiş multi-NIC VM durumunda, ana bilgisayar adı Ancak, bu eşleme bilgilerini aynı bulut hizmeti ya da sanal ağ içindeki kaynakların sınırlıdır.

Durumunda, bir *tek başına* bulut hizmeti, ana bilgisayar adları yalnızca aynı bulut hizmeti içindeki tüm VM'ler/rol örneklerinin çözümleyebilmesi olacaktır. Bir sanal ağ içindeki bir bulut hizmeti durumunda konak sanal ağ içindeki tüm VM'ler/rol örneklerinin çözümleyebilmesi olacaktır.

### <a name="internal-load-balancers-ilb--application-gateways"></a>İç yük dengeleyiciler (ILB) ve Uygulama ağ geçitleri
Bir [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md)’ın (ILB) veya [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)’in **ön uç** yapılandırmasına özel bir IP adresi atayabilirsiniz. Özel IP adresi, yalnızca kendi sanal ağı (VNet) ve VNet’e bağlı uzak ağlar tarafından erişilebilen bir iç uç nokta olarak çalışır. Ön uç yapılandırmasına dinamik veya statik bir özel IP adresi atayabilirsiniz. Ayrıca, çoklu vip senaryoları etkinleştirmek için birden çok özel IP adresleri de atayabilirsiniz.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda, her kaynak türündeki ayırma yöntemlerini (dinamik/statik) ve birden çok özel IP adresleri atamak için özelliği gösterilmektedir.

| Kaynak | Dinamik | Statik | Birden çok IP adresi |
| --- | --- | --- | --- |
| VM (içinde bir *tek başına* bulut hizmeti veya VNet) |Evet |Evet |Evet |
| PaaS rolü örneğini (içinde bir *tek başına* bulut hizmeti veya VNet) |Evet |Hayır |Hayır |
| İç yük dengeleyici ön uç |Evet |Evet |Evet |
| Uygulama ağ geçidi ön uç |Evet |Evet |Evet |

## <a name="limits"></a>Sınırlar
Aşağıdaki tabloda, Azure abonelik başına adresleme sınır IP uygulanmaz gösterilmektedir. [Destek ekibiyle iletişime geçerek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) varsayılan limitleri iş ihtiyaçlarınıza göre en üst düzeye çıkarabilirsiniz.

|  | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Genel IP adresleri (dinamik) |5 |desteğe başvurun |
| Ayrılmış genel IP adresleri |20 |desteğe başvurun |
| Genel VIP her dağıtımda (bulut hizmeti) |5 |desteğe başvurun |
| Özel VIP (ILB) her dağıtımda (bulut hizmeti) |1 |1 |

Kümesini okuma olduğundan emin olun [sınırlar için ağ](../azure-subscription-service-limits.md#networking-limits) azure'da.

## <a name="pricing"></a>Fiyatlandırma
Çoğu durumda, genel IP adresleri ücretsizdir. Ek ve/veya statik genel IP adreslerini kullanmak için nominal bir ücret yoktur. Anladığınızdan emin olun [genel IP'ler için fiyatlandırma yapısına](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Resource Manager ve klasik dağıtımlar arasındaki farklar
Aşağıda, Resource Manager ve klasik dağıtım modeli IP adresleme özellikleri karşılaştırması verilmiştir.

|  | Kaynak | Klasik | Resource Manager |
| --- | --- | --- | --- |
| **Genel IP adresi** |***VM*** |İçin bir ILPIP (yalnızca dinamik) adlandırılır. |Başvurulan bir genel IP (dinamik veya statik) |
|  ||Bir Iaas VM veya PaaS rol örneği atanan |VM'nin NIC'ye ilişkili | |
|  |***Internet'e yönelik Yük Dengeleyici*** |VIP (dinamik) veya ayrılmış IP (statik) olarak adlandırılır |Başvurulan bir genel IP (dinamik veya statik) | |
|  ||Bir bulut hizmetine atanan |Yük dengeleyicinin ön uç yapılandırma için ilişkili | |
|  | | | |
| **Özel IP adresi** |***VM*** |İçin DIP adlandırılır. |İçin özel bir IP adresi adlandırılır. |
|  ||Bir Iaas VM veya PaaS rol örneği atanan |VM'nin NIC'ye atanan | |
|  |***İç yük dengeleyiciye (ILB)*** |ILB (dinamik veya statik) atanan |(Dinamik veya statik) ILB'nin ön uç yapılandırma için atanmış | |

## <a name="next-steps"></a>Sonraki adımlar
* [Özel bir statik IP adresi bir VM'yi dağıtmak](virtual-networks-static-private-ip-classic-pportal.md) Azure portalını kullanarak.

