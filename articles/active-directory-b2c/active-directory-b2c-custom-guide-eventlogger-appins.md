---
title: Application Insights Azure Active Directory B2C gelen olayları kullanarak kullanıcı davranışı izlemek | Microsoft Docs
description: Özel ilkeler (Önizleme) kullanarak Azure AD B2C kullanıcı Yolculuklar Application ınsights'ta olay günlüklerini etkinleştirmek için adım adım kılavuzu
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 04/16/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 94d96af8db651a848ac092d1f8b85da4909427b7
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110124"
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Application Insights'ı kullanarak Azure AD B2C Yolculuklar kullanıcı davranışı izlemek

Azure Active Directory B2C (Azure AD B2C) de Azure Application Insights ile çalışır. Bunlar, kullanıcı tarafından özel olarak oluşturulmuş Yolculuklar için ayrıntılı ve özelleştirilmiş olay günlüklerini sağlar. Bu makalede, böylece başlamak gösterilmektedir:

* Kullanıcı davranışı hakkında fikir kazanırsınız.
* Kendi ilkelerinizi geliştirme veya üretim sorunlarını giderin.
* Performansı ölçme.
* Bildirimleri Application Insights oluşturun.

> [!NOTE]
> Bu özellik önizlemede.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD B2C kimlik deneyimi Framework'te şimdi sağlayıcısı içeren `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Olay verileri Azure AD B2C'ye sağlanan izleme anahtarını kullanarak doğrudan Application Insights'a gönderir.

Teknik bir profili bu sağlayıcı B2C bir olaydan tanımlamak için kullanır.  Profil olay, kaydedilecek talepleri ve izleme anahtarı adını belirtir.  Bir olay sonrası için teknik profili daha sonra eklenen bir `orchestration step` veya farklı bir `validation technical profile` özel kullanıcı gezisine içinde.

Application Insights, bir kullanıcı oturumu kaydetmek için bir bağıntı kimliği kullanarak olayları birleştirebilirsiniz. Application Insights olayı ve oturum saniye içinde kullanılabilir hale getirir ve birçok görselleştirme, verme ve analitik araçlar sunar.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md). Bu makalede, özel ilke başlangıç paketi kullandığınız varsayılır. Ancak başlangıç paketi gerekli değildir.

## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>1. Adım Application Insights kaynağı oluşturun ve izleme anahtarı edinme

Azure AD B2C ile Application Insights kullanırken, yalnızca bir kaynak oluşturmak ve bir izleme anahtarını elde etmek için gereksinimdir. Bir kaynak olarak oluşturduğunuz [Azure portalı.](https://portal.azure.com)

1. Azure portalında abonelik kiracınız içinde seçin **+ kaynak oluşturma**. Bu Kiracı Azure AD B2C kiracınızın değil.  
2. Aramak ve seçmek **Application Insights**.  
3. Kullanan bir kaynak oluşturmak **ASP.NET web uygulaması** olarak **uygulama türü**, tercihinize aboneliği altında.
4. Application Insights kaynağı oluşturduktan sonra açmak ve izleme anahtarını not edin.

![Uygulama Öngörüler genel bakış ve izleme anahtarı](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>2. Adım Güven framework uzantısı dosyanıza yeni ClaimType tanımları ekleme

Başlatıcı paketinden uzantı dosyasını açın ve aşağıdaki öğeleri ekleyin `<BuildingBlocks>` düğümü. Genellikle dosya adı. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

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

## <a name="step-3-add-new-technical-profiles-that-use-the-application-insights-provider"></a>3. Adım Application Insights sağlayıcısı kullanan yeni teknik profilleri ekleyin

Teknik profilleri Azure AD B2C, kimlik deneyimi Framework işlevlerde kabul edilebilir. Bu örnek bir oturumu açın ve olay gönderme için beş teknik profilleri tanımlar:

| Teknik profili | Görev |
| ----------------- | -----|
| AzureInsights-Common | Tüm AzureInsights teknik profilleri dahil edilecek parametreleri ortak bir dizi oluşturur | 
| JourneyContextForInsights | Application Insights'ta oturum açar ve bir bağıntı kimliği gönderir |
| AzureInsights-SignInRequest | Oluşturur bir `SignIn` olay bir oturum açma isteği alındığında talepler kümesi ile | 
| AzureInsights-UserSignup | Kullanıcı oturumu-up/oturum açma gezisine kaydolma seçeneğinde tetikleyen bir UserSignup olay oluşturur | 
| AzureInsights SignInComplete | Bir belirteç bağlı olan taraf uygulaması gönderildiğinde bir kimlik doğrulama işlemi başarılı şekilde tamamlandığını kaydeder | 

Profilleri uzantısı dosyasına starter paketinden bu öğelerine ekleyerek `<ClaimsProviders>` düğümü.  Genellikle dosya adı. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> İzleme anahtarını değiştirme `ApplicationInsights-Common` Application Insights kaynağınıza sağlar GUID teknik profili.

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

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>4. Adım. İçinde var olan bir kullanıcı gezisine Orchestration adımları gibi teknik profilleri için Application Insights ekleyin.

Çağrı `JournyeContextForInsights` orchestration adım 1:

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Çağrı `Azure-Insights-SignInRequest` orchestration bir oturum-açma/Kaydolma isteği alındı izlemek için 2. adım:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="2" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Hemen *önce* `SendClaims` orchestration adım, çağıran yeni bir adım eklemek `Azure-Insights-UserSignup`. Kullanıcı bir oturumu-up/oturum açma gezisine kaydolma düğmesini seçtiğinde tetiklenir.

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

Hemen sonra `SendClaims` orchestration adım, çağrı `Azure-Insights-SignInComplete`. Bu adımı başarıyla tamamlanan gezisine yansıtır.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="11" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Yeni düzenleme adımlarının ekledikten sonra tüm tamsayılar 1'den N'ye atlamadan adımları sırayla numaralandırmak


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>5. Adım. Değiştirilmiş uzantıları dosyanızı karşıya yüklemek, ilke çalıştırın ve Application Insights olaylarını görüntüleme

Kaydedin ve yeni güven framework uzantısı dosyasını karşıya yükleyin. Ardından, bağlı olan taraf İlkesi uygulama ya da kullanım çağıran `Run Now` Azure AD B2C arabiriminde. Saniye cinsinden, olaylar Application Insights'ta kullanılabilir.

1. Açık **Application Insights** Azure Active Directory kiracınızda kaynak.
2. Seçin **kullanım** > **olayları**.
3. Ayarlama **sırasında** için **son bir saat** ve **tarafından** için **3 dakika**.  Seçmeniz gerekebilir **yenileme** sonuçlarını görüntülemek için.

![Uygulama Öngörüler kullanım olayları Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

##  <a name="next-steps"></a>Sonraki adımlar

Talep türlerini ve olayları gereksinimlerinize uyacak şekilde kullanıcı Yolculuğunuzun ekleyin. İşte bir listesi ek talep Çözümleyicileri kullanarak olası talepler

### <a name="culture-specific-claims"></a>Kültüre özgü talepleri

```xml
Referenced using: {Culture:One of the property names below}
```

| İste | Tanım | Örnek |
| ----- | -----------| --------|
| LanguageName | İki harfli ISO kod dili için | tr |
| RegionName | İki bölge ISO kodunu harf | ABD |
| RFC5646 | RFC5646 dil kodu | tr-TR |
| LCID   | Dil kodu LCID | 31 |

### <a name="policy-specific-claims"></a>İlke özgü talepleri

```xml
Referenced using {Policy:One of the property names below}
```

| İste | Tanım | Örnek |
| ----- | -----------| --------|
| TrustFrameworkTenantId | Trustframework Kiracı kimliği | Yok |
| RelyingPartyTenantId | Bağlı olan taraf Kiracı kimliği | Yok |
| Policyıd | İlkenin ilke kimliği | Yok |
| TenantObjectId | İlke Kiracı nesne kimliği | Yok |

### <a name="openid-connect-specific-claims"></a>Openıd Connect özgü talepleri

```xml
Referenced using {OIDC:One of the property names below}
```

| İste | Openıdconnect parametresi | Örnek |
| ----- | ----------------------- | --------|
| Sor | Sor | Yok |
| LoginHint |  login_hint | Yok |
| DomainHint | domain_hint | Yok |
|  MaxAge | max_age | Yok |
| istemci kimliği | client_id | Yok |
| Kullanıcı adı | login_hint | Yok |
|  Kaynak | kaynak| Yok |
| AuthenticationContextReferences | acr_values | Yok |

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>OIDC & OAuth2 istekleriyle dahil olmayan protokol parametreleri

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Bir OIDC veya OAuth2 isteğinin bir parçası olarak dahil herhangi bir parametre adı bir talep kullanıcı gezisine eşlenebilir. Ardından olayı kaydedebilirsiniz. Örneğin, uygulama istekten adıyla bir sorgu dizesi parametresi içerebilir `app_session`, `loyalty_number` veya `any_string`.

Örnek istek uygulamadan şöyledir:

```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Ekleyerek talepleri daha sonra ekleyebilirsiniz bir `Input Claim` Application Insights olay öğesi. Bir olay özelliklerini {özellik: adı} sözdizimi ad özelliği olaya eklenen eklenir. Örneğin:

```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
```

### <a name="other-system-claims"></a>Diğer sistem talepler

Olayları olarak kaydetmek için kullanılabileceğiniz önce bazı sistem talep için talep paketi eklenmesi gerekir. Teknik profili `SimpleUJContext` bu talepler kullanılabilir olmadan önce bir düzenleme adım veya bir doğrulama teknik profili olarak çağrılmalıdır.

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


