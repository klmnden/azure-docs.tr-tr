---
title: Azure AD AngularJS Başlarken | Microsoft Docs
description: Oturum açma için Azure AD ile tümleşir ve OAuth kullanarak Azure AD korumalı API çağıran bir AngularJS tek sayfalı uygulama oluşturma.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5b99ce605d9ecea6c7d67ab9a2ea679d531787d7
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34156971"
---
# <a name="azure-ad-angularjs-getting-started"></a>Azure AD AngularJS Başlarken

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) yapar daha basit ve kolay oturum açma, oturum kapatma ve güvenli OAuth API eklemeniz için tek sayfalı uygulamalarınızı çağırır. Uygulamalarınızı, Windows Server Active Directory hesaplarını ile kullanıcıların kimliklerini doğrulamak ve tüm web Azure AD, Office 365 API'leri veya Azure API'sini gibi korur API kullanmasına olanak sağlar.

Bir tarayıcıda çalışan JavaScript uygulamaları için Azure AD Active Directory Authentication Library (ADAL) veya adal.js sağlar. Tek amacı adal.js, erişim belirteçleri almak uygulamanızı daha kolay hale getirmektir. Ne kadar kolay olduğunu göstermek için burada size bir Yapılacaklar listesi AngularJS uygulama, yapı:

* Kullanıcı uygulamaya kimlik sağlayıcısı olarak Azure AD kullanarak oturum açtığında.

* Kullanıcı hakkındaki bazı bilgileri görüntüler.
* Güvenli bir şekilde Azure AD'den taşıyıcı belirteçlerini kullanarak uygulamanın için yapmak listesi API çağırır.
* Uygulama dışında kullanıcı açar.

Eksiksiz, çalışan uygulama oluşturmak için aktarmanız gerekir:

1. Uygulamanızı Azure AD ile kaydedin.
2. ADAL yükleyin ve tek sayfa uygulaması yapılandırın.
3. ADAL güvenli sayfalarında tek sayfalı uygulama yardımcı olması için kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Ayrıca kullanıcı oluşturma ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](active-directory-howto-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>1. adım: Kayıt DirectorySearcher uygulama
Kullanıcıların kimliğini doğrulamak ve belirteçleri almak uygulamanızı etkinleştirmek için önce Azure AD kiracınıza kaydetmeniz gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden çok dizin açmış durumdaysanız, doğru dizin görüntülediğinizden emin olmak gerekebilir. Üst çubuğunda Bunu yapmak için hesabınızı tıklatın. Altında **Directory** listesinde, Azure AD Kiracı uygulamanızı kaydetmek istediğiniz yeri seçin.
3. Tıklatın **tüm hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Komut istemlerini izleyin ve yeni web uygulaması ve/veya web API oluşturun:
  * **Ad** kullanıcılar uygulamanıza açıklar.
  * **URL oturum açma** için Azure AD belirteçleri döndürecektir konumdur. Bu örnek için varsayılan konum `https://localhost:44326/`.
6. Kayıt işlemini tamamladıktan sonra Azure AD benzersiz uygulama kimliği uygulamanıza atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Adal.js OAuth örtük akış Azure AD ile iletişim kurmak için kullanır. Uygulamanız için örtük akış etkinleştirmeniz gerekir:
  1. Uygulama tıklayın ve **bildirim** satır içi bildirim Düzenleyicisi'ni açın.
  2. Bulun `oauth2AllowImplicitFlow` özelliği. Değerini ayarlamak `true`.
  3. Tıklatın **kaydetmek** bildirimi kaydetmek için.
8. Uygulamanız için Kiracı arasında izinleri verin. Git **ayarları** > **gerekli izinler**, tıklatıp **izinler** düğmesi üst çubukta. Onaylamak için **Evet**’e tıklayın.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>2. adım: Yükleme ADAL ve tek sayfalı uygulama yapılandırma
Azure AD'de bir uygulamanız varsa, adal.js yükleyin ve kimlikle ilgili kodunuzu yazın.

### <a name="configure-the-javascript-client"></a>JavaScript istemci yapılandırma
Paket Yöneticisi konsolu kullanılarak adal.js TodoSPA projeye ekleyerek başlayın:
  1. Karşıdan [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) ve ekleyin `App/Scripts/` proje dizini.
  2. Karşıdan [angular.js adal](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) ve ekleyin `App/Scripts/` proje dizini.
  3. Her komut dosyası sonundan önce yük `</body>` içinde `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Arka uç sunucusu yapılandırın
Tek sayfalı uygulama arka uç için yapmak liste tarayıcısından belirteçleri kabul etmek API için arka uç uygulama kaydı hakkında yapılandırma bilgilerini gerekir. TodoSPA projeyi açın `web.config`. Öğeleri değerleri değiştirmek `<appSettings>` Azure portalında kullanılan değerleri yansıtacak şekilde bölümü. ADAL kullandığında kodunuzu bu değerleri başvurur.
  * `ida:Tenant` Azure AD kiracınıza--Örneğin, contoso.onmicrosoft.com etki alanıdır.
  * `ida:Audience` İstemci uygulamanızın portalından kopyalandığından kimliğidir.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>3. adım: Kullanım güvenli sayfalarında tek sayfalı uygulama yardımcı olmak için ADAL
Güvenli tek bir görünüm tek sayfalı uygulamanıza yardımcı olacak Adal.js AngularJS yolu ve HTTP sağlayıcıları ile tümleştirilir.

1. İçinde `App/Scripts/app.js`, adal.js modülünde getirin:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Initialize `adalProvider` yapılandırma değerlerini uygulama kaydınızı, da kullanarak `App/Scripts/app.js`:

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. Korunmasına yardımcı olma `TodoList` tek satırlık bir kod kullanarak uygulama görünümünde: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Özet
Artık kullanıcılar oturum ve taşıyıcı belirteci korumalı istekleri için arka uç API'si sorun güvenli bir tek sayfalı uygulama vardır. Bir kullanıcı tıkladığında **TodoList** bağlantı adal.js otomatik olarak yeniden yönlendirme oturum açma için Azure ad gerekiyorsa. Ayrıca, adal.js uygulamanızın arka ucuna gönderilen tüm Ajax istekleri için otomatik olarak bir erişim belirteci ekleyecek. 

Önceki adım adal.js kullanarak bir tek sayfalı uygulama oluşturmak için tam minimum gerekli değildir. Ancak bazı özellikler tek sayfalı uygulama yararlıdır:

* Açıkça oturum açma ve oturum kapatma isteklerini yürütmek için denetleyicilerinizi içinde adal.js çağırma işlevleri tanımlayabilirsiniz. İçinde `App/Scripts/homeCtrl.js`:

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* Kullanıcı bilgileri uygulamanın kullanıcı Arabiriminde sunmak isteyebilirsiniz. ADAL hizmeti için zaten eklenmiş `userDataCtrl` , erişebilmesi için denetleyici `userInfo` ilişkili görünüm nesnesinde `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Kullanıcı veya oturum açtıysa bilmek istediğiniz birçok senaryo vardır. Aynı zamanda `userInfo` bu bilgileri toplamak için nesne. Örneğin, `index.html`, ya da gösterebilir **oturum açma** veya **oturum kapatma** düğmesi dayalı kimlik doğrulama durumu:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Azure AD ile tümleşik tek sayfalı uygulama kullanıcıların kimliğini doğrulamak, güvenli bir şekilde arka uç OAuth 2.0 kullanarak çağırmayı ve kullanıcı hakkındaki temel bilgileri alın. Henüz yapmadıysanız, bazı kullanıcılar ile Kiracı doldurmak için zaman sunulmuştur. Yapılacaklar listesi tek sayfalı uygulamanızı çalıştırma ve kullanıcılarla biri ile oturum açın. Görevler kullanıcının yapılacaklar listesine eklemek, oturumu kapatın ve yeniden oturum açın.

Adal.js genel kimlik özellikleri uygulamanıza eklemenizi kolaylaştırır. Bu tüm dirty iş sizin için mvc'deki: önbellek yönetimi, belirteçlerin süresinin ve daha fazlasını yenilemeyi bir oturum açma kullanıcı Arabirimi ile kullanıcı sunan OAuth protokol desteği.

Başvuru için (yapılandırma değerleriniz olmadan) tamamlanan örnek kullanılabilir [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Sonraki adımlar
Şimdi ilave senaryolar da taşıyabilirsiniz. Denemek isteyebilirsiniz: [tek sayfalı uygulamasından CORS web API'si çağırma](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
