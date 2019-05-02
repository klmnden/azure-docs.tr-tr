---
title: RelyingParty - Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C'de özel ilkesinin RelyingParty öğesi belirtin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/25/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: ca78e7a9ce44b492dafcc00c1663d54718ca7fac
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705088"
---
# <a name="relyingparty"></a>RelyingParty

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**RelyingParty** öğesi, Azure Active Directory (Azure AD) B2C'ye geçerli istek için zorlamak için kullanıcı yolculuğu belirtir. Ayrıca, bağlı taraf (RP) uygulaması gereken talepler listesinin verilen belirtecin bir parçası olarak belirtir. Bir web, mobil veya masaüstü uygulaması gibi bir RP uygulaması, RP ilke dosyasını çağırır. RP ilke dosyasını açarken, parola sıfırlama veya bir profil düzenleme gibi belirli bir görevi yürütür. Tek bir uygulama birden çok ilke kullanabilirsiniz ve birden çok uygulama aynı RP ilkesini kullanabilirsiniz. Tüm RP uygulamaları talepleri ile aynı belirteci alan ve kullanıcı aynı kullanıcı gezintisinde geçer.

Aşağıdaki örnekte gösterildiği bir **RelyingParty** öğesinde *B2C_1A_signup_signin* ilke dosyası:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="your-tenant.onmicrosoft.com"
  PolicyId="B2C_1A_signup_signin"
  PublicPolicyUri="http://your-tenant.onmicrosoft.com/B2C_1A_signup_signin">

  <BasePolicy>
    <TenantId>your-tenant.onmicrosoft.com</TenantId>
    <PolicyId>B2C_1A_TrustFrameworkExtensions</PolicyId>
  </BasePolicy>

  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <UserJourneyBehaviors>
      <SingleSignOn Scope="TrustFramework" KeepAliveInDays="7"/>
      <SessionExpiryType>Rolling</SessionExpiryType>
      <SessionExpiryInSeconds>300</SessionExpiryInSeconds>
      <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="your-application-insights-key" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      <ContentDefinitionParameters>
        <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
      </ContentDefinitionParameters>
    </UserJourneyBehaviors>
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Description>The policy profile</Description> 
      <Protocol Name="OpenIdConnect" />
      <Metadata>collection of key/value pairs of data</Metadata>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ...
```

İsteğe bağlı **RelyingParty** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| DefaultUserJourney | 1:1 | RP uygulaması için varsayılan kullanıcı yolculuğu. |
| UserJourneyBehaviors | 0:1 | Kullanıcı yolculuğu davranışlarını kapsamı. |
| TechnicalProfile | 1:1 | RP uygulaması tarafından desteklenen bir teknik profili. Teknik profili, RP uygulaması, Azure AD B2C başvurmak için bir sözleşme sağlar. |

## <a name="defaultuserjourney"></a>DefaultUserJourney

`DefaultUserJourney` Öğe tanımlayıcı genellikle temel ya da uzantıları ilkede tanımlanan kullanıcı yolculuğunun bir başvuru belirtir. Aşağıdaki örnekler belirtilen kaydolma veya oturum açma kullanıcı yolculuğu **RelyingParty** öğesi:

*B2C_1A_signup_signin* İlkesi:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn">
  ...
```

*B2C_1A_TrustFrameWorkBase* veya *B2C_1A_TrustFrameworkExtensionPolicy*:

```XML
<UserJourneys>
  <UserJourney Id="SignUpOrSignIn">
  ...
```

**DefaultUserJourney** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ReferenceId | Evet | Kullanıcı yolculuğu ilkesinde tanımlayıcısı. Daha fazla bilgi için [kullanıcı yolculuklarından](userjourneys.md) |

## <a name="userjourneybehaviors"></a>UserJourneyBehaviors

**UserJourneyBehaviors** öğesi aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| SingleSignOn | 0:1 | Çoklu oturum açma (SSO) oturum davranışını kullanıcı yolculuğu kapsamı. |
| Ssosession |0:1 | Oturumun kimlik doğrulama davranışı. Olası değerler: `Rolling` veya `Absolute`. `Rolling` Değeri (varsayılan) gösteren kullanıcının uygulamada sürekli olarak etkin olduğu sürece, kullanıcının oturum açmış durumda kalır. `Absolute` Değeri gösterir kullanıcı uygulama oturumu tarafından belirtilen bir süre sonra yeniden kimlik doğrulamaya zorlayabilir zorlanır yaşam süresi. |
| SessionExpiryInSeconds | 0:1 | Bir tamsayı olarak belirtilen Azure AD B2C'in oturum tanımlama bilgisinin ömrü, başarılı kimlik doğrulamadan sonra kullanıcının tarayıcısında depolanan. |
| JourneyInsights | 0:1 | Azure Application Insights izleme anahtarı. |
| ContentDefinitionParameters | 0:1 | İçerik tanımı yük URI eklenecek anahtar-değer çiftlerinin bir listesi. |

### <a name="singlesignon"></a>SingleSignOn

**SingleSignOn** öğesi şu özniteliği içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Kapsam | Evet | Çoklu oturum açma davranışı kapsamı. Olası değerler: `Suppressed`, `Tenant`, `Application`, veya `Policy`. `Suppressed` Değeri gösterir davranışı bastırılır. Örneğin, tek bir oturum açma oturumu, söz konusu olduğunda kullanıcı için hiçbir oturumu korunur ve kullanıcı bir kimlik sağlayıcısı seçim için her zaman istemde bulunulacak. `TrustFramework` Değeri gösterir davranışı güven Framework'teki tüm ilkeler için uygulanır. Örneğin, bir güven çerçevesi için iki ilke yolculuklarından arasında gezinme bir kullanıcı için bir kimlik sağlayıcısı seçim sorulmaz. `Tenant` Değeri gösterir davranışı kiracıdaki tüm ilkeleri uygulanır. Örneğin, bir kiracı için iki ilke yolculuklarından giderek bir kullanıcı için bir kimlik sağlayıcısı seçim sorulmaz. `Application` Değeri gösterir davranışı istekte uygulama yönelik tüm ilkeleri uygulanır. Örneğin, bir uygulama için iki ilke yolculuklarından arasında gezinme bir kullanıcı için bir kimlik sağlayıcısı seçim sorulmaz. `Policy` Değeri gösterir davranış yalnızca bir ilke için geçerlidir. Örneğin, bir güven çerçevesi için iki ilke yolculuklarından arasında gezinme bir kullanıcı için bir kimlik sağlayıcısı seçim ilkeleri arasında geçiş yaparken istenir. |
| KeepAliveInDays | Evet | kullanıcının oturum açmış durumda kalır ne kadar süreyle denetler. KMSI'yi işlevselliği 0 kapatır değeri ayarlanamadı. Daha fazla bilgi için [Oturumumu açık bırak](active-directory-b2c-reference-kmsi-custom.md). |

## <a name="journeyinsights"></a>JourneyInsights

**JourneyInsights** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| TelemetryEngine | Evet | Değer olmalıdır `ApplicationInsights`. | 
| Instrumentationkey | Evet | Application ınsights öğesi için izleme anahtarını içeren dize. |
| DeveloperMode | Evet | Olası değerler: `true` veya `false`. Varsa `true`, Application Insights telemetri işleme ardışık düzeninden hızlandırır. Bu ayar, geliştirme için iyidir ancak ayrıntılı etkinlik günlükleri yalnızca özel ilkeler geliştirmede yardımcı olmak için tasarlanmış yüksek miktarlarda, kısıtlanmış. Geliştirme modunda, üretim ortamında kullanmayın. Geliştirme sırasında gönderilen ve kimlik sağlayıcılardan gelen tüm talepler günlükleri toplayın. Üretim ortamında kullandıysanız, PII (özel olarak tanımlanabilir bilgiler) sahip oldukları App Insights günlüğünde toplanan sorumluluğunu Geliştirici varsayar. Bu değer ayarlandığında bu ayrıntılı günlükleri yalnızca toplanır `true`.|
| ClientEnabled | Evet | Olası değerler: `true` veya `false`. Varsa `true`, sayfa görünümü ve istemci tarafı hataları izlemek için Application Insights istemci tarafı komut dosyası gönderir. | 
| ServerEnabled | Evet | Olası değerler: `true` veya `false`. Varsa `true`, mevcut UserJourneyRecorder JSON özel olay Application Insights'a gönderir. | 
| TelemetryVersion | Evet | Değer olmalıdır `1.0.0`. | 

Daha fazla bilgi için [günlükleri toplama](active-directory-b2c-troubleshoot-custom.md)

## <a name="contentdefinitionparameters"></a>ContentDefinitionParameters

Azure AD B2C'de özel ilkeleri kullanarak, bir sorgu dizesi parametresi gönderebilirsiniz. Parametreyi HTML uç noktanıza ileterek sayfa içeriğini dinamik olarak değiştirebilirsiniz. Örneğin web veya mobil uygulamanızdan ilettiğiniz bir parametreye göre Azure AD B2C kaydolma veya oturum açma sayfanızdaki arka plan görüntüsünü değiştirebilirsiniz. Azure AD B2C sorgu dizesi parametre aspx dosyası gibi dinamik HTML dosyasını geçirir. 

Aşağıdaki örnekte adlı bir parametre geçirir `campaignId` değeriyle `hawaii` sorgu dizesinde:

`https://login.microsoft.com/contoso.onmicrosoft.com/oauth2/v2.0/authorize?pB2C_1A_signup_signin&client_id=a415078a-0402-4ce3-a9c6-ec1947fcfb3f&nonce=defaultNonce&redirect_uri=http%3A%2F%2Fjwt.io%2F&scope=openid&response_type=id_token&prompt=login&campaignId=hawaii`

**ContentDefinitionParameters** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| ContentDefinitionParameter | 0: n | İçerik tanımı yük URI sorgu dizesi olarak anahtar değer çifti içeren bir dize. |

**ContentDefinitionParameter** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Name | Evet | Anahtar değer çifti adı. |

Daha fazla bilgi için [özel ilkeler kullanarak dinamik içerik ile kullanıcı arabirimini yapılandırma](active-directory-b2c-ui-customization-custom-dynamic.md)

## <a name="technicalprofile"></a>TechnicalProfile

**TechnicalProfile** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- | 
| Kimlik | Evet | Değer olmalıdır `PolicyProfile`. |

**TechnicalProfile** aşağıdaki öğeleri içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| DisplayName | 0:1 | Kullanıcılara görüntülenen teknik profili adını içeren dize. |
| Açıklama | 0:1 | Kullanıcılara görüntülenen teknik profil açıklama içeren dize. |
| Protokol | 1:1 | Federasyon için kullanılan protokol. |
| Meta Veriler | 0:1 | Koleksiyonu *öğesi* bağlı olan taraf ve diğer topluluk katılımcılar arasındaki etkileşimi yapılandırmak için bir işlem sırasında uç noktasıyla iletişim protokolü tarafından kullanılan anahtar/değer çiftleri. |
| OutputClaims | 0:1 | Teknik profilde çıktı olarak gerçekleştirilen talep türleri listesi. Bu öğelerin her biri başvuru içeren bir **ClaimType** önceden tanımlanmış **ClaimsSchema** bölüm veya bu ilke dosyası devraldığı bir ilke. |
| SubjectNamingInfo | 0:1 | Konu adı belirteçleri kullanılır. |

**Protokolü** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| Name | Evet | Teknik profilinin bir parçası kullanılan bir Azure AD B2C tarafından desteklenen geçerli bir protokol adı. Olası değerler: `OpenIdConnect` veya `SAML2`. `OpenIdConnect` Değer için Openıd Connect 1.0 protokolü standart Openıd foundation belirtimi uyarınca temsil eder. `SAML2` OASIS belirtimi uyarınca SAML 2.0 protokolü standart temsil eder. SAML belirteci, üretim ortamında kullanmayın. |

## <a name="outputclaims"></a>OutputClaims

**OutputClaims** öğesi aşağıdaki öğeyi içerir:

| Öğe | Oluşumlar | Açıklama |
| ------- | ----------- | ----------- |
| outputClaim | 0: n | Bağlı olan taraf için abone olur ilke için desteklenen bir beklenen talep türü adı. Bu talep, teknik profil için bir çıktı görevi görür. |

**OutputClaim** öğesi aşağıdaki öznitelikler içerir:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimTypeReferenceId | Evet | Bir başvuru bir **ClaimType** önceden tanımlanmış **ClaimsSchema** ilke dosyasının bir bölümünde. |
| defaultValue | Hayır | Talep değeri boşsa, kullanılabilir bir varsayılan değer. |
| PartnerClaimType | Hayır | Talep ClaimType tanımında yapılandırıldığı gibi farklı bir ad gönderir. |

### <a name="subjectnaminginfo"></a>SubjectNamingInfo

İle **SubjectNameingInfo** öğesi, belirteç konu değerini kontrol:
- **JTW belirteci** - `sub` talep. Sorumlu olduğu hakkında bir uygulamanın kullanıcı gibi bilgileri belirteci onaylar budur. Bu değer sabittir ve yeniden atandı yeniden veya değiştirilemez. Bir kaynağa erişmek için belirteci kullanıldığında gibi güvenli yetkilendirme denetimleri gerçekleştirmek için kullanılabilir. Varsayılan olarak, konu talep, dizinde kullanıcının nesne kimliği ile doldurulur. Daha fazla bilgi için [belirteç, oturum ve çoklu oturum açma yapılandırması](active-directory-b2c-token-session-sso.md).
- **SAML belirteci** - `<Subject><NameID>` konu öğeyi tanımlayan bir öğe.

**SubjectNamingInfo** öğesi aşağıdaki öznitelik içeriyor:

| Öznitelik | Gerekli | Açıklama |
| --------- | -------- | ----------- |
| ClaimType | Evet | Bir çıkış talebin başvuru **PartnerClaimType**. Çıkış talep bağlı olan taraf ilkesinde tanımlanmalıdır **OutputClaims** koleksiyonu. |

Aşağıdaki örnek, bir bağlı olan taraf, Openıd Connect tanımlamak gösterilmektedir. Konu adı bilgisi olarak yapılandırılmış `objectId`:

```XML
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surname" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="identityProvider" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
  </TechnicalProfile>
</RelyingParty>
```
JWT belirteci içeren `sub` kullanıcı nesne kimliği ile talep:

```JSON
{
  ...
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  ...
}
```


