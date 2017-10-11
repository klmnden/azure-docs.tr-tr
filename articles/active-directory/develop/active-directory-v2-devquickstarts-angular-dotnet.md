---
title: "Başlarken azure AD v2.0 .NET AngularJS tek sayfa uygulaması | Microsoft Docs"
description: "Hem kişisel Microsoft hesabı olan kullanıcılar oturum açtığında bir Açısal JS tek sayfa uygulaması oluşturma ve iş veya Okul hesapları."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 6a341781-278f-461b-92ca-7572a06e6852
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c68180c0ecabf5c0732f0db77ef1f3cc93be965b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a>Oturum açma bir AngularJS tek sayfalı uygulama için - .NET ekleyin
Bu makalede bir AngularJS uygulamasına Azure Active Directory v2.0 uç kullanarak oturum açın. desteklenen Microsoft hesapları ekleyeceğiz.  V2.0 uç noktası, uygulamanızda tek tümleştirme gerçekleştirmek ve hem kişisel hem de iş/Okul hesapları olan kullanıcıların kimlik doğrulaması sağlar.

Bu örnek görevleri arka uç REST API'si, .NET 4.5 MVC framework kullanılarak yazılmış ve Azure AD'den OAuth taşıyıcı belirteçlerini kullanarak güvenliği depolayan basit bir Yapılacaklar listesi tek sayfa uygulamasıdır.  AngularJS uygulama bizim açık kaynak JavaScript kimlik doğrulama kitaplığı kullanır [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) tüm oturum açma işlemine işlemek ve REST API çağırma belirteçleri almak için.  Aynı desende diğer REST API'leri gibi kimlik doğrulaması için uygulanabilir [Microsoft Graph](https://graph.microsoft.com).

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası tarafından desteklenir.  V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="download"></a>İndir
Başlamak için indir ve Visual Studio yüklemeniz gerekir.  Sonra kopyalayabilir veya [karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) iskelet uygulama:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

İskelet uygulama basit bir AngularJS uygulama için tüm Demirbaş kod içerir, ancak kimlikle ilgili şey eksik.  Takip istemiyorsanız, bunun yerine, kopyalayabilir veya [karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) tamamlanan örnek.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
İlk olarak, bir uygulama oluşturun [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya adımları izleyin: [ayrıntılı adımları](active-directory-v2-app-registration.md).  Emin olun:

* Ekleme **Web** uygulamanız için platform.
* Doğru girin **yeniden yönlendirme URI'si**. Bu örnek için varsayılan değer `https://localhost:44326/`.
* Bırakın **izin örtük akış** Etkin onay kutusu. 

Aşağı kopyalama **uygulama kimliği** uygulamanıza atanan, kısa süre içinde gerekir. 

## <a name="install-adaljs"></a>Adal.js yükleyin
Başlatmak için indirilen proje gidin ve adal.js yükleyin.  Varsa [bower](http://bower.io/) yüklüyse, bu komutu yalnızca çalıştırabilirsiniz.  Hiçbir bağımlılık sürümü uyuşmazlığı için yalnızca daha yüksek sürümünü seçin.

```
bower install adal-angular#experimental
```

Alternatif olarak, el ile yükleyebileceğiniz [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) ve [angular.js adal](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Her iki dosyalarına ekleme `app/lib/adal-angular-experimental/dist` dizininde `TodoSPA` projesi.

Şimdi projesini Visual Studio'da açın ve ana sayfa gövdesi sonunda adal.js yükleyin:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>REST API ayarlayın
Şimdi ayarlamaları olsa da, arka uç REST API çalışma alın.  Proje kök dizininde açın `web.config` ve değiştirme `audience` değeri.  REST API AJAX isteği Açısal uygulamadan aldığı belirteçleri doğrulamak için bu değeri kullanır.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

REST API nasıl çalıştığını ele harcamanız oluşturacağız her zaman olmasıdır.  Kodda karıştırın geçici bir çözüm, ancak Azure AD ile API'leri web güvenliğini sağlama hakkında daha fazla bilgi edinmek istiyorsanız kullanıma çekinmeyin [bu makalede](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>Kullanıcıların oturumunu açma
Bazı kimlik kod yazma süresi.  Zaten sorunsuz şekilde Açısal yönlendirme yöntemleriyle oynadığı bir AngularJS sağlayıcısı bu adal.js içeren fark etmiş olabilirsiniz.  Uygulama adal modülü ekleyerek başlayın:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Şimdi başlatabilir `adalProvider` uygulama kimliği:

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from the registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

Harika, artık adal.js uygulamanızı güvenli ve kullanıcılar oturum açmak için gerekli tüm bilgilere sahiptir.  Uygulamasında belirli bir rota için oturum açma zorlamak için gereken tek şey bir kod satırı:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Şimdi bir kullanıcı tıkladığında `TodoList` bağlantı adal.js otomatik olarak yeniden yönlendirme oturum açma için Azure ad gerekiyorsa.  Oturum açma ve oturum kapatma isteklerini denetleyicilerinizi adal.js çağırarak açıkça gönderebilirsiniz:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect the user to sign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect the user to log out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a>Kullanıcı bilgilerini görüntüle
Kullanıcının oturum açtığı, uygulamanızda oturum açmış kullanıcının kimlik doğrulaması veri erişim gerekecek.  Adal.js gösterir bu bilgileri `userInfo` nesnesi.  Bu nesne bir görünümde erişmek için ilk adal.js karşılık gelen denetleyicisi kök kapsamını ekleyin:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Doğrudan ele alabileceği sonra `userInfo` görünümünüzü nesnesinde: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Aynı zamanda `userInfo` kullanıcı veya oturum varsa belirlemek için nesne.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>REST API çağrısı
Son olarak, bazı belirteçleri almak ve oluşturma, okuma, güncelleştirme için REST API çağrısı ve görevleri silme zamanı geldi.  İyi ne tahmin?  Yapmanıza gerek yoktur *bir şey*.  Adal.js otomatik olarak ele alma, önbellekleme ve yenileme belirteçleri gerçekleştirir.  Bu ayrıca REST API'sine gönderdiğiniz giden AJAX istekleri için bu simgeleri ekleme dikkatli olun.  

Bu tam olarak nasıl çalışır? Sihirli sayesinde tümü olduğu [AngularJS dinleyiciler](https://docs.angularjs.org/api/ng/service/$http), giden ve gelen http iletileri dönüştürmek adal.js izin verir.  Ayrıca, adal.js penceresi AngularJS uygulama aynı uygulama Kimliğine yönelik belirteçleri kullanması gereken gibi tüm istekler aynı etki alanına göndermek varsayar.  Aynı uygulama kimliği hem Açısal uygulama ve NodeJS REST API kullandık nedeni budur.  Elbette, bu davranışı geçersiz kılabilir ve diğer REST API'leri için gerekirse - belirteçleri almak için adal.js söyleyin ancak basit bu senaryo için varsayılanları yapar.

Azure AD'den taşıyıcı belirteçlerini istekleri göndermek için ne kadar kolay olduğunu gösteren bir parçacığı aşağıda verilmiştir:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Tebrikler!  Azure AD tümleşik tek sayfa uygulaması tamamlanmıştır.  Şimdi, bir paketi alın.  Kullanıcıların kimliğini doğrulamak, güvenli bir şekilde arka uç Openıd Connect kullanarak REST API çağrısı ve kullanıcı hakkındaki temel bilgileri alın.  Kutudan çıktığında, kişisel bir Microsoft Account veya Azure AD'den bir iş/Okul hesabı olan herhangi bir kullanıcı destekler.  Uygulamayı çalıştırın ve bir tarayıcıda gidin `https://localhost:44326/`.  Kişisel bir Microsoft hesabı veya iş/Okul hesabı kullanarak oturum açın.  Görevler kullanıcının yapılacaklar listesine ekleyin ve oturumu kapatın.  Hesap diğer bir tür oturum açmayı deneyin. İş/Okul kullanıcıları oluşturmak için Azure AD kiracısı gerekiyorsa [edinebileceğinizi öğrenin burada](active-directory-howto-tenant.md) (boş).

V2.0 uç noktası hakkında bilgi almaya devam etmek için head dön bizim [v2.0 Geliştirici Kılavuzu](active-directory-appmodel-v2-overview.md).  Ek kaynaklar için gözden geçirin:

* [Azure-Samples github'da >>](https://github.com/Azure-Samples)
* [Yığın taşması üzerinde Azure AD >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* Azure AD belgelerinde bulunan [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.

