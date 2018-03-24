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
ms.openlocfilehash: c9457e51858d4a073d8baffdd435c8100d95d566
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br>Bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| En çok, 30                         | En çok, 128               | 650 Mbps                    |
|**VpnGw2**| En çok, 30                         | En çok, 128               | 1 Gbps                      |
|**VpnGw3**| En çok, 30                         | En çok, 128               | 1,25 Gb/sn                   |
|**Temel** | En çok, 10                         | En çok, 128               | 100 Mbps                    | 
|          |                                 |                        |                             | 

- Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. İnternet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle bu aktarım hızı kesin değildir.

- Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

- SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

- VpnGw1, VpnGw2 ve VpnGw3 yalnızca Resource Manager dağıtım modelini kullanarak VPN ağ geçitleri için desteklenir.