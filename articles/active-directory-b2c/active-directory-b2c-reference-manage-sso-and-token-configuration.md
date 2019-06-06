---
title: SSO ve Azure Active Directory B2C'de özel ilkeleri kullanarak belirteci özelleştirme yönetme | Microsoft Docs
description: SSO ve Azure Active Directory B2C'de özel ilkeleri kullanarak belirteci özelleştirme yönetme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f3621b176e4bbfdfbd171339d6d01a1f91ed0ae7
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509294"
---
# <a name="manage-sso-and-token-customization-using-custom-policies-in-azure-active-directory-b2c"></a>SSO ve Azure Active Directory B2C'de özel ilkeleri kullanarak belirteci özelleştirme yönetme

Bu makale, belirteç, oturum ve yapılandırmaları çoklu oturum açma (SSO) kullanarak nasıl yönetebileceğinizi hakkında bilgi sağlar [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de.

## <a name="token-lifetimes-and-claims-configuration"></a>Belirteç ömürleri ve talep yapılandırma

Üzerinde belirteç ömrünü ayarlarını değiştirmek için eklediğiniz bir [ClaimsProviders](claimsproviders.md) etkisi ilkeyi bağlı olan taraf dosyasındaki öğesi.  **ClaimsProviders** öğesi alt öğesi olan [TrustFrameworkPolicy](trustframeworkpolicy.md) öğesi. 

BasePolicy öğesi ve bağlı olan taraf dosyanın RelyingParty öğesi arasında ClaimsProviders öğesi ekleyin.

İçinde belirteç ömrünü etkiler bilgiyi gerekecektir. XML şu örnekteki gibi görünür:

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

Aşağıdaki değerleri, önceki örnekte ayarlanır:

- **Belirteç ömürleri erişim** - değeri ayarı ile erişim belirteci ömrü **token_lifetime_secs** meta veri öğesi. 3600 saniye (60 dakika) varsayılan değerdir.
- **Kimlik belirteci ömrü** - değeri ayarı ile kimlik belirteci ömrü **id_token_lifetime_secs** meta veri öğesi. 3600 saniye (60 dakika) varsayılan değerdir.
- **Yenileme belirteci ömrü** - değeri ayarı ile yenileme belirteci ömrü **refresh_token_lifetime_secs** meta veri öğesi. Varsayılan değer 1209600 (14 gün) saniyedir.
- **Yenileme belirteci kayan pencere ömrü** - istiyorsanız, yenileme belirteci kayan pencere ömrü ayarlayın, değerini **rolling_refresh_token_lifetime_secs** meta veri öğesi. 7776000 (90 gün) varsayılan değerdir. Bir kayan pencere ömrü uygulamak istemiyorsanız öğeyle değiştirin `<Item Key="allow_infinite_rolling_refresh_token">True</Item>`.
- **Verici (iss) talebi** -verici (iss) talebi ile ayarlanır **IssuanceClaimPattern** meta veri öğesi. Geçerli değerler `AuthorityAndTenantGuid` ve `AuthorityWithTfp`.
- **Ayar talep ilke Kimliğini temsil eden** -bu değeri ayarlamak için Seçenekler `TFP` (güven framework ilke) ve `ACR` (kimlik doğrulaması bağlamı Başvurusu). `TFP` Önerilen değerdir. Ayarlama **AuthenticationContextReferenceClaimPattern** değeriyle `None`. 

    İçinde **ClaimsSchema** öğesi, bu öğeyi ekleyin: 
    
    ```XML
    <ClaimType Id="trustFrameworkPolicy">
      <DisplayName>Trust framework policy name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
    ```
    
    İçinde **OutputClaims** öğesi, bu öğeyi ekleyin:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
    ```

    ACR için kaldırma **AuthenticationContextReferenceClaimPattern** öğesi.

- **Konu (sub) talebi** -bu seçenek varsayılan olarak objectID için bu ayarı geçiş yapmak istiyorsanız, `Not Supported`, bu satırı değiştirin: 

    ```XML
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    ```
    
    Bu satırla:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    ```

## <a name="session-behavior-and-sso"></a>Oturum davranışını ve SSO

Oturum davranışını ve SSO yapılandırmaları değiştirmek için eklediğiniz bir **UserJourneyBehaviors** öğesinin içindeki [RelyingParty](relyingparty.md) öğesi.  **UserJourneyBehaviors** öğesi hemen izlemelidir **DefaultUserJourney**. İçi, **UserJourneyBehavors** öğesi, bu örnekteki gibi görünmelidir:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```

Aşağıdaki değerler, önceki örnekte yapılandırılır:

- **Çoklu oturum açma (SSO)** -çoklu oturum açma ile yapılandırılmış **SingleSignOn**. Geçerli değerler `Tenant`, `Application`, `Policy`, ve `Suppressed`. 
- **Web uygulaması oturumunun ömrü (dakika)** - web uygulaması oturumunun ömrü ile ayarlanır **SessionExpiryInSeconds** öğesi. Varsayılan değer 86400 (1440 dakika) saniyedir.
- **Web uygulaması oturumu zaman aşımı** - web uygulaması oturumu zaman aşımı ile ayarlanır **Ssosession** öğesi. Geçerli değerler `Absolute` ve `Rolling`.
