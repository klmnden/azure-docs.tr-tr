---
title: Azure Active Directory B2C'de Oturumumu açık bırak | Microsoft Docs
description: Yedekleme tutun bana imzalı içinde (KMSI) Azure Active Directory B2C, kurmayı öğrenin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/03/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e99dacbe7ae0f42919616e04e60bf4f21b9bd985
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835377"
---
# <a name="enable-keep-me-signed-in-kmsi-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de (KMSI) içinde Oturumumu açık bırak seçeneğini etkinleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Web ve yerel uygulamaları Azure Active Directory (Azure AD) B2C tutmak bana imzalı içinde (KMSI) işlevselliği etkinleştirebilirsiniz. Bu özellik, kullanıcı adı ve parola girmek sormadan uygulamanın kullanıcıları döndüren erişimi verir. Bir kullanıcı oturumu kapattığında bu erişimi iptal edilir.

Kullanıcılar bu seçeneği genel bilgisayarlarda etkinleştirmemeniz gerekir.

![Oturumumu açık onay kutusu bir canlı gösteren örnek kaydolma oturum açma sayfası](./media/active-directory-b2c-reference-kmsi-custom/kmsi.PNG)

## <a name="prerequisites"></a>Önkoşullar

Yerel hesap kaydolma ve oturum açma izin verecek şekilde yapılandırılmış bir Azure AD B2C kiracısı. Bir kiracı yoksa, adımları kullanarak bir tane oluşturabilirsiniz [Öğreticisi: Bir Azure Active Directory B2C kiracısı oluşturmayı](tutorial-create-tenant.md).

## <a name="add-a-content-definition-element"></a>İçerik tanımı öğesi Ekle

Altında **BuildingBlocks** uzantısı dosyanızın öğe ekleme bir **ContentDefinitions** öğesi.

1. Altında **ContentDefinitions** öğe, Ekle bir **ContentDefinition** tanımlayıcısına sahip öğe `api.signuporsigninwithkmsi`.
2. Altında **ContentDefinition** öğe, Ekle **LoadUri**, **RecoveryUri**, ve **DataUri** öğeleri. `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` Değerini **DataUri** KMSI onay kutusuna oturum açma sayfalarını getirir makine anlaşılır tanımlayıcısı bir öğedir. Bu değer değiştirilmemelidir.

    ```XML
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsigninwithkmsi">
          <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

## <a name="add-a-sign-in-claims-provider-for-a-local-account"></a>Bir yerel hesap oturum açma talep sağlayıcısı Ekle

Yerel hesap oturum açma kullanarak bir talep sağlayıcısı olarak tanımlayabilirsiniz **ClaimsProvider** uzantısının ilkenizin öğesinde:

1. Açık *TrustFrameworkExtensions.xml* çalışma dizininizin dosyasından.
2. Bulma **ClaimsProviders** öğesi. Yoksa, kök öğe altında ekleyin. [Başlangıç paketi](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) bir yerel hesap oturum açma talep sağlayıcısı içerir.
3. Ekleme bir **ClaimsProvider** öğeyle **DisplayName** ve **TechnicalProfile** aşağıdaki örnekte gösterildiği gibi:

    ```XML
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Local Account SignIn</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="login-NonInteractive">
            <Metadata>
              <Item Key="client_id">ProxyIdentityExperienceFrameworkAppId</Item>
              <Item Key="IdTokenAudience">IdentityExperienceFrameworkAppId</Item>
            </Metadata>
            <InputClaims>
              <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
              <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
            </InputClaims>
          </TechnicalProfile>
        </TechnicalProfiles>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

### <a name="add-the-application-identifiers-to-your-custom-policy"></a>Uygulama tanımlayıcıları, özel ilkeniz için ekleyin

Uygulama tanımlayıcıları için ekleme *TrustFrameworkExtensions.xml* dosya.

1. İçinde *TrustFrameworkExtensions.xml* dosya, bulma **TechnicalProfile** tanımlayıcısını öğeyle `login-NonInteractive` ve **TechnicalProfile** öğeyle bir tanımlayıcı `login-NonInteractive-PasswordChange` ve tüm değerleri Değiştir `IdentityExperienceFrameworkAppId` açıklandığı gibi kimlik deneyimi çerçevesi uygulamanın uygulama tanımlayıcısı ile [Başlarken](active-directory-b2c-get-started-custom.md).

    ```
    <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
    ```

2. Tüm değerleri değiştirin `ProxyIdentityExperienceFrameworkAppId` açıklandığı Proxy kimlik deneyimi çerçevesi uygulamanın uygulama tanımlayıcısı ile [Başlarken](active-directory-b2c-get-started-custom.md).
3. Uzantıları dosyayı kaydedin.

## <a name="create-a-kmsi-enabled-user-journey"></a>KMSI'yi etkin kullanıcı yolculuğu oluşturma

Yerel hesap oturum açma talep sağlayıcısı kullanıcı yolculuğunuza ekleyin.

1. İlkenizin temel dosyasını açın. Örneğin, *TrustFrameworkBase.xml*.
2. Bulma **UserJourneys** öğenin ve tüm içeriğini kopyalayın **UserJourney** tanımlayıcısını kullanır öğesi `SignUpOrSignIn`.
3. Uzantı dosyası açın. Örneğin, *TrustFrameworkExtensions.xml* ve bulma **UserJourneys** öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm yapıştırın **UserJourney** öğesi alt öğesi olarak kopyaladığınız **UserJourneys** öğesi.
5. Yeni kullanıcı yolculuğu için olan tanımlayıcıyla değiştirin. Örneğin: `SignUpOrSignInWithKmsi`.
6. Son olarak, ilk düzenleme adımı değiştirin **ContentDefinitionReferenceId** için `api.signuporsigninwithkmsi`. Kullanıcı yolculuğu onay kutusu bu değer ayarı sağlar.
7. Kaydet ve uzantı dosyasını karşıya yükleyin ve tüm doğrulamalarını başarılı olduğunu doğrulayın.

    ```XML
    <UserJourneys>
      <UserJourney Id="SignUpOrSignInWithKmsi">
        <OrchestrationSteps>
          <OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsigninwithkmsi">
            <ClaimsProviderSelections>
              <ClaimsProviderSelection ValidationClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
            <ClaimsExchanges>
              <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="2" Type="ClaimsExchange">
            <Preconditions>
              <Precondition Type="ClaimsExist" ExecuteActionsIf="true">
                <Value>objectId</Value>
                <Action>SkipThisOrchestrationStep</Action>
              </Precondition>
            </Preconditions>
            <ClaimsExchanges>
              <ClaimsExchange Id="SignUpWithLogonEmailExchange" TechnicalProfileReferenceId="LocalAccountSignUpWithLogonEmail" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <!-- This step reads any user attributes that we may not have received when in the token. -->
          <OrchestrationStep Order="3" Type="ClaimsExchange">
            <ClaimsExchanges>
              <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
          </OrchestrationStep>
          <OrchestrationStep Order="4" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
        </OrchestrationSteps>
        <ClientDefinition ReferenceId="DefaultWeb" />
      </UserJourney>
    </UserJourneys>
    ```

## <a name="create-a-relying-party-file"></a>Bir bağlı olan taraf dosyası oluşturun

Oluşturduğunuz kullanıcı yolculuğu başlatır bağlı olan taraf (RP) dosyasını güncelleştirin.

1. Bir kopyasını *SignUpOrSignIn.xml* çalışma dizininizde dosya ve yeniden adlandırın. Örneğin, *SignUpOrSignInWithKmsi.xml*.
2. Yeni bir dosya açıp güncelleştirme **Policyıd** özniteliğini **TrustFrameworkPolicy** benzersiz bir değere sahip. İlkenizi adıdır. Örneğin: `SignUpOrSignInWithKmsi`.
3. Değişiklik **Referenceıd** özniteliğini **DefaultUserJourney** oluşturduğunuz yeni kullanıcı yolculuğu tanımlayıcısını eşleştirilecek öğe. Örneğin: `SignUpOrSignInWithKmsi`.

    KMSI'yi kullanarak yapılandırılmış **UserJourneyBehaviors** öğeyle **SingleSignOn**, **Ssosession**, ve **SessionExpiryInSeconds** ilk alt öğeleri olarak. **KeepAliveInDays** özniteliği denetimleri ne kadar süreyle kullanıcının oturum açmış durumda kalır. Aşağıdaki örnekte, KMSI oturumu otomatik olarak kullanım süresi sonu `7` ne sıklıkta sessiz kimlik doğrulaması kullanıcının gerçekleştirdiği bağımsız olarak gün. Ayarı **KeepAliveInDays** değerini `0` KMSI işlevselliği devre dışı bırakır. Varsayılan olarak, bu değer `0`. Varsa değerini **Ssosession** olduğu `Rolling`, KMSI oturumu tarafından Genişletilmiş `7` kullanıcı sessiz kimlik doğrulaması gerçekleştirdiğinde gün.  Varsa `Rolling` olan seçili gün sayısı için minimum tutmalısınız.

    Değerini **SessionExpiryInSeconds** SSO oturumunun sona erme saati temsil eder. KMSI'yi oturumun veya dolup dolmadığını kontrol etmek için bu Azure AD B2C tarafından dahili olarak kullanılır. Değerini **KeepAliveInDays** web tarayıcısında SSO tanımlama bilgisinin süre sonu/Max-Age değeri belirler. Farklı **SessionExpiryInSeconds**, **KeepAliveInDays** tarayıcı kapatıldığında, tanımlama bilgisi silinmesini önlemek için kullanılır. Yalnızca SSO oturum tanımlama bilgisinin denetlenen yoksa, bir kullanıcının sessiz bir şekilde oturum **KeepAliveInDays**ve süresi dolduğunda, hangi tarafından denetlenir **SessionExpiryInSeconds**.

    Bir kullanıcı değil etkinleştirirseniz **Oturumumu açık bırak** tarafından belirtilen süre geçtikten sonra kaydolma ve oturum açma sayfasında bir oturum süresinin sona **SessionExpiryInSeconds** geçti veya tarayıcı kapatıldı. Bir kullanıcı etkinleştirirse **Oturumumu açık bırak**, değerini **KeepAliveInDays** değerini geçersiz kılar **SessionExpiryInSeconds** ve oturumu sona erme saati belirler. Kullanıcılar tarayıcıyı kapatın ve yeniden açın bile kullanıcılar hala sessiz bir şekilde zaman içinde olduğu sürece oturum **KeepAliveInDays**. Değerini ayarlamak önerilir **SessionExpiryInSeconds** (1200 saniye), kısa bir süre değerini sırasında olmasını **KeepAliveInDays** oldukça uzun bir süre (7 gün) gösterildiği şekilde ayarlanabilir Aşağıdaki örnekte:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
      <UserJourneyBehaviors>
        <SingleSignOn Scope="Tenant" KeepAliveInDays="7" />
        <SessionExpiryType>Absolute</SessionExpiryType>
        <SessionExpiryInSeconds>1200</SessionExpiryInSeconds>
      </UserJourneyBehaviors>
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="displayName" />
          <OutputClaim ClaimTypeReferenceId="givenName" />
          <OutputClaim ClaimTypeReferenceId="surname" />
          <OutputClaim ClaimTypeReferenceId="email" />
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

4. Yaptığınız değişiklikleri kaydedin ve ardından dosyayı karşıya yükleyin.
5. Azure portalında, karşıya yüklediğiniz özel bir ilkeyi test etmek için ilke sayfasına gidin ve ardından **Şimdi Çalıştır**.

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








