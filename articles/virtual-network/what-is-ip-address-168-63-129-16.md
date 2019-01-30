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
ms.date: 01/21/2019
ms.author: genli
ms.openlocfilehash: e018cbf0c71a9acf76e60f38aff1aa1ba8a81516
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55229318"
---
# <a name="what-is-ip-address-1686312916"></a>IP adresi: 168.63.129.16 nedir?

IP adresi: 168.63.129.16 Azure platformu kaynaklar için bir iletişim kanalı kolaylaştırmak için kullanılan bir sanal genel IP adresidir. Müşteriler, Azure'da kendi özel sanal ağ için herhangi bir adres alanı tanımlayabilirsiniz. Bu nedenle, Azure platformu kaynakların benzersiz bir genel IP adresi sunulmalıdır. Bu genel IP adresinin şunları sağlar:

- VM aracısının "Hazır" bir durumda olduğunu göstermek için Azure platformuyla iletişim kurmasına olanak tanır.
- Özel bir DNS sunucusu olmayan kaynaklara (örneğin, VM) filtrelenmiş ad çözümlemesi sağlamak için DNS sanal sunucu ile iletişimi sağlar. Bu filtreleme müşterilerin yalnızca ana bilgisayar adlarını kaynaklarını çözümleyebileceğinden emin olur.
- Gelen yük dengeli bir kümedeki VM'lerin sistem durumunu belirlemek için yük dengeleyici sistem durumu araştırmaları etkinleştirir.
- PaaS rolü için konuk Aracısı sinyal iletileri sağlar.

## <a name="scope-of-ip-address-1686312916"></a>IP adresi: 168.63.129.16 kapsamı

Genel sanal IP adresi 168.63.129.16 tüm bölgeler ve tüm Ulusal Bulutlar kullanılır. Bu özel bir genel IP adresi değişmez. Varsayılan ağ güvenliği Grup kuralı tarafından engellenmiş olur. Tüm yerel güvenlik duvarı ilkeleri bu IP adreslerine izin verecek öneririz. Bu özel IP adresi ve kaynaklar arasındaki iletişimi güvenli çünkü yalnızca iç Azure platformu, bu IP adresinden bir ileti edinebilir. Bu adresi engellendiğinde beklenmeyen davranış çeşitli senaryoları ortaya çıkabilir.

Ayrıca, 168.63.129.16 için yapılandırılmış uç noktasına traffics genel sanal IP adresi bir [yük dengeleyici durum araştırması](../load-balancer/load-balancer-custom-probe-overview.md) saldırı trafiği değerlendirilmemelidir. Sanal olmayan ağ senaryosunda, sistem durumu araştırması özel bir IP kaynağı.


## <a name="next-steps"></a>Sonraki adımlar

- [Güvenlik grupları](security-overview.md)
- [Ağ güvenlik grubu oluşturma, değiştirme veya silme](manage-network-security-group.md)
