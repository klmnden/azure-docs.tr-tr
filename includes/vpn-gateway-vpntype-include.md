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
ms.openlocfilehash: 5de227e5de5ef9b41f6e0f64db86b7195259f7d6
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
* **PolicyBased:** PolicyBased Vpn'lere statik yönlendirme ağ geçitleri Klasik dağıtım modelinde adı daha önce. İlke temelli VPN'ler, şifrelemek ve şirket içi ağınızla Azure Vnet'iniz arasında adres öneklerinin birleşimleriyle yapılandırılmış IPSec ilkeleri temelindeki IPSec tüneller üzerinden paketleri doğrudan. İlke (veya trafik seçici) çoğunlukla VPN cihazı yapılandırmasında bir erişim listesi olarak tanımlanır. PolicyBased VPN türüyle ilgili değer *PolicyBased*. PolicyBased VPN kullanırken aşağıdaki sınırlamalar göz önünde bulundurun:
  
  * PolicyBased VPN can **yalnızca** temel ağ geçidi SKU üzerinde kullanılabilir. Bu VPN türü diğer ağ geçidi SKU'ları ile uyumlu değil.
  * PolicyBased VPN kullanırken, yalnızca 1 tünel olabilir.
  * PolicyBased VPN S2S bağlantılarının ve yapılandırmaları yalnızca belirli yalnızca kullanabilirsiniz. Çoğu VPN ağ geçidi yapılandırmaları, RouteBased VPN gerektirir.
* **RouteBased**: RouteBased VPN daha önce adı dinamik yönlendirme ağ geçitleri Klasik dağıtım modelinde. RouteBased VPN IP iletme veya yönlendirme tablosunu paketleri kendi ilgili arabirimlerine yönlendirmek "yolları" kullanın. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. İlke (veya trafik Seçici) RouteBased VPN için yapılandırılmış olan herhangi herhangi olarak (veya joker karakterler). RouteBased VPN türüyle ilgili değer *RouteBased*.
