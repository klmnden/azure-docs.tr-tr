---
title: REST API, Azure Active Directory B2C'de bir yönetim adımı olarak değişimleri talep | Microsoft Docs
description: Bir API ile tümleştirmek, Azure Active Directory B2C özel ilkeler bir konu.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/24/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 55740b74aef5ce3d2def5ad22cfe3ededa1204d8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60316893"
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Çözüm: Bir düzenleme adımı, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C altını kimlik deneyimi çerçevesi (IEF) (Azure AD B2C) etkileşim kullanıcı yolculuğu bir RESTful API ile tümleştirmek kimlik Geliştirici sağlar.  

Bu kılavuzun sonunda, RESTful hizmetleriyle etkileşime giren bir Azure AD B2C kullanıcı yolculuğu oluşturmak mümkün olacaktır.

IEF Taleplerde veri gönderip geri Taleplerde veri alır. REST API talep Değişimi:

- Bir düzenleme adımı tasarlanmış olabilir.
- Dış bir eylem tetikleyebilirsiniz. Örneğin, bir olay dış veritabanında oturum açabilirsiniz.
- Bir değer getirir ve ardından kullanıcı veritabanında depolamak için kullanılabilir.

Daha sonra yürütmenin akışını değiştirileceği alınan talepler kullanabilirsiniz.

Ayrıca, bir doğrulama profili olarak etkileşim tasarlayabilirsiniz. Daha fazla bilgi için [izlenecek yol: Kullanıcı girişini doğrulama olarak, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme](active-directory-b2c-rest-api-validation-custom.md).

Bir kullanıcı bir profil düzenleme gerçekleştirdiğinde istediğimizi senaryodur:

1. Kullanıcı, bir dış sistemde arayın.
2. Burada kullanıcının kayıtlı Şehir alın.
3. Bu öznitelik, talep olarak uygulamaya döndürür.

## <a name="prerequisites"></a>Önkoşullar

- Bölümünde anlatıldığı gibi yerel bir hesap oturumu açma kaydolma/oturum açma, tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).
- İle etkileşim kurmak için bir REST API uç noktası. Bu kılavuzda, örnek olarak basit bir Azure işlevi uygulaması Web kancası kullanır.
- *Önerilen*: Tamamlamak [REST API, bir doğrulama adımı olarak exchange gözden geçirme talep](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-the-rest-api-function"></a>1. Adım: REST API işlevi hazırlama

> [!NOTE]
> REST API işlevleri kurulumu, bu makalenin kapsamı dışında ' dir. [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-reference) bulutta RESTful hizmetleri oluşturmak için mükemmel bir araç takımı sunar.

Adlı bir talep aldığında bir Azure işlevi ayarladığımız `email`ve ardından talep döndürür `city` atanan değeri ile `Redmond`. Azure işlevi örnek açıktır [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

`userMessage` Azure işlevinin döndürdüğü talep, bu bağlamda isteğe bağlıdır ve IEF dikkate almaz. Potansiyel olarak uygulamaya geçirilmesini ve daha sonra kullanıcıya bir ileti kullanabilirsiniz.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

Bir Azure işlev uygulaması, belirli bir işlev tanımlayıcı içeren işlev URL'sini Al kolaylaştırır. Bu durumda, URL'dir: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. Bunu test etmek için kullanabilirsiniz.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>2. Adım: RESTful API talep değişimi TrustFrameworExtensions.xml dosyanızdaki teknik profili olarak yapılandırma

Teknik profili RESTful hizmeti ile istenen Exchange tam bir yapılandırmadır. TrustFrameworkExtensions.xml dosyasını açın ve içine aşağıdaki XML parçacığını ekleyin `<ClaimsProvider>` öğesi.

> [!NOTE]
> RESTful sağlayıcısı aşağıdaki XML'de `Version=1.0.0.0` protokol olarak açıklanmıştır. Dış hizmetiyle etkileşime gireceğini işlevi olarak düşünün. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
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

`<InputClaims>` Öğe IEF REST hizmeti için gönderilecek Talepleri tanımlar. Bu örnekte, talep içeriğini `givenName` REST hizmeti talep olarak gönderilecek `email`.  

`<OutputClaims>` Öğe IEF REST hizmetinden beklediği talepleri tanımlar. Alınan talep sayısından bağımsız olarak, IEF yalnızca tanımlanan burada kullanır. Bu örnekte, bir talep olarak alınan. `city` bir IEF için eşlenmiş adlı talep `city`.

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>3. Adım: Yeni Talep ekleyin `city` TrustFrameworkExtensions.xml dosyanıza şemasında

Talep `city` henüz herhangi bir yere bizim şemasında tanımlı değil. Bu nedenle, bir tanım öğesinin içine ekleyin `<BuildingBlocks>`. Bu öğe TrustFrameworkExtensions.xml dosyasının başında bulabilirsiniz.

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>4. Adım: REST hizmeti talep değişimi TrustFrameworkExtensions.xml, profil düzenleme kullanıcı yolculuğunda düzenleme adımı olarak ekleyin

Kullanıcı sonra Profil düzenleme kullanıcı yolculuğu bir adıma (1-4 Aşağıdaki XML'de düzenleme adımlarının) kimlik doğrulaması ve kullanıcının güncelleştirilmiş profil bilgilerini (5. adım) ayarının ekleyin.

> [!NOTE]
> Burada REST API çağrısı bir düzenleme adımı kullanılan birçok kullanım örnekleri vardır. Bir düzenleme adımı, bir kullanıcı ilk kez kaydı gibi bir görevi başarıyla tamamlandıktan sonra bir dış sistem için bir güncelleştirme veya profil güncelleştirmesi olarak bilgilerin eşitlenmiş tutmak için kullanılabilir. Bu durumda, bu uygulamaya profil düzenledikten sonra sağlanan bilgileri genişletmek için kullanılır.

Kopya profili Düzenle TrustFrameworkBase.xml dosyasından TrustFrameworkExtensions.xml dosyanızın içinde kullanıcı yolculuğu XML kodu `<UserJourneys>` öğesi. Daha sonra 6. adım altında değişikliği yapmak.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Sipariş sürümünüzü eşleşmiyorsa, önce adım olarak kod yerleştirdiğinizden emin olun `ClaimsExchange` türü `SendClaims`.

Kullanıcı yolculuğu için son XML şu şekilde görünmelidir:

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
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>5. Adım: Talep Ekle `city` bağlı olan taraf için ilke dosyasına talep uygulamanıza gönderilir ve böylece

ProfileEdit.xml bağlı olan taraf (RP) dosyanızı düzenleyin ve değiştirme `<TechnicalProfile Id="PolicyProfile">` öğe aşağıdakileri ekleyin: `<OutputClaim ClaimTypeReferenceId="city" />`.

Yeni Talep ekledikten sonra teknik profili şöyle görünür:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a>6. Adım: Değişikliklerinizi karşıya yüklemek ve test

İlkenin mevcut sürümlerin üzerine yazın.

1.  (İsteğe bağlı:) Devam etmeden önce mevcut sürümü (indirerek) uzantıları dosyanızı kaydedin. İlk karmaşıklığı düşük tutmak için uzantıları dosya birden çok sürümünü yüklemezseniz öneririz.
2.  (İsteğe bağlı:) İlke kimliği için dosya İlkesi Düzenle yeni sürümünü değiştirerek Yeniden Adlandır `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Uzantıları dosyasını karşıya yükleyin.
4.  İlkeyi düzenleme RP dosyasını karşıya yükleyin.
5.  Kullanım **Şimdi Çalıştır** ilkesini test etme. Uygulamaya IEF döndürür belirteci gözden geçirin.

Her şeyin doğru şekilde ayarlanıp ayarlanmadığını, yeni bir talep belirteci içerecektir `city`, değerle `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Sonraki adımlar

[Doğrulama adımını bir REST API'si kullanın](active-directory-b2c-rest-api-validation-custom.md)

[Kullanıcılarınızdan daha fazla bilgi toplamak için profil düzenleme değiştirme](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
