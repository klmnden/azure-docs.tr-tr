---
title: Microsoft kimlik platformu JavaScript hızlı başlangıç - Azure
description: JavaScript uygulamaları Microsoft kimlik platformu kullanarak erişim belirteçleri gerektiren bir API çağrısı nasıl öğrenin.
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
ms.openlocfilehash: 305479c8872883e434731b5062b408f97f68cc43
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055929"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-single-page-application"></a>Hızlı Başlangıç: Kullanıcılar oturum ve bir JavaScript tek sayfa uygulamasında erişim belirteci alma

Bu hızlı başlangıçta, bir JavaScript tek sayfalı uygulama (SPA) kişisel hesaplar, iş ve Okul hesapları kullanıcılara nasıl kaydolabilirsiniz gösteren kod örneği kullanmayı öğrenin. JavaScript SPA'ya da Microsoft Graph API'sini veya herhangi bir web API'sini çağırmak için bir erişim belirteci edinebilirsiniz.

![Bu hızlı başlangıçtaki örnek uygulamasını nasıl çalışır?](media/quickstart-v2-javascript/javascriptspa-intro.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, aşağıdaki Kurulum gerektirir:
* Node.js sunucusu ile projeyi çalıştırmak için indirme ve yükleme [Node.js](https://nodejs.org/en/download/).
* Proje dosyalarını düzenlemek için indirme ve yükleme [Visual Studio Code](https://code.visualstudio.com/download).
* Proje bir Visual Studio çözümü olarak çalıştırmak için indirme ve yükleme [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Kaydolun ve hızlı başlangıç uygulamanızı indirin
> Hızlı Başlangıç uygulamanızı başlatmak için aşağıdaki seçeneklerden birini kullanın.
>
> ### <a name="option-1-express-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1 (hızlı): Kaydet ve otomatik Uygulamanızı yapılandırmak ve ardından, kod örneğini indirin
>
> 1. Oturum [Azure portalında](https://portal.azure.com) bir iş veya Okul hesabı veya kişisel Microsoft hesabı kullanarak.
> 1. Kullanmak istediğiniz birden fazla Kiracı için hesap erişmenizi sağ üst köşedeki hesabı seçin ve ardından Azure Active Directory (Azure AD) kiracısı ile portal oturumunuzu ayarlayın.
> 1. Yeni Git [Azure Portalı - Uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade/quickStartType/JavascriptSpaQuickstartPage/sourceType/docs) bölmesi.
> 1. Uygulamanız için bir ad girin ve seçin **kaydetme**.
> 1. Karşıdan yükle ve otomatik olarak yeni Uygulamanızı yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-manual-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2 (el ile): Kaydetme ve uygulama ve kod örneğinizi el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydetme
>
> 1. Oturum [Azure portalında](https://portal.azure.com) bir iş veya Okul hesabı veya kişisel Microsoft hesabı kullanarak.
>
> 1. Kullanmak istediğiniz birden fazla Kiracı için hesap erişmenizi sağ üst köşedeki hesabınızı seçin ve ardından Azure AD kiracısına portal oturumunuzu ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformu Git [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. Seçin **yeni kayıt**.
> 1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
> 1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
> 1. Altında **yeniden yönlendirme URI'si** bölümünde aşağı açılan listesinde seçin **Web** platform ve ardından değerine `http://localhost:30662/`.
> 1. **Kaydol**’u seçin. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** daha sonra kullanmak için değer.
> 1. Bu Hızlı Başlangıç [örtük izin akışı](v2-oauth2-implicit-grant-flow.md) etkinleştirilecek. Kayıtlı uygulama sol bölmesinde seçin **kimlik doğrulaması**.
> 1. İçinde **Gelişmiş ayarlar** bölümündeki **örtük vermeyi**seçin **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutuları. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırması gerekir çünkü kimlik ve erişim belirteçler gereklidir.
> 1. Bölmenin en üstünde seçin **Kaydet**.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1\. adım: Uygulamanızı Azure portalında yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yeniden yönlendirme eklemeniz gereken URI olarak `http://localhost:30662/` ve etkinleştirme **örtük vermeyi**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için şu değişiklikleri yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-javascript/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-project"></a>2\. adım: Projenizi indirin

Geliştirme ortamınız için uygun seçeneği seçin:

* Node.js kullanarak bir web sunucusu ile projeyi çalıştırmak için [core proje dosyaları indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip). Dosyaları açmak için bir düzenleyici gibi kullanın [Visual Studio Code](https://code.visualstudio.com/).

* (İsteğe bağlı) IIS sunucusu ile projeyi çalıştırmak için [Visual Studio projeyi indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip). Zip dosyasını yerel bir klasöre ayıklar (örneğin, *C:\Azure-Samples*).



#### <a name="step-3-configure-your-javascript-app"></a>3\. adım: JavaScript uygulamanızı yapılandırın

> [!div renderon="docs"]
> İçinde *JavaScriptSPA* klasörünü Düzenle *index.html*, ayarlayıp `clientID` ve `authority` altındaki değerler `msalConfig`.

> [!div class="sxs-lookup" renderon="portal"]
> İçinde *JavaScriptSPA* klasörünü Düzenle *index.html*, değiştirin `msalConfig` aşağıdaki kod ile:

```javascript
var msalConfig = {
    auth: {
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/Enter_the_Tenant_info_here"
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
> - *\<Enter_the_Application_Id_here >* olduğu **uygulama (istemci) kimliği** , kayıtlı uygulama için.
> - *\<Enter_the_Tenant_info_here >* aşağıdaki seçeneklerden birine ayarlayın:
>    - Uygulamanız destekliyorsa *kuruluş bu dizinde hesapları*, bu değeri ile değiştirin **Kiracı kimliği** veya **Kiracı adı** (örneğin,  *contoso.microsoft.com*).
>    - Uygulamanız destekliyorsa *herhangi bir kuruluş dizini hesaplarında*, bu değeri ile değiştirin **kuruluşlar**.
>    - Uygulamanız destekliyorsa *herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında*, bu değeri ile değiştirin **ortak**. İçin desteği kısıtlamak için *kişisel Microsoft hesapları yalnızca*, bu değeri ile değiştirin **tüketiciler**.
>
> > [!TIP]
> > **Uygulama (istemci) Kimliği**, **Dizin (kiracı) Kimliği** ve **Desteklenen hesap türleri** değerlerini bulmak için Azure portalında uygulamanın **Genel bakış** sayfasına gidin.
>

#### <a name="step-4-run-the-project"></a>4\. Adım: Projeyi çalıştırma

* Kullanıyorsanız [Node.js](https://nodejs.org/en/download/):

    1. Sunucu başlatmak için proje dizininden aşağıdaki komutu çalıştırın:

        ```batch
        npm install
        node server.js
        ```

    1. Bir web tarayıcısı açın ve gidin `http://localhost:30662/`.
    1. Seçin **oturum** oturum açma başlatın ve ardından Microsoft Graph API'sini çağırmak için.


* Kullanıyorsanız [Visual Studio](https://visualstudio.microsoft.com/downloads/)proje çözümü seçin ve ardından projeyi çalıştırmak için F5'i seçin.

Tarayıcıda uygulama yüklendikten sonra seçin **oturum**. İlk kez oturum açarken, profilinizi erişmek ve oturum açmak için uygulama izin vermek için onay vermeniz istenir. Başarıyla açıldıktan sonra kullanıcı profili bilgilerinize sayfada görüntülenmesi gerekir.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msaljs"></a>msal.js

MSAL kitaplığı, kullanıcılar oturum açtığında ve Microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteçleri ister. Bu hızlı başlangıçta *index.html* dosyası kitaplığına bir başvuru içerir:

```html
<script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.min.js"></script>
```
> [!TIP]
> Altında yayımlanan en son sürüm ile önceki sürümü değiştirebilirsiniz [MSAL.js sürümleri](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases).


Alternatif olarak, yüklü Node.js varsa, Node.js paket yöneticisini (npm) aracılığıyla en son sürümünü indirebilirsiniz:

```batch
npm install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Hızlı Başlangıç kod ayrıca MSAL kitaplığı başlatılamıyor işlemini gösterir:

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
> |`ClientId`     | Azure Portalı'nda kayıtlı uygulamanın uygulama kimliği.|
> |`authority`    | (İsteğe bağlı) Hesap türleri için daha önce yapılandırma bölümünde açıklandığı gibi destekler yetkilisi URL'si. Varsayılan yetkilisi `https://login.microsoftonline.com/common`. |
> |`cacheLocation`  | (İsteğe bağlı) Tarayıcı depolama kimlik doğrulama durumu için ayarlar. SessionStorage varsayılandır.   |
> |`storeAuthStateInCookie`  | (İsteğe bağlı) Tarayıcı tanımlama bilgileri kimlik doğrulaması akışlarında doğrulanması için gerekli kimlik doğrulama isteği durumu depolar kitaplığı. Belirli azaltmak IE ve Edge tarayıcılarda bu tanımlama bilgisinin ayarlanıp [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues). |

Kullanılabilir yapılandırılabilir seçenekleri hakkında daha fazla bilgi için bkz. [başlatmak istemci uygulamaları](msal-js-initializing-client-applications.md).

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
> | `scopes`   | (İsteğe bağlı) Oturum açma zaman kullanıcı onayı için istenen kapsamlarını içerir. Örneğin, `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (diğer bir deyişle, `api://<Application ID>/access_as_user`). |

> [!TIP]
> Alternatif olarak kullanmak isteyebilirsiniz `loginRedirect` geçerli sayfa bir açılan pencere yerine oturum açma sayfasına yeniden yönlendirmek için yöntemi.

### <a name="request-tokens"></a>Belirteç isteme

MSAL belirteçlerini almak için üç yöntemi kullanır: `acquireTokenRedirect`, `acquireTokenPopup`, ve `acquireTokenSilent`

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

Kullanıcıları, Microsoft kimlik platformu uç noktası ile etkileşim kurmak için gerek duyduğunuz senaryolara durumlar vardır. Örneğin:
* Kullanıcıların parolalarını süresi dolduğundan, kimlik bilgilerini yeniden girmeniz gerekebilir.
* Uygulamanızın kullanıcı onay için gereken ek kaynak kapsamlar için erişim izni istiyor.
* İki öğeli kimlik doğrulaması gereklidir.

Uygulamaların çoğu için önerilen ve normal desenin çağırmaktır `acquireTokenSilent` ilk olarak, daha sonra özel durumu yakalar ve sonra çağrı `acquireTokenPopup` (veya `acquireTokenRedirect`) etkileşimli bir isteği başlatmak için.

Çağırma `acquireTokenPopup` oturum açmak için bir açılan pencere sonuçlanıyor. (Veya `acquireTokenRedirect` sonuçları kullanıcılarını Microsoft kimlik platformu uç noktaya yönlendirme içinde.) Bu pencerede, kullanıcıların kimlik bilgilerini onaylayan, gerekli kaynak için izin vermek veya iki öğeli kimlik doğrulamasını tamamlayan etkileşim gerekir.

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
> Bu hızlı başlangıçta kullanılmaktadır `loginRedirect` ve `acquireTokenRedirect` Microsoft Internet Explorer yöntemleriyle nedeniyle bir [bilinen sorun](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Known-issues-on-IE-and-Edge-Browser#issues) açılır pencereleri işlenmesi için Internet Explorer tarafından ilgili.

## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıç için uygulama oluşturma hakkında daha ayrıntılı adım adım yönergeler için bkz:

> [!div class="nextstepaction"]
> [Oturum açın ve MS Graph'i çağırmaya öğretici](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

Belgeleri, SSS, sorunları ve daha fazlasını MSAL depoya göz atmak için bkz:

> [!div class="nextstepaction"]
> [MSAL.js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)
