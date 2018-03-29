---
title: Bir ASP.NET Web API’sini korumak için Azure Active Directory B2C’yi kullanma öğreticisi
description: Bir ASP.NET web API’sini korumak ve bir ASP.NET web uygulamasından çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.author: davidmu
ms.date: 1/23/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: f4e1c18f151a9c815258f01ea198d3d173d0b44e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-use-azure-active-directory-b2c-to-protect-an-aspnet-web-api"></a>Öğretici: Bir ASP.NET web API’sini korumak için Azure Active Directory B2C’yi kullanma

Bu öğreticide bir ASP.NET web uygulamasından bir Azure Active Directory (Azure AD) B2C korumalı web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza web API’si kaydetme
> * Bir web API’si için kapsamları tanımlama ve yapılandırma
> * Web API’sine uygulama izinleri verme
> * Kaynak kodu bir web API’sini korumak için Azure AD B2C kullanmak üzere güncelleştirme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Azure Active Directory B2C’yi bir ASP.NET Web Uygulamasında Kullanıcı Kimlik Doğrulaması için kullanma öğreticisini](active-directory-b2c-tutorials-web-app.md) tamamlayın.
* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.

## <a name="register-web-api"></a>Web API’si kaydetme

Web API’si kaynaklarının Azure Active Directory’den bir [erişim belirteci](../active-directory/develop/active-directory-dev-glossary.md#access-token) sunan [istemci uygulamalar](../active-directory/develop/active-directory-dev-glossary.md#client-application) tarafından [korunan kaynak isteklerini](../active-directory/develop/active-directory-dev-glossary.md#resource-server) kabul etmesi ve yanıtlaması için kiracınızda kayıtlı olmaları gerekir. Kayıt, kiracınızda [uygulama ve hizmet sorumlusu nesnesini](../active-directory/develop/active-directory-dev-glossary.md#application-object) belirler. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin.

2. B2C ayarlarında **Uygulamalar**’a tıklayın ve ardından **+ Ekle**’ye tıklayın.

    Örnek web API’sini kiracınıza kaydetmek için aşağıdaki ayarları kullanın.
    
    ![Yeni bir API ekleme](media/active-directory-b2c-tutorials-web-api/web-api-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek Web API’si | Web API’nizi geliştiricilere tanıtan bir **Ad** girin. |
    | **Web uygulamasını / web API'sini dahil etme** | Yes | Web API’si için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Yes | API [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `https://localhost:44332` | Yanıt URL'leri, Azure AD B2C'nin, API’niz tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek web API’si yerel olarak (localhost) çalışır ve 44332 numaralı bağlantı noktasını dinler. |
    | **Uygulama Kimliği URI'si** | myAPISample | URI, kiracıdaki API’yi benzersiz olarak tanımlar. Bu, kiracı başına birden çok API kaydetmenize olanak sağlar. [Kapsamlar](../active-directory/develop/active-directory-dev-glossary.md#scopes) korumalı API kaynağına erişimi yönetir ve Uygulama Kimliği URI’si başına tanımlanır. |
    | **Yerel istemci** | Hayır | Bu, bir web API’si olduğu için ve yerel bir istemci olmadığı için Hayır’ı seçin. |
    
3. API’nizi kaydetmek için **Oluştur** seçeneğine tıklayın.

Kayıtlı API’ler Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web API’nizi seçin. Web API’sinin özellik bölmesi görüntülenir.

![Web API’si özellikleri](./media/active-directory-b2c-tutorials-web-api/b2c-web-api-properties.png)

**Uygulama İstemci Kimliği**’ni not alın. Kimlik, API’yi benzersiz bir şekilde tanımlar ve öğreticinin sonraki bölümlerinde API’yi yapılandırırken gerekir.

Web API’nizi Azure AD B2C ile kaydetmek bir güven ilişkisini tanımlar. API B2C ile kaydedildiğinden, API artık diğer uygulamalardan aldığı B2C erişim belirteçlerine güvenebilir.

## <a name="define-and-configure-scopes"></a>Kapsamları tanımlama ve yapılandırma

[Kapsamlar](../active-directory/develop/active-directory-dev-glossary.md#scopes) korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlayacaksınız.

### <a name="define-scopes-for-the-web-api"></a>Web API’si için kapsamları tanımlama

Kayıtlı API’ler Azure AD B2C kiracısı için uygulamalar listesinde görüntülenir. Listeden web API’nizi seçin. Web API’sinin özellik bölmesi görüntülenir.

**Yayımlanan kapsamlar (Önizleme)** seçeneğine tıklayın.

API için kapsamları tanımlamak için, aşağıdaki girişleri ekleyin. 

![web API’sinde tanımlanan kapsamlar](media/active-directory-b2c-tutorials-web-api/scopes-defined-in-web-api.png)

| Ayar      | Önerilen değer  | Açıklama                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Kapsam** | Hello.Read | Hello’ya okuma erişimi |
| **Kapsam** | Hello.Write | Hello’ya yazma erişimi |

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

### <a name="grant-app-permissions-to-web-api"></a>Web API’sine uygulama izinleri verme

Bir uygulamadan korumalı web API’sini çağırmak için, API’ye uygulama izinleri vermeniz gerekir. 

1. Azure portalında, hizmetler listesinden **Azure AD B2C**’yi seçin ve kayıtlı uygulama listesini görmek için **Uygulamalar**’a tıklayın.

2. Uygulama listesinden **Örnek Web Uygulamam**’ı seçip **API erişimi (Önizleme)** ve ardından **Ekle**’y tıklayın.

3. **API seçin** açılır menüsünde, kayıtlı **Örnek Web API’si** web API’nizi seçin.

4. **Kapsamları seçin** açılır menüsünde, web API’si kaydında tanımladığınız kapsamları seçin.

    ![uygulama için kapsamları seçme](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. **Tamam**’a tıklayın.

**Örnek Web Uygulamam**, korumalı **Örnek Web API’si** öğesini çağırmak için kaydedilir. Kullanıcılar web uygulamasını kullanmak için Azure AD B2C ile [kimlik doğrulaması yapar](../active-directory/develop/active-directory-dev-glossary.md#authentication). Web uygulaması, korumalı web API’sine erişmek için Azure AD B2C’den bir [yetkilendirme izni](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) alır.

## <a name="update-web-api-code"></a>Web API kodunu güncelleştirme

API’yi kaydettikten ve kapsamları tanımladıktan sonra, web API kodunuzu Azure AD B2C kiracınızı kullanmak için yapılandırmanız gerekir. Bu öğreticide örnek bir web API’sini yapılandıracaksınız. 

Örnek web API’si şu önkoşul öğreticisinde indirdiğiniz projede bulunur: [Bir ASP.NET Web Uygulamasında Kullanıcı Kimlik Doğrulaması için Azure Active Directory B2C kullanma öğreticisi](active-directory-b2c-tutorials-web-app.md). Önkoşul öğreticisini tamamlamadıysanız, devam etmeden önce tamamlayın.

Örnek çözümde iki proje vardır:

**Örnek web uygulaması (TaskWebApp):** Görev listesi oluşturmak ve düzenlemek için kullanılan web uygulaması. Web uygulaması, kullanıcıların bir e-posta adresiyle kaydolması veya oturum açması için **kaydolma veya oturum açma** ilkesi kullanır.

**Örnek web API’si uygulaması (TaskService):** Görev listesi oluşturma, okuma, güncelleştirme ve silme işlevlerini destekleyen web API’si. Web API’si Azure AD B2C tarafından korunur ve web uygulaması tarafından çağrılır.

Örnek web uygulaması ve web API’si, yapılandırma değerlerini her projenin Web.config dosyasındaki uygulama ayarları olarak tanımlar.

**B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.

### <a name="configure-the-web-app"></a>Web uygulamasını yapılandırma

1. **TaskWebApp** projesinde **Web.config**’i açın.

2. API’yi yerel olarak çalıştırmak üzere, **api:TaskServiceUrl** için localhost ayarını kullanın. Web.config’i aşağıdaki gibi değiştirin: 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. API’nin URI’sini yapılandırın. Bu, web uygulamasının API isteği göndermek için kullandığı URI’dir. Ayrıca istenen izinleri yapılandırın.

```C#
<add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/myAPISample/" />
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
    <add key="ida:ClientId" value="<The Application ID for your web API obtained from the Azure portal>"/>
    ```

4. İlke ayarını, kaydolma ve oturum açma ilkelerinizi oluştururken oluşturulan ad ile güncelleştirin.

    ```C#
    <add key="ida:SignUpSignInPolicyId" value="b2c_1_SiUpIn" />
    ```

5. Kapsamlar ayarını portalda oluşturduğunuz değerle eşleşecek şekilde yapılandırın.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample-web-app-and-web-api"></a>Örnek web uygulaması ve web API’sini çalıştırma

**TaskWebApp** ve **TaskService** projelerinin ikisini birden çalıştırmanız gerekir. 

1. Çözüm Gezgini’nde, çözüme sağ tıklayıp **Başlangıç Projelerini Ayarla...** seçeneğini belirleyin. 
2. **Birden fazla başlangıç projesi** radyo düğmesini seçin.
3. İki proje için de **Eylem**’i **Başlat** olarak değiştirin.
4. Yapılandırmayı kaydetmek için Tamam’a tıklayın.
5. İki uygulamayı da çalıştırmak için **F5**'e basın. Her uygulama kendi tarayıcı sekmesinde açılır. `https://localhost:44316/` web uygulamasıdır.
    `https://localhost:44332/` web API’sidir.

6. Web uygulamasında, web uygulaması için kaydolmak üzere menü başlığındaki kaydolun / oturum açın bağlantısına tıklayın. [Web uygulaması öğreticisinde](active-directory-b2c-tutorials-web-app.md) oluşturduğunuz hesabı kullanın. 
7. Oturum açtığınızda, **Yapılacaklar listesi** bağlantısına tıklayarak bir yapılacaklar listesi öğesi oluşturun.

Bir yapılacaklar listesi öğesi oluşturduğunuzda, web uygulaması yapılacaklar listesi öğesini oluşturmak için web API’sine bir istek gönderir. Korumalı web API’niz Azure AD B2C kiracınızdaki korumalı web API’sini çağırır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure AD B2C’de kapsamları kaydedip tanımlayarak bir ASP.NET web API’sini koruma adımlarını öğrendiniz. Kod adım adım öğreticileri dahil bu senaryoyu geliştirme hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Active Directory B2C kaydolma, oturum açma, profil düzenleme ve parola sıfırlama ile ASP.NET web uygulaması oluşturma](active-directory-b2c-devquickstarts-web-dotnet-susi.md)