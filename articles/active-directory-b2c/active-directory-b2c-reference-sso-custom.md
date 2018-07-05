---
title: SSO oturum yönetimi Azure Active Directory B2C'de özel ilkelerini kullanma | Microsoft Docs
description: Azure AD B2C'de özel ilkeler kullanarak SSO oturumları yönetmeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/20/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 351b48f2e2766b4974a5a41b5e95acfbd63dbfc9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443231"
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Çoklu oturum açma (SSO) oturum yönetimi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C kullanıcı zaten doğrulandıktan sonra Azure AD B2C kullanıcı ile nasıl etkileşim denetlemek bir yöneticinin sağlar. Bu, SSO oturum yönetimi gerçekleştirilir. Örneğin, yönetici kimlik sağlayıcıları seçimini görüntülenip görüntülenmeyeceğini veya yerel hesap ayrıntılarını yeniden girilmesi gerekip gerekmediğini kontrol edebilirsiniz. Bu makalede, Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

## <a name="overview"></a>Genel Bakış

SSO oturum yönetimi iki bölümden oluşur. İlk Azure AD B2C ile doğrudan kullanıcının etkileşimleri ve Facebook gibi dış tarafların kullanıcının etkileşim diğer fırsatlarla ile ilgilidir. Azure AD B2C, geçersiz kılmaz veya dış kuruluşlar tarafından tutulan SSO oturumları atlayabilir. Bunun yerine dış tarafa almak için Azure AD B2C rotayı "sosyal veya Kurumsal kimlik sağlayıcısına seçmesini reprompt gereğinden kurtulursunuz hatırlanır". Ultimate SSO karar harici taraflarla kalır.

## <a name="how-does-it-work"></a>Nasıl çalışır?

SSO oturum yönetimi ile aynı semantiğe diğer teknik profili içinde özel ilkeleri kullanır. Düzenleme adımı çalıştırıldığında, adımı ile ilişkilendirilen teknik profil için sorgulanır bir `UseTechnicalProfileForSessionManagement` başvuru. Varsa, başvurulan SSO oturum sağlayıcısının kullanıcı oturumu katılımcı olup olmadığını görmek için denetlenir. Bu nedenle SSO oturum sağlayıcısının yeniden doldurmak için kullanılıp kullanılmadığını oturumu. Benzer şekilde, bir düzenleme adımı yürütülmesi tamamlandığında, sağlayıcı bir SSO oturum sağlayıcısı belirtilmişse oturumda bilgi depolamak için kullanılır.

Azure AD B2C birkaç kullanılabilir SSO oturum sağlayıcıları tanımlanır:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO yönetimi sınıfları kullanarak belirtilen `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` teknik profil öğesidir.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Adı belirleyen gibi bu sağlayıcı hiçbir şey yapmaz. Bu sağlayıcı için belirli bir teknik profil SSO davranışı gizleme için kullanılabilir.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Bu sağlayıcı bir oturumda talep depolamak için kullanılabilir. Bu sağlayıcı genellikle yerel hesapları yönetmek için kullanılan bir teknik profili başvurulur. 

> [!NOTE]
> Sonraki adımda, ön koşulları tarafından kullanılan veya uygulamaya döndürülen gereken herhangi bir talep oturumda depolanan veya kullanıcılar profilinden okunması genişletilmiş emin olmanız DefaultSSOSessionProvider talep bir oturumda depolamak için kullanılırken, Dizin. Bu, kimlik doğrulaması yolculuğunuza 's eksik taleplere dönmüyor garanti eder.

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

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Bu sağlayıcı "Kimlik sağlayıcısı seçin" ekranında gizlemek için kullanılır. Genellikle Facebook gibi bir dış kimlik sağlayıcısı için yapılandırılmış bir teknik profili başvurulur. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

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

Sağlayıcı bir SAML kimlik sağlayıcısı oturum depolamak için kullanırken, yukarıdaki öğelerin her ikisi de false olmalıdır. Varsayılan değerleri true olarak yukarıdaki öğeleri B2C SAML oturumunun depolamak için Sağlayıcı kullanırken, true veya belirtilmemiş olması gerekir.

>[!NOTE]
> SAML oturumu kapatma gerekiyor `SessionIndex` ve `NameID` tamamlanması.

## <a name="next-steps"></a>Sonraki adımlar

Geri bildirim ve öneriler Sevdiğimiz! Bu konu ile ilgili zorluklarla varsa etiketini kullanarak Stack Overflow sitesinde sonrası ['azure-ad-b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Özellik istekleri için bunlar için oy bizim [geri bildirim Forumu](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

