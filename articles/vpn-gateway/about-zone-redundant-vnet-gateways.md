---
title: Azure kullanılabilirlik alanları, bölgesel olarak yedekli sanal ağ geçitleri hakkında | Microsoft Docs
description: Kullanılabilirlik alanları VPN Gateway ve ExpressRoute ağ geçitleri hakkında bilgi edinin.
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 0ba818ef3c24d0e88e662adf87b22cc938fe5fab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60391081"
---
# <a name="about-zone-redundant-virtual-network-gateways-in-azure-availability-zones"></a>Azure kullanılabilirlik alanları, bölgesel olarak yedekli sanal ağ geçitleri hakkında

VPN ve ExpressRoute ağ geçitleri dağıtabilir [Azure kullanılabilirlik alanları](../availability-zones/az-overview.md). Bu seçenek, dayanıklılık, ölçeklenebilirlik ve yüksek kullanılabilirlik için sanal ağ geçitleri getirir. Ağ geçitleri Azure kullanılabilirlik alanları, fiziksel ve mantıksal olarak dağıtma, ağ geçitleri bir bölge içinde bölge düzeyinde hatalardan Azure'a, şirket içi ağ bağlantısını korurken ayırır.

### <a name="zrgw"></a>Bölgesel olarak yedekli ağ geçitleri

Sanal ağ geçitlerinizi kullanılabilirlik alanları genelinde otomatik olarak dağıtmak için bölgesel olarak yedekli sanal ağ geçitlerini de kullanabilirsiniz. Bölgesel olarak yedekli ağ geçitleri ile azure'da görev açısından kritik, ölçeklenebilir hizmetleriniz için erişim bölge dayanıklılığı yararlı.

<br>
<br>

![Bölgesel olarak yedekli ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Bölgesel ağ geçitleri

Belirli bir bölgedeki ağ geçidi dağıtmak için bölgesel ağ geçitlerini de kullanabilirsiniz. Bölgesel bir ağ geçidi dağıttığınızda, ağ geçidinin tüm örnekleri aynı kullanılabilirlik alanında dağıtılır.

<br>
<br>

![Bölgesel ağ geçitleri grafiği](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>Ağ Geçidi SKU'ları

Bölgesel olarak yedekli ve bölgesel ağ geçidi yeni ağ geçidi SKU'ları olarak kullanılabilir. Yeni sanal ağ geçidi SKU'ları Azure AZ bölgelerde ekledik. Bölgesel olarak yedekli ve bölgesel ağ geçitleri için belirli şeylerdir bu SKU, ExpressRoute ve VPN ağ geçidi için karşılık gelen mevcut SKU'lara benzerdir.

Yeni ağ geçidi SKU'ları şunlardır:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>Genel IP SKU'ları

Bölgesel olarak yedekli ağ geçitleri ve bölgesel ağ geçitlerini kullanan Azure genel IP kaynağı üzerinde *standart* SKU. Azure genel IP kaynağı yapılandırmasını dağıttığınız ağ geçidi bölge yedekli olup olmadığını belirler veya bölgesel. Bir genel IP kaynağı oluşturursanız, bir *temel* SKU, ağ geçidi herhangi bir bölge artıklığı olmaz ve ağ geçidi kaynakları bölgesel olacaktır.

### <a name="pipzrg"></a>Bölgesel olarak yedekli ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **standart** bölge belirtmeden genel IP SKU'su, davranışı farklı ağ geçidi bir VPN ağ geçidi veya ExpressRoute ağ geçidi olduğuna bağlı olarak. 

* Bir VPN ağ geçidi için iki ağ geçidi örnekleri herhangi 2 bölgesi yedeklilik sağlamak için bu üç bölgeler dışında dağıtılır. 
* İkiden fazla örnekleri olabileceği bir ExpressRoute ağ geçidi için ağ geçidi üç tüm bölgeler arasında yayılabilir.

### <a name="pipzg"></a>Bölgesel ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **standart** genel IP SKU'su ve bölgeyi (1, 2 veya 3) belirtin, tüm ağ geçidi örnekleri aynı bölgede dağıtılır.

### <a name="piprg"></a>Bölgesel ağ geçitleri

Genel bir IP adresi kullanarak oluşturduğunuzda **temel** genel IP SKU'su, ağ geçidi bölgesel bir ağ geçidi olarak dağıtılır ve ağ geçidinde yerleşik bölge artıklığı sahip değil.

## <a name="faq"></a>SSS

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Bu yeni SKU'lara dağıtabilirim olduğunda ne değişecek mi?

Açısından bakıldığında, ağ geçitlerinizi bölge yedekliliği ile dağıtabilirsiniz. Bu, ağ geçitleri tüm örneklerini Azure kullanılabilirlik alanları genelinde dağıtılacak ve her kullanılabilirlik alanı farklı hata ve güncelleme etki alanı olduğu anlamına gelir. Bu ağ geçitlerinizi daha güvenilir, kullanılabilir ve dayanıklı bölge hatalarını sağlar.

### <a name="can-i-use-the-azure-portal"></a>Azure portalını kullanabilir miyim?

Evet, yeni SKU'lara dağıtmak için Azure portalını kullanabilirsiniz. Ancak, bu yeni SKU'lara Azure kullanılabilirlik alanları olan bu Azure bölgelerindeki görürsünüz.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>Hangi bölgeler benim için yeni SKU'lara kullanmak kullanılabilir mi?

Yeni SKU'lara Azure kullanılabilirlik alanları - Orta ABD, Fransa Orta, Kuzey Avrupa, Batı Avrupa ve Batı ABD 2 bölgelerinde sahip Azure bölgelerinde kullanılabilir. Bundan sonra bölgesel olarak yedekli ağ geçitleri kullanılabilir, diğer Azure ortak bölgelerde bulunacağız.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kullanabilir miyim Değiştir/geçiş/yükseltmesi mevcut sanal ağ geçitlerim bölgesel olarak yedekli ya da bölgesel ağ geçitleri?

Bölgesel olarak yedekli ya da bölgesel ağ geçitleri için var olan sanal ağ geçitlerinizi geçişi şu anda desteklenmiyor. Bununla birlikte, mevcut ağ geçidinizi silin ve bölgesel olarak yedekli ya da bölgesel bir ağ geçidi yeniden oluşturun.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Ben, aynı sanal ağdaki VPN hem Expressroute ağ geçitleri dağıtabilir miyim?

Aynı sanal ağda hem VPN hem de Express Route ağ geçitlerinin aynı anda var olmalarına desteklenir. Bununla birlikte, / 27 ayırmalısınız. ağ geçidi alt ağı için IP adresi aralığı.

## <a name="next-steps"></a>Sonraki Adımlar

[Alanlar arası yedekli sanal ağ geçidi oluşturma](create-zone-redundant-vnet-gateway.md)
