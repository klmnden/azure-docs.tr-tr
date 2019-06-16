---
title: Azure önbelleği için Redis örnekleri | Microsoft Docs
description: Redis için Azure Cache’i kullanmayı öğrenin
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: 73c771ab18d1cc2944298818c1cab90eb2f277ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829812"
---
# <a name="azure-cache-for-redis-samples"></a>Redis için Azure Önbelleği örnekleri
Bu konu, önbellek, okuma ve yazma verileri azure'a veya önbellekten bağlanma ve ASP.NET Azure önbelleği için Redis sağlayıcıları kullanan gibi senaryoları kapsayan, Redis örnekleri için Azure önbellek listesini sağlar. Örneklerden bazıları indirilebilir projelerdir ve bazı adım adım rehberlik ve kod parçacıkları içerir ancak indirilebilir bir projeye bağlamayın.

## <a name="hello-world-samples"></a>Hello world örnekleri
Bu bölümdeki örnekler, bir Azure önbelleği için Redis örneği bağlama ve okuma ve çeşitli dillerde kullanarak önbelleğe veri yazma temel bilgileri gösterir ve Redis istemcileri.

[Merhaba Dünya](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek gösterir kullanarak çeşitli önbellek işlemleri gerçekleştirme [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET istemcisi.

Bu örnek, gösterir nasıl yapılır:

* Çeşitli bağlantı seçenekleri kullanın
* Okuma ve yazma için ve zaman uyumlu ve zaman uyumsuz işlemler kullanarak önbellekten nesneleri
* Belirtilen anahtar değer döndürmek için Redis MGET/MSET komutları kullanın.
* Redis işlem işlemleri
* Redis listeler ve sıralanmış kümeleri ile çalışma
* Store .NET nesneleri kullanarak JsonConvert seri hale getiricileri genişletme
* Redis kümesi etiketleme uygulamak için kullanma
* Redis küme ile çalışmanıza

Daha fazla bilgi için [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) GitHub ve daha fazla kullanım senaryoları için belgelere bakın [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/tests) birim testleri.

[Azure Cache, Redis Python ile kullanılmak üzere nasıl](cache-python-get-started.md) Python kullanarak Redis için Azure Cache kullanmaya başlama işlemini gösterir ve [redis-py](https://github.com/andymccurdy/redis-py) istemci.

[Önbellekte .NET nesneleriyle çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) yazmanıza izin ve bir Azure önbelleği için Redis örneğinden okuyabilir .NET nesneleri serileştirmek için bir yol gösterir. 

## <a name="use-azure-cache-for-redis-as-a-scale-out-backplane-for-aspnet-signalr"></a>Azure Cache için Redis genişletme devre kartına ASP.NET SignalR için kullanın
[Kullanan Azure Cache için ASP.NET SignalR genişletme devre kartı olarak Redis için](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) örneği, nasıl Azure önbelleği için Redis bir SignalR devre kartı olarak kullanabileceğinizi gösterir. Devre kartına hakkında daha fazla bilgi için bkz: [Redis ile SignalR ölçeğini genişletme](https://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="azure-cache-for-redis-customer-query-sample"></a>Azure önbelleği için Redis müşteri sorgu örneği
Bu örnek, bir önbelleğe alınan verilere ve verilere erişim kalıcı depolama alanından arasında karşılaştırır performans gösterir. Bu örnek, iki proje yok.

* [Azure önbelleği için Redis performans verileri önbelleğe alarak nasıl geliştireceğiniz gösteri](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Tanıtım için çekirdek önbellek ve veritabanı](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>ASP.NET oturum durumu ve çıktı önbelleği
[Kullanan Azure Cache için ASP.NET SessionState ve OutputCache depolamak Redis](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) örnek gösterir nasıl ASP.NET oturumunu ve çıkış SessionState ve OutputCache sağlayıcıları kullanarak Redis önbelleği depolamak için Azure önbelleği için Redis kullanmak için .

## <a name="manage-azure-cache-for-redis-with-maml"></a>Azure önbelleği için Redis MAML ile yönetme
[Yönetilen Azure önbelleği için Azure yönetim kitaplıkları'nı kullanarak Redis](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) örneği, nasıl, Azure yönetim kitaplıkları yönetmek için kullanabilirsiniz - gösterir (oluşturma / güncelleştirme / silme) önbelleğinizi. 

## <a name="custom-monitoring-sample"></a>Özel İzleme örneği
[Erişim Azure önbelleği için Redis izleme verilerini](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) örnek, izleme verilerini, Azure önbelleği için Redis dışında Azure portalı için nasıl erişebileceğinizi gösterir.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>PHP Redis ile yazılan bir Twitter tarzı kopyalama
[Retwis](https://github.com/SyntaxC4-MSFT/retwis) Redis Hello World örnek verilmiştir. Redis ve PHP kullanılarak yazılmış en az bir Twitter tarzı sosyal ağ kopya olduğunu kullanarak [Predis](https://github.com/nrk/predis) istemci. Kaynak kodu, çok basit ve farklı göstermek için aynı anda Redis veri yapıları için tasarlanmıştır.

## <a name="bandwidth-monitor"></a>Bant genişliği İzleyicisi
[Bant genişliği İzleyici](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) örnek, istemcide kullanılan bant genişliğini izlemenize olanak sağlar. Bant genişliği ölçmek için örneği önbellek istemci makinede çalıştırmak, önbellek çağrı yapmak ve bant genişliği İzleyici örnek tarafından bildirilen bant genişliği gözlemleyin.

