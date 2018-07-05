---
title: Azure Active Directory B2C'de Microsoft hesabı yapılandırma | Microsoft Docs
description: Tüketicilere uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan Microsoft hesapları ile kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c788b14a99125a208390cd4f8ead338efed06933
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444176"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere Microsoft hesapları sağlayın
## <a name="create-a-microsoft-account-application"></a>Bir Microsoft hesabı uygulaması oluşturun
Microsoft hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için bir Microsoft hesabı uygulaması oluşturun ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için bir Microsoft hesabı gerekir. Yoksa, adresinden edinebilirsiniz [ https://www.live.com/ ](https://www.live.com/).

1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ve Microsoft hesabı kimlik bilgilerinizle oturum açın.
2. Tıklayın **uygulama ekleme**.
   
    ![Microsoft hesabı - yeni bir uygulama ekleme](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. Sağlayan bir **adı** tıklayın ve uygulama için **uygulama oluşturma**.
   
    ![Microsoft hesabı - uygulama adı](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. Değerini kopyalayın **uygulama kimliği**. Kiracınızdaki bir kimlik sağlayıcısı olarak Microsoft hesabı yapılandırmak için gerekir.
   
    ![Microsoft hesabı - uygulama kimliği](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. Tıklayarak **Ekle platform** ve **Web**.
   
    ![Microsoft hesabı - platformu Ekle](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft hesabı - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yeniden yönlendirme URI'leri** alan. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile.
   
    ![Microsoft hesabı - yeniden yönlendirme URL'si](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. Tıklayarak **yeni parola oluştur** altında **uygulama gizli dizilerini** bölümü. Ekranda görüntülenen yeni parolayı kopyalayın. Kiracınızdaki bir kimlik sağlayıcısı olarak Microsoft hesabı yapılandırmak için gerekir. Bu parola, bir önemli güvenlik kimlik bilgisidir.
   
    ![Microsoft hesabı - yeni parola oluşturma](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft hesabı - yeni parola](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. İfadesini içeren kutuyu **Canlı SDK desteği** altında **Gelişmiş Seçenekler** bölümü. **Kaydet**’e tıklayın.
   
    ![Microsoft hesabı - Canlı SDK desteği](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak Microsoft hesabı yapılandırın
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "MSA" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **Microsoft hesabı**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama** uygulama kimliği ve daha önce oluşturduğunuz Microsoft hesabı uygulama parolasını girin.
7. Tıklayın **Tamam** ve ardından **Oluştur** Microsoft hesabı yapılandırmanızı kaydetmek için.

