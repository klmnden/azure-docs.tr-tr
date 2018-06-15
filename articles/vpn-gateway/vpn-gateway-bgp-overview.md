---
title: Azure VPN Gateways ile BGP'ye genel bakış | Microsoft Docs
description: Bu makale Azure VPN Gateways ile BGP’ye genel bakış sağlar.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: ''
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: 13a17eb3d78e70a09864bf218f1027d6e98486a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23883512"
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Azure VPN Gateways ile BGP’ye genel bakış
Bu makale Azure VPN Gateways içindeki BGP (Sınır Ağ Geçidi Protokolü) desteğine genel bakış sağlar.

BGP iki veya daha fazla ağ arasında yönlendirme ve ulaşılabilirlik bilgilerini takas etmek üzere İnternet’te yaygın olarak kullanılan standart yönlendirme protokolüdür. BGP, Azure Virtual Networks bağlamında kullanıldığında her iki ağ geçidini ön eklerin ilgili ağ geçitlerinden veya yönlendiricilerden geçmeye yönelik kullanılabilirliği ve ulaşılabilirliği konusunda bilgilendiren “yolları” değiştirmek amacıyla, Azure VPN Gateways’i ve BGP eşlikleri veya komşuları olarak adlandırılan şirket içi VPN cihazlarınızı etkinleştirir. BGP ayrıca bir BGP ağ geçidinin öğrendiği yolları bir BGP eşliğinden diğer tüm BGP eşliklerine yayarak birden fazla ağ arasında geçiş yönlendirmesi sağlayabilir. 

## <a name="why"></a>BGP neden kullanılır?
BGP, Azure Yol Tabanlı VPN ağ geçitleri ile birlikte kullanabileceğiniz isteğe bağlı bir özelliktir. Özelliği etkinleştirmeden önce şirket içi VPN cihazlarınızın da BGP’yi desteklediğinden emin olmanız gerekir. Azure VPN ağ geçitlerini ve şirket içi VPN cihazlarınızı BGP olmadan kullanmaya devam edebilirsiniz. BGP ile ağlarınız ve Azure arasında statik yollar kullanmaya (BGP olmadan) *karşılık* BGP ile dinamik yönlendirme kullanmanın eşdeğeridir.

BGP’nin çeşitli avantajları ve yeni özellikleri vardır:

### <a name="prefix"></a>Otomatik ve esnek ön ek güncelleştirmelerini destekler
BGP ile yalnızca belirli bir BGP eşliğine IPSec S2S VPN tüneli üzerinden en küçük ön eki bildirmeniz gerekir. Şirket içi VPN cihazınızın BGP eşliği IP adresinin ana bilgisayar ön eki (/32) kadar küçük olabilir. Azure Virtual Network’un erişmesine izin vermek üzere hangi şirket içi ağ ön eklerinin Azure’a tanıtılacağını denetleyebilirsiniz.

Ayrıca, büyük özel bir IP adres alanı (örneğin, 10.0.0.0/8) gibi VNet adres ön bazıları içerebilir büyük ön ekler tanıtabilirsiniz. Ön ekler VNet ön herhangi biri ile aynı olamaz olsa unutmayın. VNet ön eklerinizle aynı olan yollar reddedilir.

### <a name="multitunnel"></a>Birden fazla tünel VNet ve bir şirket içi site arasında BGP'yi temel alan otomatik yük devretme desteği
Azure VNet ile şirket içi VPN cihazlarınız arasında aynı konumda birden fazla bağlantı kurabilirsiniz. Bu özellik aktif-aktif bir yapılandırmadaki iki ağ arasında birden fazla tünel (yol) sağlar. Tünellerden birinin bağlantısı kesilirse karşılık gelen yollar BGP aracılığıyla geri çekilir ve trafik kalan tünellere otomatik olarak geçer.

Aşağıdaki diyagramda yüksek oranda kullanılabilen bu kurulumun basit bir örneği gösterilmiştir:

![Birden fazla etkin yol](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="transitrouting"></a>Şirket içi ağlarınız ile birden fazla Azure Vnet arasında geçiş yönlendirme desteği
BGP, doğrudan veya dolaylı olarak bağlı olmasına bakılmaksızın birden fazla ağ geçidinin farklı ağlardaki ön ekleri öğrenmesini ve yaymasını sağlar. Bu özellik şirket içi siteleriniz arasında veya birden fazla Azure Sanal Ağında Azure VPN ağ geçitleri ile geçiş yönlendirmesi sağlayabilir.

Aşağıdaki diyagramda Microsoft Networks içindeki Azure VPN ağ geçitleri üzerinden iki şirket içi ağ arasındaki trafiği geçebilen birden fazla yola sahip çoklu atlamalı bir topolojinin örneği gösterilmektedir:

![Çoklu atlamalı geçiş](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="faq"></a>BGP SSS
[!INCLUDE [vpn-gateway-faq-bgp-include](../../includes/vpn-gateway-faq-bgp-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
BGP’yi şirketler arası ve VNet’ten VNet’te bağlantılar için yapılandırma adımları için bkz. [Azure VPN ağ geçitlerinde BGP kullanmaya başlama](vpn-gateway-bgp-resource-manager-ps.md).

