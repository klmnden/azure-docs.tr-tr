---
title: "Azure Active Directory v2.0 Node.js web uygulaması oturum açma | Microsoft Docs"
description: "Kişisel bir Microsoft hesabı ve bir iş veya Okul hesabı kullanarak bir kullanıcı oturum açtığında bir Node.js web uygulaması oluşturmayı öğrenin."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 6d49c742f72440e22830915c90de009d9188db2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a>Oturum açma bir Node.js web uygulamasına ekleme

> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası ile çalışır. V2.0 uç noktası veya v1.0 uç nokta kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 

Bu öğreticide, aşağıdaki görevleri gerçekleştirmek için Passport kullanın:

* Bir web uygulaması, Azure Active Directory (Azure AD) ve v2.0 uç noktası kullanarak kullanıcı oturum.
* Kullanıcı hakkındaki bilgileri görüntüler.
* Uygulama dışında kullanıcı oturum açabilir.

**Passport**, Node.js için kimlik doğrulama ara yazılımıdır. Esnek ve modüler, Passport sorunsuz bir şekilde bırakılan hiçbir Express tabanlı veya restify web uygulamasına. Passport, bir dizi kapsamlı strateji kimlik doğrulamasını bir kullanıcı adı ve parola, Facebook, Twitter veya diğer seçenekleri kullanarak destekler. Azure AD için bir strateji geliştirdik. Bu makalede, modülünü yüklemek ve Azure AD eklemek nasıl gösteriyoruz `passport-azure-ad` eklentisi.

## <a name="download"></a>İndir
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs) korunur. Öğreticiyi izlemek için şunları yapabilirsiniz [uygulamanın çatısını bir .zip dosyası karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Bu öğretici sonunda tamamlanmış uygulama da alabilirsiniz.

## <a name="1-register-an-app"></a>1: bir uygulamayı Kaydet
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya izleyin [bu adımları ayrıntılı](active-directory-v2-app-registration.md) uygulama kaydetmek için. Emin olun:

* Kopya **uygulama kimliği** uygulamanıza atanmış. Bu öğretici için ihtiyacınız.
* Ekleme **Web** uygulamanız için platform.
* Kopya **yeniden yönlendirme URI'si** portalından. Varsayılan URI değeri kullanmalısınız `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-add-prerequisities-to-your-directory"></a>2: ön koşullar dizininize eklemek
Zaten orada değilseniz kök klasörünüze gitmek için dizinleri bir komut isteminde değiştirin. Aşağıdaki komutları çalıştırın:

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

Ayrıca, kullandığımız `passport-azure-ad` hızlı başlangıç çatısında önizlememiz:

* `npm install passport-azure-ad`

Bu kitaplıklar yükler, `passport-azure-ad` kullanır.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3: passport düğümü js stratejisi kullanmak için uygulamanızı ayarlayın
Openıd Connect kimlik doğrulama protokolünü kullanmak için Express ara yazılımını ayarlayın. Passport, oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcının oturumunu yönetmek ve kullanıcı, başka şeylerin hakkında bilgi almak için kullanın.

1.  Proje kök dizininde Config.js dosyasını açın. İçinde `exports.creds` bölümünde, uygulamanızın yapılandırma değerlerini girin.
  
  * `clientID`**Uygulama kimliği** Azure portalında uygulamanıza atanan.
  * `returnURL`**Yeniden yönlendirme URI'si** portalda girdiğiniz.
  * `clientSecret`: Portalda oluşturulan gizli anahtarı.

2.  Proje kök dizininde App.js dosyasını açın. İle birlikte OIDCStrategy stratey çağrılacak `passport-azure-ad`, aşağıdaki çağrıyı ekleyin:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  Oturum açma isteklerini işlemek için az önce başvurduğunuz stratejiyi kullanın:

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
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

Passport, tüm kendi stratejileri (Twitter, Facebook vb.) benzer bir desen kullanır. Tüm strateji yazıcıları desene bağlı kalır. Stratejisi geçirmek bir `function()` bir belirteç kullanan ve `done` parametre olarak. Tüm iş yaptıktan sonra strateji döndürülür. Kullanıcıyı depolayın ve böylece bunları yeniden istemeniz gerekmez belirteci kaydedin;.

  > [!IMPORTANT]
  > Önceki kod sunucunuza doğrulanabilir herhangi bir kullanıcı alır. Bu otomatik kaydı bilinir. Bir üretim sunucusunda herkes bunları seçtiğiniz bir kayıt sürecinden geçerler gerekmeden let istemezsiniz. Bu genellikle tüketici uygulamalarında görürsünüz düzeni olur. Uygulama, Facebook ile kaydetmenize olanak sağlayabilir, ancak ek bilgileri girmenizi ister. Bu öğretici için bir komut satırı programı doğru kullanıyorsanız, döndürülen belirteç nesnesinden e-posta ayıklanamıyor. Sonra ek bilgilerini girmesini isteyebilir. Bu bir test sunucusu olduğundan, bellek içi veritabanına doğrudan kullanıcı ekleyin.
  > 
  > 

4.  Oturum açtığınız kullanıcılar izlemek için kullandığınız yöntemleri ekleyin Passport'un gerektirdiği gibi. Bu seri hale getirme ve seri durumdan kullanıcının bilgileri içerir:

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  Express altyapısını yükler kodu ekleyin. Varsayılan /views kullanın ve Express /routes desen sağlar:

  ```JavaScript

  // Set up Express (section 2)

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

6.  POST yönlendirir o elde gerçek oturum açma istekleri için devre dışı eklemek `passport-azure-ad` altyapısı:

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4: Azure AD ile oturum açma ve oturum kapatma isteklerini yürütmek için Passport kullan
Uygulamanız artık Openıd Connect kimlik doğrulama protokolü kullanarak v2.0 uç noktası ile iletişim kurmak için ayarlanır. `passport-azure-ad` Stratejisi, kimlik doğrulama iletileri hazırlayın, Azure AD'den belirteçleri doğrulamak ve kullanıcı oturumunu sürdürme tüm ayrıntılarını mvc'deki. Tüm yapmak için sol etmektir kullanıcılarınız oturum açın ve oturumu kapatın ve oturum kullanıcının hakkında daha fazla bilgi toplamak için bir yol sağlar.

1.  Ekleme **varsayılan**, **oturum açma**, **hesap**, ve **oturum kapatma** App.js dosyanıza yöntemleri:

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
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  Ayrıntıları aşağıdadır:
    
    * `/` Rota index.ejs görünümüne yeniden yönlendirir. (Varsa) istek kullanıcı geçirir.
    * `/account` İlk yol *, kimlik doğrulaması yapmasını sağlar* (Bu aşağıdaki kodda uygulamanız). Ardından, kullanıcı istekte geçirir. Böylece kullanıcı hakkında daha fazla bilgi elde edebilirsiniz budur.
    * `/login` Rota çağrıları, `azuread-openidconnect` doğrulayıcıdan `passport-azuread`. Değil başarılı olursa, kullanıcının yeniden geri `/login`.
    * `/logout` Logout.ejs görüntüleyin (ve rota) yol çağırır. Bu tanımlama bilgilerini temizler ve ardından kullanıcıyı geri index.ejs döndürür.

2.  Ekleme **EnsureAuthenticated** daha önce kullanılan yöntem `/account`:

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  App.js içinde sunucu oluşturun:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a>5: kullanıcı Web sitesinde Göster Express'te yolları ve görünümleri oluşturma
Yollar ve kullanıcı bilgilerini göster görünümleri ekleyin. Yolları ve görünümleri de işlemek `/logout` ve `/login` oluşturduğunuz yollar.

1. Kök dizininde oluşturmak `/routes/index.js` rota.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  Kök dizininde oluşturmak `/routes/user.js` rota.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js`ve `/routes/user.js` isteği varsa kullanıcı da dahil olmak üzere, geçer basit yollar.

3.  Kök dizininde oluşturmak `/views/index.ejs` görünümü. Bu sayfa çağrıları, **oturum açma** ve **oturum kapatma** yöntemleri. Aynı zamanda `/views/index.ejs` hesap bilgileri yakalamak için görünümü. Koşullu kullanabilirsiniz `if (!user)` aracılığıyla istekte geçirilen kullanıcı olarak. Bu bulgu açtığınız kullanıcı sahip olur.

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  Kök dizininde oluşturmak `/views/account.ejs` görünümü. `/views/account.ejs` Görünümü sağlar, ek bilgileri görüntülemek, `passport-azuread` kullanıcı isteğine koyar.

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  Bir düzen ekleyin. Kök dizininde oluşturmak `/views/layout.ejs` görünümü.

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
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  Derleme ve uygulamanızı çalıştırmak için Çalıştır `node app.js`. Ardından, Git `http://localhost:3000`.

7.  Kişisel bir Microsoft hesabı veya bir iş veya Okul hesabınızla oturum açın. Kullanıcının kimliğini ApplicationTier/account listesinde yansıtılır unutmayın. 

Endüstri standardı protokoller kullanılarak güvenli bir web uygulaması şimdi sahipsiniz. Kişisel ve iş veya Okul hesaplarını kullanarak uygulamanızı kullanıcıların kimliklerini doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Başvuru için tamamlanan örnek (yapılandırma değerleriniz olmadan) olarak sağlanan [bir .zip dosyası](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). Aynı zamanda Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Ardından, size daha ileri seviyeli konulara geçebilirsiniz. Denemek isteyebilirsiniz:

[Node.js web API'si v2.0 uç noktası kullanarak güvenli hale getirme](active-directory-v2-devquickstarts-node-api.md)

Bazı ek kaynaklar aşağıda verilmiştir:

* [Azure AD v2.0 Geliştirici Kılavuzu](active-directory-appmodel-v2-overview.md)
* [Yığın taşması "azure-active-directory" etiketi](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
Güvenlik olayları oluştuğunda bildirim almak için kaydolun öneririz. Üzerinde [Microsoft Teknik Güvenlik bildirimleri](https://technet.microsoft.com/security/dd252948) sayfasında, güvenlik danışma uyarılara abone.

