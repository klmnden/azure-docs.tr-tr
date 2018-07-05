---
title: Google + Azure Active Directory B2C'yi yapılandırma | Microsoft Docs
description: Tüketicilere uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan hesapları Google + ile kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: fe5722e8d5a0a8a5bf172577777ccb899fb7b94d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443435"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere Google + hesapları sağlayın
## <a name="create-a-google-application"></a>Google + uygulama oluşturma
Google + Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak kullanmak için Google + uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için bir Google + hesabınız olması. Yoksa, adresinden edinebilirsiniz [ https://accounts.google.com/SignUp ](https://accounts.google.com/SignUp).

1. Git [Google geliştiriciler konsol](https://console.developers.google.com/) ve Google + hesabı kimlik bilgilerinizle oturum açın.
2. Tıklayın **proje oluştur**, girin bir **proje adı**ve ardından **Oluştur**.
   
    ![Google + - kullanmaya başlayın](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - yeni proje](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. Tıklayın **API Yöneticisi** ve ardından **kimlik bilgilerini** sol gezinti bölmesinde.
4. Tıklayın **OAuth onay ekranı** en üstteki sekmedeki.
   
    ![Google + - kimlik bilgileri](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. Seçin veya geçerli **e-posta adresi**, sağlayan bir **ürün adı**, tıklatıp **Kaydet**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. Tıklayın **yeni kimlik bilgileri** seçip **OAuth istemcisi kimliği**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. Altında **uygulama türü**seçin **Web uygulaması**.
   
    ![Google + - OAuth onay ekranı](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. Sağlayan bir **adı** uygulamanız için girin `https://login.microsoftonline.com` içinde **yetkili JavaScript kaynakları** alan ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **yetkili yeniden yönlendirme URI'leri** alan. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile. **{Tenant}** duyarlıdır. **Oluştur**’a tıklayın.
   
    ![Google + - istemci kodu oluşturma](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. Değerlerini kopyalamayı **istemci kimliği** ve **gizli**. Her ikisi de Google + kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırmak için gerekir. **İstemci gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
   
    ![Google + - gizli](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Google + kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırma
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "G +" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **Google**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama** istemci kimliği ve daha önce oluşturduğunuz Google + uygulamasının istemci gizli anahtarını girin.
7. Tıklayın **Tamam** ve ardından **Oluştur** Google + yapılandırmanızı kaydetmek için.

