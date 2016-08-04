<properties
    pageTitle="Azure B2C önizlemesi için Node.js web uygulamasına oturum açma ekleme | Microsoft Azure"
    description="B2C kiracısı kullanarak kullanıcı oturumu açan Node.js web uygulaması oluşturma."
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="brandwe"/>


# B2C önizlemesi: Node.js web uygulamasına oturum açma ekleme

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

**Passport**, Node.js için kimlik doğrulama ara yazılımıdır. Son derece esnek ve modüler olan Passport, Express tabanlı veya Restify web uygulamasına sorunsuz bir şekilde yüklenebilir. Bir dizi kapsamlı strateji; bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler. 

> [AZURE.NOTE] Bu makale, Azure Active Directory B2C kullanarak oturum açma, kaydolma ve profil yönetimi uygulama konularını kapsamaz. Kullanıcının kimliği doğrulandıktan sonra web API'leri çağırmaya odaklanır. Henüz başlamadıysanız Azure AD B2C ile ilgili temel bilgileri öğrenmek için [.NET web uygulaması ile çalışmaya başlama öğreticisini](active-directory-b2c-devquickstarts-web-dotnet.md) kullanarak başlamanız gerekir.

Azure Active Directory (Azure AD) için bir strateji geliştirdik. Bu modülü yükleyin ve ardından Azure AD `passport-azure-ad` eklentisini ekleyin.

Bunu yapmanız için gerekenler:

1. Bir uygulamayı Azure AD kullanarak kaydedin.
2. `passport-azure-ad` eklentisini kullanmak için uygulamanızı ayarlayın.
3. Azure AD'ye yönelik oturum açma ve oturum kapatma isteklerini yürütmek için Passport kullanın.
4. Kullanıcı verilerini yazdırın.

Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS). İzlemek için [uygulamanın çatısını bir .zip dosyası olarak indirebilirsiniz](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip). Ayrıca çatıyı kopyalayabilirsiniz:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

Bu öğretici sonunda tamamlanmış uygulama sağlanır.

> [AZURE.WARNING]   Bu B2C önizlemesi için hem web API'si görev sunucusu hem de ona bağlanan istemciye yönelik aynı **İstemci Kimliği**/**Uygulama Kimliği** ve ilkelerini kullanmanız gerekir. Ayrıca bu, iOS ve Android öğreticileri için de geçerlidir. Bu Hızlı Başlangıçların biriyle daha önce bir uygulama oluşturduysanız bu değerleri kullanın ve yeni değerler oluşturmayın.

## Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir.  Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır. Henüz yoksa devam etmeden önce bu kılavuzda [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## Uygulama oluşturma

Ardından B2C dizininizde uygulama oluşturmanız gerekir. Bu, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir. Tek bir mantıksal uygulama içerdikleri için hem istemci uygulaması hem de web API'si tek bir **Uygulama Kimliği** ile temsil edilir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

- Uygulamaya bir **web uygulaması**/**web API'si** ekleyin.
- **Yanıt URL'si** olarak `http://localhost/TodoListService` adresini girin. Bu URL, bu kod örneği için varsayılan URL'dir.
- Uygulamanız için bir **Uygulama gizli anahtarı** oluşturun ve bunu kopyalayın. Buna daha sonra ihtiyacınız olacak. Kullanmadan önce bu değerin [XML kaçışlı](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) olması gerektiğini unutmayın.
- Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna da daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama üç kimlik deneyimi içerir: Kaydolma, oturum açma ve Facebook ile oturum açma. Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) tanımlanan şekilde bir ilke oluşturmanız gerekir. Üç ilkenizi oluştururken şunları yaptığınızdan emin olun:

- Kaydolma ilkenizde **Görünen adı** ve diğer kaydolma özniteliklerini seçin.
- Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin. Diğer talepleri de seçebilirsiniz.
- Oluşturduktan sonra her ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adlarına daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi oluşturduktan sonra, uygulamanızı oluşturmaya hazırsınız.

Bu makalenin, henüz oluşturduğunuz ilkeleri nasıl kullanacağınıza yönelik bilgileri kapsamadığını unutmayın. İlkelerin Azure AD B2C'de nasıl çalıştığını öğrenmeye [.NET web uygulaması ile çalışmaya başlama öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md) ile başlayın.

## Dizininize önkoşullar ekleme

Zaten orada değilseniz kök klasörünüze yönelik dizinleri komut satırından değiştirin. Aşağıdaki komutları çalıştırın:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

Ayrıca Hızlı Başlangıç çatısında önizlememiz için `passport-azure-ad` kullandık.

- `npm install passport-azure-ad`

Bu, `passport-azure-ad` öğesinin bağlı olduğu kitaplıkları yükleyecek.

## Uygulamanızı Passport Node.js stratejisi kullanmak için ayarlama
OpenID Connect kimlik doğrulama protokolünü kullanmak için Express ara yazılımını yapılandırın. Passport, oturum açma ve oturum kapatma isteklerini yürütmek, kullanıcı oturumlarını yönetmek ve diğer eylemler boyunca kullanıcılar hakkında bilgiler almak için kullanılacak.

Proje kökündeki `config.js` dosyasını açın ve `exports.creds` bölümüne uygulamanızın yapılandırma değerlerini girin.
- `clientID`: Kayıt portalında uygulamanıza atanan **Uygulama Kimliği**.
- `returnURL`: Portalda girdiğiniz **Yeniden Yönlendirme URI'si**.
- `tenantName`: Uygulamanızın kiracı adı; örneğin, **contoso.onmicrosoft.com**.

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Proje kökündeki `app.js` dosyasını açın. `passport-azure-ad` ile birlikte gelen `OIDCStrategy` stratejisini çağırmak için aşağıdaki çağrıyı ekleyin.


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

Oturum açma isteklerini işlemek için az önce başvurduğunuz stratejiyi kullanın.

```JavaScript
// Use the OIDCStrategy in Passport (Section 2).
//
//   Strategies in Passport require a "validate" function that accepts
//   credentials (in this case, an OpenID identifier), and invokes a callback
//   by using a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    tenantName: config.creds.tenantName
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
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
Passport, tüm stratejileri (Twitter ve Facebook dahil) için benzer bir desen kullanır. Tüm strateji yazıcıları bu desene bağlı kalır. Stratejiyi incelediğinizde, stratejiye parametre olarak bir `done` ve belirtece sahip `function()` öğesini geçirdiğinizi görebilirsiniz. Strateji, tüm işini tamamladıktan sonra size geri gelir. Kullanıcıyı depolayın ve belirteci kaydedin; böylece bunları yeniden istemeniz gerekmez.

> [AZURE.IMPORTANT]
Önceki kod, sunucu tarafından kimliği doğrulanan tüm kullanıcıları alır. Bu otomatik kayıttır. Üretim sunucularını kullandığınızda, önceden ayarladığınız kayıt işleminden geçmedilerse kullanıcıların girişine izin vermek istemezsiniz. Tüketici uygulamalarında bu deseni sıkça görebilirsiniz. Bu uygulamalar, Facebook kullanarak kaydolmanıza izin verir ancak ardından ek bilgileri doldurmanızı ister. Uygulamamız bir örnek olmasaydı, döndürülen belirteç nesnesinden e-posta adresi ayıklayabilir ve ardından kullanıcıdan ek bilgileri doldurmasını isteyebilirdik. Bu bir test sunucusu olduğundan kullanıcıları yalnızca bellek içi veritabanına ekleriz.

Passport'un gerektirdiği gibi oturum açan kullanıcıları izlemenize olanak sağlayan yöntemleri ekleyin. Bu, kullanıcı bilgilerini serileştirmeyi ve seri durumdan çıkarmayı kapsar:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent sign-in sessions, Passport needs to be able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing the user ID when Passport serializes a user
//   and finding the user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array to hold users who have signed in
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

Express altyapısını yüklemek için kodu ekleyin. Aşağıda Express'in sağladığı varsayılan `/views` ve `/routes` desenini kullandığımızı görebilirsiniz.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware to support
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

Gerçek oturum açma isteklerini `passport-azure-ad` altyapısına devreden `POST` yollarını ekleyin:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request. The first step in OpenID authentication involves redirecting
//   the user to an OpenID provider. After the user is authenticated,
//   the OpenID provider redirects the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it redirects the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request. If authentication fails, the user will be redirected back to the
//   sign-in page. Otherwise, the primary route function will be called.
//   In this example, it will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## Azure AD'de oturum açma ve oturum kapatma isteklerini yürütmek için Passport'u kullanın.

Uygulamanız artık OpenID Connect kimlik doğrulama protokolü kullanarak v2.0 uç noktası ile iletişim kurmak için düzgün şekilde yapılandırıldı. `passport-azure-ad` kimlik doğrulama iletileri oluşturma, Azure AD kaynaklı belirteçleri doğrulama ve kullanıcı oturumunun bakımını yapma işlemlerinin ayrıntılarını ele aldı. Yalnızca kullanıcılarınıza oturum açma ve oturum kapatma ile oturum açan kullanıcılar hakkında ek bilgiler toplama için bir yol sağlamanız kaldı.

Öncelikle `app.js` dosyanıza varsayılan, oturum açma, hesap ve oturum kapatma yöntemlerini ekleyin:

```JavaScript

//Routes (Section 4)

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

Bu yöntemleri ayrıntılı olarak incelemek için:
- `/` yolu, (eğer varsa) istek içindeki kullanıcıyı geçirerek `index.ejs` görünümüne yeniden yönlendirir.
- `/account` yolu öncelikle kimlik doğrulamanızın yapıldığını doğrular (bu işlemin uygulanma şekli aşağıda verilmiştir). Ardından istekteki kullanıcıyı geçirir, böylece kullanıcı hakkında ek bilgiler alabilirsiniz.
- `/login` yolu `passport-azure-ad` kaynağından`azuread-openidconnect` kimlik doğrulayıcısını çağırır. Başarılı olmazsa yol kullanıcıyı `/login` konumuna yeniden yönlendirir.
- `/logout` yalnızca `logout.ejs` öğesini (ve yolunu) çağırır. Bu işlem, tanımlama bilgilerini temizler ve kullanıcıyı `index.ejs` konumuna yeniden yönlendirir.


`app.js` öğesinin son bölümü için `/account` yolunda kullanılan `EnsureAuthenticated` yöntemini ekleyin.

```JavaScript

// Simple route middleware to ensure that the user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected. If
//   the request is authenticated (typically via a persistent sign-in session),
//   then the request will proceed. Otherwise, the user will be redirected to the
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

Son olarak, sunucunun kendisini `app.js` içinde oluşturun.

```JavaScript

app.listen(3000);

```


## İlkelerinizi çağırmak için Express içinde görünümler ve yollar oluşturma

Şimdi `app.js` öğeniz tamamlandı. Sadece oturum açma ve oturum kapatma ilkelerini çağırmanıza olanak sağlayan yolları ve görünümleri eklemeniz gerekli. Bunlar ayrıca oluşturduğunuz `/logout` ve `/login` yollarını da işler.

Kök dizin kısmında `/routes/index.js` yolunu oluşturun.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

Kök dizin kısmında `/routes/user.js` yolunu oluşturun.

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Bu basit yollar, isteklerle birlikte görünümlerinize geçer. Var olan kullanıcılar da yollara dahil edilir.

Kök dizin kısmında `/views/index.ejs` görünümünü oluşturun. Bu, oturum açma ve oturum kapatma için ilkeleri çağıran basit bir sayfadır. Hesap bilgilerini almak için de kullanabilirsiniz. Kullanıcının oturum açtığına kanıt sağlamak için kullanıcı isteğin içinden geçerken koşullu `if (!user)` kullanabileceğinizi unutmayın.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please sign in.</h2>
    <a href="/login/?p=your facebook policy">Sign in with Facebook</a>
    <a href="/login/?p=your email sign-in policy">Sign in with email</a>
    <a href="/login/?p=your email sign-up policy">Sign up with email</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account info</a></br>
    <a href="/logout">Log out</a>
<% } %>
```

`passport-azure-ad` tarafından kullanıcı isteğine koyulan ek bilgileri görüntüleyebilmek için kök dizin kısmında `/views/account.ejs` görünümü oluşturun.

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
<p>Full Claims</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

Artık uygulamanızı oluşturabilir ve çalıştırabilirsiniz.

`node app.js` öğesini çalıştırın ve şu konuma gidin: `http://localhost:3000`


E-posta veya Facebook ile uygulamaya kaydolun veya oturum açın. Oturumu kapatın ve farklı kullanıcı olarak tekrar oturum açın.

##Sonraki adımlar

Tamamlanan örnek, başvuru için (yapılandırma değerleriniz olmadan) [.zip dosyası olarak sağlanır](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Aynı zamanda GitHub'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

Artık daha ileri seviyeli konu başlıklarına geçebilirsiniz. Deneyebilecekleriniz:

[Node.js'de B2C modeli kullanarak web API'sinin güvenliğini sağlama](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on to more advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing the your B2C App's UX >>]()

-->



<!----HONumber=Jun16_HO2-->


