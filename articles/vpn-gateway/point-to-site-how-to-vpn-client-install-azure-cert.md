---
title: "P2S istemci sertifikası yüklemek | Azure"
description: "Bu makalede P2S sertifika kimlik doğrulaması için istemci sertifikası yüklemenize yardımcı olur."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/24/2017
ms.author: cherylmc
ms.openlocfilehash: fc0cc37794ef291c9d27b094211b38cec90da55c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-client-certificate-for-point-to-site-azure-certificate-authentication-connections"></a>Noktadan siteye Azure sertifika kimlik doğrulaması bağlantıları için bir istemci sertifikası yükleme

Noktadan siteye Azure sertifika kimlik doğrulaması kullanan bir sanal ağa bağlanan tüm istemciler, bir istemci sertifikası gerektirir. Bu makalede P2S kullanarak bir sanal ağa bağlanırken kimlik doğrulaması için kullanılan bir istemci sertifikası yüklemenize yardımcı olur.

## <a name="generate"></a>Oluşturma ve bir istemci sertifikasını dışarı aktarma

Bir istemci sertifikası ya da bir kuruluş CA'sı çözümü kullanılarak oluşturulan bir kök sertifikası veya otomatik olarak imzalanan sertifika oluşturabilirsiniz. Bkz: [PowerShell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) adımlar için yönergeler. İstemci sertifikalarını oluşturduktan sonra bunları .pfx dosyaları olarak dışarı aktarın. Tüm sertifika zincirini verilirken eklediğinizden emin olun.

## <a name="installwin"></a>Windows istemcilerinde bir sertifika yükleyin

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Mac istemcileri üzerinde bir sertifika yükleyin

Mac VPN istemcileri, Resource Manager dağıtım modeli için yalnızca desteklenir. Klasik dağıtım modeli için desteklenmez.

> [!NOTE]
>  IKEv2 şu anda Önizleme sürümündedir.
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırması adımlarla devam edin.

* [Azure portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
* [PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
* [Azure portal (klasik)](vpn-gateway-howto-point-to-site-classic-azure-portal.md)