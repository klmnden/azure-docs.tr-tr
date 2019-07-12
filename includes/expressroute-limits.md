---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 07/25/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 45d34297bf37a6e46bc57e95ff49def49051e32e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67622129"
---
| Resource | Varsayılan/üst sınır |
| --- | --- |
| Abonelik başına ExpressRoute devreleri |10 |
| ExpressRoute bağlantı hatları ile Azure Resource Manager, abonelik başına bölge başına |10 |
| Azure özel standart ExpressRoute eşdüzey hizmet sağlama için en fazla yol sayısı tanıtılan |4,000 |
| Azure özel ExpressRoute Premium eklentisi ile eşleme için en fazla yol sayısı tanıtılan |10,000 |
| Maksimum sayısı, Azure özel bir ExpressRoute bağlantısı için sanal ağ adresi alanından eşlemeden tanıtılan |200 |
| En fazla yol sayısı ile standart ExpressRoute eşlemesi Microsoft'a tanıtılan |200 |
| Maksimum sayısı, ExpressRoute Premium eklentisi ile eşleme Microsoft'a tanıtılan |200 |
| ExpressRoute bağlantı hatları ile aynı konumda eşleme aynı sanal ağa bağlı en fazla sayısı |4 |
| ExpressRoute devreleri farklı eşleme konumları aynı sanal ağa bağlı en fazla sayısı |4 |
| ExpressRoute bağlantı hattı izin verilen sanal ağ bağlantılarının sayısı |Bkz: [ExpressRoute bağlantı hattı başına sanal ağ sayısı](#vnetpercircuit) tablo.  |

#### <a name="vnetpercircuit"></a> ExpressRoute bağlantı hattı başına sanal ağ sayısı
| **Bağlantı hattı boyutu** | **Standart sanal ağ bağlantılarının sayısı** | **Premium eklenti ile sanal ağ bağlantılarının sayısı** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |
| 40 Gbps* |10 |100 |
| 100 Gbps* |10 |100 |

**ExpressRoute doğrudan yalnızca 100 GB/sn*
