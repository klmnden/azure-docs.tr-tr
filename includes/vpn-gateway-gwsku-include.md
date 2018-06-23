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
ms.openlocfilehash: 05dc8ae48a9164e4f7118d378ab0eb7c30a4249e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "30196856"
---
Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU’ları seçin.

###  <a name="benchmark"></a>Tünele, bağlantıya ve performansa göre Ağ Geçidi SKU’ları

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>Özellik kümesi tarafından ağ geçidi SKU'ları

Yeni VPN ağ geçidi SKU'ları üzerindeki ağ geçitlerini sunulan özellik kümeleri kolaylaştırır:

| **SKU**| **Özellikler**|
| ---    | ---         |
|**Temel** (*)   | **Rota tabanlı VPN**: P2S 10 tünelleri; P2S için; RADIUS kimlik doğrulaması P2S için hiçbir Ikev2<br>**İlke tabanlı VPN** (IKEv1): 1 tünel; P2S yok|
| **VpnGw1, VpnGw2 ve VpnGw3** | **Rota tabanlı VPN**: 30 tünelleri (*), P2S, BGP, etkin-etkin, özel IPSec/IKE İlkesi, ExpressRoute/VPN birlikte kullanımı |
|        |             |

( * ) Rota tabanlı bir VPN ağ geçidini (VpnGw1, VpnGw2, VpnGw3) şirket içi ilke tabanlı birden fazla güvenlik duvarı cihazına bağlamak için "PolicyBasedTrafficSelectors" yapılandırması gerçekleştirebilirsiniz. Ayrıntılı bilgi için bkz. [PowerShell kullanarak VPN ağ geçitlerini şirket içi ilke tabanlı birden fazla VPN cihazına bağlama](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

(**) Temel SKU eski SKU olarak kabul edilir. Temel SKU belirli özelliği sınırlamaları vardır. Yeni ağ geçidi SKU'ları birine temel SKU kullanan bir ağ geçidini yeniden boyutlandırmak, bunun yerine, silme ve VPN ağ geçidinizi yeniden içerir bir yeni SKU değiştirmeniz gerekir.

###  <a name="workloads"></a>Ağ geçidi SKU'ları - üretim vs. Geliştirme-Test İş Yükleri Karşılaştırması

SLA ve özellik kümeleri farklılıkları nedeniyle, geliştirme, test ve üretim için aşağıdaki SKU'ları öneririz:

| **İş yükü**                       | **SKU'lar**               |
| ---                                | ---                    |
| **Üretim, kritik iş yükleri** | VpnGw1, VpnGw2, VpnGw3 |
| **Geliştirme-test veya kavram kanıtı**   | Temel (*)                 |
|                                    |                        |

(**) Temel SKU eski SKU kabul edilir ve özellik kısıtlamaları vardır. Temel SKU kullanmadan önce gereksinim duyduğunuz özelliği desteklendiğinden emin olun.

Eski SKU'ları (eski) kullanıyorsanız, üretim SKU standart ve HighPerformance önerilerdir. Bilgi ve eski SKU'ları için yönergeler için bkz: [ağ geçidi SKU'ları (eski)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).