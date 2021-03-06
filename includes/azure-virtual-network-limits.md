---
title: include dosyası
description: include dosyası
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 06/25/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 3e66bf61caf786473d89eab9a6567bb05aff0d19
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67457582"
---
<a name="azure-resource-manager-virtual-networking-limits"></a>Ağ limitleri - Azure Resource Manager'ın aşağıdaki sınırlar geçerlidir yalnızca ağ aracılığıyla yönetilen kaynakları için **Azure Resource Manager** her Abonelikteki bölge başına. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Size yakın zamanda tüm varsayılan sınır en fazla limitlerini artırdık. Üst sınır sütun yok ise, kaynak sınırları ayarlanabilir sahip değil. Geçmişte destek artırılana limitler sahipti ve güncelleştirilmiş sınırları aşağıdaki tablolarda görmüyorsanız [ücretsiz bir çevrimiçi müşteri destek isteği açın](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Resource | Varsayılan/üst sınır | 
| --- | --- |
| Sanal ağlar |1000 |
| Sanal ağ başına alt ağ sayısı |3,000 |
| Sanal ağ başına sanal ağ eşlemesi |500 |
| Sanal ağ başına DNS sunucusu |20 |
| Sanal ağ başına özel IP adresleri |65,536 |
| Ağ arabirimi özel IP adresleri |256 |
| Sanal makine başına özel IP adresleri |256 |
| Bir sanal makine veya rol örneği, bir NIC eş zamanlı TCP veya UDP akışlar |500,000 |
| Ağ arabirim kartları |65,536 |
| Ağ Güvenlik Grupları |5,000 |
| NSG başına NSG kuralları |1000 |
| IP adresleri ve aralıkları kaynak veya hedef bir güvenlik grubu için belirtilen |4,000 |
| Uygulama güvenliği grupları |3,000 |
| IP yapılandırması, NIC başına başına uygulama güvenlik grupları |20 |
| Uygulama güvenlik grubu başına IP yapılandırmaları |4,000 |
| İçindeki bir ağ güvenlik grubunun tüm güvenlik kuralları belirtilen uygulama güvenlik grupları |100 |
| Kullanıcı tanımlı yol tabloları |200 |
| Yol tablosu başına kullanıcı tanımlı yollar |400 |
| Azure VPN ağ geçidi başına noktadan siteye kök sertifika |20 |
| Sanal ağ dokunduğunda |100 |
| Sanal ağ TAP başına ağ arabirimi DOKUNUN yapılandırması |100 |

#### <a name="publicip-address"></a>Genel IP adresi sınırlamaları
| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Genel IP adresi - dinamik | 1\.000 temel. |Desteğe başvurun. |
| Genel IP adresleri - statik | 1\.000 temel. |Desteğe başvurun. |
| Genel IP adresleri - statik | Standart için 200.|Desteğe başvurun. |
| Genel IP ön ek uzunluğu | /28 | Desteğe başvurun. |

#### <a name="load-balancer"></a>Yük Dengeleyici sınırları
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

| Resource | Varsayılan/üst sınır |
| --- | --- |
| Yük dengeleyiciler | 1000 | 
| Her bir kaynak, temel kuralları | 250 |
| Her bir kaynak, standart kuralları | 1\.500 | 
| IP yapılandırması başına kuralları | 299 |
| NIC başına kuralları | 300 |
| Ön uç IP yapılandırmaları, temel | 200 |
| Ön uç IP yapılandırmaları, standart | 600 |
| Arka uç havuzu, temel | 100, tek bir kullanılabilirlik kümesi |
| Arka uç havuzu, standart | 1\.000, tek bir sanal ağ |
| Standart yük dengeleyici başına arka uç kaynaklarına<sup>1</sup> | 150 |
| Yüksek kullanılabilirlik bağlantı noktaları, standart | ön uç iç başına 1 |

<sup>1</sup>sınır en fazla 150 kaynakları tek başına sanal makine kaynaklarının herhangi bir birleşimini, kaynakların ve sanal makine ölçek kümesi kaynaklarının kullanılabilirlik kümesi.

#### <a name="virtual-networking-limits-classic"></a>Yalnızca ağ aracılığıyla yönetilen kaynakları için aşağıdaki sınırlar geçerlidir **Klasik** abonelik başına dağıtım modeli. Bilgi edinmek için nasıl [, abonelik limitleri göre geçerli kaynak kullanımınızı görüntüleyin](../articles/networking/check-usage-against-limits.md).

| Resource | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Sanal ağlar |100 |100 |
| Yerel ağ siteleri |20 |Desteğe başvurun. |
| Sanal ağ başına DNS sunucusu |20 |20 |
| Sanal ağ başına özel IP adresleri |4,096 |4,096 |
| Bir sanal makine veya rol örneği, bir NIC eş zamanlı TCP veya UDP akışlar |1\.000.000 iki veya daha fazla NIC için en fazla 500.000. |1\.000.000 iki veya daha fazla NIC için en fazla 500.000. |
| Ağ güvenlik grupları (Nsg'ler) |200 |200 |
| NSG başına NSG kuralları |1000 |1000 |
| Kullanıcı tanımlı yol tabloları |200 |200 |
| Yol tablosu başına kullanıcı tanımlı yollar |400 |400 |
| Genel IP adresleri (dinamik) |500 |500 |
| Ayrılmış genel IP adresleri |500 |500 |
| Dağıtım başına genel VIP |5 |Desteğe başvurun |
| Dağıtım başına özel VIP (iç Yük Dengeleme) |1 |1 |
| Uç nokta erişim denetim listeleri (ACL'ler) |50 |50 |
