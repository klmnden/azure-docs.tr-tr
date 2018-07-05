---
title: Azure Active Directory B2C h yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan h hesaplarıyla tüketicilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1f9a0f56158f08dd3b22078f111c9ec6911b726c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444438"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-qq-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere h hesapları sağlayın

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-qq-application"></a>H uygulaması oluşturma

H bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için h uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için h hesabına ihtiyacınız var. Yoksa, tek tek alabilirsiniz [ https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033 ](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>H Geliştirici programına kaydolun

1. Git [h Geliştirici Portalı](http://open.qq.com) ve h hesabı kimlik bilgilerinizle oturum açın.
2. Oturum açtıktan sonra Git [ http://open.qq.com/reg ](http://open.qq.com/reg) kendiniz bir geliştirici olarak kaydedilecek.
3. Menüde**个人**(bireysel Geliştirici).
4. Forma gerekli bilgileri girin ve tıklayın**下一步**(sonraki adım).
5. E-posta doğrulama işlemi tamamlayın.

> [!NOTE]
> Bir geliştirici olarak kaydedildikten sonra onaylanması birkaç gün beklemeniz gerekir. 

### <a name="register-a-qq-application"></a>H uygulamayı kaydetme

1. [https://connect.qq.com/index.html](https://connect.qq.com/index.html) kısmına gidin.
2. Tıklayarak**应用管理**(Uygulama Yönetimi).
3. Tıklayarak**创建应用**(uygulama oluşturma).
4. Gerekli uygulama bilgilerini girin.
5. Tıklayarak**创建应用**(uygulama oluşturma).
6. Gerekli bilgileri girin.
7. İçin**授权回调域**(geri çağırma URL'si) alanına `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Örneğin, varsa, `tenant_name` olduğundan, contoso.onmicrosoft.com olması için URL'yi ayarlayın `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
8. Tıklayarak**创建应用**(uygulama oluşturma).
9. Onay sayfasındaki tıklayarak**应用管理**(uygulama yönetimi sayfasına geri dönmek için uygulama yönetimi).
10. Tıklayarak**查看**(görüntüleme) yeni oluşturduğunuz uygulamayı yanında.
11. Tıklayarak**修改**(Düzenle).
12. Sayfanın üst kısmından kopyalama **uygulama kimliği** ve **uygulama anahtarı**.

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a>Kiracınızdaki bir kimlik sağlayıcısı olarak h yapılandırın
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "H" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **h**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklayın **Tamam** ve ardından **Oluştur** h yapılandırmanızı kaydetmek için.

