---
title: Azure AD B2C uygulaması bilgiler olayları kullanarak kullanıcı davranışı izlemek nasıl | Microsoft Docs
description: Özel ilkeler - Önizleme kullanarak Azure AD B2C kullanıcı Yolculuklar Application ınsights'ta olay günlüklerini etkinleştirmek için adım adım kılavuzu
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 1e8c4a53266072db71952ee35b15e5de54fabb95
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="tracking-user-behavior-inside-azure-ad-b2c-journeys-using-application-insights"></a>Application Insights kullanarak Azure AD B2C Yolculuklar içindeki kullanıcı davranışını izleme

Azure Active Directory B2C de Azure Application Insights ile çalışır.  Bunlar, özel oluşturulan kullanıcı Yolculuklar için ayrıntılı ve özelleştirilmiş olay günlüklerini sağlar.  Bu kılavuz, böylece başlayacağınızı gösterir: 
* Kullanıcı davranışına ilişkin Öngörüler elde edin
* Geliştirme veya üretim kendi ilke sorunlarını giderme
* Ölçü performansı
* Application Insights bildirimler oluşturma

## <a name="how-it-works"></a>Nasıl çalışır?
Yeni bir sağlayıcı için Azure AD B2C'in kimlik deneyimi Framework eklendi: `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`.  Olay verileri doğrudan Azure AD B2C'ye sağlanan izleme anahtarını kullanarak Application Insights gönderir.  Teknik bir profili bu sağlayıcı B2C bir olaydan tanımlamak için kullanır.  Profil olay, kaydedilecek talepleri ve izleme anahtarı adını belirtir.  Bir olay sonrası için teknik profili sonra am olarak eklenen `orchestration step` veya farklı bir `validation technical profile` özel kullanıcı gezisine içinde.  Olaylar, bir kullanıcı oturumu kaydetmek için bir bağıntı kimliği'ni kullanarak Application Insights tarafından birleştirilmiş.  Application Insights olayı ve oturum saniye içinde kullanılabilir hale getirir ve birçok görselleştirme, verme ve analitik araçlar sunar.



## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md).  Bu kılavuz, özel ilke başlangıç paketi kullanılmakta olduğunu varsayar.  Ancak gerekli değildir.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>1. Adım Application Insights kaynağı oluşturun ve izleme anahtarı edinme

Application Insights olan güçlü bir araçtır. Azure AD B2C ile kullanırken, yalnızca bir kaynak oluşturmak ve bir izleme anahtarını elde etmek için gereksinimdir.  Application Insights, içinde oluşturulmalıdır [Azure portalı.](https://portal.azure.com)

[Application Insights için tam belgeleri](https://docs.microsoft.com/azure/application-insights/)

1. Tıklayın `+ Create a resource` Azure portalında abonelik Kiracı içinde.  Bu Kiracı Azure AD B2C kiracınızın değil.  
2. Arayın ve seçin `Application Insights`  
3. Bir kaynak oluşturmak `Application Type` `ASP.NET web application` tercihinizi aboneliği altında.
4. Oluşturulan, Application Insights kaynağı ve Not açın  `Instrumentation Key` 

![Uygulama Öngörüler genel bakış ve izleme anahtarı](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>2. Adım Güven Framework uzantısı dosyanıza yeni ClaimType tanımları ekleme

### <a name="open-the-extension-file-from-the-starter-pack-and-add-the-following-elements-to-the-buildingblocks-node--the-extensions-filename-is-typically-yourtenantonmicrosoftcom-b2c1atrustframeworkextensionsxml"></a>Başlatıcı paketinden uzantı dosyasını açın ve aşağıdaki öğeleri ekleyin `<BuildingBlocks>` düğümü.  Uzantıları filename genellikle. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

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

Teknik profilleri Azure AD B2C'in kimlik deneyimi Framework işlevlerde kabul.  Beş teknik profili bir oturumu açın ve olay gönderme için bu örnekte tanımlanır.

| Teknik profili       | Görev |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | Tüm Azure Öngörüler teknik profilleri dahil edilecek ortak parametrelerinin     | 
| JourneyContextForInsights   | App Insights oturumu açar ve bir bağıntı kimliği gönderir |
| AzureInsights-SignInRequest     | bir oturum açma isteği alındığında talepler kümesi bir "Signın" olay oluşturur      | 
| AzureInsights-UserSignup | kaydolma seçeneği kaydolma/signın gezisine kullanıcı tarafından tetiklenen zaman "UserSignup" adlı bir olay oluşturur     | 
| AzureInsights-SignInComplete | bir belirteç bağlı olan taraf uygulaması gönderildiğinde bir kimlik doğrulama işlemi başarılı şekilde tamamlandığını kaydeder     | 

Profilleri uzantısı dosyasına starter paketinden bu öğelerine ekleyerek `<ClaimsProviders>` düğümü.  Uzantıları filename genellikle. `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`

> [!IMPORTANT]
> Değişiklik `Instrumentation Key` içinde `ApplicationInsights-Common` teknik profiline Application Insights kaynağınıza tarafından sağlanan GUID.

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
            <!-- 
                            An input claim with a PartnerClaimType="eventName" is required. This is used by the AzureApplicationInsightsProvider
                            to create an event with the specified value.
                        -->
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
            <!-- 
                            A boolean indicating whether delevoper mode is enabled. This controls how events are buffered. In a development environment
                            with minimal event volume, enabling developer mode results in events being sent immediately to ApplicationInsights.
                        -->
            <Item Key="DeveloperMode">false</Item>
            <!-- 
                            A boolean indicating whether telemetry should be enabled or not.
                        -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!--
                            Properties of an event are added using the syntax {property:NAME} where NAME is the name of the property being added
                            to the event. DefaultValue can be either a static value or one resolved by one of the supported DefaultClaimResolvers.
                        -->
            <InputClaim ClaimTypeReferenceId="PolicyId" PartnerClaimType="{property:Policy}" DefaultValue="{Policy:PolicyId}" />
            <InputClaim ClaimTypeReferenceId="CorrelationId" PartnerClaimType="{property:JourneyId}" />
            <InputClaim ClaimTypeReferenceId="Culture" PartnerClaimType="{property:Culture}" DefaultValue="{Culture:RFC5646}" />
          </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

```

## <a name="step-4-add-the-technical-profiles-for-application-insights-as-orchestration-steps-in-an-existing-user-journey"></a>4. Adım. İçinde var olan bir kullanıcı gezisine Orchestration adımları gibi teknik profilleri için Application Insights ekleyin.

Çağrı `JournyeContextForInsights` orchestration adım 1 olarak

```xml
<!-- Initialize a session with Application Insights -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Çağrı `Azure-Insights-SignInRequest` orchestration bir oturum-açma/Kaydolma isteği alındı izlemek için 2. adım.

```xml
<!-- Track that we have received a sign in request -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Hemen **önce** `SendClaims` orchestration adım, çağıran yeni bir adım eklemek `Azure-Insights-UserSignup`. Bir kaydolma/signın gezisine kaydolma düğme tıklatıldığında tetiklenir.

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

Hemen sonra `SendClaims` orchestration adım, çağrı `Azure-Insights-SignInComplete`.   Bu adımı başarıyla tamamlanan gezisine yansıtır.

```xml
        <!-- Track that we have successfully sent a token -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Yeni düzenleme adımlarının eklendikten sonra adımları sırayla herhangi tamsayılar 1'den N atlamadan yeniden numaralandır


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>5. Adım. Değiştirilmiş uzantıları dosyanızı karşıya yüklemek, ilke çalıştırın ve Application Insights olaylarını görüntüleme

Kaydedin ve yeni güven framework uzantıları dosyasını karşıya yükleyin.  Ardından, bağlı olan taraf ilke uygulama ya da kullanım çağıran `Run Now` Azure AD B2C arabiriminde.  Saniye içinde olaylarınızı Application Insights'ta kullanılabilir.

1. Application Insights kaynağı, Azure Active Directory kiracınızda açın.
2. Tıklayın `Events` içinde `USAGE` alt.
3. Ayarlama `During` için `Last hour` ve `By` için `3 minutes`.  ' İ tıklatmanız gerekir `Refresh` sonuçlarını görüntülemek için

![Uygulama Öngörüler kullanım olayları Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





##  <a name="next-steps"></a>SONRAKİ ADIMLAR

Ek talep türlerini ve olayları gereksinimlerinize uyacak şekilde kullanıcı Yolculuğunuzun ekleyin.  Olası talepleri ek talep Çözümleyicileri kullanarak bir listesi aşağıda verilmiştir.

### <a name="culture-specific-claims"></a>Kültüre özgü talepleri

```xml
Culture-specific Claims
            Referenced using {Culture:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="JourneyCultureDefaultClaimProcessor"/>
        public enum SupportedClaim
        {
             /// The two letter ISO code for the language i.e. en
            LanguageName
             
            /// The two letter ISO code for the region i.e. US
            RegionName,
 
            /// The RFC5646 language code i.e. en-US
            RFC5646,

            /// The LCID of language code i.e. 1033
            LCID
        }

```

### <a name="policy-specific-claims"></a>İlke özgü talepleri

```xml
Policy-specific Claims
Referenced using {Policy:One of the property names below}
 
        /// An enumeration of the claims supported by the <see cref="PolicyDefaultClaimProcessor"/> 
        public enum SupportedClaim
        {
            /// The trustframework tenant id
            TrustFrameworkTenantId,
  
            /// The tenant id of the relying party
            RelyingPartyTenantId,
 
            /// The policy id of the policy
            PolicyId,
 
            /// The tenant object id of the policy
            TenantObjectId
}

```

### <a name="openidconnect-specific-claims"></a>Openıdconnect özgü talepleri

```xml
OpenIDConnect Specific Claims
Referenced using {OIDC:One of the property names below}
 
/// 
        /// An enumeration of the claims supported by the <see cref="OpenIdConnectDefaultClaimProcessor"/>

        public enum SupportedClaim
        {
            /// The OpenIdConnect prompt parameter
            Prompt,
 
            /// The OpenIdConnect login_hint parameter
            LoginHint,

            /// The OpenIdConnect domain_hint parameter
            DomainHint,
 
             /// The OpenIdConnect max_age parameter
            MaxAge,
 
            /// The OpenIdConnect client_id parameter
            ClientId,
 
            /// The OpenIdConnect username parameter
            Username,

            /// The OpenIdConnect password parameter
            Password,
 
            /// The OpenIdConnect resource type parameter
            Resource,
 
            /// The OpendIdConext acr_values parameter
             AuthenticationContextReferences
        }
 
```

### <a name="non-protocol-parameters-included-with-oidc--oauth2-requests"></a>OIDC & OAuth2 istekleriyle dahil olmayan protokol parametreleri

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Bir OIDC veya OAuth2 isteğinin bir parçası olarak dahil herhangi bir parametre adı bir talep kullanıcı gezisine eşlenebilir.  Ardından olayı kaydedilebilir. Örneğin, uygulama istekten adıyla bir sorgu dizesi parametresi içerebilir `app_session`, `loyalty_number` veya `any_string`.

Örnek istek uygulamadan:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Application Insights olayını kullanarak bir giriş talep öğe ekleyerek sonra bu talepleri eklenebilmesi için:
```
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="app_session" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="loyalty_number" DefaultValue="{OAUTH-KV:loyalty_number}" />
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



