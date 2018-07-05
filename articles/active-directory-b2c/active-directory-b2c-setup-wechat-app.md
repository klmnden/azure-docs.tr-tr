---
title: Azure Active Directory B2C WeChat yapılandırmasında | Microsoft Docs
description: Uygulamalarınızda Azure Active Directory B2C ile güvenliği sağlanan WeChat hesaplarıyla tüketicilere kaydolma ve oturum açma sağlar.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 3/26/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: a18d41a4f45b147790a17664156659d282e710d4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37445981"
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma tüketicilere WeChat hesapları sağlayın

> [!NOTE]
> Bu özellik önizlemede.
> 

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

WeChat bir Azure Active Directory (Azure AD) B2C kimlik sağlayıcısı olarak kullanmak için bir WeChat uygulama oluşturabilir ve doğru parametreleri sağlamanız gerekir. Bunu yapmak için WeChat hesabına ihtiyacınız var. Yoksa, bir mobil uygulamalarını biri aracılığıyla kaydolmadan veya h numaranızı kullanarak alabilirsiniz. Bundan sonra WeChat Geliştirici programına kayıtlı hesabınızı edinin. Daha fazla bilgi bulabilirsiniz [burada](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>WeChat uygulamayı kaydetme

1. [https://open.weixin.qq.com/](https://open.weixin.qq.com/) adresine gidin ve oturum açın.
2. Tıklayarak**管理中心**(Yönetim Merkezi).
3. Yeni bir uygulamayı kaydetmek için gerekli adımları izleyin.
4. İçin**授权回调域**(geri çağırma URL'si) girin `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`. Örneğin, varsa, `tenant_name` olduğundan, contoso.onmicrosoft.com olması için URL'yi ayarlayın `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.
5. Bulup kopyalayabilirsiniz **uygulama kimliği** ve **uygulama anahtarı**. Bunlara daha sonra ihtiyacınız.

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a>WeChat kiracınızdaki bir kimlik sağlayıcısı olarak yapılandırma
1. Bu adımları [B2C özellikleri dikey penceresine gitme](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalında.
2. B2C özellikleri dikey penceresinde tıklayın **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kullanımı kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "WeChat" girin.
5. Tıklayın **kimlik sağlayıcısı türü**seçin **WeChat**, tıklatıp **Tamam**.
6. Tıklayın **bu kimlik sağlayıcısını ayarlama**
7. Girin **uygulama anahtarı** olarak daha önce kopyaladığınız **istemci kimliği**.
8. Girin **uygulama gizli anahtarı** olarak daha önce kopyaladığınız **gizli**.
9. Tıklayın **Tamam** ve ardından **Oluştur** WeChat yapılandırmanızı kaydetmek için.

