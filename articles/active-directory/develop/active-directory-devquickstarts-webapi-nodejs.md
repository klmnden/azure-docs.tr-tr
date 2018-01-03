---
title: "Azure AD Node.js web API'si Başlarken | Microsoft Docs"
description: "Node.js REST web kimlik doğrulaması için Azure AD ile tümleştirilir API'si oluşturma."
services: active-directory
documentationcenter: nodejs
author: craigshoemaker
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: cshoe
ms.custom: aaddev
ms.openlocfilehash: 411f646574af2f86621cbb3cd7175b6a9478972a
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="azure-ad-nodejs-web-api-getting-started"></a>Azure AD Node.js web API'si Başlarken

Bu makalede, güvenli hale getirmek gösterilmiştir bir [Restify](http://restify.com/) API uç noktası ile [Passport](http://passportjs.org/) kullanarak [passport azure ad](https://github.com/AzureAD/passport-azure-ad) Azure Active ile iletişim işlemek için Modülü Directory (AAD). 

Bu öğretici kapsamında sorunları kapsamaktadır güvenliğini sağlama API uç noktaları ile ilgili. Oturum açma ve kimlik doğrulama belirteçleri korunuyor endişelere burada uygulanmaz ve bir istemci uygulaması sorumluluğundadır. Bir istemci uygulaması çevreleyen ayrıntılarını gözden [Node.js web uygulamasına oturum açma ve oturum kapatma Azure AD ile](active-directory-devquickstarts-openidconnect-nodejs.md).

Bu makale ile ilişkili tam kod örneği kullanılabilir [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="create-the-sample-project"></a>Örnek Proje oluşturma
Bu sunucu uygulamasının yanı sıra Restify ve Passport desteklemek için AAD geçirilen bilgi hesap için birkaç paket bağımlılıkları gerektirir.

Başlamak için aşağıdaki kodu adlı bir dosyaya ekleyin `package.json`:

```Shell
{
  "name": "node-aad-demo",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "3.0.8",
    "restify": "6.0.1",
    "restify-plugins": "1.6.0"
  }
}
```

Bir kez `package.json` oluşturulan çalıştırmak `npm install` Paket bağımlılıklarını yüklemek için komut isteminde. 

### <a name="configure-the-project-to-use-active-directory"></a>Active Directory kullanmak üzere proje yapılandırma

Uygulamayı yapılandırma başlamak için Azure CLI üzerinden edinebilirsiniz birkaç hesaba özel değer vardır. CLI ile çalışmaya başlamak için en kolay yolu, Azure bulut kabuğunu kullanmaktır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bulut Kabuğu'nda aşağıdaki komutu girin: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

[Bağımsız değişkenleri](/cli/azure/ad/app?view=azure-cli-latest#az_ad_app_create) için `create` komutu ekleyin:

| Bağımsız değişken  | Açıklama |
|---------|---------|
|`display-name` | Kayıt kolay adı |
|`homepage` | Url, kullanıcıların oturum açmak ve uygulamanızın kullanın |
|`identifier-uris` | Azure AD'nin bu uygulama için kullanabileceği benzersiz URI'ler alanı ayrılmış |

Azure Active Directory'ye bağlanmadan önce aşağıdaki bilgiler gereklidir:

| Ad  | Açıklama | Yapılandırma dosyasındaki değişken adı |
| ------------- | ------------- | ------------- |
| Kiracı adı  | [Kiracı adı](active-directory-howto-tenant.md) kimlik doğrulaması için kullanmak istediğiniz | `tenantName`  |
| İstemci Kimliği  | İstemci kimliği, AAD için kullanılan OAuth terim: _uygulama kimliği_. |  `clientID`  |

Azure bulut Kabuğu'nda kayıt yanıttan kopyalama `appId` adlı yeni bir dosya oluşturun ve değeri `config.js`. Ardından, aşağıdaki kodu ekleyin ve değerlerinizi köşeli parantez içindeki belirteçleri ile değiştirin:

```JavaScript
const tenantName    = //<YOUR_TENANT_NAME>;
const clientID      = //<YOUR_APP_ID_FROM_CLOUD_SHELL>;
const serverPort    = 3000;

module.exports.serverPort = serverPort;

module.exports.credentials = {
  identityMetadata: `https://login.microsoftonline.com/${tenantName}.onmicrosoft.com/.well-known/openid-configuration`, 
  clientID: clientID
};
```
Tek tek yapılandırma ayarları ile ilgili daha fazla bilgi için gözden [passport azure ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modülü belgeleri.

## <a name="implement-the-server"></a>Uygulama sunucusu
[Passport azure ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modülü özellikleri iki kimlik doğrulama stratejileri: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) ve [taşıyıcı](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) stratejileri. Bu makalede gerçekleştirilen sunucu taşıyıcı stratejisi API uç noktası güvenli hale getirmek için kullanır.

### <a name="step-1-import-dependencies"></a>1. adım: Alma bağımlılıkları
Adlı yeni bir dosya oluşturun `app.js` ve aşağıdaki metni yapıştırın:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require('restify-plugins')
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

Bu bölümde kod:

- `restify` Ve `restify-plugins` modülleri başvurulan bir Restify sunucusu kurmak için.

- `passport` Ve `passport-azure-ad` modülleri AAD ile iletişim kurmak için sorumlu.

- `config` Değişkeni değerlerle başlatılır `config.js` önceki adımda oluşturduğunuz dosya.

- Bir dizi için oluşturulan `authenticatedUserTokens` güvenli uç noktalarına geçirilen kullanıcı belirteçleri depolamak için.

- `serverPort` Ya da işlem ortamı bağlantı noktasından veya yapılandırma dosyasından tanımlanır.

### <a name="step-2-instantiate-an-authentication-strategy"></a>2. adım: kimlik doğrulama stratejisi örneği
Bir uç noktası güvenli şekilde kimliği doğrulanmış bir kullanıcıdan geçerli İsteğin kaynaklandığı olup olmadığını belirlemek için sorumlu bir strateji sağlamanız gerekir. Burada `authenticatonStrategy` değişkenidir örneği `passport-azure-ad` `BearerStrategy` sınıfı. Sonra aşağıdaki kodu ekleyin `require` deyimleri.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let userToken = authenticatedUserTokens.find((user) => user.sub === token.sub);

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, user, token);
});
```
Bu uygulama kimlik doğrulama belirteçleri içine ekleyerek otomatik kaydı kullanan `authenticatedUserTokens` zaten mevcut değilse dizi.

Stratejisi yeni bir örneğini oluşturulduktan sonra onu Passport içine geçmelidir `use` yöntemi. Aşağıdaki kodu ekleyin `app.js` Passport stratejisi kullanmak için.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>3. adım: Sunucu yapılandırması
Şimdi tanımlanan kimlik doğrulama stratejisi ile bazı temel ayarlarla Restify sunucusu kurma ve güvenlik için Passport kullanacak şekilde ayarlayın.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Bu sunucu başlatıldı ve yetkilendirme üstbilgileri ayrıştırmak için yapılandırılır ve Passport kullanacak şekilde ayarlayın.


### <a name="step-4-define-routes"></a>4. adım: yolları tanımlayın
Şimdi yolları tanımlayın ve AAD ile güvenli hale getirmek karar verebilirsiniz. Bu proje kök düzeyinde olduğu açık iki rotaları içerir ve `/api` rota kimlik doğrulama gerektirecek şekilde ayarlanır.

İçinde `app.js` kök düzeyinde rota için aşağıdaki kodu ekleyin:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

Kök yol rota üzerinden tüm isteklere izin verir ve test etmek için bir komut içeren bir ileti döndürür `/api` rota. Bunun aksine, `/api` rota kullanarak aşağı kilitli [ `passport.authenticate` ](http://passportjs.org/docs/authenticate). Kök yol sonra aşağıdaki kodu ekleyin.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Bu yapılandırma yalnızca bir taşıyıcı belirteci erişim içerebilir kimliği doğrulanmış istekler verir `/api`. Seçeneğini `session: false` bir belirteç API için her istek ile geçirilen gerektirecek şekilde oturumlarını devre dışı bırakmak için kullanılır.

Son olarak, sunucunun çağırarak yapılandırılan bağlantı noktası üzerinde dinleme ayarlanır `listen` yöntemi.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Sunucusu uygulanır, bir komut istemi açarak sunucunun başlayın ve girin:

```Shell
npm start
```

Çalıştıran sunucuya, test sonuçları için sunucunun bir istek gönderebilir. Kök yol yanıttan tanıtmak için bir bash kabuğunda açın ve aşağıdaki kodu girin:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Sunucunuz doğru şekilde yapılandırdıysanız, yanıtı şuna benzemelidir:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Ardından, bash kabuğundan aşağıdaki komutu girerek kimlik doğrulaması gerektiren rota test edebilirsiniz:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Sunucunun doğru şekilde yapılandırdıktan sonra sunucu durum koduyla yanıt vermesi gerektiğini `Unauthorized`.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Güvenli bir API oluşturduğunuza göre API için kimlik doğrulama belirteçleri geçirebilmek için olan bir istemci uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Girişte belirtildiği gibi oturum açma, oturumunuzu ve belirteçleri yönetme işleyen sunucuya bağlanmak için bir istemci karşılık gelen uygulamalıdır. Kod tabanlı örnekler için istemci uygulamalarında bakabilirsiniz [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) ve [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Adım adım öğretici için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Node.js web uygulamasına oturum açma ve Azure AD ile oturum kapatma](active-directory-devquickstarts-openidconnect-nodejs.md)