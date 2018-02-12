---
title: "Güvenli Azure VPN ağ geçidi için RADIUS kimlik doğrulaması NPS sunucusu ile çok faktörlü kimlik doğrulaması | Microsoft Docs"
description: "Açıklar Azure ağ geçidi RADIUS kimlik doğrulaması, çok faktörlü kimlik doğrulaması için NPS sunucusu ile tümleştirme."
services: vpn-gateway
documentationcenter: na
author: genlin
manager: willchen
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: genli
ms.openlocfilehash: 0754c6cab9de2f93e9a57e202227a81c51bedf4c
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Azure VPN ağ geçidi RADIUS kimlik doğrulaması, çok faktörlü kimlik doğrulaması için NPS sunucusu ile tümleştirme 

Makale, ağ ilkesi sunucusu (NPS) sunucu noktadan siteye VPN bağlantıları için çok faktörlü kimlik doğrulama (MFA) sunmak için Azure VPN ağ geçidi RADIUS kimlik doğrulaması ile tümleştirmek açıklar. 

## <a name="prerequisite"></a>Önkoşul

MFA'yı etkinleştirmek için kullanıcıların Azure Active Directory (şirket içi veya yalnızca bulutta eşitlenmiş) olmalıdır ve kullanıcı zaten MFA için sağlama işlemini tamamlamalısınız.  Kullanıcılar, https://myapps.microsoft.com kullanarak sağlama işlemini tamamlayabilirsiniz.

## <a name="detailed-steps"></a>Ayrıntılı adımlar

### <a name="step-1-create-virtual-network-gateway"></a>Adım 1'sanal ağ geçidi oluştur

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Sanal ağ geçidi barındıracak sanal ağında seçin **alt ağlar**ve ardından **ağ geçidi alt ağı** bir alt ağ oluşturmak için. 

    ![Görüntünün ağ geçidi alt ağı ekleme hakkında](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Aşağıdaki ayarlara sahip bir sanal ağ geçidi oluşturun:

    - **Ağ geçidi türü**: seçin **VPN**.
    - **VPN türü**: seçin **rota tabanlı**.
    - **SKU**: SKU tür gereksinimlerinize göre seçin.
    - **Sanal ağ**: ağ geçidi alt ağı oluşturduğunuz sanal ağı seçin.

        ![Görüntünün sanal ağ geçidi ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>2. adım, Azure MFA için NPS yapılandırma

1. NPS sunucusunda [Azure MFA için NPS uzantıyı yüklemek](../multi-factor-authentication/multi-factor-authentication-nps-extension.md#install-the-nps-extension).
2. NSP konsolu açın, sağ **RADUIS istemcileri**seçin **yeni**. Aşağıdaki ayarlarla bir RADUIS istemci oluşturun:

    - **Kolay ad**: herhangi bir ad yazın.
    - **Adresi (IP veya DNS)**: 1. adımda oluşturduğunuz ağ geçidi alt ağı yazın.
    - **Paylaşılan gizliliği**: tüm parolayı yazın ve unutmayın. Daha sonra kullanacağız.

    ![Görüntünün RADUIS istemci ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  İçinde **Gelişmiş** sekmesinde, satıcı adı ayarlamak **RADIUS standart** emin olun **ek seçenekler** seçilmemiş.

    ![Görüntünün RADUIS istemci Gelişmiş ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Gidin **ilkeleri** > **ağ ilkeleri**, çift **Microsoft Routing ve Uzaktan erişim sunucusuna bağlantılar** İlkesi, select  **Erişim izni verme**ve ardından **Tamam**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>3. adım yapılandırma sanal ağ geçidi

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
2. Açık, oluşturduğunuz sanal ağ geçidi ağ geçidi türü olduğundan emin olun **VPN** ve VPN türü **rota tabanlı**.
3. Tıklatın **noktası site yapılandırması** > **şimdi yapılandırmak**ve ardından aşağıdaki ayarları ekleyin:

    - **Adres havuzu**: 1. adımda oluşturduğunuz ağ geçidi alt ağı yazın.
    - **Kimlik doğrulama türü**: seçin **RADIUS kimlik doğrulaması**.
    - **Sunucu IP adresi**: NPS sunucusunun IP adresini yazın.

    ![Görüntünün site ayarlarına noktası hakkında](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
- [Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)
