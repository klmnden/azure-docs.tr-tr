---
title: 'Azure Active Directory B2C: KMSI | Microsoft Docs'
description: "'Bana imzalı koru' kurma gösteren bir konu"
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: ajalexander
ms.assetid: 926e9711-71c0-49e8-b658-146ffb7386c0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2016
ms.author: vigunase
ms.openlocfilehash: d09e15c6f6765eac436f573f89c6703039cd8397
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: '(KMSI) içinde imzalı tut' etkinleştir  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C '(KMSI) içinde imzalı tut' işlevselliğini etkinleştirmek için yerel uygulamalar ve web şimdi sağlar. Bu özellik kullanıcı adı ve parolayı girmek için sormadan kullanıcıların uygulamaya döndürme erişimi verir. Kullanıcı oturumu kapattıktan sonra bu erişimi iptal edilir. 

Bu seçenek ortak bilgisayarlardaki denetleme kullanıcılara karşı öneririz. 

![görüntü](images/kmsi.PNG)


## <a name="prerequisites"></a>Ön koşullar

Yerel izin verecek şekilde yapılandırılmış bir Azure AD B2C kiracısı hesap oturumu-up/oturum açma, açıklandığı gibi [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-enable-kmsi"></a>KMSI etkinleştirme

Güven framework uzantıları ilkenizde aşağıdaki değişiklikleri yapın.

## <a name="adding-a-content-definition-element"></a>İçerik tanım öğesi ekleme 

`BuildingBlocks` Uzantı dosyanızı düğümünün içermelidir bir `ContentDefinitions` öğesi. 

1. İçinde `ContentDefinitions` bölümünde, yeni bir tanımlama `ContentDefinition` kimlikli `api.signuporsigninwithkmsi`.
2. Yeni `ContentDefinition` içermelidir bir `LoadUri`, `RecoveryUri` ve `DataUri` gibi.
3. Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` KMSI onay kutusuna oturum açma sayfaları getiren bir makine anlaşılabilir tanımlayıcısıdır. Lütfen bu değeri değiştirmeyin emin olun. 

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



## <a name="add-a--local-account-sign-in-claims-provider"></a>Bir yerel hesap oturum açma talep sağlayıcısı ekleme 

Bir talep sağlayıcısı olarak yerel hesap oturum açma tanımlayabilirsiniz `<ClaimsProvider>` ilkenizin uzantısı dosyasında düğümü:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. 
2. Bul `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
3. Başlatıcı paketinden [Başlarken](active-directory-b2c-get-started-custom.md) 'Yerel hesap oturum açma' Talep sağlayıcı ile gelir. 
4. Aksi takdirde, yeni bir ekleme `<ClaimsProvider>` şekilde düğümü:

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
            <InputClaim ClaimTypeReferenceId="client_id" DefaultValue="ProxyIdentityExperienceFrameworkAppID" />
            <InputClaim ClaimTypeReferenceId="resource_id" PartnerClaimType="resource" DefaultValue="IdentityExperienceFrameworkAppID" />
           </InputClaims>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
 </ClaimsProviders>
```

### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkeniz uygulama kimlikleri Ekle

Uygulama kimlikleri uzantıları dosyasına ekleyin (`TrustFrameworkExtensions.xml`):

1. Uzantıları dosyasında (TrustFrameworkExtensions.xml) öğesi bulunamıyor `<TechnicalProfile Id="login-NonInteractive">` ve`<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Tüm örneklerinin yerine `IdentityExperienceFrameworkAppId` açıklandığı gibi kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip [Başlarken](active-directory-b2c-get-started-custom.md). Örnek aşağıda verilmiştir:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Tüm örneklerinin yerine `ProxyIdentityExperienceFrameworkAppId` açıklandığı gibi Proxy kimlik deneyimi Framework uygulamanın uygulama Kimliğine sahip [Başlarken](active-directory-b2c-get-started-custom.md).

4. Uzantıları dosyanızı kaydedin.

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Etkin kullanıcı gezisine bir KMSI oluşturma

Şimdi yerel hesap oturum açma Talep sağlayıcı kullanıcı Yolculuğunuzun eklemeniz gerekir. 

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2. Bul `<UserJourneys>` öğesi ve kopyalama tüm `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve Bul `<UserJourneys>` öğesi. Öğe yoksa, ekleyin.
4. Tüm Yapıştır `<UserJourney>` bir alt öğesi olarak kopyaladığınız düğümü `<UserJourneys>` öğesi.
5. Yeni kullanıcı gezisine Kimliğini yeniden adlandırın (örneğin, olarak yeniden adlandırın `SignUpOrSignInWithKmsi`).
6. Son olarak, içinde `OrchestrationStep 1` değiştirme `ContentDefinitionReferenceId` için `api.signuporsigninwithkmsi` , önceki adımlarda definied. Bu onay kutusuna kullanıcı gezisine sağlar. 
7. Tamamladığınızda uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.

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

## <a name="create-a-relying-party-rp-file"></a>Bir bağlı olan taraf (RP) dosyası oluşturun

Ardından, oluşturduğunuz kullanıcı gezisine başlatır bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizi SignUpOrSignIn.xml kopyası. Sonra (örneğin, SignUpOrSignInWithKmsi.xml) yeniden adlandırın.

2. Yeni dosya ve güncelleştirme açmak `PolicyId` için öznitelik `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkeniz (örneğin, SignUpOrSignInWithKmsi) adıdır.

3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleşecek şekilde `Id` (örneğin, SignUpOrSignInWithKmsi) oluşturulan yeni kullanıcı gezisine biri.

4. KMSI yapılandırılmıştır `UserJourneyBehaviors`. 

5. **`KeepAliveInDays`**Kullanıcı oturum açmış durumda kalır ne kadar süreyle denetler. Aşağıdaki örnekte, KMSI oturumu otomatik olarak ne sıklıkta kullanıcı sessiz kimlik doğrulaması gerçekleştirdiğinde bakılmaksızın 14 gün sonra süresi dolar.

   Ayarı `KeepAliveInDays` değeri 0 KMSI işlevselliği devre dışı bırakır. Varsayılan olarak, bu değer 0'dır

6. Varsa  **`SessionExpiryType`**  olan *çalışırken*, KMSI oturum kullanıcı sessiz kimlik doğrulama işlemini her gerçekleştirdiğinde 14 gün tarafından Genişletilmiş sonra.  Varsa *çalışırken* olan seçili gün sayısını azda tutmanızı öneririz. 

       <RelyingParty>
       <DefaultUserJourney ReferenceId="SignUpOrSignInWithKmsi" />
       <UserJourneyBehaviors>
         <SingleSignOn Scope="Tenant" KeepAliveInDays="14" />
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

7. Yaptığınız değişiklikleri kaydedin ve dosyayı karşıya yükleyin.

8. Azure portalında, yüklediğiniz, özel ilke sınamak için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**.


## <a name="link-to-sample-policy"></a>Örnek ilkesine bağladığınızda

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








