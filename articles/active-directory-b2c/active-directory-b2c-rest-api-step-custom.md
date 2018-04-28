---
title: 'Azure Active Directory B2C: REST API alışverişleri orchestration adım olarak talep | Microsoft Docs'
description: Konuyla ilgili bir API ile tümleştirmek Azure Active Directory B2C özel ilkeler
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/24/2017
ms.author: davidmu
ms.openlocfilehash: 0a3f69f8262e67de624d4cd97e0cad49c6a96ef0
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>İzlenecek yol: Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine orchestration adım olarak tümleştirin.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C altını çizen kimlik deneyimi Framework (IEF) (Azure AD B2C) etkileşim kullanıcı gezisine bir RESTful API'si ile tümleştirmek kimlik Geliştirici sağlar.  

Bu kılavuzun sonunda RESTful hizmetlerle etkileşimde bulunan bir Azure AD B2C kullanıcı gezisine oluşturmak mümkün olacaktır.

IEF Taleplerde verileri gönderir ve geri Taleplerde verilerini alır. REST API exchange talepleri:

- Orchestration adım olarak tasarlanmış olabilir.
- Bir dış eylem tetikleyebilir. Örneğin, bir olay dış veritabanında kaydedebilirsiniz.
- Bir değer getirebilir ve kullanıcı veritabanında depolamak için kullanılabilir.

Daha sonra akışını değiştirmek için alınan talep kullanabilirsiniz.

Bir doğrulama profili olarak etkileşim de tasarlayabilirsiniz. Daha fazla bilgi için bkz: [izlenecek yol: REST API tümleştirme kullanıcı girişini doğrulama olarak Azure AD B2C kullanıcı Yolculuğunuzun alışverişlerine talep](active-directory-b2c-rest-api-validation-custom.md).

Bir kullanıcı profili Düzenle gerçekleştirdiğinde, istediğimizi senaryodur:

1. Kullanıcı bir dış sistemde arayın.
2. Bu kullanıcının, kayıtlı Şehir alın.
3. Bu öznitelik uygulamayı bir talep olarak geri dönün.

## <a name="prerequisites"></a>Önkoşullar

- Bölümünde açıklandığı gibi bir yerel hesap oturumu-up/oturum açmayı tamamlamak için yapılandırılmış bir Azure AD B2C kiracısı [Başlarken](active-directory-b2c-get-started-custom.md).
- Etkileşim için bir REST API uç noktası. Bu kılavuzda, bir basit Azure işlevi app Web kancası örnek olarak kullanılır.
- *Önerilen*: tamamlamak [REST API talep exchange izlenecek bir doğrulama adımı olarak](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-the-rest-api-function"></a>1. adım: REST API işlevi hazırlama

> [!NOTE]
> REST API işlevleri bu makalenin kapsamı dışındadır kurulması. [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-reference) RESTful hizmetlerini bulutta oluşturmak için mükemmel bir araç sağlar.

Adlı bir talep aldığında bir Azure işlevi ayarlarız `email`ve ardından talep döndürür `city` atanan değeriyle `Redmond`. Azure işlevi örnek açıktır [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

`userMessage` Azure işlevi döndürür talep bu bağlamda isteğe bağlıdır ve IEF bunu göz ardı eder. Potansiyel olarak uygulamaya geçirilen ve daha sonra kullanıcıya bir ileti kullanabilirsiniz.

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

Bir Azure işlevi uygulama belirli bir işlev tanımlayıcısını içeren işlevi URL'sini alma kolay hale getirir. Bu durumda, URL'si şöyledir: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. Bunu test etmek için kullanabilirsiniz.

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>2. adım: RESTful API'si talep exchange TrustFrameworExtensions.xml dosyanızdaki teknik bir profil olarak yapılandırın.

İstenen RESTful hizmeti ile exchange tam yapılandırmasını bir teknik profilidir. TrustFrameworkExtensions.xml dosyasını açın ve aşağıdaki XML parçacığını içine ekleyin `<ClaimsProvider>` öğesi.

> [!NOTE]
> Aşağıdaki XML, RESTful sağlayıcısı `Version=1.0.0.0` protokol olarak açıklanmıştır. Dış hizmetiyle etkileşime gireceğini işlevi olarak düşünün. <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

`<InputClaims>` Öğesi IEF REST hizmeti gönderilecek Talepleri tanımlar. Bu örnekte, talep içeriğini `givenName` REST hizmeti talep olarak gönderileceği `email`.  

`<OutputClaims>` Öğesi IEF REST hizmetinden beklediği talepleri tanımlar. Alınan talep sayısından bağımsız olarak, IEF yalnızca tanımlanan burada kullanır. Bu örnekte, olarak bir talep alınan `city` bir IEF eşlenecek adlı talep `city`.

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>3. adım: yeni talep ekleme `city` TrustFrameworkExtensions.xml dosyanızın şemaya

Talep `city` henüz herhangi bir yere bizim şemada tanımlı değil. Bu nedenle, öğe içindeki bir tanım ekleyin `<BuildingBlocks>`. Bu öğe TrustFrameworkExtensions.xml dosyasının başında bulabilirsiniz.

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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>4. adım: Profil düzenleme kullanıcı Yolculuğunuzun TrustFrameworkExtensions.xml içinde bir orchestration adım gibi REST Hizmeti talepleri exchange içerir

Kullanıcı sonra Profil düzenleme kullanıcı gezisine adıma (1-4 Aşağıdaki XML düzenleme adımlarının) kimlik doğrulaması ve kullanıcının güncelleştirilmiş profil bilgilerini (5. adım) sağlamıştır ekleyin.

> [!NOTE]
> REST API çağrısı orchestration adım olarak kullanıldığı birçok kullanım örnekleri vardır. Orchestration adım olarak, bu profili güncelleştirme olarak veya bir kullanıcı ilk kez kayıt gibi bir görevi başarıyla tamamlandıktan sonra bir dış sistem için bir güncelleştirme olarak bilgilerin eşitlenmiş tutmak için kullanılabilir. Bu durumda, bu profili düzenledikten sonra uygulamaya sağlanan bilgileri artırmak için kullanılır.

Kopya profil düzenleme Itanium tabanlı sistemler için kullanıcı gezisine XML kodu TrustFrameworkBase.xml dosyasından TrustFrameworkExtensions.xml dosyanıza içinde `<UserJourneys>` öğesi. Ardından 6. adım altında değişiklik yapın.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Sipariş sürümünüzü eşleşmiyorsa, önce adım olarak kod ekleme emin olun `ClaimsExchange` türü `SendClaims`.

Kullanıcı gezisine için son XML aşağıdaki gibi görünmelidir:

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

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>5. adım: talep ekleme `city` bağlı olan taraf için ilke dosya talep uygulamanıza gönderilir şekilde

ProfileEdit.xml bağlı olan taraf (RP) dosyanızı düzenleyin ve değiştirme `<TechnicalProfile Id="PolicyProfile">` öğesi aşağıdakileri ekleyin: `<OutputClaim ClaimTypeReferenceId="city" />`.

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

## <a name="step-6-upload-your-changes-and-test"></a>6. adım: değişikliklerinizi karşıya yüklemek ve test etme

İlkenin mevcut sürümlerin üzerine yazılır.

1.  (İsteğe bağlı:) Devam etmeden önce mevcut sürümü (yükleyerek) uzantıları dosyanızın kaydedin. İlk karmaşıklık düşük tutmak için birden fazla sürümünü uzantıları dosya karşıya yüklemeyin öneririz.
2.  (İsteğe bağlı:) İlke Kimliği İlkesi Düzenle dosyası için yeni sürümü değiştirerek yeniden adlandırın `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Uzantıları dosyasını karşıya yükleyin.
4.  İlkeyi Düzenle RP dosyasını karşıya yükleyin.
5.  Kullanım **Şimdi Çalıştır** ilkesini test etmek. Uygulamaya IEF döndüren belirteci gözden geçirin.

Her şeyin doğru şekilde ayarlanmış olması durumunda, yeni talep belirteci içerecektir `city`, değerle `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

[Doğrulama adımı olarak REST API kullanın](active-directory-b2c-rest-api-validation-custom.md)

[Kullanıcılardan ek bilgi toplamak için profil düzenleme değiştirme](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
