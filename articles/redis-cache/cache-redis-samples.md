---
title: Azure Redis önbelleği örnekleri | Microsoft Docs
description: Azure Redis önbelleği kullanmayı öğrenin
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: 0b6f89807d3252b750bd5208a7f758a06c9903d6
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
ms.locfileid: "27909705"
---
# <a name="azure-redis-cache-samples"></a>Azure Redis önbelleği örnekleri
Bu konuda, Azure Redis önbelleği örnekleri, önbellek, okuma ve yazma verileri için ve önbellekten bağlanma ve ASP.NET Redis önbelleği sağlayıcılarını kullanma gibi senaryolar kapsayan bir listesini sağlar. Bazı örnekler indirilebilir proje yoksa ve bazı adım adım yönergeler sağlar ve kod parçaları içeren ancak indirilebilir bir projeye bağlamayın.

## <a name="hello-world-samples"></a>Hello world örnekleri
Bu bölümdeki örnekler ve istemcilerin Redis Azure Redis önbelleği örneğine bağlanmayı ve veri okuma ve çeşitli dillerde kullanarak önbelleğe yazma temelleri gösterilmektedir.

[Merhaba Dünya](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek göstermektedir kullanarak çeşitli önbellek işlemleri gerçekleştirmek nasıl [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET istemci.

Bu örnek göstermektedir nasıl yapılır:

* Çeşitli bağlantı seçenekleri kullanın
* Okuma ve yazma için ve zaman uyumlu ve zaman uyumsuz işlemler kullanarak önbelleğinden nesneleri
* Dönüş değerleri belirtilen anahtar için Redis MGET/MSET komutları kullanın
* Redis işlem işlemleri
* Redis listeleri ve sıralanmış kümeleri ile çalışma
* Mağaza .NET nesneleri JsonConvert serileştiricileri kullanma
* Etiketleme uygulamak için Redis kümelerini kullanın
* Redis kümesi ile çalışma

Daha fazla bilgi için bkz: [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) github'da ve daha fazla kullanım senaryoları için belgelere bakın [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) birim testleri.

[Python ile Azure Redis önbelleği kullanma](cache-python-get-started.md) Azure Python kullanarak Redis önbelleği kullanmaya başlamak nasıl gösterir ve [redis-py](https://github.com/andymccurdy/redis-py) istemci.

[Önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) yazmanıza izin ve bir Azure Redis önbelleği örnekten okumak için .NET nesnelerini seri hale getirmek için bir yolunu gösterir. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>ASP.NET SignalR için bir genişletme devre kartı olarak Redis önbelleği kullanma
[Genişletme ASP.NET SignalR için devre kartı olarak Redis önbelleği kullanma](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) örneği, Azure Redis önbelleği SignalR devre kartı olarak nasıl kullanabileceğinizi gösterir. Devre kartına hakkında daha fazla bilgi için bkz: [SignalR genişletme Redis ile](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis önbelleği müşteri sorgu örnek
Bu örnek bir önbellekten veri erişimi ve kalıcı depolama biriminden verilere erişme arasında karşılaştırır performans gösterir. Bu örnek iki proje yok.

* [Verileri önbelleğe alarak Redis önbelleği performansı nasıl geliştirebilir gösteri](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Gösteri için çekirdek önbellek ve veritabanı](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET oturum durumu ve çıktı önbelleği
[Kullanmak Azure Redis ASP.NET SessionState ve OutputCache depolamak için önbelleği](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) örnek gösterilmektedir nasıl, Azure Redis önbelleği ASP.NET oturumunu ve çıkış SessionState ve OutputCache sağlayıcıları için Redis kullanarak önbelleği depolamak için kullanılacak.

## <a name="manage-azure-redis-cache-with-maml"></a>Azure Redis önbelleği ile aynı anda MAML Yönet
[Yönetmek Azure Azure yönetim kitaplıklarını kullanarak Redis önbelleği](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) örnek gösterilmektedir nasıl, Azure yönetim kitaplıklarını yönetmek için kullanabileceğiniz - (oluştur / güncelleştir / Sil) önbelleğiniz. 

## <a name="custom-monitoring-sample"></a>Özel örnek izleme
[Erişim Redis önbelleği izleme verileri](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) örnek gösteren Azure Portal dışında Azure Redis önbelleği için izleme verilerini nasıl erişebilirsiniz.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>PHP ve Redis kullanılarak yazılmış bir Twitter tarzı kopyalama
[Retwis](https://github.com/SyntaxC4-MSFT/retwis) Redis Hello World örnek verilmiştir. Redis ve PHP kullanılarak yazılmış en az bir Twitter tarzı sosyal ağ kopya olduğu kullanarak [Predis](https://github.com/nrk/predis) istemci. Kaynak kodu çok basit ve veri yapıları farklı göstermek için aynı anda Redis için tasarlanmıştır.

## <a name="bandwidth-monitor"></a>Bant genişliği İzleyicisi
[Bant genişliği İzleyici](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) örnek istemcide kullanılan bant genişliği izlemenize olanak sağlar. Bant genişliği ölçmek için önbellek istemci makinede örneği çalıştırmak, önbellek çağrı yapmak ve bant genişliği İzleyici örnek tarafından bildirilen bant genişliği inceleyin.

