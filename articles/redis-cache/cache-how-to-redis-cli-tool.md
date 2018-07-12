---
title: Redis-cli, Azure Redis Cache ile kullanma | Microsoft Docs
description: Redis CLI ile Azure Redis Cache kullanmayı öğrenin.
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38299100"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-redis-cache"></a>Redis komut satırı aracı ile Azure Redis Cache kullanma

*redis-cli.exe* istemci olarak bir Redis Cache ile etkileşim kurmak için kullanabileceğiniz bir popüler komut satırı aracıdır. Bu araç ayrıca Azure Redis Cache ile kullanılabilir.

Aracı yükleyerek Windows platformlarında kullanılabilir [için Windows komut satırı araçları Redis](https://github.com/MSOpenTech/redis/releases/). 

Komut satırı aracı başka bir platform üzerinde çalıştırmasını istiyorsanız, Redis Önbelleği'ndeki indirme [ http://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Önbellek erişim bilgileri toplama

Üç yöntemi kullanarak önbelleğe erişmek için gereken bilgiler toplayabilir:

1. Azure CLI kullanarak [az redis anahtarlarını Listele](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell kullanarak [Get-AzureRmRedisCacheKey](https://docs.microsoft.com/powershell/module/azurerm.rediscache/Get-AzureRmRedisCacheKey?view=azurermps-4.4.1)
3. Azure portalını kullanarak.

Bu bölümde, Azure portalından anahtarları alır.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Redis-cli.exe erişimini etkinleştirin

Azure Redis Cache ile yalnızca SSL bağlantı noktası (6380) varsayılan olarak etkindir. `redis-cli.exe` Komut satırı aracı, SSL'yi desteklemez. Bunu kullanmak için iki yapılandırma seçeneğiniz vardır:

1. [SSL olmayan bağlantı noktası (6379) etkin](cache-configure.md#access-ports) - **bu yapılandırma önerilmez** çünkü bu yapılandırmada, erişim anahtarlarını TCP üzerinden açık metin olarak gönderilir. Bu değişiklik, bir önbelleğe erişim tehlikeye atabilir. Bu yapılandırmayı göz önünde tutabileceğiniz olduğu tek senaryo, yalnızca bir test önbellek eriştiğiniz andır.

2. İndirme ve yükleme [stunnel](https://www.stunnel.org/downloads.html).

    Çalıştırma **stunnel GUI başlangıç** sunucu başlatılamadı.

    Stunnel sunucusu görev simgesini sağ tıklatıp **Günlüğü penceresini göster**.

    Stunnel Günlüğü penceresini menüsünde **yapılandırma** > **yapılandırmasını Düzenle** geçerli yapılandırma dosyasını açın.

    Eklemek için şu girdiyi *redis-cli.exe* altında **hizmet tanımları** bölümü. Gerçek önbellek adınızı yerine Ekle `yourcachename`. 

    ```
    [redis-cli]
    client = yes
    accept = 127.0.0.1:6380
    connect = yourcachename.redis.cache.windows.net:6380
    ```

    Yapılandırma dosyasını kaydedip kapatın. 
  
    Stunnel Günlüğü penceresini menüsünde **yapılandırma** > **yeniden yapılandırma**.


## <a name="connect-using-the-redis-command-line-tool"></a>Redis komut satırı aracını kullanarak bağlanın.

Stunnel kullanırken çalıştırmak *redis-cli.exe*ve yalnızca başarılı, *bağlantı noktası*, ve *erişim anahtarı* (birincil veya ikincil) önbelleğe bağlanma.

```
redis-cli.exe -p 6380 -a YourAccessKey
```

![redis CLI ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-stunnel.png)

Bir test önbellekle kullanıyorsanız, **güvensiz** SSL olmayan bağlantı noktası çalıştırın, `redis-cli.exe` geçirin, *ana bilgisayar adı*, *bağlantı noktası*, ve *erişim anahtarı*(birincil veya ikincil) test önbelleğe bağlanma.

```
redis-cli.exe -h yourcachename.redis.cache.windows.net -p 6379 -a YourAccessKey
```

![redis CLI ile stunnel](media/cache-how-to-redis-cli-tool/cache-redis-cli-non-ssl.png)




## <a name="next-steps"></a>Sonraki adımlar

Kullanma hakkında daha fazla bilgi edinin [Redis Konsolu](cache-configure.md#redis-console) sorunu komutları.

