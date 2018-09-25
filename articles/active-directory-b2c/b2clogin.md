---
title: B2clogin.com kullanma | Microsoft Docs
description: Login.microsoftonline.com yerine b2clogin.com kullanma hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c24582fce44006d9a3972d73078aa8cb0d212c11
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47053853"
---
# <a name="using-b2clogincom"></a>B2Clogin.com kullanma

Bundan sonra size tüm müşterilerin kullanmak için teşvik `<YourDirectoryName>.b2clogin.com` ve biz kullanımdan kaldıracağız `login.microsoftonline.com`. B2Clogin.com ek avantajlar gibi sağlar:
* Diğer Microsoft hizmetleriyle aynı tanımlama bilgisi bundan böyle paylaşın.
* Microsoft tüm başvuruları, URL'de kaldırabilirsiniz (değiştirebilirsiniz `<YourDirectoryName>.onmicrosoft.com` , dizin kimliği ile). Örneğin: `https://<YourDirectoryName>.b2clogin.com/tfp/<YourDirectoryID>/<policyname>/v2.0/.well-known/openid-configuration`.

B2clogin.com için taşıma için yapmanız gerekenler

* Sosyal kimlik sağlayıcısı uygulamalarınız için yeniden yönlendirme URI'leri değiştirme
* B2Clogin.com yerine kullanılacak, uygulamanızın Düzenle `login.microsoftonline.com` ilke başvuruları ve belirteç uç noktaları için.
* MSAL kullanıyorsanız, ayarlanacak ihtiyacınız `ValidateAuthority=false`.  

##<a name="redirect-uris-for-social-identity-providers"></a>Sosyal kimlik sağlayıcıları için yeniden yönlendirme URI'leri

Sosyal hesap kimlik sağlayıcıları dizininizde ayarlanan varsa uygulamalarında değişiklikler yapmasını gerekir.  Uygulaması için Azure AD B2C geri yönlendirmek için güvenli URL'lerin bir listesini içeren her bir sosyal sağlayıcısı ile parametre yoktur.  Şu anda, büyük olasılıkla şekilde ayarlamanız geri bazı yeniden yönlendirme elinizde `login.microsoftonline.com` site, bu URL'yi değiştirmeniz gerekir böylece `YourDirectoryName.b2clogin.com` yetkili bir yeniden yönlendirme URI'si olacaktır.  Kaldırdığınızdan emin olun `/te` de.  Böylece her bir kimlik sağlayıcısı denetlemek için tam URL almak için ilgili sayfayı bu URL'ye küçük farklılıklar vardır.  

| Kimlik sağlayıcısı |
|-------------------|
|[Microsoft hesabı](active-directory-b2c-setup-msa-app.md)|
|[Facebook](active-directory-b2c-setup-fb-app.md)|
|[Google](active-directory-b2c-setup-goog-app.md)|
|[Amazon](active-directory-b2c-setup-amzn-app.md)|
|[LinkedIn](active-directory-b2c-setup-li-app.md)|
|[Twitter](active-directory-b2c-setup-twitter-app.md)|
|[GitHub](active-directory-b2c-setup-github-app.md)|
|[Weibo](active-directory-b2c-setup-weibo-app.md)|
|[QQ](active-directory-b2c-setup-qq-app.md)|
|[WeChat](active-directory-b2c-setup-wechat-app.md)|
|[Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)|
|[Özel OIDC](active-directory-b2c-setup-oidc-idp.md)|

##<a name="update-your-application-references"></a>Uygulama başvurularını güncelleştir

Uygulamanızın büyük olasılıkla başvurduğu `login.microsoftonline.com` ilke başvuruları ve belirteç uç noktaları gibi çeşitli yerlerde.  Yetkilendirme uç noktası, belirteç uç noktası ve veren güncelleştirildiğini emin olun.  

##<a name="set-validateauthorityfalse-in-msal"></a>Ayarlama `ValidateAuthority=false` MSAL içinde

MSAL kullanıyorsanız ayarlamanız gerekir `ValidateAuthority=false`.  Daha fazla bilgi için [bu belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.clientapplicationbase?view=azure-dotnet).