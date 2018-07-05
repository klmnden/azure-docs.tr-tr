---
title: SSO ve belirteci özelleştirme özel ilkeleri Azure Active Directory B2C ile yönetme | Microsoft Docs
description: SSO ve belirteci özelleştirme özel ilkeleri yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 811fb8b2de59c9d324ab4acb8b0f51b4cec80aee
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441806"
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Azure Active Directory B2C: SSO ve belirteci özelleştirme özel ilkeleri yönetme
Özel ilkeleri kullanarak belirteç, oturum ve çoklu oturum açma (SSO) yapılandırmaları üzerinde aynı denetim gibi yerleşik ilkeleri aracılığıyla sağlar.  Hangi öğrenmek için her ayarın belgelere bakın [burada](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Belirteç ömürleri ve talep yapılandırma
Eklemenize gerek, belirteç ömrünü ayarları değiştirmek için bir `<ClaimsProviders>` etkisi ilkeyi bağlı olan taraf dosyasındaki öğesi.  `<ClaimsProviders>` Öğesi alt öğesi olan `<TrustFrameworkPolicy>`.  İçinde belirteç ömrünü etkiler bilgiyi gerekecektir.  XML şu örnekteki gibi görünür:

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

**Belirteç ömürleri erişim** -erişim belirteci ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla saniyeler içinde "token_lifetime_secs" =.  3600 saniye (60 dakika) yerleşik, varsayılan değerdir.

**Kimlik belirteci ömrü** -kimlik belirteci ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla saniyeler içinde "id_token_lifetime_secs" =.  3600 saniye (60 dakika) yerleşik, varsayılan değerdir.

**Yenileme belirteci ömrü** -yenileme belirteci ömrü içindeki değeri değiştirerek değiştirilebilir `<Item>` anahtarla saniyeler içinde "refresh_token_lifetime_secs" =.  Yerleşik varsayılan değeri 1209600 (14 gün) saniyedir.

**Yenileme belirteci kayan pencere ömrü** - istiyorsanız, yenileme belirteci kayan pencere ömrü ayarlayın, içindeki değeri değiştirmek `<Item>` anahtarla saniyeler içinde "rolling_refresh_token_lifetime_secs" =.  7776000 (90 gün) yerleşik, varsayılan değerdir.  Bir kayan pencere ömrü uygulamak istemiyorsanız şu satır ile değiştirin:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Verici (iss) talebi** - verici (iss) talebi değiştirmek istiyorsanız içindeki değeri değiştirmek `<Item>` anahtarla = "IssuanceClaimPattern".  Geçerli değerler `AuthorityAndTenantGuid` ve `AuthorityWithTfp`.

**Ayar talep ilke Kimliğini temsil eden** -bu değeri ayarlamak için Seçenekler şunlardır: TFP (güven framework ilke) ve ACR (kimlik doğrulaması bağlamı Başvurusu).  
TFP için bu ayarı öneririz, bunu yapmak için olun `<Item>` ile Key = "AuthenticationContextReferenceClaimPattern" var ve değer `None`.
İçinde `<OutputClaims>` öğesi, bu öğeyi ekleyin:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
ACR için kaldırma `<Item>` anahtarla = "AuthenticationContextReferenceClaimPattern".

**Konu (sub) talebi** -bu değiştirmek istiyorsanız, bu seçenek için objectID, alınır `Not Supported`, aşağıdakileri yapın:

Bu satırı değiştirin 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Bu satırla:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Oturum davranışını ve SSO

Oturum davranışını ve SSO yapılandırmaları değiştirmek için eklemek gereken bir `<UserJourneyBehaviors>` öğesinin içindeki `<RelyingParty>` öğesi.  `<UserJourneyBehaviors>` Öğesi hemen izlemelidir `<DefaultUserJourney>`.  İçi, `<UserJourneyBehavors>` öğesi şu şekilde görünmelidir:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Çoklu oturum açma (SSO) yapılandırması** - tek oturum açma yapılandırmasını değiştirmek için değerini değiştirmeniz gerekir `<SingleSignOn>`.  Geçerli değerler `Tenant`, `Application`, `Policy` ve `Disabled`. 

**Web uygulaması oturumunun ömrü (dakika)** - web uygulaması oturumunun ömrü değiştirmek için değerini değiştirmeniz gerekir `<SessionExpiryInSeconds>` öğesi.  Varsayılan değeri yerleşik ilkeleri 86400 (1440 dakika) saniyedir.

**Web uygulaması oturumu zaman aşımı** - web uygulaması oturumu zaman aşımı, değiştirilecek değerini değiştirmeniz gerekir `<SessionExpiryType>`.  Geçerli değerler `Absolute` ve `Rolling`.
