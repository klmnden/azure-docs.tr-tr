---
title: "Azure Redis önbelleği ölçeklendirme | Microsoft Docs"
description: "Azure Redis önbelleği örneklerinizi ölçeklendirin öğrenin"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-azure-redis-cache"></a>Azure Redis önbelleği ölçeklendirme
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini seçimi esneklik sağlayan farklı önbellek teklifleri vardır. Bir önbellek oluşturulduktan sonra uygulamanızın gereksinimlerine değiştirirseniz boyutu ve önbellek fiyatlandırma katmanı ölçeklendirebilirsiniz. Bu makalede, Azure portalı ve Azure PowerShell ve Azure CLI gibi araçları kullanarak önbelleğinde ölçeklendirme gösterilmektedir.

## <a name="when-to-scale"></a>Ne zaman ölçeklendirme yapılmalıdır?
Kullanabileceğiniz [izleme](cache-how-to-monitor.md) sistem durumunu ve önbelleğiniz performansını izlemek ve önbellek ölçeklendirmek ne zaman belirlemenize yardımcı olması için Azure Redis önbelleği özellikleri. 

Ölçeklendirme gerekiyorsa belirlemenize yardımcı olması için aşağıdaki ölçümleri izleyebilirsiniz.

* Redis sunucu iş yükü
* Bellek kullanımı
* Ağ bant genişliği
* CPU kullanımı

Önbelleğinizi artık uygulamanızın gereksinimlerini karşılıyor mu karar verirseniz, fiyatlandırma katmanı, uygulamanız için uygun bir büyük veya küçük önbelleğine ölçeklendirebilirsiniz. Fiyatlandırma katmanı kullanmak için hangi önbellek belirleme hakkında daha fazla bilgi için bkz: [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Ölçek bir önbellek
Önbelleğinizi, ölçeklendirmek için [gözatmak için önbellek](cache-configure.md#configure-redis-cache-settings) içinde [Azure portal](https://portal.azure.com) tıklatıp **ölçek** gelen **kaynak menü**.

![Ölçek](./media/cache-how-to-scale/redis-cache-scale-menu.png)

İstenen fiyatlandırma Katmanı'ndan seçin **fiyatlandırma katmanı seçme** tıklayın ve dikey **seçin**.

![Fiyatlandırma katmanı][redis-cache-pricing-tier-blade]


Aşağıdaki kısıtlamalarla farklı bir fiyatlandırma katmanı için ölçekleme yapabilirsiniz:

* Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanı ölçek olamaz.
  * Ölçeklendirme olamaz bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbelleği.
  * Ölçeklendirme olamaz bir **standart** aşağı önbelleğe bir **temel** önbelleği.
* Ölçeklendirme yapılabilir bir **temel** için önbelleğe bir **standart** önbellek ancak değiştiremiyor boyutu aynı anda. Farklı bir boyut gerekiyorsa, istenen boyuta sonraki bir ölçeklendirme işlemi yapabilirsiniz.
* Ölçeklendirme olamaz bir **temel** doğrudan önbelleğe bir **Premium** önbelleği. Ölçeklendirme gerekir **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** bir sonraki ölçeklendirme işleminde.
* Büyük bir değerden aşağı ölçeklendirme olamaz **C0 (250 MB)** boyutu.
 
Önbellek yeni fiyatlandırma katmanı, ölçekleme sırasında bir **ölçeklendirme** durum görüntülenir **Redis önbelleği** dikey.

![Ölçeklendirme][redis-cache-scaling]

Ölçeklendirme tamamlandığında, durum değişiklikleri **ölçeklendirme** için **çalıştıran**.

## <a name="how-to-automate-a-scaling-operation"></a>Bir ölçeklendirme işlemi otomatikleştirme
Azure portalında önbelleği örneklerinizi ölçeklendirme yanı sıra, PowerShell cmdlet'leri, Azure CLI kullanarak ölçeklendirebilirsiniz ve Microsoft Azure yönetim kitaplıkları (MAML) kullanarak. 

* [PowerShell kullanarak ölçek](#scale-using-powershell)
* [Azure CLI kullanarak ölçek](#scale-using-azure-cli)
* [Ölçek MAML kullanma](#scale-using-maml)

### <a name="scale-using-powershell"></a>PowerShell kullanarak ölçek
Kullanarak PowerShell ile Azure Redis önbelleği örneklerinizi ölçeklendirebilirsiniz [kümesi AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet zaman `Size`, `Sku`, veya `ShardCount` özellikleri değiştirilemez. Aşağıdaki örnek adlı bir önbellek ölçeklendirmek nasıl gösterir `myCache` 2,5 GB önbelleğe. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

PowerShell ile ölçeklendirme ile ilgili daha fazla bilgi için bkz: [Powershell kullanarak Redis önbelleği ölçeklendirmek için](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Azure CLI kullanarak ölçek
Azure CLI kullanarak Azure Redis önbelleği örneklerinizi ölçeklendirin çağrısı `azure rediscache set` komut ve yeni boyutu, sku veya küme boyutu, istenen ölçekleme işlemi bağlı olarak eklemek istediğiniz yapılandırma değişikliklerini geçirin.

Azure CLI ile ölçeklendirme ile ilgili daha fazla bilgi için bkz: [var olan bir Redis Önbelleği Ayarları Değiştir](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Ölçek MAML kullanma
Azure Redis önbelleği ölçeklendirmek için örnekler kullanarak [Microsoft Azure yönetim kitaplıkları (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), çağrı `IRedisOperations.CreateOrUpdate` yöntemi ve yeni boyutu geçişinde `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Daha fazla bilgi için bkz: [yönetmek MAML kullanarak Redis önbelleği](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) örnek.

## <a name="scaling-faq"></a>Ölçeklendirme ile ilgili SSS
Aşağıdaki listede, Azure Redis önbelleği ölçeklendirme hakkında sık sorulan soruların yanıtlarını içerir.

* [İçin ya da Premium önbelleği içinde ölçekleme?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Ölçeklendirme sonra ı my önbellek adını veya erişim anahtarlarını değiştirme gerekiyor mu?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Ölçeklendirme nasıl çalışır?](#how-does-scaling-work)
* [Ölçeklendirme sırasında ı my önbellekten veri kaybedersiniz?](#will-i-lose-data-from-my-cache-during-scaling)
* [My özel veritabanlarını ölçeklendirme sırasında etkilenen ayarlıyor?](#is-my-custom-databases-setting-affected-during-scaling)
* [My önbellek ölçeklendirme sırasında kullanılabilir olacak?](#will-my-cache-be-available-during-scaling)
* [Desteklenmeyen işlemleri](#operations-that-are-not-supported)
* [Ne kadar ölçeklendirme sürer?](#how-long-does-scaling-take)
* [Ölçeklendirme tamamlandığında nasıl anlayabilirim?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>İçin ya da Premium önbelleği içinde ölçekleme?
* Ölçeklendirme olamaz bir **Premium** aşağı önbelleğe bir **temel** veya **standart** fiyatlandırma katmanı.
* Birinden ölçeklendirebilirsiniz **Premium** fiyatlandırma katmanı başka bir önbellek.
* Ölçeklendirme olamaz bir **temel** doğrudan önbelleğe bir **Premium** önbelleği. Öğesinden önce Ölçeklendirmesi gerekir **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** bir sonraki ölçeklendirme işleminde.
* Kümeleme etkinleştirilirse oluşturduğunuzda, **Premium** önbellek, şunları yapabilirsiniz [küme boyutunu değiştirme](cache-how-to-premium-clustering.md#cluster-size). Önbelleğinizi etkin Kümeleme olmadan oluşturulduysa, daha sonraki bir zamanda kümeleme yapılandıramazsınız.
  
  Daha fazla bilgi için bkz. [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Ölçeklendirme sonra ı my önbellek adını veya erişim anahtarlarını değiştirme gerekiyor mu?
Hayır, önbellek adı ve anahtarları bir ölçeklendirme işlemi sırasında değiştirilmez.

### <a name="how-does-scaling-work"></a>Ölçeklendirme nasıl çalışır?
* Zaman bir **temel** önbellek için farklı bir boyutu ölçeği, onu kapatılır ve yeni bir önbellek yeni boyut kullanılarak sağlanır. Bu süre boyunca önbelleğe kullanılamıyor ve önbellekteki tüm verileri kaybolur.
* Zaman bir **temel** için önbellek ölçeklendirilmiş bir **standart** önbellek, çoğaltma önbelleği sağlandığında ve veri çoğaltma önbelleğine birincil önbellekten kopyalanır. Önbellek ölçeklendirme işlemi sırasında kullanılabilir olarak kalır.
* Zaman bir **standart** önbellek boyutu farklı ya da çok ölçeklendirilmiş bir **Premium** önbellek, çoğaltmaları biri kapatmak ve yeni boyuta yeniden sağlanan üzerinden aktarılan veri ve onu, ön bellek düğümleri biri başarısız sırasında gerçekleşen işlem benzer yeniden sağlanmadan önce bir yük devretme diğer çoğaltma gerçekleştirir.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Ölçeklendirme sırasında ı my önbellekten veri kaybedersiniz?
* Zaman bir **temel** önbellek için yeni bir boyutu ölçeği, verilerin tümü kaybolur ve önbellek ölçeklendirme işlemi sırasında kullanılamaz.
* Zaman bir **temel** için önbellek ölçeklendirilmiş bir **standart** önbellek, önbellekteki verileri genellikle korunur.
* Zaman bir **standart** daha büyük bir boyut veya katmanı, önbellek ölçeklendirilmiş veya **Premium** önbelleği için daha büyük bir boyutu ölçeği, tüm verileri genellikle korunur. Ölçekleme sırasında bir **standart** veya **Premium** önbellek veri daha küçük bir ölçüye ne kadar veri yeni boyutu, ölçeği olduğunda ilgili önbellekteyse bağlı olarak kayıp. Veri ölçeklendirdiğinizde kaybolursa, anahtarları kullanarak çıkarılacak [allkeys lru](http://redis.io/topics/lru-cache) çıkarma ilkesi. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>My özel veritabanlarını ölçeklendirme sırasında etkilenen ayarlıyor?
Bazı fiyatlandırma katmanları farklı sahip [veritabanları sınırları](cache-configure.md#databases), böylece bazı noktalar IF ölçekleme için özel bir değer yapılandırıldığında `databases` önbellek oluşturma sırasında ayarlama.

* Bir fiyatlandırma katmanı ile bir alt ölçeklendirme `databases` geçerli katmanı sınırından:
  * Varsayılan sayısını kullanıyorsanız `databases` tüm fiyatlandırma katmanlarına 16 olduğu, veri kaybı olmamasına.
  * Özel bir dizi kullanıyorsanız `databases` için ölçekleme, bu katman için sınırlar içinde kalan `databases` ayarı tutulur ve veri kaybı olmamasına.
  * Özel bir dizi kullanıyorsanız `databases` yeni katmanı sınırlarını aşıyor `databases` ayarı yeni katmanı sınırlarına kadar yerleştirilen ve kaldırılan veritabanlarındaki tüm veriler kaybolur.
* Aynı veya daha yüksek bir fiyatlandırma katmanı için ölçeklendirme `databases` geçerli katmanı sınırından, `databases` ayarı tutulur ve veri kaybı olmamasına.

% 99,9 SLA kullanılabilirlik için standart ve Premium önbellekleri sahip olsa da, veri kaybı için hiçbir SLA yoktur.

### <a name="will-my-cache-be-available-during-scaling"></a>My önbellek ölçeklendirme sırasında kullanılabilir olacak?
* **Standart** ve **Premium** önbellekleri kullanılabilir olmaya devam eder ölçekleme işlemi sırasında.
* **Temel** önbellekleri işlemleri farklı bir boyut ölçeklendirme sırasında çevrimdışı, ancak gelen ölçekleme sırasında kullanılabilir olmaya devam etmesi **temel** için **standart**.

### <a name="operations-that-are-not-supported"></a>Desteklenmeyen işlemleri
* Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanı ölçek olamaz.
  * Ölçeklendirme olamaz bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbelleği.
  * Ölçeklendirme olamaz bir **standart** aşağı önbelleğe bir **temel** önbelleği.
* Ölçeklendirme yapılabilir bir **temel** için önbelleğe bir **standart** önbellek ancak değiştiremiyor boyutu aynı anda. Farklı bir boyut gerekiyorsa, istenen boyuta sonraki bir ölçeklendirme işlemi yapabilirsiniz.
* Ölçeklendirme olamaz bir **temel** doğrudan önbelleğe bir **Premium** önbelleği. Öğesinden önce Ölçeklendirmesi gerekir **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** bir sonraki ölçeklendirme işleminde.
* Büyük bir değerden aşağı ölçeklendirme olamaz **C0 (250 MB)** boyutu.

Bir ölçeklendirme işlemi başarısız olursa, hizmet işlemini geri döndürmeyi dener ve önbellek özgün durumuna döner.

### <a name="how-long-does-scaling-take"></a>Ne kadar ölçeklendirme sürer?
Yaklaşık 20 önbellekte ne kadar veri bağlı olarak dakika sürer ölçeklendirme.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Ölçeklendirme tamamlandığında nasıl anlayabilirim?
Azure portalında ölçeklendirme işlemi sürüyor görebilirsiniz. Ölçeklendirme tamamlandığında, önbellek durumu değişikliklerini **çalıştıran**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



