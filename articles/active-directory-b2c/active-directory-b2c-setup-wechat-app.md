---
title: Azure Active Directory B2C kullanarak bir WeChat hesabı ile kaydolma ve oturum açma ayarlama | Microsoft Docs
description: Azure Active Directory B2C kullanarak uygulamalarınızda WeChat hesaplar kullanan müşteriler için kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: aa44edcf009d381894a581172ea5edffefe946a0
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66508132"
---
# <a name="set-up-sign-up-and-sign-in-with-a-wechat-account-using-azure-active-directory-b2c"></a>Azure Active Directory B2C kullanarak bir WeChat hesabı ile kaydolma ve oturum açma ayarlama

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

WeChat hesabı bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için kiracınızda temsil ettiği bir uygulama oluşturmak gerekir. WeChat hesabınız yoksa, bilgileri alabileceğiniz [ https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html ](https://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>WeChat uygulamayı kaydetme

1. Oturum [ https://open.weixin.qq.com/ ](https://open.weixin.qq.com/) WeChat kimlik bilgilerinizle.
2. Seçin**管理中心**(Yönetim Merkezi).
3. Yeni bir uygulama kaydetmek için adımları izleyin.
4. Girin `https://your-tenant_name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` içinde**授权回调域**(geri çağırma URL'si). Kiracı adınızın contoso ise, örneğin, olması için URL'yi ayarlayın `https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp`.
5. Kopyalama **uygulama kimliği** ve **uygulama anahtarı**. Kiracınız için kimlik sağlayıcısı eklemek için bunlar gerekli olacaktır.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırma

1. [Azure portalda](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portalın sol üst köşesinde **Tüm hizmetler**’i seçin ve **Azure AD B2C**’yi arayıp seçin.
4. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
5. Sağlayan bir **adı**. Örneğin, *WeChat*.
6. Seçin **kimlik sağlayıcısı türü**seçin **WeChat (Önizleme)** , tıklatıp **Tamam**.
7. Seçin **bu kimlik sağlayıcısını ayarlama** olarak daha önce kaydedilen uygulama kimliği girin **istemci kimliği** olarak kayıtlı uygulama anahtarı girin **gizli** , Daha önce oluşturduğunuz WeChat uygulama.
8. Tıklayın **Tamam** ve ardından **Oluştur** WeChat yapılandırmanızı kaydetmek için.

