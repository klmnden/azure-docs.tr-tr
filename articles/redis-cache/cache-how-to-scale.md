---
title: Azure Redis önbelleğine ölçeklendirme | Microsoft Docs
description: Azure Redis önbelleği örneklerinizin ölçeklendirmeyi öğrenin
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: wesmc
ms.openlocfilehash: 885258379e71ea945e41c4b43c34b35b16dd4a7a
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42057432"
---
# <a name="how-to-scale-azure-redis-cache"></a>Azure Redis önbelleğine ölçeklendirme
Azure Redis önbelleği, önbellek boyutunu ve özelliklerini, tercih ettiğiniz esneklik sağlayan farklı bir önbellek teklifleri sahiptir. Önbellek oluşturulduktan sonra uygulamanızın gereksinimlerini değiştirirseniz, boyutu ve fiyatlandırma katmanı önbellek ölçeklendirebilirsiniz. Bu makalede Azure portalı ve Azure PowerShell ve Azure CLI gibi araçları kullanarak, önbellek ölçeklendirme gösterilmektedir.

## <a name="when-to-scale"></a>Ne zaman ölçeklendirme yapılmalıdır?
Kullanabileceğiniz [izleme](cache-how-to-monitor.md) durumunu ve performansını önbelleğinizin izlemek ve ne zaman önbellek ölçeklendirme belirlemenize yardımcı olmak için Azure Redis Cache özellikleri. 

Ölçeği artırmanız gerekiyorsa belirlemeye yardımcı olması için aşağıdaki ölçümleri izleyebilirsiniz.

* Redis Sunucu Yükü
* Bellek Kullanımı
* Ağ Bant Genişliği
* CPU Kullanımı

Önbelleğinizi artık uygulamanızın gereksinimlerini karşılıyor mu olduğunu belirlerseniz, uygulamanız için en uygun fiyatlandırma daha büyük veya küçük bir önbellek ölçeklendirebilirsiniz. Fiyatlandırma katmanı kullanmak için hangi önbellek belirleme hakkında daha fazla bilgi için bkz. [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Önbellek ölçeklendirme
Önbelleğinizi, ölçeklendirmek için [önbelleğe Gözat](cache-configure.md#configure-redis-cache-settings) içinde [Azure portalında](https://portal.azure.com) tıklatıp **ölçek** gelen **kaynak menüsünde**.

![Ölçek](./media/cache-how-to-scale/redis-cache-scale-menu.png)

İstenen fiyatlandırma katmanı seçin **fiyatlandırma katmanı seçme** dikey de **seçin**.

![Fiyatlandırma katmanı][redis-cache-pricing-tier-blade]


Aşağıdaki kısıtlamalarla birlikte farklı bir fiyatlandırma katmanına ölçeklendirebilirsiniz:

* Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanına ölçeklendirilemez.
  * Ölçek genişletilemiyor bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbellek.
  * Ölçek genişletilemiyor bir **standart** aşağı önbelleğe bir **temel** önbellek.
* Değerine ölçeklendirme bir **temel** için önbellek bir **standart** önbellek ancak değiştiremez boyutu aynı anda. Başka bir boyutu gerekiyorsa, istediğiniz boyuta izleyen bir ölçeklendirme işlemi yapabilirsiniz.
* Ölçek genişletilemiyor bir **temel** doğrudan önbelleğe bir **Premium** önbellek. İlk olarak, değerine ölçeklendirme **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** , sonraki ölçeklendirme işlem.
* Büyük bir değerden aşağı ölçeklendirilemez **C0 (250 MB)** boyutu.
 
Önbellek, yeni fiyatlandırma katmanına ölçeklendirme sırasında bir **ölçeklendirme** durumu görüntülenir **Redis Cache** dikey penceresi.

![Ölçeklendirme][redis-cache-scaling]

Ölçeklendirme tamamlandığında, durum değişiklikleri **ölçeklendirme** için **çalıştıran**.

## <a name="how-to-automate-a-scaling-operation"></a>Bir ölçeklendirme işlemi otomatikleştirme
Azure portalında önbelleği örneklerinizin ölçeklendirme yanı sıra PowerShell cmdlet'lerini, Azure CLI kullanarak ölçeklendirilebilir ve Microsoft Azure yönetim kitaplıkları'nı (MAML) kullanarak. 

* [PowerShell kullanarak ölçek](#scale-using-powershell)
* [Azure CLI kullanarak ölçek](#scale-using-azure-cli)
* [Aynı anda MAML kullanarak ölçek](#scale-using-maml)

### <a name="scale-using-powershell"></a>PowerShell kullanarak ölçek
Kullanarak PowerShell ile Azure Redis önbelleği örneklerinizin ölçeğini [Set-AzureRmRedisCache](https://docs.microsoft.com/powershell/module/azurerm.rediscache/set-azurermrediscache?view=azurermps-6.6.0) cmdlet'i, `Size`, `Sku`, veya `ShardCount` özellikleri değiştirilemez. Aşağıdaki örnekte adlı önbellek ölçeklendirme gösterilmiştir `myCache` 2,5 GB önbellek için. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

PowerShell ile ölçeklendirme hakkında daha fazla bilgi için bkz. [Powershell kullanarak Redis önbelleği ölçeklendirme](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Azure CLI kullanarak ölçek
Azure CLI kullanarak Azure Redis önbelleği örneklerinizin ölçeklendirmek için çağrı `azure rediscache set` komut ve yeni boyutu, sku veya istenen ölçeklendirme işleme bağlı olarak, küme boyutu istenen yapılandırma değişiklikleri geçirin.

Azure CLI ile ölçeklendirme hakkında daha fazla bilgi için bkz. [mevcut bir Redis Cache'in ayarlarını değiştir](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Aynı anda MAML kullanarak ölçek
Azure Redis Cache ölçeklendirmek için örnekler kullanarak [Microsoft Azure yönetim kitaplıkları (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), çağrı `IRedisOperations.CreateOrUpdate` yöntemi ve yeni boyutu geçişinde `RedisProperties.SKU.Capacity`.

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

Daha fazla bilgi için [yönetme MAML kullanarak Redis önbelleği](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) örnek.

## <a name="scaling-faq"></a>Ölçeklendirme hakkında SSS
Aşağıdaki liste, Azure Redis Cache ölçeklendirme hakkında sık sorulan soruların yanıtlarını içerir.

* [İçin ya da Premium önbelleğindeki ölçeklendirebilir miyim?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Ölçeklendirme sonra miyim önbellek adı ya da erişim anahtarlarımı değiştirmeniz gerekiyor mu?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Ölçeklendirme nasıl çalışır?](#how-does-scaling-work)
* [My önbellekten ölçeklendirme sırasında miyim veriyi kaybedeceksiniz?](#will-i-lose-data-from-my-cache-during-scaling)
* [My özel veritabanlarını ölçeklendirme sırasında etkilenen ayarlıyor?](#is-my-custom-databases-setting-affected-during-scaling)
* [Önbelleğimin ölçeklendirme sırasında kullanıma sunulacak?](#will-my-cache-be-available-during-scaling)
* [Yapılandırılmış, coğrafi çoğaltma ile neden önbelleğimin ölçeklemek ya da bir kümedeki parça değiştirmek gönderemiyorum?](#scaling-limitations-with-geo-relication)
* [Desteklenmeyen işlemleri](#operations-that-are-not-supported)
* [Nasıl ölçekleme kadar sürer?](#how-long-does-scaling-take)
* [Ölçeklendirme tamamlandığında nasıl anlayabilirim?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>İçin ya da Premium önbelleğindeki ölçeklendirebilir miyim?
* Ölçek genişletilemiyor bir **Premium** aşağı önbelleğe bir **temel** veya **standart** fiyatlandırma katmanı.
* Bir ölçek **Premium** fiyatlandırma katmanı başka bir önbellek.
* Ölçek genişletilemiyor bir **temel** doğrudan önbelleğe bir **Premium** önbellek. İlk olarak, değerine ölçeklendirme **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** , sonraki ölçeklendirme işlem.
* Kümeleme etkinleştirilirse oluştururken, **Premium** önbellek, yapabilecekleriniz [küme boyutu değiştirmek](cache-how-to-premium-clustering.md#cluster-size). Önbelleğinizi etkin Kümeleme olmadan oluşturulduysa, daha sonraki bir zamanda kümeleme yapılandırabilirsiniz.
  
  Daha fazla bilgi için bkz. [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Ölçeklendirme sonra miyim önbellek adı ya da erişim anahtarlarımı değiştirmeniz gerekiyor mu?
Hayır, önbellek adını ve anahtarlarını bir ölçeklendirme işlemi sırasında değiştirilmez.

### <a name="how-does-scaling-work"></a>Ölçeklendirme nasıl çalışır?
* Olduğunda bir **temel** önbellek farklı bir boyuta ölçeklendirilir, kapandığında ve yeni bir önbellek yeni boyut kullanılarak sağlanır. Bu süre boyunca önbellek kullanılamaz duruma ve önbellekteki tüm verileri kaybolur.
* Olduğunda bir **temel** önbellek ölçeği genişletilmiş bir **standart** önbellek, çoğaltma önbelleği sağlandığında ve veri çoğaltma önbelleğe birincil önbellekteki kopyalanır. Önbellek ölçeklendirme işlemi sırasında kullanılabilir durumda kalır.
* Olduğunda bir **standart** başka bir boyutu veya için önbellek ölçeklendirilmiş bir **Premium** önbellek, çoğaltmalarından birini kapatılır ve yeni boyut ve üzerinden aktarılan veriler, ardından başka bir çoğaltma için sağlama silinmeden önce kullanılması, benzer önbellek düğümlerinden birinin hata sırasında gerçekleşen işlem için bir yük devretme gerçekleştirir.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>My önbellekten ölçeklendirme sırasında miyim veriyi kaybedeceksiniz?
* Olduğunda bir **temel** önbellek yeni bir boyuta ölçeklendirilir, tüm veriler kaybolur ve önbellek ölçeklendirme işlemi sırasında kullanılamaz.
* Olduğunda bir **temel** önbellek ölçeği genişletilmiş bir **standart** önbellek, önbellekteki verilerin genellikle korunur.
* Olduğunda bir **standart** daha büyük bir boyut veya katmanı önbellek ölçeği veya **Premium** önbellek daha büyük bir boyuta ölçeklendirilir, tüm veriler genellikle korunur. Ölçeklendirme bir **standart** veya **Premium** veri daha küçük bir boyut gösteriyor önbellek için yeni boyut ölçeklendiğinde ilgili önbellekte ne kadar veri olduğuna bağlı olarak kayıp. Veri ölçeği azaltma sırasında kaybolsa bile anahtarları kullanarak çıkarılan [allkeys lru](http://redis.io/topics/lru-cache) çıkarma ilkesi. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>My özel veritabanlarını ölçeklendirme sırasında etkilenen ayarlıyor?
Özel bir değer için yapılandırılmışsa `databases` önbellek oluşturma işlemi sırasında ayarlama, bazı fiyatlandırma katmanlarını etkilenebileceğini sahip farklı [veritabanları sınırları](cache-configure.md#databases). Bu senaryoda ölçeklerken bazı noktalar şunlardır:

* Bir fiyatlandırma katmanına düşük ile ölçeklendirme `databases` geçerli katmanı sınırından:
  * Varsayılan sayısını kullanıyorsanız `databases`16 tüm fiyatlandırma katmanlarına yönelik olan, veri kaybedilmez.
  * Özel bir dizi kullanıyorsanız `databases` için ölçekleme, bu katman için sınırlar içinde kalan `databases` ayarı korunur ve veri kaybedilmez.
  * Özel bir dizi kullanıyorsanız `databases` , yeni katmanı limitlerini `databases` ayarı yeni katman sınırlarına kadar değerin ve kaldırılan veritabanlarındaki tüm veriler kaybolur.
* Aynı veya daha yüksek bir fiyatlandırma katmanına ölçeklendirme `databases` geçerli katmanı sınırından, `databases` ayarı korunur ve veri kaybedilmez.

Standart ve Premium önbellekler kullanılabilirlik için % 99,9 SLA olsa da, veri kaybı için SLA yoktur.

### <a name="will-my-cache-be-available-during-scaling"></a>Önbelleğimin ölçeklendirme sırasında kullanıma sunulacak?
* **Standart** ve **Premium** önbellekler kullanılabilir olmaya devam eder bir ölçeklendirme işlemi sırasında. Ancak, bağlantı blips, standart ve Premium önbellekler ölçeklendirme sırasında ve ayrıca da temel katmandan standart önbellekler için ölçeklendirme sırasında ortaya çıkabilir. Bu bağlantı blips küçük olmaları beklenir ve redis istemcilerinin kendi anında yeniden bağlantı görüyor olmalısınız.
* **Temel** önbellekler çevrimdışı işlemleri farklı bir boyuta ölçeklendirme sırasında. Temel önbellekler kullanılabilir olmaya devam eder gelen ölçeklerken **temel** için **standart** ancak küçük bağlantı blip karşılaşabilirsiniz. Bir bağlantı blip ortaya çıkarsa, redis istemcilerinin kendi anında yeniden bağlantı mümkün olması gerekir.


### <a name="scaling-limitations-with-geo-replication"></a>Coğrafi çoğaltma ile ölçekleme sınırlamaları

Artık iki önbellekler arasında coğrafi çoğaltma bağlantı ekledikten sonra bir ölçeklendirme işlemi başlatabilir ya da bir kümedeki parça sayısını değiştirmek mümkün olmayacak. Bu komutları için önbellek bağlantısını kaldırmanız gerekir. Daha fazla bilgi için [yapılandırma coğrafi çoğaltma](cache-how-to-geo-replication.md).


### <a name="operations-that-are-not-supported"></a>Desteklenmeyen işlemleri
* Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanına ölçeklendirilemez.
  * Ölçek genişletilemiyor bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbellek.
  * Ölçek genişletilemiyor bir **standart** aşağı önbelleğe bir **temel** önbellek.
* Değerine ölçeklendirme bir **temel** için önbellek bir **standart** önbellek ancak değiştiremez boyutu aynı anda. Başka bir boyutu gerekiyorsa, istediğiniz boyuta izleyen bir ölçeklendirme işlemi yapabilirsiniz.
* Ölçek genişletilemiyor bir **temel** doğrudan önbelleğe bir **Premium** önbellek. İlk ölçeklendirme **temel** için **standart** bir ölçeklendirme işlemi ve ölçek kümesinden **standart** için **Premium** sonraki işlem.
* Büyük bir değerden aşağı ölçeklendirilemez **C0 (250 MB)** boyutu.

Bir ölçeklendirme işlemi başarısız olursa hizmeti işlemi geri almak çalışır ve önbelleği özgün durumuna geri döner.


### <a name="how-long-does-scaling-take"></a>Nasıl ölçekleme kadar sürer?
Yaklaşık 20 önbellekte ne kadar veri olduğuna bağlı olarak dakika sürer ölçeklendirilmesi.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Ölçeklendirme tamamlandığında nasıl anlayabilirim?
Azure portalında bir ölçeklendirme işlemi sürüyor görebilirsiniz. Ölçeklendirme tamamlandığında, önbellek durumu değişerek **çalıştıran**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



