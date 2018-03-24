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
ms.openlocfilehash: 97d33bfcc8251b10ba121b7fb013800904450563
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. 20 kök sertifika yükleyebilirsiniz.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Sertifikalar oluşturmak için hangi Araçlar kullanabilir miyim?

Kuruluş PKI çözümü (iç PKI), Azure PowerShell, MakeCert ve OpenSSL kullanabilirsiniz.

### <a name="certsettings"></a>Sertifika ayarları ve parametreler için yönergeler var mı?

* **İç PKI/Kuruluş PKI çözümü:** adımlarına bakın [oluşturmak sertifikaları](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** bkz [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) adımları makalesinde bulabilirsiniz.

* **MakeCert:** bkz [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) adımları makalesinde bulabilirsiniz.

* **OpenSSL:** 

    * Sertifikaları verirken Base64 için kök sertifika dönüştürmek emin olun.

    * İçin istemci sertifikası:

      * Özel anahtar oluştururken uzunluğu 4096 olarak belirtin.
      * Sertifika için oluştururken *-uzantıları* parametresini belirtin *usr_cert*.