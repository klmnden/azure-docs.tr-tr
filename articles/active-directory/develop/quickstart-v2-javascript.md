---
title: Azure AD v2 JavaScript hızlı başlangıcı | Microsoft Docs
description: JavaScript uygulamaları Azure Active Directory v2.0 uç noktası tarafından erişim belirteçlerini gerektiren bir API çağrısı nasıl bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6a97e03f3c195b9fbd0ee7a09950414b7a940c7c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56217488"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir JavaScript uygulamasında erişim belirteci alma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıçta, tek sayfalı uygulama (SPA) kişisel hesaplarında oturum, iş ve Okul hesapları, Microsoft Graph API'sini veya herhangi bir web API'sini çağırmak için erişim belirteci almak bir JavaScript nasıl erişileceğini gösteren bir kod örneğini nasıl kullanacağınızı öğreneceksiniz.

![Bu Hızlı Başlangıcın oluşturduğu örnek uygulama nasıl çalışır?](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Kaydolun ve hızlı başlangıç uygulamanızı indirin
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. [Azure portal - Uygulama Kaydı (Önizleme)](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) sayfasına gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> 1. Oturum [Azure portalında](https://portal.azure.com/) bir uygulamayı kaydetme.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin ve ardından **Uygulama kayıtları (Önizleme) > Yeni kayıt** seçeneğini belirleyin.
> 1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
> 1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
> 1. Seçin **Web** platform altında **yeniden yönlendirme URI'si** bölümünde ve değerine `http://localhost:30662/`.
> 1. Bittiğinde **Kaydet**’i seçin.  Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** değeri.
> 1. Bu Hızlı Başlangıç [örtük izin akışı](v2-oauth2-implicit-grant-flow.md) etkinleştirilecek. Kayıtlı uygulama sol gezinti bölmesinde seçin **kimlik doğrulaması**.
> 1. İçinde **Gelişmiş ayarlar**altında **örtük vermeyi**, her ikisini de etkinleştirmek **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutularını. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırmak sonun kimliği ve erişim belirteçler gereklidir.
> 1. **Kaydet**’i seçin.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yeniden yönlendirme eklemeniz gereken URI olarak `http://localhost:30662/` ve etkinleştirme **örtük vermeyi**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için şu değişiklikleri yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2. Adım: Projenizi indirin

Geliştirme ortamınız için uygun olan bu seçeneklerden birini seçebilirsiniz.
* [-Web sunucusu, Node.js gibi core proje dosyaları indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Örneğin, bir yerel klasör zip dosyasını ayıklayın **C:\Azure-Samples**.

#### <a name="step-3-configure-your-javascript-app"></a>3. Adım: JavaScript uygulamanızı yapılandırın

> [!div renderon="docs"]
> Düzen `index.html` ayarlayıp `clientID` ve `authority` altındaki değerler `applicationConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> Düzen `index.html` değiştirin `applicationConfig` ile:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!div renderon="docs"]
>
> Konumlar:
> - `Enter_the_Application_Id_here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
> - `Enter_the_Tenant_Info_Here` - aşağıdaki seçeneklerden birine ayarlanır:
>   - Uygulamanız **Bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Kiracı Kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com) ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar ve kişisel Microsoft hesaplarını** destekliyorsa bu değeri `common` ile değiştirin
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.

> [!NOTE]
> Sunucu bağlantı noktası 30662 dinleyecek şekilde yapılandırılmış *server.js* dosyası [Node.js](https://nodejs.org/en/download/) proje ve *.csproj* dosyası [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)proje.
>

#### <a name="step-4-run-the-project"></a>4. Adım: Projeyi çalıştırma

* Node.js kullanıyorsanız:

    1. Sunucu başlatmak için proje dizininden aşağıdaki komutu çalıştırın:

        ```batch
        npm install
        node server.js
        ```

    1. Bir web tarayıcısı açın ve gidin `http://localhost:30662/`.
    1. Tıklayın **oturum** oturum başlatın ve ardından Microsoft Graph API'sini çağırmak için düğme.


* Visual Studio kullanıyorsanız, proje çözümü seçin ve ENTER tuşuna basın emin olun **F5** projeyi çalıştırın.

## <a name="more-information"></a>Daha Fazla Bilgi

### <a name="msaljs"></a>*msal.js*

MSAL, kullanıcılar ve belirteçler, Microsoft Azure Active Directory tarafından (Azure AD) korumalı bir API'ye erişmek için kullanılan istek imzalamak için kullanılan kitaplığıdır. Hızlı Başlangıç'ın *index.html* kitaplığına bir başvuru içerir:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.2.3/js/msal.min.js"></script>
```

Alternatif olarak, yüklü bir düğüm varsa, npm indirebilirsiniz:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Hızlı Başlangıç kod ayrıca kitaplığı başlatılamıyor işlemini gösterir:

```javascript
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, applicationConfig.authority, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Konum  |  |
> |---------|---------|
> |`ClientId`     |Azure portalına kaydedilen uygulamaya ait Uygulama Kimliği|
> |`authority`    |Yetkilisi URL'dir. Geçirme *null* varsayılan yetkilisi ayarlar `https://login.microsoftonline.com/common`. Uygulamanız tek kiracılı (bir dizinde hedefleme hesapları) ise, bu değeri ayarlamak `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Kimlik doğrulaması uygulamaya geri yönlendirir sonra çağrılan geri çağırma yöntemi. Burada, `acquireTokenRedirectCallBack` geçirilir. LoginPopup kullanıyorsanız bu yöntem null.|
> |`options`  |İsteğe bağlı parametreler koleksiyonu. Bu durumda `storeAuthStateInCookie` ve `cacheLocation` isteğe bağlı bir yapılandırmadır. Bkz: [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) seçenekleri hakkında daha fazla ayrıntı için. |

### <a name="sign-in-users"></a>Kullanıcılar oturum

Aşağıdaki kod parçacığı, kullanıcılarının oturumunu açmak gösterilmektedir:

```javascript
myMSALObj.loginPopup(applicationConfig.graphScopes).then(function (idToken) {
    //Callback code here
})
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | (İsteğe bağlı) Oturum açma zaman kullanıcı onayı için istenen kapsamlarını içerir. Örneğin, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (diğer bir deyişle, `api://<Application ID>/access_as_user` ). Burada, `applicationConfig.graphScopes` geçirilir. |

> [!TIP]
> Alternatif olarak kullanmak isteyebilirsiniz `loginRedirect` geçerli sayfa bir açılan pencere yerine oturum açma sayfasına yeniden yönlendirmek için yöntemi.

### <a name="request-tokens"></a>Belirteç isteme

MSAL belirteçlerini almak için kullanılan üç yöntem vardır: `acquireTokenRedirect`, `acquireTokenPopup` ve `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `loginRedirect` veya `loginPopup` yöntemi ilk kez yürütüldüğünde `acquireTokenSilent` yapılan sonraki çağrılar için korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir. Veya belirteçleri yenileme isteği için çağrıları sessizce yapılır.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | API için erişim belirtecinde döndürülecek istenen kapsamlarını içerir. Örneğin, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (diğer bir deyişle, `api://<Application ID>/access_as_user`). Burada, `applicationConfig.graphScopes` geçirilir.|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Kullanıcıları, Azure AD v2.0 uç noktası ile etkileşim kurmak için gerek duyduğunuz senaryolara durumlar vardır. Örneğin:
* Kullanıcıların parolalarını süresi dolduğundan, kimlik bilgilerini yeniden girmeniz gerekebilir
* Uygulamanız, kullanıcıya onay için gereken ek kaynak kapsamlar için erişim izni isteme
* İki faktörlü kimlik doğrulama gerektiğinde

Uygulamaların çoğu için önerilen ve normal desenin çağırmaktır `acquireTokenSilent` ilk olarak, daha sonra özel durumu yakalar ve sonra çağrı `acquireTokenRedirect` (veya `acquireTokenPopup`) etkileşimli bir isteği başlatmak için.

Çağırma `acquireTokenPopup(scope)` oturum açmak için bir açılan pencere içindeki sonuçlar (veya `acquireTokenRedirect(scope)` sonuçları kullanıcılarını Azure AD v2.0 uç noktasına yönlendirme içinde) gerekli kaynak için onay vermiş kullanıcılar ya da kendi kimlik bilgilerini onaylayarak etkileşim kurmak gereken yere veya iki faktörlü kimlik doğrulaması tamamlanıyor.

```javascript
myMSALObj.acquireTokenPopup(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> [!NOTE]
> Bu hızlı başlangıçta kullanılmaktadır `loginRedirect` ve `acquireTokenRedirect` kullanılan tarayıcı nedeni Internet Explorer olduğunda yöntemleri bir [bilinen sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) açılır pencereleri işlenmesi için Internet Explorer tarayıcı tarafından ilgili.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç için uygulamayı oluşturmak nasıl ilişkin daha ayrıntılı adım adım yönergeler için aşağıdaki JavaScript Eğitim programını deneyin.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Bu hızlı başlangıçta uygulama oluşturmak için adımları öğrenin

> [!div class="nextstepaction"]
> [Graph API'si çağırma öğreticisi](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>MSAL depo belgeleri, SSS, sorunları ve daha fazlası için Gözat

> [!div class="nextstepaction"]
> [msal.js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)
