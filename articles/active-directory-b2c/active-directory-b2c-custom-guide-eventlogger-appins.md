---
title: Olayları uygulama anlayışları'ndan Azure Active Directory B2C kullanarak kullanıcı davranışını izleme | Microsoft Docs
description: Özel ilkeler (Önizleme) kullanarak Azure AD B2C kullanıcı yolculuklarından Application ınsights'ta olay günlüklerinde etkinleştirmeyi öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/12/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0c2f9a2a3d431e2948c7d50541b576b23c3ece6a
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66507546"
---
# <a name="track-user-behavior-in-azure-active-directory-b2c-using-application-insights"></a>Azure Active Directory B2C, Application Insights kullanarak kullanıcı davranışını izleme

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory (Azure AD) B2C Azure Application Insights ile birlikte kullandığınızda, ayrıntılı alın ve olay günlükleri, kullanıcı yolculuklarından için özelleştirilebilir. Bu makalede şunları öğreneceksiniz:

* Kullanıcı davranışına dayalı Öngörüler elde edin.
* Kendi ilkelerinizi geliştirme veya üretim sorunlarını giderin.
* Performansı ölçme.
* Bildirimleri uygulama anlayışları'ndan oluşturun.

## <a name="how-it-works"></a>Nasıl çalışır?

Azure AD B2C'de kimlik deneyimi çerçevesi sağlayıcısı içerir `Handler="Web.TPEngine.Providers.AzureApplicationInsightsProvider, Web.TPEngine, Version=1.0.0.0`. Bu olay verilerini doğrudan Application Insights'a Azure AD B2C'ye sağlanan izleme anahtarını kullanarak gönderir.

Teknik profili, bir Azure AD B2C olaydan tanımlamak için bu sağlayıcıyı kullanır. Profil, olay, kaydedilen talepleri ve izleme anahtarını adını belirtir. Bir olay göndermek için teknik profil ardından olarak eklenen bir `orchestration step`, ya da farklı bir `validation technical profile` özel kullanıcı yolculuğu içinde.

Application Insights olayları, bir kullanıcı oturumu kaydetmek için bir bağıntı kimliği kullanarak birleştirebilirsiniz. Application Insights, olay ve oturumunun saniye içinde kullanılabilir hale getirir ve pek çok görselleştirme, dışarı aktarma ve analiz araçları sunar.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md). Bu makalede, özel ilke başlangıç paketi kullandığınız varsayılır. Ancak başlangıç paketi gerekli değildir.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Azure AD B2C ile Application Insights'ı kullanırken, yapmak için ihtiyacınız olan bir kaynak oluşturmak ve izleme anahtarını alın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure aboneliğinizi tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve aboneliğinizi içeren dizine seçme. Bu Kiracı Azure AD B2C kiracınızı değil.
3. Seçin **kaynak Oluştur** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Application Insights**.
4. **Oluştur**’a tıklayın.
5. Girin bir **adı** kaynak için.
6. İçin **uygulama türü**seçin **ASP.NET web uygulaması**.
7. İçin **kaynak grubu**, varolan bir grubu seçin veya yeni bir grup için bir ad girin.
8. **Oluştur**’a tıklayın.
4. Application Insights kaynağı oluşturduktan sonra açmak için genişletme **Essentials**, izleme anahtarını kopyalayın.

![Application Insights'a genel bakış ve izleme anahtarı](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-insights.png)

## <a name="add-new-claimtype-definitions"></a>Yeni ClaimType tanımları ekleme

Açık *TrustFrameworkExtensions.xml* aşağıdaki öğeleri ekleyin ve dosya başlangıç Paketi'nden [BuildingBlocks](buildingblocks.md) öğesi:

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

## <a name="add-new-technical-profiles"></a>Yeni teknik Profil ekleme

Kimlik deneyimi çerçevesi, Azure AD B2C işlevlerde teknik profiller kabul edilebilir. Bu tabloda bir oturumu açın ve olayları kullanılan teknik profiller tanımlar.

| Teknik profili | Görev |
| ----------------- | -----|
| AzureInsights-Common | Tüm AzureInsights teknik profilleri dahil edilecek parametreleri ortak bir dizi oluşturur. | 
| AzureInsights-SignInRequest | Oturum açma isteği alındığında talepler kümesi bir oturum açma olayı oluşturur. | 
| AzureInsights-UserSignup | Kullanıcı oturumu açma kaydolma/oturum açma yolculuğu kaydolma seçeneği tetiklendiğinde UserSignup olay oluşturur. | 
| AzureInsights SignInComplete | Bir kimlik doğrulaması başarılı olarak tamamlanmasına bağlı taraf uygulaması için bir belirteç gönderildiğinde kaydeder. | 

Profillerine ekler *TrustFrameworkExtensions.xml* başlangıç paketi dosyasından. Bu öğeleri eklemek **ClaimsProviders** öğesi:

```xml
<ClaimsProvider>
  <DisplayName>Application Insights</DisplayName>
  <TechnicalProfiles>
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

> [!IMPORTANT]
> İzleme anahtarını değiştirme `ApplicationInsights-Common` teknik profil için Application Insights kaynağınıza sağlayan GUID.

## <a name="add-the-technical-profiles-as-orchestration-steps"></a>Teknik profil düzenleme adımlarının Ekle

Çağrı `Azure-Insights-SignInRequest` düzenleme oturum-içinde açma/Kaydolma isteği alındı izlemek için 2. adım:

```xml
<!-- Track that we have received a sign in request -->
<OrchestrationStep Order="1" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInRequest" TechnicalProfileReferenceId="AzureInsights-SignInRequest" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Hemen *önce* `SendClaims` düzenleme adımı, çağıran yeni adım Ekle `Azure-Insights-UserSignup`. Kullanıcı yolculuğu oturumu-kaydolma/oturum açma kaydolma düğmeyi seçtiğinde tetiklenir.

```xml
<!-- Handles the user clicking the sign up link in the local account sign in page -->
<OrchestrationStep Order="8" Type="ClaimsExchange">
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
</OrchestrationStep>
```

Hemen sonra `SendClaims` düzenleme adımı, çağrı `Azure-Insights-SignInComplete`. Bu adım başarıyla tamamlanan bir yolculuk gösterir.

```xml
<!-- Track that we have successfully sent a token -->
<OrchestrationStep Order="10" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="TrackSignInComplete" TechnicalProfileReferenceId="AzureInsights-SignInComplete" />
  </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Yeni düzenleme adımlarının ekledikten sonra herhangi bir tamsayı 1'den N'ye atlamadan adımları sırayla numaralandırmak


## <a name="upload-your-file-run-the-policy-and-view-events"></a>Dosyanızı karşıya yüklemek, ilke ve olaylarını görüntüleme

Kaydet ve karşıya yükleme *TrustFrameworkExtensions.xml* dosya. Ardından, bağlı olan taraf İlkesi uygulama ya da kullanım çağrı **Şimdi Çalıştır** Azure portalında. Saniye cinsinden olaylarınızı uygulama anlayışları'nda kullanılabilir.

1. Açık **Application Insights** Azure Active Directory kiracınızda bulunan kaynak.
2. Seçin **kullanım** > **olayları**.
3. Ayarlama **sırasında** için **son bir saat** ve **tarafından** için **3 dakika**.  Seçmeniz gerekebilir **Yenile** sonuçlarını görüntülemek için.

![Application Insights kullanım-olayları Blase](./media/active-directory-b2c-custom-guide-eventlogger-appins/app-ins-graphic.png)

## <a name="next-steps"></a>Sonraki adımlar

Kendi gereksinimlerinize uyacak şekilde, kullanıcı yolculuğunun talep türlerini ve olaylar ekleyin. Kullanabileceğiniz [Çözümleyicileri talep](claim-resolver-overview.md) veya talep türü, talepleri ekleyerek herhangi bir dize bir **giriş talep** Application Insights olayı veya AzureInsights yaygın teknik profiline öğesi. 

- **ClaimTypeReferenceId** bir talep türü başvurudur.
- **PartnerClaimType** Azure Insights'ta görünür özelliğin adıdır. Söz dizimi kullanın `{property:NAME}`burada `NAME` olaya eklenen özellik. 
- **DefaultValue** herhangi bir dize değeri veya talep Çözümleyicisi'ni kullanın. 

```XML
<InputClaim ClaimTypeReferenceId="app_session" PartnerClaimType="{property:app_session}" DefaultValue="{OAUTH-KV:app_session}" />
<InputClaim ClaimTypeReferenceId="loyalty_number" PartnerClaimType="{property:loyalty_number}" DefaultValue="{OAUTH-KV:loyalty_number}" />
<InputClaim ClaimTypeReferenceId="language" PartnerClaimType="{property:language}" DefaultValue="{Culture:RFC5646}" />
```

