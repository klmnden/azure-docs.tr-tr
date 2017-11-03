---
title: "Azure sanal ağında silemezsiniz | Microsoft Docs"
description: "Azure sanal ağında silemezsiniz sorunu gidermek öğrenin."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: 3fd0beed7fe76d1cd0861cdc278ac66ead2d0bd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Sorun giderme: Azure sanal ağ silinemedi

Microsoft Azure sanal ağında silmeye çalıştığınızda hata alabilirsiniz. Bu makalede, bu sorunu gidermenize yardımcı olmak için sorun giderme adımlarını sağlar. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Sorun giderme rehberi 

1. [Bir sanal ağ geçidi sanal ağda çalışır durumda olup olmadığını denetleyin](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).
2. [Bir uygulama ağ geçidi sanal ağda çalışır durumda olup olmadığını denetleyin](#check-whether-an-application-gateway-is-running-in-the-virtual-network).
3. [Azure Active Directory etki alanı hizmeti sanal ağında etkinleştirilip etkinleştirilmediğini kontrol](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).
4. [Sanal ağ diğer kaynağa bağlı olup olmadığını denetleyin](#check-whether-the-virtual-network-is-connected-to-other-resource).
5. [Bir sanal makinenin sanal ağda hala çalışıp çalışmadığını denetleyin](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).
6. [Sanal ağ içinde geçiş takıldı olup olmadığını denetleyin](#check-whether-the-virtual-network-is-stuck-in-migration).

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Bir sanal ağ geçidi sanal ağda çalışır durumda olup olmadığını denetleyin

Sanal ağı kaldırmak için öncelikle sanal ağ geçidi kaldırmanız gerekir.

Klasik sanal ağlar için Git **genel bakış** Azure portalında bir Klasik sanal ağının sayfası. İçinde **VPN bağlantıları** bölümünde, ağ geçidi sanal ağda çalışıyorsa, ağ geçidinin IP adresi görürsünüz. 

![Ağ geçidi çalışır durumda olup olmadığını denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Sanal ağlar için Git **genel bakış** sanal ağın sayfası. Denetleme **bağlı cihazları** sanal ağ geçidi için.

![Bağlı bir aygıt denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Ağ geçidi kaldırmadan önce ilk herhangi kaldırın **bağlantı** ağ geçidi nesneleri. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Bir uygulama ağ geçidi sanal ağda çalışır durumda olup olmadığını denetleyin

Git **genel bakış** sanal ağın sayfası. Denetleme **bağlı cihazları** uygulama ağ geçidi için.

![Bağlı bir aygıt denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Bir uygulama ağ geçidi varsa, sanal ağı silmeden önce onu kaldırmanız gerekir.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Azure Active Directory etki alanı hizmeti sanal ağda etkin olup olmadığını denetleyin

Active Directory etki alanı hizmeti etkin ve sanal ağa bağlı değilse, bu sanal ağ silinemiyor. 

![Bağlı bir aygıt denetleyin](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Hizmetini devre dışı bırakmak için aşağıdaki adımları izleyin:

1. [Klasik Azure portalı](https://manage.windowsazure.com)'na gidin.
2. Sol bölmede seçin **Active Directory**.
3. Active Directory etki alanı hizmeti etkin olan Azure Active Directory (Azure AD) dizini seçin.
4. **Configure (Yapılandır)** sekmesini seçin.
5. Altında **etki alanı Hizmetleri**, değiştirme **bu dizin için etki alanı Hizmetleri'ni etkinleştirme** için seçenek **Hayır**.  

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Sanal ağ diğer kaynağa bağlı olup olmadığını denetleyin

Bağlantı hattı bağlantıları, bağlantıları ve sanal ağ eşlemesi bulunabilir denetleyin. Bunlardan herhangi bir sanal ağ silme başarısız olmasına neden olabilir. 

Önerilen silme sipariş aşağıdaki gibidir:

1. Ağ Geçidi bağlantıları
2. Ağ geçitleri
3. IP'leri
4. Sanal Ağ eşlemesi bulunabilir
5. Uygulama hizmeti ortamı (ana)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Bir sanal makinenin sanal ağda hala çalışıp çalışmadığını denetleyin

Hiçbir sanal makinenin sanal ağ olduğundan emin olun.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Sanal ağ içinde geçiş takıldı olup olmadığını denetleyin

Sanal ağı geçiş durumunda takılıyorsa silinemiyor. Geçiş işlemi iptal etmek için aşağıdaki komutu çalıştırın ve sanal ağ silin.

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Sanal Ağ](virtual-networks-overview.md)
- [Azure Sanal Ağ hakkında sık sorulan sorular (SSS)](virtual-networks-faq.md)