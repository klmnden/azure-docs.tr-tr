---
title: Doğrudan Azure Active Directory B2C kullanarak oturum ayarlama | Microsoft Docs
description: Oturum açma adı önceden veya düz bir sosyal kimlik sağlayıcısına yeniden yönlendirme öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 308fb8ea7f3429ce61b0872da9b1c10648b3f44b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695310"
---
# <a name="set-up-direct-sign-in-using-azure-active-directory-b2c"></a>Doğrudan Azure Active Directory B2C kullanarak oturum ayarlayın

Oturum açma için Azure Active Directory (AD) B2C'yi kullanarak uygulamanızı ayarlarken, oturum açma adı veya doğrudan oturum açma, bir Microsoft hesabı Facebook veya LinkedIn gibi belirli sosyal kimlik sağlayıcısı için önceden doldurabilir. 

## <a name="prepopulate-the-sign-in-name"></a>Önceden oturum açma adı

Oturum açma kullanıcı yolculuğu sırasında belirli bir kullanıcı veya etki alanı adı bir bağlı taraf uygulaması hedefleyebilir. Bir kullanıcı bir uygulama belirtebilirsiniz, yetkilendirme isteğinde hedeflenirken `login_hint` sorgu parametresi ile oturum açma kullanıcı adı. Kullanıcı yalnızca parola sağlaması gerekir ancak azure AD B2C oturum açma adını otomatik olarak doldurur.

![oturum açma ipucunu kullanarak](./media/direct-signin/login-hint.png) 

Kullanıcı oturum açma metin kutusundaki değeri değiştiremezsiniz.

Özel bir ilke kullanıyorsanız, geçersiz kılma `SelfAsserted-LocalAccountSignin-Email` teknik profili. İçinde `<InputClaims>` bölümünde, signInName talebi için DefaultValue `{OIDC:LoginHint}`. `{OIDC:LoginHint}` Değişken değerini içeren `login_hint` parametresi. Azure AD B2C signInName talep değerini okur ve signInName textbox önceden doldurur.

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

## <a name="redirect-sign-in-to-a-social-provider"></a>Sosyal sağlayıcılar için oturum açma yeniden yönlendirme

Uygulamanız Google, Facebook veya LinkedIn gibi sosyal medya hesaplarını eklemek için oturum açma yolculuğu yapılandırılıp yapılandırılmadığını belirtebilmeniz için `domain_hint` parametresi. Bu sorgu parametresi, sosyal kimlik sağlayıcısı oturum açma için kullanılması gereken hakkında Azure AD B2C'ye bir ipucu sağlar. Örneğin, uygulama belirtiyorsa `domain_hint=facebook.com`, oturum açma doğrudan Facebook oturum açma sayfasına gider.

![etki alanı ipucu kullanma](./media/direct-signin/domain-hint.png) 

Özel bir ilke kullanıyorsanız, etki alanı adını kullanarak yapılandırabilirsiniz `<Domain>domain name</Domain>` herhangi bir XML öğesi `<ClaimsProvider>`. 

```xml
<ClaimsProvider>
    <!-- Add the domain hint value to the claims provider -->
    <Domain>facebook.com</Domain>
    <DisplayName>Facebook</DisplayName>
    <TechnicalProfiles>
    ...
```


