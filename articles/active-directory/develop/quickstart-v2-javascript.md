---
title: Azure AD v2 JavaScript hızlı başlangıcı | Microsoft Docs
description: JavaScript uygulamaları Azure Active Directory v2.0 uç noktası tarafından erişim belirteçlerini gerektiren bir API çağrısı nasıl bilgi edinin
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 1b884571707aab71e8a8d124ba68f938e5a63a43
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063753"
---
# <a name="quickstart-sign-in-users-and-acquire-an-access-token-from-a-javascript-application"></a>Hızlı Başlangıç: kullanıcılarının oturumunu ve bir JavaScript uygulamasında erişim belirteci alma

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Bu hızlı başlangıçta, tek sayfalı uygulama (SPA) kişisel hesaplarında oturum, iş ve Okul hesapları, Microsoft Graph API'sini veya herhangi bir web API'sini çağırmak için erişim belirteci almak bir JavaScript nasıl erişileceğini gösteren bir kod örneğini nasıl kullanacağınızı öğreneceksiniz.

![Bu Hızlı Başlangıç ile oluşturulan örnek uygulamasını nasıl çalışır?](media/quickstart-v2-javascript/javascriptspa-intro.png)

> [!div renderon="docs"]
> ## <a name="register-your-application-and-download-your-quickstart-app"></a>Uygulamanızı kaydedin ve hızlı başlangıç uygulamanızı indirin
>
> ### <a name="step-1-register-your-application"></a>1. adım: uygulamanızı kaydetme
>
> 1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app) bir uygulamayı kaydetme.
> 1. İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin.
> 1. Emin **destekli Kurulum** onay kutusunun temizlenmiş ve ardından **Oluştur**.
> 1. Tıklayın **Platform Ekle**, ardından **Web**.
> 1. Emin **izin örtük akış** olduğu *işaretli*.
> 1. Altında **yeniden yönlendirme URL'lerini** ekleme `http://localhost:30662/`.
> 1. **Kaydet**’e tıklayın.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. adım: uygulamanızı Azure portalında yapılandırma
> Çalışmak bu hızlı başlangıç için kod örneği için bir yeniden yönlendirme eklemeniz gereken URI olarak `http://localhost:30662/` ve etkinleştirme **örtük vermeyi**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Benim için şu değişiklikleri yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Önceden yapılandırılmış](media/quickstart-v2-javascript/green-check.png) uygulamanız bu özniteliklerle yapılandırılır

#### <a name="step-2-download-the-project"></a>2. adım: Proje indirme

Geliştirme ortamınız için uygun olan bu seçeneklerden birini seçebilirsiniz.
* [-Web sunucusu, Node.js gibi core proje dosyaları indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip)
* [Visual Studio projesini indirin](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip)

Zip dosyasını yerel bir klasöre ayıklar (örneğin, **C:\Azure-Samples**).

#### <a name="step-3-configure-your-javascript-app"></a>3. adım: JavaScript uygulamanızı yapılandırın

> [!div renderon="docs"]
> Düzen `index.html` değiştirin `Enter_the_Application_Id_here` altında `applicationConfig` yeni kaydettiğiniz uygulamayı uygulama kimliği.

> [!div class="sxs-lookup" renderon="portal"]
> Düzen `index.html` değiştirin `applicationConfig` ile:

```javascript
var applicationConfig = {
    clientID: "Enter_the_Application_Id_here",
    graphScopes: ["user.read"],
    graphEndpoint: "https://graph.microsoft.com/v1.0/me"
};
```
> [!NOTE]
>Kullanırsanız [Node.js](https://nodejs.org/en/download/), *server.js* dosya sunucusunun 30662 bağlantı noktası üzerinde dinleme başlatmak yapılandırılır.
> Kullanırsanız [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), kod örneği 's *.csproj* dosya sunucusunun 30662 bağlantı noktası üzerinde dinleme başlatmak yapılandırılır.
>

#### <a name="step-4-run-the-project"></a>4. adım: Proje çalıştırma

Bir komut satırında node.js kullanıyorsanız, sunucuyu başlatmak için proje dizininden aşağıdaki çalıştırabilirsiniz:
 ```batch
 npm install
 node server.js
 ```
Bir web tarayıcısı açın ve gidin `http://localhost:30662/`. Tıklayın **oturum** oturum başlatın ve ardından Microsoft Graph API'sini çağırmak için düğme.

Visual Studio kullanıyorsanız, proje çözümü seçin ve ENTER tuşuna basın emin olun **F5** projeyi çalıştırın.

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
var myMSALObj = new Msal.UserAgentApplication(applicationConfig.clientID, null, acquireTokenRedirectCallBack, {storeAuthStateInCookie: true, cacheLocation: "localStorage"});
```

> |Konum  |  |
> |---------|---------|
> |`ClientId`     |Azure portalında uygulama kimliği uygulamadan kayıtlı|
> |`authority`    |Yetkilisi URL'dir. Geçirme *null* varsayılan yetkilisi ayarlar `https://login.microsoftonline.com/common`. Uygulamanız tek kiracılı (bir dizinde hedefleme hesapları) ise, bu değeri ayarlamak `https://login.microsoftonline.com/<tenant name or ID>`|
> |`tokenReceivedCallback`| Kimlik doğrulaması uygulamaya geri yönlendirir sonra çağrılan geri çağırma yöntemi. Burada `acquireTokenRedirectCallBack` geçirilir. LoginPopup kullanıyorsanız bu yöntem null.|
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
> | `scopes`   | (İsteğe bağlı) Oturum açma zaman kullanıcı onayı için istenen kapsam içerir (örn: `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (yani `api://<Application ID>/access_as_user` ). Burada `applicationConfig.graphScopes` geçirilir. |

> [!TIP]
> Alternatif olarak kullanmak isteyebilirsiniz `loginRedirect` geçerli sayfa bir açılan pencere yerine oturum açma sayfasına yeniden yönlendirmek için yöntemi.


### <a name="request-tokens"></a>Belirteç isteme

MSAL belirteçlerini almak için kullanılan üç yöntem vardır: `acquireTokenRedirect`, `acquireTokenPopup` ve `acquireTokenSilent`:

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma

`acquireTokenSilent` Belirteç edinme ve herhangi bir kullanıcı etkileşimi olmadan yenileme yöntemi işler. Sonra `loginRedirect` veya `loginPopup` yöntemi ilk kez yürütüldüğünde `acquireTokenSilent` yapılan sonraki çağrılar için korunan kaynaklara erişim için kullanılan belirteçleri elde etmek için yaygın kullanılan yöntemdir. Veya belirteçleri yenileme isteği için çağrıları sessizce yapılır.

```javascript
myMSALObj.acquireTokenSilent(applicationConfig.graphScopes).then(function (accessToken) {
    // Callback code here
})
```

> |Konum  |  |
> |---------|---------|
> | `scopes`   | API için erişim belirtecinde döndürülecek istenen kapsam içerir (örn: `[ "user.read" ]` Microsoft Graph için veya `[ "<Application ID URL>/scope" ]` özel Web API'leri için (yani `api://<Application ID>/access_as_user` ). Burada `applicationConfig.graphScopes` geçirilir.|

#### <a name="get-a-user-token-interactively"></a>Etkileşimli bir kullanıcı belirteci alma

 Kullanıcıları, Azure AD v2.0 uç noktası ile etkileşim kurmak için gerek duyduğunuz senaryolara durumlar vardır. Örneğin:
* Kullanıcıların parolalarını süresi dolduğundan, kimlik bilgilerini yeniden girmeniz gerekebilir
* Uygulamanız, kullanıcıya onay için gereken ek kaynak kapsamlar için erişim izni isteme
* İki faktörlü kimlik doğrulaması gereklidir

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
> [Graph API'si öğreticisini çağırın](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-javascriptspa)

### <a name="browse-the-msal-repo-for-documentation-faq-issues-and-more"></a>MSAL depo belgeleri, SSS, sorunları ve daha fazlası için Gözat

> [!div class="nextstepaction"]
> [msal.js GitHub deposu](https://github.com/AzureAD/microsoft-authentication-library-for-js)


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
