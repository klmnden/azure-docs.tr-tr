---
title: 'Noktadan siteye bir istemci sertifikası yükleyin: Azure | Microsoft Docs'
description: P2S sertifika kimlik doğrulaması - Windows, Mac, Linux için istemci sertifikası yükleyin.
services: vpn-gateway
documentationcenter: na
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: c278c1c85961fbeb0779cad98f8ac16d4961ba75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679988"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>P2S sertifika kimlik doğrulaması bağlantıları için istemci sertifikalarını yükleme

Noktadan siteye Azure sertifika doğrulaması kullanarak bir sanal ağa bağlanan tüm istemciler, bir istemci sertifikası gerektirir. Bu makale P2S kullanarak bir sanal ağa bağlanırken kimlik doğrulaması için kullanılan bir istemci sertifikasını yükleme yardımcı olur.

## <a name="generate"></a>Bir istemci sertifikası alın

Hangi istemci bağlanmak istediğiniz işletim sistemi ne olursa olsun, her zaman bir istemci sertifikası yüklü olmalıdır. Bir kuruluş CA çözümü kullanılarak oluşturulan bir kök sertifika veya otomatik olarak imzalanan kök sertifika bir istemci sertifikası oluşturabilirsiniz. Bkz: [PowerShell](vpn-gateway-certificates-point-to-site.md), [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md), veya [Linux](vpn-gateway-certificates-point-to-site-linux.md) istemci sertifikası oluşturma adımları için yönergeleri. 

## <a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="installmac"></a>Mac

>[!NOTE]
>Mac VPN istemcileri, Resource Manager dağıtım modeli için yalnızca desteklenir. Klasik dağıtım modeli için desteklenmez.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="installlinux"></a>Linux

Linux istemci sertifikasını istemcinin istemci yapılandırmasının bir parçası olarak yüklenir. Bkz: [istemci yapılandırması - Linux](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli) yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırma adımları için devam [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md).