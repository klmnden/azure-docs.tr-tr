---
title: Azure Active Directory B2C Weibo yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan Weibo hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c11931b0b1c568591a89277844722dceeac59039
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711476"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma Weibo hesaplarıyla tüketicileri sağlayın

> [!NOTE]
> Bu özellik önizlemede. Bu kimlik sağlayıcısı, üretim ortamınızda kullanmayın.
> 

## <a name="create-a-weibo-application"></a>Weibo uygulaması oluşturma

Azure Active Directory (Azure AD) B2C içinde kimlik sağlayıcısı Weibo kullanmak için Weibo uygulama oluşturabilir ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Weibo hesabı gerekir. Yoksa, her seferde alabilirsiniz [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Weibo developer program kaydolun

1. Git [Weibo Geliştirici Portalı](http://open.weibo.com/) ve Weibo hesabı kimlik bilgilerinizle oturum açın.
2. Oturum açtıktan sonra görünen adınızı sağ üst köşedeki tıklayın.
3. Açılır listede seçin**编辑开发者信息**(Geliştirici bilgilerini Düzenle).
4. Forma gerekli bilgileri girin ve tıklayın**提交**(Gönder).
5. E-posta doğrulama işlemini tamamlayın.
6. Git [kimlik doğrulama sayfasında](http://open.weibo.com/developers/identity/edit).
7. Forma gerekli bilgileri girin ve tıklayın**提交**(Gönder).

### <a name="register-a-weibo-application"></a>Weibo uygulamayı Kaydet

1. Git [yeni Weibo uygulama kayıt sayfasına](http://open.weibo.com/apps/new).
2. Gerekli uygulama bilgilerini girin.
3. Tıklatın**创建**(Oluştur).
4. Değerleri kopyalamak **uygulama anahtarı** ve **uygulama gizli anahtarı**. Bu daha sonra ihtiyacınız olacak.
5. Gerekli fotoğrafı karşıya ve gerekli bilgileri girin.
6. Tıklatın**保存以上信息**(Kaydet).
7. Tıklatın**高级信息**(Gelişmiş).
8. Tıklatın**编辑**(OAuth2.0 için alanın yanındaki Düzenle)**授权设置**(yeniden yönlendirme URL'si).
9. Girin `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` OAuth2.0 için**授权设置**(yeniden yönlendirme URL'si). Örneğin, varsa, `tenant_name` olan contoso.onmicrosoft.com, URL olması için ayarlama `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Tıklatın**提交**(Gönder).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Kimlik sağlayıcısı kiracınızda Weibo yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Weibo" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Weibo**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklatın **Tamam** ve ardından **oluşturma** Weibo yapılandırmanızı kaydetmek için.

