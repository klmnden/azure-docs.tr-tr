---
title: "Java ile Azure Redis Önbelleği kullanma | Microsoft Belgeleri"
description: "Java kullanarak Azure Redis Önbelleği kullanmaya başlama"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 5369dcd6ad1ec93c63eb442db9fc5ffdcca37375
ms.openlocfilehash: b95f37db90b105962c01545e25c8e14c53257ebc


---
# <a name="how-to-use-azure-redis-cache-with-java"></a>Java ile Azure Redis Önbelleği kullanma
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Önbelleği, Microsoft tarafından yönetilen ayrılmış bir Redis önbelleğine erişmenizi sağlar. Önbelleğinize Microsoft Azure’daki her uygulamadan erişilebilir.

Bu konu Java kullanarak Azure Redis Önbelleği kullanmayı gösterir.

## <a name="prerequisites"></a>Ön koşullar
[Jedis](https://github.com/xetorthio/jedis) - Redis için Java istemcisi

Bu öğreticide Jedis kullanılmıştır, ancak [http://redis.io/clients](http://redis.io/clients) adresinde listelenmiş herhangi bir Java istemcisini kullanabilirsiniz.

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis önbelleği oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ana bilgisayar adını ve erişim anahtarlarını alma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>SSL kullanarak güvenli bir şekilde önbelleğe bağlanma
[jedis](https://github.com/xetorthio/jedis)’in en son derlemeleri, Azure Redis Önbelleği’ne SSL kullanarak bağlanma konusunda destek sağlar. Aşağıdaki örnekte, 6380 SSL bitiş noktasını kullanarak Azure Redis Önbelleği’ne nasıl bağlanılacağı gösterilmektedir. Önceki [Ana bilgisayar adını ve erişim anahtarlarını alma](#retrieve-the-host-name-and-access-keys) bölümünde açıklanan şekilde `<name>` öğesini önbelleğinizin adı ile, `<key>` öğesini ise birincil veya ikincil anahtarınızla değiştirin.

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> SSL olmayan bağlantı noktası, yeni Azure Redis Önbelleği örnekleri için devre dışıdır. SSL desteği olmayan farklı bir istemci kullanıyorsanız bkz. [SSL olmayan bağlantı noktasını etkinleştirme](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Önbelleğe bir şey ekleme ve bunu alma
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Sonraki adımlar
* Önbelleğinizin sistem durumunu [izleyebilmeniz](https://msdn.microsoft.com/library/azure/dn763945.aspx) için [önbellek tanılamayı etkinleştirin](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics).
* Resmi [Redis belgeleri](http://redis.io/documentation)ni okuyun.



<!--HONumber=Feb17_HO2-->


