---
title: Azure Active Directory B2C'de Oturumumu açık bırak | Microsoft Docs
description: "'Oturumumu açık bırak' kurmak nasıl yazılacağını gösteren bir konu."
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 6bad6e1f2b204f76b075652a9d3f27367a8de49f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441326"
---
# <a name="azure-active-directory-b2c-enable-keep-me-signed-in-kmsi"></a>Azure Active Directory B2C: '(KMSI) oturum açmış tut' etkinleştir  
[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C, artık web ve '(KMSI) oturum açmış tut' işlevselliğini etkinleştirmek için yerel uygulamalar sağlar. Bu özellik, kullanıcıların uygulamaya kullanıcı adı ve parola girmek sormadan döndüren erişimi verir. Kullanıcı oturumu kapattıktan sonra bu erişimi iptal edilir. 

Genel bilgisayarlarda bu seçenek işaretlendiğinde kullanıcıların karşı öneririz. 

![görüntü](images/kmsi.PNG)


## <a name="prerequisites"></a>Önkoşullar

Yerel izin verecek şekilde yapılandırılmış bir Azure AD B2C kiracısı hesap oturumu açma kaydolma/oturum açma, açıklandığı [Başlarken](active-directory-b2c-get-started-custom.md).

## <a name="how-to-enable-kmsi"></a>KMSI'yi etkinleştirme

Güven framework uzantıları ilkenizde aşağıdaki değişiklikleri yapın.

## <a name="adding-a-content-definition-element"></a>İçerik tanımı öğesi ekleniyor 

`BuildingBlocks` Uzantı dosyanızın düğüm içermelidir bir `ContentDefinitions` öğesi. 

1. İçinde `ContentDefinitions` bölümünde, yeni bir tanımlama `ContentDefinition` kimlikli `api.signuporsigninwithkmsi`.
2. Yeni `ContentDefinition` içermelidir bir `LoadUri`, `RecoveryUri` ve `DataUri` gibi.
3. Datauri`urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` KMSI onay kutusuna oturum açma sayfalarını getirir bir makine anlaşılır tanımlayıcısı. Bu değer değişmez emin olun. 

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



## <a name="add-a--local-account-sign-in-claims-provider"></a>Bir yerel hesap oturum açma talep sağlayıcısı Ekle 

İçin bir talep sağlayıcısı olarak yerel hesapla oturum aç'ı tanımlayabilirsiniz `<ClaimsProvider>` uzantısının ilkenizin düğümünde:

1. Uzantı dosyası (TrustFrameworkExtensions.xml) çalışma dizininizi açın. 
2. Bulma `<ClaimsProviders>` bölümü. Yoksa, kök düğümü ekleyin.
3. Başlangıç Paketi'nden [Başlarken](active-directory-b2c-get-started-custom.md) 'Yerel hesapla oturum aç' talep sağlayıcısı ile birlikte gelir. 
4. Aksi takdirde, yeni bir ekleme `<ClaimsProvider>` gösterildiği gibi düğüm:

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

### <a name="add-the-application-ids-to-your-custom-policy"></a>Özel ilkenizi uygulama kimlikleri ekleme

Uygulama kimlikleri uzantıları dosyaya ekleyin (`TrustFrameworkExtensions.xml`):

1. Öğesi (TrustFrameworkExtensions.xml) uzantıları dosyasında bulma `<TechnicalProfile Id="login-NonInteractive">` ve `<TechnicalProfile Id="login-NonInteractive-PasswordChange">`

2. Tüm örneklerinin yerine `IdentityExperienceFrameworkAppId` açıklandığı gibi kimlik deneyimi çerçevesi uygulamasının uygulama kimliği ile [Başlarken](active-directory-b2c-get-started-custom.md). Örnek aşağıda verilmiştir:

   ```
   <Item Key="client_id">8322dedc-cbf4-43bc-8bb6-141d16f0f489</Item>
   ```

3. Tüm örneklerinin yerine `ProxyIdentityExperienceFrameworkAppId` açıklandığı Proxy kimlik deneyimi çerçevesi uygulamasının uygulama kimliği ile [Başlarken](active-directory-b2c-get-started-custom.md).

4. Uzantıları dosyanızı kaydedin.

## <a name="create-a-kmsi-in-enabled-user-journey"></a>Etkin kullanıcı yolculuğunda bir KMSI oluşturma

Şimdi, kullanıcı yolculuğu için yerel hesapla oturum aç talep sağlayıcısı eklemek gerekir. 

1. Temel dosya ilkenizin (örneğin, TrustFrameworkBase.xml) açın.
2. Bulma `<UserJourneys>` öğenin ve tüm kopyalama `<UserJourney>` içeren düğüm `Id="SignUpOrSignIn"`.
3. Uzantı dosyası (örneğin, TrustFrameworkExtensions.xml) açın ve bulun `<UserJourneys>` öğesi. Öğe yoksa bir tane ekleyin.
4. Tüm yapıştırın `<UserJourney>` alt öğesi olarak kopyaladığınız düğüm `<UserJourneys>` öğesi.
5. Yeni kullanıcı yolculuğu kimliği yeniden adlandırın (örneğin, Yeniden Adlandır `SignUpOrSignInWithKmsi`).
6. Son olarak `OrchestrationStep 1` değiştirme `ContentDefinitionReferenceId` için `api.signuporsigninwithkmsi` , önceki adımda el. Bu onay kutusu kullanıcı yolculuğu sağlar. 
7. İşiniz uzantısının değiştirme. Kaydet ve bu dosyayı karşıya yükleyin. Tüm Doğrulamalar başarıyla emin olun.

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

Ardından, oluşturduğunuz kullanıcı yolculuğu başlatan bağlı olan taraf (RP) dosyasını güncelleştirin:

1. Çalışma dizininizde SignUpOrSignIn.xml kopyalanmasına. Ardından, dosyayı (örneğin, SignUpOrSignInWithKmsi.xml) yeniden adlandırın.

2. Yeni bir dosya açıp güncelleştirme `PolicyId` özniteliğini `<TrustFrameworkPolicy>` benzersiz bir değere sahip. İlkenizi (örneğin, SignUpOrSignInWithKmsi) adıdır.

3. Değiştirme `ReferenceId` özniteliğini `<DefaultUserJourney>` eşleştirilecek `Id` (örneğin, SignUpOrSignInWithKmsi) oluşturduğunuz yeni kullanıcı yolculuğunun.

4. KMSI'yi yapılandırılmıştır `UserJourneyBehaviors`. 

5. **`KeepAliveInDays`** kullanıcının oturum açmış durumda kalır ne kadar süreyle denetler. Aşağıdaki örnekte, KMSI oturumu otomatik olarak ne sıklıkta sessiz kimlik doğrulaması kullanıcının gerçekleştirdiği bakılmaksızın 14 gün sonra süresi dolar.

   Ayar `KeepAliveInDays` 0 değerine KMSI işlevselliği devre dışı bırakır. Varsayılan olarak, bu değer 0'dır

6. Varsa **`SessionExpiryType`** olduğu *çalışırken*, ardından KMSI oturumu kullanıcı sessiz kimlik doğrulaması gerçekleştirdiğinde 14 gün kadar uzatılır.  Varsa *çalışırken* olan seçili gün sayısı için en düşük tutmanızı öneririz. 

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

7. Yaptığınız değişiklikleri kaydedin ve ardından dosyayı karşıya yükleyin.

8. Azure portalında, karşıya yüklediğiniz özel bir ilkeyi test etmek için ilke dikey penceresine gidin ve ardından **Şimdi Çalıştır**.


## <a name="link-to-sample-policy"></a>Örnek ilkesine yönelik bağlantı

Örnek ilke bulabilirsiniz [burada](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/keep%20me%20signed%20in).








