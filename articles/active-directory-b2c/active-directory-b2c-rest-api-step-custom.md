---
title: Değişimleri - Azure Active Directory B2C REST API talepleri
description: Özel ilkeler, Active Directory B2C REST API talep değişimleri ekleyin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 0bdef508e12a3b11143149b330da73838b53f860
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439009"
---
# <a name="add-rest-api-claims-exchanges-to-custom-policies-in-azure-active-directory-b2c"></a>Özel ilkeleri Azure Active Directory B2C REST API talep değişimleri ekleyin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bir RESTful API'sine etkileşim ekleyebilirsiniz, [özel ilkeler](active-directory-b2c-overview-custom.md) Azure Active Directory (Azure AD) B2C'de. Bu makalede, RESTful Hizmetleri ile etkileşime giren bir Azure AD B2C kullanıcı yolculuğu oluşturulacağını gösterir.

Etkileşim, bir talep değişimi REST API talepler ve Azure AD B2C arasındaki bilgi içerir. Talep değişimleri, aşağıdaki özelliklere sahiptir:

- Bir düzenleme adımı tasarlanmış olabilir.
- Dış bir eylem tetikleyebilirsiniz. Örneğin, bir olay dış veritabanında oturum açabilirsiniz.
- Bir değer getirir ve ardından kullanıcı veritabanında depolamak için kullanılabilir.
- Yürütmenin akışını değiştirebilirsiniz.

Bu makalede gösterilen senaryo, aşağıdaki eylemleri içerir:

1. Kullanıcı, bir dış sistemde arayın.
2. Burada kullanıcının kayıtlı Şehir alın.
3. Bu öznitelik, talep olarak uygulamaya döndürür.

## <a name="prerequisites"></a>Önkoşullar

- Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).
- İle etkileşim kurmak için bir REST API uç noktası. Bu makalede, basit bir Azure işlevini örnek olarak kullanır. Azure işlevi oluşturmak için bkz [Azure portalında ilk işlevinizi oluşturma](../azure-functions/functions-create-first-azure-function.md).

## <a name="prepare-the-api"></a>API hazırlama

Bu bölümde, Azure işlevi için bir değer almaya hazırlama `email`ve ardından için bir değer döndürmesi `city` kullanılabilen Azure AD B2C tarafından talep olarak.

Run.csx dosyasının aşağıdaki kodu kullanmak için oluşturduğunuz Azure işlevi için değiştirin:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
  log.LogInformation("C# HTTP trigger function processed a request.");
  string email = req.Query["email"];
  string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
  dynamic data = JsonConvert.DeserializeObject(requestBody);
  email = email ?? data?.email;

  return email != null
    ? (ActionResult)new OkObjectResult(
      new ResponseContent
      {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        city = "Redmond"
      })
      : new BadRequestObjectResult("Please pass an email on the query string or in the request body");
}

public class ResponseContent
{
    public string version { get; set; }
    public int status { get; set; }
    public string city {get; set; }
}
```

## <a name="configure-the-claims-exchange"></a>Talep değişimi yapılandırın

Teknik profili, talep exchange yapılandırmasını sağlar.

Açık *TrustFrameworkExtensions.xml* dosyasını açıp aşağıdaki **ClaimsProvider** XML öğesi içindeki **ClaimsProviders** öğesi.

```XML
<ClaimsProvider>
  <DisplayName>REST APIs</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AzureFunctions-WebHook">
      <DisplayName>Azure Function Web Hook</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ServiceUrl">https://myfunction.azurewebsites.net/api/HttpTrigger1?code=bAZ4lLy//ZHZxmncM8rI7AgjQsrMKmVXBpP0vd9smOzdXDDUIaLljA==</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        <Item Key="AllowInsecureAuthInProduction">true</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
      </OutputClaims>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

**InputClaims** öğe REST hizmete gönderilen talepleri tanımlar. Bu örnekte, talep değerini `givenName` REST hizmeti talep olarak gönderilecek `email`. **OutputClaims** öğe REST hizmetinden beklenen talepleri tanımlar.

## <a name="add-the-claim-definition"></a>Talep tanımını ekleyin

İçin bir tanım ekleyin `city` içinde **BuildingBlocks** öğesi. Bu öğe TrustFrameworkExtensions.xml dosyasının başında bulabilirsiniz.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="city">
      <DisplayName>City</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your city</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

## <a name="add-an-orchestration-step"></a>Düzenleme adımı ekleyin

Burada REST API çağrısı bir düzenleme adımı kullanılan birçok kullanım örnekleri vardır. Bir düzenleme adımı, bir kullanıcı ilk kez kaydı gibi bir görevi başarıyla tamamlandıktan sonra bir dış sistem için bir güncelleştirme veya profil güncelleştirmesi olarak bilgilerin eşitlenmiş tutmak için kullanılabilir. Bu durumda, bu uygulamaya profil düzenledikten sonra sağlanan bilgileri genişletmek için kullanılır.

Profil düzenleme kullanıcı yolculuğu için bir adım ekleyin. Kullanıcı olduktan sonra (1-4 Aşağıdaki XML'de düzenleme adımlarının) kimlik doğrulaması ve kullanıcının güncelleştirilmiş profil bilgilerini (5. adım) sağlamıştır. Kopya profili Düzenle kullanıcı yolculuğu XML kodundan *TrustFrameworkBase.xml* dosyasını, *TrustFrameworkExtensions.xml* içinde dosya **UserJourneys** öğesi. Daha sonra 6. adım olarak değişikliği yapmak.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
  </ClaimsExchanges>
</OrchestrationStep>
```

Kullanıcı yolculuğu için son XML şu örnekteki gibi görünmelidir:

```XML
<UserJourney Id="ProfileEdit">
  <OrchestrationSteps>
    <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
      <ClaimsProviderSelections>
        <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
        <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
      </ClaimsProviderSelections>
    </OrchestrationStep>
    <OrchestrationStep Order="2" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
        <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="3" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>localAccountAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="4" Type="ClaimsExchange">
      <Preconditions>
        <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
          <Value>authenticationSource</Value>
          <Value>socialIdpAuthentication</Value>
          <Action>SkipThisOrchestrationStep</Action>
        </Precondition>
      </Preconditions>
      <ClaimsExchanges>
        <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="5" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <!-- Add a step 6 to the user journey before the JWT token is created-->
    <OrchestrationStep Order="6" Type="ClaimsExchange">
      <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-WebHook" />
      </ClaimsExchanges>
    </OrchestrationStep>
    <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
  </OrchestrationSteps>
  <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="add-the-claim"></a>Talep Ekle

Düzen *ProfileEdit.xml* dosya ve ekleme `<OutputClaim ClaimTypeReferenceId="city" />` için **OutputClaims** öğesi.

Yeni Talep ekledikten sonra teknik profili, bu örnekteki gibi görünür:

```XML
<TechnicalProfile Id="PolicyProfile">
  <DisplayName>PolicyProfile</DisplayName>
  <Protocol Name="OpenIdConnect" />
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
    <OutputClaim ClaimTypeReferenceId="tenantId" AlwaysUseDefaultValue="true" DefaultValue="{Policy:TenantObjectId}" />
    <OutputClaim ClaimTypeReferenceId="city" />
  </OutputClaims>
  <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="upload-your-changes-and-test"></a>Değişikliklerinizi karşıya yüklemek ve test

1. (İsteğe bağlı:) Devam etmeden önce mevcut sürümü (indirerek) dosyalarını kaydedin.
2. Karşıya yükleme *TrustFrameworkExtensions.xml* ve *ProfileEdit.xml* ve var olan dosyanın üzerine yazmak için seçin.
3. Seçin **B2C_1A_ProfileEdit**.
4. İçin **uygulama seçin** özel ilke genel bakış sayfasında adlı web uygulamasını seçin *webapp1* daha önce kaydettiğiniz. Emin olun **yanıt URL'si** olduğu `https://jwt.ms`.
4. Seçin **Şimdi Çalıştır**. Hesap kimlik bilgilerinizle oturum açın ve tıklayın **devam**.

Her şeyin doğru şekilde ayarlanıp ayarlanmadığını, yeni bir talep belirteci içeren `city`, değerle `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_profileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıca, bir doğrulama profili olarak etkileşim tasarlayabilirsiniz. Daha fazla bilgi için [izlenecek yol: Kullanıcı girişini doğrulama olarak, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme](active-directory-b2c-rest-api-validation-custom.md).
- [Kullanıcılarınızdan daha fazla bilgi toplamak için profil düzenleme değiştirme](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
