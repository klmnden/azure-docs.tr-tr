---
title: 'Oluşturma ve noktadan siteye için sertifikalar verme: Linux: CLI: Azure | Microsoft Docs'
description: Otomatik olarak imzalanan kök sertifika oluşturma, ortak anahtarını dışarı aktarmak ve Linux (strongSwan) CLI kullanarak istemci sertifikalarını oluşturmak.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: cherylmc
ms.openlocfilehash: b673be47d4951adab08f04efc56410095f549356
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60766183"
---
# <a name="generate-and-export-certificates"></a>Oluşturma ve sertifika dışarı aktarma

Noktadan siteye bağlantılar, kimlik doğrulaması için sertifikaları kullanır. Bu makalede Linux CLI ve strongSwan kullanarak istemci sertifikalarını oluşturmak ve otomatik olarak imzalanan kök sertifika oluşturma gösterilmektedir. Farklı bir sertifika yönergeler arıyorsanız bkz [Powershell](vpn-gateway-certificates-point-to-site.md) veya [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) makaleler. Adımları yerine CLI GUI kullanarak strongSwan yükleme hakkında daha fazla bilgi için bkz. [istemci yapılandırması](point-to-site-vpn-client-configuration-azure-cert.md#install) makalesi.

## <a name="generate-and-export"></a>Oluşturma ve dışarı aktarma
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye yapılandırmanızı devam [VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
