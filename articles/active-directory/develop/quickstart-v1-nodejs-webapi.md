---
title: Azure AD Node.js web API'sini kullanmaya başlama | Microsoft Docs
description: Node.js REST web kimlik doğrulaması için Azure AD ile tümleşen bir API oluşturmak nasıl.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 3b203e5be82c01e7d586c90bae454aca23ebd630
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581717"
---
# <a name="azure-ad-nodejs-web-api-getting-started"></a>Azure AD Node.js web API'sini kullanmaya başlama

Bu makalede güvenliğinin nasıl sağlanacağını gösterir. bir [restify'ı](http://restify.com/) API uç noktası ile [Passport](http://passportjs.org/) kullanarak [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) modülü, Azure Active ile iletişimi işlemek için Directory (AAD). 

Bu öğreticinin kapsamı konuları kapsayan güvenliğini sağlama API uç noktaları ile ilgili. Oturum açma ve kimlik doğrulama belirteçlerinizi koruma endişelere yer bırakmadan, burada uygulanmaz ve ve bir istemci uygulamanın sorumluluğudur. Bir istemci uygulaması çevreleyen ayrıntılarını gözden geçirin [Node.js web uygulamasına oturum açma ve Azure AD ile oturum kapatma](quickstart-v1-openid-connect-code.md).

Bu makalede ile ilişkili tam kod örneği kullanılabilir [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic).

## <a name="create-the-sample-project"></a>Örnek proje oluşturun
Bu sunucu uygulaması birkaç Paket bağımlılıklarını yanı sıra Restify ve Passport desteklemek için hesap için AAD geçirilen bilgilerini gerektirir.

Başlamak için adlı bir dosyaya aşağıdaki kodu ekleyin `package.json`:

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

### <a name="configure-the-project-to-use-active-directory"></a>Active Directory'yi kullanmak için proje yapılandırma

Uygulama yapılandırmaya başlamak için Azure CLI üzerinden edinebilirsiniz birkaç hesaba özel değerler vardır. CLI ile başlamanın en kolay yolu, Azure Cloud Shell'i kullanmaktır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Cloud shell'de aşağıdaki komutu girin: 

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

[Bağımsız değişkenleri](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) için `create` komutu ekleyin:

| Bağımsız değişken  | Açıklama |
|---------|---------|
|`display-name` | Kolay ad kaydı |
|`homepage` | Kullanıcılar oturum açabilir ve uygulamanızı nerede URL'si |
|`identifier-uris` | Azure AD'nin bu uygulama için kullanabileceği benzersiz bir URI'leri boşluk ayrılmış |

Azure Active Directory'ye bağlanabilmesi için önce aşağıdaki bilgilere ihtiyacınız vardır:

| Ad  | Açıklama | Yapılandırma dosyasında değişken adı |
| ------------- | ------------- | ------------- |
| Kiracı adı  | [Kiracı adı](quickstart-create-new-tenant.md) kimlik doğrulaması için kullanmak istediğiniz | `tenantName`  |
| İstemci Kimliği  | İstemci kimliği, AAD için kullanılan OAuth terimi: _uygulama kimliği_. |  `clientID`  |

Azure Cloud shell'de kayıt yanıttan kopyalama `appId` adlı yeni bir dosya oluşturun ve değeri `config.js`. Ardından, aşağıdaki kodu ekleyin ve değerlerinizi köşeli parantez içindeki belirteçleri ile değiştirin:

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
Bağımsız yapılandırma ayarları ile ilgili daha fazla bilgi için gözden [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modülü belgeleri.

## <a name="implement-the-server"></a>Uygulama sunucusu
[Passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modülü özellikleri iki kimlik doğrulama stratejileri: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) ve [taşıyıcı](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) stratejiler. Bu makalede gerçekleştirilen sunucu taşıyıcı stratejisi API uç noktası güvenli hale getirmek için kullanır.

### <a name="step-1-import-dependencies"></a>1. adım: Alma bağımlılıkları
Adlı yeni bir dosya oluşturun `app.js` aşağıdaki metni yapıştırın:

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

Bu kod bölümünde:

- `restify` Ve `restify-plugins` modülleri başvurulan bir Restify sunucusu kurmak için.

- `passport` Ve `passport-azure-ad` modüllerdir AAD ile iletişim kurmak için sorumlu.

- `config` Değişkeni değerlerle başlatılan `config.js` önceki adımda oluşturulan dosya.

- Bir dizi için oluşturulan `authenticatedUserTokens` güvenli uç noktalarına geçirilen kullanıcı simgeleri depolamak için.

- `serverPort` Ya da işlem ortamının bağlantı noktasından veya yapılandırma dosyasından tanımlanır.

### <a name="step-2-instantiate-an-authentication-strategy"></a>2. adım: kimlik doğrulama stratejisi örneği
Bir uç nokta güvenli olarak geçerli isteğin kimliği doğrulanmış bir kullanıcıdan kaynaklanan olup olmadığını belirlemek için sorumlu bir strateji sağlamanız gerekir. Burada `authenticatonStrategy` değişkendir örneği `passport-azure-ad` `BearerStrategy` sınıfı. Sonra aşağıdaki kodu ekleyin `require` deyimleri.

```JavaScript
const authenticationStrategy = new BearerStrategy(config.credentials, (token, done) => {
    let currentUser = null;

    let userToken = authenticatedUserTokens.find((user) => {
        currentUser = user;
        user.sub === token.sub;
    });

    if(!userToken) {
        authenticatedUserTokens.push(token);
    }

    return done(null, currentUser, token);
});
```
Bu uygulama, kimlik doğrulaması belirteçlere ekleyerek otomatik kaydı kullanır `authenticatedUserTokens` zaten mevcut değilse dizisi.

Stratejisinin yeni bir örneği oluşturulduktan sonra bunu Passport içine geçmelidir `use` yöntemi. Aşağıdaki kodu ekleyin `app.js` Passport stratejisini kullanma.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>3. adım: Sunucu yapılandırması
Artık tanımlanan kimlik doğrulama stratejisi ile bazı temel ayarlarla Restify sunucusu ayarlamak ve güvenlik için Passport'u kullanmak ayarlayın.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directroy with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Bu sunucu başlatılır ve yetkilendirme üstbilgileri ayrıştırmak için yapılandırılmış ve ardından Passport kullanacak şekilde ayarlanır.


### <a name="step-4-define-routes"></a>4. adım: yolları tanımlayın
Artık yolları tanımlayın ve AAD ile güvenli hale getirmek karar verebilirsiniz. Bu proje kök düzeyinde olduğu açık iki rotaları içerir ve `/api` rota, kimlik doğrulama isteyecek şekilde ayarlanır.

İçinde `app.js` kök düzey rota için aşağıdaki kodu ekleyin:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

Kök yolu rota üzerinden tüm istekleri sağlar ve test etmek için bir komut içeren bir ileti döndürür `/api` rota. Bunun aksine, `/api` yol kullanılarak kilitlenmemiş kilitli [ `passport.authenticate` ](http://passportjs.org/docs/authenticate). Kök yol sonra aşağıdaki kodu ekleyin.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Bu yapılandırma yalnızca bir taşıyıcı belirteç erişim dahil, kimliği doğrulanmış istekler verir `/api`. Seçeneği `session: false` bir belirteç API için her bir istekle geçirilen gerektirecek şekilde oturumlarını devre dışı bırakmak için kullanılır.

Son olarak, sunucunun çağırarak yapılandırılmış bağlantı noktasında dinleyecek şekilde ayarlanır `listen` yöntemi.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Sunucusu uygulanır, bir komut istemi'ni açarak sunucuyu başlatın ve girin:

```Shell
npm start
```

Çalıştıran sunucu ile test sonuçları için sunucuya gönderilen bir istek gönderebilirsiniz. Kök yol yanıtı göstermek için bir bash Kabuğu'nu açın ve aşağıdaki kodu girin:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/
```

Sunucunuz doğru şekilde yapılandırdıysanız, yanıt benzer şekilde görünmelidir:

```Shell
HTTP/1.1 200 OK
Server: Azure Active Directroy with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Ardından, bash kabuğunuz aşağıdaki komutu girerek kimlik doğrulaması gerektiren bir yol test edebilirsiniz:

```Shell 
curl -isS -X GET http://127.0.0.1:3000/api
```

Sunucunun doğru şekilde yapılandırdıktan sonra sunucu durum koduyla yanıt vermesi gerekir `Unauthorized`.

```Shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directroy with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```
Güvenli bir API oluşturduğunuza göre API için kimlik doğrulama belirteçlerinizi geçirebilmek için bir istemci uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Giriş belirtildiği gibi oturum açma, oturum kapatma ve belirteçleri yönetme işleyen sunucuya bağlanmak için bir istemci karşılığı uygulamalıdır. Kod tabanlı örnekler için istemci uygulamalara yönlendirebiliriz [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) ve [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android). Adım adım bir öğretici için şu makaleye başvurun:

> [!div class="nextstepaction"]
> [Node.js web uygulamasına oturum açma ve Azure AD ile oturum kapatma](quickstart-v1-openid-connect-code.md)