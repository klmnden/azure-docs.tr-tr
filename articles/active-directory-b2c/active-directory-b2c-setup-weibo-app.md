---
title: Azure Active Directory B2C Weibo yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan Weibo hesaplarıyla tüketicilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bfd7dde290bd040f8457e6d095fdf896e802764b
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444798"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere Weibo hesapları sağlayın

> [!NOTE]
> Bu özellik önizlemede. Bu kimlik sağlayıcısı, üretim ortamında kullanmayın.
> 

## <a name="create-a-weibo-application"></a>Weibo uygulaması oluşturma

Weibo bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için bir Weibo uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için Weibo hesabına ihtiyacınız var. Yoksa, tek tek alabilirsiniz [ http://weibo.com/signup/signup.php?lang=en-us ](http://weibo.com/signup/signup.php?lang=en-us).

### <a name="register-for-the-weibo-developer-program"></a>Weibo Geliştirici programına kaydolun

1. Git [Weibo Geliştirici Portalı](http://open.weibo.com/) ve Weibo hesabı kimlik bilgilerinizle oturum açın.
2. Oturum açtıktan sonra sağ üst köşesinde görünen ad'ınızı tıklayın.
3. Açılır menüden seçin**编辑开发者信息**(Geliştirici bilgilerini Düzenle).
4. Forma gerekli bilgileri girin ve tıklayın**提交**(Gönder).
5. E-posta doğrulama işlemi tamamlayın.
6. Git [kimlik doğrulaması sayfası](http://open.weibo.com/developers/identity/edit).
7. Forma gerekli bilgileri girin ve tıklayın**提交**(Gönder).

### <a name="register-a-weibo-application"></a>Weibo uygulamayı kaydetme

1. Git [yeni Weibo uygulaması kayıt sayfasındaki](http://open.weibo.com/apps/new).
2. Gerekli uygulama bilgilerini girin.
3. Tıklayın**创建**(oluşturma).
4. Değerlerini kopyalamayı **uygulama anahtarı** ve **uygulama gizli anahtarı**. Bu daha sonra gerekecektir.
5. Gerekli fotoğrafı karşıya yüklemesine ve gerekli bilgileri girin.
6. Tıklayın**保存以上信息**(Kaydet).
7. Tıklayın**高级信息**(Gelişmiş bilgi).
8. Tıklayın**编辑**(OAuth2.0 için alanın yanındaki Düzenle)**授权设置**(yeniden yönlendirme URL'si).
9. Girin `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` OAuth2.0 için**授权设置**(yeniden yönlendirme URL'si). Örneğin, varsa, `tenant_name` olduğundan, contoso.onmicrosoft.com olması için URL'yi ayarlayın `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
10. Tıklayın**提交**(Gönder).  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak Weibo yapılandırın
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Weibo" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **Weibo**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklayın **Tamam** ve ardından **Oluştur** Weibo yapılandırmanızı kaydetmek için.

