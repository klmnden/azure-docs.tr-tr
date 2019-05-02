---
title: Azure Active Directory B2C, özel bir ilkede otomatik olarak onaylanan teknik profil tanımlama | Microsoft Docs
description: Azure Active Directory B2C özel bir ilke otomatik olarak onaylanan bir teknik profili tanımlayın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 41305cc5825344a61ff15ddb5deb629cd0f1c679
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691020"
---
# <a name="define-a-self-asserted-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Bir Azure Active Directory B2C özel ilke otomatik olarak onaylanan teknik profil tanımlama

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory (Azure AD) B2C kullanıcı giriş sağlamak için burada beklenen tüm etkileşimleri teknik profiller Self onaylanan. Örneğin, bir kaydolma sayfasında, oturum açma sayfası veya parola sıfırlama sayfası.

## <a name="protocol"></a>Protokol

**Adı** özniteliği **Protokolü** öğesi ayarlanması gerekiyor `Proprietary`. **İşleyici** özniteliği için Azure AD B2C tarafından kullanılan protokol işleyicisi bütünleştirilmiş kodun tam adı içermelidir Self onaylanan: `Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

Aşağıdaki örnek, bir e-posta için otomatik olarak onaylanan teknik profili kaydolma gösterir:

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
```
 
## <a name="input-claims"></a>Giriş talepleri

Otomatik olarak onaylanan teknik profili içinde kullanabileceğiniz **InputClaims** ve **InputClaimsTransformations** (çıktı otomatik olarak onaylanan sayfada görüntülenen talep değerini önceden doldurmak için öğeleri Talepler). Örneğin, profil düzenleme ilkesinin kullanıcı yolculuğu ilk Azure AD B2C dizin hizmetinden kullanıcı profili okur, sonra otomatik olarak onaylanan teknik profili, kullanıcı profilinde depolanan kullanıcı verileri ile giriş talepleri ayarlar. Bu talep kullanıcı profilinizden toplanan ve ardından var olan verilere daha sonra düzenleyebilirsiniz kullanıcıya gösterilir.

```XML
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
    <InputClaim ClaimTypeReferenceId="userPrincipalName" />
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
```


## <a name="output-claims"></a>Çıkış talep

**OutputClaims** öğesi kullanıcıdan veri toplamak için sunulan talepler bir listesini içerir. Çıkış talep bazı değerleri ile önceden doldurmak için daha önce açıklanan giriş talepleri kullanın. Öğe, varsayılan bir değer de içerebilir. Taleplerde sırasını **OutputClaims** Azure AD B2C ekranında talepleri işler sırasını denetler. **DefaultValue** yalnızca talep hiçbir zaman önce alındıysa özniteliği etkinleşir. Ancak, kullanıcı değeri boş ayrılsa bile bunu önce önceki bir düzenleme adımı ayarlandıysa, varsayılan değer etkili olmaz. Varsayılan değer kullanılmasını zorlamak için ayarlanmış **AlwaysUseDefaultValue** özniteliğini `true`. Bir değer sağlamak için belirli bir çıkış talep zorlamak için ayarlanmış **gerekli** özniteliği **OutputClaims** öğesine `true`.

**ClaimType** öğesinde **OutputClaims** koleksiyon ayarlamak için gereksinim duyduğu **UserInputType** öğesi herhangi bir kullanıcı için giriş türü gibiAzureADB2Ctarafındandesteklenen`TextBox`veya `DropdownSingleSelect`. Veya **OutputClaim** öğesi ayarlamalısınız bir **DefaultValue**.  

**OutputClaimsTransformations** öğe koleksiyonu içerebilir **OutputClaimsTransformation** çıkış talep değiştirmek veya yenilerini oluşturmak için kullanılan öğeleri.

Aşağıdaki çıkış talep her zaman kümesine `live.com`:

```XML
<OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" AlwaysUseDefaultValue="true" />
```

### <a name="use-case"></a>Kullanım örneği

Çıkış talep için dört senaryo vardır:

- **Toplama çıkış talep kullanıcıdan** - kullanıcıdan bilgi toplamak gerektiğinde doğum tarihi gibi talep eklemelisiniz **OutputClaims** koleksiyonu. Kullanıcıya sunulan talepler belirtmelisiniz **UserInputType**, gibi `TextBox` veya `DropdownSingleSelect`. Otomatik olarak onaylanan teknik profili, aynı Talep veren bir doğrulama teknik profili varsa, Azure AD B2C kullanıcı talebine sunmaz. Kullanıcının herhangi bir çıkış talep varsa, Azure AD B2C'Teknik profili atlar.
- **Bir çıkış talep kümesinde bir varsayılan değer ayarlama** - kullanıcıdan veri toplamak veya doğrulama teknik profilinden veriyor. `LocalAccountSignUpWithLogonEmail` Teknik profili kümeleri kendi kendine onaylanan **yürütülen SelfAsserted-girişi** için talep `true`.
- **Doğrulama teknik profil çıkışı döndürür** -teknik profilinizi bazı talep döndüren bir doğrulama teknik profili çağırabilir. Talep Kabarcık ve kullanıcı yolculuğunda düzenleme sonraki adımlarda dönmek isteyebilirsiniz. Örneğin, yerel bir hesabıyla oturum açtığınızda, otomatik olarak onaylanan teknik profil adlı `SelfAsserted-LocalAccountSignin-Email` adlı doğrulama teknik profili çağırır `login-NonInteractive`. Bu teknik profili, kullanıcı kimlik bilgilerini doğrular ve ayrıca kullanıcı profilini döndürür. 'UserPrincipalName', 'displayName', 'givenName' ve 'Soyadı' gibi.
- **Talepleri çıkış talep dönüştürme ile çıktı**

Aşağıdaki örnekte, `LocalAccountSignUpWithLogonEmail` teknik profili gösterir, çıkış talep ve kümeleri kendi kendine onaylanan **yürütülen SelfAsserted-girişi** için `true`. `objectId`, `authenticationSource`, `newUser` Taleplerdir çıktısını `AAD-UserWriteUsingLogonEmail` doğrulama teknik profil ve kullanıcıya gösterilmez.

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    <Item Key="language.button_continue">Create</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="issuer_secret" StorageReferenceId="B2C_1A_TokenSigningKeyContainer" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
    <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
    <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
    <OutputClaim ClaimTypeReferenceId="authenticationSource" />
    <OutputClaim ClaimTypeReferenceId="newUser" />

    <!-- Optional claims, to be collected from the user -->
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="givenName" />
    <OutputClaim ClaimTypeReferenceId="surName" />
  </OutputClaims>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
  </ValidationTechnicalProfiles>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
</TechnicalProfile>

```

## <a name="persist-claims"></a>Talep Sürdür

Varsa **PersistedClaims** öğesi eksik, otomatik olarak onaylanan teknik profili, Azure AD B2C'ye verilerini sürdürmez. Bunun yerine, verileri kalıcı hale getirmekten sorumlu olan bir doğrulama teknik profil için bir çağrı yapılır. Örneğin, kaydolma İlkesi kullanan `LocalAccountSignUpWithLogonEmail` teknik profili, yeni kullanıcı profili toplamak için kendi kendine onaylanan. `LocalAccountSignUpWithLogonEmail` Teknik profili içinde Azure AD B2C hesabı oluşturmak için doğrulama teknik profili çağırır.

## <a name="validation-technical-profiles"></a>Doğrulama teknik profiller

Doğrulama teknik profili, bazılarını veya tümünü başvuru teknik profili, çıkış talep doğrulamak için kullanılır. Otomatik olarak onaylanan teknik profil çıkış talep giriş talepleri doğrulama teknik profil yer almalıdır. Doğrulama teknik profili, kullanıcı girişini doğrulayan ve kullanıcıya bir hata döndürebilir. 

Doğrulama teknik profili gibi herhangi bir teknik profil ilkesi olabilir [Azure Active Directory](active-directory-technical-profile.md) veya [REST API](restful-technical-profile.md) teknik profiller. Önceki örnekte, `LocalAccountSignUpWithLogonEmail` teknik profili doğrulama signinName dizinde mevcut değil. Değilse, doğrulama teknik profili yerel bir hesap oluşturup objectID, authenticationSource newUser döndürür. `SelfAsserted-LocalAccountSignin-Email` Teknik profil çağrıları `login-NonInteractive` kullanıcı kimlik bilgilerini doğrulamak için doğrulama teknik profili.

Ayrıca, bir REST API teknik profili iş mantığınızı çağrı, giriş talep üzerine veya kullanıcı verilerini daha fazla Kurumsal satır iş kolu uygulaması ile tümleştirerek zenginleştirin. Daha fazla bilgi için [doğrulama teknik profili](validation-technical-profile.md)

## <a name="metadata"></a>Meta Veriler

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| setting.showContinueButton | Hayır | Devam düğmesine görüntüler. Olası değerler: `true` (varsayılan), veya `false` |
| setting.showCancelButton | Hayır | İptal düğmesi görüntüler. Olası değerler: `true` (varsayılan), veya `false` |
| setting.operatingMode | Hayır | Bu özellik, bir oturum açma sayfası için giriş doğrulama ve hata iletileri gibi bir kullanıcı adı alanı davranışını denetler. Beklenen değer: `Username` veya `Email`. |
| ContentDefinitionReferenceId | Evet | Tanımlayıcısını [içerik tanımı](contentdefinitions.md) Bu teknik profil ile ilişkili. |
| EnforceEmailVerification | Hayır | İçin kaydolma veya düzenleme profili, e-posta doğrulama zorlar. Olası değerler: `true` (varsayılan) veya `false`. | 
| setting.showSignupLink | Hayır | Kaydolun düğmesi görüntüler. Olası değerler: `true` (varsayılan), veya `false` |
| setting.retryLimit | Hayır | Bir kullanıcının doğrulama teknik profili karşı denetlenir verilerini sağlamak için deneme sayısını denetler. Örneğin, bir kullanıcı zaten var ve sınırına kadar denemeye tutan bir hesapla kaydolmak için çalışır.
| SignUpTarget | Hayır | Kaydolma hedef exchange tanımlayıcısı. Kullanıcı Kaydolma düğmesine tıkladığında, Azure AD B2C'yi belirtilen exchange tanımlayıcısı yürütür. |

## <a name="cryptographic-keys"></a>Şifreleme anahtarları

**CryptographicKeys** öğesi kullanılmaz.













