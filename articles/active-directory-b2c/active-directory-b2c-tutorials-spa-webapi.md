---
title: Öğretici - Azure Active Directory B2C kullanarak tek sayfalı bir uygulamadan ASP.NET Core web API'sine erişim izni verme| Microsoft Docs
description: Bir .NET Core web API’sini korumak ve tek sayfalı bir uygulamadan çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: davidmu1
ms.author: davidmu
ms.date: 3/02/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: 0e9e3074e2cdd9ec3adc814779811d150cd11010
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-app-using-azure-active-directory-b2c"></a>Öğretici - Azure Active Directory B2C kullanarak tek sayfalı bir uygulamadan ASP.NET Core web API'sine erişim izni verme

Bu öğreticide tek sayfalı bir uygulamadan Azure Active Directory (Azure AD) B2C korumalı ASP.NET Core web API’si kaynağını çağırma işlemi gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza web API’si kaydetme
> * Bir web API’si için kapsamları tanımlama ve yapılandırma
> * Web API’sine uygulama izinleri verme
> * Kaynak kodu bir web API’sini korumak için Azure AD B2C kullanmak üzere güncelleştirme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Tek sayfalı bir uygulamada Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisini](active-directory-b2c-tutorials-spa.md) tamamlayın.
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.
* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) veya üzeri
* [Node.js](https://nodejs.org/en/download/)’yi yükleme

## <a name="register-web-api"></a>Web API’si kaydetme

Web API’si kaynaklarının Azure Active Directory’den bir [erişim belirteci](../active-directory/develop/active-directory-dev-glossary.md#access-token) sunan [istemci uygulamalar](../active-directory/develop/active-directory-dev-glossary.md#client-application) tarafından [korunan kaynak isteklerini](../active-directory/develop/active-directory-dev-glossary.md#resource-server) kabul etmesi ve yanıtlaması için kiracınızda kayıtlı olmaları gerekir. Kayıt, kiracınızda [uygulama ve hizmet sorumlusu nesnesini](../active-directory/develop/active-directory-dev-glossary.md#application-object) belirler. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin.

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın.

    Örnek web API’sini kiracınıza kaydetmek için aşağıdaki ayarları kullanın.
    
    ![Yeni bir API ekleme](media/active-directory-b2c-tutorials-spa-webapi/web-api-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Hello Core API’si | Web API’nizi geliştiricilere tanıtan bir **Ad** girin. |
    | **Web uygulamasını / web API'sini dahil etme** | Yes | Web API’si için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Yes | API [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `http://localhost:44332` | Yanıt URL'leri, Azure AD B2C'nin, API’niz tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek web API’si yerel olarak (localhost) çalışır ve 5000 numaralı bağlantı noktasını dinler. |
    | **Uygulama Kimliği URI'si** | HelloCoreAPI | URI, kiracıdaki API’yi benzersiz olarak tanımlar. Bu, kiracı başına birden çok API kaydetmenize olanak sağlar. [Kapsamlar](../active-directory/develop/active-directory-dev-glossary.md#scopes) korumalı API kaynağına erişimi yönetir ve Uygulama Kimliği URI’si başına tanımlanır. |
    | **Yerel istemci** | Hayır | Bu, bir web API’si olduğu için ve yerel bir istemci olmadığı için Hayır’ı seçin. |
    
3. API’nizi kaydetmek için **Oluştur** seçeneğine tıklayın.

Kayıtlı API’ler Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web API’nizi seçin. Web API’sinin özellik bölmesi görüntülenir.

![Web API’si özellikleri](./media/active-directory-b2c-tutorials-spa-webapi/b2c-web-api-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, API’yi benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde API’yi yapılandırırken gerekir.

Web API’nizi Azure AD B2C ile kaydetmek bir güven ilişkisini tanımlar. API B2C ile kaydedildiğinden, API artık diğer uygulamalardan aldığı B2C erişim belirteçlerine güvenebilir.

## <a name="define-and-configure-scopes"></a>Kapsamları tanımlama ve yapılandırma

[Kapsamlar](../active-directory/develop/active-directory-dev-glossary.md#scopes) korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma izinlerini tanımlayacaksınız.

### <a name="define-scopes-for-the-web-api"></a>Web API’si için kapsamları tanımlama

Kayıtlı API’ler Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web API’nizi seçin. Web API’sinin özellik bölmesi görüntülenir.

**Yayımlanan kapsamlar (Önizleme)** seçeneğine tıklayın.

API için kapsamları tanımlamak için, aşağıdaki girişleri ekleyin. 

![web API’sinde tanımlanan kapsamlar](media/active-directory-b2c-tutorials-spa-webapi/scopes-web-api.png)

| Ayar      | Önerilen değer  | Açıklama                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Kapsam** | demo.read | Demo API’ye okuma erişimi |

**Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

### <a name="grant-app-permissions-to-web-api"></a>Web API’sine uygulama izinleri verme

Bir uygulamadan korumalı web API’sini çağırmak için, API’ye uygulama izinleri vermeniz gerekir. Bu öğreticide, [Tek sayfalı bir uygulamada (Javascript) Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](active-directory-b2c-tutorials-spa.md) oluşturduğunuz tek sayfalı uygulamayı kullanın.

1. Azure portalında, hizmetler listesinden **Azure AD B2C**’yi seçin ve kayıtlı uygulama listesini görmek için **Uygulamalar**’a tıklayın.

2. Uygulama listesinden **Örnek tek sayfalı uygulamam**’ı seçip **API erişimi (Önizleme)** ve ardından **Ekle**’ye tıklayın.

3. **API seçin** açılır menüsünde, kayıtlı **Hello Core API** adlı web API’nizi seçin.

4. **Kapsamları seçin** açılır menüsünde, web API’si kaydında tanımladığınız kapsamları seçin.

    ![uygulama için kapsamları seçme](media/active-directory-b2c-tutorials-spa-webapi/selecting-scopes-for-app.png)

5. **Tamam**’a tıklayın.

**Örnek tek sayfalı uygulamam**, korumalı **Hello Core API**’sini çağırmak için kaydedilir. Kullanıcılar WPF masaüstü uygulamasını kullanmak için Azure AD B2C ile [kimlik doğrulaması yapar](../active-directory/develop/active-directory-dev-glossary.md#authentication). Masaüstü uygulaması, korumalı web API’sine erişmek için Azure AD B2C’den bir [yetkilendirme izni](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) alır.

## <a name="update-code"></a>Kodu güncelleştirme

API’yi kaydettikten ve kapsamları tanımladıktan sonra, web API kodunuzu Azure AD B2C kiracınızı kullanmak için yapılandırmanız gerekir. Bu öğreticide, GitHub’dan indirebileceğiniz örnek bir .NET Core web uygulaması yapılandıracaksınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi.git
```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. **B2C-WebAPI.sln** çözümünü Visual Studio’da açın.

2. **appsettings.json** dosyasını açın. Web api’sini kiracınızı kullanacak şekilde yapılandırmak için aşağıdaki değerleri güncelleştirin:

    ```javascript
    "AzureAdB2C": 
      {
        "Tenant": "<your tenant name>.onmicrosoft.com", 
        "ClientId": "<The Application ID for your web API obtained from the Azure portal>",
        "Policy": "<Your sign up sign in policy e.g. B2C_1_SiUpIn>",
        "ScopeRead": "demo.read"  
      },
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın ASP.NET Core web API'sini çağırmasına izin vermek için [CORS](https://docs.microsoft.com/aspnet/core/security/cors) özelliğini etkinleştirmeniz gerekir.

1. **Startup.cs** dosyasında `ConfigureServices()` yöntemine CORS ekleyin.

    ```C#
    public void ConfigureServices(IServiceCollection services) {
      services.AddCors();
    ```

2. **Startup.cs** dosyasında, `Configure()` yönteminde CORS’yi yapılandırın.

    ```C#
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory) {
      app.UseCors(builder =>
        builder.WithOrigins("http://localhost:6420").AllowAnyHeader().AllowAnyMethod());
    ```

### <a name="configure-the-single-page-app"></a>Tek sayfalı uygulamayı yapılandırma

Tek sayfalı uygulama, kullanıcının kaydolma ve oturum açma işlemleri için Azure Ad B2C kullanır ve korumalı ASP.NET Core web API’sini çağırır. .NET Core web api’sini çağırmak için tek sayfalı uygulama çağrısını güncelleştirmeniz gerekir.
Uygulama ayarlarını değiştirmek için:

1. Node.js tek sayfalı uygulama örneğinde `index.html` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarında **b2cScopes** ve **webApi** değerlerini değiştirin:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<Application ID for your SPA obtained from portal app registration>',
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_SiUpIn",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/HelloCoreAPI/demo.read"],
        webApi: 'http://localhost:58553/api/values',
    };
    ```

## <a name="run-the-spa-app-and-web-api"></a>SPA uygulamasını ve web API’sini çalıştırma

Hem Node.js tek sayfa uygulamasını hem de .NET Core web API’sini çalıştırmanız gerekir.

### <a name="run-the-aspnet-core-web-api"></a>ASP.NET Core Web API’sini çalıştırma 

**F5**’e basarak Visual Studio’da **B2C-WebAPI.sln** çözümünün hatalarını ayıklayın.

Proje başlatıldığında varsayılan tarayıcınızda web api’sinin istekler için kullanılabilir olduğunu duyuran bir web sayfası gösterilir.

### <a name="run-the-single-page-app"></a>Tek sayfalı uygulamayı çalıştırma

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-msal-singlepageapp`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    node server.js
    ```

    Konsol penceresi uygulamanın barındırıldığı bağlantı noktası numarasını görüntüler.
    
    ```
    Listening on port 6420...
    ```

4. Bir tarayıcı kullanarak uygulamayı görüntülemek için `http://localhost:6420` adresine gidin.
5. [Tek sayfalı bir uygulamada Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme (Javascript)](active-directory-b2c-tutorials-spa.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
6. **API’yı Çağır** düğmesine tıklayın.

Kaydolduktan veya bir kullanıcı hesabıyla oturum açtıktan sonra örnek, korumalı web api’sini çağırır ve bir sonuç döndürür.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure AD B2C’de kapsamları kaydedip tanımlayarak bir web API’sini koruma adımlarını öğrendiniz. Kullanılabilir Azure AD B2C kod örneklerine giderek daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure AD B2C kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)
