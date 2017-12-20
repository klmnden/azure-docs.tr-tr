---
title: Node.js ile Azure Redis Cache kullanma | Microsoft v
description: "Node.js ve node_redis kullanarak Azure Redis Cache kullanmaya başlama"
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
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: f2c448af24e180db58f3ef3d39e90036dda3f7eb
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Node.js ile Azure Redis Cache kullanma
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache, Microsoft tarafından yönetilen güvenli, ayrılmış bir Redis önbelleğine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu konu Node.js kullanarak Azure Redis Cache kullanmayı gösterir. 

## <a name="prerequisites"></a>Ön koşullar
[node_redis](https://github.com/mranney/node_redis) yükleyin:

    npm install redis

Bu öğreticide, [node_redis](https://github.com/mranney/node_redis) kullanılmaktadır. Diğer Node.js istemcilerini kullanmaya ilişkin örnekler için [Node.js Redis istemcileri](http://redis.io/clients#nodejs) listesindeki Node.js istemcilerinin kendi belgelerine bakın.

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis Cache oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ana bilgisayar adını ve erişim anahtarlarını alma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>SSL kullanarak güvenli bir şekilde önbelleğe bağlanma
[node_redis](https://github.com/mranney/node_redis)’in en son derlemeleri, Azure Redis Cache’e SSL kullanarak bağlanma konusunda destek sağlar. Aşağıdaki örnekte, 6380 SSL bitiş noktasını kullanarak Azure Redis Cache’e nasıl bağlanılacağı gösterilmektedir. Önceki [Ana bilgisayar adını ve erişim anahtarlarını alma](#retrieve-the-host-name-and-access-keys) bölümünde açıklanan şekilde `<name>` öğesini önbelleğinizin adı ile, `<key>` öğesini ise birincil veya ikincil anahtarınızla değiştirin.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> SSL olmayan bağlantı noktası, yeni Azure Redis Cache örnekleri için devre dışıdır. SSL desteği olmayan farklı bir istemci kullanıyorsanız bkz. [SSL olmayan bağlantı noktasını etkinleştirme](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Önbelleğe bir şey ekleme ve bunu alma
Aşağıdaki örnekte, size bir Azure Redis Cache örneğine bağlanma, önbellekte öğe depolama ve önbellekten öğe alma işlemlerinin nasıl yapılacağı gösterilmektedir. Redis’i [node_redis](https://github.com/mranney/node_redis) istemcisiyle kullanmaya ilişkin daha fazla örnek için bkz. [http://redis.js.org/](http://redis.js.org/).

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

