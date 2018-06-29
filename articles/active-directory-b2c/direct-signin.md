---
title: Doğrudan Azure Active Directory B2C kullanarak oturum açın ayarlama | Microsoft Docs
description: Oturum açma adı önceden veya düz bir sosyal kimlik sağlayıcısına yeniden yönlendirme öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/18/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: cc3baa8fe4139acec94a722bf8c2bccb708a9470
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102697"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Doğrudan Azure Active Directory B2C kullanarak oturum açın ayarlayın

Oturum açma için Azure Active Directory (AD) B2C kullanarak uygulamanızı ayarlarken, oturum açma adı veya doğrudan oturum açma Facebook, LinkedIn veya bir Microsoft hesabı gibi belirli bir sosyal kimlik sağlayıcısı için önceden girebilirsiniz. 

## <a name="prepopulate-the-sign-in-name"></a>Oturum açma adı önceden

Oturum açma kullanıcı Yolculuk sırasında belirli bir kullanıcı veya etki alanı adı bir bağlı olan taraf uygulaması hedefleyebilir. Bir kullanıcı bir uygulama belirtebilirsiniz, yetkilendirme isteği hedeflerken `login_hint` sorgu parametresi oturum açma kullanıcı adı. Kullanıcı yalnızca parola sağlaması gerekir sırasında azure AD B2C oturum açma adı otomatik olarak doldurur.

![oturum açma ipucunu kullanarak](./media/direct-signin/login-hint.png) 

Kullanıcı oturum açma textbox değeri değiştirin yapabiliyor.

Özel bir ilke kullanıyorsanız, geçersiz kılma `SelfAsserted-LocalAccountSignin-Email` teknik profili. İçinde `<InputClaims>` signInName talep DefaultValue bölümünde, `{OIDC:LoginHint}`. `{OIDC:LoginHint}` Değişken değerini içeren `login_hint` parametresi. Azure AD B2C signInName talep değerini okur ve signInName textbox önceden doldurur.

```xml
<ClaimsProvider>
  <DisplayName>Local Account</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
      <InputClaims>
        <!-- Add the login hint value to the sign-in names claim type -->
        <InputClaim ClaimTypeReferenceId="signInName" DefaultValue="{OIDC:LoginHint}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="redirect-sign-in-to-a-social-provider"></a>Oturum açma bir sosyal sağlayıcısına yeniden yönlendirme

Facebook, LinkedIn veya Google, gibi sosyal hesaplarını dahil etmek, uygulamanız için oturum açma gezisine yapılandırılıp yapılandırılmadığını belirtebilirsiniz `domain_hint` parametresi. Bu sorgu parametresi, oturum açma için kullanılması gereken sosyal kimlik sağlayıcısı hakkında Azure AD B2C'ye bir ipucu sağlar. Örneğin, uygulama belirtiyorsa `domain_hint=facebook.com`, oturum açma doğrudan Facebook oturum açma sayfasına gider.

![etki alanı ipucunu kullanarak](./media/direct-signin/domain-hint.png) 

Özel bir ilke kullanıyorsanız, etki alanı adını kullanarak yapılandırabilirsiniz `<Domain>domain name</Domain>` XML öğesi herhangi `<ClaimsProvider>`. 

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


