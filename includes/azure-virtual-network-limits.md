---
title: include dosyası
description: include dosyası
services: networking
author: jimdial
ms.service: networking
ms.topic: include
ms.date: 08/16/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: 3a7c91f4a83cd69bdb87ffaccce555b04eca67cc
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597667"
---
<a name="virtual-networking-limits-classic"></a>Aşağıdaki sınırlar yalnızca abonelik başına klasik dağıtım modeliyle yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Sanal ağlar |50 |100 |
| Yerel ağ siteleri |20 |desteğe başvurun |
| Sanal ağ başına DNS sunucusu sayısı |20 |20 |
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

> [!NOTE]
> Size yakın zamanda tüm varsayılan sınır en fazla limitlerini artırdık. Yoksa hiçbir **sınırı** sütun, ardından kaynak sınırları ayarlanabilir yok. Olan destek geçmişte artırılana limitler ve güncelleştirilmiş sınırları aşağıdaki gibi görmezsiniz varsa [ücretsiz bir çevrimiçi müşteri destek isteği açın](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Kaynak | Varsayılan limit | 
| --- | --- |
| Sanal ağlar |1000 |
| Sanal ağ başına alt ağ sayısı |3000 |
| Sanal ağ başına sanal ağ eşlemesi |100 |
| Sanal ağ başına DNS sunucusu sayısı |20 |
| Sanal ağ başına özel IP Adresi sayısı |65536 |
| Ağ arabirimi başına özel IP Adresleri |256 |
| Bir sanal makine veya rol örneği, bir NIC eş zamanlı TCP veya UDP akışlar |500K |
| Ağ Arabirimleri (NIC) |65536 |
| Ağ Güvenlik Grupları (NSG) |5000 |
| NSG başına NSG kuralları |1000 |
| IP adresleri ve aralıkları kaynak veya hedef bir güvenlik grubu için belirtilen |4000 |
| Uygulama güvenliği grupları |3000 |
| IP yapılandırması, NIC başına başına uygulama güvenlik grupları |20 |
| Uygulama güvenlik grubu başına IP yapılandırmaları |4000 |
| İçindeki bir ağ güvenlik grubunun tüm güvenlik kuralları belirtilen uygulama güvenlik grupları |100 |
| Kullanıcı tanımlı yol tabloları |200 |
| Yol tablosu başına kullanıcı tanımlı yol sayısı |400 |
| VPN Ağ Geçidi başına Noktadan Siteye Kök Sertifika Sayısı |20 |
| Sanal ağ dokunduğunda |100 |
| Sanal ağ TAP başına ağ arabirimi DOKUNUN yapılandırması |100 |

#### <a name="publicip-address"></a>Genel IP adresi sınırlamaları
| Kaynak | Varsayılan limit | Üst Sınır |
| --- | --- | --- |
| Genel IP adresi - dinamik |(Temel) 1000 |desteğe başvurun |
| Genel IP adresleri - statik |200 (Temel) |desteğe başvurun |
| Genel IP adresleri - statik |200 (standart) |desteğe başvurun |

#### <a name="load-balancer"></a>Yük Dengeleyici sınırları
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md)

| Kaynak | Varsayılan limit |
| --- | --- | --- |
| Yük Dengeleyiciler | 1000 | 
| Her bir kaynak, temel kuralları | 250 |
| Her bir kaynak, standart kuralları | 1500 | 
| IP yapılandırması başına kuralları | 299 |
| Ön uç IP yapılandırmaları, temel | 200 |
| Ön uç IP yapılandırmaları, standart | 600 |
| Arka uç havuzu, temel | 100, tek bir kullanılabilirlik kümesi |
| Arka uç havuzu, standart | 1000, tek bir sanal ağ |
| Standart yük dengeleyici başına arka uç kaynaklarına * | 150 |
| HA bağlantı noktaları, standart | İç ön uç başına 1 |

** En fazla 150 kaynaklar, tek başına sanal makineler, kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri herhangi bir birleşimi.

Varsayılan sınırları artırmanız gerekirse [desteğe başvurun](../articles/azure-supportability/resource-manager-core-quotas-request.md ).

