---
title: Azure AD ile Web API'sinin güvenliğini sağlama | Microsoft Docs
description: Kimlik doğrulama için Azure AD ile tümleştirilen bir Node.js REST web API'si oluşturmayı öğrenin.
services: active-directory
documentationcenter: nodejs
author: CelesteDG
manager: mtillman
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f72cbd719cea585144be3757f0791a74bde452ab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299201"
---
# <a name="quickstart-secure-a-web-api-with-azure-active-directory"></a>Hızlı Başlangıç: Web API'si Azure Active Directory ile güvenli hale getirme

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

Bu hızlı başlangıçta, Azure Active Directory (Azure AD) ile iletişimi işlemek üzere [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad) modülünü kullanarak [Passport](http://passportjs.org/) ile [Restify](http://restify.com/) API'sinin güvenliğini sağlamayı öğreneceksiniz.

Bu hızlı başlangıç, API uç noktalarının güvenliğiyle ilgili konuları kapsar. Oturum açma ve kimlik doğrulama belirteçlerini alıkoyma konuları ele alınmaz. Bunlar istemci uygulamasının sorumluluğundadır. İstemci uygulamasıyla ilişkili ayrıntılar için, [Azure AD ile Node.js web uygulaması oturumu açma ve oturumunu kapatma](quickstart-v1-openid-connect-code.md).

Bu makaleyle ilişkilendirilmiş kod örneğinin tamamı [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi-basic)'da sağlanır.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şu önkoşulları tamamlayın.

### <a name="create-the-sample-project"></a>Örnek projeyi oluşturun

Sunucu uygulamasının hem Restify ile Passport'u desteklemek için birkaç paket bağımlılığına hem de Azure AD'ye geçirilen hesap bilgilerine ihtiyacı vardır.

Başlamak için, aşağıdaki kodu `package.json` adlı dosyaya ekleyin:

```Shell
{
  "name": "active-directory-webapi-nodejs",
  "version": "0.0.1",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "passport": "0.4.0",
    "passport-azure-ad": "4.0.0",
    "restify": "7.7.0"
  }
}
```

`package.json` oluşturulduktan sonra, komut isteminizde `npm install` komutunu çalıştırarak paket bağımlılıklarını yükleyin.

#### <a name="configure-the-project-to-use-active-directory"></a>Active Directory'yi kullanmak için projeyi yapılandırın

Uygulamayı yapılandırmaya başlamak için, Azure CLI'dan alabileceğiniz hesaba özgü birkaç değer vardır. CLI'yı kullanmaya başlamanın en kolay yolu, Azure Cloud Shell'i kullanmaktır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bulut kabuğuna aşağıdaki komutu girin:

```azurecli-interactive
az ad app create --display-name node-aad-demo --homepage http://localhost --identifier-uris http://node-aad-demo
```

`create` komutunun [bağımsız değişkenleri](/cli/azure/ad/app?view=azure-cli-latest#az-ad-app-create) şunlardır:

| Bağımsız Değişken  | Açıklama |
|---------|---------|
|`display-name` | Kaydın kolay adı |
|`homepage` | Kullanıcıların oturum açıp uygulamanızı kullanabileceği Url |
|`identifier-uris` | Azure AD'nin bu uygulama için kullanabileceği boşlukla ayrılmış benzersiz URI'ler |

Azure Active Directory'ye bağlanabilmeniz için önce şu bilgilere ihtiyacınız vardır:

| Ad  | Açıklama | Yapılandırma Dosyasında Değişken Adı |
| ------------- | ------------- | ------------- |
| Kiracı Adı  | Kimlik doğrulaması için kullanmak istediğiniz [kiracı adı](quickstart-create-new-tenant.md) | `tenantName`  |
| İstemci Kimliği  | İstemci kimliği, AAD _Uygulama Kimliği_ için kullanılan OAuth terimidir. |  `clientID`  |

Azure Cloud Shell'deki kayıt yanıtından `appId` değerini kopyalayın ve `config.js` adlı yeni bir dosya oluşturun. Ardından, aşağıdaki kodu ekleyin; kendi değerlerinizi köşeli ayraç içindeki belirteçlerle değiştirin:

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

Tek tek yapılandırma ayarlarıyla ilgili daha fazla bilgi için, [passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modülünün belgelerini gözden geçirin.

### <a name="implement-the-server"></a>Sunucuyu uygulama

[Passport-azure-ad](https://github.com/AzureAD/passport-azure-ad#5-usage) modül iki kimlik doğrulama stratejileri özellikleri: [OIDC](https://github.com/AzureAD/passport-azure-ad#51-oidcstrategy) ve [taşıyıcı](https://github.com/AzureAD/passport-azure-ad#52-bearerstrategy) stratejiler. Bu makalede uygulanan sunucu, API uç noktasının güvenliğini sağlamak için Taşıyıcı stratejisini kullanır.

### <a name="step-1-import-dependencies"></a>1. Adım: İçeri aktarma bağımlılıkları

`app.js` adlı yeni bir dosya oluşturun ve aşağıdaki metni yapıştırın:

```JavaScript
const
      restify = require('restify')
    , restifyPlugins = require ('restify').plugins
    , passport = require('passport')
    , BearerStrategy = require('passport-azure-ad').BearerStrategy
    , config = require('./config')
    , authenticatedUserTokens = []
    , serverPort = process.env.PORT || config.serverPort
;
```

Kodun bu bölümünde:

- `restify` Ve eklentileri modülleri bir Restify sunucusu kurmak için başvuru.
- Azure AD ile iletişim kurmak `passport` ve `passport-azure-ad` modüllerinin sorumluluğundadır.
- `config` değişkeni, önceki adımda oluşturulan `config.js` dosyasından alınan değerlerle başlatılır.
- Kullanıcı belirteçleri güvenli uç noktalara geçirilirken bu belirteçleri depolamak üzere `authenticatedUserTokens` için bir dizi oluşturulur.
- `serverPort`, işlem ortamının bağlantı noktasından veya yapılandırma dosyasından tanımlanır.

### <a name="step-2-instantiate-an-authentication-strategy"></a>2. Adım: Kimlik doğrulama stratejisi örneği

Uç noktayı güvenlik altına alırken, geçerli isteklerin kimliği doğrulanmış bir kullanıcıdan kaynaklanıp kaynaklanmadığını saptamaktan sorumlu olacak bir strateji sağlamalısınız. Burada, `authenticatonStrategy` değişkeni `passport-azure-ad` `BearerStrategy` sınıfının bir örneğidir. `require` deyiminin arkasında aşağıdaki kodu ekleyin.

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

Bu uygulama, kimlik doğrulama belirteçlerini `authenticatedUserTokens` dizisine ekleyerek (henüz eklenmemişse) otomatik kayıt kullanır.

Stratejinin yeni örneği oluşturulduktan sonra, bunu `use` yöntemi üzerinden Passport'a geçirmelisiniz. Stratejiyi Passport'ta kullanmak için aşağıdaki kodu `app.js` dosyasına ekleyin.

```JavaScript
passport.use(authenticationStrategy);
```

### <a name="step-3-server-configuration"></a>3. Adım: Sunucu yapılandırması

Kimlik doğrulama stratejisi tanımlandıktan sonra, artık bazı temel ayarlarla Restify sunucusunu ayarlayabilir ve güvenlik için Passport kullanılmasını sağlayabilirsiniz.

```JavaScript
const server = restify.createServer({ name: 'Azure Active Directory with Node.js Demo' });
server.use(restifyPlugins.authorizationParser());
server.use(passport.initialize());
server.use(passport.session());
```
Bu sunucu, kimlik doğrulama üst bilgilerini ayrıştırmak ve ardından Passport kullanılacak şekilde ayarlamak için başlatılır ve yapılandırılır.

### <a name="step-4-define-routes"></a>4. Adım: Yolları tanımlayın

Şimdi yolları tanımlayabilir ve Azure AD ile hangisinin güvenlik altına alınacağına karar verebilirsiniz. Bu projede iki yol vardır; kök düzeyi açıktır ve `/api` yolu kimlik doğrulaması gerektirecek şekilde ayarlanmıştır.

`app.js` dosyasına kök düzeyi yolu için aşağıdaki kodu ekleyin:

```JavaScript
server.get('/', (req, res, next) => {
    res.send(200, 'Try: curl -isS -X GET http://127.0.0.1:3000/api');
    next();
});
```

Kök yol tüm isteklerin yoldan geçmesine izin verir ve `/api` yolunu test etmeye yönelik komutu içeren bir ileti döndürür. Buna karşılık, `/api` yolu [`passport.authenticate`](http://passportjs.org/docs/authenticate) kullanılarak kilitlenmiştir. Aşağıdaki kodu kök yolun arkasına ekleyin.

```JavaScript
server.get('/api', passport.authenticate('oauth-bearer', { session: false }), (req, res, next) => {
    res.json({ message: 'response from API endpoint' });
    return next();
});
```

Bu yapılandırma yalnızca taşıyıcı belirteç içeren kimliği doğrulanmış isteklere `/api` için erişim verir. Her istekle birlikte API'ye belirteç geçirilmesini gerekli kılmak için `session: false` seçeneği kullanılarak oturumlar devre dışı bırakılır.

Son olarak, `listen` yöntemi çağrılarak sunucu yapılandırılmış bağlantı noktasından dinleyecek şekilde ayarlanır.

```JavaScript
server.listen(serverPort);
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Artık sunucu uygulandığına göre, bir komut istemi açıp şunu girerek sunucuyu başlatabilirsiniz:

```shell
npm start
```

Sunucu çalışırken, sonuçları test etmek için sunucuya bir istek gönderebilirsiniz. Kök yoldan gelen yanıtı göstermek için, bir bash kabuğu açın ve aşağıdaki kodu girin:

```shell
curl -isS -X GET http://127.0.0.1:3000/
```

Sunucunuzu doğru yapılandırdıysanız, yanıt şuna benzer olmalıdır:

```shell
HTTP/1.1 200 OK
Server: Azure Active Directory with Node.js Demo
Content-Type: application/json
Content-Length: 49
Date: Tue, 10 Oct 2017 18:35:13 GMT
Connection: keep-alive

Try: curl -isS -X GET http://127.0.0.1:3000/api
```

Ardından, bash kabuğunuza aşağıdaki komutu girerek kimlik doğrulama gerektiren yolu test edebilirsiniz:

```shell
curl -isS -X GET http://127.0.0.1:3000/api
```

Sunucuyu doğru yapılandırdıysanız, sunucu `Unauthorized` durumuyla yanıt vermelidir.

```shell
HTTP/1.1 401 Unauthorized
Server: Azure Active Directory with Node.js Demo
WWW-Authenticate: token is not found
Date: Tue, 10 Oct 2017 16:22:03 GMT
Connection: keep-alive
Content-Length: 12

Unauthorized
```

Artık güvenli bir API oluşturduğunuza göre, API'ye kimlik doğrulama belirteçleri geçirebilen bir istemci uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Oturum açma, oturum kapatma ve belirteçleri yönetme işlemlerini yapan sunucuya bağlanmak için istemci tarafını uygulamalısınız. Kod tabanlı örnekler için, [iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios) ve [Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)'de istemci uygulamalarına bakın.
* Adım adım öğretici için bkz. [Azure AD ile Node.js web uygulaması oturumunu açma ve kapatma](quickstart-v1-openid-connect-code.md).
