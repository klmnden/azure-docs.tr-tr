<properties
    pageTitle="Node.js ile Azure Redis Önbelleği kullanma | Microsoft Azure"
    description="Node.js ve node_redis kullanarak Azure Redis Önbelleği kullanmaya başlama"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="05/31/2016"
    ms.author="sdanie"/>

# Node.js ile Azure Redis Önbelleği kullanma

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Önbelleği, Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu konu Node.js kullanarak Azure Redis Önbelleği kullanmayı gösterir. Node.js ile Azure Redis Önbelleği’nin başka bir örneği için, bkz. [Azure Web Sitesinde Socket.IO ile bir Node.js Sohbet Uygulaması Oluşturma](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## Ön koşullar

[node_redis](https://github.com/mranney/node_redis) yükleyin:

    npm install redis

Bu öğreticide [node_redis](https://github.com/mranney/node_redis) kullanılmıştır, ancak [http://redis.io/clients](http://redis.io/clients) adresinde listelenmiş herhangi bir Node.js istemcisini kullanabilirsiniz.

## Azure’da Redis önbelleği oluşturma

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Ana bilgisayar adını ve erişim anahtarlarını alma

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## SSL olmayan uç noktayı etkinleştirme

Bazı Redis istemcileri SSL’yi desteklemez ve varsayılan olarak [SSL olmayan bağlantı noktası yeni Azure Redis Önbelleği örnekleri için devre dışıdır](cache-configure.md#access-ports). Bu yazma sırasında, [node_redis](https://github.com/mranney/node_redis) istemcisi SSL’yi desteklemez. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Önbelleğe bir şey ekleme ve bunu alma

      var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Çıktı:

    OK
    value


## Sonraki adımlar

- Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics).
- Resmi [Redis belgeleri](http://redis.io/documentation)ni okuyun.






<!---HONumber=Jun16_HO2-->


