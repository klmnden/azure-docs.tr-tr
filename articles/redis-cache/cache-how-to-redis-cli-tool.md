---
title: Redis CLI ile Azure Redis önbelleği kullanma | Microsoft Docs
description: Azure Redis önbelleği redis CLI kullanmayı öğrenin.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: wesmc
ms.openlocfilehash: facc3e2079c8f5e19266f5379762d71754a6ed84
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182594"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Azure Redis önbelleği Redis komut satırı aracını kullanma

*redis-cli.exe* bir istemci olarak Redis önbelleği ile etkileşim için bir popüler komut satırı aracıdır. Bu araç, Azure Redis önbelleği ile kullanmak için de kullanılabilir.

Aracı'nı yükleyerek Windows platformları için kullanılabilir [Windows için komut satırı araçları Redis](https://github.com/MSOpenTech/redis/releases/). 

Komut satırı aracı başka bir platformda çalıştırmak istiyorsanız, Redis Önbelleği'nden indirin [ http://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Önbellek erişim bilgileri toplayın

Üç yöntemi kullanarak önbelleğe erişmek için gerekli bilgiler toplayabilirsiniz:

1. Azure CLI kullanarak [az redis listesi anahtarları](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell kullanarak [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. Azure portalını kullanarak.

Bu bölümde, Azure portalından anahtarları alır.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Redis-cli.exe için erişimi etkinleştirme

Azure Redis önbelleği ile yalnızca SSL bağlantı noktası (6380) varsayılan olarak etkindir. `redis-cli.exe` Komut satırı aracı SSL'yi desteklemez. Kullanmak için iki yapılandırma seçeneğiniz vardır:

1. [SSL olmayan bağlantı noktası (6379) etkinleştir](cache-configure.md#access-ports) - **bu yapılandırma önerilmez** çünkü bu yapılandırmada, erişim anahtarları TCP üzerinden düz metin olarak gönderilir. Bu değişiklik, önbelleğiniz erişimi tehlikeye atabilir. Yalnızca bir test önbellek erişirken bu yapılandırmayı göz önünde tutabileceğiniz burada yalnızca bir senaryodur.

2. İndirme ve yükleme [stunnel](https://www.stunnel.org/downloads.html).

    Çalıştırma **stunnel GUI başlangıç** sunucu başlatmak için.

    Stunnel sunucunun görev simgesini sağ tıklatın ve **günlük penceresini göster**.

    Stunnel günlük Pencere menüsünde tıklatın **yapılandırma** > **yapılandırmasını Düzenle** geçerli yapılandırma dosyasını açın.

    Eklemek için şu girdiyi *redis cli.exe* altında **hizmet tanımları** bölümü. Gerçek önbellek adınızı yerine INSERT `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Yapılandırma dosyasını kaydedip kapatın. 
  
    Stunnel günlük Pencere menüsünde tıklatın **yapılandırma** > **yeniden yapılandırma**.


## <a name="connect-using-the-redis-command-line-tool"></a>Redis komut satırı aracını kullanarak bağlanın.

Stunnel kullanırken çalıştırmak *redis cli.exe*ve yalnızca geçirin, *bağlantı noktası*, ve *erişim tuşu* (birincil veya ikincil) önbelleğe bağlanma.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![redis CLI ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Bir test önbelleği ile kullanıyorsanız, **güvensiz** SSL olmayan bağlantı noktası çalıştırmak, `redis-cli.exe` ve geçirmek, *ana bilgisayar adı*, *bağlantı noktası*, ve *erişim tuşu*(birincil veya ikincil) test önbelleğe bağlanma.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![redis CLI ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [Redis konsol](cache-configure.md#redis-console) sorunu komutları.

