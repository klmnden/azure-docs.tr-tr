---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/20/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b1a9d93d9fccf02ba1517e429625150736e539e9
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159321"
---
Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU’ları seçin. Sanal ağ geçidi SKU'ları Azure kullanılabilirlik alanları için bkz: [Azure kullanılabilirlik alanları ağ geçidi SKU'ları](../articles/vpn-gateway/about-zone-redundant-vnet-gateways.md).

###  <a name="benchmark"></a>Tünele, bağlantıya ve performansa göre Ağ Geçidi SKU’ları

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Özellik kümesi tarafından ağ geçidi SKU'ları

Yeni VPN ağ geçidi SKU'ları ağ geçitlerinde sunulan özellik kümeleri açısından kolaylık sağlar:

| **SKU**| **Özellikler**|
| ---    | ---         |
|**Temel** (\*\*)   | **Rota tabanlı VPN**: S2S/bağlantılar için 10 tünel; RADIUS kimlik doğrulaması olmaması için P2S; P2S için Ikev2 yok<br>**İlke tabanlı VPN**: (IKEv1): 1 S2S/bağlantı tünel; P2S yok|
| **VpnGw1, VpnGw2 ve VpnGw3** | **Rota tabanlı VPN**: 30 tünele kadar (*), P2S, BGP, etkin-etkin, özel IPSec/IKE İlkesi, ExpressRoute/VPN birlikte kullanımı |
|        |             |

( * ) Rota tabanlı bir VPN ağ geçidini (VpnGw1, VpnGw2, VpnGw3) şirket içi ilke tabanlı birden fazla güvenlik duvarı cihazına bağlamak için "PolicyBasedTrafficSelectors" yapılandırması gerçekleştirebilirsiniz. Ayrıntılı bilgi için bkz. [PowerShell kullanarak VPN ağ geçitlerini şirket içi ilke tabanlı birden fazla VPN cihazına bağlama](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

(\*\*) Temel SKU eski SKU olarak kabul edilir. Temel SKU, belirli özellik sınırlamaları vardır. Yeni ağ geçidi SKU'ları birine bir temel SKU kullanan bir ağ geçidini yeniden boyutlandırma, bunun yerine VPN ağ geçidiniz geçemezsiniz içeren yeni bir SKU'ya için değiştirmeniz gerekir.

###  <a name="workloads"></a>Ağ geçidi SKU'ları - üretim vs. Geliştirme-Test İş Yükleri Karşılaştırması

SLA ve özellik kümeleri farklılıklar nedeniyle üretim ve geliştirme ve test için aşağıdaki SKU'ları öneririz:

| **İş yükü**                       | **SKU'lar**               |
| ---                                | ---                    |
| **Üretim, kritik iş yükleri** | VpnGw1, VpnGw2, VpnGw3 |
| **Geliştirme-test veya kavram kanıtı**   | Temel (\*\*)                 |
|                                    |                        |

(\*\*) Temel SKU eski SKU olarak kabul edilir ve özellik sınırlamaları vardır. Temel SKU kullanmadan önce gereksinim duyduğunuz özellik desteklendiğinden emin olun.

Eski SKU'ları (eski) kullanıyorsanız, üretim standart ve yüksek performanslı ' dir. Bilgi ve yönergeler için eski SKU'ları için bkz. [ağ geçidi SKU'ları (eski)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).
