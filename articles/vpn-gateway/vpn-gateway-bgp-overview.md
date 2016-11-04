---
title: Azure VPN Gateways ile BGP’ye Genel Bakış | Microsoft Docs
description: Bu makale Azure VPN Gateways ile BGP’ye genel bakış sağlar.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: ''

ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/16/2016
ms.author: yushwang

---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Azure VPN Gateways ile BGP’ye genel bakış
Bu makale Azure VPN Gateways içindeki BGP (Sınır Ağ Geçidi Protokolü) desteğine genel bakış sağlar.

## <a name="about-bgp"></a>BGP hakkında
BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, Azure Virtual Networks bağlamında kullanıldığında her iki ağ geçidini ön eklerin ilgili ağ geçitlerinden veya yönlendiricilerden geçmeye yönelik kullanılabilirliği ve ulaşılabilirliği konusunda bilgilendiren “yolları” değiştirmek amacıyla, Azure VPN Gateways’i ve BGP eşlikleri veya komşuları olarak adlandırılan şirket içi VPN cihazlarınızı etkinleştirir. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir.

### <a name="why-use-bgp?"></a>BGP neden kullanılır?
BGP, Azure Yol Tabanlı VPN ağ geçitleri ile birlikte kullanabileceğiniz isteğe bağlı bir özelliktir. Özelliği etkinleştirmeden önce şirket içi VPN cihazlarınızın da BGP’yi desteklediğinden emin olmanız gerekir. Azure VPN ağ geçitlerini ve şirket içi VPN cihazlarınızı BGP olmadan kullanmaya devam edebilirsiniz. BGP ile ağlarınız ve Azure arasında statik yollar kullanmaya (BGP olmadan) *karşılık* BGP ile dinamik yönlendirme kullanmanın eşdeğeridir.

BGP’nin çeşitli avantajları ve yeni özellikleri vardır:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Otomatik ve esnek ön ek güncelleştirmelerini destekler
BGP ile yalnızca belirli bir BGP eşliğine IPSec S2S VPN tüneli üzerinden en küçük ön eki bildirmeniz gerekir. Şirket içi VPN cihazınızın BGP eşliği IP adresinin ana bilgisayar ön eki (/32) kadar küçük olabilir. Azure Virtual Network’un erişmesine izin vermek üzere hangi şirket içi ağ ön eklerinin Azure’a tanıtılacağını denetleyebilirsiniz.

Ayrıca büyük bir özel IP adres alanı (örn. 10.0.0.0/8) gibi VNet adres ön eklerinden bazılarını içerebilecek büyük ön ekler tanıtabilirsiniz. Ancak, ön ekler VNet ön eklerinizden herhangi biri ile aynı olamaz. VNet ön eklerinizle aynı olan yollar reddedilir.

> [!IMPORTANT]
> Şu anda varsayılan yolun (0.0.0.0/0) Azure VPN ağ geçitlerinde tanıtılması engellenecektir. Bu özellik etkinleştirildiğinde daha fazla güncelleştirme sağlanacaktır.
> 
> 

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Bir VNet ile şirket içi site arasında BGP’yi temel alan otomatik yük devretme ile birden fazla tüneli destekler
Azure VNet ile şirket içi VPN cihazlarınız arasında aynı konumda birden fazla bağlantı kurabilirsiniz. Bu özellik aktif-aktif bir yapılandırmadaki iki ağ arasında birden fazla tünel (yol) sağlar. Tünellerden birinin bağlantısı kesilirse karşılık gelen yollar BGP aracılığıyla geri çekilir ve trafik kalan tünellere otomatik olarak kayar.

Aşağıdaki diyagramda yüksek oranda kullanılabilen bu kurulumun basit bir örneği gösterilmiştir:

![Birden fazla etkin yol](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Şirket içi ağlarınız ile birden fazla Azure VNet arasında geçiş yönlendirme desteği
BGP, doğrudan veya dolaylı olarak bağlı olmasına bakılmaksızın birden fazla ağ geçidinin farklı ağlardaki ön ekleri öğrenmesini ve yaymasını sağlar. Bu özellik şirket içi siteleriniz arasında veya birden fazla Azure Sanal Ağında Azure VPN ağ geçitleri ile geçiş yönlendirmesi sağlayabilir.

Aşağıdaki diyagramda Microsoft Networks içindeki Azure VPN ağ geçitleri üzerinden iki şirket içi ağ arasındaki trafiği geçebilen birden fazla yola sahip çoklu atlamalı bir topolojinin örneği gösterilmektedir:

![Çoklu atlamalı geçiş](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP SSS
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
BGP’yi şirketler arası ve VNet’ten VNet’te bağlantılar için yapılandırma adımları için bkz. [Azure VPN ağ geçitlerinde BGP kullanmaya başlama](vpn-gateway-bgp-resource-manager-ps.md).

<!--HONumber=Oct16_HO3-->


