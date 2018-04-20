---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5e7e365a73c9ba9cfd91d4f93759e7af55a0ad48
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
Aşağıdaki tabloda PolicyBased ve RouteBased VPN ağ geçitleri için gereksinimleri listelenmiştir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik modeli PolicyBased VPN ağ geçitleri statik ağ geçitleri ile aynıdır ve rota tabanlı ağ geçitleri dinamik ağ geçitleri ile aynıdır.

|  | **PolicyBased temel VPN ağ geçidi** | **RouteBased temel VPN ağ geçidi** | **RouteBased standart VPN ağ geçidi** | **RouteBased yüksek performanslı VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden siteye bağlantı (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantısı sayısı** |1 |10 |10 |30 |
| **P2S bağlantısı sayısı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** (*) |Desteklenmiyor |Desteklenmiyor |Desteklenen |Desteklenen |

  (*) BGP Klasik dağıtım modeli için desteklenmiyor.
