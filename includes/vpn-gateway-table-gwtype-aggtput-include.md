---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8fb9e9ea0e126509697b4874bf1e5e0b6a380e7f
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425772"
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br> SSTP Bağlantıları** | **P2S<br> IKEv2/OpenVPN Connections** | **Toplam<br>Aktarım Hızı Kıyaslaması** | **BGP** | **Bölgesel olarak yedekli** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Temel** | En çok, 10    | En çok, 128  | Desteklenmiyor  | 100 Mbps  | Desteklenmiyor| Hayır |
|**VpnGw1**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Desteklenen | Hayır |
|**VpnGw2**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Desteklenen | Hayır |
|**VpnGw3**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Desteklenen | Hayır |
|**VpnGw1AZ**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Desteklenen | Evet |
|**VpnGw2AZ**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Desteklenen | Evet |
|**VpnGw3AZ**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Desteklenen | Evet |


(*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. Bir VPN Gateway’e yönelik Toplam Aktarım Hızı Karşılaştırması S2S ve P2S’nin birleşimidir. **Çok sayıda P2S bağlantısına sahipseniz S2S bağlantınız aktarım hızı sınırlamalarından dolayı olumsuz etkilenebilir.** Toplam aktarım hızı kıyaslaması Internet trafiğinin koşulları ve uygulamanızın davranışı nedeniyle garantili bir verimlilik değildir.

* Bu bağlantı sınırları ayrıdır. Örneğin, bir VpnGw1 SKU’sunda 128 SSTP bağlantısına ek olarak 250 IKEv2 bağlantısına sahip olabilirsiniz.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* VPN ağ geçitleri için VpnGw1, VpnGw2 ve VpnGw3 yalnızca Resource Manager dağıtım modeli kullanıldığında desteklenir.

* Temel SKU eski SKU olarak kabul edilir. Temel SKU, belirli özellik sınırlamaları vardır. Yeni ağ geçidi SKU'ları birine bir temel SKU kullanan bir ağ geçidini yeniden boyutlandırma, bunun yerine VPN ağ geçidiniz geçemezsiniz içeren yeni bir SKU'ya için değiştirmeniz gerekir. Temel SKU kullanmadan önce gereksinim duyduğunuz özellik desteklendiğinden emin olun.
