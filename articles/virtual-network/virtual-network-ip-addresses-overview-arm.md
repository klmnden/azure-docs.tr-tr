---
title: "Azure’da IP adresi türleri | Microsoft Docs"
description: "Azure’da genel ve özel IP adresleri hakkında bilgi edinin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2017
ms.author: jdial
ms.openlocfilehash: 95f2b57b2012df816c76a1b6ec55ca9f92e134a3
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>Azure’da IP adresi türleri ve ayırma yöntemleri

Diğer Azure kaynaklarıyla, şirket içi ağınızla ve İnternet’le iletişim kurmak için Azure kaynaklarına IP adresleri atayabilirsiniz. Azure'da kullanabileceğiniz iki tür IP adresi vardır:

* **Genel IP adresleri**: Genel kullanıma yönelik Azure hizmetleri dahil olmak üzere Internet ile iletişim için kullanılır.
* **Özel IP adresleri**: Bir Azure sanal ağı (VNet) içinde ve şirket içi ağınızı Azure'a genişletmek için bir VPN ağ geçidi veya ExpressRoute bağlantı hattı kullanıyorsanız şirket içi ağınız içinde iletişim kurmak için kullanılır.

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  Bu makale, Microsoft’un çoğu yeni dağıtım için [klasik dağıtım modeli](virtual-network-ip-addresses-overview-classic.md) yerine önerdiği Resource Manager dağıtım modelini açıklamaktadır.
> 

Klasik dağıtım modeliyle ilgili bilginiz varsa [klasik ve Resource Manager IP adreslemesi arasındaki farkları](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) inceleyin.

## <a name="public-ip-addresses"></a>Genel IP adresleri

Genel IP adresleri, Azure kaynaklarının İnternet ile ve [Azure Redis Cache](https://azure.microsoft.com/services/cache), [Azure Olay Hub’ları](https://azure.microsoft.com/services/event-hubs), [SQL veritabanları](../sql-database/sql-database-technical-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Azure depolama alanı](../storage/common/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gibi genel kullanıma yönelik Azure hizmetleriyle iletişim kurmasını sağlar.

Azure Resource Manager’daki bir [genel IP](virtual-network-public-ip-address.md) adresi, kendi özelliklerine sahip olan bir kaynaktır. Genel bir IP adresini ilişkilendirebileceğiniz kaynakların bazıları şunlardır:

* Sanal makine ağ arabirimleri
* İnternet'e yönelik yük dengeleyiciler
* VPN ağ geçitleri
* Uygulama ağ geçitleri

### <a name="ip-address-version"></a>IP adresi sürümü

Genel IP adresleri IPv4 veya IPv6 adresiyle oluşturulur. Genel IPv6 adresleri yalnızca İnternet'e yönelik yük dengeleyicilere atanabilir.

### <a name="sku"></a>SKU

Genel IP adresleri aşağıdaki SKU'lardan biriyle oluşturulur:

#### <a name="basic"></a>Temel

SKU'ların kullanıma sunulmasından önce oluşturulan tüm genel IP adresleri Temel SKU genel IP adresleridir. SKU'ların kullanıma sunulması genel IP adresinin ait olmasını istediğiniz SKU'yu belirleme seçeneğini sunmuştur. Temel SKU adresleri:

- Statik veya dinamik atama yöntemiyle atanmıştır.
- Ağ arabirimleri, VPN ağ geçitleri, uygulama ağ geçitleri ve İnternet'e yönelik yük dengeleyiciler gibi genel IP adresi atanabilecek bir Azure kaynağına atanmıştır.
- Belirli bir bölgeye atanabilir.
- Bölgesel olarak yedekli değildir. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

#### <a name="standard"></a>Standart

Standart SKU genel IP adresleri:

- Yalnızca statik ayırma yöntemiyle atanmıştır.
- Ağ arabirimlerine veya Standart İnternet'e yönelik yük dengeleyicilere atanmıştır. Azure yük dengeleyici SKU'ları hakkında daha fazla bilgi edinmek için bkz. [Azure yük dengeleyici standart SKU'su](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Varsayılan ayarda bölgesel olarak yedeklidir. Belirli bir alanda oluşturulabilir ve belirli bir kullanılabilirlik alanında oluşturulma garantisi vardır.  Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
 
> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir.  Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

Standart SKU önizleme sürümündedir. Standart SKU genel IP adresi oluşturmadan önce önizlemeye kaydolmanız ve adresi desteklenen bir konumda oluşturmanız gerekir. Önizlemeye kaydolmak için bkz. [Standart SKU önizlemesine kaydolma](virtual-network-public-ip-address.md#register-for-the-standard-sku-preview). Desteklenen konumların (bölgelerin) listesi için [Bölge kullanılabilirliği](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) sayfasını, ek bölgesel destek için de [Azure Sanal Ağı güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasını inceleyin.


### <a name="allocation-method"></a>Ayırma yöntemi

Bir IP adresinin genel bir IP adresi kaynağına ayrıldığı iki yöntem vardır: *Dinamik* veya *statik*. Varsayılan ayırma yöntemi, bir IP adresinin oluşturulduğu sırada **ayrılmadığı** yöntem olan *dinamik*tir. Bunun yerine, genel IP adresi ilişkili kaynağı (VM veya yük dengeleyici gibi) başlattığınızda (veya oluşturduğunuzda) ayrılır. Kaynağı durdurduğunuzda (veya sildiğinizde) IP adresi serbest kalır. Örneğin IP adres kaynak A tarafından serbest bırakıldıktan sonra farklı bir kaynağa atanabilir. Kaynak A durdurulmuş haldeyken IP adresi farklı bir kaynağa atanmışsa kaynak A yeniden başlatıldığında farklı bir IP adresi atanır.

İlişkili kaynağın IP adresinin aynı kalmasını sağlamak için ayırma yöntemini açıkça *statik* olarak ayarlayabilirsiniz. Statik IP adresi anında atanır. Adres, yalnızca kaynağı sildiğinizde veya ayırma yöntemini *dinamik* olarak değiştirdiğinizde serbest kalır.

> [!NOTE]
> Ayırma yöntemini *statik* olarak ayarladığınızda bile genel IP adresi kaynağına atanan gerçek IP adresini belirtemezsiniz. Azure IP adresini kaynağın oluşturulduğu Azure konumundaki kullanılabilir IP adreslerinden oluşan bir havuzdan atar.
>

Statik genel IP adresleri yaygın olarak aşağıdaki senaryolarda kullanılır:

* Azure kaynaklarınızla iletişim kurabilmesi için güvenlik duvarı ayarlarını güncelleştirmeniz gerektiğinde.
* IP adresindeki bir değişikliğin A kayıtlarının güncelleştirilmesini gerektireceği DNS ad çözümlemesi.
* Azure kaynaklarınız, IP adresi tabanlı bir güvenlik yöntemi kullanan diğer uygulama ve hizmetlerle iletişim kurar.
* Bir IP adresine bağlı SSL sertifikalarınız.

> [!NOTE]
> Azure genel IP adreslerini her bir Azure bölgesine atanmış olan benzersiz aralıktan atar. Ayrıntılar için bkz. [Azure Veri Merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>DNS ana bilgisayar adı çözümlemesi
Bir genel IP kaynağı için DNS etki alanı ad etiketi belirtebilirsiniz; bu durumda, Azure tarafından yönetilen DNS sunucularında genel IP adresine yönelik olarak *etkialanıadetiketi*.*konum*.cloudapp.azure.com için bir eşleme oluşturulur. Örneğin, **Batı ABD** Azure *konumunda* *etkialanıadetiketi* olarak **contoso** değerini içeren bir genel IP kaynağı oluşturursanız, **contoso.westus.cloudapp.azure.com** şeklindeki tam etki alanı adı (FQDN), kaynağın genel IP adresi olarak çözümlenir. Bu FQDN'yi kullanarak Azure'daki genel IP adresini işaret eden özel bir etki alanı CNAME kaydı oluşturabilirsiniz.

> [!IMPORTANT]
> Oluşturulan her bir etki alanı ad etiketi kendi Azure konumunda benzersiz olmalıdır.  
>

### <a name="virtual-machines"></a>Sanal makineler

Genel bir IP adresini bir [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makinenin **ağ arabirimine** giderek ilişkilendirebilirsiniz. Bir sanal makineye dinamik veya statik bir genel IP adresi atayabilirsiniz. [IP adreslerini ağ arabirimlerine atama](virtual-network-network-interface-addresses.md) hakkında daha fazla bilgi edinin.

### <a name="internet-facing-load-balancers"></a>İnternet'e yönelik yük dengeleyiciler

Herhangi bir [SKU](#SKU) ile oluşturulmuş genel bir IP adresini bir [Azure Load Balancer](../load-balancer/load-balancer-overview.md)'ın **ön uç** yapılandırmasına atayarak yük dengeleyiciyle ilişkilendirebilirsiniz. Genel IP adresi yükü dengelenmiş bir sanal IP adresi (VIP) olarak işlev görür. Bir yük dengeleyici ön ucuna dinamik veya statik bir genel IP adresi atayabilirsiniz. Ayrıca, SSL tabanlı web sitelerinde çok kiracılı bir ortam gibi [çok VIP'li](../load-balancer/load-balancer-multivip-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) senaryoları mümkün kılmak için bir yük dengeleyici ön ucuna birden çok genel IP adresi de atayabilirsiniz. Azure yük dengeleyici SKU'ları hakkında daha fazla bilgi edinmek için bkz. [Azure yük dengeleyici standart SKU'su](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="vpn-gateways"></a>VPN ağ geçitleri

[Azure VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json), bir Azure sanal ağını diğer Azure sanal ağlarına veya bir şirket içi ağa bağlar. VPN Ağ Geçidinin uzak ağ ile iletişim kurmasını sağlamak için genel IP adresi atanır. VPN ağ geçitlerine yalnızca *dinamik* genel IP adresleri atayabilirsiniz.

### <a name="application-gateways"></a>Uygulama ağ geçitleri

Genel bir IP adresini bir Azure **Application Gateway**’in [ön uç](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırmasına atayarak ağ geçidiyle ilişkilendirebilirsiniz. Bu genel IP adresi yükü dengelenmiş bir VIP olarak işlev görür. Bir uygulama ağ geçidi ön uç yapılandırmasına yalnızca *dinamik* genel IP adresleri atayabilirsiniz.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda, genel bir IP adresinin en üst düzey bir kaynakla tam olarak hangi özellik üzerinden ilişkilendirilebileceği ve kullanılabilecek olası ayırma yöntemleri (dinamik veya statik) gösterilmiştir.

| En üst düzey kaynak | IP Adresi ilişkilendirme | Dinamik | Statik |
| --- | --- | --- | --- |
| Sanal makine |Ağ arabirimi |Evet |Evet |
| İnternet'e yönelik yük dengeleyici |Ön uç yapılandırması |Evet |Evet |
| VPN ağ geçidi |Ağ geçidi IP yapılandırması |Evet |Hayır |
| Uygulama ağ geçidi |Ön uç yapılandırması |Evet |Hayır |

## <a name="private-ip-addresses"></a>Özel IP adresleri
Özel IP adresleri, Azure kaynaklarının İnternet’ten erişilebilen bir IP adresi gerekmeksizin bir [sanal ağdaki](virtual-networks-overview.md) diğer kaynaklarla veya bir VPN ağ geçidi ya da ExpressRoute bağlantı hattı üzerinden bir şirket içi ağ ile iletişim kurmasına imkan tanır.

Azure Resource Manager dağıtım modelinde, özel IP adresleri aşağıdaki Azure kaynağı türleriyle ilişkilendirilir:

* Sanal makine ağ arabirimleri
* İç yük dengeleyiciler (ILB)
* Uygulama ağ geçitleri

### <a name="ip-address-version"></a>IP adresi sürümü

Özel IP adresleri IPv4 veya IPv6 adresiyle oluşturulur. Özel IPv6 adresleri yalnızca dinamik ayırma yöntemiyle atanabilir. Bir sanal ağ üzerindeki özel IPv6 adresleri arasında iletişim kuramazsınız. İnternet'e yönelik yük dengeleyici aracılığıyla İnternet'ten özel bir IPv6 adresiyle iletişim kurabilirsiniz. Ayrıntılar için [IPv6 ile İnternet’e yönelik yük dengeleyici oluşturma](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bölümüne bakın.

### <a name="allocation-method"></a>Ayırma yöntemi

Kaynağın dağıtıldığı sana alt ağın adres aralığından özel bir IP adresi ayrılır. Özel IP adresi ayırmak için kullanılan iki yöntem vardır:

- **Dinamik**: Azure her alt ağ adres aralığında ilk dört adresi ayırır ve bu adresleri atamaz. Azure, alt ağ adres aralığından sonraki kullanılabilir adresi bir kaynağa atar. Örneğin, alt ağ adres aralığının 10.0.0.0/16 olduğunu varsayarsak ve 10.0.0.0.4-10.0.0.14 adresleri zaten atanmışsa (.0-.3 ayrılmıştır), Azure kaynağa 10.0.0.15 adresini atar. Dinamik, varsayılan ayırma yöntemidir. Dinamik IP adresleri bir kez atandıktan sonra, ancak ağ arabirimi silinirse, aynı sanal ağ içinde farklı bir alt ağa atanırsa veya ayırma yöntemi statik olarak değiştirilip farklı bir IP adresi belirtilirse serbest bırakılır. Varsayılan olarak, dinamik olan ayırma yöntemini statik olarak değiştirdiğinizde Azure dinamik olarak atanmış önceki adresi statik adres olarak atar.
- **Statik**: Alt ağın adres aralığından siz bir adres seçe ve atarsınız. Alt ağ adres aralığından atadığınız adres, alt ağa adres aralığının ilk dört adresi dışında kalan ve şu anda alt ağda başka bir kaynağa atanmamış olan herhangi bir adres olabilir. Statik adresler ancak ağ arabirimi silindiğinde serbest bırakılır. Ayırma yöntemini dinamik olarak değiştirirseniz, Azure daha önce statik IP adresi olarak atanmış olan adresi dinamik adres olarak atar. Bu adres, alt ağ adres aralığında bir sonraki kullanılabilir adres olmayabilir. Ağ arabirimi aynı sanal ağ içinde farklı bir alt ağa atandığında da adres değişir; ama ağ arabirimini farklı bir alt ağa atamak için, önce statik ayırma yöntemini dinamik olarak değiştirmeniz gerekir. Ağ arabirimini farklı bir alt ağa atadıktan sonra, ayırma yöntemini yine statik yapabilir ve yeni alt ağ adres aralığından bir IP adresi atayabilirsiniz.

### <a name="virtual-machines"></a>Sanal makineler

[Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makinesinin bir veya birden çok **ağ arabirimine** bir veya birden çok özel IP adresi atanır. Her özel IP adresi için ayırma yöntemini dinamik veya statik olarak belirtebilirsiniz.

#### <a name="internal-dns-hostname-resolution-for-virtual-machines"></a>İç DNS ana bilgisayar adı çözümlemesi (sanal makineler için)

Siz açıkça özel DNS sunucuları yapılandırmadığınız sürece, tüm Azure sanal makineleri varsayılan olarak [Azure tarafından yönetilen DNS sunucuları](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) ile yapılandırılmıştır. Bu DNS sunucuları aynı sanal ağda bulunan sanal makineler için iç ad çözümleme sağlar.

Bir sanal makine oluşturduğunuzda, Azure tarafından yönetilen DNS sunucularına ana bilgisayar adı için özel IP adresine yönelik bir eşleme eklenir. Sanal makinenin birden çok ağ arabirimi veya ağ arabirimi için birden çok IP yapılandırması olması durumunda, ana bilgisayar adı birincil ağ arabirimine ilişkin birincil IP yapılandırmasının özel IP adresiyle eşlenir.

Azure tarafından yönetilen DNS sunucularıyla yapılandırılan sanal makineler kendi sanal ağındaki tüm sanal makinelerin ana bilgisayar adlarını bu sanal makinelerin özel IP adreslerine çözümleyebilir. Bağlı sanal ağlarda sanal makinelerin ana bilgisayar adlarını çözümlemek için, özel DNS sunucusu kullanmalısınız.

### <a name="internal-load-balancers-ilb--application-gateways"></a>İç yük dengeleyiciler (ILB) ve Uygulama ağ geçitleri

Bir [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)’ın (ILB) veya [Azure Application Gateway](../application-gateway/application-gateway-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)’in **ön uç** yapılandırmasına özel bir IP adresi atayabilirsiniz. Özel IP adresi, yalnızca kendi sanal ağı ve sanal ağa bağlı uzak ağlar tarafından erişilebilen bir iç uç nokta olarak çalışır. Ön uç yapılandırmasına dinamik veya statik bir özel IP adresi atayabilirsiniz.

### <a name="at-a-glance"></a>Bir bakışta
Aşağıdaki tabloda, özel bir IP adresinin en üst düzey bir kaynakla tam olarak hangi özellik üzerinden ilişkilendirilebileceği ve kullanılabilecek olası ayırma yöntemleri (dinamik veya statik) gösterilmiştir.

| En üst düzey kaynak | IP adresi ilişkilendirme | Dinamik | Statik |
| --- | --- | --- | --- |
| Sanal makine |Ağ arabirimi |Evet |Evet |
| Yük dengeleyici |Ön uç yapılandırması |Evet |Evet |
| Uygulama ağ geçidi |Ön uç yapılandırması |Evet |Evet |

## <a name="limits"></a>Sınırlar
IP adresleme için uygulanan limitler, Azure’daki tüm [ağ limitlerinin](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) belirtildiği dizide bulunabilir. Limitler bölge ve abonelik başınadır. [Destek ekibiyle iletişime geçerek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) varsayılan limitleri iş ihtiyaçlarınıza göre en üst düzeye çıkarabilirsiniz.

## <a name="pricing"></a>Fiyatlandırma
Genel IP adreslerinin nominal bir ücreti olabilir. Azure'da IP adresi fiyatlandırması hakkında daha fazla bilgi edinmek için [IP adresi fiyatlandırması](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını inceleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalını kullanarak statik genel IP’li VM dağıtma](virtual-network-deploy-static-pip-arm-portal.md)
* [Şablon kullanarak statik genel IP’li VM dağıtma](virtual-network-deploy-static-pip-arm-template.md)
* [Azure portalını kullanarak statik özel IP adresli VM dağıtma](virtual-networks-static-private-ip-arm-pportal.md)
