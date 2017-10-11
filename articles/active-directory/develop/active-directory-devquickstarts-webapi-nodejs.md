---
title: "Azure AD Node.js Başlarken | Microsoft Docs"
description: "Node.js REST web kimlik doğrulaması için Azure AD ile tümleştirilir API'si oluşturma."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>Node.js için Web API ile çalışmaya başlama
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport*, Node.js için kimlik doğrulama ara yazılımıdır. Esnek ve modüler, Passport sorunsuz bir şekilde her Express tabanlı bırakılabilir veya Restify web uygulamasına. Bir dizi kapsamlı strateji bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler. Microsoft Azure Active Directory için (Azure AD) bir strateji geliştirdik. Bu modülü yükleyin ve ardından Microsoft Azure Active Directory ekleyin `passport-azure-ad` eklentisi.

Bunu yapmanız için gerekenler:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. Passport'un kullanmak için uygulamanızı ayarlayın `passport-azure-ad` eklentisi.
3. Yapılacaklar listesi web API'sini çağırmak için bir istemci uygulaması yapılandırın.

Bu öğretici için kod [GitHub'da](https://github.com/Azure-Samples/active-directory-node-webapi) korunur.

> [!NOTE]
> Bu makalede, oturum açma, kaydolma nasıl uygulanacağını veya Azure AD B2C ile profil Yönetimi kapsamaz. Kullanıcının kimliği zaten doğrulanmış sonra web API'leri çağırmaya odaklanır.  İle başlamanızı öneririz [Azure Active Directory belge ile tümleştirmek nasıl](active-directory-how-to-integrate.md) Azure Active Directory ile ilgili temel bilgileri öğrenin.
>
>

Biz GitHub çalışan bu örnekte bir MIT lisansı altında için tüm kaynak kodunu yayımlanan, bu nedenle kopya (veya çatalı bile daha iyi) çekinmeyin ve geri bildirim sağlamak ve çekme istekleri.

## <a name="about-nodejs-modules"></a>Node.js modüllerini hakkında
Bu kılavuzda Node.js modüllerini kullanırız. Modüller, uygulamanız için belirli işlevler sağlayan yüklenebilir JavaScript paketlerdir. Genellikle NPM yükleme dizininde Node.js NPM komut satırı aracını kullanarak modüllerini yükleyin. Ancak, HTTP modülü gibi bazı modüller çekirdek Node.js pakete dahil edilir.

Yüklü modülleri kaydedilir **node_modules** Node.js yükleme dizinine kökündeki dizin. Her bir modülü **node_modules** directory tutar kendi **node_modules** bağımlı herhangi bir modül içeren dizini. Ayrıca, her gerekli modülüne sahip bir **node_modules** dizini. Bu özyinelemeli dizin yapısını bağımlılık zinciri temsil eder.

Bu bağımlılık zinciri yapı daha büyük bir uygulama yer sonuçlanır. Ancak, ayrıca tüm bağımlılıkları karşılanırsa ve geliştirme kullanılan modülleri sürümünü üretimde kullanılır güvence altına alır. Bu, üretim uygulamanızın davranışını daha tahmin edilebilir hale getirir ve kullanıcıları etkileyebilecek sürüm oluşturma sorunları önler.

## <a name="step-1-register-an-azure-ad-tenant"></a>1. adım: Azure AD kiracısı kaydetme
Bu örneği kullanmak için Azure Active Directory kiracısı gerekir. Hangi Kiracı ya da bir alma, emin değilseniz bkz [Azure AD kiracısı alma](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>2. adım: uygulama oluşturma
Sonraki dizininizde, uygulamanız ile güvenli bir şekilde iletişim kurması için gereken bilgileri Azure AD'ye verir bir uygulama oluşturun.  Hem istemci uygulaması hem de web API'si tek bir tarafından temsil edilen **uygulama kimliği** bu durumda, bir mantıksal uygulama içerdikleri için.  Bir uygulama oluşturmak için [bu talimatları](active-directory-how-applications-are-added.md) izleyin. Bir satır iş kolu uygulaması geliştiriyorsanız [bu ek yönergeler yararlı olabilecek](../active-directory-applications-guiding-developers-for-lob-applications.md).

Bir uygulama oluşturmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Üst menüsünde hesabınızı seçin. Ardından, altında **Directory** listesinde, Active Directory Kiracı uygulamanızı kaydetmek istediğiniz yeri seçin.

3. Soldaki menüde seçin **daha Hizmetleri**ve ardından **Azure Active Directory**.

4. Seçin **uygulama kayıtlar**ve ardından **Ekle**.

5. Oluşturmak için istemleri izleyerek bir **Web uygulaması ve/veya Webapı**.

      * **Adı** uygulamanızı son kullanıcıların uygulamayı açıklar.

      * **Oturum açma URL'si** , uygulamanızın temel URL.  Örnek kod, varsayılan URL `https://localhost:8080`.

6. Kaydettikten sonra Azure AD, uygulamanızın benzersiz bir uygulama kimliği atar Bu değer gereken sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.

7. Gelen **ayarları** -> **özellikleri** sayfasında uygulamanız için uygulama kimliği URI'si güncelleştirin. **Uygulama kimliği URI'si** uygulamanız için benzersiz bir tanımlayıcıdır. Kuralı kullanmaktır `https://<tenant-domain>/<app-name>`, örneğin: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Oluşturma bir **anahtar** uygulamanızdan için **ayarları** sayfasında ve bir yere kopyalayın. Kısa süre içinde gerekir.

## <a name="step-3-download-nodejs-for-your-platform"></a>3. adım: Platformunuz için Node.js indirme
Bu örneği başarılı bir şekilde kullanmak için Node.js çalışan yüklemesine sahip olmanız gerekir.

Adresinden node.js'yi yükleyin [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>4. adım: Yükleme MongoDB, platformda
Bu örneği başarılı bir şekilde kullanmak için MongoDB çalışan yüklemesine olması gerekir. Mongodb'yi, REST API sunucu örneklerinde kalıcı yapmak için kullanın.

Adresinden [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Bu kılavuz, mongodb://localhost bu yazma zamanında olduğu, MongoDB için varsayılan yükleme ve sunucu uç noktalarını kullanmak varsayar.
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a>5. adım: Restify modüllerini web API'nize yükleme
Bizim REST API oluşturmak için Restify kullanıyoruz. Restify, Express'ten türetilen minimal ve esnek Node.js uygulama çerçevesi. Connect üstünde REST API'leri oluşturmaya yönelik bir dizi sağlam özelliğe sahiptir.

### <a name="install-restify"></a>Restify'ı yükleme
1. Komut satırında, dizinleri değiştirmek **azuread'i** dizini. Varsa **azuread'i** directory olmayabilir, oluşturun.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Aşağıdaki komutu yazın:

    `npm install restify`

    Bu komut Restify'ı yükler.

#### <a name="did-you-get-an-error"></a>Hata mı aldınız?
Bazı işletim sistemlerinde NPM kullandığınızda bildiren bir hata alabilirsiniz **hata: EPERM, chmod ' / usr/yerel/bin /..'** ve öneride hesabı yönetici olarak çalıştırmayı deneyin. Bu gerçekleşirse, NPM daha yüksek bir ayrıcalık düzeyinde çalıştırmak için sudo komutunu kullanın.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>DTRACE ilgili bir hata mı aldınız?
Restify'ı yüklerken şuna benzer bir hata görebilirsiniz:

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
Restify, DTrace kullanarak REST çağrılarını izlemek için güçlü bir mekanizma sağlar. Bununla birlikte, birçok işletim sistemi DTrace gerekmez. Bu hataları güvenle yoksayabilirsiniz.

Bu komutun çıktısı aşağıdaki çıkış benzer görünmelidir:

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


## <a name="step-6-install-passportjs-in-your-web-api"></a>6. adım: Yükleme Passport.js, Web API
[Passport](http://passportjs.org/), Node.js için kimlik doğrulama ara yazılımıdır. Esnek ve modüler, Passport sorunsuz bir şekilde her Express tabanlı bırakılabilir veya Restify web uygulamasına. Bir dizi kapsamlı strateji bir kullanıcı adı ve parola, Facebook, Twitter ve daha fazlası ile kimlik doğrulamasını destekler.

Azure Active Directory için bir strateji geliştirdik. Biz bu modülü yükleyin ve ardından Azure Active Directory stratejisi eklentisini ekleyin.

1. Komut satırında, dizinleri değiştirmek **azuread'i** dizini.

2. Passport.js yüklemek için aşağıdaki komutu girin:

    `npm install passport`

    Komut çıktısı şuna benzer görünmelidir:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a>7. adım: Passport Azure AD web API'nize ekleme
Sonraki OAuth stratejisini kullanarak eklediğimiz `passport-azure-ad`, Passport Azure Active Directory connect stratejileri dizisi. Bu REST API'si örneğindeki taşıyıcı belirteçler için bu stratejiyi kullanın.

> [!NOTE]
> OAuth2 herhangi bir bilinen belirteç türünün verilebilir bir altyapı sağlasa da yalnızca belirli belirteç türleri yaygın olarak kullanılır. Taşıyıcı belirteçleri uç noktaları korumak için en yaygın kullanılan belirteçleridir. Bunlar OAuth2 belirteci en yaygın olarak verilen türüdür. Birçok uygulama, taşıyıcı belirteçlerini düzenlenen belirteçlerin türü olduğunu varsayar.
>
>

Komut satırında, dizinleri değiştirmek **azuread'i** dizini.

Passport.js yüklemek için aşağıdaki komutu yazın `passport-azure-ad module`:

`npm install passport-azure-ad`

Komutunun çıktısını aşağıdaki çıkış benzer görünmelidir:


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



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a>8. adım: Web API'nize MongoDB modülleri ekleme
Bizim veri deposu olarak MongoDB kullanırız. Bu nedenle, biz model ve şemaları yönetmek için yaygın olarak kullanılan eklenti çağrılan Mongoose yüklemeniz gerekir. Biz de (hangi MongoDB olarak da adlandırılır) MongoDB veritabanı sürücüsünü yüklemeniz gerekir.

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>9. adım: ek modülleri yükleme
Sonraki kalan gerekli modüllerini yükleyin.

1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

    `cd azuread`

2. Bu modüllerdeki yüklemek için aşağıdaki komutları girin, **node_modules** dizini:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>10. adım: bağımlılıklarınız ile bir server.js oluşturma
Server.js dosyasında bizim web API sunucunuz için işlevselliğin çoğu sağlar. Size büyük bir kısmını bu dosyaya ekleyin. Üretim için işlevselliği, ayrı yollar ve denetleyiciler gibi daha küçük dosyalar halinde yeniden düzenlemeniz öneririz. Bu gösteride, biz bu işlevsellik için server.js kullanın.

1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

    `cd azuread`

2. Oluşturma bir `server.js` dosya, en sık kullandığınız düzenleyicide ve aşağıdaki bilgileri ekleyin:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Dosyayı kaydedin. İçin bir kısa süre içinde getireceğiz.

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a>11. adım: Azure AD ayarlarınızı depolamak için bir yapılandırma dosyası oluşturma
Bu kod dosyası Passport.js için Azure Active Directory portalınızdan yapılandırma parametrelerini geçirir. Portal kılavuzun ilk bölümünde web API'si eklendiğinde bu yapılandırma değerlerini oluşturuldu. Kodu kopyaladıktan sonra bu parametre değerlerine eklemeniz gerekenler açıklanacaktır.

1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

    `cd azuread`

2. Oluşturma bir `config.js` dosya, en sık kullandığınız düzenleyicide ve aşağıdaki bilgileri ekleyin:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. Dosyayı kaydedin.

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a>12. adım: yapılandırma değerlerini server.js dosyanıza ekleyin.
Bu değerleri uygulamamız oluşturulan .config dosyası okuma gerekir. Bunu yapmak için .config dosyası gerekli bir kaynak olarak bizim uygulamada ekleriz. Sonra genel değişkenler config.js belge değişkenlerde eşleşecek şekilde ayarlayın.

1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

    `cd azuread`

2. Açık, `server.js` dosya, en sık kullandığınız düzenleyicide ve aşağıdaki bilgileri ekleyin:

    ```Javascript
    var config = require('./config');
    ```
3. Yeni bir bölüm eklemek `server.js` aşağıdaki kod ile:

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Dosyayı kaydedin.

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>13. adım: Mongoose kullanarak MongoDB Model ve şema bilgilerini ekleme
Şimdi Biz bu üç dosyayı REST API hizmetinde birleştirmek gibi ödeme başlatmak için bu hazırlık geçiyor.

Bu kılavuz için 4. adımda açıklandığı gibi bizim görevleri depolamak için MongoDB kullanın.

İçinde `config.js` dosya adım 11 oluşturduğumuz, biz Veritabanımıza adlı `tasklist`, biz sonunda put olduğu için bizim **mogoose_auth_local** bağlantı URL'si. Bu veritabanını MongoDB'de önceden oluşturmanız gerekmez. Bunun yerine, MongoDB bu bize (veritabanı önceden var olmayan varsayılarak) bizim sunucu uygulamasının ilk çalıştırılmasında oluşturur.

Biz sunucu kullanmak için isteriz hangi MongoDB veritabanını söylediyse, model ve şema bizim sunucunun görevler için oluşturmak için bazı ek kod yazmanız gerekir.

### <a name="discussion-of-the-model"></a>Modelin tartışma
Bizim şema modeli basit bir işlemdir. Size gereken şekilde genişletin.

Ad: Göreve atanan kişi adı. A **dize**.

Görev: Görevin kendisi. A **dize**.

Tarihi: görev son tarihi. A **DATETIME**.

TAMAMLANDI: görev varsa veya tamamlanmış. A **BOOLEAN**.

### <a name="creating-the-schema-in-the-code"></a>Kod içinde şema oluşturma
1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

    `cd azuread`

2. Açık, `server.js` dosya, en sık kullandığınız düzenleyicide ve sonra aşağıdaki bilgileri yapılandırma girdisi altına ekleyin:

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Koddan anlayabilirsiniz gibi bizim şema ilk oluşturuyoruz. Biz tanımlarken kod genelindeki verileri depolamak için kullandığımız bir model nesnesi oluşturuyoruz sonra bizim **yollar**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>14. adım: bizim görev REST API sunucunuz için bizim yollar ekleme
Biz çalışmak için bir veritabanı modeliniz olduğuna göre biz giden yollar bizim REST API sunucunuz için kullanmak ekleyelim.

### <a name="about-routes-in-restify"></a>Restify'daki yollar hakkında
Yollar Restify'da Express yığınını yaparlar aynı şekilde çalışır. Yolları, istemci uygulamalarının çağırmasını beklediğiniz URI'yi kullanarak tanımlarsınız. Genellikle, yollarınızı ayrı bir dosyaya tanımlayın. Bizim amacıyla, biz server.js dosyasında bizim yollar yerleştirin. Üretim kullanımı için kendi dosyasına bu yollar faktörü öneririz.

Bir Restify yolu için genel bir desen aşağıdaki gibidir:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Bu, en temel düzeyindeki desendir. Restify (ve hızlı) uygulama türlerini tanımlama ve farklı uç noktalar arasında karmaşık yönlendirme sağlama gibi çok daha kapsamlı işlevsellik sağlar. Bizim amacıyla, biz Bu yolları basit tutuyor musunuz.

### <a name="add-default-routes-to-our-server"></a>Bizim sunucuya varsayılan yollar ekleme
Biz şimdi temel CRUD yollarını oluşturma, alma, güncelleştirme, ekleme ve silme.

1. Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasörü:

    `cd azuread`

2. Açık `server.js` dosya, en sık kullandığınız düzenleyicide ve sonra aşağıdaki bilgileri yaptığınız önceki veritabanı girişlerinin altına ekleyin:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
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


// Delete a task by name.

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

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

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
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
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

### <a name="add-error-handling-in-our-apis"></a>Hata işleme bizim API'leri ekleme
```

///--- Errors for communicating something interesting back to the client.

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


## <a name="step-15-create-your-server"></a>15. adım: sunucunuzu oluşturma
Veritabanımıza tanımlamış olduğunuz ve bizim yollar yerdir. Son bir şey yapmak için bizim çağrıları yöneten sunucu örneğini eklemektir.

Restify'ı (ve hızlı) çok sayıda REST API sunucunuz için özelleştirme yapabilirsiniz, ancak yeniden biz en temel kurulumu bizim amaçlar için kullanacağınız.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a>16. adım: yolları (olmadan kimlik doğrulaması için şimdi) sunucusu ekleyin
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
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
// Register a default '/' handler.
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

## <a name="step-17-run-the-server-before-adding-oauth-support"></a>17. adım: (OAuth desteğini eklemeden önce) sunucu çalıştırma
Biz kimlik doğrulaması eklemeden önce sunucunuzu sınayın.

Sunucunuzu test etmek için en kolay yolu, komut satırında curl kullanarak ' dir. Bunu önce çıktıyı JSON olarak ayrıştırmanıza kurmamızı sağlayan bir yardımcı programı gerekiyor.

1. (Aşağıdaki örneklerde, bu aracı kullanın) aşağıdaki JSON aracını yükleyin:

    `$npm install -g jsontool`

    Bu işlem, JSON aracını global olarak yükler. Şimdi biz elde, sunucuyla Yürüt:

2. Öncelikle, mongoDB örneğinizin çalıştığından emin olun:

    `$sudo mongod`

3. Ardından, dizini değiştirin ve curling başlatın:

    `$ cd azuread` `$ node server.js`

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

4. Ardından, biz bu şekilde bir görev ekleyebilirsiniz:

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
    Ve şu görevleri bu şekilde Brandon için listeleyebilirsiniz:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Tüm bu çalışırsa, biz OAuth REST API sunucusuna ekleme yapmaya hazırsınız demektir.

Bir REST API MongoDB sunucusuyla var!

## <a name="step-18-add-authentication-to-our-rest-api-server"></a>Adım 18: kimlik doğrulaması bizim REST API sunucusuna ekleme
Çalışan bir REST API sahibiz, Azure AD ile yararlı hale getirme başlayalım.

Komut satırında, dizinleri değiştirmek **azuread'i** , zaten orada değilseniz klasör.

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>passport-azure-ad ile birlikte dahil edilen OIDCBearerStrategy'yi kullanma
Şu ana kadar herhangi türde bir yetkilendirme olmadan genel bir REST TODO sunucusu oluşturduğumuz. Burada, bir araya getirilmesi Başlat budur.

1. İlk olarak, biz biz Passport'u kullanmak istediğinizi belirtmeniz gerekir. Bu hak, diğer sunucu yapılandırmanızın sonra koyun:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > API'leri yazarken, her zaman veri benzersiz kullanıcının yanılmayacağı belirteçten bağlamanızı öneririz. Bu sunucu, Yapılacaklar öğelerini depoladığında, bunları nesne kimliği (token.oid olarak adlandırılır), belirteçte biz "sahip" alanına yerleştirin kullanıcının göre depolar. Bu, yalnızca bu kullanıcının kendi TODOs erişebilmesini sağlar. Olmadığından hiçbir Etkilenme "sahip" API'SİNDE bir dış kullanıcı kimlikleri doğrulanır olsa bile başkalarının TODOs isteyebilir.                    

2. Sonraki birlikte taşıyıcı stratejisi kullanalım `passport-azure-ad`. Şimdilik koda bakın ve rest kısa süre içinde açıklayacağız. Bu, ne yukarıda yapıştırdığınız sonra koyun:

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
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


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
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

    passport.use(bearerStrategy);
```

Passport, strateji yazarlarının hepsinin bağlı tüm kendi stratejileri (Twitter, Facebook vb.) için benzer bir desen kullanır. Stratejisi baktığınızda, biz bir belirteç ve yapılan bir parametre olarak olan bir işlev geçirmek bakın. Kendi iş yaptıktan sonra strateji bize gelir. Bunu yaptıktan sonra biz kullanıcıyı depolayın ve biz bunları yeniden istemeniz gerekmez belirteci kaydedin;.

> [!IMPORTANT]
> Önceki kod olur bizim sunucusuna kimlik doğrulaması için herhangi bir kullanıcı alır. Bu otomatik kaydı bilinir. Üretim sunucularında, herkesin bunları gerekmeden izin verme olduğunu öneririz karar kayıt işlemiyle gidin. Genellikle, Facebook ile kaydolursunuz ancak daha sonra ek bilgileri doldurmanızı isteyin izin tüketici uygulamalarında görürsünüz düzeni budur. Bu komut satırı programı olmasaydı, size e-posta döndürülen ve ek bilgileri doldurmasını kullanıcıya sorulan belirteç nesnesi ayıklanan. Bu bir test sunucusu olduğundan bunları yalnızca bellek içi veritabanına ekleriz.
>
>

### <a name="protect-some-endpoints"></a>Bazı uç noktaları koruma
Belirterek uç noktaları koruma `passport.authenticate()` kullanmak istediğiniz protokolü ile çağırın.

Bizim sunucu kodu daha ilginç bir şeyler, rota düzenleyelim yapma.

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

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. adım: sunucu uygulamanızı yeniden çalıştırın ve sizi reddettiğini olun
Kullanalım `curl` yeniden biz artık OAuth2 koruma bizim uç noktalarına karşı olup olmadığına bakın. Biz, herhangi bir istemci SDK Bu uç noktaya karşı çalıştırmadan önce bu test yapabilirsiniz. Döndürülen üstbilgileri doğru yolu yapacağız durumunda bize için yeterli olmalıdır.

1. Öncelikle, mongoDB örneğinizin çalıştığından emin olun:

    `$sudo mongod`

2. Ardından, dizini değiştirin ve curling başlatın.

      `$ cd azuread` `$ node server.js`

3. Temel bir POST deneyin.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Bir 401 burada aradığınız cevaptır. Bu yanıt Passport katmanının tam olarak ne olduğu yetkili uç noktası için yeniden yönlendirme yaptığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar
OAuth2 uyumlu bir istemci kullanmadan bu sunucu ile kadar gitti. Bir ek izlenecek yol gitmesi gerekir.

Restify ve OAuth2 kullanarak bir REST API'si uygulama artık öğrendiniz. Ayrıca tutup hizmetinizi geliştirme göre bu örnekte nasıl oluşturulacağını öğrenmek için birden fazla yeterli kod vardır.

ADAL Yolculuğunuzun sonraki adımlarda ilgileniyorsanız, desteklenen bazı ADAL istemcileri ile çalışmaya devam öneririz aşağıda verilmiştir.

Geliştirici makinenizi aşağıya doğru kopyalamak ve kılavuzda açıklandığı gibi yapılandırın.

[İOS için ADAL](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[Android için ADAL](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
