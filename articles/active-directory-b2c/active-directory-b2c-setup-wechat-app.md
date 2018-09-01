---
title: Azure Active Directory B2C kullanarak bir WeChat hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda WeChat hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 418f399213f2b1b60d4d4f464fd9fe89c6f35ba7
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338216"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir WeChat hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

WeChat hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. WeChat hesabınız yoksa, bilgileri alabileceğiniz [ http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html ](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>WeChat uygulamayı kaydetme

1. Oturum [ https://open.weixin.qq.com/ ](https://open.weixin.qq.com/) WeChat kimlik bilgilerinizle.
2. Seçin**管理中心**(Yönetim Merkezi).
3. Yeni bir uygulama kaydetmek için adımları izleyin.
4. Girin `https://{tenant_name}.b2clogin.com/te/{tenant_name}.onmicrosoft.com/oauth2/authresp` içinde**授权回调域**(geri çağırma URL'si). Örneğin, varsa, `tenant_name` olan contoso, olması için URL'yi ayarlayın `https://contoso.b2clogin.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Kopyalama **uygulama kimliği** ve **uygulama anahtarı**. Kiracınız için kimlik sağlayıcısı eklemek için bunlar gerekli olacaktır.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırma

1. Oturum [Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel Yöneticisi olarak.
2. Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olmak için Azure portalın sağ üst köşesinde bu dizine geçin. Abonelik bilgilerinizi ve ardından **Dizin Değiştir**’i seçin. 

    ![Azure AD B2C kiracınıza geçiş yapma](./media/active-directory-b2c-setup-wechat-app/switch-directories.png)

    Kiracınızı içeren dizini seçin.

    ![Dizin seçme](./media/active-directory-b2c-setup-wechat-app/select-directory.png)

3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *WeChat*.
6. Seçin **kimlik sağlayıcısı türü**seçin **WeChat (Önizleme)**, tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği** olarak kayıtlı uygulama anahtarı girin **gizli** , Daha önce oluşturduğunuz WeChat uygulama.
8. Tıklayın **Tamam** ve ardından **Oluştur** WeChat yapılandırmanızı kaydetmek için.

