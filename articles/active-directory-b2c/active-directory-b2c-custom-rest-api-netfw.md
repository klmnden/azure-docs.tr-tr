---
title: "Azure Active Directory B2C: Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine kullanıcı girişini doğrulama tümleştirin."
description: "Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine kullanıcı girişini doğrulama tümleştirin."
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 09/30/2017
ms.author: yoelh
ms.openlocfilehash: fd9c95ae78590aa772fde10c8c80914c905767a8
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-of-user-input"></a>Kullanıcı girişi doğrulama Azure AD B2C kullanıcı Yolculuğunuzun REST API talep alışverişlerine tümleştirme
Kimlik deneyimi çerçevesiyle hangi altını çizen Azure Active Directory B2C (Azure AD B2C) tümleştirebileceğiniz kullanıcı gezisine bir RESTful API'si ile. Bu kılavuzda, Azure AD B2C .NET Framework RESTful Hizmetleri (web API) ile nasıl etkileşim girer öğreneceksiniz.

## <a name="introduction"></a>Giriş
Azure AD B2C kullanarak RESTful hizmetinizi çağırarak kullanıcı gezisine için kendi iş mantığı ekleyebilirsiniz. Kimlik deneyimi Framework RESTful hizmeti verileri gönderir. bir *giriş talep* koleksiyonu ve RESTful içinde geri verileri alan bir *çıkış talep* koleksiyonu. RESTful Hizmeti Tümleştirme ile şunları yapabilirsiniz:

* **Kullanıcı giriş verilerini doğrulamak**: Azure AD ile devam ettirmeden hatalı biçimlendirilmiş bir veri bu eylemi engeller. Kullanıcı değeri geçerli değil, RESTful hizmetiniz bir girdi sağlanması kullanıcıya yönlendiren bir hata iletisi döndürür. Örneğin, kullanıcı tarafından sağlanan e-posta adresi, müşterinizin veritabanında var olduğunu doğrulayın.
* **Giriş talep üzerine**: bir kullanıcı adı tüm küçük harfler veya tüm büyük harflerle girerse, örneğin, adı büyük harfle yalnızca ilk harfi ile biçimlendirebilirsiniz.
* **Kullanıcı verilerini daha fazla şirket iş kolu satır uygulamaları ile tümleştirerek zenginleştirmek**: bilgisayarınızı RESTful hizmeti kullanıcının e-posta adresini alır, Müşteri'nin veritabanını sorgulamak ve Azure AD B2C'ye kullanıcının bağlılık numarasını döndürür. İade talepleri depolanabilir, kullanıcının Azure AD hesabı sonraki hesaplanan *düzenleme adımlarının*, veya erişim belirtecinde yer dahildir.
* **Özel iş mantığı çalıştırmak**: anında iletme bildirimleri göndermek, şirket veritabanlarını güncelleştirebilir, bir kullanıcı geçiş işlemini çalıştırmak, izinleri yönetmek, Denetim veritabanları ve başka eylemler gerçekleştirebilir.

Aşağıdaki yollarla RESTful hizmetleriyle tümleştirme tasarlayabilirsiniz:

* **Doğrulama teknik profil**: Belirtilen teknik profili doğrulama teknik profilinde RESTful hizmeti çağrısı olur. Kullanıcı gezisine İleri gitmeden önce doğrulama teknik profili kullanıcı tarafından sağlanan verileri doğrular. Doğrulama teknik profili ile şunları yapabilirsiniz:
   * Giriş talepler göndermek.
   * Giriş talepleri doğrulayın ve özel hata iletileri atar.
   * Geri çıkış talepler göndermek.

* **Talep exchange**: Bu tasarım doğrulama teknik profiline benzer, ancak orchestration adım içinde gerçekleşir. Bu tanım sınırlıdır:
   * Giriş talepler göndermek.
   * Geri çıkış talepler göndermek.

## <a name="restful-walkthrough"></a>RESTful gözden geçirme
Bu kılavuzda, .NET Framework web kullanıcı girişini doğrular ve kullanıcı bağlılık numarasını sağlayan API geliştirin. Örneğin, uygulamanızın erişim izni verebilir *Platin avantajları* bağlılık numarasına göre.

Genel Bakış:
* RESTful hizmet (.NET Framework web API) geliştirin.
* RESTful hizmeti kullanıcı gezisine kullanın.
* Giriş talep göndermek ve bunları kodunuzda okuyun.
* Kullanıcının ilk adını doğrulayın.
* Geri bağlılık numarasını gönderin. 
* Bağlılık numarası bir JSON Web Token (JWT için) ekleyin.

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri ile çalışmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-an-aspnet-web-api"></a>1. adım: bir ASP.NET web API oluşturma

1. Visual Studio'da seçerek bir proje oluşturun **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje** penceresinde, seçin **Visual C#** > **Web** > **ASP.NET Web uygulaması (.NET Framework)**.

3. İçinde **adı** kutusuna, uygulama için bir ad yazın (örneğin, *Contoso.AADB2C.API*) ve ardından **Tamam**.

    ![Yeni visual studio projesi oluşturma](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4. İçinde **yeni ASP.NET Web uygulaması** penceresinde, seçin bir **Web API** veya **Azure API uygulaması** şablonu.

    ![Web API şablonunu seçin](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

5. Emin olun, kimlik doğrulamasının ayarlanmış **doğrulaması yok**.

6. Seçin **Tamam** projesi oluşturmak için.

## <a name="step-2-prepare-the-rest-api-endpoint"></a>2. adım: REST API uç noktası hazırlama

### <a name="step-21-add-data-models"></a>2.1. adım: veri modelleri ekleme
Modelleri giriş talepleri temsil eder ve çıktı veri RESTful hizmetinizde talepleri. Kodunuzu bir JSON dizesinde giriş talepleri modelden nesnesine bir C# (modelinizi) seri durumdan giriş verilerini okur. ASP.NET web API otomatik olarak çıkış talep modele geri JSON seri durumdan çıkarır ve ardından HTTP yanıt iletisinin gövdesine seri hale getirilmiş verileri yazar. 

Aşağıdakileri yaparak giriş talepleri temsil eden bir model oluşturun:

1. Çözüm Gezgini zaten açık değilse seçin **Görünüm** > **Çözüm Gezgini**. 
2. Çözüm Gezgini'nde sağ **modelleri** klasöründe seçin **Ekle**ve ardından **sınıfı**.

    ![Model ekleme](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-model.png)

3. Sınıf adını `InputClaimsModel`ve ardından aşağıdaki özellikleri ekleyin `InputClaimsModel` sınıfı:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class InputClaimsModel
        {
            public string email { get; set; }
            public string firstName { get; set; }
            public string lastName { get; set; }
        }
    }
    ```

4. Yeni bir model oluşturmak `OutputClaimsModel`ve ardından aşağıdaki özellikleri ekleyin `OutputClaimsModel` sınıfı:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class OutputClaimsModel
        {
            public string loyaltyNumber { get; set; }
        }
    }
    ```

5. Daha fazla bir model oluşturma `B2CResponseContent`, giriş doğrulama hatası iletileri atmak için kullanın. Aşağıdaki özellikleri ekleyin `B2CResponseContent` sınıfı, eksik başvuruları sağlayın ve ardından dosyayı kaydedin:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class B2CResponseContent
        {
            public string version { get; set; }
            public int status { get; set; }
            public string userMessage { get; set; }

            public B2CResponseContent(string message, HttpStatusCode status)
            {
                this.userMessage = message;
                this.status = (int)status;
                this.version = Assembly.GetExecutingAssembly().GetName().Version.ToString();
            }    
        }
    }
    ```

### <a name="step-22-add-a-controller"></a>2.2. adım: bir denetleyici ekleme
Web API'de, bir _denetleyicisi_ HTTP isteklerini işleyen bir nesnedir. Denetleyici çıkış talep veya, ilk adı, geçersiz bir çakışma HTTP hata iletisi döndürürse döndürür.

1. Çözüm Gezgini'nde sağ **denetleyicileri** klasöründe seçin **Ekle**ve ardından **denetleyicisi**.

    ![Yeni denetleyici ekleyin](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2. İçinde **İskele Ekle** penceresinde, seçin **Web API denetleyici - boş**ve ardından **Ekle**.

    ![Select Web API 2 denetleyici - boş](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3. İçinde **denetleyici Ekle** penceresinde, denetleyici adı **IdentityController**ve ardından **Ekle**.

    ![Denetleyici adı yazın](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    Yapı iskelesi adlı bir dosya oluşturur *IdentityController.cs* içinde *denetleyicileri* klasör.

4. Varsa *IdentityController.cs* dosyası değil açık zaten, çift tıklayın ve ardından dosyasındaki kodu aşağıdaki kodla değiştirin:

    ```csharp
    using Contoso.AADB2C.API.Models;
    using Newtonsoft.Json;
    using System;
    using System.NET;
    using System.Web.Http;

    namespace Contoso.AADB2C.API.Controllers
    {
        public class IdentityController: ApiController
        {
            [HttpPost]
            public IHttpActionResult SignUp()
            {
                // If no data came in, then return
                if (this.Request.Content == null) throw new Exception();

                // Read the input claims from the request body
                string input = Request.Content.ReadAsStringAsync().Result;

                // Check the input content value
                if (string.IsNullOrEmpty(input))
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Request content is empty", HttpStatusCode.Conflict));
                }

                // Convert the input string into an InputClaimsModel object
                InputClaimsModel inputClaims = JsonConvert.DeserializeObject(input, typeof(InputClaimsModel)) as InputClaimsModel;

                if (inputClaims == null)
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Can not deserialize input claims", HttpStatusCode.Conflict));
                }

                // Run an input validation
                if (inputClaims.firstName.ToLower() == "test")
                {
                    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Test name is not valid, please provide a valid name", HttpStatusCode.Conflict));
                }

                // Create an output claims object and set the loyalty number with a random value
                OutputClaimsModel outputClaims = new OutputClaimsModel();
                outputClaims.loyaltyNumber = new Random().Next(100, 1000).ToString();

                // Return the output claim(s)
                return Ok(outputClaims);
            }
        }
    }
    ```

## <a name="step-3-publish-the-project-to-azure"></a>3. adım: Proje için Azure yayımlama
1. Çözüm Gezgini'nde sağ **Contoso.AADB2C.API** proje ve ardından **Yayımla**.

    ![Microsoft Azure App Service'te yayımlama](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-1.png)

2. İçinde **Yayımla** penceresinde, seçin **Microsoft Azure App Service**ve ardından **Yayımla**.

    ![Yeni Microsoft Azure App Service oluştur](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    **App Service Oluştur** penceresi açılır. İçinde Microsoft azure'da ASP.NET web uygulamasını çalıştırmak için tüm gerekli Azure kaynakları oluşturun.

    > [!NOTE]
    >Yayımlama hakkında daha fazla bilgi için bkz: [bir ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet#publish-to-azure).

3. İçinde **Web uygulaması adı** benzersiz uygulama adı yazın (geçerli karakterler a-z, 0-9 ve tire (-). Http://<app_name>.azurewebsites.NET, web uygulamasının URL'si şöyledir nerede *app_name* web uygulamanızın adıdır. Otomatik oluşturulmuş benzersiz adı kabul edebilirsiniz.

    ![Uygulama hizmeti özellikleri sağlar](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4. Azure kaynaklarını oluşturmaya başlamak için seçin **oluşturma**.  
    ASP.NET web uygulaması oluşturulduktan sonra sihirbazın Azure'da yayımlar ve ardından varsayılan tarayıcıda uygulama başlatır.

6. Web uygulamanızın URL'sini kopyalayın.

## <a name="step-4-add-the-new-loyaltynumber-claim-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>4. adım: yeni ekleme `loyaltyNumber` TrustFrameworkExtensions.xml dosyanızı şemaya talep
`loyaltyNumber` Talep bizim şemada henüz tanımlanmamış. İçinde bir tanım ekleyin `<BuildingBlocks>` başında bulabilirsiniz öğesi *TrustFrameworkExtensions.xml* dosya.

```xml
<BuildingBlocks>
    <ClaimsSchema>
        <ClaimType Id="loyaltyNumber">
            <DisplayName>loyaltyNumber</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Customer loyalty number</UserHelpText>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-5-add-a-claims-provider"></a>5. adım: bir talep sağlayıcı ekleme 
Her Talep sağlayıcı uç noktaları ve protokolleri Talep sağlayıcı ile iletişim kurmak için gerekli belirleyen bir veya daha fazla teknik profilleri olması gerekir. 

Bir talep sağlayıcı birden çok teknik profilleri çeşitli nedenleri olabilir. Örneğin, birden çok teknik profilleri Talep sağlayıcı birden çok protokolü destekler, uç noktaları değişen yetenekleri sağlayabilirsiniz ya da sürümlerden güvence düzeyi çeşitli sahip talep içerebilir çünkü tanımlanabilir. Bir kullanıcı gezisine ancak başka bir önemli talep serbest bırakmak için uygun olabilir. 

Aşağıdaki XML parçacığını bir talep sağlayıcı düğümü iki teknik profilleriyle içerir:

* **TechnicalProfile kimliği = "REST API SignUp"**: RESTful hizmetini tanımlar. 
   * `Proprietary`protokol olarak bir RESTful tabanlı sağlayıcısı için tanımlanır. 
   * `InputClaims`Azure AD B2C ' REST hizmeti gönderilecek Talepleri tanımlar. 

   Bu örnekte, talep içeriğini `givenName` REST hizmeti olarak gönderir `firstName`, talep içeriğini `surname` REST hizmeti olarak gönderir `lastName`, ve `email` olarak gönderir. `OutputClaims` Öğesi RESTful hizmetinden Azure AD B2C geri alınır talepleri tanımlar.

* **TechnicalProfile Id="LocalAccountSignUpWithLogonEmail"**: Adds a validation technical profile to an existing technical profile (defined in base policy). Kaydolma seyahat sırasında doğrulama teknik profili önceki teknik profili çağırır. RESTful hizmeti 409 (çakışma hatası) HTTP hata verirse, kullanıcıya hata iletisi görüntülenir. 

Bulun `<ClaimsProviders>` düğümü altında aşağıdaki XML parçacığını ekleyin `<ClaimsProviders>` düğümü:

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
    
    <!-- Custom Restful service -->
    <TechnicalProfile Id="REST-API-SignUp">
        <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
        <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
        <Metadata>
        <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
        <Item Key="AuthenticationType">None</Item>
        <Item Key="SendClaimsIn">Body</Item>
        </Metadata>
        <InputClaims>
        <InputClaim ClaimTypeReferenceId="email" />
        <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
        <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
        </InputClaims>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
    </TechnicalProfile>

<!-- Change LocalAccountSignUpWithLogonEmail technical profile to support your validation technical profile -->
    <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="loyaltyNumber" />
        </OutputClaims>
        <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="REST-API-SignUp" />
        </ValidationTechnicalProfiles>
    </TechnicalProfile>

    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>6. adım: Ekleme `loyaltyNumber` talep uygulamanıza gönderilir şekilde bağlı olan taraf İlkesi dosyanıza talep
Düzenleme, *SignUpOrSignIn.xml* bağlı olan taraf (RP) dosya ve TechnicalProfile kimliği değiştirme aşağıdaki eklemek için "PolicyProfile" öğesi =: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.

Yeni Talep ekledikten sonra bağlı olan taraf kodu şöyle görünür:

```xml
<RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="loyaltyNumber" DefaultValue="" />
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
    </RelyingParty>
</TrustFrameworkPolicy>
```

## <a name="step-7-upload-the-policy-to-your-tenant"></a>7. adım: ilke kiracınız için karşıya yükleme

1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından açın **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Açık **tüm ilkeleri**. 

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. TrustFrameworkExtensions.xml dosyasını karşıya yükleyin ve doğrulama başarılı olduğundan emin olun.

7. SignUpOrSignIn.xml dosyasıyla bir önceki adımı yineleyin.

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a>8. adım: Test Şimdi Çalıştır kullanarak özel İlkesi
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

    > [!NOTE]
    > **Şimdi Çalıştır** Kiracı'preregistered için en az bir uygulama gerekiyor. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.

    ![B2C_1A_signup_signin penceresi](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3. Sınama işlemi yazarak **Test** içinde **verilen ad** kutusu.  
    Azure AD B2C penceresinin en üstünde bir hata iletisi görüntüler.

    ![İlkeniz test](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4.  İçinde **verilen ad** ("Test" dışında) bir ad yazın.  
    Azure AD B2C, kullanıcı oturum açtığında ve uygulamanıza bir loyaltyNumber gönderir. Bu JWT sayısında unutmayın.

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1507125903,
  "nbf": 1507122303,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
  "acr": "b2c_1a_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1507122303,
  "auth_time": 1507122303,
  "loyaltyNumber": "290",
  "given_name": "Emily",
  "emails": ["B2cdemo@outlook.com"]
}
```

## <a name="optional-download-the-complete-policy-files-and-code"></a>(İsteğe bağlı) Kod ve tam ilke dosyaları indirme
* Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).
* Tam koddan indirebilirsiniz [başvurusu için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).
    
## <a name="next-steps"></a>Sonraki adımlar
* [Temel kimlik doğrulaması (kullanıcı adı ve parola), RESTful API'si güvenli](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
* [İstemci sertifikaları, RESTful API'si güvenli](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
