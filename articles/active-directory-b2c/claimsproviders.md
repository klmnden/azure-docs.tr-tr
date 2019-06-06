---
title: ClaimsProviders - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke ClaimsProvider öğesi belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8d2570af6abb34a87ac4c69dd63408c8ec2e8005
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511513"
---
# <a name="claimsproviders"></a>ClaimsProviders

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir talep sağlayıcı kümesini içeren [teknik profiller](technicalprofiles.md). Her talep sağlayıcısı uç noktaları ve Talep sağlayıcı ile iletişim kurmak için gerekli Protokolü belirleyen bir veya daha fazla teknik profiller olması gerekir. Bir talep sağlayıcısı, birden fazla teknik profili olabilir. Örneğin, talep sağlayıcısı, birden çok protokol, farklı özelliklere sahip çeşitli uç noktalarını destekler veya farklı güvencesi düzeylerinde farklı talepler serbest bırakır çünkü birden fazla teknik profili tanımlanabilir. Bir kullanıcı yolculuğu, ancak başka bir hassas talep serbest bırakmak için kabul edilebilir olabilir.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
  ...
</ClaimsProviders>
```

**ClaimsProviders** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1:n | Çeşitli kullanıcı yolculuklarından yararlanılabilir akredite talep sağlayıcısı. |

## <a name="claimsprovider"></a>ClaimsProvider

**ClaimsProvider** öğe aşağıdaki alt öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ---------- | ----------- |
| Etki Alanı | 0:1 | Talep sağlayıcının etki alanı adını içeren bir dize. Örneğin, Facebook teknik profili, Talep sağlayıcı içeriyorsa, etki alanı adı Facebook.com ' dir. Bu etki alanı adı, teknik profili tarafından geçersiz kılınmadığı sürece Talep sağlayıcı tanımlanan tüm teknik profiller için kullanılır. Etki alanı adı da içinde başvurulabilir bir **domain_hint**. Daha fazla bilgi için **sosyal sağlayıcılar için yönlendirme oturum** bölümünü [doğrudan Azure Active Directory B2C kullanarak oturum kümesi](direct-signin.md). |
| displayName | 0:1 | Kullanıcılara gösterilen Talep sağlayıcı adını içeren bir dize. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Talep sağlayıcı tarafından desteklenen teknik profiller bir dizi |

**ClaimsProvider** teknik profillerinize talep sağlayıcısını nasıl ilişki kuracağını düzenler. Aşağıdaki örnek, Azure Active Directory teknik profilleri ile Azure Active Directory talep sağlayıcısı gösterir:

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

Aşağıdaki örnek, Facebook talep sağlayıcısından gösterir **Facebook OAUTH** teknik profili.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
