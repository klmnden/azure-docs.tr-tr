---
title: Azure Active Directory ile oturum açmak ve oturumu kapatmak için Node.js Express web uygulaması oluşturma | Microsoft Docs
description: Oturum açmak için Azure AD ile tümleştirilen bir Node.js Express MVC web uygulaması oluşturmayı öğrenin.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 19563f76c261fda1fca53babcb553f2dceeaa345
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990102"
---
# <a name="quickstart-build-a-nodejs-express-web-app-for-sign-in-and-sign-out-with-azure-active-directory"></a>Hızlı başlangıç: Azure Active Directory ile oturum açmak ve oturumu kapatmak için Node.js Express web uygulaması oluşturma

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Passport, Node.js için kimlik doğrulama ara yazılımıdır. Esnek ve modüler özellikteki Passport, Express tabanlı veya Restify web uygulamasına sorunsuz bir şekilde yüklenebilir. Bir dizi kapsamlı strateji; bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler. Azure Active Directory (Azure AD) için bu modülü yükleyip Azure AD `passport-azure-ad` eklentisini ekleyeceğiz.

Bu hızlı başlangıçta Passport'u kullanarak şunları yapmayı öğreneceksiniz:

* Kullanıcının Azure AD ile uygulamada oturum açmasını sağlama.
* Kullanıcıyla ilgili bilgileri görüntüleme.
* Kullanıcının uygulama oturumunu kapatma.

Eksiksiz, çalışan bir uygulama oluşturmak için şunları yapmalısınız:

1. Uygulamayı kaydetme.
2. Uygulamanızı `passport-azure-ad` stratejisini kullanacak şekilde ayarlama.
3. Azure AD'ye yönelik oturum açma ve oturum kapatma isteklerini yürütmek için Passport kullanın.
4. Kullanıcıyla ilgili verileri yazdırma.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şu önkoşulları tamamlayın:

* [Uygulamanın çatısını bir .zip dosyası olarak indirin](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip)
  
    * Çatıyı kopyalayın:

        ```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

    Bu hızlı başlangıç için kod [GitHub'da](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS) korunur. Tamamlanmış uygulama bu hızlı başlangıcın sonunda da sağlanır.

* Altında kullanıcıları oluşturabileceğiniz ve uygulamayı kaydedebileceğiniz bir Azure AD kiracınız olsun. Henüz kiracınız yoksa, [nasıl alabileceğinizi öğrenin](quickstart-create-new-tenant.md).

## <a name="step-1-register-an-app"></a>1. Adım: Uygulama kaydetme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sayfanın en üstündeki menüden hesabınızı seçin. **Dizin** listesi altında, uygulamanızı kaydetmek istediğiniz Azure Active Directory kiracısını seçin.
1. Ekranın sol tarafındaki menüde **Tüm hizmetler**'i ve ardından **Azure Active Directory**'yi seçin.
1. **Uygulama kayıtları**'nı ve ardından **Ekle**'yi seçin.
1. İstemleri izleyerek bir **Web Uygulaması** ve/veya **Web API'si** oluşturun.

    * **Ad**, uygulamanın adıdır ve uygulamanızı kullanıcılara açıklar.
    * **Oturum Açma URL'si** uygulamanızın temel URL'sidir. Çatının varsayılan değeri: `http://localhost:3000/auth/openid/return`.

1. Kaydolduktan sonra, Azure AD uygulamanıza benzersiz bir uygulama kimliği atar. Bu değeri aşağıdaki bölümlerde kullanacağınız için uygulama sayfasından kopyalayın.
1. Uygulamanızın **Ayarlar > Özellikler** sayfasında, Uygulama Kimliği URI'sini güncelleştirin. 
    
    **Uygulama Kimliği URI'si** uygulamanızın benzersiz tanıtıcısıdır. Genel kural olarak `https://<tenant-domain>/<app-name>` biçimi kullanılır, örneğin: `https://contoso.onmicrosoft.com/my-first-aad-app`.

1. Uygulamanızın **Ayarlar > Yanıt URL'leri** sayfasında 5. adımdaki Oturum Açma URL'sini ekleyin ve **Kaydet**'i seçin.
1. Gizli anahtar oluşturmak için [Web API'lerine erişmek için uygulama kimlik bilgileri veya izinleri ekleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis) bölümündeki 4. adımı uygulayın.

   > [!IMPORTANT]
   > Uygulama anahtarı değerini kopyalayın. Bu değer `clientSecret` için gerekir ve aşağıdaki **3. Adımda** kullanmanız gerekecektir. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>2. Adım: Dizininize önkoşullar ekleme

1. Zaten orada değilseniz komut satırından kök klasörünüze geçin ve şu komutları çalıştırın:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

1. `passport-azure-ad` değerine de ihtiyacınız olacağı için şu komutu çalıştırın:

    * `npm install passport-azure-ad`

Bu komut `passport-azure-ad` öğesinin bağlı olduğu kitaplıkları yükler.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. Adım: Uygulamanızı passport-node-js stratejisini kullanacak şekilde ayarlama

Burada Express'i OpenID Connect kimlik doğrulama protokolünü kullanacak şekilde yapılandıracaksınız. Passport oturum açma ve oturumu kapatma istekleri, kullanıcı oturumunu yönetme ve kullanıcı hakkında bilgi alma gibi birçok görev için kullanılır.

1. Proje kökündeki `config.js` dosyasını açın ve `exports.creds` bölümüne uygulamanızın yapılandırma değerlerini girin.

    * `clientID`, kayıt portalında uygulamanıza atanan **Uygulama Kimliğidir**.
    * `returnURL`, portala girdiğiniz **Yanıt URL'si** değeridir.
    * `clientSecret`, portalda oluşturduğunuz gizli dizidir.

1. Şimdi proje kökündeki `app.js` dosyasını açın. Ardından `passport-azure-ad` ile birlikte gelen `OIDCStrategy` stratejisini çağırmak için aşağıdaki çağrıyı ekleyin.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

1. Daha sonra oturum açma isteklerini işlemek için az önce başvurduğumuz stratejiyi kullanın.

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
        process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
            if (err) {
            return done(err);
            }
            if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
            }
            return done(null, user);
        });
        });
    }
    ));
    ```
   Passport, tüm stratejiler (Twitter, Facebook vb.) için tüm strateji yazarlarının da benimsediği benzer bir düzen kullanır. Stratejiyi incelediğinizde, stratejiye parametre olarak belirtece ve tamamlandı bilgisine sahip olan bir işlev geçirdiğinizi görebilirsiniz. Strateji işlemi tamamladıktan sonra bize geri döner. Ardından yeniden istememize gerek kalmaması için kullanıcıyı depolamak ve belirteci kaydetmek isteriz.

   > [!IMPORTANT]
   > Yukarıdaki kod, kimlik doğrulaması yapan kullanıcıyı sunucumuza yönlendirir. Bu işlem, otomatik kimlik doğrulama olarak da bilinir. Kendi belirleyeceğiniz bir işlemi kullanarak kaydolmalarını sağlamadığınız kimseyi üretim sunucunuzda kimlik doğrulamasından geçirmenizi önermeyiz. Bu deseni genelde tüketici uygulamalarında görürsünüz. Bu uygulamalara Facebook kullanarak kaydolursunuz ancak daha sonra sizden bazı ek bilgiler sağlamanızı isterler. Bu bir örnek uygulama olmasaydı döndürülen belirteç nesnesinden e-posta adresini ayıklayabilir ve ardından kullanıcıdan ek bilgileri doldurmasını isteyebilirdik. Bu bir test sunucusu olduğundan bunları yalnızca bellek içi veritabanına ekliyoruz.

1. Passport için gereken oturum açan kullanıcıları takip etmemizi sağlayan metotları ekleyin. Bu metotlar kullanıcı bilgilerini serileştirme ve seri durumdan çıkarmayı kapsar.

    ```JavaScript
    // Passport session setup. (Section 2)

    //   To support persistent sign-in sessions, Passport needs to be able to
    //   serialize users into the session and deserialize them out of the session. Typically,
    //   this is done simply by storing the user ID when serializing and finding
    //   the user by ID when deserializing.
    passport.serializeUser(function(user, done) {
        done(null, user.email);
    });

    passport.deserializeUser(function(id, done) {
        findByEmail(id, function (err, user) {
            done(err, user);
        });
    });

    // array to hold signed-in users
    var users = [];

    var findByEmail = function(email, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            log.info('we are using user: ', user);
            if (user.email === email) {
                return fn(null, user);
            }
        }
        return fn(null, null);
    };
    ```

1. Express altyapısını yüklemek için kodu ekleyin. Burada Express tarafından sunulan varsayılan /views ve /routes desenini kullanıyoruz.

    ```JavaScript
    // configure Express (section 2)

      var app = express();
      app.configure(function() {
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.logger());
      app.use(express.methodOverride());
      app.use(cookieParser());
      app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
      app.use(bodyParser.urlencoded({ extended : true }));
      // Initialize Passport!  Also use passport.session() middleware, to support
      // persistent login sessions (recommended).
      app.use(passport.initialize());
      app.use(passport.session());
      app.use(app.router);
      app.use(express.static(__dirname + '/../../public'));
    });
    ```

1. Gerçek oturum açma isteklerini `passport-azure-ad` altyapısına devreden yolları ekleyin:

    ```JavaScript
    // Our Auth routes (section 3)

    // GET /auth/openid
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. The first step in OpenID authentication involves redirecting
    //   the user to their OpenID provider. After authenticating, the OpenID
    //   provider redirects the user back to this application at
    //   /auth/openid/return.
    app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
        log.info('Authentication was called in the Sample');
        res.redirect('/');
    });

    // GET /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.get('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });

    // POST /auth/openid/return
    //   Use passport.authenticate() as route middleware to authenticate the
    //   request. If authentication fails, the user is redirected back to the
    //   sign-in page. Otherwise, the primary route function is called,
    //   which, in this example, redirects the user to the home page.
    app.post('/auth/openid/return',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('We received a return from AzureAD.');
        res.redirect('/');
      });
    ```

## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Adım: Azure AD'de oturum açma ve oturum kapatma isteklerini yürütmek için Passport'u kullanma

Uygulamanız artık OpenID Connect kimlik doğrulama protokolü kullanarak uç noktası ile iletişim kurmak için düzgün şekilde yapılandırıldı. `passport-azure-ad` kimlik doğrulama iletileri oluşturma, Azure AD kaynaklı belirteçleri doğrulama ve kullanıcı oturumlarının bakımını yapma işlemlerinin tüm ayrıntılarını ele aldı. Geriye yalnızca kullanıcılarınıza oturum açma ve oturum kapatma ile oturum açan kullanıcılar hakkında ek bilgiler toplama için bir yol sağlamak kaldı.

1. `app.js` dosyanıza varsayılan, oturum açma, hesap ve oturum kapatma yöntemlerini ekleyin:

    ```JavaScript
    //Routes (section 4)

    app.get('/', function(req, res){
      res.render('index', { user: req.user });
    });

    app.get('/account', ensureAuthenticated, function(req, res){
      res.render('account', { user: req.user });
    });

    app.get('/login',
      passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
      function(req, res) {
        log.info('Login was called in the Sample');
        res.redirect('/');
    });

    app.get('/logout', function(req, res){
      req.logout();
      res.redirect('/');
    });
    ```

1. Bunları ayrıntılı olarak gözden geçirin:

    * `/` yolu, (eğer varsa) istek içindeki kullanıcıyı geçirerek index.ejs görünümüne yeniden yönlendirir.
    * `/account` yolu öncelikle *kimlik doğrulamasından geçtiğimizden emin olur* (bunu aşağıdaki örnekte uygulayacağız) ve ardından istekte kullanıcıyı ileterek kullanıcı hakkında ek bilgi almamızı sağlar.
    * `/login` yolu `passport-azuread` kaynağından azuread-openidconnect kimlik doğrulayıcımızı çağırır. Başarılı olmazsa yol kullanıcıyı /login konumuna yeniden yönlendirir.
    * `/logout` yolu yalnızca logout.ejs (ve yol) çağrısı yaparak tanımlama bilgilerini siler ve kullanıcıyı index.ejs konumuna geri gönderir.

1. `app.js` kodunun son bölümde önceden gösterildiği gibi `/account` içinde kullanılan **EnsureAuthenticated** metodunu ekleyin.

    ```JavaScript
    // Simple route middleware to ensure user is authenticated. (section 4)

    //   Use this route middleware on any resource that needs to be protected. If
    //   the request is authenticated (typically via a persistent sign-in session),
    //   the request proceeds. Otherwise, the user is redirected to the
    //   sign-in page.
    function ensureAuthenticated(req, res, next) {
      if (req.isAuthenticated()) { return next(); }
      res.redirect('/login')
    }
    ```

1. Son olarak, sunucunun kendisini `app.js` içinde oluşturun:

    ```JavaScript
    app.listen(3000);
    ```

## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>5. Adım: Kullanıcımızı web sitesinde görüntüleme, Express'te görünümleri ve yolları oluşturma

`app.js` tamamlandı. Şimdi yapmanız gereken kullanıcıya sunulan yollara ve görünümleri eklemenin yanı sıra oluşturduğumuz `/logout` ve `/login` yollarını işlemek.

1. Kök dizin kısmında `/routes/index.js` yolunu oluşturun.

    ```JavaScript
    /*
     * GET home page.
     */

    exports.index = function(req, res){
      res.render('index', { title: 'Express' });
    };
    ```

1. Kök dizin kısmında `/routes/user.js` yolunu oluşturun.

    ```JavaScript
    /*
     * GET users listing.
     */

    exports.list = function(req, res){
      res.send("respond with a resource");
    };
    ```

    Bunlar varsa kullanıcı da dahil olmak üzere isteği görünümlerimize iletir.

1. Kök dizin kısmında `/views/index.ejs` görünümünü oluşturun. Bu, oturum açma ve oturumu kapatma metotlarımızı çağıran ve hesap bilgilerini almamız sağlayan basit bir web sayfasıdır. İstek ile gönderilen kullanıcı, oturum açmış bir kullanıcı olduğunu belirttiğinden `if (!user)` koşullu değerini kullanabiliriz.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

1. `passport-azure-ad` tarafından kullanıcı isteğine koyulan ek bilgileri görüntüleyebilmek için kök dizin kısmında `/views/account.ejs` görünümü oluşturun.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
    <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

1. Sayfanın görünümünü değiştirmek için bir düzen ekleyin. Kök dizin kısmında `/views/layout.ejs` görünümünü oluşturun.

    ```HTML

    <!DOCTYPE html>
    <html>
        <head>
            <title>Passport-OpenID Example</title>
        </head>
        <body>
            <% if (!user) { %>
                <p>
                <a href="/">Home</a> |
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

## <a name="step-6-build-and-run-your-app"></a>6. Adım: Uygulamanızı derleme ve çalıştırma

1. `node app.js` dosyasını çalıştırıp `http://localhost:3000` adresine gidin.
1. Kişisel Microsoft hesabınız ya da iş veya okul hesabıyla oturum açın.

    Kullanıcı kimliğinin /account listesinde gösterildiğine dikkat edin. Artık kullanıcılarınızın kimliğini hem kişisel hem de iş/okul hesaplarıyla doğrulayabileceğiniz sektör standardı protokollerle güvenlik sağlayan bir web uygulamanız var.

    Tamamlanan örnek, başvuru için (yapılandırma değerleriniz olmadan) [.zip dosyası olarak sağlanır](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/master.zip). Alternatif olarak Github'dan da kopyalayabilirsiniz:

    ```git clone --branch master https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

## <a name="next-steps"></a>Sonraki adımlar

Artık diğer senaryoları deneyebilirsiniz:

> [!div class="nextstepaction"]
> [Azure AD ile bir Web API'sinin güvenliğini sağlama](quickstart-v1-nodejs-webapi.md)