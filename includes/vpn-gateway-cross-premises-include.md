---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/16/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8c82e3d2c978a7980d02bc686346acdcb21dd591
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425775"
---
|  | **Noktadan Siteye** | **Siteden Siteye** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Desteklenen Azure Hizmetleri** |Cloud Services ve Virtual Machines |Cloud Services ve Virtual Machines |[Hizmetler listesi](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Tipik Bant Genişlikleri** |Ağ geçici SKU’sunu temel alanlar: |Tipik olarak < 1 Gbps toplu |50 Mb/sn, 100 Mb/sn, 200 Mb/sn, 500 Mb/sn, 1 Gb/sn, 2 Gb/sn, 5 Gb/sn, 10 Gb/sn |
| **Desteklenen Protokoller** |Güvenli Yuva Protokolü (SSTP), OpenVPN ve IPSec tüneli |IPsec |VLAN, NSP'nin VPN’si teknolojileri (MPLS, VPLS,...) üzerinden doğrudan bağlantı |
| **Yönlendirme** |RouteBased (dinamik) |PolicyBased (statik yönlendirme) ve RouteBased’i (dinamik yönlendirme VPN) destekliyoruz |BGP |
| **Bağlantı dayanıklılığı** |Etkin-Edilgen |aktif-pasif veya aktif-aktif |etkin-edilgen |
| **Tipik kullanım örneği** |Prototip oluşturma, Cloud Services ve Virtual Machines için geliştirme / test / laboratuvar senaryoları |Cloud Services ve Virtual Machines için geliştirme / test / laboratuvar senaryoları ve küçük ölçekli üretim iş yükleri |Tüm Azure hizmetlerine erişim (doğrulanmış liste), Kurumsal sınıfta ve görev açısından kritik iş yükleri, Yedekleme, Büyük Veri, DR sitesi olarak Azure |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Fiyatlandırma** |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Fiyatlandırma](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Teknik Belgeler** |[VPN Gateway Belgeleri](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway Belgeleri](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute Belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) |
| **SSS** |[VPN Gateway ile ilgili SSS](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN Gateway ile ilgili SSS](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute ile ilgili SSS](../articles/expressroute/expressroute-faqs.md) |
