---
title: Azure Active Directory B2C özel ilkelerinde teknik Profiller hakkında | Microsoft Docs
description: Azure Active Directory B2C, özel bir ilkede kullanılan nasıl teknik profilleri hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 80b8969ba657506705db2b1a3bbc5b389d0a992c
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512448"
---
# <a name="about-technical-profiles-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkeleri teknik profilleri hakkında

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Teknik profili, farklı türde bir özel ilke kullanarak Azure Active Directory (Azure AD) B2C'de taraflar ile iletişim kurmak için yerleşik bir mekanizma ile bir çerçeve sunar. Teknik profilleri, bir kullanıcı oluşturabilir veya bir kullanıcı profilini okuma için Azure AD B2C kiracınız ile iletişim kurmak için kullanılır. Teknik profili, kullanıcı etkileşimi etkinleştirmek için otomatik olarak onaylanan olabilir. Örneğin, oturum açmak için kullanıcının kimlik bilgilerini toplayın ve ardından kayıt sayfasına veya parola sıfırlama sayfası işleme. 

## <a name="type-of-technical-profiles"></a>Teknik profil türü

Teknik profili, bu tür senaryolar sağlar:

- [Azure Active Directory](active-directory-technical-profile.md) -Azure Active Directory B2C'yi kullanıcı yönetimi için destek sağlar.
- [JWT belirteci vericisi](jwt-issuer-technical-profile.md) -bağlı taraf uygulaması için döndürülen bir JWT belirteci yayar. 
- **Telefon Etmenli sağlayıcısını** -çok faktörlü kimlik doğrulaması.
- [OAuth1](oauth1-technical-profile.md) -herhangi bir OAuth 1.0 protokolü kimlik sağlayıcısı ile Federasyon.
- [OAuth2](oauth2-technical-profile.md) -herhangi bir OAuth 2.0 protokolü kimlik sağlayıcısı ile Federasyon.
- [Openıdconnect](openid-connect-technical-profile.md) -Openıd Connect Protokolü kimlik sağlayıcısı ile Federasyon.
- [Talep dönüştürme](claims-transformation-technical-profile.md) - çağrı çıkış talep dönüştürmeleri işlemek için talep değerleri, talepleri doğrulamak veya çıkış talep kümesi için varsayılan değerleri ayarlayın.
- [RESTful sağlayıcısı](restful-technical-profile.md) -REST API hizmetlerini, kullanıcı girişini doğrulama gibi çağrısına kullanıcı veri zenginleştirme veya satır iş kolu uygulamaları ile tümleştirin.
- [SAML2](saml-technical-profile.md) -tüm SAML Protokolü kimlik sağlayıcısı ile Federasyon.
- [Kendi kendine onaylanan](self-asserted-technical-profile.md) -kullanıcıyla etkileşim. Örneğin, oturum açmak için kullanıcının kimlik bilgileri toplamak, kayıt sayfasını işlemek veya parola sıfırlama.
- **WsFed** -tüm WsFed Protokolü kimlik sağlayıcısı ile Federasyon. 
- [Oturum yönetimi](active-directory-b2c-reference-sso-custom.md) -işlemek oturumları farklı türde. 
- **Application ınsights**

## <a name="technical-profile-flow"></a>Teknik profil akışı

Tüm tür teknik profili, aynı kavramı paylaşır. Giriş talepleri göndermek, talep dönüştürme çalıştırın ve bir kimlik sağlayıcısı, REST API veya Azure AD Dizin Hizmetleri gibi yapılandırılmış taraf ile iletişim kurar. İşlemi tamamlandıktan sonra teknik profil çıkışı döndürür ve çıkış talep dönüştürme çalışabilir. Aşağıdaki diyagramda, teknik profili içinde başvurulan eşleme ve dönüşümleri nasıl işlendiği gösterilmektedir. Taraf teknik profili, herhangi bir talep dönüştürme yürütüldüğünde, sonra etkileşimde bağımsız olarak çıkış talep teknik profilinden hemen talep paketinde depolanır. 

![Teknik profil akışı](./media/technical-profiles-overview/technical-profile-idp-saml-flow.png)
 
1. **InputClaimsTransformation** -giriş her giriş talep [dönüştürme talep](claimstransformations.md) talep paketi ve yürütmenin sonrasına, talepleri talep paketindeki geri isimlerine çıkış seçilir. Bir sonraki giriş talepleri dönüştürme giriş talepleri çıkış talep giriş talepleri dönüştürme olabilir.
2. **InputClaims** - toplanmış talep paketinden talep ve teknik profil için kullanılır. Örneğin, bir [teknik profil otomatik olarak onaylanan](self-asserted-technical-profile.md) giriş talep kullanıcının sağladığı çıkış talep önceden doldurmak için kullanır. Bir REST API teknik profili, giriş parametreleri REST API uç noktasına göndermesi için giriş talepleri kullanır. Azure Active Directory giriş talep okumak, güncelleştirmek veya bir hesabı silmek için benzersiz bir tanımlayıcı olarak kullanır.
3. **Teknik profil yürütme** -talepleri ile yapılandırılmış taraf teknik profili birbiriyle değiştirir. Örneğin:
    - Kullanıcıyı oturum açma işlemini tamamlamak için kimlik sağlayıcısına yönlendirir. Başarılı oturum açma sonra kullanıcı geri döndürür ve teknik profil yürütme devam eder.
    - Parametreleri InputClaims gönderme ve bilgi OutputClaims olarak geri alma sırasında bir REST API çağrısı.
    - Veya kullanıcı hesabı güncelleştirilemiyor.
    - Gönderir ve mesaj MFA doğrular.
4. **ValidationTechnicalProfiles** - bir [kendi kendine teknik profil onaylanan](self-asserted-technical-profile.md), girdi çağırabilirsiniz [doğrulama teknik profili](validation-technical-profile.md). Doğrulama teknik profili, kullanıcı tarafından profili verileri doğrular ve bir hata iletisi veya Tamam ile veya çıkış talep olmadan döndürür. Örneğin, ciddi bir şekilde Azure AD B2C'yi yeni bir hesap oluşturmadan önce Dizin Hizmetleri'nde kullanıcı zaten var olup olmadığını denetler. Kendi iş mantığınızı eklemek için bir REST API teknik profili çağırabilirsiniz.<p>Doğrulama teknik profili ' çıkış taleplerini kapsamını doğrulama teknik profili çağıran teknik profil ve diğer doğrulama teknik profiller aynı teknik profili altındaki sınırlıdır. Çıkış talep düzenleme sonraki adımda kullanmak istiyorsanız, çıkış talep doğrulama teknik profili çağıran teknik profile eklemeniz gerekir.
5. **OutputClaims** -talep, talep paketi için geri döndürülür. Bu talep sonraki düzenlemeleri adım veya çıkış talep dönüştürmeleri kullanabilirsiniz.
6. **OutputClaimsTransformations** -giriş her çıkış talep [dönüştürme talep](claimstransformations.md) talep paketinden seçilir. Önceki adımdan teknik profil çıkış talep giriş talepleri çıkış talep dönüşümünün olabilir. Yürütmeden sonra çıkış talep, talep paketindeki geri yerleştirilir. Bir çıkış talep dönüştürme, çıkış talep giriş talepleri bir sonraki çıkış talep dönüştürme de olabilir.
7. **Çoklu oturum açma (SSO) oturum yönetimi** - [SSO oturum yönetimi](active-directory-b2c-reference-sso-custom.md) kullanıcı zaten doğrulandıktan sonra bir kullanıcı etkileşimi denetler. Örneğin, yönetici kimlik sağlayıcıları seçimini görüntülenip görüntülenmeyeceğini veya yerel hesap ayrıntılarını yeniden girilmesi gerekip gerekmediğini kontrol edebilirsiniz.

Teknik profil ayarlarını değiştirebilir veya yeni işlevler eklemek için başka bir teknik profili devralabilir.  **IncludeTechnicalProfile** teknik profil türetilmiş, temel teknik profiline başvuru bir öğedir.  

Örneğin, **AAD UserReadUsingAlternativeSecurityId NoError** teknik profili içerir **AAD UserReadUsingAlternativeSecurityId**. Bu teknik profili **RaiseErrorIfClaimsPrincipalDoesNotExist** meta veri öğesi `true`ve bir sosyal hesap dizinde mevcut değilse bir hata oluşturur. **AAD UserReadUsingAlternativeSecurityId NoError** bu davranışı geçersiz kılar ve kullanıcının değil mevcutsa, hata iletisi devre dışı bırakır.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
  <Metadata>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">false</Item>
  </Metadata>
  <IncludeTechnicalProfile ReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
</TechnicalProfile>
``` 

**AAD UserReadUsingAlternativeSecurityId** içerir `AAD-Common` teknik profili.

```XML
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
  <Metadata>
    <Item Key="Operation">Read</Item>
    <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
    <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">User does not exist. Please sign up before you can sign in.</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="AlternativeSecurityId" PartnerClaimType="alternativeSecurityId" Required="true" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="otherMails" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surname" />
  </OutputClaims>
  <IncludeTechnicalProfile ReferenceId="AAD-Common" />
</TechnicalProfile>
```

Her ikisi de **AAD UserReadUsingAlternativeSecurityId NoError** ve **AAD UserReadUsingAlternativeSecurityId** gerekli belirtmeyin **Protokolü** öğe olduğundan içinde belirtilen **AAD yaygın** teknik profili.

```XML
<TechnicalProfile Id="AAD-Common">
  <DisplayName>Azure Active Directory</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
</TechnicalProfile>
```

Teknik profil dahil edin veya başka bir içerebilecek başka bir teknik profili devralır. Düzey sayısına bir sınır yoktur. İş gereksinimlerine bağlı olarak, kullanıcı yolculuğunuza çağırabilir **AAD UserReadUsingAlternativeSecurityId** sosyal bir kullanıcı hesabı yoksa, bir hata oluşturan veya  **AAD UserReadUsingAlternativeSecurityId NoError** hangi hata yükseltmek değil.











