---
title: TechnicalProfiles | Microsoft Docs
description: Azure Active Directory B2C'de özel bir ilke TechnicalProfiles öğesi belirtin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f712634c83fa290ab24d5e8437a82d5f93af0b7f
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66512291"
---
# <a name="technicalprofiles"></a>TechnicalProfiles

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

A **TechnicalProfiles** Talep sağlayıcı tarafından desteklenen teknik profiller bir dizi öğesi içeriyor. Her talep sağlayıcısı uç noktaları ve Talep sağlayıcı ile iletişim kurmak için gerekli Protokolü belirleyen bir veya daha fazla teknik profiller olması gerekir. Bir talep sağlayıcısı, birden fazla teknik profili olabilir.

```XML
<ClaimsProvider>
  <DisplayName>Display name</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Technical profile identifier">
      <DisplayName>Display name of technical profile</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">URL of service</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
      </Metadata>
      <InputTokenFormat>JWT</InputTokenFormat>
      <OutputTokenFormat>JWT</OutputTokenFormat>
      <CryptographicKeys>
        <Key ID="Key identifier" StorageReferenceId="Storage key container identifier"/>
        ...
      </CryptographicKeys>
      <InputClaimsTransformations>
        <InputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <InputClaimsTransformations>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="givenName" DefaultValue="givenName" PartnerClaimType="firstName" />
        ...
      </PersistedClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="Claims transformation identifier" />
        ...
      <OutputClaimsTransformations>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="Technical profile identifier" />
        ...
      </ValidationTechnicalProfiles>
      <SubjectNamingInfo ClaimType="Claim type identifier" />
      <IncludeTechnicalProfile ReferenceId="Technical profile identifier" />
      <UseTechnicalProfileForSessionManagement ReferenceId="Technical profile identifier" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

**TechnicalProfile** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
|---------|---------|---------|
| Kimlik | Evet | Teknik profil benzersiz tanımlayıcısı. Teknik profili, bu diğer öğeleri tanımlayıcıdan ilke dosyasını kullanarak başvurulabilir. Örneğin, **OrchestrationSteps** ve **ValidationTechnicalProfile**. |

**TechnicalProfile** aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Etki Alanı | 0:1 | Teknik profil için etki alanı adı. Facebook kimlik sağlayıcısı teknik profilinizi belirtir, örneğin, etki alanı Facebook.com adıdır. |
| displayName | 0:1 | Kullanıcılara görüntülenen teknik profil adı. |
| Açıklama | 0:1 | Kullanıcılara görüntülenen teknik Profil açıklaması. |
| Protocol | 0:1 | Diğer taraf iletişimi için kullanılan protokol. |
| Meta Veriler | 0:1 | Bir işlem sırasında uç noktasıyla iletişim protokolü tarafından kullanılan anahtar/değer çiftleri koleksiyonu. |
| InputTokenFormat | 0:1 | Giriş belirteci biçimi. Olası değerler: `JSON`, `JWT`, `SAML11`, veya `SAML2`. `JWT` Değeri temsil eden bir JSON Web belirteci IETF belirtimi uyarınca. `SAML11` Değeri OASIS belirtimi uyarınca bir SAML 1.1 güvenlik belirteci temsil eder.  `SAML2` Değeri OASIS belirtimi uyarınca bir SAML 2.0 güvenlik belirteci temsil eder. |
| OutputTokenFormat | 0:1 | Çıkış belirteci biçimi. Olası değerler: `JSON`, `JWT`, `SAML11`, veya `SAML2`. |
| CryptographicKeys | 0:1 | Teknik profili içinde kullanılan şifreleme anahtarları listesi. |
| InputClaimsTransformations | 0:1 | Herhangi bir talep, Talep sağlayıcı veya bağlı olan taraf için gönderilmeden önce yürütülmesi gereken talep dönüştürmeleri önceden tanımlı başvuru listesi. |
| InputClaims | 0:1 | Önceden tanımlanmış başvuruları talep olarak geçen türleri listesini teknik profili içinde giriş. |
| PersistedClaims | 0:1 | Önceden tanımlanmış başvuruları talep teknik profiline ilgili talep sağlayıcısı tarafından kalıcı türleri listesi. |
| OutputClaims | 0:1 | Önceden tanımlanmış başvuruları talep teknik profilde çıktı olarak geçen türleri listesi. |
| OutputClaimsTransformations | 0:1 | Talep sağlayıcısından gelen talepleri alındıktan sonra yürütülmesi gereken talep dönüştürmeleri önceden tanımlı başvuru listesi. |
| ValidationTechnicalProfiles | 0: n | Doğrulama amacıyla teknik profili kullanan diğer teknik profiline başvuru listesi. Daha fazla bilgi için [doğrulama teknik profili](validation-technical-profile.md)|
| SubjectNamingInfo | 0:1 | Konu adı gelen talepleri ayrı olarak burada belirtilen belirteçleri konu adına üretimini denetler. Örneğin, OAuth veya SAML.  |
| IncludeClaimsFromTechnicalProfile | 0:1 | Tüm bu teknik profile eklenecek giriş ve çıkış talep istediğiniz bir teknik profili tanımlayıcısı. Başvurulan teknik profili aynı ilke dosyasında tanımlanmış olması gerekir. |
| IncludeTechnicalProfile |0:1 | Bu teknik profile eklenecek tüm verileri istediğiniz bir teknik profili tanımlayıcısı. Başvurulan teknik profili, aynı ilke dosyasında mevcut olmalıdır. |
| UseTechnicalProfileForSessionManagement | 0:1 | Oturum yönetimi için kullanılacak farklı bir teknik profili. |
|EnabledForUserJourneys| 0:1 |Teknik profili bir kullanıcı yolculuğunda yürütülürse denetler.  |

### <a name="protocol"></a>Protocol

**Protokolü** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Name | Evet | Teknik profilinin bir parçası kullanılan bir Azure AD B2C tarafından desteklenen geçerli bir protokol adı. Olası değerler: `OAuth1`, `OAuth2`, `SAML2`, `OpenIdConnect`, `WsFed`, `WsTrust`, `Proprietary`, `session management`, `self-asserted`, veya `None`. |
| İşleyici | Hayır | Protokol adı ayarlandığında `Proprietary`, Azure AD B2C tarafından kullanılan protokol işleyicisi belirlemek için bütünleştirilmiş kodun tam adı belirtin. |

### <a name="metadata"></a>Meta Veriler

A **meta verileri** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Öğe | 0: n | Teknik profiline ilişkili meta verileri. Her teknik profili türü farklı bir meta veri öğeleri kümesi vardır. Daha fazla bilgi için teknik profil türleri bölümüne bakın. |

#### <a name="item"></a>Öğe

**Öğesi** öğesinin **meta verileri** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Anahtar | Evet | Meta veri anahtarı. Her teknik profil türü, meta veri öğeleri listesi için bkz. |

### <a name="cryptographickeys"></a>CryptographicKeys

**CryptographicKeys** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Anahtar | 1:n | Bu teknik profili içinde kullanılan bir şifreleme anahtarı. |

#### <a name="key"></a>Anahtar

**Anahtar** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kimlik | Hayır | Belirli bir anahtar çifti diğer öğelerden ilkesi dosyasında başvurulan bir benzersiz tanımlayıcısı. |
| StorageReferenceId | Evet | Bir tanımlayıcıyla diğer öğelerden ilkesi dosyasında başvurulan bir depolama anahtar kapsayıcısı. |

### <a name="inputclaimstransformations"></a>InputClaimsTransformations

**InputClaimsTransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| InputClaimsTransformation | 1:n | Herhangi bir talep, Talep sağlayıcı veya bağlı olan taraf için gönderilmeden önce yürütülmesi gereken bir talep dönüştürme tanımlayıcısı. Talep dönüştürme mevcut ClaimsSchema talep değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

#### <a name="inputclaimstransformation"></a>InputClaimsTransformation

**InputClaimsTransformation** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosya ya da üst ilke dosyası zaten tanımlanmış bir talep dönüştürme tanımlayıcısı. |

### <a name="inputclaims"></a>InputClaims

**InputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| Inputclaim | 1:n | Beklenen bir giriş talep türü. |

#### <a name="inputclaim"></a>Inputclaim

**Inputclaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyası veya üst ilke dosyası ClaimsSchema bölümünde önceden tanımlanmış bir talep türü tanımlayıcısı. |
| defaultValue | Hayır | Sonuçta elde edilen talep bir Inputclaim teknik profili tarafından kullanılabilir. böylece, talep tarafından ClaimTypeReferenceId belirtilmişse bir talep oluşturmak için kullanılacak bir varsayılan değer yok. |
| PartnerClaimType | Hayır | Belirtilen ilke talep türü dış ortak talep türünü tanımlayıcısını eşlenir. Daha sonra PartnerClaimType özniteliği belirtilmezse, belirtilen ilke talep türü adın aynısını iş ortağı talebi türü eşleştirilir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, iş ortağı 'first_name' adlı bir talep kullanırken ilk talep adı 'givenName', ' dir. |

### <a name="persistedclaims"></a>PersistedClaims

**PersistedClaims** öğesi aşağıdaki öğeleri içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| PersistedClaim | 1:n | Kalıcı hale getirmek için talep türü. |

#### <a name="persistedclaim"></a>PersistedClaim

**PersistedClaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyası veya üst ilke dosyası ClaimsSchema bölümünde önceden tanımlanmış bir talep türü tanımlayıcısı. |
| defaultValue | Hayır | Sonuçta elde edilen talep bir Inputclaim teknik profili tarafından kullanılabilir. böylece, talep tarafından ClaimTypeReferenceId belirtilmişse bir talep oluşturmak için kullanılacak bir varsayılan değer yok. |
| PartnerClaimType | Hayır | Belirtilen ilke talep türü dış ortak talep türünü tanımlayıcısını eşlenir. Daha sonra PartnerClaimType özniteliği belirtilmezse, belirtilen ilke talep türü adın aynısını iş ortağı talebi türü eşleştirilir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, iş ortağı 'first_name' adlı bir talep kullanırken ilk talep adı 'givenName', ' dir. |

### <a name="outputclaims"></a>OutputClaims

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| outputClaim | 1:n | Beklenen bir çıkış talep türü. |

#### <a name="outputclaim"></a>outputClaim

**OutputClaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | İlke dosyası veya üst ilke dosyası ClaimsSchema bölümünde önceden tanımlanmış bir talep türü tanımlayıcısı. |
| defaultValue | Hayır | Sonuçta elde edilen talep bir Inputclaim teknik profili tarafından kullanılabilir. böylece, talep tarafından ClaimTypeReferenceId belirtilmişse bir talep oluşturmak için kullanılacak bir varsayılan değer yok. |
|AlwaysUseDefaultValue |Hayır |Varsayılan değer kullanımını zorlar.  |
| PartnerClaimType | Hayır | Belirtilen ilke talep türü dış ortak talep türünü tanımlayıcısını eşlenir. Daha sonra PartnerClaimType özniteliği belirtilmezse, belirtilen ilke talep türü adın aynısını iş ortağı talebi türü eşleştirilir. Talep türü adınız diğer taraftan farklı olduğunda bu özelliği kullanın. Örneğin, iş ortağı 'first_name' adlı bir talep kullanırken ilk talep adı 'givenName', ' dir. |

### <a name="outputclaimstransformations"></a>OutputClaimsTransformations

**OutputClaimsTransformations** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| OutputClaimsTransformation | 1:n | Herhangi bir talep, Talep sağlayıcı veya bağlı olan taraf için gönderilmeden önce yürütülmesi gereken talep dönüştürmeleri tanımlayıcıları. Talep dönüştürme mevcut ClaimsSchema talep değiştirmek veya yenilerini oluşturmak için kullanılabilir. |

#### <a name="outputclaimstransformation"></a>OutputClaimsTransformation

**OutputClaimsTransformation** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosya ya da üst ilke dosyası zaten tanımlanmış bir talep dönüştürme tanımlayıcısı. |

### <a name="validationtechnicalprofiles"></a>ValidationTechnicalProfiles

**ValidationTechnicalProfiles** öğesi aşağıdaki öğeyi içerir:

| Öğe | Örnekleri | Açıklama |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1:n | Kullanılan teknik profiller tanımlayıcıların bazılarını veya tümünü başvuru teknik profili, çıkış talep doğrulayın. Tüm giriş talepleri başvurulan teknik profili, başvuru teknik profili çıkış Taleplerde yer almalıdır. |

#### <a name="validationtechnicalprofile"></a>ValidationTechnicalProfile

**ValidationTechnicalProfile** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosya ya da üst ilke dosyası zaten tanımlanmış bir teknik profili tanımlayıcısı. |

###  <a name="subjectnaminginfo"></a>SubjectNamingInfo

**SubjectNamingInfo** aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimType | Evet | İlke dosyası ClaimsSchema bölümünde önceden tanımlanmış bir talep türü tanımlayıcısı. |

### <a name="includetechnicalprofile"></a>IncludeTechnicalProfile

**IncludeTechnicalProfile** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosya ya da üst ilke dosyası zaten tanımlanmış bir teknik profili tanımlayıcısı. |

### <a name="usetechnicalprofileforsessionmanagement"></a>UseTechnicalProfileForSessionManagement

**UseTechnicalProfileForSessionManagement** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | İlke dosya ya da üst ilke dosyası zaten tanımlanmış bir teknik profili tanımlayıcısı. |

### <a name="enabledforuserjourneys"></a>EnabledForUserJourneys
**ClaimsProviderSelections** bir kullanıcı yolculuğunun talep sağlayıcısı seçme seçenekleri ve bunların sırası listesini tanımlar. İle **EnabledForUserJourneys** kullanıcıya hangi Talep sağlayıcı kullanılabilir filtrelemenize, öğe. **EnabledForUserJourneys** öğesi aşağıdaki değerlerden birini içerir:

- **Her zaman**, teknik profil yürütün.
- **Hiçbir zaman**, teknik profil atlayın.
- **OnClaimsExistence** teknik profilde belirtilen belirli talep, yalnızca mevcut olduğunda yürütün.
- **OnItemExistenceInStringCollectionClaim**, yalnızca bir öğe bir dize koleksiyonu talebi zaman var. yürütün.
- **OnItemAbsenceInStringCollectionClaim** yalnızca, bir öğe bir dize koleksiyonu talebi yok yürütün.

Kullanarak **OnClaimsExistence**, **OnItemExistenceInStringCollectionClaim** veya **OnItemAbsenceInStringCollectionClaim**, aşağıdaki vermenizi gerektirir meta veri: **ClaimTypeOnWhichToEnable** uyumluluğunun değerlendirilebilmesi için bir talebin türü belirtiyor. **ClaimValueOnWhichToEnable** Karşılaştırılacak değer belirtir.

Aşağıdaki teknik profili, yalnızca yürütülen **identityProviders** dize koleksiyonu içeren değerini `facebook.com`:

```XML
<TechnicalProfile Id="UnLink-Facebook-OAUTH">
  <DisplayName>Unlink Facebook</DisplayName>
...
    <Metadata>
      <Item Key="ClaimTypeOnWhichToEnable">identityProviders</Item>
      <Item Key="ClaimValueOnWhichToEnable">facebook.com</Item>
    </Metadata>
...
  <EnabledForUserJourneys>OnItemExistenceInStringCollectionClaim</EnabledForUserJourneys>
</TechnicalProfile>
```
