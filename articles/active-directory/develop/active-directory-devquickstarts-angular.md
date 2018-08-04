---
title: Azure AD AngularJS ile çalışmaya başlama | Microsoft Docs
description: Oturum açma için Azure AD ile tümleştirilir ve OAuth kullanarak Azure AD ile korunan API'leri çağıran bir AngularJS tek sayfa uygulaması oluşturmayı öğrenin.
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
ms.openlocfilehash: 0c7f6a0e447e3b48cdd1df684dc105ece1e98f66
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502058"
---
# <a name="azure-ad-angularjs-getting-started"></a>Azure AD AngularJS ile çalışmaya başlama

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) sağlar, tek sayfalı uygulamalar için oturum açma, oturum kapatma ve güvenli bir OAuth API eklemenize olanak sade ve çağırır. Ancak, uygulamalarınızı herhangi bir web API'si Azure AD, Office 365 API'lerini veya Azure API gibi korunmasına yardımcı olan Windows Server Active Directory hesaplarıyla kullanıcıların kimliğini doğrulamak ve etkinleştirir.

Bir tarayıcıda çalışan JavaScript uygulamaları için Azure AD Active Directory Authentication Library (ADAL) veya adal.js sağlar. Tek amacı, adal.js erişim belirteçlerini almak uygulamanızı kolaylaştırır sağlamaktır. Ne kadar kolay olduğunu göstermek için buraya bir Yapılacaklar listesi AngularJS uygulama, oluşturacağız:

* Kullanıcının uygulamaya kimlik sağlayıcısı olarak Azure AD kullanarak oturum açtığında.

* Kullanıcı hakkında bazı bilgiler görüntüler.
* Azure AD'den taşıyıcı belirteçlerini kullanarak güvenli bir şekilde uygulamanın için yapmak listesi API'sini çağırır.
* Uygulama dışında oturum açtığında.

Eksiksiz, çalışan uygulamayı oluşturmak için şunları yapmanız:

1. Uygulamanızı Azure AD'ye kaydedin.
2. ADAL'ı yükleyin ve tek sayfalı uygulamayı yapılandırma.
3. Tek sayfa uygulamasında güvenli sayfaları için ADAL'ı kullanın.

Başlamak için [uygulama çatıyı indirmeniz](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) veya [tamamlanan örnek indirme](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Ayrıca kullanıcılar oluşturur ve bir uygulamayı kaydetme Azure AD kiracısı gerekir. Bir kiracı yoksa [edinebileceğinizi öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-the-directorysearcher-application"></a>1. adım: DirectorySearcher uygulamayı kaydetme
Kullanıcıların kimliğini doğrulama ve belirteçleri almak üzere uygulamanızı etkinleştirme için ilk Azure AD kiracınıza kaydetmek gerekir:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Birden çok dizini için oturum doğru dizinde görüntülediğiniz emin olmak gerekebilir. Bunu, üst çubukta yapmak için hesabınızı'ı tıklatın. Altında **dizin** listesinde, istediğiniz uygulamanızı kaydetmek için Azure AD kiracısı seçin.
3. Tıklayın **tüm hizmetleri** sol bölmesi ve ardından **Azure Active Directory**.
4. Tıklayın **uygulama kayıtları**ve ardından **Ekle**.
5. Komut istemlerini izleyin ve yeni web uygulaması ve/veya web API oluşturun:
  * **Adı** uygulamanızı kullanıcılara açıklar.
  * **Oturum açma URL'si** istediğiniz Azure AD belirteçleri döndürecektir konumdur. Bu örnek için varsayılan konum `https://localhost:44326/`.
6. Kayıt işlemini tamamladıktan sonra Azure AD uygulamanız için benzersiz uygulama Kimliğidir atar. Bu değer gerekir sonraki bölümlerde, bu nedenle uygulama sekmesinden kopyalayın.
7. Adal.js OAuth örtük akış Azure AD ile iletişim kurmak için kullanır. Örtük akış, uygulamanız için etkinleştirmeniz gerekir:
  1. Uygulama tıklayıp **bildirim** satır içi bildirim düzenleyicisini açın.
  2. Bulun `oauth2AllowImplicitFlow` özelliği. Değerini ayarlamak `true`.
  3. Tıklayın **Kaydet** bildirimi kaydetmek için.
8. Uygulamanız için kiracınız genelinde izinleri verin. Git **ayarları** > **gerekli izinler**, tıklatıp **izin ver** üst çubukta düğmesi. Onaylamak için **Evet**’e tıklayın.

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a>2. adım: Yükleme ADAL ve tek sayfalı uygulamayı yapılandırma
Azure AD'de bir uygulamanız olduğuna göre adal.js yükleyebilir ve kimlikle ilgili kodunuzu yazın.

### <a name="configure-the-javascript-client"></a>JavaScript istemcisini yapılandırma
Paket Yöneticisi konsolu kullanarak adal.js TodoSPA projeye ekleyerek başlayın:
  1. İndirme [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) ve eklemeniz `App/Scripts/` proje dizini.
  2. İndirme [angular.js adal](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) ve eklemeniz `App/Scripts/` proje dizini.
  3. Her komut dosyası bitmeden önce yük `</body>` içinde `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a>Arka uç sunucusu yapılandırın
Tek sayfalı uygulamanın arka uç için yapmak liste tarayıcısından belirteçlerini kabul etmesini API için arka uç uygulama kaydı hakkında yapılandırma bilgilerini gerekir. TodoSPA projeyi `web.config`. Öğe değerlerini değiştirin `<appSettings>` Azure portalda kullanılan değerleri yansıtacak şekilde bölümü. ADAL kullandığında, kodunuzun bu değerleri başvurur.
  * `ida:Tenant` Azure AD kiracınızın--Örneğin, contoso.onmicrosoft.com etki alanıdır.
  * `ida:Audience` portaldan kopyaladığınız uygulama kimliğidir.

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a>3. adım: Kullanım tek sayfalı uygulama güvenli sayfalarında yardımcı olmak için ADAL
Tek sayfalı uygulamanızı güvenli tek bir görünüm yardımcı olabilmemiz Adal.js AngularJS yolu ve HTTP sağlayıcıları ile tümleştirilir.

1. İçinde `App/Scripts/app.js`, adal.js modülünde getirin:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Başlatma `adalProvider` uygulama kaydınızı yapılandırma değerlerini de kullanarak `App/Scripts/app.js`:

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
3. Korunmasına yardımcı olma `TodoList` tek satırlık bir kod kullanarak uygulamayı görünümünde: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Özet
Artık kullanıcılar oturum ve taşıyıcı belirteci korumalı istekleri arka uç API'si için sorun güvenli bir tek sayfalı uygulama vardır. Kullanıcı tıkladığında **TodoList** bağlantı adal.js otomatik olarak yönlendirir ve Azure ad oturum açma için gerekirse. Ayrıca, adal.js otomatik olarak bir erişim belirteci uygulamanın arka uca gönderilen tüm Ajax istekleri bağlanacağı. 

Önceki adal.js kullanarak tek sayfalı uygulama oluşturmak için tam en düşük gerekli adımlardır. Ancak bazı özellikler tek sayfalı uygulamada yararlıdır:

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
* Kullanıcı bilgilerini uygulamanın kullanıcı Arabiriminde sunmak isteyebilirsiniz. ADAL hizmeti için zaten eklenmiş `userDataCtrl` erişebilmesi için bu denetleyici `userInfo` ilişkili görünümünde, nesne `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Kullanıcı veya oturum açtıysa bilmek isteyebilirsiniz birçok senaryo vardır. Ayrıca `userInfo` bu bilgileri toplamak için nesne. Örneğin, `index.html`, ya da gösterebilirsiniz **oturum açma** veya **oturum kapatma** düğmesi tabanlı kimlik doğrulama durumu:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Azure AD ile tümleşik tek sayfalı uygulama kullanıcıların kimliğini doğrulamak, güvenli bir şekilde arka ucuna OAuth 2.0 kullanarak çağırmayı ve kullanıcı ile ilgili temel bilgileri alın. Henüz yapmadıysanız, kiracınızın bazı kullanıcılar ile doldurmak için zaman sunulmuştur. Yapılacaklar listesi tek sayfalı uygulamanızı çalıştırın ve oturum kullanıcılarla birini açın. Kullanıcının yapılacaklar listesini görevleri ekleyin, oturumu kapatın ve yeniden oturum açın.

Adal.js ortak kimlik özellikleri uygulamanıza eklemenize kolaylaştırır. Bu tüm kirli iş sizin için üstlenir: önbellek yönetimi, kullanıcı ve süresi dolan yenileme bir oturum açma kullanıcı Arabirimi ile sunma OAuth protokol desteği.

Başvuru için tamamlanmış bir örnek (olmadan yapılandırma değerlerinize) kullanılabilir [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Sonraki adımlar
Artık için ilave senaryolar da taşıyabilirsiniz. Denemek isteyebilirsiniz: [tek sayfalı bir uygulamadan CORS web API'si çağırma](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
