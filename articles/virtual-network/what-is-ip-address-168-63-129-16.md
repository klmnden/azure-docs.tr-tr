---
title: 'IP adresi: 168.63.129.16 nedir? | Microsoft Docs'
description: 'IP adresi: 168.63.129.16 ve kaynaklarınız ile nasıl çalıştığı hakkında bilgi edinin.'
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: v-jesits
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/15/2019
ms.author: genli
ms.openlocfilehash: acfd5230d1bd572ea5179651558e3f736a8570af
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833134"
---
# <a name="what-is-ip-address-1686312916"></a>IP adresi: 168.63.129.16 nedir?

IP adresi: 168.63.129.16 Azure platformu kaynaklar için bir iletişim kanalı kolaylaştırmak için kullanılan bir sanal genel IP adresidir. Müşteriler, Azure'da kendi özel sanal ağ için herhangi bir adres alanı tanımlayabilirsiniz. Bu nedenle, Azure platformu kaynakların benzersiz bir genel IP adresi sunulmalıdır. Bu genel IP adresinin şunları sağlar:

- VM aracısının "Hazır" bir durumda olduğunu göstermek için Azure platformuyla iletişim kurmasına olanak tanır.
- Özel bir DNS sunucusu olmayan kaynaklara (örneğin, VM) filtrelenmiş ad çözümlemesi sağlamak için DNS sanal sunucu ile iletişimi sağlar. Bu filtreleme müşterilerin yalnızca ana bilgisayar adlarını kaynaklarını çözümleyebileceğinden emin olur.
- Sağlar [sistem durumu araştırmalarının Azure yük dengeleyiciden](../load-balancer/load-balancer-custom-probe-overview.md) Vm'leri sistem durumunu belirlemek için.
- DHCP hizmeti azure'da dinamik bir IP adresi almak VM sağlar.
- PaaS rolü için konuk Aracısı sinyal iletileri sağlar.

## <a name="scope-of-ip-address-1686312916"></a>IP adresi: 168.63.129.16 kapsamı

Genel IP adresini 168.63.129.16 tüm bölgeler ve tüm Ulusal Bulutlar kullanılır. Bu özel bir genel IP adresi Microsoft'a ait ve değişmez. Varsayılan ağ güvenliği Grup kuralı tarafından engellenmiş olur. Tüm yerel güvenlik duvarı ilkeleri bu IP adreslerine izin verecek öneririz. Bu özel IP adresi ve kaynaklar arasındaki iletişimi güvenli çünkü yalnızca iç Azure platformu, bu IP adresinden bir ileti edinebilir. Bu adresi engellendiğinde beklenmeyen davranış çeşitli senaryoları ortaya çıkabilir.

[Azure Load Balancer sistem durumu araştırmalarının](../load-balancer/load-balancer-custom-probe-overview.md) bu IP adresinden kaynaklanan. Bu IP adres bloğu, araştırmaları başarısız olur.

Bir sanal ağ (Klasik) senaryosunda, durum yoklaması, özel bir IP kaynağı ve 168.63.129.16 kullanılmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik grupları](security-overview.md)
- [Ağ güvenlik grubu oluşturma, değiştirme veya silme](manage-network-security-group.md)
