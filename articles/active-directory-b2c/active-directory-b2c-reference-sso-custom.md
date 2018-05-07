---
title: Özel ilkeler - Azure AD B2C kullanarak SSO oturum yönetimi | Microsoft Docs
description: Azure AD B2C'de özel ilkeler kullanarak SSO oturumları yönetmeyi öğrenin.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 10/20/2017
ms.author: davidmu
ms.openlocfilehash: ca7160d39d5d26ca69345ce636f22afbe44b25db
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Çoklu oturum açma (SSO) oturum yönetimi

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C yönetici kullanıcı kimliğini doğrulamasından sonra Azure AD B2C bir kullanıcı ile nasıl etkileşim denetlemenizi sağlar. Bu SSO oturum yönetimi gerçekleştirilir. Örneğin, yönetici kimlik sağlayıcıları seçimini görüntülenip görüntülenmeyeceğini veya yerel hesap ayrıntılarını yeniden girilmesi gerekip gerekmediğini kontrol edebilirsiniz. Bu makalede, Azure AD B2C SSO ayarlarının nasıl yapılandırılacağı açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

SSO oturum yönetimi iki bölümden oluşur. İlk Azure AD B2C ile doğrudan kullanıcı etkileşimleri ve diğer anlaşmalar kullanıcının etkileşimleri dış taraflarla facebook ile ilgilidir. Azure AD B2C, geçersiz kılmaz veya dış kuruluşlar tarafından tutulan SSO oturumları atlayabilir. Bunun yerine harici taraf almak için Azure AD B2C yönlendir ", kullanıcının kendi sosyal veya Kurumsal kimlik sağlayıcısı seçmesini reprompt gerek önleme hatırlanır". Ultimate SSO karar dış taraflarla kalır.

## <a name="how-does-it-work"></a>Nasıl çalışır?

SSO oturum yönetimi diğer teknik profili özel ilkelerinde olarak aynı semantiğini kullanır. Orchestration adımı çalıştırıldığında, adımla ilişkili teknik profil için sorgulanan bir `UseTechnicalProfileForSessionManagement` başvuru. Varsa, başvurulan SSO oturum sağlayıcısı kullanıcı oturumu katılımcı olup olmadığını görmek için denetlenir. Bu nedenle SSO oturum sağlayıcısının yeniden doldurmak için kullanılıp kullanılmadığını oturumu. Benzer şekilde, orchestration adımının yürütülmesi tamamlandığında, sağlayıcı bir SSO oturum sağlayıcısı belirtilmişse oturumda bilgilerini depolamak için kullanılır.

Azure AD B2C kullanılabilir SSO oturum sağlayıcıları bir dizi tanımlanmış:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

SSO yönetimi sınıfları kullanarak belirtilen `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` teknik profili öğesidir.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Bu sağlayıcı adı belirleyen gibi hiçbir şey yapmaz. Bu sağlayıcı için belirli bir teknik profil SSO davranışı gizleme için kullanılabilir.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Bu sağlayıcı talep bir oturumda depolamak için kullanılabilir. Bu sağlayıcı genellikle yerel hesaplarını yönetmek için kullanılan bir teknik profili başvurulur. 

> [!NOTE]
> DefaultSSOSessionProvider bir oturumda talep depolamak için kullanırken, sonraki adımlarda ön koşullar tarafından kullanılan veya uygulamaya döndürülen gereken herhangi bir talep oturumda depolanan veya kullanıcıların profilinden okuma tarafından engagement'ta emin olmak gerekir Dizin. Bu, kimlik doğrulama Yolculuğunuzun 's eksik taleplere başarısız olmayan güvence altına alır.

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

Oturumda talep eklemek için kullanın `<PersistedClaims>` teknik profili öğesidir. Oturum, kalıcı yeniden doldurmak için sağlayıcı kullanıldığında talep için talep paketi eklenir. `<OutputClaims>` Talep oturumdan almak için kullanılır.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Bu sağlayıcı "Kimlik sağlayıcısı seçin" Ekran gizlemek için kullanılır. Genellikle, Facebook gibi bir dış kimlik sağlayıcısı için yapılandırılmış bir teknik profilinde başvuruluyor. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Bu sağlayıcı dış SAML kimlik sağlayıcısı yanı sıra, uygulamalar arasındaki Azure AD B2C SAML oturumları yönetmek için kullanılır.

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

Teknik profilinde iki meta veri öğeleri şunlardır:

| Öğe | Varsayılan Değer | Olası Değerler | Açıklama
| --- | --- | --- | --- |
| IncludeSessionIndex | true | true/false | Sağlayıcıya oturum dizini saklanması gerektiğini gösterir. |
| RegisterServiceProviders | true | true/false | Sağlayıcı bir onaylama verilen tüm SAML hizmet sağlayıcıları kaydedeceğini gösterir. |

Sağlayıcı bir SAML kimlik sağlayıcısı oturumu depolamak için kullanırken, yukarıdaki öğelerin her ikisi de false olmalıdır. Varsayılanları doğruysa B2C SAML oturumunun depolamak için Sağlayıcı kullanırken, yukarıdaki öğeleri doğru veya belirtilmemiş olması gerekir.

>[!NOTE]
> SAML oturum kapatma gerektirir `SessionIndex` ve `NameID` tamamlamak için.

## <a name="next-steps"></a>Sonraki adımlar

Biz görüş ve öneriler memnuniyet! Bu konu ile yaşarsanız etiketini kullanarak yığın taşması sonrası ['azure ad b2c'](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Bunlar için özellik istekleri için oy bizim [geri bildirim Forumunda](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

