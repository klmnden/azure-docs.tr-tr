---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9a17f34333503436d3da340670abdde154e45ef6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38727547"
---
1 Temmuz 2018 tarihinden itibaren TLS 1.0 ve 1.1 desteği Azure VPN Gateway'den kaldırılıyor. VPN Gateway, yalnızca TLS 1.2’yi destekleyecektir. TLS kullanan Windows 7 ve Windows 8 noktadan siteye istemcilerinize yönelik TLS desteği ve bağlantısını korumak için aşağıdaki güncelleştirmeleri yüklemeniz önerilir:

•   [TLS kullanımını sağlayan Microsoft EAP uygulaması güncelleştirmesi](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•   [WinHTTP’de varsayılan güvenli protokol olarak TLS 1.1 ve TLS 1.2’yi etkinleştirmeye yönelik güncelleştirme](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Aşağıdaki eski algoritmalar da TLS için 1 Temmuz 2018'de kullanım dışı kalacaktır:

* RC4 (Rivest Şifrelemesi 4)
* DES (Veri Şifreleme Algoritması)
* 3DES (Üçlü Veri Şifreleme Algoritması)
* MD5 (İleti Özeti 5)
