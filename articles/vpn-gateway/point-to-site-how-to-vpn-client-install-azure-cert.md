---
title: P2S istemci sertifikası yüklemek | Azure
description: P2S sertifika kimlik doğrulaması için bir Mac veya Windows istemci sertifikası yükleyin.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager, azure-service-management
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc
ms.openlocfilehash: bf2788fff64ab8b3a5ccf75b8a80f2bd5aba5151
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30318002"
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Noktadan siteye Azure sertifika kimlik doğrulaması bağlantıları için bir istemci sertifikası yükleme

Noktadan siteye Azure sertifika kimlik doğrulaması kullanan bir sanal ağa bağlanan tüm istemciler, bir istemci sertifikası gerektirir. Bu makalede P2S kullanarak bir sanal ağa bağlanırken kimlik doğrulaması için kullanılan bir istemci sertifikası yüklemenize yardımcı olur.

## <a name="generate"></a>Oluşturma ve bir istemci sertifikasını dışarı aktarma

Bir istemci sertifikası ya da bir kuruluş CA'sı çözümü kullanılarak oluşturulan bir kök sertifikası veya otomatik olarak imzalanan sertifika oluşturabilirsiniz. Bkz: [PowerShell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) adımlar için yönergeler. İstemci sertifikalarını oluşturduktan sonra bunları .pfx dosyaları olarak dışarı aktarın. Tüm sertifika zincirini verilirken eklediğinizden emin olun.

## <a name="installwin"></a>Sertifikayı - Windows Yükle

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Sertifikayı - Mac yükle

Mac VPN istemcileri, Resource Manager dağıtım modeli için yalnızca desteklenir. Klasik dağıtım modeli için desteklenmez.

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırması adımlarla devam edin.

* [Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
