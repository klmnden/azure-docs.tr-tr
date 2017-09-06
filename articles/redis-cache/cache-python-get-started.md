---
title: Python ile Azure Redis Cache kullanma | Microsoft Belgeleri
description: "Python kullanarak Azure Redis Cache kullanmaya başlama"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.translationtype: Human Translation
ms.sourcegitcommit: 5369dcd6ad1ec93c63eb442db9fc5ffdcca37375
ms.openlocfilehash: cdbee52574d0ffbe82ef3dc98f2848f4d00ba2ff
ms.contentlocale: tr-tr
ms.lasthandoff: 02/11/2017


---
# <a name="how-to-use-azure-redis-cache-with-python"></a>Python ile Azure Redis Cache kullanma
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Bu konu Python kullanarak Azure Redis Cache kullanmayı gösterir.

## <a name="prerequisites"></a>Ön koşullar
[redis-py](https://github.com/andymccurdy/redis-py) yükleyin.

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis Cache oluşturma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Ana bilgisayar adını ve erişim anahtarlarını alma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-the-non-ssl-endpoint"></a>SSL olmayan uç noktayı etkinleştirme
Bazı Redis istemcileri SSL’yi desteklemez ve varsayılan olarak [SSL olmayan bağlantı noktası yeni Azure Redis Cache örnekleri için devre dışıdır](cache-configure.md#access-ports). Bu yazma sırasında, [redis-py](https://github.com/andymccurdy/redis-py) istemcisi SSL’yi desteklemez. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Önbelleğe bir şey ekleme ve bunu alma
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

