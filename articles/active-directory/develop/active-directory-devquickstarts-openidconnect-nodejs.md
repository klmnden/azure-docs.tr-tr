---
title: Azure AD Node.js web uygulaması kullanmaya başlama | Microsoft Docs
description: Oturum açma için Azure AD ile tümleşen bir Node.js Express MVC web uygulaması oluşturmayı öğrenin.
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
ms.topic: article
ms.date: 04/20/2018
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 7aa48b65423db2a3af032ed64d9d571fa603668d
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144763"
---
# <a name="azure-ad-nodejs-web-app-getting-started"></a>Azure AD Node.js web uygulaması kullanmaya başlama
Burada Passport kullanın:

* Kullanıcı uygulamayı Azure Active Directory (Azure AD) ile oturum açın.
* Kullanıcı hakkındaki bilgileri görüntüler.
* Uygulama dışında bir kullanıcı oturum açabilir.

Passport Node.js için kimlik doğrulaması ara yazılımı. Esnek ve modüler özellikteki Passport sorunsuz bir şekilde her Express tabanlı bırakılabilir veya restify web uygulamasına. Dizi kapsamlı strateji, bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazla kullandığı kimlik doğrulama desteği. Microsoft Azure Active Directory için bir strateji geliştirdik. Bu modülü yükleyin ve ardından Microsoft Azure Active Directory Ekle `passport-azure-ad` eklenti.

Bunu yapmak için aşağıdaki adımları uygulayın:

1. Bir uygulamayı kaydetme.
2. Kullanmak için uygulamanızı ayarlayın `passport-azure-ad` stratejisi.
3. Azure AD'ye yönelik oturum açma ve oturum kapatma isteklerini yürütmek için Passport kullanın.
4. İlgili kullanıcı verilerini yazdırın.

Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS) korunur. Örneği takip etmek için [uygulamanın çatısını bir .zip dosyası indirme](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

De Bu öğretici sonunda tamamlanmış uygulama sağlanır.

## <a name="step-1-register-an-app"></a>1. adım: bir uygulamayı kaydetme
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sayfanın üst kısmındaki menüde hesabınızı seçin. Altında **dizin** listesinde, istediğiniz uygulamanızı kaydetmek için Active Directory kiracısı seçin.

3. Seçin **tüm hizmetleri** ekrana tıklayın ve ardından sol tarafındaki menüde **Azure Active Directory**.

4. Seçin **uygulama kayıtları**ve ardından **Ekle**.

5. Oluşturmak için istemleri izleyerek bir **Web uygulaması** ve/veya **Webapı**.
  * **Adı** uygulamanızı kullanıcılara uygulamayı açıklar.

  * **Oturum açma URL'si** uygulamanızın temel URL'si. Çatıyı'nın varsayılan `http://localhost:3000/auth/openid/return`.

6. Kaydolduktan sonra Azure AD uygulamanızı benzersiz bir uygulama kimliği atar. Bu değere ihtiyacınız aşağıdaki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
7. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. **Uygulama kimliği URI'si** uygulamanız için benzersiz bir tanımlayıcıdır. Kuralı biçimi kullanmaktır `https://<tenant-domain>/<app-name>`, örneğin: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Gelen **ayarları** -> **yanıt URL'leri** sayfasında uygulamanız için oturum açma URL'SİNDE adım 5'ten eklenen URL'sini ekleyin ve tıklayarak kaydedin.

9. Gizli bir anahtar oluşturmak için 4 adımda izleyin [uygulama kimlik bilgileri veya web API'lerine erişim izni eklemek için](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis).

   > [!IMPORTANT]
   > Uygulama anahtar değerini kopyalayın. Bu değer `clientSecret`, için gereken **3. adım** aşağıda. 

## <a name="step-2-add-prerequisites-to-your-directory"></a>2. adım: dizininize Önkoşullar ekleme
1. Komut satırından, zaten orada değilseniz kök klasörünüze yönelik dizinleri değiştirin ve ardından aşağıdaki komutları çalıştırın:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Ayrıca, gereksinim duyduğunuz `passport-azure-ad`:
    * `npm install passport-azure-ad`

Bu kitaplıklarını yükler, `passport-azure-ad` bağlıdır.

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. adım: passport düğüm js stratejisini kullanmak için uygulamanızı ayarlayın.
Burada, Express Openıd Connect kimlik doğrulama protokolünü kullanacak biçimde yapılandırın. Passport, sorunu oturum açma ve oturum kapatma istekleri dahil olmak üzere çeşitli şeyler, kullanıcının oturumunu yönetmek ve kullanıcı hakkında bilgi almak için kullanılır.

1. Başlamak için açık `config.js` dosya projenin kökünde ve ardından uygulamanızın yapılandırma değerlerini girin `exports.creds` bölümü.

  * `clientID` Olduğu **uygulama kimliği** kayıt portalında uygulamanıza atanan.

  * `returnURL` Olduğu **yanıt URL'si** portalda girdiğiniz.

  * `clientSecret` Portalda oluşturulan bir gizli dizidir.

2. Ardından, açık `app.js` projenin köküne dosya. Ardından çağırmak için aşağıdaki çağrıyı ekleyin `OIDCStrategy` birlikte strateji `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
        name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Bundan sonra biz bizim oturum açma isteklerini işlemek için az önce başvurduğunuz stratejiyi kullanın.

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
Passport, tüm strateji yazıcıları bağlı tüm stratejileri (Twitter, Facebook vb.) için benzer bir desen kullanır. Stratejisinde bakarak, biz ve parametreler olarak yapılan bir belirtece sahip bir işlev geçirdiğinizi bakın. Kendi iş yaptıktan sonra strateji geri ABD için gelir. Sonra kullanıcıyı depolayın ve belirteci kaydedin; böylece biz bunları yeniden istemeniz gerekmez istiyoruz.

> [!IMPORTANT]
Önceki kod, kimlik doğrulaması için Sunucumuz gerçekleşen herhangi bir kullanıcı alır. Bu, otomatik kayıt bilinir. Herhangi bir üretim sunucusuna bunları karar bir işlem aracılığıyla kayıt olmadan kimlik doğrulaması çok izin vermeyin öneririz. Bu genellikle Facebook ile kaydedebilirsiniz ancak ardından ek bilgileri vermeniz istenir olanak tanıyan tüketici uygulamalarında gördüğünüz desendir. Bu örnek bir uygulama oluyorum, kullanıcının e-posta adresi döndürdü ve ardından ek bilgileri doldurmanızı kullanıcı kullanıcıdan belirteç nesnesinden doldurmasını isteyebilirdik. Bu bir test sunucusu olduğundan bunları bellek içi veritabanına ekleriz.


4. Ardından, oturum açmış kullanıcıların Passport gerektirdiği şekilde izlemek bize olanak tanıyan yöntemler ekleyelim. Bu yöntemler, serileştirme ve seri kaldırma kullanıcının bilgilerini içerir.

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

5. Ardından, Express altyapısını yüklemek için kod ekleyelim. Burada varsayılan /views kullanırız ve Express /routes düzeni sağlar.

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

6. Son olarak, gerçek oturum açma istekleri için kapalı el rotaları ekleyelim `passport-azure-ad` altyapısı:

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


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. adım: Azure AD'de oturum açma ve oturum kapatma isteklerini yürütmek için Passport kullanın
Uygulamanız artık Openıd Connect kimlik doğrulama protokolü kullanarak uç noktası ile iletişim kurmak için düzgün şekilde yapılandırılmıştır. `passport-azure-ad` alınan kimlik doğrulama iletilerinde hazırlayın, Azure AD belirteçleri doğrulama ve kullanıcı oturumlarını sürdürme tüm ayrıntılarını ele aldı. Kalan tüm kullanıcılarınız oturum açın ve oturum kapatma için bir yol sağlar ve oturum açmış kullanıcılar hakkında ek bilgi toplamak.

1. İlk olarak, varsayılan, oturum açma, hesap ve oturum kapatma yöntemlerini ekleyelim bizim `app.js` dosyası:

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

2. Bu ayrıntılı inceleyelim:

  * `/`Rota (varsa) kullanıcı istekte geçirme index.ejs görünümüne yeniden yönlendirir.
  * `/account` İlk yol *biz yetkilendirilmesini sağlar* (biz uygulamak, aşağıdaki örnekte) ve böylece kullanıcı hakkında ek bilgi aldığımız kullanıcı istekte geçirir.
  * `/login` Rota çağırır bizim azuread openıdconnect kimlik doğrulayıcıdan `passport-azuread`. Başarılı olmazsa, kullanıcı geri /login için yönlendirir.
  * `/logout` Tanımlama bilgilerini temizler ve ardından kullanıcı index.ejs için geri döndürür. yalnızca çağrıların logout.ejs (ve rota).

3. Son bölümü için `app.js`, ekleyelim **EnsureAuthenticated** kullanılır yöntemi `/account`daha önce gösterildiği gibi.

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

4. Son olarak, sunucunun kendisini oluşturalım içinde `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a>5. adım: kullanıcı Web sitesinde görüntülemek için görünümler ve yollar Express'te oluşturun
Artık `app.js` tamamlandı. Sadece biz kullanıcıya, olarak işleme bilgileri gösteren görünümleri ve yollar eklemek ihtiyacımız `/logout` ve `/login` oluşturduğumuz yollar.

1. Kök dizin kısmında `/routes/index.js` yolunu oluşturun.

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Kök dizin kısmında `/routes/user.js` yolunu oluşturun.

    ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
    ```

 Bu istek kullanıcı varsa dahil olmak üzere bizim görünümlerine geçirin.

3. Kök dizin kısmında `/views/index.ejs` görünümünü oluşturun. Bu, bizim oturum açma ve oturum kapatma yöntemlerini çağırır ve hesap bilgilerini almak sağlıyor basit bir sayfadır. Koşullu kullanabiliriz bildirimi `if (!user)` istekte aracılığıyla geçirilen bir oturum açmış olan kullanıcının sahibiz kanıt kullanıcıdır.

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

4. Oluşturma `/views/account.ejs` biz ek bilgileri görüntüleyebilmek için kök dizin kısmında görüntülemek, `passport-azure-ad` kullanıcı isteğine koyulan.

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
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. Bu görünüm iyi bir düzen ekleyerek olalım. Oluştur ' / kök dizininin altındaki views/layout.ejs görünüm.

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

## <a name="next-steps"></a>Sonraki adımlar
Son olarak, derleme ve uygulamanızı çalıştırın. Çalıştırma `node app.js`ve ardından `http://localhost:3000`.

Kişisel bir Microsoft hesabı veya bir iş veya Okul hesabı kullanarak oturum açma ve kullanıcı kimliği/Account listesinde nasıl yansıtılır dikkat edin. Artık kullanıcıların hem kişisel ve iş/Okul hesapları ile kullanıcıların kimliğini doğrulayabilen endüstri standardı protokoller ile güvenli bir web uygulamasına sahipsiniz.

Tamamlanan örnek, başvuru için (yapılandırma değerleriniz olmadan) [.zip dosyası olarak sağlanır](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Alternatif olarak, Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Artık daha ileri seviyeli konulara geçebilirsiniz. Denemek isteyebilirsiniz:

[Web API'si Azure AD ile güvenli hale getirme](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
