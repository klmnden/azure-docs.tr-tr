---
title: Öğretici - ASP.NET web API'si için erişim verme - Azure Active Directory B2C | Microsoft Docs
description: Bir ASP.NET web API'sini korumak ve bir ASP.NET web uygulamasından çağırmak için Active Directory B2C kullanma Öğreticisi.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 77e3eaeffba862c727e021427e5f27967fcf35bd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687990"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>Öğretici: Bir ASP.NET web Azure Active Directory B2C kullanarak API erişim izni verme

Bu öğreticide, bir korumalı web API'si kaynağına Azure Active Directory (Azure AD) B2C'de bir ASP.NET web uygulamasından çağırmak nasıl gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Önkoşullar ve adımlar tamamlamak [Öğreticisi: Etkinleştirme kimlik doğrulaması, Azure Active Directory B2C kullanarak bir web uygulamasında](active-directory-b2c-tutorials-web-app.md).

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API'si kaynaklarına kabul edebilir ve korunan kaynak isteklerini yanıtlamak önce kiracınızda bir erişim belirteci mevcut istemci uygulamalar tarafından kayıtlı olması gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
4. Seçin **uygulamaları**ve ardından **Ekle**.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. İçin **içeren web uygulaması / web API'sini** ve **örtük akışa izin ver**seçin **Evet**.
7. İçin **yanıt URL'si**, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü bir uç noktasını girin. Bu öğreticide örnek yerel olarak çalışır ve adresindeki dinler `https://localhost:44332`.
8. İçin **uygulama kimliği URI'si**, web API'niz için kullanılan tanımlayıcı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**’a tıklayın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.

## <a name="configure-scopes"></a>Kapsamlarını yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

1. Seçin **uygulamaları**ve ardından *webapi1*.
2. Seçin **yayımlanan kapsamlar**.
3. İçin **kapsam**, girin `Hello.Read`ve açıklama yazın `Read access to hello`.
4. İçin **kapsam**, girin `Hello.Write`ve açıklama yazın `Write access to hello`.
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar istemci vermek için kullanılan Web API'sine uygulama izni.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için API'ye uygulama izinleri vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C'adlı bir web uygulamasında oluşturulan *webapp1*. Web API'sini çağırmak için bu uygulamayı kullanın.

1. Seçin **uygulamaları**ve ardından *webapp1*.
2. Seçin **API erişimi**ve ardından **Ekle**.
3. İçinde **API seçin** açılır menüsünde, select *webapi1*.
4. İçinde **kapsamları seçin** açılır menüsünde, select **Hello.Read** ve **Hello.Write** daha önce tanımladığınız kapsamları.
5. **Tamam** düğmesine tıklayın.

Uygulamanız korumalı web API'sini çağırmak için kaydedilir. Bir kullanıcının uygulamayı kullanmak için Azure AD B2C ile kimliğini doğrular. Uygulama, korumalı web API'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örnek yapılandırma

API'yi kaydettikten ve kapsamları tanımladıktan sonra web API'si Azure AD B2C kiracınızı kullanacak şekilde yapılandırın. Bu öğreticide örnek bir web API’sini yapılandıracaksınız. Örnek web API'si, önkoşul öğreticisinde indirdiğiniz projede dahildir.

Örnek çözümde iki proje vardır:

Örnek çözümde aşağıdaki iki projeler şunlardır:

- **TaskWebApp** - oluşturma ve görev listesini düzenleyin. Örnek kullanır **kaydolma veya oturum açma** kaydolma veya kullanıcılarının oturumunu kullanıcı akışı.
- **TaskService** - destekleyen oluşturma, okuma, güncelleştirme ve silme görev listesi işlevi. API'si Azure AD B2C tarafından korunan ve TaskWebApp tarafından çağrılır.

### <a name="configure-the-web-application"></a>Web uygulamasını yapılandır

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
2. **TaskWebApp** projesinde **Web.config**’i açın.
3. API’yi yerel olarak çalıştırmak üzere, **api:TaskServiceUrl** için localhost ayarını kullanın. Web.config’i aşağıdaki gibi değiştirin: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. API’nin URI’sini yapılandırın. Bu web uygulamasının API isteği göndermek için kullandığı URI'dir. Ayrıca istenen izinleri yapılandırın.

    ```C#
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. **TaskService** projesinde **Web.config**’i açın.
2. API’yi kiracınızı kullanmak için yapılandırın.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. İstemci kimliğini API’niz için kayıtlı Uygulama Kimliğine ayarlayın.

    ```C#
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

4. Kullanıcı oturum açma adını ayarıyla Yukarı Akış güncelleştirme ve kullanıcı oturum açma akışı.

    ```C#
    <add key="ida:SignUpSignInUserFlowId" value="B2C_1_signupsignin1" />
    ```

5. Kapsamlar ayarını portalda oluşturduğunuz değerle eşleşecek şekilde yapılandırın.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

**TaskWebApp** ve **TaskService** projelerinin ikisini birden çalıştırmanız gerekir. 

1. Çözüm Gezgini’nde, çözüme sağ tıklayıp **Başlangıç Projelerini Ayarla...** seçeneğini belirleyin. 
2. Seçin **birden fazla başlangıç projesi**.
3. İki proje için de **Eylem**’i **Başlat** olarak değiştirin.
4. Tıklayın **Tamam** yapılandırmayı kaydetmek için.
5. İki uygulamayı da çalıştırmak için **F5**'e basın. Her uygulama kendi tarayıcı sekmesinde açılır `https://localhost:44316/` web uygulamasıdır.
    `https://localhost:44332/` web API’sidir.

6. Web uygulamasında tıklayın **kaydolma / oturum açma** web uygulamasına oturum açmak için. Daha önce oluşturduğunuz hesabı kullanın. 
7. Oturum açtıktan sonra tıklayın **Yapılacaklar listesi** ve bir Yapılacaklar listesi öğesi oluşturun.

Bir Yapılacaklar listesi öğesi oluşturduğunuzda, web uygulaması, web API'sine Yapılacaklar listesi öğesi oluşturmak için bir istek gönderir. Korumalı web uygulaması, Azure AD B2C kiracınızdaki korumalı web API'sini çağırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için Kapsamları yapılandırma
> * Web API'sine izinler verin
> * Örnek uygulamayı yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Kimlik sağlayıcıları Azure Active Directory B2C uygulamalarınızın ekleyin](tutorial-add-identity-providers.md)
