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
ms.date: 08/24/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 408026a8e75272cea92ad62e3a75aabaadf98351
ms.openlocfilehash: 00c4d2e31391297955ecba891e919bda65ddaf8e


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

## <a name="enable-the-non-ssl-endpoint"></a>SSL olmayan uç noktayı etkinleştirme
Bazı Redis istemcileri SSL’yi desteklemez ve varsayılan olarak [SSL olmayan bağlantı noktası yeni Azure Redis Önbelleği örnekleri için devre dışıdır](cache-configure.md#access-ports). Bu yazma sırasında, [Jedis](https://github.com/xetorthio/jedis) istemcisi SSL’yi desteklemez. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Önbelleğe bir şey ekleme ve bunu alma
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a>Sonraki adımlar
* Önbelleğinizin sistem durumunu [izleyebilmeniz](https://msdn.microsoft.com/library/azure/dn763945.aspx) için [önbellek tanılamayı etkinleştirin](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics).
* Resmi [Redis belgeleri](http://redis.io/documentation)ni okuyun.




<!--HONumber=Nov16_HO5-->


