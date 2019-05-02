---
title: Azure Active Directory B2C'de özel ilkeleri kullanarak çoklu oturum açma oturumu Yönetim | Microsoft Docs
description: Azure AD B2C'de özel ilkeler kullanarak SSO oturumları yönetmeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: c5e23f88b805f2d294a42c6e3e75a1ad63d3cc53
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726764"
---
# <a name="single-sign-on-session-management-in-azure-active-directory-b2c"></a>Azure Active Directory B2C tek oturum açma oturumu Yönetimi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kullanıcı zaten doğrulandıktan sonra tek oturum açma (SSO) oturum yönetimi Azure Active Directory (Azure AD) B2C'de, yönetici bir kullanıcı denetimi etkileşim sağlar. Örneğin, yönetici kimlik sağlayıcıları seçimini görüntülenip görüntülenmeyeceğini veya yerel hesap ayrıntılarını yeniden girilmesi gerekip gerekmediğini kontrol edebilirsiniz. Bu makalede, Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

SSO oturum yönetimi iki bölümden oluşur. İlk Azure AD B2C ile doğrudan kullanıcının etkileşimleri ve Facebook gibi dış tarafların kullanıcının etkileşim diğer fırsatlarla ile ilgilidir. Azure AD B2C, geçersiz kılmaz veya dış kuruluşlar tarafından tutulan SSO oturumları atlayabilir. Bunun yerine dış tarafa almak için Azure AD B2C rotayı "sosyal veya Kurumsal kimlik sağlayıcısına seçmesini reprompt gereğinden kurtulursunuz hatırlanır". Ultimate SSO karar harici taraflarla kalır.

SSO oturum yönetimi ile aynı semantiğe diğer teknik profili içinde özel ilkeleri kullanır. Düzenleme adımı çalıştırıldığında, adımı ile ilişkilendirilen teknik profil için sorgulanır bir `UseTechnicalProfileForSessionManagement` başvuru. Varsa, başvurulan SSO oturum sağlayıcısının kullanıcı oturumu katılımcı olup olmadığını görmek için denetlenir. Bu nedenle, SSO oturum sağlayıcısının yeniden doldurmak için kullanılıyorsa oturumu. Benzer şekilde, bir düzenleme adımı yürütülmesi tamamlandığında, sağlayıcı bir SSO oturum sağlayıcısı belirtilmişse oturumda bilgi depolamak için kullanılır.

Azure AD B2C birkaç kullanılabilir SSO oturum sağlayıcıları tanımlanır:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO yönetimi sınıfları kullanarak belirtilen `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` teknik profil öğesidir.

## <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Adı belirleyen gibi bu sağlayıcı hiçbir şey yapmaz. Bu sağlayıcı için belirli bir teknik profil SSO davranışı gizleme için kullanılabilir.

## <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Bu sağlayıcı bir oturumda talep depolamak için kullanılabilir. Bu sağlayıcı genellikle yerel hesapları yönetmek için kullanılan bir teknik profili başvurulur. Sonraki adımda, ön koşulları tarafından kullanılan veya uygulamaya döndürülen gereken herhangi bir talep oturumda depolanan veya kullanıcılar profilinden okunması genişletilmiş emin olmanız DefaultSSOSessionProvider talep bir oturumda depolamak için kullanılırken, Dizin. Bu, kimlik doğrulaması yolculuğunuza 's eksik taleplere dönmüyor garanti eder.

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

Oturumda talep eklemek için `<PersistedClaims>` teknik profil öğesidir. Kalıcı oturumu yeniden doldurmak için sağlayıcı kullanıldığında talep için talep paketi eklenir. `<OutputClaims>` Talep oturumdan almak için kullanılır.

## <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Bu sağlayıcı "Kimlik sağlayıcısı seçin" ekranında gizlemek için kullanılır. Genellikle Facebook gibi bir dış kimlik sağlayıcısı için yapılandırılmış bir teknik profili başvurulur. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

## <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Bu sağlayıcı, Azure AD B2C SAML oturumları arasında dış SAML kimlik sağlayıcısı yanı sıra uygulamaları yönetmek için kullanılır.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Teknik profili içinde iki meta veri öğeleri şunlardır:

| Öğe | Varsayılan Değer | Olası Değerler | Açıklama
| --- | --- | --- | --- |
| IncludeSessionIndex | true | true/false | Oturum dizini depolanması gereken sağlayıcıya gösterir. |
| RegisterServiceProviders | true | true/false | Sağlayıcı bir onay verilmiş tüm SAML hizmet sağlayıcılarının kaydolmalıdır gösterir. |

Sağlayıcı bir SAML kimlik sağlayıcısı oturum depolamak için kullanırken, yukarıdaki öğelerin her ikisi de false olmalıdır. Varsayılan değerleri true olarak yukarıdaki öğeleri B2C SAML oturumunun depolamak için Sağlayıcı kullanırken, true veya belirtilmemiş olması gerekir. SAML oturumu kapatma gerekiyor `SessionIndex` ve `NameID` tamamlanması.

