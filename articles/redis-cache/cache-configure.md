---
title: "Azure Redis önbelleği yapılandırma | Microsoft Docs"
description: "Azure Redis önbelleği için varsayılan Redis yapılandırma anlamak ve Azure Redis önbelleği örnekleri yapılandırmayı öğrenin"
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: wesmc
ms.openlocfilehash: 2e2e22c17bce4bdaf4988001db8de31b68f497fc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-configure-azure-redis-cache"></a>Azure Redis önbelleğini yapılandırma
Bu konuda, Azure Redis önbelleği örnekleri için kullanılabilir yapılandırmaları açıklanmaktadır. Bu konuda, Azure Redis önbelleği örnekleri için varsayılan Redis sunucu yapılandırması da kapsar.

> [!NOTE]
> Yapılandırma ve premium önbellek özellikleri kullanma hakkında daha fazla bilgi için bkz: [kalıcılığı yapılandırma](cache-how-to-premium-persistence.md), [kümeleri yapılandırma](cache-how-to-premium-clustering.md), ve [sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Redis önbelleği ayarları
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis önbelleği ayarları görüntülenebilir ve yapılandırılan **Redis önbelleği** dikey penceresini kullanarak **kaynak menü**.

![Redis önbelleği ayarları](./media/cache-configure/redis-cache-settings.png)

Görüntüleyebilir ve kullanarak aşağıdaki ayarları yapılandırın **kaynak menü**.

* [Genel Bakış](#overview)
* [Etkinlik Günlüğü](#activity-log)
* [Erişim denetimi (IAM)](#access-control-iam)
* [Etiketler](#tags)
* [Sorunları tanılama ve çözme](#diagnose-and-solve-problems)
* [Ayarlar](#settings)
    * [Erişim tuşları](#access-keys)
    * [Gelişmiş ayarlar](#advanced-settings)
    * [Redis önbelleği Danışmanı](#redis-cache-advisor)
    * [Ölçeklendirme](#scale)
    * [Küme boyutu redis](#cluster-size)
    * [Redis veri kalıcılığını](#redis-data-persistence)
    * [Güncelleştirmeleri zamanlama](#schedule-updates)
    * [Coğrafi çoğaltma](#geo-replication)
    * [Sanal Ağ](#virtual-network)
    * [Güvenlik duvarı](#firewall)
    * [özellikleri](#properties)
    * [Kilitler](#locks)
    * [Otomasyon komut dosyası](#automation-script)
* [Yönetim](#administration)
    * [Veri içeri aktarma](#importexport)
    * [Verileri dışarı aktarma](#importexport)
    * [Yeniden başlatma](#reboot)
* [İzleme](#monitoring)
    * [Ölçümleri redis](#redis-metrics)
    * [Uyarı kuralları](#alert-rules)
    * [Tanılama](#diagnostics)
* [Destek ve sorun giderme ayarları](#support-amp-troubleshooting-settings)
    * [Kaynak durumu](#resource-health)
    * [Yeni destek isteği](#new-support-request)


## <a name="overview"></a>Genel Bakış

**Genel Bakış** , fiyatlandırma katmanı ve seçilen önbellek ölçümleri adı gibi önbelleğiniz hakkında temel bilgileri sizinle bağlantı noktaları sağlar.

### <a name="activity-log"></a>Etkinlik günlüğü

Tıklatın **etkinlik günlüğü** önbelleğiniz üzerinde gerçekleştirilen eylemler görüntülemek için. Diğer kaynaklar dahil etmek için bu görünümü genişletmek için filtreleme de kullanabilirsiniz. Denetim günlükleri ile çalışma hakkında daha fazla bilgi için bkz: [denetim işlemleri Resource Manager ile](../azure-resource-manager/resource-group-audit.md). Azure Redis önbelleği olaylar izleme hakkında daha fazla bilgi için bkz: [işlemleri ve Uyarıları](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM)** bölüm Azure portalında rol tabanlı erişim denetimi (RBAC) için destek sağlar. Bu yapılandırma, kuruluşların kendi erişim yönetimi sadece ve tam olarak gereksinimlerini yardımcı olur. Daha fazla bilgi için bkz: [Azure portalında rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Etiketler

**Etiketleri** bölüm kaynaklarınızı düzenlemenize yardımcı olur. Daha fazla bilgi için bkz: [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Sorunları tanılama ve çözme

Tıklatın **Tanıla ve sorunları** çözümlemek için ortak sorunlar ve stratejileri sağlanmalıdır.



## <a name="settings"></a>Ayarlar
**Ayarları** bölümü erişmek ve önbelleğiniz için aşağıdaki ayarları yapılandırmanıza olanak sağlar.

* [Erişim tuşları](#access-keys)
* [Gelişmiş ayarlar](#advanced-settings)
* [Redis önbelleği Danışmanı](#redis-cache-advisor)
* [Ölçeklendirme](#scale)
* [Küme boyutu redis](#cluster-size)
* [Redis veri kalıcılığını](#redis-data-persistence)
* [Güncelleştirmeleri zamanlama](#schedule-updates)
* [Coğrafi çoğaltma](#geo-replication)
* [Sanal Ağ](#virtual-network)
* [Güvenlik duvarı](#firewall)
* [özellikleri](#properties)
* [Kilitler](#locks)
* [Otomasyon komut dosyası](#automation-script)



### <a name="access-keys"></a>Erişim tuşları
Tıklatın **erişim anahtarları** görüntülemek veya önbelleğiniz için erişim anahtarlarını yeniden oluşturmak için. Bu anahtarları önbelleğiniz için bağlanan istemciler tarafından kullanılır.

![Redis önbelleği erişim tuşları](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Gelişmiş ayarlar
Aşağıdaki ayarları yapılandırılır **Gelişmiş ayarları** dikey.

* [Erişim bağlantı noktaları](#access-ports)
* [Bellek ilkeleri](#memory-policies)
* [Keyspace bildirimleri (Gelişmiş ayarları)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Erişim Bağlantı Noktaları
SSL olmayan erişim yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL olmayan bağlantı noktasını etkinleştirmek için **Hayır** için **yalnızca SSL aracılığıyla erişime izin** üzerinde **Gelişmiş ayarları** dikey ve ardından **kaydetmek**.

![Redis önbelleği erişim bağlantı noktaları](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Bellek ilkeleri
**Maxmemory İlkesi**, **maxmemory ayrılmış**, ve **maxfragmentationmemory ayrılmış** ayarlarını **Gelişmiş ayarları** dikey önbelleği için bellek ilkeleri yapılandırın.

![Redis önbelleği Maxmemory İlkesi](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory İlkesi** önbelleği için çıkarma İlkesi yapılandırır ve aşağıdaki çıkarma ilkelerden seçmenize olanak sağlar:

* `volatile-lru` -Varsayılan çıkarma İlkesi budur.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Hakkında daha fazla bilgi için `maxmemory` ilkeleri Bkz [çıkarma ilkeleri](http://redis.io/topics/lru-cache#eviction-policies).

**Maxmemory ayrılmış** ayarı, yük devretme sırasında çoğaltma gibi önbellek olmayan işlemleri için ayrılmış MB bellek miktarını yapılandırır. Bu değer ayarlandığında yük değişiklik gösterdiğinde daha tutarlı bir Redis sunucu deneyim sahip olmanızı sağlar. Bu değer ağır yazma iş yükleri için yüksek ayarlamanız gerekir. Bu tür işlemler için ayrılmış bellek, önbelleğe alınan veri depolama için kullanılabilir değil.

**Maxfragmentationmemory ayrılmış** ayarı için bellek parçalanması uyum sağlamak için ayrılmış MB bellek miktarını yapılandırır. Bu değer ayarlandığında önbellek dolu ya da tam ve parçalanma yakın oranı yüksek olduğunda daha tutarlı bir Redis sunucu deneyim sahip olmanızı sağlar. Bu tür işlemler için ayrılmış bellek, önbelleğe alınan veri depolama için kullanılabilir değil.

Yeni bir bellek ayırma değeri seçerken dikkat etmeniz gereken tek şey (**maxmemory ayrılmış** veya **maxfragmentationmemory ayrılmış**) Bu değişiklik büyük miktarlarda veri ile çalışan bir önbellek nasıl etkileyebileceğini değil. Örneğin, 49 GB veri 53 GB'a önbellekle sahip ardından ayırma değeri için 8 GB değiştirin, bu değişikliği sistem 45 GB azaltmak için en çok kullanılabilir bellek bırakın. Her iki geçerli `used_memory` veya `used_memory_rss` değerlerdir 45 GB yeni sınırdan daha yüksek sonra sistem erene kadar veri Tahliye gerekecek `used_memory` ve `used_memory_rss` 45 GB olan. Çıkarma sunucu yükü ve bellek parçalanması artırabilir. Önbellek ölçümleri gibi hakkında daha fazla bilgi için `used_memory` ve `used_memory_rss`, bkz: [kullanılabilir Ölçümler ve aralıklarını raporlama](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> **Maxmemory ayrılmış** ve **maxfragmentationmemory ayrılmış** ayarlarını bulunan ve yalnızca için standart ve Premium önbelleğe alır.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Keyspace bildirimleri (Gelişmiş ayarları)
Keyspace bildirimleri yapılandırılır redis **Gelişmiş ayarları** dikey. Keyspace bildirimleri belirli olaylar oluştuğunda bildirimleri almak istemcilerinin izin verir.

![Redis önbelleği Gelişmiş ayarlar](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspace bildirimleri ve **bildir-keyspace-olayları** ayarı bulunan ve yalnızca standart ve Premium önbellekler için.
> 
> 

Daha fazla bilgi için bkz: [Redis Keyspace bildirimleri](http://redis.io/topics/notifications). Örnek kod için bkz: [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) dosyasını [Merhaba Dünya](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Cache Danışmanı
**Redis önbelleği Danışmanı** dikey önbelleğiniz için öneriler görüntüler. Normal işlemler sırasında öneri yok görüntülenir. 

![Öneriler](./media/cache-configure/redis-cache-no-recommendations.png)

Önbelleğinizi yüksek bellek kullanımı, ağ bant genişliği veya sunucu iş yükü gibi işlemleri sırasında tüm koşullar meydana gelirse, bir uyarı görüntülenir **Redis önbelleği** dikey.

![Öneriler](./media/cache-configure/redis-cache-recommendations-alert.png)

Daha fazla bilgi bulunabilir **önerileri** dikey.

![Öneriler](./media/cache-configure/redis-cache-recommendations.png)

Bu ölçümleri izleyebilirsiniz [izleme grafikleri](cache-how-to-monitor.md#monitoring-charts) ve [kullanım grafiklerini](cache-how-to-monitor.md#usage-charts) bölümlerini **Redis önbelleği** dikey.

Her fiyatlandırma katmanının istemci bağlantıları, bellek ve bant genişliği için farklı sınırları vardır. Bir öneri, önbelleğiniz aralıksız bir süre boyunca Bu ölçümler için maksimum kapasite yaklaşıyor değilse oluşturulur. Ölçümleri ve İnceleme sınırları hakkında daha fazla bilgi için **önerileri** aracı, aşağıdaki tabloya bakın:

| Redis önbelleği ölçüm | Daha fazla bilgi |
| --- | --- |
| Ağ bant genişliği kullanımı |[Önbellek performansı - kullanılabilir bant genişliği](cache-faq.md#cache-performance) |
| Bağlanan istemciler |[Redis sunucu yapılandırması - maxclients varsayılan](#maxclients) |
| Sunucu iş yükü |[Kullanım grafikler - Redis sunucu iş yükü](cache-how-to-monitor.md#usage-charts) |
| Bellek kullanımı |[Önbellek performansı - boyutu](cache-faq.md#cache-performance) |

Önbelleğinizi yükseltmek için tıklayın **Şimdi Yükselt** fiyatlandırma katmanını değiştirmek için ve [ölçek](#scale) önbelleğiniz. Bir fiyatlandırma katmanı seçme hakkında daha fazla bilgi için bkz: [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Ölçek
Tıklatın **ölçek** görüntülemek veya önbelleğiniz için fiyatlandırma katmanını değiştirmek için. Ölçeklendirme ile ilgili daha fazla bilgi için bkz: [ölçek Azure Redis önbelleği nasıl](cache-how-to-scale.md).

![Redis önbelleği fiyatlandırma katmanı](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Küme boyutu redis
Tıklatın **(Önizleme) Redis küme boyutu** çalışan bir küme boyutunu değiştirmek için Etkin kümeleme ile premium önbelleği.

> [!NOTE]
> Azure Redis önbelleği Premium katmanına genel kullanılabilirlik yayımlanan olsa da, Redis küme boyutu özelliği şu anda önizlemede olduğuna dikkat edin.
> 
> 

![Küme boyutu redis](./media/cache-configure/redis-cache-redis-cluster-size.png)

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya 1 ile 10 arasında bir sayı yazın **parça sayısı** metin kutusu ve tıklatın **Tamam** kaydetmek için.

> [!IMPORTANT]
> Redis kümeleme yalnızca Premium önbellekler için kullanılabilir. Daha fazla bilgi için bkz. [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Tıklatın **Redis veri kalıcılığını** etkinleştirmek için devre dışı bırakmak veya premium önbelleğiniz veri kalıcılığını yapılandırma. Azure Redis önbelleği Redis kalıcılığı kullanarak sunar [RDB kalıcılığı](cache-how-to-premium-persistence.md#configure-rdb-persistence) veya [AOF kalıcılığı](cache-how-to-premium-persistence.md#configure-aof-persistence).

Daha fazla bilgi için bkz: [Premium Azure Redis önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Redis veri kalıcılığını yalnızca Premium önbellekler için kullanılabilir. 
> 
> 

### <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Zamanlama güncelleştirmeleri** dikey önbelleğiniz için Redis server güncelleştirmeleri için bir bakım penceresi tanımlamanızı sağlar. 

> [!IMPORTANT]
> Bakım penceresi yalnızca sunucu güncelleştirmelerini Redis için geçerlidir ve değil tüm Azure güncelleştirmelerini veya önbellek barındıran sanal makineleri için işletim sistemini güncelleştirir.
> 
> 

![Güncelleştirmeleri zamanlama](./media/cache-configure/redis-schedule-updates.png)

Bir bakım penceresi belirtmek için istenen gün kontrol edin ve her gün için bakım penceresi başlangıç saati belirtin ve tıklatın **Tamam**. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

> [!IMPORTANT]
> **Zamanlama güncelleştirmeleri** işlevdir yalnızca Premium katmanı önbellekler için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz: [Azure Redis önbelleği yönetim - güncelleştirmeleri zamanla](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** dikey iki Premium katmanı Azure Redis önbelleği örnekleri bağlama için bir mekanizma sağlar. Bir önbellek birincil bağlantılı önbellek ve diğer ikincil bağlantılı önbelleği olarak atanır. İkincil bağlantılı önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlantılı önbelleğine çoğaltılır. Bu işlev, Azure bölgeler arasında bir önbellek çoğaltmak için kullanılabilir.

> [!IMPORTANT]
> **Coğrafi çoğaltma** yalnızca Premium katmanı önbellekler için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz: [Azure Redis önbelleği için coğrafi çoğaltma yapılandırma](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Sanal Ağ
**Sanal ağ** bölümü önbelleğiniz için sanal ağ ayarlarını yapılandırmanıza olanak sağlar. VNET ile birlikte premium önbelleği oluşturma hakkında bilgi için destek ve ayarlarını güncelleştirmek, bkz: [Premium Azure Redis önbelleği için sanal ağ desteği yapılandırma](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Sanal ağ ayarları yapılandırıldı premium önbelleklere kullanılabilir yalnızca önbellek oluşturma sırasında VNET desteğiyle. 
> 
> 

### <a name="firewall"></a>Güvenlik duvarı

Güvenlik duvarı kuralları yapılandırma tüm Azure Redis önbelleği katmanları için kullanılabilir.

Tıklatın **Güvenlik Duvarı** görüntülemek ve önbellek için güvenlik duvarı kurallarını yapılandırmak için.

![Güvenlik duvarı](./media/cache-configure/redis-firewall-rules.png)

Güvenlik duvarı kuralları ile bir başlangıç ve bitiş IP adresi aralığı belirtebilirsiniz. Güvenlik duvarı kuralları yapılandırıldığında, yalnızca belirtilen IP adresi aralıklarında'ten istemci bağlantılarını önbelleğe bağlanabilir. Bir güvenlik duvarı kuralı kaydedildi olduğunda kısa bir gecikme kural etkilidir önce. Bu gecikme, normal bir dakikadan az olur.

> [!IMPORTANT]
> Güvenlik duvarı kuralları yapılandırılmış olsa bile Azure Redis önbelleği sistemleri izleme bağlantılarından her zaman, izin verilir.
> 
> 

### <a name="properties"></a>Özellikler
Tıklatın **özellikleri** önbellek uç noktası ve bağlantı noktaları da dahil olmak üzere önbelleğiniz hakkındaki bilgileri görüntülemek için.

![Redis önbelleği özellikleri](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Kilitler
**Kilitler** bölümü abonelik, kaynak grubu veya kaynak yanlışlıkla silinmesi ya da kritik kaynaklara değiştirme kuruluşunuzda bulunan diğer kullanıcıların önlemek için kilitlemenizi sağlar. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Otomasyon betiği

Tıklatın **Otomasyon betiğini** oluşturmak ve dağıtılmış kaynaklarınızın gelecekteki dağıtımlar için bir şablonu dışarı aktarmak için. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [kaynakları Azure Resource Manager şablonları ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Yönetim ayarları
Ayarlarında **Yönetim** bölüm önbelleğiniz için aşağıdaki yönetim görevlerini gerçekleştirmek izin. 

![Yönetim](./media/cache-configure/redis-cache-administration.png)

* [Veri içeri aktarma](#importexport)
* [Verileri dışarı aktarma](#importexport)
* [Yeniden başlatma](#reboot)


### <a name="importexport"></a>İçeri/Dışarı Aktarma
İçeri/dışarı aktarma alma ve içeri aktarma ve Redis önbelleği veritabanı'nı (RDB) anlık görüntü premium önbellekten bir Azure depolama hesabındaki bir sayfa blob'u verme önbellekteki verileri verme olanak tanıyan bir Azure Redis önbelleği veri yönetimi, bir işlemdir. İçeri/dışarı aktarma, farklı Azure Redis önbelleği örnekleri arasında geçirmek veya önbellek kullanmadan önce verileri ile doldurmak sağlar.

İçeri aktarma, herhangi bir bulut veya ortamını Linux, Windows ya da herhangi bir bulut sağlayıcısına Amazon Web Hizmetleri ve diğerleri gibi çalışan Redis çalıştıran herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Veri alma, önceden doldurulmuş haldedir verilerle önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure Redis önbelleği RDB dosyaları Azure Storage'dan belleğe yükler ve ardından anahtarları önbelleğe ekler.

Dışarı aktarma, Azure Redis uyumlu RDB dosyaları Redis için önbellekte depolanan veriler vermenize olanak sağlar. Verileri bir Azure Redis önbelleği örneğinden diğerine veya başka bir Redis sunucuya taşımak için bu özelliği kullanın. Dışa aktarma işlemi sırasında geçici bir dosya Azure Redis önbelleği sunucu örneğini barındıran VM oluşturulur ve dosya belirtilen depolama hesabına yüklenir. Ya da durumunu başarı veya hata ile dışarı aktarma işlemi tamamlandıktan sonra geçici dosya silindi.

> [!IMPORTANT]
> İçeri/dışarı aktarma yalnızca Premium katmanı önbellekler için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz: [içeri ve dışarı aktarma Azure Redis önbelleği verilerde](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Yeniden başlatma
**Yeniden** dikey önbelleğiniz düğümlerinin yeniden başlatılmasını sağlar. Bu yeniden başlatma özelliği, bir önbellek düğümü ise dayanıklılık için uygulamanızı test etmek sağlar.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot.png)

Kümeleme özelliği etkinleştirilmiş bir premium önbelleği varsa, hangi parça önbelleğin yeniden başlatmayı seçebilirsiniz.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot-cluster.png)

Bir veya daha fazla düğüm, önbelleğin yeniden, istediğiniz düğümleri seçin ve ' **yeniden**. Kümeleme özelliği etkinleştirilmiş bir premium önbelleği varsa, yeniden başlatın ve ardından shard(s) seçin **yeniden**. Birkaç dakika sonra seçilen düğümü yeniden başlatma ve bu birkaç dakika sonra yeniden çevrimiçi.

> [!IMPORTANT]
> Yeniden başlatma için tüm fiyatlandırma katmanlarına kullanıma sunulmuştur. Daha fazla bilgi ve yönergeler için bkz: [Azure Redis önbelleği yönetim - yeniden başlatma](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>İzleme

**İzleme** bölümü, tanılama ve Redis önbelleği için izlemeyi yapılandırmanıza olanak sağlar. Azure Redis önbelleği izleme ve Tanılama hakkında daha fazla bilgi için bkz: [Azure Redis önbelleği izleme](cache-how-to-monitor.md).

![Tanılama](./media/cache-configure/redis-cache-diagnostics.png)

* [Ölçümleri redis](#redis-metrics)
* [Uyarı kuralları](#alert-rules)
* [Tanılama](#diagnostics)

### <a name="redis-metrics"></a>Redis ölçümleri
Tıklatın **Redis ölçümleri** için [görüntülemek ölçümleri](cache-how-to-monitor.md#view-cache-metrics) önbelleğiniz için.

### <a name="alert-rules"></a>Uyarı kuralları

Tıklatın **uyarı kuralları** Redis önbelleği ölçümleri temel uyarılarını yapılandırmak için. Daha fazla bilgi için bkz: [uyarıları](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Tanılama

Önbellek ölçümleridir Azure İzleyicisi'nde varsayılan olarak, [30 gün boyunca depolanan](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) ve ardından silinir. Önbellek ölçümlerinizi 30 günden daha uzun bir süre devam ettirmek için tıklatın **tanılama** için [depolama hesabı yapılandırma](cache-how-to-monitor.md#export-cache-metrics) önbellek tanılamayı depolamak için kullanılır.

>[!NOTE]
>Önbellek ölçümlerinizi depolama arşivleme ek olarak, şunları da yapabilirsiniz [olay hub'ına akış veya günlük analizi için Gönder](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

## <a name="support--troubleshooting-settings"></a>Destek ve sorun giderme ayarları
Ayarlarında **destek + sorun giderme** bölüm sağlar, önbellek ile ilgili sorunları çözmek için Seçenekler.

![Destek ve sorun giderme](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Kaynak durumu](#resource-health)
* [Yeni destek isteği](#new-support-request)

### <a name="resource-health"></a>Kaynak durumu
**Kaynak durumu** kaynağınız izler ve beklendiği gibi çalışıp çalışmadığını belirtir. Azure kaynak sistem durumu hizmeti hakkında daha fazla bilgi için bkz: [Azure kaynak sistem durumu genel bakış](../resource-health/resource-health-overview.md).

> [!NOTE]
> Kaynak durumu sanal ağında barındırılan Azure Redis önbelleği örnekleri durumunu raporlamak şu anda alamıyor. Daha fazla bilgi için bkz: [tüm önbellek özellikleri VNET önbelleğinde barındırdığında çalışır?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Yeni destek isteği
Tıklatın **yeni destek isteği** önbellek hesabınız için bir destek isteği açın.





## <a name="default-redis-server-configuration"></a>Varsayılan Redis sunucu yapılandırması
Yeni Azure Redis önbelleği örnekleri aşağıdaki varsayılan Redis yapılandırma değerlerle yapılandırılır:

> [!NOTE]
> Bu bölümdeki ayarları kullanılarak değiştirilemez `StackExchange.Redis.IServer.ConfigSet` yöntemi. Bu yöntem, bu bölümdeki komutlar biriyle çağrılırsa, aşağıdaki örneğe benzer bir özel durum oluşur:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Yapılandırılabilir gibi herhangi bir değeri **en fazla bellek İlkesi**, Azure portalında veya Azure CLI veya PowerShell gibi komut satırı yönetim araçları aracılığıyla yapılandırılabilir.
> 
> 

| Ayar | Varsayılan değer | Açıklama |
| --- | --- | --- |
| `databases` |16 |16 veritabanı varsayılan sayısı olmakla birlikte fiyatlandırma katmanını temel alan farklı bir numara yapılandırabilirsiniz. <sup>1</sup> varsayılan veritabanı DB 0, farklı bir bağlantı başına kullanma temel seçebileceğiniz `connection.GetDatabase(dbid)` nerede `dbid` arasında bir sayı `0` ve `databases - 1`. |
| `maxclients` |Fiyatlandırma katmanını bağlıdır<sup>2</sup> |Bu değer bağlanan istemciler aynı anda izin verilen en fazla sayısıdır. Sınıra ulaşıldıktan sonra Redis tüm yeni bağlantıları 'en fazla istemci sayısı üst sınırına' hata dönülür. |
| `maxmemory-policy` |`volatile-lru` |Redis ne zaman kaldırılacağını nasıl seçtiği için Maxmemory İlkesi ayarı olan `maxmemory` (önbellek oluşturduğunuzda seçtiğiniz sunumu önbellek boyutunu) ulaştı. Azure Redis önbelleği ile varsayılan ayardır `volatile-lru`, LRU algoritması kullanılarak ayarlanan bir sona erme ile anahtarlarını kaldırır. Bu ayar Azure Portalı'nda yapılandırılabilir. Daha fazla bilgi için bkz: [bellek ilkeleri](#memory-policies). |
| `maxmemory-samples` |3 |Bellekten tasarruf etmek LRU ve TTL algoritmaları en az hassas algoritmaları yerine yaklaşık algoritmaları şunlardır. Varsayılan olarak, denetimleri üç anahtarları ve çekmeleri daha az son kullanılan bir Redis. |
| `lua-time-limit` |5.000 |Milisaniye cinsinden Lua komut dosyası yürütme zaman sınırı. En fazla çalışma zamanı ulaştıysanız, Redis bir komut dosyası yürütme en fazla izin verilen süre sonra hala kullanılıyor ve bir hata ile sorguları yanıtlamak başlatır günlüğe kaydeder. |
| `lua-event-limit` |500 |Komut dosyası olay sırasının en büyük boyutu. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |İstemci çıkış arabelleği sınırları veri yeterince hızlı sunucudan (bir ortak Pub/alt istemci iletileri yayımcı oluşturmak üzere kadar hızlı kullanamayacaklarını nedeni) herhangi bir nedenden dolayı okuyorsanız olmayan istemciler bağlantısının kesilmesi zorlamak için kullanılabilir. Daha fazla bilgi için bkz. [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>sınırını `databases` her Azure Redis fiyatlandırma katmanı önbelleği için farklıdır ve önbellek oluşturma sırasında ayarlanabilir. Öyle değilse `databases` ayarı belirtilen önbellek oluşturma sırasında varsayılan değer olan 16.

* Temel ve standart önbellekler
  * C0 - 16 veritabanları kadar (250 MB) önbelleği
  * C1 - 16 veritabanları kadar (1 GB) önbelleği
  * C2 - 16 veritabanları kadar (2,5 GB) önbelleği
  * C3 - 16 veritabanları kadar (6 GB) önbelleği
  * C4 - 32 veritabanları kadar (13 GB) önbelleği
  * C5 - 48 veritabanları kadar (26 GB) önbelleği
  * C6 (53 GB'a) önbellek - en fazla 64 veritabanları
* Premium önbelleklere
  * P1 (6 GB - 60 GB) - en fazla 16 veritabanları
  * P2 (13 GB - 130 GB) - en fazla 32 veritabanları
  * P3 (26 GB - 260 GB) - 48 veritabanları kadar
  * P4 (53 GB'a - 530 GB) - en fazla 64 veritabanları
  * Etkin - Redis kümesi ile tüm premium önbelleklere Redis kümesi yalnızca 0 veritabanı kullanımını destekler. böylece `databases` sınırlamak için bir premium önbelleği etkin Redis kümesi ile etkili bir şekilde 1 ve [seçin](http://redis.io/commands/select) komutu izin verilmez. Daha fazla bilgi için bkz: [kümeleme kullanmak üzere istemci Uygulamam herhangi bir değişiklik yapmanız gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Veritabanları hakkında daha fazla bilgi için bkz: [Redis veritabanları nelerdir?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> `databases` Ayarını önbelleği oluşturma sırasında yalnızca yapılandırılmış ve yalnızca PowerShell, CLI veya diğer yönetim istemcileri kullanarak olabilir. Yapılandırma örneği için `databases` PowerShell kullanarak önbellek oluşturma sırasında bkz [yeni AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` her Azure Redis fiyatlandırma katmanı önbelleği için farklıdır.

* Temel ve standart önbellekler
  * C0 - 256 bağlantıları kadar (250 MB) önbelleği
  * C1 - 1.000 bağlantıları kadar (1 GB) önbelleği
  * C2 - 2.000 bağlantıları kadar (2,5 GB) önbelleği
  * C3 - 5.000 bağlantıları kadar (6 GB) önbelleği
  * C4 - 10.000 bağlantıları kadar (13 GB) önbelleği
  * C5 - 15.000 bağlantıları kadar (26 GB) önbelleği
  * C6 - 20.000 bağlantıları kadar (53 GB'a) önbelleği
* Premium önbelleklere
  * P1 (6 GB - 60 GB) - 7.500 bağlantıları kadar
  * P2 (13 GB - 130 GB) - en fazla 15.000 bağlantıları
  * P3 (26 GB - 260 GB) - 30.000 bağlantıları kadar
  * P4 (53 GB'a - 530 GB) - en fazla 40.000 bağlantıları kadar

> [!NOTE]
> Her önbellek boyutunu sağlar, ancak *kadar* belirli bir sayıda bağlantıları, her bağlantı için Redis ek yükü ile ilişkili. TLS/SSL şifreleme sonucunda CPU ve bellek kullanımı gibi ek yükü örneği olacaktır. Belirtilen önbellek boyutu için en fazla bağlantı üst sınırına hafifçe yüklü bir önbellek varsayar. Varsa yük bağlantı yükünü *artı* istemci işlemleri yükü sistem kapasitesini aşıyor, geçerli önbellek boyutu bağlantı sınırını aştınız değil olsa bile önbelleği kapasite sorunlar yaşayabilirsiniz.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Azure Redis Önbelleği'nde desteklenmeyen komutlar redis
> [!IMPORTANT]
> Microsoft tarafından yapılandırma ve Azure Redis önbelleği örnekleri yönetimini yönetilir için aşağıdaki komutları devre dışı bırakıldı. Bunları Invoke çalışırsanız, benzer bir hata iletisi alırsınız `"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * YAPILANDIRMA
> * HATA AYIKLAMA
> * GEÇİRME
> * KAYDET
> * KAPATMA
> * SLAVEOF
> * Küme - komutları devre dışı bırakılır yazma ancak salt okunur Küme komutları izin verilir.
> 
> 

Redis komutları hakkında daha fazla bilgi için bkz: [ http://redis.io/commands ](http://redis.io/commands).

## <a name="redis-console"></a>Konsol redis
Komutları kullanarak, Azure Redis önbelleği örnekleri için güvenli bir şekilde verebilir **Redis konsol**, tüm ön bellek katmanlarını Azure portalında kullanılabilir olduğu.

> [!IMPORTANT]
> - Redis konsolu ile çalışmıyor [VNET](cache-how-to-premium-vnet.md). Önbelleğinizi VNET parçası olduğunda, yalnızca sanal ağ istemcilerinde önbelleğe erişebilir. Redis konsol sanal ağ dışında olan yerel tarayıcınızda çalıştığından, önbelleğiniz bağlanamıyor.
> - Azure Redis Önbelleği'nde desteklenen tüm Redis komutları. Azure Redis önbelleği için devre dışı Redis komutların listesi için önceki bkz [Redis Azure Redis Önbelleği'nde desteklenmeyen komutlar](#redis-commands-not-supported-in-azure-redis-cache) bölümü. Redis komutları hakkında daha fazla bilgi için bkz: [ http://redis.io/commands ](http://redis.io/commands).
> 
> 

Redis konsoluna erişmek için tıklatın **konsol** gelen **Redis önbelleği** dikey.

![Konsol redis](./media/cache-configure/redis-console-menu.png)

Önbellek örneğinizin karşı komutları vermek için istenen komut konsolunda yazın.

![Konsol redis](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a>Redis konsol kümelenmiş premium önbelleği ile kullanma

Önbellek kümelenmiş bir premium ile Redis konsolunu kullanarak, tek bir önbellek parça için komutları verebilir. Bir komutu belirli bir parça için sorun, ilk istenen parça parça Seçici tıklayarak bağlayın.

![Konsol redis](./media/cache-configure/redis-console-premium-cluster.png)

Bağlı parça daha farklı bir parça depolanan bir anahtara erişim denerseniz, aşağıdaki iletiye benzer bir hata iletisini alıyorsunuz:

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

Önceki örnekte, seçili parça parça 1 olan ancak `myKey` parça 0, belirtildiği gibi bulunur `(shard 0)` hata iletisi kısmı. Erişmek için bu örnekte, `myKey`, parça parça seçicisini kullanarak 0'i seçin ve ardından istenen komutu yürütün.


## <a name="move-your-cache-to-a-new-subscription"></a>Yeni bir abonelik önbelleğiniz taşıma
Tıklayarak yeni bir abonelik önbelleğiniz taşıyabilirsiniz **taşıma**.

![Redis önbelleği taşıma](./media/cache-configure/redis-cache-move.png)

Bir kaynak grubundan diğerine ve başka bir bir aboneliğe ilişkin kaynakları taşıma hakkında daha fazla bilgi için bkz: [yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Sonraki adımlar
* Redis komutları ile çalışma hakkında daha fazla bilgi için bkz: [nasıl ı çalıştırabilirsiniz Redis komutları?](cache-faq.md#how-can-i-run-redis-commands)

