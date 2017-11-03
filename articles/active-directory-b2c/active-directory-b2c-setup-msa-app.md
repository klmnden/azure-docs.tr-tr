---
title: "Azure Active Directory B2C: Microsoft hesabı yapılandırması | Microsoft Docs"
description: "Uygulamalarınızda Azure Active Directory B2C tarafından güvence altına alınan Microsoft hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 59879dc0b3fc1d7af3e2a1f67f1701f451de9126
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: için tüketicileri Microsoft hesapları ile kaydolma ve oturum açma sağlayın
## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturma
Microsoft hesabı kimlik sağlayıcısı Azure Active Directory (Azure AD) B2C içinde kullanmak için bir Microsoft hesabı uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Microsoft hesabı gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [https://www.live.com/](https://www.live.com/).

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ve Microsoft hesabı kimlik bilgilerinizle oturum açın.
2. Tıklatın **bir uygulama ekleyin**.
   
    ![Microsoft hesabı - yeni bir uygulama ekleme](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Sağlayan bir **adı** tıklatın ve uygulama için **uygulaması oluşturma**.
   
    ![Microsoft hesabı - uygulama adı](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Değerini kopyalayın **uygulama kimliği**. Microsoft hesabı kimlik sağlayıcısı kiracınızda yapılandırmak için gerekir.
   
    ![Microsoft hesabı - uygulama kimliği](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Tıklayın **Ekle platform** ve **Web**.
   
    ![Microsoft hesabı - platform ekleme](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft hesabı - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URI'ler** alan. Değiştir **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com).
   
    ![Microsoft hesabı - yeniden yönlendirme URL'si](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Tıklayın **yeni bir parola oluşturmak** altında **uygulama parolaları** bölümü. Ekranda görüntülenen yeni parolayı kopyalayın. Microsoft hesabı kimlik sağlayıcısı kiracınızda yapılandırmak için gerekir. Bu parolayı bir önemli güvenlik kimlik bilgisidir.
   
    ![Microsoft hesabı - yeni bir parola oluştur](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft hesabı - yeni parola](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. Belirten kutuyu **Live SDK'sı desteği** altında **Gelişmiş Seçenekler** bölümü. **Kaydet** düğmesine tıklayın.
   
    ![Microsoft hesabı - Live SDK'sı desteği](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsoft hesabı kimlik sağlayıcısı kiracınızda yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "MSA" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Microsoft hesabı**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** uygulama kimliği ve daha önce oluşturduğunuz Microsoft hesabı uygulama parolasını girin.
7. Tıklatın **Tamam** ve ardından **oluşturma** Microsoft Hesap yapılandırmanızı kaydetmek için.

