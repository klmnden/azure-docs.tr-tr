---
title: Olayları uygulama anlayışları'ndan Azure Active Directory B2C kullanarak kullanıcı davranışını izleme | Microsoft Docs
description: Azure AD B2C kullanıcı yolculuklarından olay günlüklerinde Application ınsights'ta özel ilkeler (Önizleme) kullanarak etkinleştirmek için adım adım kılavuzu
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 04/16/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c77feed3b86358c74f741b53aa03ecb454dc9a62
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2018
ms.locfileid: "43337111"
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Application Insights'ı kullanarak Azure AD B2C yolculuklarından kullanıcı davranışını izleme

Azure Active Directory B2C (Azure AD B2C), Azure Application Insights ile de çalışır. Bunlar, kullanıcı tarafından özel olarak oluşturulmuş yolculuklarından için ayrıntılı ve özelleştirilmiş olay günlüklerini sağlar. Bu makalede, böylece başlama işlemini gösterir:

* Kullanıcı davranışına dayalı Öngörüler elde edin.
* Kendi ilkelerinizi geliştirme veya üretim sorunlarını giderin.
* Performansı ölçme.
* Bildirimleri uygulama anlayışları'ndan oluşturun.

> [!NOTE]
> Bu özellik önizlemede.

## <a name="how-it-works"></a>Nasıl çalışır?

Artık yer sağlayıcısı olarak Azure AD B2C kimlik deneyimi çerçevesi `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Bu olay verilerini doğrudan Application Insights'a Azure AD B2C'ye sağlanan izleme anahtarını kullanarak gönderir.

Teknik profili, bir olay B2C tanımlamak için bu sağlayıcısı kullanır.  Profil, olay, kaydedilecek olan talepleri ve izleme anahtarını adını belirtir.  Bir olay göndermek için teknik profil ardından olarak eklenen bir `orchestration step` veya farklı bir `validation technical profile` özel kullanıcı yolculuğu içinde.

Application Insights olayları, bir kullanıcı oturumu kaydetmek için bir bağıntı kimliği kullanarak birleştirebilirsiniz. Application Insights, olay ve oturumunun saniye içinde kullanılabilir hale getirir ve pek çok görselleştirme, dışarı aktarma ve analiz araçları sunar.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md). Bu makalede, özel ilke başlangıç paketi kullandığınız varsayılır. Ancak başlangıç paketi gerekli değildir.

## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>1. Adım Bir Application Insights kaynağı oluşturun ve izleme anahtarı edinme

Azure AD B2C ile Application Insights'ı kullanırken, bir kaynak oluşturmak ve bir izleme anahtarı edinmek için tek gereksinim olmasıdır. Kaynak oluşturma [Azure portalı.](https://portal.azure.com)

1. Azure portalında abonelik kiracınızdaki seçin **+ kaynak Oluştur**. Bu Kiracı Azure AD B2C kiracınızı değil.  
2. Arayın ve seçin **Application Insights**.  
3. Kullanan Kaynak Oluştur **ASP.NET web uygulaması** olarak **uygulama türü**, tercihinize bir aboneliği altında.
4. Application Insights kaynağı oluşturduktan sonra açın ve izleme anahtarını not edin.

![Application Insights'a genel bakış ve izleme anahtarı](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>2. Adım Yeni ClaimType tanımları güven framework uzantısı dosyanıza ekleyin

Başlangıç Paketi'nden uzantı dosyasını açın ve aşağıdaki öğeleri ekleyin `<BuildingBlocks>` düğümü. Genellikle dosya adı. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

```xml
<ClaimsSchema>
  <ClaimType Id="EventType">
    <DisplayName>EventType</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="PolicyId">
    <DisplayName>PolicyId</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="Culture">
    <DisplayName>Culture</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="CorrelationId">
    <DisplayName>CorrelationId</DisplayName>
    <DataType>string</DataType>
    <AdminHelpText />
    <UserHelpText />
  </ClaimType>
  <!--Additional claims used for passing claims to Application Insights Provider -->
  <ClaimType Id="federatedUser">
    <DisplayName>federatedUser</DisplayName>
    <DataType>boolean</DataType>
    <UserHelpText />
  </ClaimType>
  <ClaimType Id="parsedDomain">
    <DisplayName>Parsed Domain</DisplayName>
    <DataType>string</DataType>
    <UserHelpText>The domain portion of the email address.</UserHelpText>
  </ClaimType>
  <ClaimType Id="userInLocalDirectory">
    <DisplayName>userInLocalDirectory</DisplayName>
    <DataType>boolean</DataType>
    <UserHelpText />
  </ClaimType>
</ClaimsSchema>
```

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>3. Adım Application Insights sağlayıcısı kullanan yeni bir teknik Profil ekleme

Kimlik deneyimi çerçevesi, Azure AD B2C işlevlerde teknik profiller kabul edilebilir. Bu örnek, bir oturumu açın ve olayları beş teknik profiller tanımlar:

| Teknik profili | Görev |
| ----------------- | -----|
| AzureInsights-Common | Tüm AzureInsights teknik profilleri dahil edilecek parametreleri ortak bir dizi oluşturur. | 
| JourneyContextForInsights | Uygulama anlayışları'nda oturum açar ve bir bağıntı kimliği gönderir |
| AzureInsights-SignInRequest | Oluşturur bir `SignIn` bir oturum açma isteği alındığında talepler kümesi içeren olay | 
| AzureInsights-UserSignup | Kullanıcı oturumu açma kaydolma/oturum açma yolculuğu kaydolma seçeneği tetiklendiğinde UserSignup olay oluşturur | 
| AzureInsights SignInComplete | Bir kimlik doğrulaması başarılı olarak tamamlanmasına bağlı taraf uygulaması için bir belirteç gönderildiğinde kaydeder | 

Profiller, bu öğelere ekleyerek başlangıç Paketi'nden uzantısı dosyaya ekleyin `<ClaimsProviders>` düğümü.  Genellikle dosya adı. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> İzleme anahtarını değiştirme `ApplicationInsights-Common` teknik profil için Application Insights kaynağınıza sağlayan GUID.

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="JourneyContextForInsights">
      <DisplayName>Application Insights</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="CorrelationId" />
      </OutputClaims>
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-SignInRequest">
      <InputClaims>
        <!-- An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider to create an event with the specified value. -->
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInRequest" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-SignInComplete">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="SignInComplete" />
        <InputClaim ClaimTypeReferenceId="federatedUser" PartnerClaimType="{property:FederatedUser}" DefaultValue="false" />
        <InputClaim ClaimTypeReferenceId="parsedDomain" PartnerClaimType="{property:FederationPartner}" DefaultValue="Not Applicable" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-UserSignup">
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="EventType" PartnerClaimType="eventName" DefaultValue="UserSignup" />
      </InputClaims>
      <IncludeTechnicalProfile ReferenceId="AzureInsights-Common" />
    </TechnicalProfile>
    <TechnicalProfile Id="AzureInsights-Common">
      <DisplayName>Alternate Email</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.Insights.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <!-- The ApplicationInsights instrumentation key which will be used for logging the events -->
        <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
        <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights. -->
        <Item Key="DeveloperMode">false</Item>
        <!-- A Boolean that indicates whether telemetry should be enabled or not. -->
        <Item Key="DisableTelemetry ">false</Item>
      </Metadata>
      <InputClaims>
        <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
        <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
        <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
        <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
      </InputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>4. Adım. Var olan bir kullanıcı yolculuğunda düzenleme adımı gibi teknik profiller için Application Insights ekleyin.

Çağrı `JournyeContextForInsights` düzenleme adımı 1 olarak:

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Çağrı `Azure-Insights-SignInRequest` düzenleme oturum-içinde açma/Kaydolma isteği alındı izlemek için 2. adım:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Hemen *önce* `SendClaims` düzenleme adımı, çağıran yeni adım Ekle `Azure-Insights-UserSignup`. Kullanıcı yolculuğu oturumu-kaydolma/oturum açma kaydolma düğmeyi seçtiğinde tetiklenir.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="9" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
      <Value>newUser</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
      <Value>newUser</Value>
      <Value>false</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackUserSignUp" TechnicalProfileReferenceId="AzureInsights-UserSignup" />
  </ClaimsExchanges>
```

Hemen sonra `SendClaims` düzenleme adımı, çağrı `Azure-Insights-SignInComplete`. Bu adımı başarıyla tamamlanan bir yolculuk yansıtır.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="11" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Yeni düzenleme adımlarının ekledikten sonra herhangi bir tamsayı 1'den N'ye atlamadan adımları sırayla numaralandırmak


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>5. Adım. Değiştirilen uzantıları dosyanızı karşıya yüklemek, ilke çalıştırın ve Application Insights olayları görüntülemek

Kaydet ve yeni güven framework uzantı dosyasını karşıya yükleyin. Ardından, bağlı olan taraf İlkesi uygulama ya da kullanım çağrı `Run Now` Azure AD B2C arabiriminde. Saniye cinsinden olaylarınızı uygulama anlayışları'nda kullanılabilir.

1. Açık **Application Insights** Azure Active Directory kiracınızda bulunan kaynak.
2. Seçin **kullanım** > **olayları**.
3. Ayarlama **sırasında** için **son bir saat** ve **tarafından** için **3 dakika**.  Seçmeniz gerekebilir **Yenile** sonuçlarını görüntülemek için.

![Application Insights kullanım-olayları Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

##  <a name="next-steps"></a>Sonraki adımlar

Kendi gereksinimlerinize uyacak şekilde, kullanıcı yolculuğunun talep türlerini ve olaylar ekleyin. Ek talep Çözümleyicileri kullanarak olası talep bir listesine buradan ulaşabilirsiniz

### <a name="culture-specific-claims"></a>Kültüre özgü talep

```xml
Referenced using: {Culture:One of the property names below}
```

| İste | Tanım | Örnek |
| ----- | -----------| --------|
| LanguageName | İki harfli ISO kod dili için | tr |
| RegionName | İki harfli ISO kod bölge için | ABD |
| RFC5646 | RFC5646 dil kodu | tr-TR |
| LCID   | Dil kodu LCID | 31 |

### <a name="policy-specific-claims"></a>İlke özgü talep

```xml
Referenced using {Policy:One of the property names below}
```

| İste | Tanım | Örnek |
| ----- | -----------| --------|
| TrustFrameworkTenantId | Trustframework Kiracı kimliği | Yok |
| RelyingPartyTenantId | Bağlı olan taraf Kiracı kimliği | Yok |
| Policyıd | İlkenin ilke kimliği | Yok |
| TenantObjectId | İlke Kiracı nesnesi kimliği | Yok |

### <a name="openid-connect-specific-claims"></a>Openıd Connect özgü talep

```xml
Referenced using {OIDC:One of the property names below}
```

| İste | Openıdconnect parametresi | Örnek |
| ----- | ----------------------- | --------|
| istemi | istemi | Yok |
| LoginHint |  login_hint | Yok |
| DomainHint | domain_hint | Yok |
|  MaxAge | max_age | Yok |
| ClientID | client_id | Yok |
| Kullanıcı adı | login_hint | Yok |
|  Kaynak | kaynak| Yok |
| AuthenticationContextReferences | acr_values | Yok |

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>OIDC & OAuth2 isteklerle olmayan protokol parametreleri

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

OIDC veya OAuth2 bir isteğin parçası olarak dahil herhangi bir parametre adı içindeki kullanıcı yolculuğu bir talep eşlenebilir. Daha sonra olay kaydedebilirsiniz. Örneğin, uygulamadan gelen istek adı ile bir sorgu dizesi parametresi içerebilir `app_session`, `loyalty_number` veya `any_string`.

Bir örnek uygulamadan gelen istek şu şekildedir:

```
https://sampletenant.b2clogin.com/tfp/sampletenant.onmicrosoft.com/B2C_1A_signup_signin/oauth2/v2.0/authorize?client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Sonra ekleyerek talep ekleyebilirsiniz bir `Input Claim` Application Insights olayı öğesi. Bir olay özelliklerini, söz dizimi aracılığıyla {özelliği: NAME}, adı özelliği için olay eklenirse eklenir. Örneğin:

```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Diğer sistem talepler

Olayları kaydetmek kullanılabilir olmadan önce bazı sistem talep için talep paketi eklenmesi gerekir. Teknik profil `SimpleUJContext` düzenleme adımı ya da doğrulama teknik profili bu talepler kullanılabilir olmadan önce çağrılmalıdır.

```xml
<ClaimsProvider>
  <DisplayName>User Journey Context Provider</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="SimpleUJContext">
      <DisplayName>User Journey Context Provide</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="IP-Address" />
        <OutputClaim ClaimTypeReferenceId="CorrelationId" />
        <OutputClaim ClaimTypeReferenceId="DateTimeInUtc" />
        <OutputClaim ClaimTypeReferenceId="Build" />
      </OutputClaims>
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```


