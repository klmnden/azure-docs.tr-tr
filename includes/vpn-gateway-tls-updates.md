---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e4d20cd39d2a843ee1ab57a412ac668b3495fdb1
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188195"
---
>[!NOTE]
>1 Temmuz 2018 tarihinden itibaren TLS 1.0 ve 1.1 desteği Azure VPN Gateway'den kaldırılıyor. VPN Gateway, yalnızca TLS 1.2’yi destekleyecektir. Desteğin sürmesi için bkz: [TLS1.2 desteğini etkinleştirmek için güncelleştirmeleri](#tls1).

Ayrıca, aşağıdaki eski algoritmaları da için TLS 1 Temmuz 2018'de kullanım dışı kalacaktır:

* RC4 (Rivest Şifrelemesi 4)
* DES (Veri Şifreleme Algoritması)
* 3DES (Üçlü Veri Şifreleme Algoritması)
* MD5 (İleti Özeti 5)

### <a name="tls1"></a>TLS 1.2 Windows 7 ve Windows 8.1 için destek nasıl etkinleştirebilirim?

[!INCLUDE [tls 1.2](vpn-gateway-tls-include.md)]
