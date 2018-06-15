---
title: Google + Azure Active Directory B2C yapılandırmasında | Microsoft Docs
description: Tüketiciye hesaplarıyla Google + uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5377e4b56bca09a1785d14bfe4c32de01e6db7d3
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711374"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: tüketicileri Google + hesapları ile kaydolma ve oturum açma sağlar
## <a name="create-a-google-application"></a>Bir Google + uygulama oluşturma
Google + Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için bir Google + uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Google + hesabınızın olması gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Git [Google geliştiriciler konsol](https://console.developers.google.com/) ve Google + hesabı kimlik bilgilerinizle oturum açın.
2. Tıklatın **proje oluştur**, girin bir **proje adı**ve ardından **oluşturma**.
   
    ![Google + - kullanmaya başlama](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - yeni proje](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Tıklatın **API Yöneticisi** ve ardından **kimlik bilgileri** sol gezinti bölmesinde.
4. Tıklatın **OAuth izni ekran** üst sekmesini.
   
    ![Google + - kimlik bilgileri](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Seçin veya geçerli bir belirtin **e-posta adresi**, sağlayan bir **ürün adı**, tıklatıp **kaydetmek**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Tıklatın **yeni kimlik bilgileri** ve ardından **OAuth istemci kimliği**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Altında **uygulama türü**seçin **Web uygulaması**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Sağlayan bir **adı** uygulamanız için girin `https://login.microsoftonline.com` içinde **yetkili JavaScript çıkış** alan, ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yetkili URI'ler yeniden yönlendirme** alan. Değiştir **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com). **{Tenant}** duyarlıdır. **Oluştur**’a tıklayın.
   
    ![Google + - istemci kodu oluştur](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Değerleri kopyalamak **istemci kimliği** ve **gizli**. Her ikisi de Google + kimlik sağlayıcısı kiracınızda yapılandırmak için gerekir. **İstemci parolası** önemli güvenlik kimlik bilgileri.
   
    ![Google + - gizli](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Google + kiracınızda kimlik sağlayıcısı yapılandırma
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "G +" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Google**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** ve istemci Kimliğini ve daha önce oluşturduğunuz Google + uygulamasının istemci parolasını girin.
7. Tıklatın **Tamam** ve ardından **oluşturma** Google + yapılandırmanızı kaydetmek için.

