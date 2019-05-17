---
title: Microsoft kimlik platformu JavaScript hızlı başlangıç | Azure
description: JavaScript uygulamaları Microsoft kimlik platformu tarafından erişim belirteçlerini gerektiren bir API çağrısı nasıl öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23003186aa413e313578c57616ae03c435f140e1
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785393"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application-spa"></a>Hızlı Başlangıç: Kullanıcılar oturum ve JavaScript tek sayfalı uygulama (SPA) bir erişim belirteci alma

Bu hızlı başlangıçta, nasıl bir JavaScript tek sayfalı uygulama (SPA) kişisel hesaplarında oturum, iş ve Okul hesapları ve Microsoft Graph API'sini veya herhangi bir web API'sini çağırmak için erişim belirteci almak gösteren bir kod örneğini nasıl kullanacağınızı öğreneceksiniz.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu Hızlı Başlangıç için aşağıdaki Kurulum gerekir:
* Node.js sunucusu ile projeyi çalıştırmak için
    * [Node.js](https://nodejs.org/en/download/)’yi yükleme
    * Yükleme [Visual Studio Code](https://code.visualstudio.com/download) proje dosyalarını düzenlemek için
* Proje bir Visual Studio çözümü olarak çalıştırmak için yükleme [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Kaydolun ve hızlı başlangıç uygulamanızı indirin
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * [Express] [Seçenek 1: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * [El ile] [Seçeneği 2: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma](#option-2-register-and-manually-configure-your-application-and-code-sample)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. seçenek: Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'e tıklayın.
> 1. Yönergeleri izleyerek yeni uygulamanızı tek tıkla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. seçenek: Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydedin
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
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
* [Core proje dosyaları indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip), Node.js kullanarak bir web sunucusu ile çalıştırın. Dosyaları açmaya gibi bir düzenleyici kullanın [Visual Studio Code](https://code.visualstudio.com/).

* (İsteğe bağlı) [Visual Studio projeyi indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)IIS sunucusu ile çalıştırmak için. Örneğin, bir yerel klasör zip dosyasını ayıklayın **C:\Azure-Samples**.



#### <a name="step-3-configure-your-javascript-app"></a>3. adım: JavaScript uygulamanızı yapılandırın

> [!div renderon="docs"]
> Klasörü altında *JavaScriptSPA*, Düzenle `index.html` ayarlayıp `clientID` ve `authority` altındaki değerler `msalConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> Klasörü altında *JavaScriptSPA*, Düzen `index.html` değiştirin `msalConfig` ile:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

```
> [!div renderon="docs"]
>
> Konumlar:
> - `Enter_the_Application_Id_here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
> - `Enter_the_Tenant_Info_Here` - aşağıdaki seçeneklerden birine ayarlanır:
>   - Uygulamanız **Bu kuruluş dizinindeki hesapları** destekliyorsa, bu değeri **Kiracı Kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com) ile değiştirin
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız destekliyorsa **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**, bu değeri ile değiştirin `common`. İçin desteği kısıtlamak için *kişisel Microsoft hesapları yalnızca*, bu değeri ile değiştirin `consumers`.
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.
>

#### <a name="step-4-run-the-project"></a>4. Adım: Projeyi çalıştırma

* Kullanıyorsanız [Node.js](https://nodejs.org/en/download/):

    1. Sunucu başlatmak için proje dizininden aşağıdaki komutu çalıştırın:

        ```batch
        npm install
        node server.js
        ```

    1. Bir web tarayıcısı açın ve gidin `http://localhost:30662/`.
    1. Tıklayın **oturum** oturum başlatın ve ardından Microsoft Graph API'sini çağırmak için düğme.


* Kullanıyorsanız [Visual Studio](https://visualstudio.microsoft.com/downloads/), proje çözümü seçin ve ENTER tuşuna basın emin **F5** projeyi çalıştırın.

Tarayıcıda uygulama yüklendikten sonra tıklayın **oturum**.  İlk kez oturum açarken, profilinizi erişmek ve oturum açmak için uygulama izin vermek için onay vermeniz istenir. Üzerinde başarılı oturum açma sayfasında gösterilen kullanıcı profili bilgilerinize görmeniz gerekir.

## <a name="more-information"></a>Ek bilgiler

### <a name="msaljs"></a>*msal.js*

MSAL, kullanıcılar ve belirteçler Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan istek imzalamak için kullanılan kitaplığıdır. Hızlı Başlangıç'ın *index.html* kitaplığına bir başvuru içerir:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> Yukarıdaki sürüm altında yayımlanan en son sürümünü değiştirebilirsiniz [MSAL.js sürümleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


Alternatif olarak, yüklü düğüm varsa, npm aracılığıyla en son sürümünü indirebilirsiniz:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Hızlı Başlangıç kod ayrıca kitaplığı başlatılamıyor işlemini gösterir:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_Info_Here"
    },
    cache: {
        cacheLocation: "localStorage",
        storeAuthStateInCookie: true
    }
};

var myMSALObj = new Msal.UserAgentApplication(msalConfig);
```

> |Konum  |  |
> |---------|---------|
> |`ClientId`     |Azure portalına kaydedilen uygulamaya ait Uygulama Kimliği|
> |`authority`    | (İsteğe bağlı) Bu hesap türlerini destekleyecek şekilde yapılandırma bölümü açıklanan şekilde yetkilisi URL'dir. Varsayılan yetkilisi `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | (İsteğe bağlı) Bu tarayıcı depolama kimlik doğrulama durumu için ayarlar. SessionStorage varsayılandır.   |
> |`storeAuthStateInCookie`  | (İsteğe bağlı) Tarayıcı tanımlama bilgileri kimlik doğrulama akışları doğrulanması için gerekli kimlik doğrulama isteği durumu kitaplığı depolar. Bu belirli azaltmak IE ve Edge tarayıcılarda ayarlanır [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues). |

 Bkz: [wiki](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-basics#configuration-options) yapılandırılabilir seçenekleri hakkında daha fazla ayrıntı için.

### <a name="sign-in-users"></a>Kullanıcılar oturum

Aşağıdaki kod parçacığı, kullanıcılarının oturumunu açmak gösterilmektedir:

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.loginPopup(requestObj).then(function (loginResponse) {
    //Login Success callback code here
}).catch(function (error) {
    console.log(error);
});
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | (İsteğe bağlı) Oturum açma zaman kullanıcı onayı için istenen kapsamlarını içerir. Örneğin, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (diğer bir deyişle, `api://<Application ID>/access_as_user` ). |

> [!TIP]
> Alternatif olarak kullanmak isteyebilirsiniz `loginRedirect` geçerli sayfa bir açılan pencere yerine oturum açma sayfasına yeniden yönlendirmek için yöntemi.

### <a name="request-tokens"></a>Belirteç isteme

MSAL belirteçlerini almak için kullanılan üç yöntem vardır: `acquireTokenRedirect`, `acquireTokenPopup` ve `acquireTokenSilent`

#### <a name="get-a-user-token-silently"></a>Kullanıcı belirtecini sessizce alma

`acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `loginRedirect` veya `loginPopup` yöntemi ilk kez yürütüldüğünde `acquireTokenSilent` yapılan sonraki çağrılar için korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir. Veya belirteçleri yenileme isteği için çağrıları sessizce yapılır.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenSilent(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | API için erişim belirtecinde döndürülecek istenen kapsamlarını içerir. Örneğin, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (diğer bir deyişle, `api://<Application ID>/access_as_user`).|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli olarak kullanıcı belirteci alma

Kullanıcıları, Microsoft kimlik platformu uç ile etkileşim kurmak için gerek duyduğunuz senaryolara durumlar vardır. Örneğin:
* Kullanıcıların parolalarını süresi dolduğundan, kimlik bilgilerini yeniden girmeniz gerekebilir
* Uygulamanız, kullanıcıya onay için gereken ek kaynak kapsamlar için erişim izni isteme
* İki faktörlü kimlik doğrulama gerektiğinde

Uygulamaların çoğu için önerilen ve normal desenin çağırmaktır `acquireTokenSilent` ilk olarak, daha sonra özel durumu yakalar ve sonra çağrı `acquireTokenPopup` (veya `acquireTokenRedirect`) etkileşimli bir isteği başlatmak için.

Çağırma `acquireTokenPopup` oturum açmak için bir açılan pencere sonuçlanıyor (veya `acquireTokenRedirect` sonuçları kullanıcılarını Microsoft kimlik platformu uç noktaya yönlendirme içinde) kullanıcılar için ya da kimlik bilgilerini onaylayarak etkileşim kurmak gereken yere onayı gerekli verme Kaynak veya iki faktörlü kimlik doğrulaması tamamlanıyor.

```javascript
var requestObj = {
    scopes: ["user.read"]
};

myMSALObj.acquireTokenPopup(requestObj).then(function (tokenResponse) {
    // Callback code here
    console.log(tokenResponse.accessToken);
}).catch(function (error) {
    console.log(error);
});
```
> [!NOTE]
> Bu hızlı başlangıçta kullanılmaktadır `loginRedirect` ve `acquireTokenRedirect` kullanılan tarayıcı nedeni Internet Explorer olduğunda yöntemleri bir [bilinen sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) açılır pencereleri işlenmesi için Internet Explorer tarayıcı tarafından ilgili.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç için uygulamayı oluşturmak nasıl ilişkin daha ayrıntılı adım adım yönergeler için aşağıdaki JavaScript Eğitim programını deneyin.

### <a name="learn-the-steps-to-create-the-application-for-this-quickstart"></a>Bu hızlı başlangıçta uygulama oluşturmak için adımları öğrenin

> [!div class="nextstepaction"]
> [Oturum açın ve MS Graph'i çağırmaya öğretici](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>MSAL depo belgeleri, SSS, sorunları ve daha fazlası için Gözat

> [!div class="nextstepaction"]
> [MSAL.js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)
