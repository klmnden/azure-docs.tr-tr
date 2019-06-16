---
title: Redis CLI ile Azure önbelleği için Redis kullanma | Microsoft Docs
description: Redis CLI ile Azure önbelleği için Redis kullanmayı öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: yegu
ms.openlocfilehash: 318d02f5da816ae8fe2fe199b9c87b3748d5d1fc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66133041"
---
# <a name="how-to-use-the-redis-command-line-tool-with-azure-cache-for-redis"></a>Redis komut satırı aracı Azure önbelleği için Redis kullanma

*redis-cli.exe* ile bir Azure önbelleği için Redis bir istemci olarak etkileşim kurmak için kullanabileceğiniz bir popüler komut satırı aracıdır. Bu araç ayrıca Azure önbelleği için Redis ile kullanılabilir.

Aracı yükleyerek Windows platformlarında kullanılabilir [için Windows komut satırı araçları Redis](https://github.com/MSOpenTech/redis/releases/). 

Komut satırı aracı başka bir platform üzerinde çalıştırmasını istiyorsanız, Azure önbelleği Redis'i indirin [ https://redis.io/download ](https://redis.io/download).

## <a name="gather-cache-access-information"></a>Önbellek erişim bilgileri toplama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Üç yöntemi kullanarak önbelleğe erişmek için gereken bilgiler toplayabilir:

1. Azure CLI kullanarak [az redis anahtarlarını Listele](https://docs.microsoft.com/cli/azure/redis?view=azure-cli-latest#az-redis-list-keys)
2. Azure PowerShell kullanarak [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-AzRedisCacheKey)
3. Azure portalını kullanarak.

Bu bölümde, Azure portalından anahtarları alır.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## <a name="enable-access-for-redis-cliexe"></a>Redis-cli.exe erişimini etkinleştirin

Azure Cache Redis ile yalnızca SSL bağlantı noktası (6380) varsayılan olarak etkindir. `redis-cli.exe` Komut satırı aracı, SSL'yi desteklemez. Bunu kullanmak için iki yapılandırma seçeneğiniz vardır:

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

