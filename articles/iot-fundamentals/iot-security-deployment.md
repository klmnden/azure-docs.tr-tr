---
title: Azure nesnelerin interneti (IOT) dağıtımınızın güvenliğini sağlama | Microsoft Docs
description: Bu makalede, Azure IOT dağıtımınızın güvenliğini sağlama işlemi açıklanmaktadır
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: b6a444e64e58611e36208cfa615b4a148de3596c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182685"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-solution-accelerator-cipher-suites"></a>IOT Çözüm Hızlandırıcısı şifre paketleri

IOT Çözüm Hızlandırıcıları şu sırayla aşağıdaki şifre paketleri destekler.

| Şifre paketi | Uzunluk |
| --- | --- |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. FS 7680 bit RSA) |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FS 7680 bit RSA) |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_RSA\_ile\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_ile\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Ayrıca bkz.

IOT Hub güvenlik hakkında bilgi edinin [IOT hub'a erişimi denetleme](../iot-hub/iot-hub-devguide-security.md) IOT Hub Geliştirici Kılavuzu'nda. 
