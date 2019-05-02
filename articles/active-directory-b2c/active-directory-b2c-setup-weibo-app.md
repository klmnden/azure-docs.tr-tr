---
title: Azure Active Directory B2C kullanarak bir Weibo hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda Weibo hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9701d1583a19be46c4c72a82d9f376a8db0c625c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704257"
---
# <a name="set-up-sign-up-and-sign-in-with-a-weibo-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir Weibo hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-weibo-application"></a>Weibo uygulaması oluşturma

Azure Active Directory (Azure AD) B2C'de kimlik sağlayıcısı olarak Weibo hesabı kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. Weibo hesabınız yoksa, adresinden edinebilirsiniz [ https://weibo.com/signup/signup.php?lang=en-us ](https://weibo.com/signup/signup.php?lang=en-us).

1. Oturum [Weibo Geliştirici Portalı](https://open.weibo.com/) Weibo hesabınızın kimlik bilgileriyle.
2. Oturum açtıktan sonra sağ üst köşesinde görünen ad'ınızı seçin.
3. Açılır menüden seçin**编辑开发者信息**(Geliştirici bilgilerini Düzenle).
4. Gerekli bilgileri girin ve seçin**提交**(Gönder).
5. E-posta doğrulama işlemi tamamlayın.
6. Git [kimlik doğrulaması sayfası](https://open.weibo.com/developers/identity/edit).
7. Gerekli bilgileri girin ve seçin**提交**(Gönder).

### <a name="register-a-weibo-application"></a>Weibo uygulamayı kaydetme

1. Git [yeni Weibo uygulaması kayıt sayfasındaki](https://open.weibo.com/apps/new).
2. Gerekli uygulama bilgilerini girin.
3. Seçin**创建**(oluşturma).
4. Değerlerini kopyalamayı **uygulama anahtarı** ve **uygulama gizli anahtarı**. Kiracınız için kimlik sağlayıcısı eklemek için bunların her ikisi de gerekir.
5. Gerekli fotoğrafı karşıya yüklemesine ve gerekli bilgileri girin.
6. Seçin**保存以上信息**(Kaydet).
7. Seçin**高级信息**(Gelişmiş bilgi).
8. Seçin**编辑**(OAuth2.0 için alanın yanındaki Düzenle)**授权设置**(yeniden yönlendirme URL'si).
9. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` OAuth2.0 için**授权设置**(yeniden yönlendirme URL'si). Kiracı adınızın contoso ise, örneğin, olması için URL'yi ayarlayın `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
10. Seçin**提交**(Gönder).  

## <a name="configure-a-weibo-account-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak Weibo hesabı yapılandırın

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *Weibo*.
6. Seçin **kimlik sağlayıcısı türü**seçin **Weibo (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama anahtarı girin **istemci kimliği** olarak kayıtlı uygulama gizli anahtarı girin **gizli** , daha önce oluşturduğunuz Weibo uygulama.
8. Tıklayın **Tamam** ve ardından **Oluştur** Weibo yapılandırmanızı kaydetmek için.
