---
title: Azure Active Directory B2C kullanıcı yolculuğunuzda REST API talep değişimleri tümleştirme | Microsoft Docs
description: Kullanıcı girişini doğrulama, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/30/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 466d5eff27d9a8105fb840ce4ba79571b6207092
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835509"
---
# <a name="integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-of-user-input"></a>Kullanıcı girişini doğrulama, Azure AD B2C kullanıcı yolculuğunun talep alışverişlerine REST API tümleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Kimlik deneyimi çerçevesi ile hangi altını Azure Active Directory B2C'yi (Azure AD B2C) tümleştirebilmenizi kullanıcı yolculuğu bir RESTful API ile. Bu kılavuzda, Azure AD B2C'yi .NET Framework RESTful Hizmetleri (web API) ile nasıl etkileştiğini öğreneceksiniz.

## <a name="introduction"></a>Giriş
Azure AD B2C'yi kullanarak kendi RESTful hizmeti çağırarak kullanıcı yolculuğu için kendi iş mantığı ekleyebilirsiniz. Kimlik deneyimi çerçevesi RESTful hizmeti için veri gönderen bir *giriş talep* koleksiyonu ve verileri RESTful alanından geri alan bir *çıkış talep* koleksiyonu. RESTful hizmeti Tümleştirmesi ile şunları yapabilirsiniz:

* **Kullanıcı giriş verilerini doğrulamak**: Bu eylem, Azure AD ile kalıcı hatalı oluşturulmuş veri engeller. Kullanıcıdan değer geçerli değilse, RESTful hizmetiniz bir giriş sağlamaya yönlendiren bir hata iletisi döndürür. Örneğin, kullanıcı tarafından sağlanan e-posta adresi müşterinizin veritabanında bulunduğunu doğrulayın.
* **Giriş talep üzerine**: Örneğin, bir kullanıcının tüm küçük adı veya tümüyle büyük harfe girerse, adı büyük harfle yalnızca ilk harfi ile biçimlendirebilirsiniz.
* **Kullanıcı verilerini daha fazla Kurumsal satır iş kolu uygulamaları ile tümleştirerek zenginleştirin**: RESTful hizmetinizi kullanıcının e-posta adresi alın, müşteri veritabanını sorgulama ve kullanıcının bağlılık numaranız Azure AD B2C'ye. İade talepleri depolanabilir sonraki değerlendirilir, kullanıcının Azure AD hesabı *düzenleme adımlarının*, veya erişim belirtecinde yer.
* **Özel iş mantığı çalıştırma**: Anında iletme bildirimleri göndermek, Kurumsal veritabanları güncelleştirebilir, bir kullanıcı geçiş işlemi çalıştırın, izinlerini yönetmek, veritabanları denetim ve başka işlemler gerçekleştirme.

RESTful Hizmetleri ile tümleştirme aşağıdaki yollarla tasarlayabilmek için:

* **Doğrulama teknik profili**: RESTful hizmetine yapılan çağrı belirtilen teknik profili doğrulama teknik profili içinde gerçekleşir. Kullanıcı yolculuğu İleri taşınmadan önce doğrulama teknik profili, kullanıcı tarafından sağlanan veri doğrular. Doğrulama teknik profili ile şunları yapabilirsiniz:
   * Giriş talepler göndermek.
   * Giriş talepleri doğrulamak ve özel hata iletileri görüntülemeyi tercih edebilirsiniz.
   * Geri çıkış talepler göndermek.

* **Talep değişimi**: Bu tasarım doğrulama teknik profiline benzer, ancak bir düzenleme adımı içinde gerçekleşir. Bu tanım için sınırlıdır:
   * Giriş talepler göndermek.
   * Geri çıkış talepler göndermek.

## <a name="restful-walkthrough"></a>RESTful gözden geçirme
Bu kılavuzda, .NET Framework web API'si, kullanıcı girişini doğrulayan ve kullanıcı bağlılık numarasını sağlayan geliştirin. Örneğin, uygulamanızın erişimi verebilir *Platin avantajları* bağlılık sayısına göre.

Genel Bakış:
* RESTful hizmeti (.NET Framework web API'si) geliştirin.
* RESTful hizmeti kullanıcı yolculuğunda kullanın.
* Giriş talepleri göndermek ve bunları kodunuzda okuyun.
* Kullanıcının ilk adını doğrulayın.
* Bir bağlılık sayı geri gönderin.
* Bağlılık numarası bir JSON Web Token (JWT için) ekleyin.

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) makalesi.

## <a name="step-1-create-an-aspnet-web-api"></a>1\. adım: Bir ASP.NET web API'si oluşturma

1. Visual Studio'da seçerek bir proje oluşturun **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje** penceresinde **Visual C#**  > **Web** > **ASP.NET Web uygulaması (.NET Framework)** .

3. İçinde **adı** uygulama için bir ad yazın (örneğin, *Contoso.AADB2C.API*) ve ardından **Tamam**.

    ![Visual Studio'da yeni bir Visual Studio projesi oluşturma](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-create-project.png)

4. İçinde **yeni ASP.NET Web uygulaması** penceresinde bir **Web API** veya **Azure API uygulaması** şablonu.

    ![Visual Studio'da bir web API şablonu seçme](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-select-web-api.png)

5. Emin olun, kimlik doğrulamasının ayarlandığında **kimlik doğrulaması yok**.

6. Seçin **Tamam** projeyi oluşturmak için.

## <a name="step-2-prepare-the-rest-api-endpoint"></a>2\. adım: REST API uç noktası hazırlama

### <a name="step-21-add-data-models"></a>2\.1. adım: Veri modelleri ekleme
Modelleri giriş talepleri temsil eder ve veri RESTful hizmetinizdeki çıkış talep. Kodunuz, bir C# nesnesi (modeli) için bir JSON dizesi giriş talepleri modelden seri durumdan çıkarılırken girdi verilerini okur. ASP.NET web API, çıkış talep modeline geri JSON otomatik olarak çıkarır ve ardından HTTP yanıt iletisinin gövdesine seri hale getirilmiş verileri yazar.

Aşağıdakileri yaparak giriş talepleri temsil eden bir model oluşturun:

1. Çözüm Gezgini açık değilse, seçin **görünümü** > **Çözüm Gezgini**.
2. Çözüm Gezgini'nde **Modeller** klasörüne sağ tıklayın, **Ekle**'yi ve ardından **Sınıf**'ı seçin.

    ![Model ekleme](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-model.png)

3. Sınıf adı `InputClaimsModel`ve ardından aşağıdaki özelliği ekleyin `InputClaimsModel` sınıfı:

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

4. Yeni bir model oluşturmak `OutputClaimsModel`ve ardından aşağıdaki özelliği ekleyin `OutputClaimsModel` sınıfı:

    ```csharp
    namespace Contoso.AADB2C.API.Models
    {
        public class OutputClaimsModel
        {
            public string loyaltyNumber { get; set; }
        }
    }
    ```

5. Daha fazla bir model oluşturma `B2CResponseContent`, giriş doğrulama hatası iletileri oluşturmak için kullanın. Aşağıdaki özellikleri `B2CResponseContent` sınıfı, eksik başvurular sağlayın ve ardından dosyayı kaydedin:

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

### <a name="step-22-add-a-controller"></a>2\.2. adım: Denetleyici ekleme
Web API'si, bir _denetleyicisi_ HTTP isteklerini işleyen bir nesnedir. Denetleyici, çıkış talep veya ad geçerli değil. bir çakışma HTTP hata iletisini oluşturur döndürür.

1. Çözüm Gezgini'nde **Denetleyiciler** klasörüne sağ tıklayın, **Ekle**'yi ve ardından **Denetleyici**'yi seçin.

    ![Visual Studio'da yeni bir denetleyici ekleme](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-1.png)

2. İçinde **İskele Ekle** penceresinde **Web API denetleyicisi – boş**ve ardından **Ekle**.

    ![Denetleyici - boş Visual Studio'da Web API 2 seçme](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-2.png)

3. İçinde **denetleyici Ekle** penceresi, denetleyici adı **IdentityController**ve ardından **Ekle**.

    ![Girme Visual Studio'da Denetleyici adı](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-add-controller-3.png)

    Yapı iskelesi adlı bir dosya oluşturur *IdentityController.cs* içinde *denetleyicileri* klasör.

4. Varsa *IdentityController.cs* dosya açık değilse zaten çift tıklayın ve ardından dosyasındaki kodu aşağıdaki kodla değiştirin:

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

## <a name="step-3-publish-the-project-to-azure"></a>3\. adım: Projeyi Azure'da yayımlama
1. Çözüm Gezgini'nde sağ **Contoso.AADB2C.API** proje ve ardından **Yayımla**.

    ![Visual Studio ile Microsoft Azure App Service'e yayımlama](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-1.png)

2. İçinde **Yayımla** penceresinde **Microsoft Azure App Service**ve ardından **Yayımla**.

    ![Visual Studio ile yeni Microsoft Azure App Service oluşturma](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-2.png)

    **App Service Oluştur** penceresi açılır. Öğreticide, ASP.NET web uygulamasını Azure'da çalıştırmak için tüm gerekli Azure kaynakları oluşturun.

    > [!NOTE]
    >Yayımlama hakkında daha fazla bilgi için bkz. [Azure'da bir ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

3. İçinde **Web uygulaması adı** benzersiz bir uygulama adı yazın (geçerli karakterler: a-z, 0-9 ve kısa çizgi (-). Web uygulamasının URL'si olan http://<app_name>.azurewebsites.NET, burada *app_name* web uygulamanızın adıdır. Otomatik oluşturulmuş benzersiz adı kabul edebilirsiniz.

    ![App Service özelliklerini yapılandırma](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-publish-to-azure-3.png)

4. Azure kaynaklarını oluşturmaya başlamak için seçim **Oluştur**.
    ASP.NET web uygulaması oluşturduktan sonra sihirbaz Azure'da yayımlar ve ardından uygulamayı varsayılan tarayıcıda başlatır.

6. Web uygulamasının URL'sini kopyalayın.

## <a name="step-4-add-the-new-loyaltynumber-claim-to-the-schema-of-your-trustframeworkextensionsxml-file"></a>4\. Adım: Yeni Ekle `loyaltyNumber` TrustFrameworkExtensions.xml dosyanızın şemaya talep
`loyaltyNumber` Talep bizim şemasında henüz tanımlanmadı. İçinde bir tanım ekleyin `<BuildingBlocks>` başında bulabileceğiniz öğesi *TrustFrameworkExtensions.xml* dosya.

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

## <a name="step-5-add-a-claims-provider"></a>5\. Adım: Bir talep Sağlayıcı Ekle
Her talep sağlayıcısı uç noktaları ve protokolleri ve Talep sağlayıcı ile iletişim kurmak için gereken belirleyen bir veya daha fazla teknik profiller, olması gerekir.

Bir talep sağlayıcısı, birden fazla teknik profili çeşitli nedenlerden dolayı olabilir. Örneğin, birden fazla teknik profili, çünkü birden çok protokol talep sağlayıcının desteklediği, çeşitli özellikleri uç noktalarına sahip olabilir veya sürümler güvence düzeyleri çeşitli sahip talep içerebilir tanımlanabilir. Bir kullanıcı yolculuğu ancak başka bir hassas talep serbest bırakmak için kabul edilebilir olabilir.

Aşağıdaki XML kod parçacığı, iki teknik profili ile bir talep sağlayıcı düğüm içerir:

* **TechnicalProfile Id="REST-API-SignUp"** : RESTful hizmetini tanımlar.
  * `Proprietary` bir RESTful tabanlı sağlayıcısı için protokol olarak tanımlanır.
  * `InputClaims` Azure AD B2C'den REST hizmeti için gönderilecek Talepleri tanımlar.

    Bu örnekte, talep içeriğini `givenName` REST hizmeti gönderir `firstName`, talep içeriğini `surname` REST hizmeti gönderir `lastName`, ve `email` olarak gönderir. `OutputClaims` Öğesi, Azure AD B2C geri RESTful hizmetinden alınan talepleri tanımlar.

* **TechnicalProfile kimliği = "LocalAccountSignUpWithLogonEmail"** : Doğrulama teknik profili, var olan bir teknik profilini (temel ilkede tanımlanan) ekler. Kaydolma yolculuğu teknik profil doğrulama teknik profili çağırır. RESTful hizmeti bir HTTP hatası 409 (çakışma hatası) döndürürse, kullanıcıya hata iletisi görüntülenir.

Bulun `<ClaimsProviders>` düğümünü ve ardından altına aşağıdaki XML parçacığını ekleyin `<ClaimsProviders>` düğüm:

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
        <Item Key="AllowInsecureAuthInProduction">true</Item>
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

## <a name="step-6-add-the-loyaltynumber-claim-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a>6\. Adım: Ekleme `loyaltyNumber` talep uygulamanıza gönderilir. Bu nedenle, bağlı olan taraf ilke dosyanıza talep
Düzenleme, *SignUpOrSignIn.xml* bağlı olan taraf (RP) dosya ve TechnicalProfile kimliğini değiştirme = "PolicyProfile" öğesini aşağıdakileri ekleyin: `<OutputClaim ClaimTypeReferenceId="loyaltyNumber" />`.

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

## <a name="step-7-upload-the-policy-to-your-tenant"></a>7\. Adım: Kiracınız için ilkeyi karşıya yükle

1. İçinde [Azure portalında](https://portal.azure.com), geçiş [Azure AD B2C kiracınızın bağlamında](active-directory-b2c-navigate-to-b2c-context.md)ve ardından açın **Azure AD B2C**.

2. Seçin **kimlik deneyimi çerçevesi**.

3. Açık **tüm ilkeleri**.

4. Seçin **karşıya yükleme İlkesi**.

5. Seçin **ilke varsa üzerine** onay kutusu.

6. TrustFrameworkExtensions.xml dosyasını karşıya yükleme ve doğrulama başarılı olun.

7. SignUpOrSignIn.xml dosyasıyla önceki adımı yineleyin.

## <a name="step-8-test-the-custom-policy-by-using-run-now"></a>8\. adım: Şimdi Çalıştır'i kullanarak özel bir ilkeyi test
1. Seçin **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi çerçevesi**.

    > [!NOTE]
    > **Şimdi Çalıştır** en az bir uygulamanın Kiracı'da önceden kayıtlı gerekir. Azure AD B2C uygulamaları kaydetme hakkında bilgi için bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makalesi.

2. Açık **B2C_1A_signup_signin**, yüklenmiş ve ardından bağlı olan taraf (RP) özel ilke **Şimdi Çalıştır**.

    ![Azure portalında B2C_1A_signup_signin özel ilke sayfası](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-run.png)

3. İşlem yazarak test edin **Test** içinde **verilen ad** kutusu.
    Azure AD B2C, pencerenin en üstünde bir hata iletisi görüntüler.

    ![Oturum açma, kaydolma sayfasında belirtilen ada giriş doğrulama sınaması](media/aadb2c-ief-rest-api-netfw/aadb2c-ief-rest-api-netfw-test.png)

4. İçinde **verilen ad** ("Test" dışında) bir ad yazın.
    Azure AD B2C kullanıcı oturum açtığında ve ardından uygulamanızı bir loyaltyNumber gönderir. Bu JWT sayısında unutmayın.

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
}.{
  "exp": 1507125903,
  "nbf": 1507122303,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
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

## <a name="optional-download-the-complete-policy-files-and-code"></a>(İsteğe bağlı) Tüm ilke dosyaları ve kodu indirin
* Tamamladıktan sonra [özel ilkeleri kullanmaya başlama](active-directory-b2c-get-started-custom.md) izlenecek yol, öneririz senaryonuz kendi özel ilke dosyalarını kullanarak oluşturun. Referans olması açısından sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw).
* Tam koddan indirebileceğiniz [başvuru için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/).

## <a name="next-steps"></a>Sonraki adımlar
* [(Kullanıcı adı ve parola) temel kimlik doğrulaması ile RESTful API'NİZİN güvenliğini sağlayın](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
* [İstemci sertifikaları ile RESTful API'NİZİN güvenliğini sağlayın](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
