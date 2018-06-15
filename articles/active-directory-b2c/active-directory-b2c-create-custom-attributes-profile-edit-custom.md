---
title: Azure Active Directory B2C özel ilkelerinde kendi öznitelikleri ekleme | Microsoft Docs
description: Uzantısı özellikleri, özel öznitelikleri kullanarak ve kullanıcı arabiriminde dahil olmak üzere bir kılavuz.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 08/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e4dfb92257dca4069905f17e1c3ccd43d87cd45c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34710167"
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: İlke oluşturma ve özel bir profilde özel öznitelikleri kullanma Düzenle

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makalede, Azure AD B2C dizininizde özel bir öznitelik oluşturma ve bu yeni öznitelik özel talep profil düzenleme kullanıcı gezisine de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Makalesindeki adımları [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a>Müşterilerinize Azure Active Directory B2C içinde özel ilkelerini kullanma hakkında bilgi toplamak için özel öznitelikler kullanın
Azure Active Directory (Azure AD) B2C dizininize yerleşik bir öznitelikler kümesi ile birlikte gelir: ad, Soyadı, şehir, posta kodu, userPrincipalName verildiğinde, vb.  Genellikle kendi öznitelikleri oluşturmanız gerekir.  Örneğin:
* "LoyaltyNumber." gibi bir öznitelik kalıcı hale getirmek bir müşteri dönük uygulaması gerekiyor
* Bir kimlik sağlayıcısı "uniqueUserGUID" gibi kaydedilmelidir benzersiz kullanıcı tanımlayıcısı var"
* Özel kullanıcı gezisine "migrationStatus." gibi kullanıcı durumunun kalıcı olması gerekiyor

Azure AD B2C ile her kullanıcı hesabında depolanan öznitelikler kümesi genişletebilirsiniz. Aynı zamanda okuma ve yazma bu öznitelikleri kullanarak [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

Uzantısı özellikleri dizinde kullanıcı nesneleri şemasını.  Bu makalede bağlamında aynı şeyi koşulları uzantı özelliği, özel öznitelik ve özel talep başvurun ve adı bağlamın (uygulama, nesne, ilke) bağlı olarak değişir.

Bir kullanıcı için verileri içerebilir olsa bile uzantısı özellikleri yalnızca bir uygulama nesnesinde kaydedilebilir. Uygulamaya iliştirilmiş özelliği. Uygulama nesnesi bir uzantı özelliği kaydetmek için yazma erişimi verilmesi gerekir. 100 uzantısı özellikleri (arasında tüm türleri ve tüm uygulamalar) tek bir nesne olarak yazılabilir. Uzantısı özellikleri için hedef dizin türü eklenir ve Azure AD B2C dizini kiracısında hemen erişilebilir hale gelir.
Uygulama silinirse, tüm kullanıcılar için içerdikleri herhangi bir veri yanı sıra bu uzantısı özellikleri de kaldırılır. Uzantı özelliği uygulama tarafından silinirse, hedef dizin nesneleri ve Silinen değerleri kaldırılır.

Yalnızca Kiracı kayıtlı bir uygulama bağlamında uzantısı özellikleri yok. Bu uygulama nesnesi kimliği kullanan TechnicalProfile eklenmesi gerekir.

>[!NOTE]
>Azure AD B2C dizini genellikle adlı bir Web uygulaması içeren `b2c-extensions-app`.  Bu uygulama, öncelikle Azure portal tarafından oluşturulan özel talepler için b2c yerleşik ilkeleri tarafından kullanılır.  B2c özel ilkeler için Uzantılar kaydetmek için bu uygulamayı kullanarak yalnızca ileri düzey kullanıcıların yapması önerilir.  Bunun için yönergeler bu makalenin sonraki adımlar bölümünde dahil edilmiştir.


## <a name="creating-a-new-application-to-store-the-extension-properties"></a>Uzantı özelliklerini depolamak için yeni bir uygulama oluşturma

1. Bir tarayıcı oturumu açın ve gidin [Azure portal](https://portal.azure.com) yapılandırmak istediğiniz yönetici kimlik bilgileriyle oturum açın, B2C dizini.
1. Tıklatın **Azure Active Directory** sol gezinti menüsünde. Daha fazla hizmet seçerek bulma gerekebilir >.
1. Seçin **uygulama kayıtlar** tıklatıp **yeni uygulama kaydı**
1. Aşağıdaki önerilen girdileri sağlar:
  * Web uygulaması için bir ad belirtin: **WebApp GraphAPI DirectoryExtensions**
  * Uygulama türü: Web app/API
  * Oturum açma URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions
1. Seçin ** oluşturun. Başarılı bir şekilde tamamlandığında görünür **bildirimleri**
1. Yeni oluşturulan web uygulamasını seçin: **WebApp GraphAPI DirectoryExtensions**
1. Select ayarları: **gerekli izinler**
1. API seçin **Windows Azure Active Directory**
1. Uygulama izinleri bir onay işareti koyun: **dizin verilerini okuma ve yazma**, ve **Kaydet**
1. Seçin **izinleri verin** ve onaylayın **Evet**.
1. Panonuza kopyalayın ve aşağıdaki tanımlayıcılar GraphAPI WebApp DirectoryExtensions Kaydet > Ayarlar > Özellikler >
*  **Uygulama Kimliği** . Örnek: `103ee0e6-f92d-4183-b576-8c3739027780`
* **Nesne Kimliği**. Örnek: `80d8296a-da0a-49ee-b6ab-fd232aa45201`



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a>ApplicationObjectId eklemek için özel ilkeniz değiştirme

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
><TechnicalProfile Id="AAD-Common"> Öğeleri dahil olduğundan ve tüm Azure Active Directory TechnicalProfiles içinde öğesini kullanarak yeniden için "ortak" olarak adlandırılır: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`

>[!NOTE]
>TechnicalProfile yeni oluşturulan uzantı özelliği ilk kez yazdığında tek seferlik bir hatayla karşılaşabilirsiniz.  Uzantı özelliği ilk defa oluşturulur.  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a>Yeni Uzantı özelliği kullanılarak / özel bir kullanıcı gezisine özniteliği


1. Düzenleme kullanıcı gezisine ilkeniz tanımlayan bağlı olan Party(RP) dosyasını açın.  Başlıyorsanız, doğrudan Azure portalında Azure B2C özel İlkesi bölümünden, önceden yapılandırılmış RP PolicyEdit dosya sürümünü indirmek için önerilir olabilir.  Alternatif olarak, depolama klasörünüzden XML dosyanızı açın.
2. Bir özel talep eklemek `loyaltyId`.  İçinde özel ekleyerek talep `<RelyingParty>` öğesi, bir parametre olarak geçirilen UserJourney TechnicalProfiles ve uygulama için belirteç dahil.
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. İlke uzantısının talep tanımını ekleyin `TrustFrameworkExtensions.xml` içinde `<ClaimsSchema>` gösterildiği gibi öğesi.
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. Aynı talep temel ilke dosyası tanımına ekleme `TrustFrameworkBase.xml`.  
>Ekleme bir `ClaimType` hem temel hem de uzantıları dosya tanımında normalde gerekli değildir, ancak, sonraki adımlar temel dosyasındaki TechnicalProfiles extension_loyaltyId ekleyecek beri ilke Doğrulayıcı karşıya yükleme temel dosyanın olmadan reddeder.
>TrustFrameworkBase.xml dosyasındaki "ProfileEdit" adlı kullanıcı gezisine yürütülmesini izlemek yararlı olabilir.  Düzenleyicinizde aynı ada sahip kullanıcı gezisine arayın ve Orchestration adım 5 TechnicalProfileReferenceID çağırır uyun = "SelfAsserted-ProfileUpdate".  Arama ve akışla tanımak için bu TechnicalProfile inceleyin.
5. Giriş ve çıkış TechnicalProfile "SelfAsserted-ProfileUpdate" talebi olarak loyaltyId Ekle
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

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updated by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updated by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. Talep TechnicalProfile "Uzantısı özelliğinde dizini geçerli kullanıcı için talep değerini kalıcı hale getirmek için AAD-UserWriteProfileUsingObjectId" ekleyin.
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
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. Talep TechnicalProfile bir kullanıcı oturum açtığında her zaman uzantısı özniteliğinin değerini okumaya "AAD-UserReadUsingObjectId" ekleyin. Bugüne kadarki TechnicalProfiles yerel hesaplar için yalnızca akış içinde değiştirildi.  Yeni öznitelik bir sosyal ve Federasyon hesabı akışında isterseniz, farklı bir TechnicalProfiles kümesi değiştirilmesi gerekiyor. Sonraki adımlara bakın.

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
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
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
>IncludeTechnicalProfile öğesi bu TechnicalProfile AAD yaygın tüm öğeleri ekler.

## <a name="test-the-custom-policy-using-run-now"></a>"Şimdi Çalıştır" kullanarak özel ilkeyi test etme
1. Açık **Azure AD B2C dikey** gidin **kimlik deneyimi Framework > özel ilkeleri**.
1. Karşıya yüklenen ve'ı tıklatın özel ilkeyi seçin **Şimdi Çalıştır** düğmesi.
1. Bir e-posta adresi kullanarak kaydolabilirsiniz olması gerekir.

Uygulamanıza geri yeni uzantı özelliği tarafından extension_loyaltyId öncesinde özel bir talep içerir kimliği belirteç gönderdi. Örneğe bakın.

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

### <a name="add-the-new-claim-to-the-flows-for-social-account-logins-by-changing-the-technicalprofiles-listed-below-these-two-technicalprofiles-are-used-by-socialfederated-account-logins-to-write-and-read-the-user-data-using-the-alternativesecurityid-as-the-locator-of-the-user-object"></a>Akışlar için yeni talep sosyal hesap oturum açma, aşağıda listelenen TechnicalProfiles değiştirerek ekleyin. Bu iki TechnicalProfiles tarafından sosyal ve Federasyon hesap oturumu açma alternativeSecurityId kullanıcı nesnesi Bulucu kullanarak kullanıcı verilerini okuyup yazmak için kullanılır.
```xml
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

Yerleşik ve özel ilkeleri arasında aynı uzantı öznitelikleri kullanma.
Portal deneyimi aracılığıyla uzantı öznitelikleri (diğer adıyla özel öznitelikleri) eklediğinizde, bu öznitelikleri kullanarak kayıtlı ** b2c uzantıları-her b2c kiracınızda bulunan uygulama.  Bu uzantı öznitelikleri, özel ilkeniz kullanmak için:
1. Portal.Azure.com, b2c kiracınızın içinde gitmek **Azure Active Directory** seçip **uygulama kayıtlar**
2. Bulma, **b2c uzantıları uygulaması** ve seçin
3. 'Essentials' kaydı altında **uygulama kimliği** ve **nesne kimliği**
4. Bunları AAD genel teknik, profil meta verileri gibi aşağıdaki gibi ekleyin:

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

Portal deneyimi ile tutarlılığını korumak için portalı kullanıcı arabirimini kullanarak bu öznitelikler oluşturmak *önce* , özel ilkelerini kullanın.  Portalda "ActivationStatus" özniteliği oluşturduğunuzda, ona gibi başvurması gerekir:

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a>Başvuru

* A **teknik profil (TP)** , olarak düşünülebilir bir öğe türü bir *işlevi* tanımlayan bir uç noktanın adı, meta verilerini, protokol ve exchange kimlik deneyimi Framework gerçekleştirmesi gereken talepler ayrıntıları.  Zaman bu *işlevi* bir orchestration adım çağrılan veya başka bir TechnicalProfile, InputClaims ve OutputClaims parametreler çağıran tarafından sağlanır.


* Uzantısı özelliklerindeki tam işleme için makaleye bakın [DIRECTORY şema UZANTILARI | GRAFİK API'Sİ KAVRAMLARI](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)

>[!NOTE]
>Grafik API'si uzantı öznitelikleri, kural kullanılarak adlandırılması `extension_ApplicationObjectID_attributename`. Özel ilkeler uzantıları öznitelikleri extension_attributename, böylece XML ApplicationObjectId atlama olarak bakın.
