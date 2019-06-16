---
title: Azure'da bir sanal ağ silinemiyor | Microsoft Docs
description: Azure'da bir sanal ağ silemezsiniz sorun giderme hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 5d2e10a4c5cd5b5dc1a8fe19cef7bc47f68d3fbe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235003"
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Sorun giderme: Azure'da bir sanal ağ silinemedi

Microsoft azure'da bir sanal ağ silmeye çalıştığınızda hatalar alabilirsiniz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımları sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [Bir sanal ağ geçidi sanal ağında çalışıp çalışmadığını denetleyin](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Bir uygulama ağ geçidi sanal ağında çalışıp çalışmadığını denetleyin](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Azure Active Directory etki alanı hizmeti sanal ağ içinde etkin olup olmadığını denetle](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Sanal ağ başka bir kaynağa bağlı olup olmadığını denetleyin](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Bir sanal makine sanal ağ içinde hala çalışıp çalışmadığını denetleyin](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Sanal ağ içinde geçiş takılı olup olmadığını denetleyin](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Bir sanal ağ geçidi sanal ağında çalışıp çalışmadığını denetleyin

Sanal Ağ'ı kaldırmak için sanal ağ geçidini kaldırmanız gerekir.

Klasik sanal ağlar için Git **genel bakış** Azure portalında Klasik sanal ağ sayfasının. İçinde **VPN bağlantıları** bölümünde, ağ geçidi sanal ağda çalışıyor, ağ geçidi IP adresini görürsünüz. 

![Ağ geçidi çalışıp çalışmadığını denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Sanal ağlar için Git **genel bakış** sayfasında sanal ağ. Denetleme **bağlı cihazları** sanal ağ geçidi için.

![Onay bağlı cihaza](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Ağ geçidini kaldırmadan önce öncelikle herhangi kaldırmanız **bağlantı** ağ geçidi nesneleri. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Bir uygulama ağ geçidi sanal ağında çalışıp çalışmadığını denetleyin

Git **genel bakış** sayfasında sanal ağ. Denetleme **bağlı cihazları** application gateway için.

![Onay bağlı cihaza](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Bir uygulama ağ geçidi varsa, sanal ağı silmeden önce bunu kaldırmanız gerekir.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Azure Active Directory etki alanı hizmeti sanal ağ içinde etkin olup olmadığını denetleyin

Active Directory etki alanı hizmeti etkin ve sanal ağa, bu sanal ağ silinemiyor. 

![Onay bağlı cihaza](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Hizmetini devre dışı bırakmak için bkz: [devre dışı Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../active-directory-domain-services/delete-aadds.md).

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Sanal ağ başka bir kaynağa bağlı olup olmadığını denetleyin

Bağlantı hattı bağlantıları, bağlantılar ve sanal ağ eşlemesi için denetleyin. Aşağıdakilerden herhangi biri, bir sanal ağ silme işlemi başarısız olmasına neden olabilir. 

Önerilen silme sırası aşağıdaki gibidir:

1. Ağ Geçidi bağlantıları
2. Ağ geçitleri
3. IP'ler
4. Sanal Ağ eşlemesi
5. App Service ortamı (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Bir sanal makine sanal ağ içinde hala çalışıp çalışmadığını denetleyin

Sanal makine sanal ağ içinde olduğundan emin olun.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Sanal ağ içinde geçiş takılı olup olmadığını denetleyin

Sanal ağ geçiş durumunda takılıyorsa silinemiyor. Geçiş iptal etmek için aşağıdaki komutu çalıştırın ve sanal ağ'ı silin.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Sanal Ağ](virtual-networks-overview.md)
- [Azure Sanal Ağ hakkında sık sorulan sorular (SSS)](virtual-networks-faq.md)
