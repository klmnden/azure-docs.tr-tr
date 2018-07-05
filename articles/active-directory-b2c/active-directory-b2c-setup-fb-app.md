---
title: Azure Active Directory B2C Facebook yapılandırmasında | Microsoft Docs
description: Tüketicilere Facebook hesapları Azure Active Directory B2C tarafından güvence altına alınan uygulamalarınızdaki ile kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 8/7/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 316e44ea92a25ab804c8cc499f91c45e4a66ef02
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445509"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere Facebook hesapları sağlayın
## <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturun
Facebook kimlik sağlayıcısı olarak Azure Active Directory (Azure AD) B2C'yi kullanmak için bir Facebook uygulaması oluşturun ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için Facebook hesabına ihtiyacınız var. Yoksa, adresinden edinebilirsiniz [ https://www.facebook.com/ ](https://www.facebook.com/).

1. Git [geliştiriciler için Facebook](https://developers.facebook.com/) hesap kimlik bilgileri Web sitesi ve uygulamanızı Facebook oturum açın.
2. Zaten yapmadıysanız, Facebook geliştiricisi olarak kaydolma gerekir. Bunu yapmak için tıklatın **kaydetme** (sağ üst köşesinde), Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
3. Tıklayın **uygulamalarım** ve ardından **yeni uygulama Ekle**. 
4. Formda sağlayan bir **görünen ad** ve geçerli bir **ilgili kişi e-posta**.
5. Tıklayın **uygulama kimliği oluşturma**. Bu, Facebook platform ilkeleri kabul edin ve çevrimiçi güvenlik denetimini Tamamla gerektirebilir.
6. Sol sütunda tıklayın **ayarları** seçip **temel** zaten seçili değil.
7. Seçin bir **kategori**. 
8. Tıklayın **+ Platform Ekle** seçip **Web sitesi**.
   
    ![Facebook - ayarlar](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - Settings - Web sitesi](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Girin `https://login.microsoftonline.com/` içinde **Site URL'si** alan ve ardından **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
   
    ![Facebook - Site URL'si](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Değerini kopyalayın **uygulama kimliği**. Tıklayın **Göster** ve değerini kopyalayın **uygulama gizli anahtarı**. Her ikisi de Facebook kiracınızdaki bir kimlik sağlayıcısı yapılandırmak için gerekir. **Uygulama gizli anahtarı** bir önemli güvenlik kimlik bilgisidir.
   
    ![Facebook - uygulama kimliği ve uygulama gizli anahtarı](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Tıklayın **+ Ürün Ekle** sol gezinti çubuğunda ve ardından **Ayarla** için düğme **Facebook oturum açma**.
   
    ![Facebook - Facebook oturum açma](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Tıklayın **ayarları** altında sağ gezinti çubuğunda **Facebook oturum açma**

    ![Facebook - Facebook oturum açma ayarları](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **geçerli OAuth yeniden yönlendirme URI'leri** alanındaki **istemci OAuth ayarları** bölümü. Değiştirin **{tenant}** kiracınızın adı (örneğin, contosob2c.onmicrosoft.com) ile. Tıklayın **Değişiklikleri Kaydet** sayfanın alt kısmındaki.
    
    ![Facebook - OAuth yeniden yönlendirme URI'si](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. Facebook uygulamanızı Azure AD B2C tarafından kullanılabilir hale getirmek için genel olarak kullanılabilir hale getirmek gerekir. Tıklayarak bunu yapabilirsiniz **uygulama incelemesi** sol gezinti çubuğunda ve anahtar için sayfanın üst kısmındaki kapatarak **Evet** tıklayıp **Onayla**.
    
    ![Facebook - uygulama genel](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook kimlik sağlayıcısı kiracınızdaki yapılandırın
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Facebook" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **Facebook**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama** uygulama kimliği ve uygulama gizli anahtarı (uygulamanızın daha önce oluşturduğunuz Facebook) girin, **istemci kimliği** ve **gizli** Sırasıyla alanları.
7. Tıklayın **Tamam**ve ardından **Oluştur** Facebook yapılandırmanızı kaydetmek için.

> [!NOTE]
> Ekleme bir **kimlik sağlayıcısı** kiracınıza mevcut ilkelerinizi değiştirmez. Yeni oluşturduğunuz kimlik sağlayıcısı dahil ederek ilkelerinizi güncelleştirmeyi unutmayın.
>