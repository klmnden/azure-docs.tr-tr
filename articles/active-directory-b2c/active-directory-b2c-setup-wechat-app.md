---
title: Azure Active Directory B2C WeChat yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan WeChat hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: bbdeccbdd0d6786fdf32fc2f547344b379bd0d7c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712496"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma WeChat hesaplarıyla tüketicileri sağlayın

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

Azure Active Directory (Azure AD) B2C içinde kimlik sağlayıcısı WeChat kullanmak için WeChat uygulama oluşturabilir ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir WeChat hesabı gerekir. Yoksa, bir mobil uygulamalarını biri aracılığıyla kaydolan veya h numaranızı kullanarak elde edebilirsiniz. Bundan sonra hesabınızı WeChat Geliştirici programla kayıtlı alın. Daha fazla bilgi bulabilirsiniz [burada](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>WeChat uygulamayı Kaydet

1. [https://open.weixin.qq.com/](https://open.weixin.qq.com/) adresine gidin ve oturum açın.
2. Tıklayın**管理中心**(Yönetim Merkezi).
3. Yeni bir uygulama kaydetmek için gerekli adımları uygulayın.
4. İçin**授权回调域**(geri çağırma URL'si) girin `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Örneğin, varsa, `tenant_name` olan contoso.onmicrosoft.com, URL olması için ayarlama `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Bulma ve kopyalama **uygulama kimliği** ve **uygulama anahtarı**. Bu bilgiler daha sonra ihtiyacınız olacaktır.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>Kimlik sağlayıcısı kiracınızda WeChat yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "WeChat" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **WeChat**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklatın **Tamam** ve ardından **oluşturma** WeChat yapılandırmanızı kaydetmek için.

