<properties
    pageTitle="B2C Önizlemesi: Node.js kullanarak bir web API'sini güvence altına alma | Microsoft Azure"
    description="B2C kiracısından belirteç kabul eden bir Node.js web API'si oluşturma"
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

# B2C önizlemesi: Node.js kullanarak bir web API'sini güvence altına alma

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]


> [AZURE.NOTE] Bu makale, Azure AD B2C kullanarak oturum açma, kaydolma ve profil yönetimi uygulama konularını kapsamaz. Kullanıcının kimliği doğrulandıktan sonra, web API'leri çağırmaya odaklanır. Henüz başlamadıysanız Azure Active Directory B2C ile ilgili temel bilgileri öğrenmek için [.NET web uygulamasıyla çalışmaya başlama öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md) ile başlamanız gerekir.


> [AZURE.NOTE]  Bu örnek, [iOS B2C örnek uygulamamızı](active-directory-b2c-devquickstarts-ios.md) kullanarak bağlanmak için yazılmıştır. Öncelikle bu kılavuzu uygulayın ve ardından örneği takip edin.

**Passport**, Node.js için kimlik doğrulama ara yazılımıdır. Son derece esnek ve modüler olan Passport, Express tabanlı veya Restify web uygulamasına sorunsuz bir şekilde yüklenebilir. Bir dizi kapsamlı strateji; bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler. Azure Active Directory (Azure AD) için bir strateji geliştirdik. Bu modülü yükleyin ve ardından Azure AD `passport-azure-ad` eklentisini ekleyin.

Bunun için şunları yapmanız gerekir:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. Passport'un `azure-ad-passport` eklentisini kullanmak için uygulamanızı ayarlayın.
3. "Yapılacaklar listesi" web API'sini çağırmak için bir istemci uygulaması yapılandırın.

Bu öğretici için kod [GitHub'da korunur](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs). İzlemek için [uygulamanın çatısını bir .zip dosyası olarak indirin](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/skeleton.zip) veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```

Bu öğretici sonunda tamamlanmış uygulama sağlanır.

> [AZURE.WARNING]   B2C önizlememiz için hem web API'si görev sunucusu hem de ona bağlanan istemciye yönelik aynı **İstemci Kimliği**/**Uygulama Kimliği** ve ilkeleri kullanmanız gerekir. Ayrıca bu, iOS ve Android öğreticilerimiz için de geçerlidir. Bu Hızlı Başlangıçların biriyle daha önce bir uygulama oluşturduysanız bu değerleri kullanın ve yeni değerler oluşturmayın.


## Azure AD B2C dizini alma

Azure AD B2C'yi kullanabilmek için önce dizin veya kiracı oluşturmanız gerekir.  Dizin; tüm kullanıcılarınız, uygulamalarınız, gruplarınız ve daha fazlası için bir kapsayıcıdır.  Henüz yoksa devam etmeden önce [bir B2C dizini oluşturun](active-directory-b2c-get-started.md).

## Uygulama oluşturma

Daha sonra, B2C dizininizde Azure AD'ye uygulamanız ile güvenli şekilde iletişim kurması için gereken bazı bilgiler sağlayan bir uygulama oluşturmanız gerekir. Bu durumda, tek bir mantıksal uygulama içerdikleri için hem istemci uygulaması hem de web API'si tek bir **Uygulama Kimliği** ile temsil edilir. Bir uygulama oluşturmak için [bu talimatları](active-directory-b2c-app-registration.md) izleyin. Şunları yaptığınızdan emin olun:

- Uygulamaya bir **web uygulaması/web api'si** dahil edin
- **Yanıt URL'si** olarak `http://localhost/TodoListService` adresini girin. Bu URL, bu kod örneği için varsayılan URL'dir.
- Uygulamanız için bir **Uygulama gizli anahtarı** oluşturun ve bunu kopyalayın. Buna daha sonra ihtiyacınız olacak. Kısa süre sonra ihtiyacınız olacaktır. Kullanmadan önce bu değerin [XML kaçışlı](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) olması gerektiğini unutmayın.
- Uygulamanıza atanan **Uygulama Kimliği**'ni kopyalayın. Buna da daha sonra ihtiyacınız olacak.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## İlkelerinizi oluşturma

Azure AD B2C'de her kullanıcı deneyimi, bir [ilke](active-directory-b2c-reference-policies.md) ile tanımlanır. Bu uygulama, üç kimlik deneyimi içerir: kaydol, oturum aç ve Facebook ile oturum aç. Her tür için [ilke başvurusu makalesinde](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) tanımlanan şekilde bir ilke oluşturmanız gerekir.  Üç ilkenizi oluştururken şunları yaptığınızdan emin olun:

- Kaydolma ilkenizde **Görünen adı** ve diğer kaydolma özniteliklerini seçin.
- Her ilkede uygulamanın talep ettiği **Görünen ad** ve **Nesne Kimliği** öğelerini seçin.  Diğer talepleri de seçebilirsiniz.
- Oluşturduktan sonra her bir ilkenin **Adını** kaydedin. `b2c_1_` önekine sahip olmalıdır.  Bu ilke adları daha sonra gerekecektir.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Üç ilkenizi oluşturduktan sonra uygulamanızı oluşturmaya hazırsınız.

Bu makalenin, oluşturduğunuz ilkeleri nasıl kullanacağınıza yönelik bilgileri kapsamadığını unutmayın. İlkelerin Azure AD B2C'de nasıl çalıştığını öğrenmeye [.NET web uygulaması ile çalışmaya başlama öğreticisi](active-directory-b2c-devquickstarts-web-dotnet.md) ile başlayın.

## Platformunuz için Node.js indirme

Bu örneği başarılı bir şekilde kullanmak için Node.js çalışan yüklemesine sahip olmanız gerekir.

[nodejs.org](http://nodejs.org) adresinden Node.js'yi yükleyin.

## Platformunuz için MongoDB yükleme

Bu örneği başarılı bir şekilde kullanmak için MongoDB çalışan yüklemesine de sahip olmanız gerekir. MongoDB'yi, REST API'nizi sunucu örneklerinde kalıcı hale getirmek için kullanırsınız.

[mongodb.org](http://www.mongodb.org) adresinden MongoDB'yi yükleyin.

> [AZURE.NOTE] Bu kılavuz, MongoDB için bu yazma sırasında `mongodb://localhost` olan varsayılan yükleme ve sunucu uç noktalarını kullanacağınızı varsayar.

## Restify modüllerini Web API'nize yükleme

REST API'nizi oluşturmak için Restify'ı kullanırsınız. Restify, Express'ten türetilen minimal ve esnek bir Node.js uygulama çerçevesidir. Connect üstünde REST API'leri oluşturmaya yönelik bir dizi sağlam özelliğe sahiptir.

### Restify'ı yükleme

Komut satırından dizininizi `azuread` olarak değiştirin. `azuread` dizini mevcut değilse oluşturun.

`cd azuread` veya `mkdir azuread;`

Aşağıdaki komutu kullanın:

`npm install restify`

Bu komut Restify'ı yükler.

#### Hata mı aldınız?

Bazı işletim sistemlerinde `npm` öğesini kullandığınızda `Error: EPERM, chmod '/usr/local/bin/..'` hatasını ve hesabı yönetici olarak çalıştırmanızı isteyen bir istek alabilirsiniz. Bu durum oluşursa `npm` öğesini daha yüksek bir ayrıcalık düzeyinde çalıştırmak için `sudo` komutunu kullanın.

#### Bir DTrace hatası mı aldınız?

Restify'ı yüklediğinizde şöyle bir şey görebilirsiniz:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify, DTrace kullanarak REST çağrılarını izlemek için güçlü bir mekanizma sağlar. Ancak çoğu işletim sisteminde DTrace kullanılabilir değildir. Bu hataları güvenle yoksayabilirsiniz.

Komut çıktısı şuna benzer şekilde görünmelidir:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## Passport'u web API'nize yükleme

[Passport](http://passportjs.org/), Node.js için kimlik doğrulama ara yazılımıdır. Son derece esnek ve modüler olan Passport, Express tabanlı veya Restify web uygulamasına sorunsuz bir şekilde yüklenebilir. Bir dizi kapsamlı strateji; bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler. Azure AD için bir strateji geliştirdik. Bu modülü yükleyin ve ardından Azure AD stratejisi eklentisini ekleyin.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin.

Passport'u yüklemek için aşağıdaki komutu girin:

`npm install passport`

Komut çıktısı buna benzer olmalıdır:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## Passport-azuread'i web API'nize ekleme

Ardından `passport-azuread` öğesini kullanarak Azure AD'yi Passport'a bağlayan bir stratejiler paketi olan OAuth stratejisini ekleyin. REST API'si örneğindeki taşıyıcı belirteçler için bu stratejiyi kullanın.

> [AZURE.NOTE] OAuth2, herhangi bir bilinen belirteç türünün verilebileceği bir altyapı sağlasa da yalnızca belirli belirteç türleri yaygın kullanım elde etmiştir. Uç noktaları korumaya yönelik belirteçler, taşıyıcı belirteçlerdir. Bunlar, OAuth2'de en yaygın olarak verilen belirteç türleridir. Birçok uygulama, taşıyıcı belirteçlerin verilen tek belirteç türü olduğunu varsayar.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin.

Passport `passport-azure-ad` modülünü yüklemek için aşağıdaki komutu girin:

`npm install passport-azure-ad`

Komut çıktısı buna benzer olmalıdır:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## Web API'nize MongoDB modülleri ekleme

MongoDB'yi veri deponuz olarak kullanacaksınız. Bu nedenle, hem model ve şemaları yönetmek için yaygın olarak kullanılan bir eklenti olan Mongoose'u hem de aynı zamanda MongoDB olarak adlandırılan MongoDB veritabanı sürücüsünü yüklemeniz gerekir.

* `npm install mongoose`
* `npm install mongodb`

## Ek modüller yükleme

Ardından, kalan gerekli modüllerini yükleyin.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Modülleri `node_modules` dizininize yüklemek için aşağıdaki komutları girin:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## Bağımlılıklarınız ile bir server.js dosyası oluşturma

Web API sunucunuz için işlevselliğin çoğu, `server.js` dosyası tarafından sağlanır. Kodlarımızın büyük bir kısmını bu dosyaya ekleyeceksiniz. Üretim için işlevselliği, ayrı yollar ve denetleyiciler gibi daha küçük dosyalar halinde yeniden düzenlemeniz gerekir. Bu öğreticinin amaçları doğrultusunda bu işlevsellik için server.js kullanın.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Bir düzenleyicide `server.js` dosyası oluşturun. Aşağıdaki bilgileri ekleyin:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Dosyayı kaydedin. Buna daha sonra geri döneceksiniz.

## Azure AD ayarlarınızı depolamak için bir config.js dosyası oluşturma

Bu kod dosyası, Azure AD Portal'dan `Passport.js` dosyasına yapılandırma parametrelerini geçirir. Bu yapılandırma değerlerini, kılavuzun ilk bölümünde web API'sini portala eklediğinizde oluşturdunuz. Kodu kopyaladıktan sonra bu parametre değerlerine eklemeniz gerekenleri açıklayacağız.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Bir düzenleyicide `config.js` dosyası oluşturun. Aşağıdaki bilgileri ekleyin:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration', // For using Microsoft you should never need to change this.
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>',
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### Gerekli değerler

`IdentityMetadata`: Burası, `passport-azure-ad` öğesinin kimlik sağlayıcısı için yapılandırma verilerinizi arayacağı yerdir. Ayrıca, JSON web belirteçlerini doğrulamaya yönelik anahtarlar için de buraya bakılacaktır. Azure AD kullanıyorsanız muhtemelen bunu değiştirmek istemezsiniz.

`audience`: Hizmetinizi tanımlayan portaldan alınan tekdüzen kaynak tanımlayıcısı (URI). Örneğimizde `http://localhost/TodoListService` kullanılıyor.

`tenantName`: Kiracı adınız (örneğin, **contoso.onmicrosoft.com**).

`policyName`: Sunucunuza gelen belirteçleri doğrulamak istediğiniz ilke. Bu, oturum açma için istemci uygulamasında kullandığınız aynı ilke olmalıdır.

> [AZURE.NOTE] B2C önizlememiz için istemci ve sunucu kurulumunda aynı ilkeleri kullanın. Bir kılavuzu zaten tamamlayıp bu ilkeleri oluşturduysanız tekrar yapmanıza gerek yoktur. Kılavuzu tamamladığınız için sitede istemci kılavuzlarına yönelik yeni ilkeler ayarlamanıza gerek yoktur.

## Server.js dosyanıza yapılandırma ekleme

Değerleri, uygulamanızda yeni oluşturduğunuz `config.js` dosyasından okumanız gerekir. Bunu yapmak için `.config` dosyasını gerekli bir kaynak olarak uygulamanıza ekleyin ve ardından genel değişkenleri `config.js` belgesindekiler şeklinde ayarlayın.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Bir düzenleyicide `server.js` dosyasını açın. Aşağıdaki bilgileri ekleyin:

```Javascript
var config = require('./config');
```
`server.js` öğesine aşağıdaki kodu içeren yeni bir bölüm ekleyin:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience
};
// array to hold logged-in users and the current logged-in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## Mongoose kullanarak MongoDB model ve şema bilgilerini ekleme

Bu üç dosyayı REST API hizmetinde bir araya getirdiğiniz için erken hazırlık faydalı olur.

Daha önce de belirtildiği gibi bu kılavuz için görevlerinizi depolamak üzere MongoDB'yi kullanın.

Oluşturduğunuz `config.js` dosyasında veritabanınızın **tasklist** öğesini çağırdınız. Bu aynı zamanda `mongoose_auth_local` bağlantı URL'si sonuna eklediğiniz öğeydi. Bu veritabanını MongoDB'de önceden oluşturmanız gerekmez. Zaten mevcut değilse sizin için sunucu uygulamasının ilk çalıştırılmasında veritabanını oluşturur.

Sunucuya hangi MongoDB veritabanını kullanacağınızı bildirdikten sonra sunucu görevleriniz için model ve şema oluşturmak amacıyla birkaç ek kod yazmanız gerekir.

### Modeli genişletme

Bu şema modeli oldukça basittir. Gerektiği şekilde genişletebilirsiniz.

`name`: Göreve atanan kişi. Bu bir **dizedir**.

`task`: Görevin kendisi. Bu bir **dizedir**.

`date`: Görev son tarihi. Bu bir **tarih saat** değeridir.

`completed`: Görevin tamamlanıp tamamlanmadığını gösterir. Bu bir **Boole** değeridir.

### Kod içinde şema oluşturma

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Bir düzenleyicide `server.js` dosyasını açın. Aşağıdaki bilgileri yapılandırma girdisi altına ekleyin:

```Javascript
// MongoDB setup
// Set up some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Böylece MongoDB sunucusuna bağlanılır ve şema nesnesi geri verilir.

### Kodda model oluşturmak için şemayı kullanma

Yukarıda yazdığınız kodun altına aşağıdaki kodu ekleyin:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Önce şemayı oluşturursunuz, ardından **yollarınızı** tanımlarken kod genelindeki verilerinizi depolamak için kullanacağınız bir model nesnesi oluşturursunuz.

## REST API görev sunucunuz için yollar ekleme

Çalışacak bir veritabanı modeliniz olduğuna göre REST API sunucunuz için kullanacağınız yolları ekleyin.

### Restify'daki yollar hakkında

Yollar Restify'da tam olarak Express yığınını kullandıklarında çalıştıkları gibi çalışır. Yolları, istemci uygulamalarının çağırmasını beklediğiniz URI'yi kullanarak tanımlarsınız. Çoğu durumda, yollarınızı ayrı bir dosyada tanımlamanız gerekir. Bu öğreticinin amaçları doğrultusunda yollarınızı `server.js` dosyasına yerleştireceksiniz. Üretim amaçları doğrultusunda bunları kendi dosyalarına ayırmanızı öneririz.

Bir Restify yolu için genel bir desen:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Bu, en temel düzeyindeki desendir. Restify ve Express, uygulama türlerini tanımlama ve farklı uç noktalar arasında karmaşık yönlendirme yapma gibi çok daha kapsamlı işlevsellik sağlayabilir. Bu öğreticinin amaçları doğrultusunda bu yolları basit tutacağız.

#### Sunucunuza varsayılan yollar ekleme

Şimdi **oluştur**, **al**, **güncelleştir** ve **sil** temel CRUD yollarını ekleyeceksiniz.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

Bir düzenleyicide `server.js` dosyasını açın. Aşağıdaki bilgileri, yukarıda gerçekleştirdiğiniz veritabanı girişlerinin altına ekleyin:

```Javascript
/**
*
* APIs for our REST task server
*/
// Create a task
function createTask(req, res, next) {
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
// Create a new task model, fill it up and save it to Mongodb
var _task = new Task();
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
_task.date = new Date();
_task.save(function(err) {
if (err) {
req.log.warn(err, 'createTask: unable to save');
next(err);
} else {
res.send(201, _task);
}
});
return next();
}
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
/// Simple returns the list of TODOs that were loaded.
function listTasks(req, res, next) {
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
log.info("listTasks was called for: ", owner);
Task.find({
owner: owner
}).limit(20).sort('date').exec(function(err, data) {
if (err)
return next(err);
if (data.length > 0) {
log.info(data);
}
if (!data.length) {
log.warn(err, "There is no tasks in the database. Add one!");
}
if (!owner) {
log.warn(err, "You did not pass an owner when listing tasks.");
} else {
res.json(data);
}
});
return next();
}
```

#### Yollar için hata işleme ekleme

Hata işlemeleri eklemeniz gerekir; böylece, karşılaştığınız herhangi bir sorunu istemciye anlayabileceği bir şekilde geri iletebilirsiniz.

Aşağıdaki kodu, yukarıda yazdığınız kodun altına ekleyin:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## Sunucunuzu oluşturma

Artık veritabanınızı tanımladınız ve yollarınızı yerleştirdiniz. Son olarak yapmanız gereken ise çağrılarınızı yönetecek sunucu örneğini eklemektir.

Restify ve Express, bir REST API sunucusu için kapsamlı özelleştirme sağlar ancak burada en temel kurulumu kullanırsınız.

```Javascript
/**
* Our server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that we don't drop data on uploads
server.pre(restify.pre.pause());
// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());
// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());
// Allow five requests/second by IP, and burst to 10
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## Yolları sunucuya ekleme (kimlik doğrulaması olmadan)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the passport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method like this:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## OAuth desteğini eklemeden önce sunucuyu çalıştırma

Kimlik doğrulaması eklemeden önce sunucunuzu test etmeniz gerekir.

Bunu yapmanın en kolay yolu, bir komut satırında `curl` kullanmaktır. Ancak bunu yapmadan önce çıktıyı JSON olarak ayrıştırmanıza izin veren basit bir yardımcı program gereklidir. İlk olarak, JSON aracını yükleyin.

`$npm install -g jsontool`

Bu işlem, JSON aracını global olarak yükler. JSON aracını yükledikten sonra sunucuyu test edin:

MongoDB örneğinizin çalıştığından emin olun.

`$sudo mongodb`

`azuread` dizinine değiştirin ve `curl` öğesini kullanın.

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Bir görev ekleyin:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

Yanıt şu şekilde olmalıdır:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
"Brandon" kullanıcısı için görevleri bu şekilde listeleyebilirsiniz:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Bu işe yararsa OAuth'u REST API sunucusuna ekleme yapmaya hazırsınız.

MongoDB'si olan bir REST API sunucusuna sahipsiniz.

## REST API sunucunuza kimlik doğrulaması ekleme

Artık çalışan bir REST API sunucunuz olduğuna göre bunu Azure AD için faydalı hale getirebilirsiniz.

Zaten orada değilse komut satırından dizininizi `azuread` olarak değiştirin:

`cd azuread`

### passport-azure-ad ile birlikte dahil edilen OIDCBearerStrategy'yi kullanma

Şu ana kadar herhangi türde bir yetkilendirme olmadan genel bir REST ToDo sunucusu oluşturdunuz. Artık yetkilendirmeyi bir araya getirmeye başlayabilirsiniz.

İlk olarak, Passport'u kullanmak istediğinizi belirtmeniz gerekir. Bunu doğrudan diğer sunucu yapılandırmanızın altına ekleyin:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts Passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
API'leri yazarken her zaman verileri kullanıcının yanılmayacağı belirteçten benzersiz bir öğeye bağlamanız gerekir. Sunucu, Yapılacaklar öğelerini depoladığında bu da "sahip" alanına girilen belirteçteki (token.oid üzerindne çağrılır) kullanıcının **Nesne Kimliği**'ni temel alır. Bu durum, kullanıcının kendi Yapılacaklar öğelerine erişebilmesinin yanı sıra başka kişilerin de girilen Yapılacaklar öğelerine erişememesini sağlar. "Sahip" API'sinde kullanıma sunma işlemi gerçekleşmediğinden dış kullanıcı kimlik doğrulaması yapılmış olsa bile başkalarının Yapılacaklar öğelerini isteyebilir.

Ardından, `passport-azure-ad` ile birlikte sunulan taşıyıcı stratejisi kullanın. (Şimdilik yalnızca koda bakacağız.) bunu yukarıda yapıştırdığınızdan sonra koyun:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementer.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
log.info('Found user: ', user);
return fn(null, user);
}
}
return fn(null, null);
};
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport, strateji yazarlarının hepsinin bağlı kaldığı stratejilere benzer bir tüm stratejileri (Twitter ve Facebook dahil) için bir desen kullanır. Bu desene, parametre olarak `token` ve `done` öğelerini barındıran bir `function()` geçirirsiniz. Tüm işlemlerini tamamladıktan sonra strateji size geri döner. Ardından kullanıcıyı depolayıp belirteci kaydetmeniz gerekir; böylece bunları yeniden istemeniz gerekmez.

> [AZURE.IMPORTANT]
Yukarıdaki kod, kimlik doğrulaması yapan kullanıcıyı sunucunuza yönlendirir. Bu işlem, otomatik kimlik doğrulama olarak da bilinir. Üretim sunucularında, kullanıcıların önceden belirlediğiniz bir kayıt sürecinden geçmeden içeri alınmalarını istemezsiniz. Bu deseni genelde tüketici uygulamalarında görürsünüz. Bu uygulamalara Facebook kullanarak kaydolursunuz ancak daha sonra sizden bazı el bilgileri doldurmanızı isterler. Bu bir komut satırı programı olmasaydı, döndürülen belirteç nesnesinden e-postayı ayıklayabilir ve daha sonra kullanıcıdan ek bilgileri doldurmasını isteyebilirdik. Bu bir test sunucusu olduğundan bunları yalnızca bellek içi veritabanına ekleriz.

### Uç noktaları koruma

Kullanmak istediğiniz protokolü kullanarak `passport.authenticate()` çağrısını belirttiğinizde uç noktaları korursunuz.

Daha farklı işlemler gerçekleştirmek için sunucu kodunuzdaki yolu düzenleyebilirsiniz:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## Sizi reddettiğini doğrulamak için sunucu uygulamanızı yeniden çalıştırma

Artık uç noktalarınızda OAuth2 korumasına sahip olup olmadığınızı görmek için `curl` öğesini tekrar kullanabilirsiniz. Bunu gerçekleştirmeden önce uç noktada istemci SDK'larınızdan herhangi birini çalıştırın. Döndürülen üst bilgiler, doğru yolda olduğunuzu belirtmek için yeterli olmalıdır.

MongoDB örneğinizin çalıştığından emin olun:

    $sudo mongodb

Dizine değiştirin ve `curl` öğesini kullanın:

    $ cd azuread
    $ node server.js

Temel bir POST deneyin:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 hatası, aradığınız cevaptır. Bu hata, uç noktayı yetkilendirmek için Passport katmanının yeniden yönlendirme yaptığını belirtir.


## Artık OAuth2 kullanan bir REST API'niz var

OAuth2 uyumlu bir istemci kullanmadan bu sunucu ile çalışabildiğiniz kadar çalıştınız. Bunun için ilave bir kılavuz gerekir.

Yalnızca Restify ve OAuth2 kullanarak bir REST API'si uygulama hakkında bilgi edinmek istiyorsanız artık gerekli koda sahip olduğunuza göre bu örnekte hizmetinizi oluşturmaya ve geliştirmeye devam edebilirsiniz.

Tamamlanan örnek, başvuru için (yapılandırma değerleriniz olmadan) [.zip dosyası olarak sağlanır](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/complete.zip). Aynı zamanda GitHub'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```


## Sonraki adımlar

Artık şunlar gibi daha ileri seviyeli konulara geçebilirsiniz:

[B2C ile iOS kullanarak bir web API'sine bağlanma](active-directory-b2c-devquickstarts-ios.md)



<!----HONumber=Jun16_HO2-->


