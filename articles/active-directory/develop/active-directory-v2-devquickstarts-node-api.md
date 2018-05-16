---
title: Node.js kullanarak bir Azure Active Directory v2.0 web API güvenliğini | Microsoft Docs
description: Node.js web kişisel bir Microsoft hesabı ve iş belirteçleri kabul eder API'si oluşturma ya da Okul hesapları öğrenin.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: celested
ms.reviewer: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 19ad25c7b08ff073097cacf3be359772ca0a327f
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="secure-a-web-api-by-using-nodejs"></a>Node.js kullanarak bir web API güvenliğini sağlama
> [!NOTE]
> Tüm Azure Active Directory senaryolarını ve özelliklerini v2.0 uç noktası ile çalışır. V2.0 uç noktası veya v1.0 uç nokta kullanması gerekip gerekmediğini belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

Azure Active Directory (Azure AD) v2.0 uç noktası kullandığınızda, kullanabileceğiniz [OAuth 2.0](active-directory-v2-protocols.md) erişim belirteçleri web API korumak için. OAuth 2.0 erişim belirteçleri, hem bir kişisel Microsoft hesabı ve iş sahip kullanıcılar veya Okul hesapları web API güvenli bir şekilde erişebilir.

*Passport*, Node.js için kimlik doğrulama ara yazılımıdır. Esnek ve modüler, Passport sorunsuz bir şekilde bırakılan hiçbir Express tabanlı veya restify web uygulamasına. Passport, bir dizi kapsamlı strateji kimlik doğrulamasını bir kullanıcı adı ve parola, Facebook, Twitter veya diğer seçenekleri kullanarak destekler. Azure AD için bir strateji geliştirdik. Bu makalede, modülünü yüklemek ve Azure AD eklemek nasıl gösteriyoruz `passport-azure-ad` eklentisi.

## <a name="download"></a>İndirme
Bu öğretici için kod [GitHub'da](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs) korunur. Öğreticiyi izlemek için şunları yapabilirsiniz [uygulamanın çatısını bir .zip dosyası karşıdan](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), veya çatıyı kopyalayın:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Bu öğretici sonunda tamamlanmış uygulama da alabilirsiniz.

## <a name="1-register-an-app"></a>1: bir uygulamayı Kaydet
En yeni bir uygulama oluşturma [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya izleyin [bu adımları ayrıntılı](active-directory-v2-app-registration.md) uygulama kaydetmek için. Emin olun:

* Kopya **uygulama kimliği** uygulamanıza atanmış. Bu öğretici için gerekir.
* Ekleme **mobil** uygulamanız için platform.
* Kopya **yeniden yönlendirme URI'si** portalından. Varsayılan URI değeri kullanmalısınız `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-install-nodejs"></a>2: Node.js yükleyin
Bu öğretici için örnek kullanmak için şunları yapmalısınız [Node.js yüklemek](http://nodejs.org).

## <a name="3-install-mongodb"></a>3: MongoDB yükleme
Bu örneği başarılı bir şekilde kullanmak için [MongoDB yükleme](http://www.mongodb.org). Bu örnekte, REST API'nizi sunucu örneklerinde kalıcı yapma MongoDB kullanın.

> [!NOTE]
> Bu makalede, MongoDB için varsayılan yükleme ve sunucu uç noktalarını kullanmak istediğinizi varsayarız: mongodb://localhost.
> 
> 

## <a name="4-install-the-restify-modules-in-your-web-api"></a>4: restify modüllerini web API'nize yükleme
Bizim REST API oluşturmak için Resitfy kullanırız. Restify, Express'ten türetilen minimal ve esnek Node.js uygulama çerçevesi. Restify Connect üstünde REST API'leri oluşturmak için kullanabileceğiniz özellikler sağlam bir kümesini içerir.

### <a name="install-restify"></a>Restify'ı yükleme
1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

    Varsa **azuread'i** directory olmayabilir, oluşturun:

    `mkdir azuread`

2.  Restify'ı yükleme:

    `npm install restify`

    Bu komutun çıktısı aşağıdaki gibi görünmelidir:

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a>Hata mı aldınız?
Bazı işletim sistemlerinde kullandığınızda, `npm` komutu, bu iletiyi görebilirsiniz: `Error: EPERM, chmod '/usr/local/bin/..'`. Hata hesabı yönetici olarak çalıştırmayı deneyin bir istek tarafından izlenir. Bu gerçekleşirse, komutunu `sudo` çalıştırmak için `npm` daha yüksek bir ayrıcalık düzeyinde.

#### <a name="did-you-get-an-error-related-to-dtrace"></a>DTrace için ilgili bir hata mı aldınız?
Yüklediğinizde restify, bu iletiyi görebilirsiniz:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

Restify DTrace kullanarak REST çağrılarını izleme için güçlü bir mekanizma vardır. Ancak, DTrace birçok işletim sistemlerinde kullanılamaz. Bu iletiyi güvenle yok sayabilirsiniz.


## <a name="5-install-passportjs-in-your-web-api"></a>5: yükleme Passport.js, Web API
1.  Komut isteminde dizini değiştirin **azuread'i**.

2.  Passport.js yükleyin:

    `npm install passport`

    Komut çıktısı şuna benzer görünmelidir:

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: passport azure ad web API ekleme
Ardından, passport-azuread'i kullanarak OAuth stratejisini ekleyin. `passport-azuread` Azure AD'yi Passport'a bağlayan stratejileri paketidir. Bu REST API'si örneğindeki taşıyıcı belirteçler için bu stratejiyi kullanın.

> [!NOTE]
> OAuth 2.0 herhangi bir bilinen belirteç türünün verilebilmesi için bir çerçeve sağlasa da, belirli belirteç türleri yaygın olarak kullanılır. Taşıyıcı belirteçlerini uç noktaları korumak için yaygın olarak kullanılır. Taşıyıcı belirteçlerini OAuth 2.0 belirteç en yaygın olarak verilen türüdür. Birçok OAuth 2.0 uygulamaları taşıyıcı belirteçlerini tek belirteç türü olduğunu varsayar.
> 
> 

1.  Bir komut isteminde dizini değiştirin **azuread'i**.

    `cd azuread`

2.  Passport.js yükleme `passport-azure-ad` Modülü:

    `npm install passport-azure-ad`

    Komut çıktısı şuna benzer görünmelidir:

    ```
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
    ```

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: web API MongoDB modülleri ekleme
Bu örnekte, bizim veri deposu olarak MongoDB kullanırız. 

1.  Mongoose, yaygın olarak kullanılan bir yükleme eklentisi, model ve şemaları yönetmek için: 

    `npm install mongoose`

2.  MongoDB olarak da adlandırılan MongoDB veritabanı sürücüsünü yükleyin:

    `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: ek modülleri yükleme
Kalan gerekli modüllerini yükleyin.

1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Aşağıdaki komutları yazın. Komutları node_modules dizininizde aşağıdaki modüllerini yükleyin:

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a>9: bağımlılıklarınızı için bir Server.js dosyası oluşturma
Bir Server.js dosyası web API sunucunuz için işlevselliğin çoğu tutar. Kodunuzu çoğu bu dosyaya ekleyin. Üretim amaçları doğrultusunda, daha küçük dosyalar halinde işlevselliği gibi ayrı yollar ve denetleyiciler için yeniden. Bu makalede, bu amaçla Server.js kullanırız.

1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Bir düzenleyiciyi kullanarak, bir Server.js dosyası oluşturun. Aşağıdaki bilgileri dosyaya ekleyin:

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  Dosyayı kaydedin. İçin kısa bir süre içinde döndürür.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: Azure AD ayarlarınızı depolamak için bir yapılandırma dosyası oluşturma
Bu kod dosyası Passport.js için Azure AD Portalı'ndan yapılandırma parametrelerini geçirir. Makaleyi başındaki portal web API'si eklendiğinde bu yapılandırma değerlerini oluşturuldu. Kodu kopyaladıktan sonra bu parametre değerleri put gerekenler açıklayacağız.

1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Bir düzenleyicide bir Config.js dosyası oluşturun. Aşağıdaki bilgileri ekleyin:

    ```Javascript
    // Don't commit this file to your public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need to change this.
    };

    ```



### <a name="required-values"></a>Gerekli değerler

*   **IdentityMetadata**: Bu yerdir `passport-azure-ad` kimlik sağlayıcıyı (IDP) ve anahtarları JSON Web belirteçleri (Jwt'ler) doğrulamak için yapılandırma verilerinizi arar. Azure AD kullanıyorsanız muhtemelen bunu değiştirmek istemiyorsanız.

*   **Hedef kitle**: portalından, yeniden yönlendirme URI'si.

> [!NOTE]
> Anahtarlarınızı sık aralıklarla alma. Her zaman "openid_keys" URL'den çekme ve uygulama Internet erişebildiğinizden emin olun.
> 
> 

## <a name="11-add-the-configuration-to-your-serverjs-file"></a>11: Server.js dosyanıza yapılandırma ekleme
Yeni oluşturduğunuz yapılandırma dosyasından değerleri okumaya uygulamanız gerekir. Gerekli bir kaynak olarak uygulamanıza .config dosyası ekleyin. Config.js içinde olanlar için genel değişkenleri ayarlayın.

1.  Komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Bir düzenleyicide Server.js açın. Aşağıdaki bilgileri ekleyin:

    ```Javascript
    var config = require('./config');
    ```

3.  Yeni bir bölüm için Server.js ekleyin:

    ```Javascript
    // Pass these options in to the ODICBearerStrategy.
    var options = {
    // The URL of the metadata document for your app. Put the keys for token validation from the URL found in the jwks_uri tag in the metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array to hold signed-in users and the current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>12: Mongoose kullanarak MongoDB model ve şema bilgilerini ekleme
Ardından, bu üç dosyayı REST API hizmetinde bağlayın.

Bu makalede, bizim görevleri depolamak üzere Mongodb'yi kullanın. Bu konuda aşağıdakiler ele *4. adım*.

11. adımda oluşturduğunuz Config.js dosyasında veritabanınızın adlı *tasklist*. Ne mongoose_auth_local bağlantı URL'si sonuna eklediğiniz öğeydi. Bu veritabanını MongoDB'de önceden oluşturmanız gerekmez. Veritabanı (veritabanı zaten mevcut olduğu varsayılarak) sunucu uygulamasının ilk çalıştırılmasında oluşturulur.

Sunucuya hangi MongoDB veritabanını kullanacak şekilde söylediyse. Ardından, model ve şema sunucunuzun görevler için oluşturmak için bazı ek kod yazmanız gerekir.

### <a name="the-model"></a>Modeli
Şema modeli oldukça basittir. Gerekirse genişletebilirsiniz. 

Şema modeli bu değerlere sahiptir:

*   **AD**. Göreve atanan kişi. Bu bir **dize** değeri.
*   **GÖREV**. Görev adı. Bu bir **dize** değeri.
*   **TARİH**. Görev son tarihi. Bu bir **datetime** değeri.
*   **TAMAMLANAN**. Görev olup olmadığını tamamlandı. Bu bir **Boolean** değeri.

### <a name="create-the-schema-in-the-code"></a>Kod içinde şema oluşturma
1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Server.js, düzenleyicisinde açın. Aşağıdaki bilgileri yapılandırma girdisi ekleyin:

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

Bu kod MongoDB sunucusuna bağlanır. Ayrıca, bir şema nesnesi döndürür.

#### <a name="using-the-schema-create-your-model-in-the-code"></a>Şeması'nı kullanarak, modelinizi kod oluşturma
Önceki kodun altına aşağıdaki kodu ekleyin:

```Javascript
// Create a basic schema to store your tasks and users.
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

İlk koddan anlayabilirsiniz gibi şemanızı oluşturun. Ardından, bir model nesnesi oluşturun. Model nesnesi tanımlarken kod genelindeki verilerinizi depolamak için kullandığınız, **yollar**.

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a>13: görev REST API sunucunuz için yollar
Çalışmak için bir veritabanı modeliniz olduğuna göre REST API sunucunuz için kullanacağınız yolları ekleyin.

### <a name="about-routes-in-restify"></a>Yollar hakkında restify
Yollar restify iş tam olarak Express yığınını kullandıklarında yaparlar aynı şekilde. Yolları, istemci uygulamalarının çağırmasını beklediğiniz URI'yi kullanarak tanımlarsınız. Genellikle, yollarınızı ayrı bir dosyaya tanımlayın. Bu öğreticide, biz Server.js bizim yollar yerleştirin. Üretim kullanımı için kendi dosyasına yollar faktörü öneririz.

Bir restify yolu için genel bir desen aşağıdaki gibidir:

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep the server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


En temel düzeyde düzeni budur. Restify (ve hızlı) uygulama türleri ve farklı uç noktalar arasında karmaşık yönlendirme tanımlama yeteneği gibi çok daha kapsamlı işlevsellik sağlar.

#### <a name="add-default-routes-to-your-server"></a>Sunucunuza varsayılan yollar ekleme
Temel CRUD yollarını ekleyin: **oluşturma**, **almak**, **güncelleştirme**, ve **silmek**.

1.  Bir komut isteminde dizini değiştirin **azuread'i**:

    `cd azuread`

2.  Bir düzenleyicide Server.js açın. Aşağıda, daha önce gerçekleştirdiğiniz veritabanı girişlerinin, aşağıdaki bilgileri ekleyin:

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow you to use MongoDB Server as your response to any origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it to MongoDB.
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
    /// Returns the list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you to set default headers.
    // These headers comply with CORS, and allow us to use MongoDB Server as our response to any origin.
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
    log.warn(err, "There are no tasks in the database. Add one!");
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

### <a name="add-error-handling-for-the-routes"></a>Yollar için hata işleme ekleme
İstemciye, karşılaşılan sorunla ilgili iletişim kurabilmesi bazı hata işleme ekleyin.

Zaten yazdığınız kodun altına aşağıdaki kodu ekleyin:

```Javascript
///--- Errors for communicating something more information back to the client.
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


## <a name="14-create-your-server"></a>14: sunucunuzu oluşturma
Server örneğinizi eklemek için son bir şey yapmak için kullanılır. Sunucu örneği çağrılarınızı yönetir.

Restify (ve hızlı) bir REST API sunucusu ile kullanabileceğiniz kapsamlı özelleştirme sahip. Bu öğreticide, en temel kurulumu kullanırız.

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst to 10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-the-routes-without-authentication-for-now"></a>15: (kimlik doğrulaması olmadan, şimdilik) yollar ekleme
```Javascript
/// Use CRUD to add the real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in the pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need to maintain session state. You can experiment with removing API protection.
/* To do this, remove the passport.authenticate() method:
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
## <a name="16-run-the-server"></a>16: sunucunun çalıştırın
Kimlik doğrulaması eklemeden önce sunucunuzu test etmek için iyi bir fikirdir.

Sunucunuzu test etmek için en kolay yolu, bir komut isteminde curl kullanarak gerçekleşir. Bunu yapmak için çıktıyı JSON olarak ayrıştırmanıza için kullanabileceğiniz basit bir yardımcı program gerekir. 

1.  Aşağıdaki örneklerde kullanırız JSON aracını yükleyin:

    `$npm install -g jsontool`

    Bu işlem, JSON aracını global olarak yükler.

2.  MongoDB örneğinizin çalıştığından emin olun:

    `$sudo mongod`

3.  Dizinine değiştirin **azuread'i**, ve ardından curl çalıştırın:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  Bir görev eklemek için:

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

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

5.  Liste görevler Brandon için:

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Bu komutlar hatasız çalıştırırsanız, OAuth REST API sunucusuna eklemek hazırsınız.

*Şimdi MongoDB bir REST API sunucusuna sahipsiniz!*

## <a name="17-add-authentication-to-your-rest-api-server"></a>17: REST API sunucunuza kimlik doğrulaması ekleme
Çalışan bir REST API sahip olduğunuza göre bunu Azure AD ile kullanmak şekilde ayarlayın.

Bir komut isteminde dizini değiştirin **azuread'i**:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a>Passport-azure-ad ile eklemiştir oıdcbearerstrategy'yi kullanma
Şu ana kadar herhangi türde bir yetkilendirme olmadan genel bir REST TODO sunucusu temel aldık. Şimdi, kimlik doğrulama ekleyin.

İlk olarak, Passport'u kullanmak istediğinizi belirtin. Bu hak, önceki sunucu yapılandırması sonra koyun:

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> API'leri yazarken her zaman veri benzersiz kullanıcının yanılmayacağı belirteçten bağlamak için iyi bir fikirdir. Bu sunucu, Yapılacaklar öğelerini depoladığında, bunları kullanıcı abonelik kimliği (token.sub olarak adlandırılır) belirteci göre depolar. "Sahip" alanına token.sub yerleştirin. Bu, yalnızca o kullanıcıyı kullanıcının TODOs erişebilmesini sağlar. Girilen TODOs başka hiç kimse erişemez. "Sahip" API'sinde hiçbir Etkilenme yok Bir dış kullanıcı kimlikleri doğrulanır olsa bile diğer kullanıcıların TODOs isteyebilir.
> 
> 

Ardından, birlikte Aç kimliği bağlanmak taşıyıcı stratejisi kullanın `passport-azure-ad`. Bu, ne önceki yapıştırdığınız sonra koyun:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users.
/*
/* Because of the Passport pattern, you need to manage users and info tokens
/* with a FindorCreate() method. The method must be provided by the implementor.
/* In the following code, you autoregister any user and implement a FindById().
/* It's a good idea to do something more advanced.
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
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
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

Passport, tüm kendi stratejileri (Twitter, Facebook vb.) benzer bir desen kullanır. Tüm strateji yazıcıları desene bağlı kalır. Stratejisi geçirmek bir `function()` bir belirteç kullanan ve `done` parametre olarak. Tüm iş yaptıktan sonra strateji döndürülür. Kullanıcıyı depolayın ve böylece bunları yeniden istemeniz gerekmez belirteci kaydedin;.

> [!IMPORTANT]
> Önceki kod sunucunuza doğrulanabilir herhangi bir kullanıcı alır. Bu otomatik kaydı bilinir. Bir üretim sunucusunda herkes bunları seçtiğiniz bir kayıt sürecinden geçerler gerekmeden let istemezsiniz. Bu genellikle tüketici uygulamalarında görürsünüz düzeni olur. Uygulama, Facebook ile kaydetmenize olanak sağlayabilir, ancak ek bilgileri girmenizi ister. Bu öğretici için bir komut satırı programı doğru kullanıyorsanız, döndürülen belirteç nesnesinden e-posta ayıklanamıyor. Sonra ek bilgilerini girmesini isteyebilir. Bu bir test sunucusu olduğundan, bellek içi veritabanına doğrudan kullanıcı ekleyin.
> 
> 

### <a name="protect-endpoints"></a>Uç noktaları koruma
Uç noktaları belirterek koruma **passport.authenticate()** kullanmak istediğiniz protokolü ile çağırın.

Daha gelişmiş bir seçenek için sunucu kodunuzdaki, rota düzenleyebilirsiniz:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a>18: sunucu uygulamanızı yeniden çalıştırma
Curl noktalarınızı karşı OAuth 2.0 koruma olup olmadığını görmek için yeniden kullanın. Bu uç noktaya karşı herhangi bir istemci SDK'ları çalıştırmadan önce bunu yapabilirsiniz. Döndürülen üst bilgiler, kimlik doğrulama düzgün çalışıp çalışmadığını söyleyecektir.

1.  MongoDB isntance çalıştığından emin olun:

    `$sudo mongod`

2.  Dönüştür **azuread'i** dizin ve kullanım curl:

    `$ cd azuread`

    `$ node server.js`

3.  Temel bir POST deneyin:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

Bir 401 yanıtına Passport katmanının tam olarak ne olduğu uç noktayı yetkilendirmek için yeniden yönlendirme yaptığını gösterir.

*OAuth 2.0 kullanan bir REST API hizmet artık elinizde!*

Bir OAuth 2.0 ile uyumlu istemci kullanmadan bu sunucu ile kadar gitti. Bunun için ek bir öğretici gözden gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar
Başvuru için tamamlanan örnek (yapılandırma değerleriniz olmadan) olarak sağlanan [bir .zip dosyası](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip). Aynı zamanda Github'dan kopyalayabilirsiniz:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Şimdi, daha ileri seviyeli konulara geçebilirsiniz. Denemek isteyebilirsiniz [v2.0 uç noktası ile bir Node.js web uygulaması güvenli](active-directory-v2-devquickstarts-node-web.md).

Bazı ek kaynaklar aşağıda verilmiştir:

* [Azure AD v2.0 Geliştirici Kılavuzu](active-directory-appmodel-v2-overview.md)
* [Yığın taşması "azure-active-directory" etiketi](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma
Güvenlik olayları oluştuğunda bildirim almak için kaydolun öneririz. Üzerinde [Microsoft Teknik Güvenlik bildirimleri](https://technet.microsoft.com/security/dd252948) sayfasında, güvenlik danışma uyarılara abone.

