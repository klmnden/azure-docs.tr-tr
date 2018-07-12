---
title: Node.js ile Azure Redis Cache’in nasıl kullanılacağı hakkında bilgi edinmek için hızlı başlangıç | Microsoft Docs
description: Bu hızlı başlangıçta Node.js ve node_redis ile Azure Redis Cache kullanmayı öğreneceksiniz.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: quickstart
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 05/21/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 8f71feb610884af29bdfbf170cfc411f32c50233
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38700791"
---
# <a name="quickstart-how-to-use-azure-redis-cache-with-nodejs"></a>Hızlı Başlangıç: Node.js ile Azure Redis Cache kullanma



Azure Redis Cache, Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu konu Node.js kullanarak Azure Redis Cache kullanmayı gösterir. 

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

![Önbellek uygulaması tamamlandı](./media/cache-nodejs-get-started/cache-app-complete.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar
[node_redis](https://github.com/mranney/node_redis) yükleyin:

    npm install redis

Bu öğreticide, [node_redis](https://github.com/mranney/node_redis) kullanılmaktadır. Diğer Node.js istemcilerini kullanmaya ilişkin örnekler için [Node.js Redis istemcileri](http://redis.io/clients#nodejs) listesindeki Node.js istemcilerinin kendi belgelerine bakın.


## <a name="create-a-cache"></a>Bir önbellek oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](../../includes/redis-cache-access-keys.md)]


**ANA BİLGİSAYAR ADI** ve **Birincil** erişim anahtarınız için ortam değişkenleri ekleyin. Hassas bilgileri kodunuza doğrudan eklemek yerine kodunuzdaki bu değişkenleri kullanacaksınız.

```
set REDISCACHEHOSTNAME=contosoCache.redis.cache.windows.net
set REDISCACHEKEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```


## <a name="connect-to-the-cache"></a>Önbelleğe bağlanma

[node_redis](https://github.com/mranney/node_redis)’in en son derlemeleri, Azure Redis Cache’e SSL kullanarak bağlanma konusunda destek sağlar. Aşağıdaki örnekte, 6380 SSL bitiş noktasını kullanarak Azure Redis Cache’e nasıl bağlanılacağı gösterilmektedir. 

```js
var redis = require("redis");

// Add your cache name and access key.
var client = redis.createClient(6380, process.env.REDISCACHEHOSTNAME,
    {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
```

Kodunuzdaki her işlem için yeni bağlantı oluşturmayın. Bunun yerine, mümkün olduğunca bağlantıları yeniden kullanın. 

## <a name="create-a-new-nodejs-app"></a>Yeni bir Node.js uygulaması oluşturma

*redistest.js* adlı yeni bir betik dosyası oluşturun.

Aşağıdaki örnek JavaScript’i dosyaya ekleyin. Bu kod, önbellek ana bilgisayar adı ve önemli ortam değişkenleri kullanarak Azure Redis Cache örneğine nasıl bağlanılacağını gösterir. Kod ayrıca önbellekte bir dize değeri depolar ve alır. Ayrıca `PING` ve `CLIENT LIST` komutları da yürütüldü. Redis’i [node_redis](https://github.com/mranney/node_redis) istemcisiyle kullanmaya ilişkin daha fazla örnek için bkz. [http://redis.js.org/](http://redis.js.org/).

```js
var redis = require("redis");
var bluebird = require("bluebird");

bluebird.promisifyAll(redis.RedisClient.prototype);
bluebird.promisifyAll(redis.Multi.prototype);

async function testCache() {

    // Connect to the Redis cache over the SSL port using the key.
    var cacheConnection = redis.createClient(6380, process.env.REDISCACHEHOSTNAME, 
        {auth_pass: process.env.REDISCACHEKEY, tls: {servername: process.env.REDISCACHEHOSTNAME}});
        
    // Perform cache operations using the cache connection object...

    // Simple PING command
    console.log("\nCache command: PING");
    console.log("Cache response : " + await cacheConnection.pingAsync());

    // Simple get and put of integral data types into the cache
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    console.log("\nCache command: SET Message");
    console.log("Cache response : " + await cacheConnection.setAsync("Message",
        "Hello! The cache is working from Node.js!"));    

    // Demostrate "SET Message" executed as expected...
    console.log("\nCache command: GET Message");
    console.log("Cache response : " + await cacheConnection.getAsync("Message"));    

    // Get the client list, useful to see if connection list is growing...
    console.log("\nCache command: CLIENT LIST");
    console.log("Cache response : " + await cacheConnection.clientAsync("LIST"));    
}

testCache();
```

Node.js ile betiği çalıştırın.

```
node redistest.js
```

Aşağıdaki örnekte, `Message` anahtarının Azure portaldaki Redis Konsolu kullanılarak ayarlanan, önceden önbelleğe alınmış bir değer içerdiğini görebilirsiniz. Uygulama, önbelleğe alınan bu değeri güncelleştirdi. Ayrıca uygulama, `PING` ve `CLIENT LIST` komutlarını da yürüttü.

![Önbellek uygulaması tamamlandı](./media/cache-nodejs-get-started/cache-app-complete.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticiyle devam edecekseniz, bu hızlı başlangıçta oluşturulan kaynakları tutabilir ve sonraki öğreticide yeniden kullanabilirsiniz.

Aksi takdirde, hızlı başlangıç örnek uygulamasını tamamladıysanız ücret yansıtılmaması için bu hızlı başlangıçta oluşturulan Azure kaynaklarını silebilirsiniz. 

> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
>

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu makaledeki yönergelerde *TestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

![Sil](./media/cache-nodejs-get-started/cache-delete-resource-group.png)

Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.



## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Node.js uygulamasından Azure Redis Cache’in nasıl kullanılacağını öğrendiniz. Bir ASP.NET web uygulaması ile Redis Cache’i kullanmaya ilişkin sonraki hızlı başlangıca ilerleyin.

> [!div class="nextstepaction"]
> [Azure Redis Cache kullanan bir ASP.NET web uygulaması oluşturun.](./cache-web-app-howto.md)



