<properties
    pageTitle="Python ile Azure Redis Önbelleği kullanma | Microsoft Azure"
    description="Python kullanarak Azure Redis Önbelleği kullanmaya başlama"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="05/31/2016"
    ms.author="sdanie"/>

# Python ile Azure Redis Önbelleği kullanma

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Bu konu Python kullanarak Azure Redis Önbelleği kullanmayı gösterir.


## Ön koşullar

[redis-py](https://github.com/andymccurdy/redis-py) yükleyin.


## Azure’da Redis önbelleği oluşturma

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Ana bilgisayar adını ve erişim anahtarlarını alma

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## SSL olmayan uç noktayı etkinleştirme

Bazı Redis istemcileri SSL’yi desteklemez ve varsayılan olarak [SSL olmayan bağlantı noktası yeni Azure Redis Önbelleği örnekleri için devre dışıdır](cache-configure.md#access-ports). Bu yazma sırasında, [redis-py](https://github.com/andymccurdy/redis-py) istemcisi SSL’yi desteklemez. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Önbelleğe bir şey ekleme ve bunu alma


    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Önbellek adınızı `<name>` ile ve erişim anahtarınızı `key` ile değiştirin.


<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png



<!--HONumber=Jun16_HO2-->


