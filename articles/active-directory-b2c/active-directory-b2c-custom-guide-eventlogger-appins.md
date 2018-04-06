---
title: Application Insights Azure AD B2C gelen olayları kullanarak kullanıcı davranışı izlemek | Microsoft Docs
description: Özel ilkeler (Önizleme) kullanarak Azure AD B2C kullanıcı Yolculuklar Application ınsights'ta olay günlüklerini etkinleştirmek için adım adım kılavuzu
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 3/15/2018
ms.author: davidmu
ms.openlocfilehash: 28c4cefdd7604dbddf6887dcf494ecea65d658f1
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="track-user-behavior-in-azure-ad-b2c-journeys-by-using-application-insights"></a>Application Insights'ı kullanarak Azure AD B2C Yolculuklar kullanıcı davranışı izlemek

Azure Active Directory B2C (Azure AD B2C) de Azure Application Insights ile çalışır. Bunlar, kullanıcı tarafından özel olarak oluşturulmuş Yolculuklar için ayrıntılı ve özelleştirilmiş olay günlüklerini sağlar. Bu makalede, böylece başlamak gösterilmektedir: 
* Kullanıcı davranışı hakkında fikir kazanırsınız.
* Kendi ilkelerinizi geliştirme veya üretim sorunlarını giderin.
* Performansı ölçme.
* Bildirimleri Application Insights oluşturun.

> [!NOTE]
> Bu özelliğin önizlemede değil.

## <a name="how-it-works"></a>Nasıl çalışır?
Azure AD B2C kimlik deneyimi Framework'te şimdi sağlayıcısı içeren `Handler="Web.TPEngine.Providers.UserJourneyContextProvider, Web.TPEngine, Version=1.0.0.0`. Olay verileri Azure AD B2C'ye sağlanan izleme anahtarını kullanarak doğrudan Application Insights'a gönderir. 

Teknik bir profili bu sağlayıcı B2C bir olaydan tanımlamak için kullanır. Profil olay, kaydedilecek talepleri ve izleme anahtarı adını belirtir. Bir olay sonrası için teknik profili daha sonra eklenen bir *orchestration adım* veya farklı bir *doğrulama teknik profili* özel kullanıcı gezisine içinde. 

Application Insights, bir kullanıcı oturumu kaydetmek için bir bağıntı kimliği kullanarak olayları birleştirebilirsiniz. Application Insights olayı ve oturum saniye cinsinden kullanılabilir hale getirir ve birçok görselleştirme, verme ve analitik araçlar sunar.



## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md). Bu makalede, özel ilke başlangıç paketi kullandığınız varsayılır. Ancak başlangıç paketi gerekli değildir.



## <a name="step-1-create-an-application-insights-resource-and-get-the-instrumentation-key"></a>1. Adım Application Insights kaynağı oluşturun ve izleme anahtarı edinme

Azure AD B2C ile Application Insights kullanırken, yalnızca bir kaynak oluşturmak ve bir izleme anahtarını elde etmek için gereksinimdir. Bir kaynak olarak oluşturduğunuz [Azure portal](https://portal.azure.com).

1. Azure portalında abonelik kiracınız içinde seçin **+ kaynak oluşturma**. Bu Kiracı Azure AD B2C kiracınızın değil.  
2. Aramak ve seçmek **Application Insights**.  
3. Kullanan bir kaynak oluşturmak **ASP.NET web uygulaması** olarak **uygulama türü**, tercihinize aboneliği altında.
4. Application Insights kaynağı oluşturduktan sonra açmak ve izleme anahtarını not edin. 

![Uygulama Öngörüler genel bakış ve izleme anahtarı](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-key.png)

Daha fazla bilgi için bkz: [tam Application Insights belgelerine](https://docs.microsoft.com/azure/application-insights/).

## <a name="step-2-add-new-claimtype-definitions-to-your-trust-framework-extension-file"></a>2. Adım Güven framework uzantısı dosyanıza yeni ClaimType tanımları ekleme

Başlatıcı paketinden uzantı dosyasını açın ve aşağıdaki öğeleri ekleyin `<BuildingBlocks>` düğümü. Dosya adı genellikle olduğu `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

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
      <!-- Additional claims used for passing claims to the Application Insights provider. -->
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

| Teknik profili       | Görev |
| ----------------------------- |:---------------------------------------------|
| AzureInsights-Common | Bir ortak parametrelerinin tümü dahil edilecek oluşturur `AzureInsights` teknik profilleri     | 
| JourneyContextForInsights   | Application Insights'ta oturum açar ve bir bağıntı kimliği gönderir |
| AzureInsights-SignInRequest     | Oluşturur bir `SignIn` olay bir oturum açma isteği alındığında talepler kümesi ile      | 
| AzureInsights-UserSignup | Oluşturur bir `UserSignup` kullanıcı oturumu-up/oturum açma gezisine kaydolma seçeneğinde tetikleyen olayı     | 
| AzureInsights-SignInComplete | bir belirteç bağlı olan taraf uygulaması gönderildiğinde bir kimlik doğrulama işlemi başarılı şekilde tamamlandığını kaydeder     | 

Profilleri uzantısı dosyasına starter paketinden bu öğelerine ekleyerek `<ClaimsProviders>` düğümü. Dosya adı genellikle olduğu `yourtenant.onmicrosoft.com-B2C_1A_TrustFrameworkExtensions.xml`.

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
            <!-- An input claim with PartnerClaimType="eventName" is required. The Application Insights provider uses it to create an event with the specified value. -->
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
            <!-- The Application Insights instrumentation key that will be used for logging the events. -->
            <Item Key="InstrumentationKey">xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</Item>
            <!-- A Boolean that indicates whether developer mode is enabled. This controls how events are buffered. In a development environment with minimal event volume, enabling developer mode results in events being sent immediately to Application Insights.   -->
            <Item Key="DeveloperMode">false</Item>
            <!-- A Boolean that indicates whether telemetry should be enabled or not.   -->
            <Item Key="DisableTelemetry ">false</Item>
          </Metadata>
          <InputClaims>
            <!-- Properties of an event are added through the syntax {property:NAME}, where NAME is the name of the property being added to the event. DefaultValue can be either a static value or a value that's resolved by one of the supported DefaultClaimResolvers. -->
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
<!-- Initialize a session with Application Insights. -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="JourneyContextForInsights" TechnicalProfileReferenceId="JourneyContextForInsights" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Çağrı `Azure-Insights-SignInRequest` orchestration bir oturum-açma/Kaydolma isteği alındı izlemek için 2. adım:

```xml
<!-- Track that we have received a sign-in request. -->
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
          </ClaimsExchanges>
        </OrchestrationStep>
```

Hemen *önce* `SendClaims` orchestration adım, çağıran yeni bir adım eklemek `Azure-Insights-UserSignup`. Kullanıcı bir oturumu-up/oturum açma gezisine kaydolma düğmesini seçtiğinde tetiklenir.

```xml
        <!-- Handles the user selecting the sign-up link in the local account sign-in page. -->
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
        <!-- Track that we have successfully sent a token. -->
        <OrchestrationStep Order="11" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
          </ClaimsExchanges>
        </OrchestrationStep>

```

> [!IMPORTANT]
> Yeni düzenleme adımlarının ekledikten sonra tüm tamsayılar 1'den N'ye atlamadan adımları sırayla numaralandırmak


## <a name="step-5-upload-your-modified-extensions-file-run-the-policy-and-view-events-in-application-insights"></a>5. Adım. Değiştirilmiş uzantıları dosyanızı karşıya yüklemek, ilke çalıştırın ve Application Insights olaylarını görüntüleme

Kaydedin ve yeni güven framework uzantısı dosyasını karşıya yükleyin. Ardından, bağlı olan taraf İlkesi uygulama ya da kullanım çağıran **Şimdi Çalıştır** Azure AD B2C arabiriminde. Saniye cinsinden, olaylar Application Insights'ta kullanılabilir.

1. Application Insights kaynağı, Azure Active Directory kiracınızda açın.
2. Seçin **olayları** içinde **kullanım** alt.
3. Ayarlama **sırasında** için **son bir saat** ve **tarafından** için **3 dakika**. Seçmeniz gerekebilir **yenileme** sonuçlarını görüntülemek için.

![Grafik için Application Insights kullanım olayları](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)





## <a name="next-steps"></a>Sonraki adımlar

Talep türlerini ve olayları gereksinimlerinize uyacak şekilde kullanıcı Yolculuğunuzun ekleyin. Olası talepleri, ek talep Çözümleyicileri kullanarak bir listesi aşağıda verilmiştir.

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

### <a name="openid-connect-specific-claims"></a>Openıd Connect özgü talepleri

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

### <a name="non-protocol-parameters-included-with-oidc-and-oauth2-requests"></a>OIDC ve OAuth2 isteklerini dahil olmayan protokol parametreleri

```xml
Referenced using { OAUTH-KV:Querystring parameter name }
```

Bir OIDC veya OAuth2 isteğinin bir parçası olarak dahil herhangi bir parametre adı bir talep kullanıcı gezisine eşlenebilir. Ardından olayı kaydedebilirsiniz. Örneğin, uygulama istekten adıyla bir sorgu dizesi parametresi içerebilir `app_session`, `loyalty_number`, veya `any_string`.

Örnek istek uygulamadan şöyledir:
```
https://login.microsoftonline.com/sampletenant.onmicrosoft.com/oauth2/v2.0/authorize?p=B2C_1A_signup_signin&client_id=e1d2612f-c2bc-4599-8e7b-d874eaca1ae1&nonce=defaultNonce&redirect_uri=https%3A%2F%2Fjwt.ms&scope=openid&response_type=id_token&prompt=login&app_session=0a2b45c&loyalty_number=1234567

```
Ekleyerek talepleri daha sonra ekleyebilirsiniz bir `InputClaim` Application Insights olayı öğesine:
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



