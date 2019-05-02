---
title: Azure Active Directory B2C'de kaynak sahibi parola kimlik bilgileri akışı yapılandırma | Microsoft Docs
description: Azure Active Directory B2C'de kaynak sahibi parola kimlik bilgileri akışı yapılandırmayı öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: d86caf5e5c6df29e00f17462f6a06602ff1245d8
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688857"
---
# <a name="configure-the-resource-owner-password-credentials-flow-in-azure-active-directory-b2c-using-a-custom-policy"></a>Özel bir ilke kullanarak kaynak sahibi parola kimlik bilgilerini flow'da Azure Active Directory B2C'yi yapılandırma

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory (Azure AD) B2C'de bir OAuth standart kimlik doğrulama akışı kaynak sahibi parola kimlik bilgilerini (ROPC) akışı olur. Bu akışta bir uygulama olarak da bilinen bağlı olan taraf belirteçleri için geçerli kimlik bilgilerini değiştirir. Kimlik bilgileri, bir kullanıcı kimliği ve parolası içerir. Döndürülen kimlik belirteci, erişim belirteci ve yenileme belirteci belirteçleridir.

Aşağıdaki seçenekler ROPC akışında desteklenir:

- **Yerel istemci** -kimlik doğrulaması sırasında kullanıcı etkileşimi kod, kullanıcı tarafı cihazda çalıştığında gerçekleşir.
- **Genel istemci akışı** -yalnızca bir uygulama tarafından toplanan kimlik bilgileri kullanıcı API çağrısı gönderilir. Uygulamanın kimlik bilgilerini gönderilmez.
- **Yeni Talep ekleyin** -kimlik belirteci içeriği, yeni bir talep eklemek için değiştirilebilir.

Aşağıdaki akışlara ait desteklenmez:

- **Sunucudan sunucuya** -kimlik koruma sistemi arayandan (yerel istemci) etkileşim bir parçası olarak toplanan güvenilir bir IP adresi gerekiyor. Bir sunucu tarafı API çağrısı, sunucunun IP adresi kullanılır. Çok fazla oturum açma başarısız olursa, kimlik koruma sistemi yinelenen bir IP adresinde bir saldırgan olarak görünebilir.
- **Tek sayfalı uygulama** -öncelikli olarak yazılmış JavaScript olan bir ön uç uygulaması. Genellikle, uygulama AngularJS, Ember.js veya Durandal gibi bir çerçeve kullanılarak yazılır.
- **Gizli istemci akışı** - uygulama istemci kimliği doğrulanır, ancak uygulama gizli anahtarı değil.

## <a name="prerequisites"></a>Önkoşullar

Bölümündeki adımları tamamlamanız [özel ilkeleri Azure Active Directory B2C kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="register-an-application"></a>Bir uygulamayı kaydetme

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girmeniz *ROPC_Auth_app*.
6. Seçin **Hayır** için **Web uygulaması/Web API'sini**ve ardından **Evet** için **yerel istemci**.
7. Bunlar ve ardından gibi diğer tüm değerler bırakın **Oluştur**.
8. Yeni uygulamayı seçin ve daha sonra kullanmak için uygulama kimliği kaydedin.

##  <a name="create-a-resource-owner-policy"></a>Kaynak sahibi ilkesi oluşturma

1. Açık *TrustFrameworkExtensions.xml* dosya.
2. Zaten yoksa ekleyin bir **ClaimsSchema** öğesi ve onun alt öğeleri altına ilk öğe olarak **BuildingBlocks** öğesi:

    ```XML
    <ClaimsSchema>
      <ClaimType Id="logonIdentifier">
        <DisplayName>User name or email address that the user can use to sign in</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="resource">
        <DisplayName>The resource parameter passes to the ROPC endpoint</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokenIssuedOnDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
      <ClaimType Id="refreshTokensValidFromDateTime">
        <DisplayName>An internal parameter used to determine whether the user should be permitted to authenticate again using their existing refresh token.</DisplayName>
        <DataType>string</DataType>
      </ClaimType>
    </ClaimsSchema>
    ```

3. Sonra **ClaimsSchema**, ekleme bir **ClaimsTransformations** öğesi ve onun alt öğeleri için **BuildingBlocks** öğesi:

    ```XML
    <ClaimsTransformations>
      <ClaimsTransformation Id="CreateSubjectClaimFromObjectID" TransformationMethod="CreateStringClaim">
        <InputParameters>
          <InputParameter Id="value" DataType="string" Value="Not supported currently. Use oid claim." />
        </InputParameters>
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="sub" TransformationClaimType="createdClaim" />
        </OutputClaims>
      </ClaimsTransformation>
    
      <ClaimsTransformation Id="AssertRefreshTokenIssuedLaterThanValidFromDate" TransformationMethod="AssertDateTimeIsGreaterThan">
        <InputClaims>
          <InputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" TransformationClaimType="leftOperand" />
          <InputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" TransformationClaimType="rightOperand" />
        </InputClaims>
        <InputParameters>
          <InputParameter Id="AssertIfEqualTo" DataType="boolean" Value="false" />
          <InputParameter Id="AssertIfRightOperandIsNotPresent" DataType="boolean" Value="true" />
        </InputParameters>
      </ClaimsTransformation>
    </ClaimsTransformations>
    ```

4. Bulun **ClaimsProvider** sahip öğe bir **DisplayName** , `Local Account SignIn` ve teknik profilinin ekleyin:

    ```XML
    <TechnicalProfile Id="ResourceOwnerPasswordCredentials-OAUTH2">
      <DisplayName>Local Account SignIn</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <Metadata>
        <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">We can't seem to find your account</Item>
        <Item Key="UserMessageIfInvalidPassword">Your password is incorrect</Item>
        <Item Key="UserMessageIfOldPasswordUsed">Looks like you used an old password</Item>
        <Item Key="DiscoverMetadataByTokenIssuer">true</Item>
        <Item Key="ValidTokenIssuerPrefixes">https://sts.windows.net/</Item>
        <Item Key="METADATA">https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration</Item>
        <Item Key="authorization_endpoint">https://login.microsoftonline.com/{tenant}/oauth2/token</Item>
        <Item Key="response_types">id_token</Item>
        <Item Key="response_mode">query</Item>
        <Item Key="scope">email openid</Item>
      </Metadata>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="logonIdentifier" PartnerClaimType="username" Required="true" DefaultValue="{OIDC:Username}"/>
        <InputClaim ClaimTypeReferenceId="password" Required="true" DefaultValue="{OIDC:Password}" />
        <InputClaim ClaimTypeReferenceId="grant_type" DefaultValue="password" />
        <InputClaim ClaimTypeReferenceId="scope" DefaultValue="openid" />
        <InputClaim ClaimTypeReferenceId="nca" PartnerClaimType="nca" DefaultValue="1" />
        <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="00000000-0000-0000-0000-000000000000" />
        <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="00000000-0000-0000-0000-000000000000" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="oid" />
        <OutputClaim ClaimTypeReferenceId="userPrincipalName" PartnerClaimType="upn" />
      </OutputClaims>
      <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
      </OutputClaimsTransformations>
      <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>
    ```

    Değiştirin **DefaultValue** , **client_id** ve **resource_id** oluşturduğunuz ProxyIdentityExperienceFramework uygulamanın uygulama kimliği Önkoşul öğretici.

5. Ekleme aşağıdaki **ClaimsProvider** teknik profillerini öğelerle **ClaimsProviders** öğesi:

    ```XML
    <ClaimsProvider>
      <DisplayName>Azure Active Directory</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="AAD-UserReadUsingObjectId-CheckRefreshTokenDate">
          <Metadata>
            <Item Key="Operation">Read</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokensValidFromDateTime" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="AssertRefreshTokenIssuedLaterThanValidFromDate" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromObjectID" />
          </OutputClaimsTransformations>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Session Management</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="SM-RefreshTokenReadAndSetup">
          <DisplayName>Trustframework Policy Engine Refresh Token Setup Technical Profile</DisplayName>
          <Protocol Name="None" />
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="objectId" />
            <OutputClaim ClaimTypeReferenceId="refreshTokenIssuedOnDateTime" />
          </OutputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>

    <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="JwtIssuer">
          <Metadata>
            <!-- Point to the redeem refresh token user journey-->
            <Item Key="RefreshTokenUserJourneyId">ResourceOwnerPasswordCredentials-RedeemRefreshToken</Item>
          </Metadata>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

6. Ekleme bir **UserJourneys** öğesi ve onun alt öğeleri için **TrustFrameworkPolicy** öğesi:

    ```XML
    <UserJourney Id="ResourceOwnerPasswordCredentials">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="ResourceOwnerFlow" TechnicalProfileReferenceId="ResourceOwnerPasswordCredentials-OAUTH2" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    <UserJourney Id="ResourceOwnerPasswordCredentials-RedeemRefreshToken">
      <PreserveOriginalAssertion>false</PreserveOriginalAssertion>
      <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="RefreshTokenSetupExchange" TechnicalProfileReferenceId="SM-RefreshTokenReadAndSetup" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
          <ClaimsExchanges>
            <ClaimsExchange Id="CheckRefreshTokenDateFromAadExchange" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId-CheckRefreshTokenDate" />
          </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
      </OrchestrationSteps>
    </UserJourney>
    ```

7. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
8. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
9. **Karşıya Yükle**'ye tıklayın.

## <a name="create-a-relying-party-file"></a>Bir bağlı olan taraf dosyası oluşturun

Ardından, oluşturduğunuz kullanıcı yolculuğu başlatan bağlı olan taraf dosyasını güncelleştirin:

1. Bir kopyasını *SignUpOrSignin.xml* yeniden adlandırın ve çalışma dizininizde dosya *ROPC_Auth.xml*.
2. Yeni dosyayı açıp değiştirin **Policyıd** özniteliğini **TrustFrameworkPolicy** için benzersiz bir değer. İlkenizin adını ilke kimliğidir. Örneğin, **B2C_1A_ROPC_Auth**.
3. Değiştirin **Referenceıd** özniteliğini **DefaultUserJourney** için `ResourceOwnerPasswordCredentials`.
4. Değişiklik **OutputClaims** yalnızca aşağıdaki talep içerecek şekilde öğesi:
    
    ```XML
    <OutputClaim ClaimTypeReferenceId="sub" />
    <OutputClaim ClaimTypeReferenceId="objectId" />
    <OutputClaim ClaimTypeReferenceId="displayName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="givenName" DefaultValue="" />
    <OutputClaim ClaimTypeReferenceId="surname" DefaultValue="" />
    ```

5. Üzerinde **özel ilkeleri** sayfa seçin, Azure AD B2C kiracınızın **karşıya yükleme İlkesi**.
6. Etkinleştirme **ilke varsa üzerine**, ardından gözatın ve seçin *TrustFrameworkExtensions.xml* dosya.
7. **Karşıya Yükle**'ye tıklayın.

## <a name="test-the-policy"></a>Test İlkesi

API çağrısında oluşturmak için sık kullanılan API geliştirme uygulamanızı kullanın ve yanıt ilkenizi hata ayıklamak için gözden geçirin. POST isteğinin gövdesi olarak aşağıdaki bilgilerle şu örnekteki gibi bir çağrı oluşturun:

`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token?p=B2C_1_ROPC_Auth`

- Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı.
- Değiştirin `B2C_1A_ROPC_Auth` kaynak sahibi parola kimlik bilgilerini ilkenizin tam ada sahip.

| Anahtar | Değer |
| --- | ----- |
| kullanıcı adı | `user-account` |
| password | `password1` |
| grant_type değeri | password |
| scope | openıd `application-id` offline_access |
| client_id | `application-id` |
| response_type | belirteç id_token |

- Değiştirin `user-account` kiracınızdaki bir kullanıcı hesabı adı ile.
- Değiştirin `password1` kullanıcı hesabının parolası ile.
- Değiştirin `application-id` uygulama kimliği ile *ROPC_Auth_app* kayıt.
- *Offline_access* bir yenileme belirteci almak istiyorsanız isteğe bağlıdır.

Fiili POST isteği aşağıdaki örnekteki gibi görünür:

```HTTPS
POST /yourtenant.onmicrosoft.com/oauth2/v2.0/token?B2C_1_ROPC_Auth HTTP/1.1
Host: yourtenant.b2clogin.com
Content-Type: application/x-www-form-urlencoded

username=contosouser.outlook.com.ws&password=Passxword1&grant_type=password&scope=openid+bef22d56-552f-4a5b-b90a-1988a7d634ce+offline_access&client_id=bef22d56-552f-4a5b-b90a-1988a7d634ce&response_type=token+id_token
```

Çevrimdışı erişim ile başarılı bir yanıt aşağıdaki örnekteki gibi görünür:

```JSON
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki...",
    "token_type": "Bearer",
    "expires_in": "3600",
    "refresh_token": "eyJraWQiOiJacW9pQlp2TW5pYVc2MUY0TnlfR3REVk1EVFBLbUJLb0FUcWQ1ZWFja1hBIiwidmVyIjoiMS4wIiwiemlwIjoiRGVmbGF0ZSIsInNlciI6Ij...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik9YQjNhdTNScWhUQWN6R0RWZDM5djNpTmlyTWhqN2wxMjIySnh6TmgwRlki..."
}
```

## <a name="redeem-a-refresh-token"></a>Bir yenileme belirteci kullanma

Aşağıda gösterilene benzer bir POST çağrısına oluşturun. Bilgileri aşağıdaki tabloda, istek gövdesi olarak kullanın:

`https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/v2.0/token?p=B2C_1_ROPC_Auth`

- Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı.
- Değiştirin `B2C_1A_ROPC_Auth` kaynak sahibi parola kimlik bilgilerini ilkenizin tam ada sahip.

| Anahtar | Değer |
| --- | ----- |
| grant_type değeri | refresh_token |
| response_type | id_token |
| client_id | `application-id` |
| kaynak | `application-id` |
| refresh_token | `refresh-token` |

- Değiştirin `application-id` uygulama kimliği ile *ROPC_Auth_app* kayıt.
- Değiştirin `refresh-token` ile **refresh_token** önceki yanıtta gönderildi.

Başarılı bir yanıt aşağıdaki örnekteki gibi görünür:

```JSON
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQndhT...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ilg1ZVhrNHh5b2pORnVtMWtsMll0djhkbE5QNC1jNTdkTzZRR1RWQn...",
    "token_type": "Bearer",
    "not_before": 1533672990,
    "expires_in": 3600,
    "expires_on": 1533676590,
    "resource": "bef2222d56-552f-4a5b-b90a-1988a7d634c3",
    "id_token_expires_in": 3600,
    "profile_info": "eyJ2ZXIiOiIxLjAiLCJ0aWQiOiI1MTZmYzA2NS1mZjM2LTRiOTMtYWE1YS1kNmVlZGE3Y2JhYzgiLCJzdWIiOm51bGwsIm5hbWUiOiJEYXZpZE11IiwicHJlZmVycmVkX3VzZXJuYW1lIjpudWxsLCJpZHAiOiJMb2NhbEFjY291bnQifQ",
    "refresh_token": "eyJraWQiOiJjcGltY29yZV8wOTI1MjAxNSIsInZlciI6IjEuMCIsInppcCI6IkRlZmxhdGUiLCJzZXIiOiIxLjAi...",
    "refresh_token_expires_in": 1209600
}
```

## <a name="use-a-native-sdk-or-app-auth"></a>Bir yerel SDK veya uygulama kimlik doğrulaması kullanma

Azure AD B2C genel istemci kaynak sahibi parola kimlik bilgileri için OAuth 2.0 standartları karşılar ve çoğu istemci SDK'ları ile uyumlu olması gerekir. En son bilgiler için bkz. [yerel uygulama SDK'sı için OAuth 2.0 ve modern en iyi yöntemleri uygulayarak Openıd Connect](https://appauth.io/).

## <a name="next-steps"></a>Sonraki adımlar

- Bu senaryoda tam bir örneğini görmek [Azure Active Directory B2C özel ilke başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/source/aadb2c-ief-ropc).
- Azure Active Directory B2C'de kullanılan belirteçleri hakkında daha fazla bilgi [belirteç başvurusu](active-directory-b2c-reference-tokens.md).
