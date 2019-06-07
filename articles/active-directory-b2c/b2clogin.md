---
title: Kümesi yeniden yönlendirme URL'leri b2clogin.com - Azure Active Directory B2C | Microsoft Docs
description: İçinde bir yeniden yönlendirme b2clogin.com kullanma hakkında bilgi edinmek için Azure Active Directory B2C URL'leri.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 4d70fe3f3f19723cd37080ae09dce97bfd8f3d34
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511687"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Kümesi yeniden yönlendirme URL'leri b2clogin.com için Azure Active Directory B2C için

Kaydolma ve oturum açma, Azure Active Directory (Azure AD) B2C uygulamanızda bir kimlik sağlayıcısını ayarlayın, bir yeniden yönlendirme URL'si belirtmeniz gerekir. B2clogin.com kullanma artık geçmişte login.microsoftonline.com, kullanıldı.

B2clogin.com kullanma, ek avantajlar gibi sunar:

- Tanımlama bilgisi üstbilgisi içinde Microsoft hizmetler tarafından kullanılan alanı azalır.
- URL'nizde artık Microsoft bir başvuru içerir. Örneğin, `https://your-tenant-name.b2clogin.com/tenant-id/oauth2/authresp`.

>[!NOTE]
> Hem Kiracı adı hem de GUID Kiracı şu şekilde kullanabilirsiniz:
> * `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com` (hangi hala başvurduğu `onmicrosoft.com`)
> * `https://your-tenant-name.b2clogin.com/your-tenant-guid` (Bu durumda yoktur Microsoft'a başvurmayın hiç)
>
> Ancak, kullanamazsınız bir _özel etki alanı_ için Azure Active Directory B2C kiracısı, örneğin `https://your-tenant-name.b2clogin.com/your-custom-domain-name` misiniz _değil_ çalışma.

Bu ayar b2clogin.com kullanırken değiştirmeniz gerekebileceğini göz önünde bulundurun:

- Yeniden yönlendirme ayarlama b2clogin.com kullanmak için kimlik sağlayıcısı uygulamalarınızda URL'leri. 
- Azure AD B2C uygulamanızı b2clogin.com kullanıcı akışı başvuruları ve belirteç uç noktalarını kullanacak şekilde ayarlayın. 
- MSAL kullanıyorsanız, ayarlanacak ihtiyacınız **ValidateAuthority** özelliğini `false`.
- Tüm değiştirdiğinizden emin olun **izin verilen çıkış noktaları** CORS ayarları için tanımladığınız [kullanıcı arabirimi özelleştirme](active-directory-b2c-ui-customization-custom-dynamic.md).  

## <a name="change-redirect-urls"></a>Değişiklik yeniden yönlendirme URL'leri

B2clogin.com, kimlik sağlayıcısı uygulamanızın ayarlarını kullanmak için arayın ve Azure AD B2C geri yönlendirmek için güvenli URL'lerin listesini değiştirin.  Şu anda, büyük ihtimalle bu şekilde ayarlamanız bazı login.microsoftonline.com siteye yeniden yönlendirme vardır. 

Yeniden yönlendirme URL'sini değiştirmek ihtiyacınız olacak şekilde `your-tenant-name.b2clogin.com` yetkili. Değiştirdiğinizden emin olun `your-tenant-name` adı, Azure AD B2C ile Kiracı ve kaldırma `/te` URL'de zaten varsa. Böylece her bir kimlik sağlayıcısı denetlemek için tam URL almak için ilgili sayfayı bu URL'ye küçük farklılıklar vardır.

Kimlik sağlayıcıları için kurulum bilgi aşağıdaki makalelerde bulabilirsiniz:

- [Microsoft hesabı](active-directory-b2c-setup-msa-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Google](active-directory-b2c-setup-goog-app.md)
- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn](active-directory-b2c-setup-li-app.md)
- [Twitter](active-directory-b2c-setup-twitter-app.md)
- [GitHub](active-directory-b2c-setup-github-app.md)
- [Weibo](active-directory-b2c-setup-weibo-app.md)
- [QQ](active-directory-b2c-setup-qq-app.md)
- [WeChat](active-directory-b2c-setup-wechat-app.md)
- [Azure AD](active-directory-b2c-setup-oidc-azure-active-directory.md)
- [Özel OIDC](active-directory-b2c-setup-oidc-idp.md)

## <a name="update-your-application"></a>Uygulamanızı güncelleştirme

Azure AD B2C'yi uygulamanız büyük olasılıkla başvurduğu `login.microsoftonline.com` kullanıcı akışı başvuruları ve belirteç uç noktaları gibi çeşitli yerlerde.  Kullanmak için yetkilendirme uç noktası, belirteç uç noktası ve veren güncelleştirildiğini doğrulayın `your-tenant-name.b2clogin.com`.  

## <a name="set-the-validateauthority-property"></a>ValidateAuthority özelliğini ayarlayın

MSAL kullanıyorsanız ayarlayın **ValidateAuthority** özelliğini `false`. Zaman **ValidateAuthority** ayarlanır `false`, yeniden yönlendirmeleri b2clogin.com için izin verilir. 

Aşağıdaki örnek, özelliğin nasıl ayarlayabilir gösterir:

İçinde [.Net için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet):

```CSharp
 ConfidentialClientApplication client = new ConfidentialClientApplication(...); // can also be PublicClientApplication
 client.ValidateAuthority = false;
```

Ve [Javascript için MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-js):

```Javascript
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false
  }
);
```
