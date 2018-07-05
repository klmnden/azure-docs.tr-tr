---
title: Özel ilkeleri Azure Active Directory B2C için kendi öznitelikleri ekleme | Microsoft Docs
description: Uzantı özellikleri, özel öznitelikler kullanma ve bunları kullanıcı arabiriminin dahil olmak üzere bir kılavuz.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ecde4d8cd8ee454290b16b640ba05d310cf348fe
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450441"
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: İlke oluşturma ve bir özel profilinde özel öznitelikler kullanma Düzenle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure AD B2C dizininizde özel bir öznitelik oluşturur ve özel bir profil düzenleme kullanıcı yolculuğu talebi olarak bu yeni bir öznitelik kullanın.

## <a name="prerequisites"></a>Önkoşullar

Makaledeki adımları tamamlayabilmeniz [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Müşterilerinizin Azure Active Directory B2C'de özel ilkelerini kullanma hakkında bilgi toplamak için özel öznitelikler kullanma
Azure Active Directory (Azure AD) B2C dizininize yerleşik bir öznitelikler kümesi ile birlikte gelir: ad, Soyadı, şehir, posta kodu, userPrincipalName verildiğinde, vb.  Genellikle kendi öznitelikler oluşturmak gerekir.  Örneğin:
* "LoyaltyNumber." gibi bir öznitelik kalıcı hale getirmek müşterilere yönelik uygulama gerekiyor
* Bir kimlik sağlayıcısı, "uniqueUserGUID" gibi kaydedilmelidir bir benzersiz kullanıcı tanımlayıcısı olan"
* Özel kullanıcı yolculuğu "migrationStatus." gibi kullanıcı durumunu kalıcı hale getirmek gerekiyor

Azure AD B2C ile her kullanıcı hesabında depolanan öznitelik kümesini genişletebilirsiniz. Ayrıca okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md).

Uzantı özellikleri kullanıcı, nesneyi dizin şemasını.  Bu makalede bağlamını kullanarak aynı şeyi koşulları uzantı özelliği, özel öznitelik ve özel talep başvurun ve adı (uygulama, nesne, ilke) bağlama bağlı olarak değişir.

Bir kullanıcı için veri içerebilir olsa bile uzantı özellikleri yalnızca bir uygulama nesnesi üzerinde kaydedilebilir. Özelliği, uygulamaya eklenir. Uygulama nesnesi, bir uzantı özelliği kaydetmek için yazma erişimi verilmelidir. Herhangi bir tek nesneye 100 uzantı özellikleri (genelinde tüm türlerde ve tüm uygulamalarda) yazılabilir. Uzantı özellikleri, hedef dizin türüne eklenir ve Azure AD B2C dizin kiracısında hemen erişilebilir duruma gelir.
Uygulama silinirse, tüm kullanıcılar için bunları içeren herhangi bir veri yanı sıra bu uzantı özellikleri de kaldırılır. Bir uzantı özelliği uygulama tarafından silinirse, hedef dizin nesneleri ve Silinen değerleri kaldırılır.

Kiracıda kayıtlı bir uygulama bağlamında yalnızca uzantı özellikleri yok. Bu uygulamanın nesne kimliğini kullanan TechnicalProfile eklenmesi gerekir.

>[!NOTE]
>Azure AD B2C dizini genellikle adlı bir Web uygulaması içeren `b2c-extensions-app`.  Bu uygulama, öncelikle Azure portal tarafından oluşturulan özel talepler için b2c yerleşik ilkeleri tarafından kullanılır.  B2c özel ilkeler için uzantıları kaydetmek için bu uygulamayı kullanarak yalnızca ileri düzey kullanıcılar için önerilir.  Bu yönergeler, bu makalenin sonraki adımlar bölümünde dahil edilir.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Uzantı özellikleri depolamak için yeni bir uygulama oluşturma

1. Bir tarayıcı oturumu açın ve gidin [Azure portalında](https://portal.azure.com) ve yapılandırmak istediğiniz yönetici kimlik bilgileriyle oturum açar B2C dizin.
2. Tıklayın **Azure Active Directory** sol gezinti menüsünde. Daha fazla hizmet seçerek bulma gerekebilir >.
3. Seçin **uygulama kayıtları** tıklatıp **yeni uygulama kaydı**
4. Aşağıdaki önerilen girdileri sağlar:
    * Web uygulaması için bir ad belirtin: **WebApp GraphAPI DirectoryExtensions**
    * Uygulama türü: Web uygulaması/API'si
    * Oturum açma URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
5. **Oluştur**’u seçin.
6. Yeni oluşturulan web uygulamasını seçin.
7. Seçin **ayarları** > **gerekli izinler**.
8. API seçin **Windows Azure Active Directory**.
9. Uygulama izinleri bir onay işareti koyun: **dizin verilerini okuma ve yazma**ve ardından **Kaydet**.
10. Seçin **izinleri verin** ve onaylayın **Evet**.
11. Panonuza kopyalayın ve aşağıdaki tanımlayıcıların kaydedin:
    * **Uygulama Kimliği** . Örnek: `103ee0e6-f92d-4183-b576-8c3739027780`
    * **Nesne Kimliği**. Örnek: `80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>ApplicationObjectId eklemek için özel ilkeniz değiştirme

Adımları tamamlandığında [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md), indirdiğiniz ve değiştirdiğiniz [dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) adlı *TrustFrameworkBase.xml*,  *TrustFrameworkExtensions.xml*, *SignUpOrSignin.xml*, *ProfileEdit.xml*, ve *PasswordReset.xml*. Aşağıdaki adımlarda, bu dosyalarda yapılan değişiklikler yapmaya devam edin.

1. Açık *TrustFrameworkBase.xml* dosya ve ekleme `Metadata` bölümünde aşağıdaki örnekte gösterildiği gibi. Eklemek için daha önce kaydettiğiniz nesne kimliği `ApplicationObjectId` değeri ve uygulama kimliği için kayıtlı `ClientId` değeri: 

    ```xml
    <ClaimsProviders>
        <ClaimsProvider>
          <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
          <DisplayName>Azure Active Directory</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              
          <!-- Provide objectId and appId before using extension properties. -->
          <Metadata>
            <Item Key="ApplicationObjectId">insert objectId here</Item>
            <Item Key="ClientId">insert appId here</Item>
          </Metadata>
          <!-- End of changes -->
              
          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
          </CryptographicKeys>
          <IncludeInSso>false</IncludeInSso>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
    ```

>[!NOTE]
>TechnicalProfile yeni oluşturulan bir uzantı özelliği için ilk kez yazdığında, tek seferlik bir hatayla karşılaşabilirsiniz. Uzantı özelliği ilk kez kullanıldığı oluşturulur.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Yeni Uzantı özelliğini kullanarak / özel bir kullanıcı yolculuğunda özniteliği

1. Açık *ProfileEdit.xml* dosya.
2. Bir özel talep ekleme `loyaltyId`.  İçinde özel dahil ederek talep `<RelyingParty>` öğesi, uygulama için belirteç dahildir.
    
    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="ProfileEdit" />
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
          <OutputClaim ClaimTypeReferenceId="city" />

          <!-- Provide the custom claim identifier -->
          <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
          <!-- End of changes -->
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

3. Açık *TrustFrameworkExtensions.xml* dosya ve ekleme`<ClaimsSchema>` öğesi ve onun alt öğeleri için `BuildingBlocks` öğesi:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema> 
        <ClaimType Id="extension_loyaltyId"> 
          <DisplayName>Loyalty Identification Tag</DisplayName> 
          <DataType>string</DataType> 
          <UserHelpText>Your loyalty number from your membership card</UserHelpText> 
          <UserInputType>TextBox</UserInputType> 
        </ClaimType> 
      </ClaimsSchema>
    </BuildingBlocks>
    ```

4. Aynı `ClaimType` tanımına *TrustFrameworkBase.xml*. Ekleme bir `ClaimType` hem temel hem de uzantılar dosya tanımında genellikle gerekli değildir, ancak sonraki adımlar ekler beri `extension_loyaltyId` temel dosyasındaki TechnicalProfiles için ilke Doğrulayıcı temel dosyanın karşıya yükleme reddeder. . Kullanıcı yolculuğu yürütmesi içinde "ProfileEdit" adlı izleme için yararlı olabilir *TrustFrameworkBase.xml* dosya.  Düzenleyicinizde aynı ada sahip kullanıcı yolculuğunu arayın ve düzenleme adımı 5 TechnicalProfileReferenceID çağırır uyun = "ProfileUpdate SelfAsserted".  Arama ve akışı tanımak için bu TechnicalProfile denetleyin.

5. Açık *TrustFrameworkBase.xml* dosya ve ekleme `loyaltyId` TechnicalProfile "ProfileUpdate SelfAsserted" bir giriş ve çıkış talebi olarak verilir:

    ```xml
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <DisplayName>User ID signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        
        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->

      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
      </ValidationTechnicalProfiles>
    </TechnicalProfile>
    ```

6. İçinde *TrustFrameworkBase.xml* ekleyin `loyaltyId` TechnicalProfile "Uzantı özelliğindeki dizini geçerli kullanıcı için talep değerini kalıcı hale getirmek için AAD-UserWriteProfileUsingObjectId" için talep:

    ```xml
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <Metadata>
        <Item Key="Operation">Write</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="givenName" />
        <PersistedClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </PersistedClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

7. İçinde *TrustFrameworkBase.xml* ekleyin `loyaltyId` TechnicalProfile "Her zaman bir kullanıcı oturum açtığında uzantısı özniteliğinin değerini okumak için AAD-UserReadUsingObjectId" için talep. Şimdiye kadar yalnızca yerel hesapları flow'da TechnicalProfiles değiştirildi.  Yeni öznitelik akışı sosyal ve birleştirilmiş bir hesap isteniyorsa TechnicalProfiles farklı bir dizi değiştirilmesi gerekir. Sonraki adımlara bakın.

    ```xml
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <Metadata>
        <Item Key="Operation">Read</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="otherMails" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

## <a name="test-the-custom-policy"></a>Özel bir ilkeyi test etme

1. Açık **Azure AD B2C dikey** gidin **kimlik deneyimi Çerçevesi > özel ilkeleri**.
1. Yüklenmiş ve'ı tıklatın özel bir ilkeyi seçin **Şimdi Çalıştır** düğmesi.
1. Bir e-posta adresi kullanarak kaydolma olması gerekir.

Uygulamanız için arka extension_loyaltyId tarafından öncesinde bir özel talep olarak yeni bir uzantı özelliği içerir. kimlik belirteci gönderilir. Örneğe bakın.

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>Yeni akışlar Talep'i sosyal hesap oturum açma bilgileri için aşağıda listelenen TechnicalProfiles değiştirerek ekleyin. Bu iki TechnicalProfiles alternativeSecurityId kullanıcı nesnesindeki konum belirleyici kullanarak kullanıcı verilerini okuyup yazmak için hesap sosyal ve Federasyon oturum açma bilgileri tarafından kullanılır.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Yerleşik ve özel ilkeleri arasında aynı uzantı özniteliklerini kullanma.
Uzantı öznitelikleri (özel öznitelikler olarak da bilinir), portal deneyimi aracılığıyla eklediğinizde, bu öznitelikleri kullanarak kayıtlı ** b2c-extensions-her b2c kiracısında mevcut uygulama.  Bu uzantı öznitelikleri, özel ilkeniz kullanmak için:
1. B2c kiracınızda Portal.Azure.com gidin **Azure Active Directory** seçip **uygulama kayıtları**
2. Bulma, **b2c-extensions-app** ve bunu seçin
3. 'Temel' kayıt altında **uygulama kimliği** ve **nesne kimliği**
4. Bunları, AAD yaygın teknik profil meta verilerinde gibi şunlardır:

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

Portal deneyimi ile tutarlılığı sürdürmek için portal kullanıcı arabirimini kullanarak bu öznitelikler oluşturma *önce* özel ilkeleri kullanın.  Portalda "ActivationStatus" bir öznitelik oluşturduğunuzda, ona gibi başvurması gerekir:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Başvuru

* A **teknik profil (TP)** , olarak düşünülebilir bir öğe türü bir *işlevi* uç noktanın adı, meta verileri, protokol tanımlar ve talep değişimi ayrıntıları, kimlik Deneyimi çerçevesi gerçekleştirmeniz gerekir.  Olduğunda bu *işlevi* bir düzenleme adım çağrılan veya başka bir TechnicalProfile, InputClaims ve OutputClaims parametreler çağıran tarafından sağlanır.


* Uzantı özellikleri üzerinde tam işleme için bkz [DIRECTORY şema UZANTILARI | GRAPH API KAVRAMLARI](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Uzantı öznitelikleri Graph API'de kuralıyla adlı `extension_ApplicationObjectID_attributename`. Özel ilkeler, extension_attributename, böylece XML ApplicationObjectId atlama olarak uzantıları öznitelikleri bakın.
