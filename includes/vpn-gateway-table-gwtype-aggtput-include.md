---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b698dd03473dd3cb708c47c6554869eebba48bf9
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597666"
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br> SSTP Bağlantıları** | **P2S<br> IKEv2 Bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Temel** | En çok, 10    | En çok, 128  | Desteklenmiyor  | 100 Mbps  | Desteklenmiyor|
|**VpnGw1**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Destekleniyor |
|**VpnGw2**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Destekleniyor |
|**VpnGw3**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Destekleniyor |


(*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* Bir VPN Gateway’e yönelik Toplam Aktarım Hızı Karşılaştırması S2S ve P2S’nin birleşimidir. **Çok sayıda P2S bağlantısına sahipseniz S2S bağlantınız aktarım hızı sınırlamalarından dolayı olumsuz etkilenebilir.** Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. İnternet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle bu aktarım hızı kesin değildir.

* Bu bağlantı sınırları ayrıdır. Örneğin, bir VpnGw1 SKU’sunda 128 SSTP bağlantısına ek olarak 250 IKEv2 bağlantısına sahip olabilirsiniz.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* VPN ağ geçitleri için VpnGw1, VpnGw2 ve VpnGw3 yalnızca Resource Manager dağıtım modeli kullanıldığında desteklenir.
