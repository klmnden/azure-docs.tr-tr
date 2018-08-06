---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cbbb64489acf23c1248e35269e1441dd2a6878e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513768"
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br>Bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| En çok, 30*                         | En çok, 128\*\*             | 650 Mbps                    |
|**VpnGw2**| En çok, 30*                         | En çok, 128\*\*             | 1 Gbps                      |
|**VpnGw3**| En çok, 30*                         | En çok, 128\*\*             | 1,25 Gb/sn                   |
|**Temel** | En çok, 10                         | En çok, 128               | 100 Mbps                    | 

* (*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* (**) Ek bağlantılar gerekirse desteğe başvurun. Bu yalnızca IKEv2 için geçerlidir, SSTP için bağlantısı sayısı artırılamaz.

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. İnternet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle bu aktarım hızı kesin değildir.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* VPN ağ geçitleri için VpnGw1, VpnGw2 ve VpnGw3 yalnızca Resource Manager dağıtım modeli kullanıldığında desteklenir.