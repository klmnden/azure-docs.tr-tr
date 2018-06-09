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
ms.openlocfilehash: 383c4a53913333a3e25006980dd7533b9e243a4a
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236700"
---
1 Temmuz 2018 başlatma desteği için TLS 1.0 ve 1.1 Azure VPN ağ geçidi'nden kaldırılıyor. VPN ağ geçidi yalnızca TLS 1.2 destekleyecektir. TLS kullanan, Windows 7 ve Windows 8 noktadan siteye istemciler için TLS desteği ve bağlantısını korumak için aşağıdaki güncelleştirmeleri yüklemenizi öneririz:

• [TLS kullanımını etkinleştirir Microsoft EAP uygulama güncelleştirmesi](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

• [WinHTTP güvenli iletişim kuralları varsayılan olarak, TLS 1.1 ve TLS 1.2 etkinleştirmek için güncelleştirme](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Aşağıdaki eski algoritmaları ayrıca için TLS 1 Temmuz 2018 kullanım dışı kalacaktır:

* RC4 (Rivest şifre 4)
* DES (veri şifreleme algoritması)
* 3DES (Üçlü Veri şifreleme algoritması)
* MD5 (İleti Özeti 5)
* SHA-1 (Güvenli Karma Algoritması 1)