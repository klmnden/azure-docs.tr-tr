---
title: include dosyası
description: include dosyası
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 06/20/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 9ba9bc993832350f6b6ce1c642e2dc852731b6f0
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39029995"
---
<a name="virtual-networking-limits-classic"></a>Aşağıdaki sınırlar yalnızca abonelik başına klasik dağıtım modeliyle yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Sanal ağlar |50 |100 |
| Yerel ağ siteleri |20 |desteğe başvurun |
| Sanal ağ başına DNS sunucusu sayısı |20 |100 |
| Sanal ağ başına özel IP Adresi sayısı |4096 |4096 |
| Bir sanal makine veya rol örneği, bir NIC eş zamanlı TCP veya UDP akışlar |500K |500K |
| Ağ Güvenlik Grupları (NSG) |100 |200 |
| NSG başına NSG kuralları |200 |1000 |
| Kullanıcı tanımlı yol tabloları |100 |200 |
| Yol tablosu başına kullanıcı tanımlı yol sayısı |100 |400 |
| Genel IP adresleri (dinamik) |5 |desteğe başvurun |
| Ayrılmış genel IP adresleri |20 |desteğe başvurun |
| Dağıtım başına genel VIP |5 |desteğe başvurun |
| Dağıtım başına özel VIP (ILB) |1 |1 |
| Uç Nokta Erişim Denetim Listeleri (ACL’ler) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Ağ Limitleri - Azure Resource Manager
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

| Kaynak | Varsayılan limit | Üst Sınır |
| --- | --- | --- |
| Sanal ağlar |50 |1000 |
| Sanal ağ başına alt ağ sayısı |1000 |10000 |
| Sanal ağ başına sanal ağ eşlemesi |50 ** |100 |
| Sanal ağ başına DNS sunucusu sayısı |9 |25 |
| Sanal ağ başına özel IP Adresi sayısı |16384 ** |16384 |
| Ağ arabirimi başına özel IP Adresleri |256 |256 |
| Bir sanal makine veya rol örneği, bir NIC eş zamanlı TCP veya UDP akışlar |500K |500K |
| Ağ Arabirimleri (NIC) |24000 ** |24000 |
| Ağ Güvenlik Grupları (NSG) |100 |5000 |
| NSG başına NSG kuralları |1000 ** |1000 |
| IP adresleri ve aralıkları kaynak veya hedef bir güvenlik grubu için belirtilen |2000 |4000 |
| Uygulama güvenliği grupları |500 |3000 |
| IP yapılandırması, NIC başına başına uygulama güvenlik grupları |10 |20 |
| Uygulama güvenlik grubu başına IP yapılandırmaları |1000 |4000 |
| İçindeki bir ağ güvenlik grubunun tüm güvenlik kuralları belirtilen uygulama güvenlik grupları |50 |100 |
| Kullanıcı tanımlı yol tabloları |100 |200 |
| Yol tablosu başına kullanıcı tanımlı yol sayısı |400 ** |400 |
| Genel IP adresi - dinamik |(Temel) 60 |desteğe başvurun |
| Genel IP adresleri - statik |(Temel) 20 |desteğe başvurun |
| Genel IP adresleri - statik |20 (standart) |desteğe başvurun |
| VPN Ağ Geçidi başına Noktadan Siteye Kök Sertifika Sayısı |20 |20 |

** Bu, daha önce desteğini artırılmış limitler uygulanmamış abonelikleri sınırları uygulamak varsayılan güncelleştirildi. Varsa geçmişte destek artırılana bu limitleri vardır ve bunları yeni varsayılan olarak, lütfen güncel almak istersiniz [ücretsiz bir çevrimiçi müşteri destek isteği açın](../articles/azure-resource-manager/resource-manager-quota-errors.md)

#### <a name="load-balancer"></a>Yük Dengeleyici sınırları
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md)

| Kaynak | Varsayılan limit | Üst Sınır |
| --- | --- | --- |
| Yük Dengeleyiciler | 100 | 1000 |
| Her bir kaynak, temel kuralları | 150 | 250 |
| Her bir kaynak, standart kuralları | 1250 | 1500 |
| IP yapılandırması başına kuralları | 299 |299 |
| Ön uç IP yapılandırmaları, temel | 10 | 200 |
| Ön uç IP yapılandırmaları, standart | 10 | 600 |
| Arka uç havuzu, temel | 100, tek bir kullanılabilirlik kümesi | 100, tek bir kullanılabilirlik kümesi |
| Arka uç havuzu, standart | 1000, tek bir sanal ağ | 1000, tek bir sanal ağ |
| Standart yük dengeleyici başına arka uç kaynaklarına &ast; | 50 | 150 |
| HA bağlantı noktaları, standart | İç ön uç başına 1 | İç ön uç başına 1 |

&ast; En fazla 150 kaynaklar, tek başına sanal makineler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri herhangi bir birleşimi.

Varsayılan sınırları artırmanız gerekirse [desteğe başvurun](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

