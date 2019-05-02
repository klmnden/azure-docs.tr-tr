---
title: Öğretici - erişim izni verme bir ASP.NET Core web API'si tek sayfalı bir uygulamadan - Azure Active Directory B2C | Microsoft Docs
description: Bir .NET Core web API’sini korumak ve tek sayfalı bir uygulamadan çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 13fedae2798311a59a5cee2805ce9e09b1bd5a0f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724679"
---
# <a name="tutorial-grant-access-to-an-aspnet-core-web-api-from-a-single-page-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir tek sayfa uygulamasında erişime bir ASP.NET Core Web API'si

Bu öğretici için Azure Active Directory (Azure AD) B2C korumalı ASP.NET Core web API kaynağını çağırma tek sayfalı bir uygulamadan gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar ve adımlar tamamlamak [Öğreticisi: Azure Active Directory B2C kullanarak hesaplarla tek sayfalı uygulama kimlik doğrulamasını etkinleştirme](active-directory-b2c-tutorials-spa.md).

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API'si kaynaklarına kabul edebilir ve korunan kaynak isteklerini yanıtlamak önce kiracınızda bir erişim belirteci mevcut istemci uygulamalar tarafından kayıtlı olması gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
7. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Bu öğreticide örnek yerel olarak çalışır ve adresindeki dinler `https://localhost:5000`.
8. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**’a tıklayın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.

## <a name="configure-scopes"></a>Kapsamlarını yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma izinlerini tanımlayacaksınız.

1. Seçin **uygulamaları**ve ardından *webapi1*.
2. Seçin **yayımlanan kapsamlar**.
3. İçin **kapsam**, girin `Hello.Read`ve açıklama yazın `Read access to hello`.
4. İçin **kapsam**, girin `Hello.Write`ve açıklama yazın `Write access to hello`.
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için API'ye uygulama izinleri vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C'adlı bir web uygulamasında oluşturulan *webapp1*. Web API'sini çağırmak için bu uygulamayı kullanın.

1. Seçin **uygulamaları**ve ardından *webapp1*.
2. Seçin **API erişimi**ve ardından **Ekle**.
3. İçinde **API seçin** açılır menüsünde, select *webapi1*.
4. İçinde **kapsamları seçin** açılır menüsünde, select **Hello.Read** ve **Hello.Write** daha önce tanımladığınız kapsamları.
5. **Tamam** düğmesine tıklayın.

**Örnek tek sayfalı uygulamam**, korumalı **Hello Core API**’sini çağırmak için kaydedilir. Bir kullanıcı, tek sayfalı uygulamayı kullanmak için Azure AD B2C ile kimliğini doğrular. Tek sayfalı uygulamayı bir yetkilendirme izni korumalı web API'sine erişmek için Azure AD B2C alır.

## <a name="configure-the-sample"></a>Örnek yapılandırma

API'yi kaydettikten ve kapsamları tanımladıktan sonra web API kodunda Azure AD B2C kiracınızı kullanacak şekilde yapılandırın. Bu öğreticide, Github'dan indirebileceğiniz örnek .NET Core web uygulamasını yapılandırın. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

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
        "ClientId": "<application-ID>",
        "Policy": "B2C_1_signupsignin1>",
        "ScopeRead": "Hello.Read"  
      },
    ```

#### <a name="enable-cors"></a>CORS'yi etkinleştirme

Tek sayfalı uygulamanızın ASP.NET Core web API'sini çağırmasına izin vermek için etkinleştirmeniz gerekir [CORS](https://docs.microsoft.com/aspnet/core/security/cors).

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

3. **Özellikler** bölümünden **launchSettings.json** dosyasını açın, **iisSettings** *applicationURL* ayarını bulun ve bağlantı noktası numarasını, API Yanıtlama URL’si `http://localhost:5000` için kayıtlı numaraya ayarlayın.

### <a name="configure-the-single-page-application"></a>Tek sayfalı uygulamayı yapılandırma

Tek sayfalı uygulama kaydolma, oturum açma kullanıcı için Azure AD B2C kullanır ve korumalı ASP.NET Core web API'sini çağırır. .NET Core web API'sini çağırmak için tek sayfalı uygulama güncelle

Uygulama ayarlarını değiştirmek için:

1. `index.html` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kodda, kiracı adınızı **b2cScopes** bölümüne ekleyin ve **webApi** değerini daha önce kaydetmiş olduğunuz *applicationURL* değeriyle değiştirin:

    ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    var applicationConfig = {
        clientID: '<application-ID>',
        authority: "https://<your-tenant-name>.b2clogin.com/tfp/<your-tenant-name>.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://<Your tenant name>.onmicrosoft.com/api/Hello.Read"],
        webApi: 'http://localhost:5000/api/values',
    };
    ```

## <a name="run-the-spa-application-and-web-api"></a>SPA uygulamasını ve web API'sini çalıştırma

Hem Node.js tek sayfalı uygulama hem de .NET Core web API'sini çalıştırmanız gerekir.

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

4. Adresine gidin, bir tarayıcı kullanın `http://localhost:6420` uygulamayı görüntülemek için.
5. [Tek sayfalı bir uygulamada Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme (Javascript)](active-directory-b2c-tutorials-spa.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
6. Tıklayın **API çağrısı**.

Kaydolduktan veya bir kullanıcı hesabıyla oturum açtıktan sonra örnek, korumalı web api’sini çağırır ve bir sonuç döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin](tutorial-add-identity-providers.md)
