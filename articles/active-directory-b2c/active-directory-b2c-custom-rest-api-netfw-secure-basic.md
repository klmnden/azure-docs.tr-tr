---
title: 'Azure Active Directory B2C: RESTful hizmetlerinizi HTTP temel kimlik doğrulaması kullanarak güvenli hale getirme'
description: Azure AD B2C özel, REST API talep alışverişlerine HTTP temel kimlik doğrulaması kullanarak güvenli hale getirme
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 09/25/2017
ms.author: davidmu
ms.openlocfilehash: 749157d16c1c394b173545dddb8751d58fdcfd56
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="secure-your-restful-services-by-using-http-basic-authentication"></a>HTTP temel kimlik doğrulaması kullanarak RESTful hizmetlerinizi güvenli hale getirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

İçinde bir [ilişkili Azure AD B2C makale](active-directory-b2c-custom-rest-api-netfw.md), Azure Active Directory (Azure AD B2C) kullanıcı Yolculuklar kimlik doğrulaması olmadan B2C ile tümleşen bir RESTful hizmeti (web API) oluşturun. 

Kullanıcıların doğrulandı yalnızca B2C dahil olmak üzere erişebilir, API bu makalede, HTTP temel kimlik doğrulaması RESTful hizmetinize ekleyebilir, böylece. HTTP temel kimlik doğrulaması ile özel ilkeniz (uygulama kimliği ve uygulama gizli anahtarı) kullanıcı kimlik bilgilerini ayarlayın. 

Daha fazla bilgi için bkz: [ASP.NET Web API'de temel kimlik doğrulaması](https://docs.microsoft.com/aspnet/web-api/overview/security/basic-authentication).

## <a name="prerequisites"></a>Önkoşullar
Bölümündeki adımları tamamlamanız [tümleştirmek REST API talep Azure AD B2C kullanıcı Yolculuğunuzun alışverişlerine](active-directory-b2c-custom-rest-api-netfw.md) makalesi.

## <a name="step-1-add-authentication-support"></a>1. adım: kimlik doğrulama desteği ekleme

### <a name="step-11-add-application-settings-to-your-projects-webconfig-file"></a>1.1. adım: uygulama ayarları, projenizin web.config dosyasına ekleyin.
1. Daha önce oluşturduğunuz Visual Studio projesini açın. 

2. Web.config dosyasında altına aşağıdaki uygulama ayarları ekleyin `appSettings` öğe:

    ```XML
    <add key="WebApp:ClientId" value="B2CServiceUserAccount" />
    <add key="WebApp:ClientSecret" value="your secret" />
    ```

3. Bir parola oluşturun ve ardından `WebApp:ClientSecret` değeri.

    Karmaşık bir parola oluşturmak için aşağıdaki PowerShell kodu çalıştırın. Herhangi bir rastgele değer kullanabilirsiniz.

    ```PowerShell
    $bytes = New-Object Byte[] 32
    $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
    $rand.GetBytes($bytes)
    $rand.Dispose()
    [System.Convert]::ToBase64String($bytes)
    ```

### <a name="step-12-install-owin-libraries"></a>1.2. adım: Yükleme OWIN kitaplıkları
Başlamak için Visual Studio Paket Yöneticisi konsolu kullanılarak OWIN ara yazılımı NuGet paketlerini projeye ekleyin:

```
PM> Install-Package Microsoft.Owin
PM> Install-Package Owin
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

### <a name="step-13-add-an-authentication-middleware-class"></a>1.3. adım: bir kimlik doğrulaması ara yazılımı sınıfı ekleme
Ekleme `ClientAuthMiddleware.cs` altında sınıf *App_Start* klasör. Bunu yapmak için:

1. Sağ *App_Start* klasöründe seçin **Ekle**ve ardından **sınıfı**.

   ![App_Start klasöründe ClientAuthMiddleware.cs sınıfı ekleme](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth1.png)

2. İçinde **adı** kutusuna **ClientAuthMiddleware.cs**.

   ![Yeni C# sınıfı oluşturun](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup-auth2.png)

3. Açık *App_Start\ClientAuthMiddleware.cs* dosyasını bulun ve aşağıdaki kod ile içerik dosyasını değiştirin:

    ```csharp
    
    using Microsoft.Owin;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.Linq;
    using System.Security.Principal;
    using System.Text;
    using System.Threading.Tasks;
    using System.Web;
    
    namespace Contoso.AADB2C.API
    {
        /// <summary>
        /// Class to create a custom owin middleware to check for client authentication
        /// </summary>
        public class ClientAuthMiddleware
        {
            private static readonly string ClientID = ConfigurationManager.AppSettings["WebApp:ClientId"];
            private static readonly string ClientSecret = ConfigurationManager.AppSettings["WebApp:ClientSecret"];
    
            /// <summary>
            /// Gets or sets the next owin middleware
            /// </summary>
            private Func<IDictionary<string, object>, Task> Next { get; set; }
    
            /// <summary>
            /// Initializes a new instance of the <see cref="ClientAuthMiddleware"/> class.
            /// </summary>
            /// <param name="next"></param>
            public ClientAuthMiddleware(Func<IDictionary<string, object>, Task> next)
            {
                this.Next = next;
            }
    
            /// <summary>
            /// Invoke client authentication middleware during each request.
            /// </summary>
            /// <param name="environment">Owin environment</param>
            /// <returns></returns>
            public Task Invoke(IDictionary<string, object> environment)
            {
                // Get wrapper class for the environment
                var context = new OwinContext(environment);
    
                // Check whether the authorization header is available. This contains the credentials.
                var authzValue = context.Request.Headers.Get("Authorization");
                if (string.IsNullOrEmpty(authzValue) || !authzValue.StartsWith("Basic ", StringComparison.OrdinalIgnoreCase))
                {
                    // Process next middleware
                    return Next(environment);
                }
    
                // Get credentials
                var creds = authzValue.Substring("Basic ".Length).Trim();
                string clientId;
                string clientSecret;
    
                if (RetrieveCreds(creds, out clientId, out clientSecret))
                {
                    // Set transaction authenticated as client
                    context.Request.User = new GenericPrincipal(new GenericIdentity(clientId, "client"), new string[] { "client" });
                }
    
                return Next(environment);
            }
    
            /// <summary>
            /// Retrieve credentials from header
            /// </summary>
            /// <param name="credentials">Authorization header</param>
            /// <param name="clientId">Client identifier</param>
            /// <param name="clientSecret">Client secret</param>
            /// <returns>True if valid credentials were presented</returns>
            private bool RetrieveCreds(string credentials, out string clientId, out string clientSecret)
            {
                string pair;
                clientId = clientSecret = string.Empty;
    
                try
                {
                    pair = Encoding.UTF8.GetString(Convert.FromBase64String(credentials));
                }
                catch (FormatException)
                {
                    return false;
                }
                catch (ArgumentException)
                {
                    return false;
                }
    
                var ix = pair.IndexOf(':');
                if (ix == -1)
                {
                    return false;
                }
    
                clientId = pair.Substring(0, ix);
                clientSecret = pair.Substring(ix + 1);
    
                // Return whether credentials are valid
                return (string.Compare(clientId, ClientAuthMiddleware.ClientID) == 0 &&
                    string.Compare(clientSecret, ClientAuthMiddleware.ClientSecret) == 0);
            }
        }
    }
    ```

### <a name="step-14-add-an-owin-startup-class"></a>1.4. adım: OWIN başlangıç sınıfı ekleme
Adlı bir OWIN başlangıç sınıfı ekleme `Startup.cs` API. Bunu yapmak için:
1. Projeye sağ tıklayın, **Ekle** > **yeni öğe**, arayın ve sonra **OWIN**.

   ![OWIN başlangıç sınıfı ekleme](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-OWIN-startup.png)

2. Açık *haline* dosyasını bulun ve aşağıdaki kod ile içerik dosyasını değiştirin:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    
    [assembly: OwinStartup(typeof(Contoso.AADB2C.API.Startup))]
    namespace Contoso.AADB2C.API
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                    app.Use<ClientAuthMiddleware>();
            }
        }
    }
    ```

### <a name="step-15-protect-the-identity-api-class"></a>1.5. adım: kimlik API sınıfı koruma
Controllers\IdentityController.cs açın ve eklemek `[Authorize]` ve denetleyici sınıfına etiketi. Bu etiket kimlik doğrulama gereksinimini karşılayan kullanıcılar denetleyiciye erişimi sınırlandırır.

![Denetleyiciye Authorize etiket ekleme](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-authorize.png)

## <a name="step-2-publish-to-azure"></a>2. adım: Azure'a yayımlama
Çözüm Gezgini'nde, projenize yayımlamak için sağ **Contoso.AADB2C.API** proje ve ardından **Yayımla**.

## <a name="step-3-add-the-restful-services-app-id-and-app-secret-to-azure-ad-b2c"></a>3. adım: Azure AD B2C'ye RESTful hizmetlerini uygulama kimliği ve uygulama gizli anahtarı ekleme
RESTful hizmetiniz istemci kimliği (kullanıcı adı) ve gizli anahtarı tarafından korunduktan sonra Azure AD B2C kiracınızda kimlik bilgileri saklamanız gerekir. RESTful hizmetlerinizi istediğinde, özel ilkeniz kimlik bilgilerini sağlar. 

### <a name="step-31-add-a-restful-services-client-id"></a>3.1. adım: bir RESTful hizmetlerini istemci kimliği ekleme
1. Azure AD B2C kiracınızda seçin **B2C ayarlarını** > **kimlik deneyimi Framework**.


2. Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçin **seçenekleri**seçin **el ile**.

5. İçin **adı**, türü **B2cRestClientId**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, daha önce tanımlanan uygulama Kimliğini girin.

7. İçin **anahtar kullanımı**seçin **gizli**.

8. **Oluştur**’u seçin.

9. Oluşturduğunuz olduğunu onaylayın `B2C_1A_B2cRestClientId` anahtarı.

### <a name="step-32-add-a-restful-services-client-secret"></a>3.2. adım: bir RESTful hizmetlerini gizli ekleme
1. Azure AD B2C kiracınızda seçin **B2C ayarlarını** > **kimlik deneyimi Framework**.

2. Seçin **İlkesi anahtarları** kiracınızda kullanılabilir tuşlarını görmek için.

3. **Add (Ekle)** seçeneğini belirleyin.

4. İçin **seçenekleri**seçin **el ile**.

5. İçin **adı**, türü **B2cRestClientSecret**.  
    Önek *B2C_1A_* otomatik olarak eklenebilir.

6. İçinde **gizli** kutusuna, daha önce tanımlanan uygulama gizli anahtarı girin.

7. İçin **anahtar kullanımı**seçin **gizli**.

8. **Oluştur**’u seçin.

9. Oluşturduğunuz olduğunu onaylayın `B2C_1A_B2cRestClientSecret` anahtarı.

## <a name="step-4-change-the-technical-profile-to-support-basic-authentication-in-your-extension-policy"></a>4. adım: temel kimlik doğrulaması uzantısı ilkenizde desteklemek için teknik profilini değiştirme
1. Çalışma dizininizi uzantı ilke dosyası (TrustFrameworkExtensions.xml) açın.

2. Arama `<TechnicalProfile>` içeren düğüm `Id="REST-API-SignUp"`.

3. Bulun `<Metadata>` öğesi.

4. Değişiklik *AuthenticationType* için *temel*aşağıdaki gibi:
    ```xml
    <Item Key="AuthenticationType">Basic</Item>
    ```

5. Kapatma hemen sonra `<Metadata>` öğesi, aşağıdaki XML parçacığını ekleyin: 

    ```xml
    <CryptographicKeys>
        <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
        <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
    </CryptographicKeys>
    ```
    Kod parçacığını ekledikten sonra teknik profiliniz için aşağıdaki XML kodunu gibi görünmelidir:
    
    ![Temel kimlik doğrulaması XML öğeleri ekleme](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-secure-basic-add-1.png)

## <a name="step-5-upload-the-policy-to-your-tenant"></a>5. adım: ilke kiracınız için karşıya yükleme

1. İçinde [Azure portal](https://portal.azure.com), geçiş [bağlamı Azure AD B2C kiracınızın](active-directory-b2c-navigate-to-b2c-context.md)ve ardından açın **Azure AD B2C**.

2. Seçin **kimlik deneyimi Framework**.

3. Açık **tüm ilkeleri**.

4. Seçin **karşıya İlkesi**.

5. Seçin **varsa ilkesi üzerine** onay kutusu.

6. Karşıya yükleme *TrustFrameworkExtensions.xml* dosya ve ardından doğrulama başarılı olduğundan emin olun.

## <a name="step-6-test-the-custom-policy-by-using-run-now"></a>6. adım: Test Şimdi Çalıştır kullanarak özel İlkesi
1. Açık **Azure AD B2C ayarlarını**ve ardından **kimlik deneyimi Framework**.

    >[!NOTE]
    >Çalışma şimdi Kiracı'preregistered için en az bir uygulama için gereklidir. Uygulamaları kaydetmek öğrenmek için Azure AD B2C bkz [başlama](active-directory-b2c-get-started.md) makale veya [uygulama kaydı](active-directory-b2c-app-registration.md) makale.

2. Açık **B2C_1A_signup_signin**, karşıya ve ardından bağlı olan taraf (RP) özel ilkesini **Şimdi Çalıştır**.

3. Sınama işlemi yazarak **Test** içinde **verilen ad** kutusu.  
    Azure AD B2C penceresinin en üstünde bir hata iletisi görüntüler.

    ![Kimliğinizi API testi](media/aadb2c-ief-rest-api-netfw-secure-basic/rest-api-netfw-test.png)

4. İçinde **verilen ad** ("Test" dışında) bir ad yazın.  
    Azure AD B2C, kullanıcı oturum açtığında ve ardından uygulamanızı bağlılık birkaç gönderir. Bu örnekte numarasını not edin:

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
* Tamamlandıktan sonra [özel ilkelerini kullanmaya başlama](active-directory-b2c-get-started-custom.md) gözden geçirme, öneririz, kendi özel ilke dosyalarını kullanarak senaryonuz derleme. Başvuru için sağladık [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-basic).
* Tam koddan indirebilirsiniz [başvurusu için örnek Visual Studio çözümü](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API).

## <a name="next-steps"></a>Sonraki adımlar
* [İstemci sertifikaları, RESTful API'si güvenliğini sağlamak için kullanın](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)

