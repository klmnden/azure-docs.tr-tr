---
title: Azure Active Directory B2C kullanarak bir h hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda h hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 6eba804fc96a91d17644c903e1462c31c0fc9149
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64704173"
---
# <a name="set-up-sign-up-and-sign-in-with-a-qq-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir h hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-qq-application"></a>H uygulaması oluşturma

H hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. H hesabınız yoksa, adresinden edinebilirsiniz [ https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033 ](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).

### <a name="register-for-the-qq-developer-program"></a>H Geliştirici programına kaydolun

1. Oturum [h Geliştirici Portalı](http://open.qq.com) h hesabınızın kimlik bilgileriyle.
2. Oturum açtıktan sonra Git [ http://open.qq.com/reg ](http://open.qq.com/reg) kendiniz bir geliştirici olarak kaydedilecek.
3. Seçin**个人**(bireysel Geliştirici).
4. Gerekli bilgileri girin ve seçin**下一步**(sonraki adım).
5. E-posta doğrulama işlemi tamamlayın. Bir geliştirici olarak kaydedildikten sonra onaylanması birkaç gün beklemeniz gerekir. 

### <a name="register-a-qq-application"></a>H uygulamayı kaydetme

1. [https://connect.qq.com/index.html](https://connect.qq.com/index.html) kısmına gidin.
2. Seçin**应用管理**(Uygulama Yönetimi).
5. Seçin**创建应用**(Uygulama Oluştur) ve gerekli bilgileri girin.
7. Girin `https://your-tenant-name.b2clogin.com/your-tenant-name}.onmicrosoft.com/oauth2/authresp` içinde**授权回调域**(geri çağırma URL'si). Örneğin, varsa, `tenant_name` olan contoso, olması için URL'yi ayarlayın `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
8. Seçin**创建应用**(uygulama oluşturma).
9. Onay sayfasında, seçin**应用管理**(uygulama yönetimi sayfasına geri dönmek için uygulama yönetimi).
10. Seçin**查看**(görüntüleme), oluşturduğunuz uygulamanın yanında.
11. Seçin**修改**(Düzenle).
12. Kopyalama **uygulama kimliği** ve **uygulama anahtarı**. Kiracınız için kimlik sağlayıcısı eklemek için bu değerleri her ikisi gerekir.

## <a name="configure-qq-as-an-identity-provider"></a>Kimlik sağlayıcısı olarak h yapılandırın

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *h*.
6. Seçin **kimlik sağlayıcısı türü**seçin **h (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği** olarak kayıtlı uygulama anahtarı girin **gizli** h, daha önce oluşturduğunuz uygulama.
8. Tıklayın **Tamam** ve ardından **Oluştur** h yapılandırmanızı kaydetmek için.

