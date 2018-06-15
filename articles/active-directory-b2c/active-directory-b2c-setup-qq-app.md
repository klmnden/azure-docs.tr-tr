---
title: Azure Active Directory B2C h yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan h hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7a33a1b2a68b82b1d65b1187547695cccd7c395f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711680"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma h hesaplarıyla tüketicileri sağlayın

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-qq-application"></a>H uygulaması oluşturma

H Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için h uygulaması oluşturmak ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir h hesabı gerekir. Yoksa, her seferde alabilirsiniz [ https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033 ](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>H developer program kaydolun

1. Git [h Geliştirici Portalı](http://open.qq.com) ve h hesabı kimlik bilgilerinizle oturum açın.
2. Oturum açtıktan sonra Git [ http://open.qq.com/reg ](http://open.qq.com/reg) kendiniz geliştirici olarak kaydetmek için.
3. Menüde seçin**个人**(bireysel Geliştirici).
4. Forma gerekli bilgileri girin ve tıklayın**下一步**(sonraki adım).
5. E-posta doğrulama işlemini tamamlayın.

> [!NOTE]
> Geliştirici olarak kaydolduktan sonra onaylanması birkaç gün beklemeniz gerekir. 

### <a name="register-a-qq-application"></a>H uygulamayı Kaydet

1. [https://connect.qq.com/index.html](https://connect.qq.com/index.html) kısmına gidin.
2. Tıklayın**应用管理**(Uygulama Yönetimi).
3. Tıklayın**创建应用**(Uygulama Oluştur).
4. Gerekli uygulama bilgilerini girin.
5. Tıklayın**创建应用**(Uygulama Oluştur).
6. Gerekli bilgileri girin.
7. İçin**授权回调域**(geri çağırma URL'si) alanına, `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Örneğin, varsa, `tenant_name` olan contoso.onmicrosoft.com, URL olması için ayarlama `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Tıklayın**创建应用**(Uygulama Oluştur).
9. Onay sayfasında tıklayın**应用管理**(uygulama yönetimi sayfasına dönmek için uygulama yönetimi).
10. Tıklayın**查看**(görüntüleme) az önce oluşturduğunuz uygulama yanındaki.
11. Tıklayın**修改**(Düzenle).
12. Sayfanın üst kısmından kopyalama **uygulama kimliği** ve **uygulama anahtarı**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Kimlik sağlayıcısı kiracınızda h yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "H" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **h**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklatın **Tamam** ve ardından **oluşturma** h yapılandırmanızı kaydetmek için.

