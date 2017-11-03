---
title: "Azure Active Directory B2C: WeChat yapılandırma | Microsoft Docs"
description: "Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan WeChat hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: a54aec23d951610118246e9f70cdd27752ef39a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma WeChat hesaplarıyla tüketicileri sağlayın

> [!NOTE]
> Bu özelliğin önizlemede değil.
> 

## <a name="create-a-wechat-application"></a>WeChat uygulaması oluşturma

Azure Active Directory (Azure AD) B2C içinde kimlik sağlayıcısı WeChat kullanmak için WeChat uygulama oluşturabilir ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir WeChat hesabı gerekir. Yoksa, bir mobil uygulamalarını biri aracılığıyla kaydolan veya h numaranızı kullanarak elde edebilirsiniz. Bundan sonra hesabınızı WeChat Geliştirici programla kayıtlı alın. Daha fazla bilgi bulabilirsiniz [burada](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).

### <a name="register-a-wechat-application"></a>WeChat uygulamayı Kaydet

1. Git [https://open.weixin.qq.com/](https://open.weixin.qq.com/) ve oturum açın.
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

