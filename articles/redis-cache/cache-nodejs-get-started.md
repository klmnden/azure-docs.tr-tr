---
title: "Node.js ile Azure Redis Önbelleği kullanma | Microsoft v"
description: "Node.js ve node_redis kullanarak Azure Redis Önbelleği kullanmaya başlama"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 01/06/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 65385aa918222837468f88246d0527c22c677ba7
ms.openlocfilehash: 6de51407a78b3e33e6d3de544caeee714c9d1f4c


---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Node.js ile Azure Redis Önbelleği kullanma
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Önbelleği, Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu konu Node.js kullanarak Azure Redis Önbelleği kullanmayı gösterir. Node.js ile Azure Redis Önbelleği’nin başka bir örneği için, bkz. [Azure Web Sitesinde Socket.IO ile bir Node.js Sohbet Uygulaması Oluşturma](../app-service-web/web-sites-nodejs-chat-app-socketio.md).

## <a name="prerequisites"></a>Ön koşullar
[node_redis](https://github.com/mranney/node_redis) yükleyin:

    npm install redis

Bu öğreticide, [node_redis](https://github.com/mranney/node_redis) kullanılmaktadır. Diğer Node.js istemcilerini kullanmaya ilişkin örnekler için [Node.js Redis istemcileri](http://redis.io/clients#nodejs) listesindeki Node.js istemcilerinin kendi belgelerine bakın.

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ana bilgisayar adını ve erişim anahtarlarını alma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>SSL kullanarak güvenli bir şekilde önbelleğe bağlanma
[node_redis](https://github.com/mranney/node_redis)’in en son derlemeleri, Azure Redis Önbelleği’ne SSL kullanarak bağlanma konusunda destek sağlar. Aşağıdaki örnekte, 6380 SSL bitiş noktasını kullanarak Azure Redis Önbelleği’ne nasıl bağlanılacağı gösterilmektedir. Önceki [Ana bilgisayar adını ve erişim anahtarlarını alma](#retrieve-the-host-name-and-access-keys) bölümünde açıklanan şekilde `<name>` öğesini önbelleğinizin adı ile, `<key>` öğesini ise birincil veya ikincil anahtarınızla değiştirin.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Önbelleğe bir şey ekleme ve bunu alma
Aşağıdaki örnekte, size bir Azure Redis Önbelleği örneğine bağlanma, önbellekte öğe depolama ve önbellekten öğe alma işlemlerinin nasıl yapılacağı gösterilmektedir. Redis’i [node_redis](https://github.com/mranney/node_redis) istemcisiyle kullanmaya ilişkin daha fazla örnek için bkz. [http://redis.js.org/](http://redis.js.org/).

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


## <a name="next-steps"></a>Sonraki adımlar
* Önbelleğinizin sistem durumunu [izleyebilmeniz](cache-how-to-monitor.md) için [önbellek tanılamayı etkinleştirin](cache-how-to-monitor.md#enable-cache-diagnostics).
* Resmi [Redis belgeleri](http://redis.io/documentation)ni okuyun.




<!--HONumber=Jan17_HO2-->


