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
ms.openlocfilehash: 5b9e036816aa532d32b1b4305ef6ae646ae05bae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66159251"
---
Aşağıdaki tabloda PolicyBased ve RouteBased VPN ağ geçitleri için gereksinimler listelenmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik modeli için PolicyBased VPN ağ geçitleri statik ağ geçitleri ile aynıdır ve rota tabanlı ağ geçitleri dinamik ağ geçitleri ile aynıdır.

|  | **PolicyBased temel VPN Gateway** | **Temel RouteBased VPN ağ geçidi** | **Standart RouteBased VPN ağ geçidi** | **RouteBased ve yüksek performanslı VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden siteye bağlantı (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantılarının maksimum sayısı** |1 |10 |10 |30 |
| **P2S bağlantı sayısı üst sınırı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** |Desteklenmiyor |Desteklenmiyor |Desteklenen |Desteklenen |
