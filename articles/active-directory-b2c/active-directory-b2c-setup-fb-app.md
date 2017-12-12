---
title: "Azure Active Directory B2C: Facebook yapılandırma | Microsoft Docs"
description: "Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan Facebook hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: mtillman
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 7f551a7ba9b7ddbb373e6b6418ab43fe7ce9be36
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma Facebook hesaplarıyla tüketicileri sağlayın
## <a name="create-a-facebook-application"></a>Bir Facebook uygulaması oluşturma
Facebook kimlik sağlayıcısı Azure Active Directory (Azure AD) B2C içinde kullanmak için bir Facebook uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Facebook hesabı gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [https://www.facebook.com/](https://www.facebook.com/).

1. Git [geliştiriciler için Facebook](https://developers.facebook.com/) hesap kimlik bilgileri Web sitesi ve, Facebook ile oturum açın.
2. Zaten yapmadıysanız, Facebook geliştirici olarak kaydetmeniz gerekir. Bunu yapmak için tıklatın **kaydetmek** (sağ üst köşesinde sayfasının) Facebook'ın ilkeleri kabul edin ve kayıt adımları tamamlayın.
3. Tıklatın **My uygulamaları** ve ardından **yeni bir uygulama eklemek**. 
4. Formda sağlayan bir **görünen adı** ve geçerli bir **ilgili kişi e-posta**.
5. Tıklatın **uygulama kimliği oluşturma**. Bu, Facebook platform ilkeleri kabul etmek ve çevrimiçi güvenlik denetimini tamamlamak gerektirebilir.
6. Sol sütunda tıklatın **ayarları** ve ardından **temel** zaten seçilmemişse.
7. Seçin bir **kategori**. 
8. Tıklatın **+ Platform eklemek** seçip **Web sitesi**.
   
    ![Facebook - ayarlar](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - ayarları - Web sitesi](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. Girin `https://login.microsoftonline.com/` içinde **Site URL'si** alan ve ardından **Değişiklikleri Kaydet** sayfanın sonundaki.
   
    ![Facebook - Site URL'si](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. Değerini kopyalayın **uygulama kimliği**. Tıklatın **Göster** ve değerini kopyalayın **uygulama gizli anahtarı**. Facebook kimlik sağlayıcısı kiracınızda yapılandırmak için ikisinin gerekir. **Uygulama gizli anahtarı** önemli güvenlik kimlik bilgileri.
   
    ![Facebook - uygulama kimliği & uygulama gizli anahtarı](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. Tıklatın **+ ürün ekleme** sol gezinti çubuğunda ve ardından **Ayarla** için düğmesini **Facebook oturum açma**.
   
    ![Facebook - Facebook oturum açma](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. Tıklatın **ayarları** altında sağ nav üzerinde **Facebook oturum açma**

    ![Facebook - Facebook oturum açma ayarları](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. Girin `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **geçerli OAuth yeniden yönlendirme URI'ler** alanındaki **istemci OAuth ayarları** bölümü. Değiştir **{tenant}** , kiracının adlı (örneğin, contosob2c.onmicrosoft.com). Tıklatın **Değişiklikleri Kaydet** sayfanın sonundaki.
    
    ![Facebook - OAuth yeniden yönlendirme URI'si](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. Facebook uygulamanızı Azure AD B2C tarafından kullanılabilir hale getirmek için genel kullanıma açık yapmanız gerekir. Tıklayarak bunu yapabilirsiniz **uygulama İnceleme** sol gezinti çubuğunda ve sayfanın üstünde anahtar kapatarak **Evet** tıklatıp **Onayla**.
    
    ![Facebook - uygulama genel](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebook kimlik sağlayıcısı kiracınızda yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Facebook" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Facebook**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** uygulama kimliği ve uygulama gizli anahtarı (daha önce oluşturduğunuz Facebook uygulaması) girin, **istemci kimliği** ve **gizli** sırasıyla alanları.
7. Tıklatın **Tamam**ve ardından **oluşturma** Facebook yapılandırmanızı kaydetmek için.

> [!NOTE]
> Ekleme bir **kimlik sağlayıcısı** kiracınız için varolan ilkelerinizi değiştirmez. Yeni oluşturduğunuz kimlik sağlayıcısı dahil ederek ilkelerinizi güncelleştirmeyi unutmayın.
>