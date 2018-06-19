---
title: Nesnelerin interneti dağıtımınızın güvenliğini | Microsoft Docs
description: Bu makalede, IOT dağıtımınızın güvenliğini nasıl ayrıntıları
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: dobett
ms.openlocfilehash: b90d916514e7b5ce566d4823b44d4b4ef575224d
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2018
ms.locfileid: "35972193"
---
[!INCLUDE [iot-secure-your-deployment](../../includes/iot-secure-your-deployment.md)]

## <a name="iot-solution-accelerator-cipher-suites"></a>IOT Çözüm Hızlandırıcısı şifre paketleri

IOT Çözüm Hızlandırıcıları aşağıdaki şifre paketleri, bu sırada destekler.

| Şifre paketi | Uzunluk |
| --- | --- |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. 7680 bit RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_ECDHE\_RSA\_ile\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. 7680 bit RSA) FS |256 |
| TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. 3072 bit RSA) FS |128 |
| TLS\_RSA\_ile\_AES\_256\_GCM\_SHA384 (0x9d) |256 |
| TLS\_RSA\_ile\_AES\_128\_GCM\_SHA256 (0x9c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA256 (0x3d) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA256 (0x3c) |128 |
| TLS\_RSA\_ile\_AES\_256\_CBC\_SHA (0x35) |256 |
| TLS\_RSA\_ile\_AES\_128\_CBC\_SHA (0x2f) |128 |
| TLS\_RSA\_ile\_3DES\_EDE\_CBC\_SHA (0xa) |112 |

## <a name="see-also"></a>Ayrıca bkz.

IOT hub'ı güvenlik konusunda okuma [IOT Hub'ına erişim denetim] [ lnk-devguide-security] IOT Hub Geliştirici Kılavuzu'nda.

[lnk-devguide-security]: /azure/iot-hub/iot-hub-devguide-security.md
