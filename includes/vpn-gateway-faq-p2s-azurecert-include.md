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
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2018
ms.locfileid: "30197163"
---
[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Noktadan Siteye bağlanabilirlik için kendi iç PKI kök CA’mı kullanabilir miyim?

Evet. Önceden, yalnızca otomatik olarak imzalanan kök sertifikalar kullanılabiliyordu. 20 kök sertifika yükleyebilirsiniz.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Sertifikaları oluşturmak için hangi araçları kullanabilirim?

Kurumsal PKI çözümünüzü (dahili PKI'nizi), Azure PowerShell'i, MakeCert'i ve OpenSSL'yi kullanabilirsiniz.

### <a name="certsettings"></a>Sertifika ayarları ve parametreler için yönergeler var mı?

* **Dahili PKI/Kurumsal PKI çözümü:** [Sertifikaları oluşturma](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) adımlarına bakın.

* **Azure PowerShell:** Adımlar için [Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) makalesine bakın.

* **MakeCert:** Adımlar için [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) makalesine bakın.

* **OpenSSL:** 

    * Sertifikaları dışarı aktarırken kök sertifikayı Base64'e dönüştürdüğünüzden emin olun.

    * İstemci sertifikası için:

      * Özel anahtarı oluştururken uzunluğu 4096 olarak belirtin.
      * Sertifikayı oluştururken, *-extensions* parametresini *usr_cert* olarak belirtin.