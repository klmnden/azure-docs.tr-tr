---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 605533f25b36a92a660301d28aa63cb2ecdd44f4
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37910061"
---
Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU’ları seçin.

###  <a name="benchmark"></a>Tünele, bağlantıya ve performansa göre Ağ Geçidi SKU’ları

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Özellik kümesi tarafından ağ geçidi SKU'ları

Yeni VPN ağ geçidi SKU'ları ağ geçitlerinde sunulan özellik kümeleri açısından kolaylık sağlar:

| **SKU**| **Özellikler**|
| ---    | ---         |
|**Temel** (\*\*)   | **Rota tabanlı VPN**: P2S ile 10 tünel; P2S için; RADIUS kimlik doğrulaması P2S için Ikev2 yok<br>**İlke tabanlı VPN** (IKEv1): 1 tünel; P2S yok|
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
