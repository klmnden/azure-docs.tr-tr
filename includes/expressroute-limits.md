---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c025c431d826d3a2951a9eb5c09308695e172887
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65551219"
---
| Resource | Varsayılan/üst sınır |
| --- | --- |
| Abonelik başına ExpressRoute devreleri |10 |
| ExpressRoute bağlantı hatları ile Azure Resource Manager, abonelik başına bölge başına |10 |
| Yollar, ExpressRoute standart Azure özel eşleme sayısı |4,000 |
| ExpressRoute Premium eklentisi ile Azure özel eşleme için yol sayısı |10,000 |
| Azure özel bir ExpressRoute bağlantısı için sanal ağ adresi alanından eşleme rotaları sayısı |200 | 
| Microsoft Azure ile standart ExpressRoute eşdüzey hizmet sağlama için rota sayısı |200 |
| Microsoft Azure ExpressRoute Premium eklentisi ile eşleme için yol sayısı |200 |
| ExpressRoute bağlantı hatları ile aynı konumda eşleme aynı sanal ağa bağlı en fazla sayısı |4 |
| ExpressRoute devreleri farklı eşleme konumları aynı sanal ağa bağlı en fazla sayısı |4 |
| ExpressRoute bağlantı hattı izin verilen sanal ağ bağlantılarının sayısı |Aşağıdaki tabloya bakın. |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>ExpressRoute bağlantı hattı başına sanal ağ sayısı
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

* Yalnızca ExpressRoute doğrudan
