---
title: Güvenli Azure VPN ağ geçidi için RADIUS kimlik doğrulaması NPS sunucusu ile çok faktörlü kimlik doğrulaması | Microsoft Docs
description: Açıklar Azure ağ geçidi RADIUS kimlik doğrulaması, çok faktörlü kimlik doğrulaması için NPS sunucusu ile tümleştirme.
services: vpn-gateway
documentationcenter: na
author: ahmadnyasin
manager: willchen
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/13/2018
ms.author: genli
ms.openlocfilehash: c9985f6ad8721460e973d3c43f1f035506ae697c
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100083"
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Azure VPN ağ geçidi RADIUS kimlik doğrulaması, çok faktörlü kimlik doğrulaması için NPS sunucusu ile tümleştirme 

Makale, ağ ilkesi sunucusu (NPS), noktadan siteye VPN bağlantıları için çok faktörlü kimlik doğrulama (MFA) sunmak için Azure VPN ağ geçidi RADIUS kimlik doğrulaması ile tümleştirmek açıklar. 

## <a name="prerequisite"></a>Önkoşul

MFA'yı etkinleştirmek için kullanıcıların Azure Active ya da şirket içi eşitlenen veya ortam bulut Directory (Azure AD), olması gerekir. Ayrıca, kullanıcı zaten otomatik kayıt işlemini MFA için tamamlamış olmanız gerekir.  Daha fazla bilgi için bkz: [Hesabımı iki aşamalı doğrulama için ayarlama](../active-directory/authentication/end-user/current/multi-factor-authentication-end-user-first-time.md)

## <a name="detailed-steps"></a>Ayrıntılı adımlar

### <a name="step-1-create-a-virtual-network-gateway"></a>1. adım: bir sanal ağ geçidi oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sanal ağ geçidi barındıracak sanal ağında seçin **alt ağlar**ve ardından **ağ geçidi alt ağı** bir alt ağ oluşturmak için. 

    ![Görüntünün ağ geçidi alt ağı ekleme hakkında](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Aşağıdaki ayarları belirterek bir sanal ağ geçidi oluşturun:

    - **Ağ geçidi türü**: **VPN**’i seçin.
    - **VPN türü**: seçin **rota tabanlı**.
    - **SKU**: SKU tür gereksinimlerinize göre seçin.
    - **Sanal ağ**: ağ geçidi alt ağı oluşturulan sanal ağı seçin.

        ![Görüntünün sanal ağ geçidi ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>2. adım, Azure MFA için NPS yapılandırma

1. NPS sunucusunda [Azure MFA için NPS uzantıyı yüklemek](../active-directory/authentication/howto-mfa-nps-extension.md#install-the-nps-extension).
2. NSP konsolunu açın, sağ **RADUIS istemcileri**ve ardından **yeni**. Aşağıdaki ayarları belirterek RADUIS istemci oluşturun:

    - **Kolay ad**: herhangi bir ad yazın.
    - **Adresi (IP veya DNS)**: 1. adımda oluşturduğunuz ağ geçidi alt ağı yazın.
    - **Paylaşılan gizliliği**: tüm gizli anahtarı yazın ve daha sonra kullanmak üzere unutmayın.

    ![Görüntünün RADUIS istemci ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  Üzerinde **Gelişmiş** sekmesinde, satıcı adı ayarlamak **RADIUS standart** emin olun **ek seçenekler** onay kutusu seçilmez.

    ![Görüntünün RADUIS istemci Gelişmiş ayarları hakkında](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Gidin **ilkeleri** > **ağ ilkeleri**, çift **Microsoft Routing ve Uzaktan erişim sunucusuna bağlantılar** İlkesi, select  **Erişim izni verme**ve ardından **Tamam**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>3. adım yapılandırma sanal ağ geçidi

1. Oturum [Azure portal](https://portal.azure.com).
2. Oluşturduğunuz sanal ağ geçidi'ni açın. Ağ geçidi türü olarak ayarlandığından emin olun **VPN** ve VPN türü olan **rota tabanlı**.
3. Tıklatın **noktası site yapılandırması** > **şimdi yapılandırmak**ve ardından aşağıdaki ayarları belirtin:

    - **Adres havuzu**: 1. adımda oluşturduğunuz ağ geçidi alt ağı yazın.
    - **Kimlik doğrulama türü**: seçin **RADIUS kimlik doğrulaması**.
    - **Sunucu IP adresi**: NPS sunucusunun IP adresini yazın.

    ![Görüntünün site ayarlarına noktası hakkında](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure çok faktörlü kimlik doğrulaması](../active-directory/authentication/multi-factor-authentication.md)
- [Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme](../active-directory/authentication/howto-mfa-nps-extension.md)
