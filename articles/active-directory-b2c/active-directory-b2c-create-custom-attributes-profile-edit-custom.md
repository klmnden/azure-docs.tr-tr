---
title: Özel ilkeleri Azure Active Directory B2C için kendi öznitelikleri ekleme | Microsoft Docs
description: Uzantı özellikleri ve özel özniteliklerini kullanarak ve bunları kullanıcı arabiriminin dahil olmak üzere bir kılavuz.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 41c3db1c9a7295d939aa34a36f86c0dfa9fecd91
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797133"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: Özel bir profilde özel öznitelikler kullanın ilkesini Düzenle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure Active Directory (Azure AD) B2C dizininizde özel bir öznitelik oluşturun. Bu yeni bir öznitelik, özel bir profil düzenleme kullanıcı yolculuğu talebi olarak kullanacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları [Azure Active Directory B2C: Özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-ad-b2c-by-using-custom-policies"></a>Özel ilkeler kullanarak Azure AD B2C uygulamasında müşterilerinizin hakkında bilgi toplamak için özel öznitelikler kullanma
Azure AD B2C dizininizi yerleşik bir öznitelikleri kümesi ile birlikte gelir. Örnekler **verilen ad**, **Soyadı**, **Şehir**, **posta kodu**, ve **userPrincipalName**. Genellikle bu örnekleri gibi kendi öznitelikler oluşturmak gerekir:
* Müşteri yönelik bir uygulama gibi bir öznitelik için kalıcı hale getirilmesi **LoyaltyNumber.**
* Bir kimlik sağlayıcısı gibi bir benzersiz kullanıcı tanımlayıcısı olan **uniqueUserGUID** , kaydedilmesi gerekir.
* Özel kullanıcı yolculuğu için bir kullanıcı gibi durumunu kalıcı hale getirilmesi **migrationStatus**.

Azure AD B2C, her kullanıcı hesabında depolanan öznitelik kümesini genişletir. Ayrıca okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API'si](active-directory-b2c-devquickstarts-graph-dotnet.md).

Uzantı özellikleri kullanıcı, nesneyi dizin şemasını. Koşulları *uzantı özelliği*, *özel öznitelik*, ve *özel talep* bağlamında bu makalede, aynı şeyi bakın. Adı, uygulama, nesne veya ilke gibi bağlama bağlı olarak değişir.

Bir kullanıcıya ait veriler içeriyor olabilir ancak uzantı özellikleri yalnızca bir uygulama nesnesi üzerinde kaydedilebilir. Özelliği, uygulamaya eklenir. Uygulama nesnesi, bir uzantı özelliği kaydetmek için yazma erişimi olmalıdır. Herhangi bir tek nesneye 100 uzantı özellikleri, tüm türleri ve tüm uygulamalar arasında yazılabilir. Uzantı özellikleri hedef dizin türüne eklenir ve Azure AD B2C dizin kiracısında hemen erişilebilir duruma gelir.
Uygulama silinirse, tüm kullanıcılar için bunları içeren herhangi bir veri yanı sıra bu uzantı özellikleri de kaldırılır. Bir uzantı özelliği uygulama tarafından silinirse, hedef dizin nesneleri kaldırılır ve değerleri silinir.

Kiracıda kayıtlı bir uygulama bağlamında yalnızca uzantı özellikleri yok. Uygulama kimliği olarak eklenmesi gereken nesne **TechnicalProfile** bunu kullanır.

>[!NOTE]
>Azure AD B2C dizini genellikle adlı bir web uygulaması içeren `b2c-extensions-app`. Bu uygulama, öncelikle Azure portal tarafından oluşturulan özel talepler için B2C yerleşik ilkeleri tarafından kullanılır. Yalnızca ileri düzey kullanıcılar bu uygulamayı kullanarak B2C özel ilkeler için uzantıları kaydetmenizi öneririz.  
Yönergeleri dahil edilecek **sonraki adımlar** bu makaledeki bir bölüm.

## <a name="create-a-new-application-to-store-the-extension-properties"></a>Uzantı özellikleri depolamak için yeni bir uygulama oluşturun

1. Bir tarayıcı oturumu açın ve gidin [Azure portalında](https://portal.azure.com). B2C dizini yapılandırmak istediğiniz yönetici kimlik bilgileriyle oturum açın.
2. Seçin **Azure Active Directory** sol gezinti menüsünde. Seçerek bulmak gerek duyabileceğiniz **diğer hizmetler**.
3. **Uygulama kayıtları**'nı seçin. **Yeni uygulama kaydı**’nı seçin.
4. Aşağıdaki girdileri sağlar:
    * Web uygulaması için bir ad: **WebApp GraphAPI DirectoryExtensions**.
    * Uygulama türü: **Web uygulaması/API'si**.
    * Oturum açma URL'si: **https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions**.
5. **Oluştur**’u seçin.
6. Yeni oluşturulan web uygulamasını seçin.
7. Seçin **ayarları** > **gerekli izinler**.
8. API seçin **Windows Azure Active Directory**.
9. Uygulama izinleri bir onay işareti girin: **Dizin veri okuma ve yazma**. Daha sonra **Kaydet**’e tıklayın.
10. Seçin **izinleri verin** ve onaylayın **Evet**.
11. Aşağıdaki tanımlayıcıların panonuza kopyalayın ve kaydedin:
    * **Uygulama Kimliği**. Örnek: `103ee0e6-f92d-4183-b576-8c3739027780`.
    * **Nesne Kimliği**. Örnek: `80d8296a-da0a-49ee-b6ab-fd232aa45201`.

## <a name="modify-your-custom-policy-to-add-the-applicationobjectid"></a>Eklemek için özel ilkeniz değiştirme **ApplicationObjectId**

Ardından olduğunda adımda [Azure Active Directory B2C: Özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md), indirdiğiniz ve değiştirdiğiniz [örnek dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) adlı **TrustFrameworkBase.xml**, **TrustFrameworkExtensions.xml**, **SignUpOrSignin.xml**, **ProfileEdit.xml**, ve **PasswordReset.xml**. Bu adımda, bu dosyalar için daha fazla değişiklik yapmanızı ister.

* Açık **TrustFrameworkBase.xml** dosya ve ekleme `Metadata` bölümünde aşağıdaki örnekte gösterildiği gibi. Nesne kimliği için daha önce kaydettiğiniz Ekle `ApplicationObjectId` değeri ve için kayıtlı uygulama kimliği `ClientId` değeri: 

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

> [!NOTE]
> Zaman **TechnicalProfile** Yazar ilk kez yeni oluşturulan bir uzantı özelliği için tek seferlik bir hatayla karşılaşabilirsiniz. Uzantı özelliği ilk kez kullanıldığı oluşturulur.

## <a name="use-the-new-extension-property-or-custom-attribute-in-a-user-journey"></a>Yeni Uzantı özelliği veya özel öznitelik, bir kullanıcı yolculuğunda kullanın

1. Açık **ProfileEdit.xml** dosya.
2. Bir özel talep ekleme `loyaltyId`. İçinde özel dahil ederek talep `<RelyingParty>` öğesi, dahil uygulaması için belirteci.
    
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

3. Açık **TrustFrameworkExtensions.xml** dosya ve ekleme`<ClaimsSchema>` öğesi ve onun alt öğeleri için `BuildingBlocks` öğesi:

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

4. Aynı `ClaimType` tanımına **TrustFrameworkBase.xml**. Eklemek gerekli olmayan bir `ClaimType` tanımı hem temel hem de uzantı dosyaları. Ancak, sonraki adımlar eklemek `extension_loyaltyId` için **TechnicalProfiles** temel dosyasında. Bu nedenle olmadan temel dosyanın karşıya yükleme ilkesi Doğrulayıcı reddeder. Adlı kullanıcı yolculuğu yürütülmesini izlemek yararlı olabilir **ProfileEdit** içinde **TrustFrameworkBase.xml** dosya. Düzenleyicinizde aynı ada sahip kullanıcı yolculuğunu arayın. Düzenleme adımı 5 çağırır olduğunu gözlemek **TechnicalProfileReferenceID = "SelfAsserted ProfileUpdate**. Arama yapın ve bu İnceleme **TechnicalProfile** akışı tanımak için.

5. Açık **TrustFrameworkBase.xml** dosya ve ekleme `loyaltyId` içinde bir giriş ve çıkış talebi olarak **TechnicalProfile SelfAsserted-ProfileUpdate**:

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

6. İçinde **TrustFrameworkBase.xml** ekleyin `loyaltyId` için talep **TechnicalProfile AAD-UserWriteProfileUsingObjectId**. Bu ek uzantı özelliğindeki dizini geçerli kullanıcı için talep değerini devam eder:

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

7. İçinde **TrustFrameworkBase.xml** ekleyin `loyaltyId` için talep **TechnicalProfile AAD-UserReadUsingObjectId** kullanıcının her oturum açtığında uzantısı özniteliğinin değeri okunamıyor. Şu ana kadar **TechnicalProfiles** yalnızca yerel hesapları flow'da değiştirildi. Yeni öznitelik, bir sosyal veya Federasyon hesabı farklı bir dizi akışını istiyorsanız **TechnicalProfiles** değiştirilmesi gerekir. Bkz: **sonraki adımlar** bölümü.

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

1. Azure AD B2C dikey penceresini açın ve gidin **kimlik deneyimi çerçevesi** > **özel ilkeleri**.
1. Karşıya yüklediğiniz özel bir ilkeyi seçin. Seçin **Şimdi Çalıştır**.
1. Bir e-posta adresi kullanarak kaydolun.

Uygulamanıza geri yeni uzantı özelliğinin önünde bir özel talep olarak içeren kimlik belirteci gönderilen **extension_loyaltyId**. Aşağıdaki örneğe bakın:

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

1. Aşağıdaki değiştirerek sosyal hesapları için oturum açmak için akışlar için yeni talep ekleyin **TechnicalProfiles**. Sosyal ve Federasyon hesaplarını kullanan bu iki **TechnicalProfiles** oturum açmak için. Yazma ve kullanıcı verilerini okumak **alternativeSecurityId** Bulucu kullanıcı nesnesinin olarak.

   ```xml
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
   ```

2. Aynı uzantı öznitelikleri arasında yerleşik ve özel ilkeleri kullanın. Uzantısı ya da özel, öznitelikleri portal deneyimi eklediğinizde, bu öznitelikleri kullanarak kayıtlı **b2c-extensions-app** her B2C kiracısında mevcut. Uzantı öznitelikleri, özel ilkeniz kullanmak için aşağıdaki adımları uygulayın:

   a. B2C kiracınızda Portal.Azure.com gidin **Azure Active Directory** seçip **uygulama kayıtları**.  
   b. Bulma, **b2c-extensions-app** ve bu seçeneği belirleyin.  
   c. Altında **Essentials**, girin **uygulama kimliği** ve **nesne kimliği**.  
   d. İçine dahil, **AAD yaygın** TechnicalProfile meta verileri:  

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

3. Portal deneyimiyle tutarlı kalır. Bu öznitelikler, özel ilkelerinizi kullanmadan önce portal kullanıcı arabirimini kullanarak oluşturun. Bir öznitelik oluşturduğunuzda **ActivationStatus** portalında, kendisine şu şekilde başvurması gerekir:

   ```
   extension_ActivationStatus in the custom policy.
   extension_<app-guid>_ActivationStatus via Graph API.
   ```

## <a name="reference"></a>Başvuru

Uzantı özellikleri hakkında daha fazla bilgi için bkz [Directory şema uzantıları | Graph API kavramları](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

> [!NOTE]
> * A **TechnicalProfile** bir öğe türü veya bir uç noktanın adı, meta verileri ve protokolü tanımlayan işlevi. **TechnicalProfile** kimlik deneyimi çerçevesi gerçekleştiren talep değişimi ayrıntıları. Ne zaman bu işlev çağrıldığında bir düzenleme adımı veya başka bir **TechnicalProfile**, **InputClaims** ve **OutputClaims** parametreler çağıran tarafından sağlanır .  
> * Uzantı öznitelikleri Graph API'de kuralı kullanarak adlı `extension_ApplicationObjectID_attributename`.  
> * Özel ilkeler, uzantı öznitelikleri başvurduğu **extension_attributename**. Bu başvuru atlar **ApplicationObjectId** XML.
