---
title: Öğretici - Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node.js web API'sine erişim izni verme| Microsoft Docs
description: Bir Node.js web API’sini korumak ve bir .NET masaüstü uygulamasından çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: PatAltimore
ms.author: patricka
ms.reviewer: parja
ms.date: 3/01/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory-b2c
ms.openlocfilehash: da3b8c42fc98f0957f2fa1a1ac95e12737528863
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Öğretici - Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node.js web API'sine erişim izni verme

Bu öğreticide bir Windows Presentation Foundation (WPF) masaüstü uygulamasından bir Azure Active Directory (Azure AD) B2C korumalı Node.js web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracınıza web API’si kaydetme
> * Bir web API’si için kapsamları tanımlama ve yapılandırma
> * Web API’sine uygulama izinleri verme
> * Kaynak kodu bir web API’sini korumak için Azure AD B2C kullanmak üzere güncelleştirme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisini](active-directory-b2c-tutorials-desktop-app.md) tamamlayın.
* **.NET masaüstü geliştirme** ve **ASP.NET ve web geliştirme** iş yükleriyle [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi yükleyin.
* [Node.js](https://nodejs.org/en/download/)’yi yükleme

## <a name="register-web-api"></a>Web API’si kaydetme

Web API’si kaynaklarının Azure Active Directory’den bir [erişim belirteci](../active-directory/develop/active-directory-dev-glossary.md#access-token) sunan [istemci uygulamalar](../active-directory/develop/active-directory-dev-glossary.md#client-application) tarafından [korunan kaynak isteklerini](../active-directory/develop/active-directory-dev-glossary.md#resource-server) kabul etmesi ve yanıtlaması için kiracınızda kayıtlı olmaları gerekir. Kayıt, kiracınızda [uygulama ve hizmet sorumlusu nesnesini](../active-directory/develop/active-directory-dev-glossary.md#application-object) belirler. 

[Azure portalında](https://portal.azure.com/) Azure AD B2C kiracınızın genel yöneticisi olarak oturum açın.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Azure Portalı'ndaki Hizmetler listesinden **Azure AD B2C**’yi seçin.

2. B2C ayarlarında **Uygulamalar**’a ve ardından  **Ekle**’ye tıklayın.

    Örnek web API’sini kiracınıza kaydetmek için aşağıdaki ayarları kullanın.
    
    ![Yeni bir API ekleme](media/active-directory-b2c-tutorials-desktop-app-webapi/web-api-registration.png)
    
    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Ad** | Örnek Basit Node.js web API’m | Web API’nizi geliştiricilere tanıtan bir **Ad** girin. |
    | **Web uygulamasını / web API'sini dahil etme** | Yes | Web API’si için **Evet**’i seçin. |
    | **Örtük akışa izin verme** | Yes | API [OpenID Connect oturumu](active-directory-b2c-reference-oidc.md) kullandığından **Evet**’i seçin. |
    | **Yanıt URL'si** | `http://localhost:5000` | Yanıt URL'leri, Azure AD B2C'nin, API’niz tarafından istenen belirteçleri döndürdüğü uç noktalardır. Bu öğreticide örnek web API’si yerel olarak (localhost) çalışır ve 5000 numaralı bağlantı noktasını dinler. |
    | **Uygulama Kimliği URI'si** | demoapi | URI, kiracıdaki API’yi benzersiz olarak tanımlar. Bu, kiracı başına birden çok API kaydetmenize olanak sağlar. [Kapsamlar](../active-directory/develop/active-directory-dev-glossary.md#scopes) korumalı API kaynağına erişimi yönetir ve Uygulama Kimliği URI’si başına tanımlanır. |
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
| **Kapsam** | demo.read | Demo API’ye okuma erişimi|

**Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

### <a name="grant-app-permissions-to-web-api"></a>Web API’sine uygulama izinleri verme

Bir uygulamadan korumalı web API’sini çağırmak için, API’ye uygulama izinleri vermeniz gerekir. Bu öğreticide, [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](active-directory-b2c-tutorials-desktop-app.md) oluşturduğunuz masaüstü uygulamasını kullanın.

1. Azure portalında, hizmetler listesinden **Azure AD B2C**’yi seçin ve kayıtlı uygulama listesini görmek için **Uygulamalar**’a tıklayın.

2. Uygulama listesinden **Örnek WPF Uygulamam**’ı seçip **API erişimi (Önizleme)** ve ardından **Ekle**’ye tıklayın.

3. **API seçin** açılır menüsünde, kayıtlı **Örnek Node.js web API’si** web API’nizi seçin.

4. **Kapsamları seçin** açılır menüsünde, web API’si kaydında tanımladığınız kapsamları seçin.

    ![uygulama için kapsamları seçme](media/active-directory-b2c-tutorials-web-api/selecting-scopes-for-app.png)

5. **Tamam**’a tıklayın.

**Örnek WPF Uygulamam**, korumalı **Örnek Node.js web API’si** öğesini çağırmak için kaydedilir. Kullanıcılar WPF masaüstü uygulamasını kullanmak için Azure AD B2C ile [kimlik doğrulaması yapar](../active-directory/develop/active-directory-dev-glossary.md#authentication). Masaüstü uygulaması, korumalı web API’sine erişmek için Azure AD B2C’den bir [yetkilendirme izni](../active-directory/develop/active-directory-dev-glossary.md#authorization-grant) alır.

## <a name="update-web-api-code"></a>Web API kodunu güncelleştirme

API’yi kaydettikten ve kapsamları tanımladıktan sonra, web API kodunuzu Azure AD B2C kiracınızı kullanmak için yapılandırmanız gerekir. Bu öğreticide, GitHub’dan indirebileceğiniz örnek bir Node.js web uygulaması yapılandırırsınız. 

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
Node.js web API’si örneği API’ye yapılan çağrıları korumak için Azure AD B2C’yi etkinleştirmek üzere Passport.js kitaplığını kullanır. 

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. Node.js web API’si örneğinde `index.html` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarını değiştirin:

```nodejs
var tenantID = "<your-tenant-name>.onmicrosoft.com";
var clientID = "<Application ID for your Node.js Web API>";
var policyName = "B2C_1_SiUpIn";  // Sign-in / sign-up policy name
```

### <a name="configure-the-desktop-app"></a>Masaüstü uygulamasını çalıştır

1. Visual Studio’da [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinden](active-directory-b2c-tutorials-desktop-app.md) `active-directory-b2c-wpf` çözümünü açın.

## <a name="run-the-sample"></a>Örneği çalıştırma

Node.js web API’sini çalıştır:

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    node index.js
    ```

Masaüstü uygulamasını çalıştır:

1. Masaüstü uygulamasını çalıştırmak için **F5**'e basın.
2. [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](active-directory-b2c-tutorials-desktop-app.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
3. **API’yı Çağır** düğmesine tıklayın. 

Masaüstü uygulaması, web API’sine bir istek gönderir ve oturum açmış kullanıcının görünen adı ile bir yanıt alır. Korumalı masaüstü API’niz Azure AD B2C kiracınızdaki korumalı web API’sini çağırır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Azure AD B2C’de kapsamları kaydedip tanımlayarak bir ASP.NET web API’sini koruma adımlarını öğrendiniz. Kullanılabilir Azure AD B2C kod örneklerine giderek daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure AD B2C kod örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory-b2c&sort=0)