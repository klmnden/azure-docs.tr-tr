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
ms.openlocfilehash: b3907882df09bfae1d6453fbffbd3e7562554f7c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159303"
---
* **PolicyBased:** PolicyBased VPN'ler daha önce Klasik dağıtım modelinde statik yönlendirme ağ geçitleri adı veriliyordu. İlke tabanlı VPN'ler şifreler ve şirket içi ağınız ile Azure Vnet'iniz arasında adres öneklerinin birleşimleriyle yapılandırılmış IPSec ilkeleri temelindeki IPSec tüneller üzerinden paketleri doğrudan. İlke (veya trafik seçici) çoğunlukla VPN cihazı yapılandırmasında bir erişim listesi olarak tanımlanır. PolicyBased VPN türü için değer *PolicyBased*. PolicyBased VPN kullanırken aşağıdaki sınırlamaları göz önünde bulundurun:
  
  * PolicyBased VPN'ler can **yalnızca** temel ağ geçidi SKU üzerinde kullanılabilir. Bu VPN türü diğer ağ geçidi SKU'ları ile uyumlu değil.
  * PolicyBased VPN kullanırken, yalnızca 1 tünel olabilir.
  * S2S bağlantılarının ve yapılandırmalar için yalnızca belirli yalnızca PolicyBased VPN'ler kullanabilirsiniz. Çoğu VPN ağ geçidi yapılandırmaları, RouteBased VPN gerektirir.
* **RouteBased**: RouteBased Vpn'lere daha önce Klasik dağıtım modelinde dinamik yönlendirme ağ geçitleri adı veriliyordu. RouteBased VPN IP iletme veya yönlendirme tablosunu paketleri kendi ilgili arabirimlerine yönlendirmek "yolları" kullanın. Bundan sonra tünel arabirimleri, paketleri tünellerin içinde veya dışında şifreler veya şifrelerini çözer. İlke (veya trafik Seçici) RouteBased Vpn'lere için herhangi bir ağdan herhangi olarak yapılandırılır (veya joker karakterler). RouteBased VPN türü için değer *RouteBased*.
