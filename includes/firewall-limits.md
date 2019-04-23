---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 3/25/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0467359cd9d6a067e519a62532f00459bc5f68cb
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805373"
---
| Kaynak | Varsayılan limit |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10.000 işlem, tüm kural türleri birleştirilmiş.|
|En düşük AzureFirewallSubnet boyutu |/26|
|Bağlantı noktası aralığında ağ ve uygulama kuralları|0-64,000. İş, bu sınırlama gevşetmek için devam ediyor.|
|Rota tablosu|NextHopType değeri ayarlanmış varsayılan olarak, AzureFirewallSubnet 0.0.0.0/0 rota sahip **Internet**.<br><br>Şirket içi ExpressRoute veya VPN ağ geçidi aracılığıyla zorlamalı tünel etkinleştirirseniz, açıkça Internet olarak ayarlanan NextHopType değeri ile 0.0.0.0/0 kullanıcı tanımlı yol (UDR) yapılandırmak ve bunu, AzureFirewallSubnet ile ilişkilendirmek gerekebilir. Bu, olası varsayılan ağ geçidi, şirket içi ağınıza geri BGP reklamı geçersiz kılar. Kuruluşunuz Azure Güvenlik Duvarı'nda, varsayılan ağ geçidi trafiği şirket içi ağınız üzerinden yeniden yönlendirmek zorlamalı tünel gerektiriyorsa, desteğe başvurun. Internet bağlantısı gerekli güvenlik duvarı emin olmak için aboneliğinizi korunur bir beyaz liste şunları yapabiliriz.|

<sup>1</sup>bu sınırı artırmanız gerekiyorsa, Azure desteği'ne başvurun.
