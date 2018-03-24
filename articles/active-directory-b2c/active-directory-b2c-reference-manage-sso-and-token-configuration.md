---
title: SSO ve belirteç özelleştirme özel ilkeler ile yönetme | Microsoft Docs
description: SSO ve belirteç özelleştirme özel ilkeler ile yönetme hakkında bilgi edinin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/02/2017
ms.author: davidmu
ms.openlocfilehash: c9eb7f7711a8987945b8aeaee8d6798b7a6b8284
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: SSO ve belirteç özelleştirme özel ilkeler ile yönetme
Özel ilkelerini kullanma belirteci, oturum ve çoklu oturum açma (SSO) yapılandırmaları üzerinden aynı denetimi gibi yerleşik ilkeleri aracılığıyla sağlar.  Her ayar yaptıklarını öğrenmek için lütfen belgelere bakın [burada](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Belirteç yaşam süresi ve talep yapılandırma
Üzerinde belirteci, yaşam süresi ayarları değiştirmek için eklemeniz gerekir bir `<ClaimsProviders>` etkisi istediğiniz ilkeyi bağlı olan taraf dosyasındaki öğesi.  `<ClaimsProviders>` Bir alt öğedir `<TrustFrameworkPolicy>`.  İçinde belirteç yaşam süreleri etkiler bilgileri koymak gerekir.  XML şöyle görünür:

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

**Erişim belirteci yaşam süreleri** erişim belirteç ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla "token_lifetime_secs" = saniye içinde.  Varsayılan yerleşik 3600 saniye (60 dakika) değerdir.

**Kimliği belirteç ömrü** kimliği belirteç ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla "id_token_lifetime_secs" = saniye içinde.  Varsayılan yerleşik 3600 saniye (60 dakika) değerdir.

**Belirteç ömrü yenileme** yenileme belirteç ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla "refresh_token_lifetime_secs" = saniye içinde.  Yerleşik varsayılan değeri 1209600 (14 gün) saniyedir.

**Belirteç kayan pencere ömrü yenileme** yenileme belirtecinizi kayan bir pencere ömrü ayarlamak istiyorsanız içindeki değeri değiştirmek `<Item>` anahtarla "rolling_refresh_token_lifetime_secs" = saniye içinde.  7776000 (90 gün) içinde yerleşik varsayılan değerdir.  Bir kayan için enfore istemiyorsanız, Pencere Ömrü bu satırla değiştirin:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Veren (ISS) talep** içindeki değeri veren (ISS) talep değiştirmek isterseniz, değiştirmek `<Item>` anahtarıyla = "IssuanceClaimPattern".  Geçerli değerler `AuthorityAndTenantGuid` ve `AuthorityWithTfp`.

**Ayar talep İlkesi Kimliğini temsil eden** bu değeri ayarlamak için Seçenekler şunlardır: TFP (güven framework ilke) ve ACR (kimlik doğrulaması bağlamı başvuru).  
Bu TFP için ayarı öneririz, bunu yapmak için olun `<Item>` ile anahtar = "AuthenticationContextReferenceClaimPattern" var ve değer `None`.
İçinde `<OutputClaims>` öğesi, bu öğe ekleyin:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
ACR için kaldırma `<Item>` anahtarıyla = "AuthenticationContextReferenceClaimPattern".

**Konu (alt) talep** için geçiş yapmak istiyorsanız bu seçeneği objectID için alınır `Not Supported`, aşağıdakileri yapın:

Bu satırı değiştirin 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Bu satırla:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Oturum davranışını ve SSO
Oturum davranışını ve SSO yapılandırmaları değiştirmek için eklemeniz gerekir bir `<UserJourneyBehaviors>` öğesinin içine `<RelyingParty>` öğesi.  `<UserJourneyBehaviors>` Öğesi hemen uymalıdır `<DefaultUserJourney>`.  İçini, `<UserJourneyBehavors>` öğesi aşağıdaki gibi görünmelidir:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Çoklu oturum açma (SSO) yapılandırma** değerini değiştirmenize gerek tek oturum açma yapılandırmasını değiştirmek için `<SingleSignOn>`.  Geçerli değerler `Tenant`, `Application`, `Policy` ve `Disabled`. 

**Web uygulaması oturum yaşam süresi (dakika)** değiştirmek için web uygulaması oturum yaşam değerini değiştirmek ihtiyacınız `<SessionExpiryInSeconds>` öğesi.  Varsayılan değer yerleşik ilkelerinde 86400 (1440 dakika) saniyedir.

**Web uygulaması oturum zaman aşımı** değerini değiştirmenize gerek web uygulaması oturum zaman aşımı süresini değiştirmek için `<SessionExpiryType>`.  Geçerli değerler `Absolute` ve `Rolling`.
