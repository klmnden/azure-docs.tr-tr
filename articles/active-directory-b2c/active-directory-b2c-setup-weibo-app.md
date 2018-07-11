---
title: Azure Active Directory B2C kullanarak bir Weibo hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Weibo hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2534e3e39d4a6dd1659dced5a1b0342798c0049e
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951555"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Weibo hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-weibo-application"></a>Weibo uygulaması oluşturma

Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak Weibo hesabı kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Weibo hesabınız yoksa, adresinden edinebilirsiniz [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

1. Oturum [Weibo Geliştirici Portalı](http://open.weibo.com/) Weibo hesabınızın kimlik bilgileriyle.
2. Oturum açtıktan sonra sağ üst köşesinde görünen ad'ınızı seçin.
3. Açılır menüden seçin**编辑开发者信息**(Geliştirici bilgilerini Düzenle).
4. Gerekli bilgileri girin ve seçin**提交**(Gönder).
5. E-posta doğrulama işlemi tamamlayın.
6. Git [kimlik doğrulaması sayfası](http://open.weibo.com/developers/identity/edit).
7. Gerekli bilgileri girin ve seçin**提交**(Gönder).

### <a name="register-a-weibo-application"></a>Weibo uygulamayı kaydetme

1. Git [yeni Weibo uygulaması kayıt sayfasındaki](http://open.weibo.com/apps/new).
2. Gerekli uygulama bilgilerini girin.
3. Seçin**创建**(oluşturma).
4. Değerlerini kopyalamayı **uygulama anahtarı** ve **uygulama gizli anahtarı**. Kiracınız için kimlik sağlayıcısı eklemek için bunların her ikisi de gerekir.
5. Gerekli fotoğrafı karşıya yüklemesine ve gerekli bilgileri girin.
6. Seçin**保存以上信息**(Kaydet).
7. Seçin**高级信息**(Gelişmiş bilgi).
8. Seçin**编辑**(OAuth2.0 için alanın yanındaki Düzenle)**授权设置**(yeniden yönlendirme URL'si).
9. Girin `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` OAuth2.0 için**授权设置**(yeniden yönlendirme URL'si). Örneğin, varsa, `tenant_name` olduğundan, contoso.onmicrosoft.com olması için URL'yi ayarlayın `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Seçin**提交**(Gönder).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Weibo hesabı yapılandırın

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-weibo-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-weibo-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Weibo*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Weibo (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama anahtarı girin **istemci kimliği** olarak kayıtlı uygulama gizli anahtarı girin **gizli** , daha önce oluşturduğunuz Weibo uygulama.
8. Tıklayın **Tamam** ve ardından **Oluştur** Weibo yapılandırmanızı kaydetmek için.
