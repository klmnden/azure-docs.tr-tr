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
ms.openlocfilehash: fa9c27457b1da4d233aaea2a6621af9f5d01149d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
|  | **Noktadan siteye** | **Siteden siteye** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Azure Hizmetleri desteklenmez** |Cloud Services ve Virtual Machines |Cloud Services ve Virtual Machines |[Hizmetler listesi](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Tipik bant genişlikleri** |Tipik olarak < 100 Mb/sn toplu |Genellikle < 1 GB/sn toplama |50 Mb/sn, 100 Mb/sn, 200 Mb/sn, 500 Mb/sn, 1 Gb/sn, 2 Gb/sn, 5 Gb/sn, 10 Gb/sn |
| **Desteklenen protokoller** |Güvenli Yuva Tünel Protokolü (SSTP) |IPsec |VLAN, NSP'nin VPN’si teknolojileri (MPLS, VPLS,...) üzerinden doğrudan bağlantı |
| **Yönlendirme** |RouteBased (dinamik) |PolicyBased (statik yönlendirme) ve RouteBased (dinamik yönlendirme VPN) destekliyoruz |BGP |
| **Bağlantı dayanıklılığı** |Etkin-Edilgen |Etkin-pasif veya aktif-aktif |etkin-edilgen |
| **Tipik kullanım örneği** |Prototip oluşturma, Cloud Services ve Virtual Machines için geliştirme / test / laboratuvar senaryoları |Cloud Services ve Virtual Machines için geliştirme / test / laboratuvar senaryoları ve küçük ölçekli üretim iş yükleri |Tüm Azure hizmetlerine erişim (doğrulanmış liste), Kurumsal sınıfta ve görev açısından kritik iş yükleri, Yedekleme, Büyük Veri, DR sitesi olarak Azure |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Fiyatlandırma** |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Teknik belgeler** |[VPN Gateway Belgeleri](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway Belgeleri](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute Belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) |
| **SSS** |[VPN Gateway ile ilgili SSS](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN Gateway ile ilgili SSS](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute ile ilgili SSS](../articles/expressroute/expressroute-faqs.md) |
