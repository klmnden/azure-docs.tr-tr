---
title: Çözümleyiciler Azure Active Directory B2C özel ilkelerinde'yaklaşık talep | Microsoft Docs
description: Nasıl talep çözümleyiciler Azure Active Directory B2C, özel bir ilkede kullanıldığı hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 01/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a13d4b0b44c51f78a068b1619fe083a08756af6b
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66511598"
---
# <a name="about-claim-resolvers-in-azure-active-directory-b2c-custom-policies"></a>Azure Active Directory B2C özel ilkelerinde talep Çözümleyicileri hakkında

Azure Active Directory (Azure AD) B2C, çözümleyiciler talep [özel ilkeler](active-directory-b2c-overview-custom.md) ilke adı, istek bağıntı kimliği, kullanıcı arabirimi dili ve daha fazlası gibi bir yetkilendirme isteği hakkında bağlam bilgisi sağlar.

Bir talep çözümleyicisinin bir giriş veya çıkış talep kümesinde kullanmak için bir dizeyi tanımlamak **ClaimType**altında [ClaimsSchema](claimsschema.md) öğesi olarak ayarlayın ve sonra **DefaultValue** talep Çözümleyici giriş veya çıkış talep öğesi. Azure AD B2C, talep çözümleyici değerini okur ve teknik profili içinde değerini kullanır. 

Aşağıdaki örnekte, bir talep türü adlı `correlationId` ile tanımlanmış bir **DataType** , `string`.  

```XML
<ClaimType Id="correlationId">
  <DisplayName>correlationId</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Request correlation Id</UserHelpText>
</ClaimType>
```

Teknik profili içinde talep çözümleyici talep türüne eşlenir. Azure AD B2C talep çözümleyici değerini doldurur `{Context:CorrelationId}` talep içine `correlationId` ve teknik profil için bir talep gönderir.

```XML
<InputClaim ClaimTypeReferenceId="correlationId" DefaultValue="{Context:CorrelationId}" />
```

## <a name="claim-resolver-types"></a>Talep türleri çözme

Aşağıdaki bölümlerde kullanılabilir talep Çözümleyicileri listelenmektedir.

### <a name="culture"></a>Kültür

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Kültür: LanguageName} | İki harfli ISO kod dili için. | tr |
| {Kültür: LCID}   | Dil kodu LCID'i. | 1033 |
| {Kültür: RegionName} | İki harfli ISO kod bölge için. | ABD |
| {Kültür: RFC5646} | RFC5646 dil kodu. | en-US |

### <a name="policy"></a>İlke

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {İlkesi: Policyıd} | Bağlı olan taraf ilke adı. | B2C_1A_signup_signin |
| {İlkesi: RelyingPartyTenantId} | Bağlı olan taraf İlkesi Kiracı kimliği. | Bilgisayarınızı tenant.onmicrosoft.com |
| {Policy:TenantObjectId} | Bağlı olan taraf İlkesi Kiracı nesnesi kimliği. | 00000000-0000-0000-0000-000000000000 |
| {Policy:TrustFrameworkTenantId} | Güven framework Kiracı kimliği. | Bilgisayarınızı tenant.onmicrosoft.com |

### <a name="openid-connect"></a>OpenID Connect

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {OIDC:AuthenticationContextReferences} |`acr_values` Sorgu dizesi parametresi. | Yok |
| {OIDC:ClientId} |`client_id` Sorgu dizesi parametresi. | 00000000-0000-0000-0000-000000000000 |
| {OIDC:DomainHint} |`domain_hint` Sorgu dizesi parametresi. | facebook.com |
| {OIDC:LoginHint} |  `login_hint` Sorgu dizesi parametresi. | someone@contoso.com |
| {OIDC:MaxAge} | `max_age`. | Yok |
| {OIDC:Nonce} |`Nonce` Sorgu dizesi parametresi. | defaultNonce |
| {OIDC:Prompt} | `prompt` Sorgu dizesi parametresi. | oturum açma |
| {OIDC:Resource} |`resource` Sorgu dizesi parametresi. | Yok |
| {OIDC:scope} |`scope` Sorgu dizesi parametresi. | openıd |

### <a name="context"></a>Bağlam

| İste | Açıklama | Örnek |
| ----- | ----------- | --------|
| {Bağlam: BuildNumber} | Kimlik deneyimi çerçevesi sürümü (derleme numarası).  | 1.0.507.0 |
| {Context:CorrelationId} | Bağıntı Kimliği  | 00000000-0000-0000-0000-000000000000 |
| {Context:DateTimeInUtc} |Tarih saat UTC diliminde saat.  | 10/10/2018'DEN 12:00:00 PM |
| {Context:DeploymentMode} |İlke dağıtım modu.  | Üretim |
| {Bağlam: IPAddress} | Kullanıcının IP adresi. | 11.111.111.11 |


### <a name="non-protocol-parameters"></a>Olmayan protokol parametreleri

OIDC veya OAuth2 bir isteğin parçası olarak dahil herhangi bir parametre adı içindeki kullanıcı yolculuğu bir talep eşlenebilir. Örneğin, uygulamadan gelen istek adı ile bir sorgu dizesi parametresi içerebilir `app_session`, `loyalty_number`, ya da herhangi bir özel sorgu dizesi.

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {OAUTH-KV:campaignId} | Bir sorgu dizesi parametresi. | hawaii |
| {OAUTH-KV:app_session} | Bir sorgu dizesi parametresi. | A3C5R |
| {OAUTH-KV:loyalty_number} | Bir sorgu dizesi parametresi. | 1234 |
| {OAUTH KV: herhangi bir özel sorgu dizesini} | Bir sorgu dizesi parametresi. | Yok |

### <a name="oauth2"></a>OAuth2

| İste | Açıklama | Örnek |
| ----- | ----------------------- | --------|
| {oauth2:access_token} | Erişim belirteci. | Yok |

## <a name="how-to-use-claim-resolvers"></a>Talep Çözümleyicileri kullanma

### <a name="restful-technical-profile"></a>RESTful teknik profili

İçinde bir [RESTful](restful-technical-profile.md) teknik profili, kullanıcının dil, ilke adı, kapsam ve istemci kimliği göndermek isteyebilirsiniz REST API özel iş mantığı çalıştırın ve gerekirse, yerelleştirilmiş hata iletisi yükseltmek bu taleplere göre. 

Aşağıdaki örnek, bir RESTful teknik profili gösterir:

```XML
<TechnicalProfile Id="REST">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app.azurewebsites.net/api/identity</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userLanguage" DefaultValue="{Culture:LCID}" />
    <InputClaim ClaimTypeReferenceId="policyName" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="scope" DefaultValue="{OIDC:scope}" />
    <InputClaim ClaimTypeReferenceId="clientId" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
  <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
</TechnicalProfile>
```

### <a name="direct-sign-in"></a>Doğrudan oturum açma

Talep Çözümleyicileri kullanarak, oturum açma adı veya doğrudan oturum açma, bir Microsoft hesabı Facebook veya LinkedIn gibi belirli sosyal kimlik sağlayıcısı için önceden doldurabilir. Daha fazla bilgi için [doğrudan Azure Active Directory B2C kullanarak oturum kümesi](direct-signin.md).

### <a name="dynamic-ui-customization"></a>Dinamik kullanıcı arabirimini özelleştirme

Azure AD B2C, böylece sayfa içeriği dinamik olarak oluşturulabilen HTML içerik tanımı uç noktalar için sorgu dizesi parametreleri geçirmek sağlar. Örneğin, web veya mobil uygulama geçirdiğiniz bir özel parametre temel Azure AD B2C kaydolma veya oturum açma sayfasında arka plan görüntüsü değiştirebilirsiniz. Daha fazla bilgi için [dinamik olarak Azure Active Directory B2C'de özel ilkeler kullanarak kullanıcı Arabirimi yapılandırma](active-directory-b2c-ui-customization-custom-dynamic.md). Dil parametresini temel alan, HTML sayfası da yerelleştirebilirsiniz veya istemci kimliği temel alınarak içeriği değiştirebilirsiniz.

Aşağıdaki örnek sorgu dizesinde adlı bir parametre geçirir **campaignId** değeriyle `hawaii`, **dil** kodunu `en-US`, ve **uygulama** İstemci Kimliğini temsil eden:

```XML
<UserJourneyBehaviors>
  <ContentDefinitionParameters>
    <Parameter Name="campaignId">{OAUTH-KV:campaignId}</Parameter>
    <Parameter Name="language">{Culture:RFC5646}</Parameter>
    <Parameter Name="app">{OIDC:ClientId}</Parameter>
  </ContentDefinitionParameters>
</UserJourneyBehaviors>
```

Sonuç olarak Azure AD B2C yukarıdaki parametreleri için HTML içerik sayfası gönderir:

```
/selfAsserted.aspx?campaignId=hawaii&language=en-US&app=0239a9cc-309c-4d41-87f1-31288feb2e82
```

### <a name="application-insights-technical-profile"></a>Application Insights teknik profili

Azure Application Insights ile talep çözümleyenler kullanıcı davranışları hakkında öngörü sahibi olabilir. Application Insights teknik profilde, Azure Application ınsights'ı kalıcı giriş talep gönderin. Daha fazla bilgi için [Application Insights'ı kullanarak Azure AD B2C kullanıcı davranışını izleme yolculuklarından](active-directory-b2c-custom-guide-eventlogger-appins.md). Aşağıdaki örnekte, ilke kimliği, bağıntı kimliği, dil ve istemci kimliği Azure Application Insights'a gönderir.

```XML
<TechnicalProfile Id="AzureInsights-Common">
  <DisplayName>Alternate Email</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
    <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:CorrelationId}" DefaultValue="{Context:CorrelationId}" />
    <InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
    <InputClaim ClaimTypeReferenceId="AppId" PartnerClaimType="{property:App}" DefaultValue="{OIDC:ClientId}" />
  </InputClaims>
</TechnicalProfile>
```
