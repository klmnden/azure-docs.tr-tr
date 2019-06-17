---
title: include dosyası
description: include dosyası
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 5/3/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8709d4d903bd31ff94d04ec61e226857e4190407
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66238189"
---
| Resource | Varsayılan limit |
| --- | --- |
| Veri aktarım hızı |30 Gbps<sup>1</sup> |
|Kurallar|10\.000 işlem, tüm kural türleri birleştirilmiş.|
|En düşük AzureFirewallSubnet boyutu |/26|
|Bağlantı noktası aralığında ağ ve uygulama kuralları|0-64,000. İş, bu sınırlama gevşetmek için devam ediyor.|
|Yol tablosu|NextHopType değeri ayarlanmış varsayılan olarak, AzureFirewallSubnet 0.0.0.0/0 rota sahip **Internet**.<br><br>Azure güvenlik duvarı, doğrudan Internet bağlantısı olması gerekir. Bir varsayılan yolu BGP aracılığıyla şirket içi ağınıza, AzureFirewallSubnet öğrenir, bu ile 0.0.0.0/0 UDR ile geçersiz kılmanız gerekir **NextHopType** değer kümesini olarak **Internet** doğrudan korumak için Internet bağlantısı. Varsayılan olarak, bir şirket içi ağ için zorlamalı tünel, Azure Güvenlik Duvarı'nı desteklemez.<br><br>Bir şirket içi ağ için zorlamalı tünel yapılandırma gerektirir, ancak Microsoft bunu tek olay temelinde destekler. Biz durumunuzu gözden geçirmek, desteğe başvurun. Kabul ediyoruz aboneliğinizi beyaz liste göreceksiniz ve gerekli güvenlik duvarı Internet bağlantısı karşılandığından emin olun.|

<sup>1</sup>bu sınırı artırmanız gerekiyorsa, Azure desteği'ne başvurun.
