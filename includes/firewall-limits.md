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
ms.openlocfilehash: 368a7cf1d372d79f062affe9b206d070afe5c2f7
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58505829"
---
| Kaynak | Varsayılan limit |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10.000 işlem, tüm kural türleri birleştirilmiş.|
|En düşük AzureFirewallSubnet boyutu |/26|
|Bağlantı noktası aralığında ağ ve uygulama kuralları|0-64,000. İş, bu sınırlama gevşetmek için devam ediyor.|
|Rota tablosu|NextHopType değeri ayarlanmış varsayılan olarak, AzureFirewallSubnet 0.0.0.0/0 rota sahip **Internet**.<br><br>Şirket içi ExpressRoute veya VPN ağ geçidi aracılığıyla zorlamalı tünel etkinleştirirseniz, açıkça Internet olarak ayarlanan NextHopType değeri ile 0.0.0.0/0 kullanıcı tanımlı yol (UDR) yapılandırmak ve bunu, AzureFirewallSubnet ile ilişkilendirmek gerekebilir. Bu, olası varsayılan ağ geçidi, şirket içi ağınıza geri BGP reklamı geçersiz kılar. Kuruluşunuz Azure Güvenlik Duvarı'nda, varsayılan ağ geçidi trafiği şirket içi ağınız üzerinden yeniden yönlendirmek zorlamalı tünel gerektiriyorsa, desteğe başvurun. Internet bağlantısı gerekli güvenlik duvarı emin olmak için aboneliğinizi korunur bir beyaz liste şunları yapabiliriz.|

<sup>1</sup>bu sınırı artırmanız gerekiyorsa, Azure desteği'ne başvurun.
