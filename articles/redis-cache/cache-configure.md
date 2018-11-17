---
title: Azure Redis Cache yapılandırma | Microsoft Docs
description: Azure Redis Cache için varsayılan Redis yapılandırması anlamak ve Azure Redis önbelleği örneklerinizin yapılandırma hakkında bilgi edinin
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
ms.openlocfilehash: 39c72dde6bcfec2879efd05a1769ad443c9ffd2f
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823971"
---
# <a name="how-to-configure-azure-redis-cache"></a>Azure Redis Cache yapılandırma
Bu konuda, Azure Redis Cache örnekleri için kullanılabilir yapılandırmaları açıklanmaktadır. Bu konuda, Azure Redis Cache örnekleri için varsayılan Redis sunucu yapılandırması da kapsar.

> [!NOTE]
> Yapılandırma ve premium önbellek özelliklerini kullanma hakkında daha fazla bilgi için bkz. [kalıcılığı yapılandırma](cache-how-to-premium-persistence.md), [kümeleri yapılandırma](cache-how-to-premium-clustering.md), ve [sanal ağ desteğini yapılandırma ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Redis önbelleği ayarlarını yapılandırma
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis Cache ayarları görüntülenebilir ve yapılandırılan **Redis Cache** dikey penceresini kullanarak **kaynak menüsünde**.

![Redis Cache ayarları](./media/cache-configure/redis-cache-settings.png)

Görüntüleyebilir ve kullanarak aşağıdaki ayarları yapılandırın **kaynak menüsünde**.

* [Genel Bakış](#overview)
* [Etkinlik Günlüğü](#activity-log)
* [Erişim denetimi (IAM)](#access-control-iam)
* [Etiketler](#tags)
* [Sorunları tanılama ve çözme](#diagnose-and-solve-problems)
* [Ayarlar](#settings)
    * [Erişim anahtarları](#access-keys)
    * [Gelişmiş ayarlar](#advanced-settings)
    * [Redis Cache Danışmanı](#redis-cache-advisor)
    * [Ölçeklendirme](#scale)
    * [Redis küme boyutu](#cluster-size)
    * [Redis veri kalıcılığı](#redis-data-persistence)
    * [Güncelleştirmeleri zamanlama](#schedule-updates)
    * [Coğrafi çoğaltma](#geo-replication)
    * [Sanal Ağ](#virtual-network)
    * [Güvenlik duvarı](#firewall)
    * [Özellikleri](#properties)
    * [Kilitler](#locks)
    * [Otomasyon betiği](#automation-script)
* [Yönetim](#administration)
    * [Veri içeri aktarma](#importexport)
    * [Verileri dışarı aktarma](#importexport)
    * [Yeniden başlatma](#reboot)
* [İzleme](#monitoring)
    * [Redis ölçümleri](#redis-metrics)
    * [Uyarı kuralları](#alert-rules)
    * [Tanılama](#diagnostics)
* [Destek ve sorun giderme ayarları](#support-amp-troubleshooting-settings)
    * [Kaynak durumu](#resource-health)
    * [Yeni destek isteği](#new-support-request)


## <a name="overview"></a>Genel Bakış

**Genel Bakış** fiyatlandırma katmanı ve seçilen önbellek ölçümleri, hakkındaki önbelleğinizin adı gibi temel bilgileri sizinle bağlantı noktaları sağlar.

### <a name="activity-log"></a>Etkinlik günlüğü

Tıklayın **etkinlik günlüğü** önbelleğiniz üzerinde gerçekleştirilen eylemler görüntülemek için. Bu görünüm, diğer kaynakları içerecek şekilde genişletmek için filtreleme de kullanabilirsiniz. Denetim günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Resource Manager denetim işlemleri](../azure-resource-manager/resource-group-audit.md). Azure Redis Cache olayları izleme hakkında daha fazla bilgi için bkz. [işlemler ve Uyarılar](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Erişim denetimi (IAM)

**Erişim denetimi (IAM)** bölümde, Azure portalında rol tabanlı erişim denetimi (RBAC) için destek sağlar. Bu yapılandırma yeterlidir ve tam olarak, erişim yönetimi gereksinimlerini karşılama kuruluşlara yardımcı olur. Daha fazla bilgi için [Azure portalında rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).

### <a name="tags"></a>Etiketler

**Etiketleri** bölüm kaynaklarınızı düzenlemenize yardımcı olur. Daha fazla bilgi için [etiketleri kullanarak Azure kaynaklarınızı düzenleme](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Sorunları tanılama ve çözme

Tıklayın **Tanıla ve problemleri çözmenize** çözümlemek için genel sorunlar ve stratejileri sağlanmalıdır.



## <a name="settings"></a>Ayarlar
**Ayarları** bölümü erişmek ve önbelleğiniz için aşağıdaki ayarları yapılandırmanıza olanak sağlar.

* [Erişim anahtarları](#access-keys)
* [Gelişmiş ayarlar](#advanced-settings)
* [Redis Cache Danışmanı](#redis-cache-advisor)
* [Ölçeklendirme](#scale)
* [Redis küme boyutu](#cluster-size)
* [Redis veri kalıcılığı](#redis-data-persistence)
* [Güncelleştirmeleri zamanlama](#schedule-updates)
* [Coğrafi çoğaltma](#geo-replication)
* [Sanal Ağ](#virtual-network)
* [Güvenlik duvarı](#firewall)
* [Özellikleri](#properties)
* [Kilitler](#locks)
* [Otomasyon betiği](#automation-script)



### <a name="access-keys"></a>Erişim tuşları
Tıklayın **erişim anahtarları** görüntülemek veya önbellek hesabınız için erişim anahtarlarını yeniden oluşturmak için. Bu anahtarlar önbelleğinize bağlanma istemcileri tarafından kullanılır.

![Redis önbelleği erişim tuşları](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Gelişmiş ayarlar
Aşağıdaki ayarları yapılandırılmış **Gelişmiş ayarlar** dikey penceresi.

* [Erişim bağlantı noktaları](#access-ports)
* [Bellek ilkeleri](#memory-policies)
* [Anahtar alanı bildirimleri (Gelişmiş ayarları)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Erişim Bağlantı Noktaları
SSL olmayan erişim yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL olmayan bağlantı noktasını etkinleştirmek için tıklayın **Hayır** için **yalnızca SSL aracılığıyla erişime izin ver** üzerinde **Gelişmiş ayarlar** dikey penceresinde ve ardından **Kaydet**.

![Redis Cache erişim bağlantı noktaları](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Bellek ilkeleri
**Maxmemory İlkesi**, **maxmemory ayrılmış**, ve **maxfragmentationmemory ayrılmış** ayarlarını **Gelişmiş ayarlar** Dikey önbelleği için bellek ilkeleri yapılandırın.

![Redis önbelleği Maxmemory İlkesi](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maks bellek politikası** önbelleğin çıkarma ilkesini yapılandırır ve aşağıdaki çıkarma ilkelerden seçmenize olanak sağlar:

* `volatile-lru` -Varsayılan çıkarma ilkesini budur.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Hakkında daha fazla bilgi için `maxmemory` ilkeleri, [çıkarma ilkelerinin](http://redis.io/topics/lru-cache#eviction-policies).

**Maxmemory ayrılmış** ayarı, yük devretme sırasında çoğaltma gibi önbellek olmayan işlem için ayrılan MB cinsinden bellek miktarını yapılandırır. Bu değeri ayarlamak yük farklılık gösteriyorsa, daha tutarlı bir Redis sunucusuna deneyim sahip olmanızı sağlar. Bu değer, ağır yazma iş yükleri için yüksek olarak ayarlanmalıdır. Bu işlemler için ayrılmış bellek, önbelleğe alınan verilerin depolanması için kullanılamaz.

**Maxfragmentationmemory ayrılmış** ayarı için bellek parçalanması uyum sağlamak için ayrılmış MB cinsinden bellek miktarını yapılandırır. Bu değer ayarlandığında önbellek dolu veya tam ve parçalanma yakın oranı yüksek olduğunda daha tutarlı bir Redis sunucusuna deneyim sahip olmanızı sağlar. Bu işlemler için ayrılmış bellek, önbelleğe alınan verilerin depolanması için kullanılamaz.

Yeni bir bellek ayırma değeri seçerken dikkate alınması gereken bir şey (**maxmemory ayrılmış** veya **maxfragmentationmemory ayrılmış**) Bu değişiklik ile çalışan bir önbellek nasıl etkileyebileceğini olduğu büyük miktarlarda veri. Örneğin, 49 GB veri içeren bir 53 GB önbelleğe sahip ve ardından ayırma değeri için 8 GB değiştirin, bu değişiklik 45 GB aşağı sistemi için en fazla kullanılabilir bellek bırakın. Ya da geçerli `used_memory` veya `used_memory_rss` değerler yeni 45 GB sınırından daha sonra sistem veri erene kadar Tahliye gerekecektir `used_memory` ve `used_memory_rss` 45 GB'ın altında olan. Çıkarma, Sunucu yükünü ve bellek Parçalanmayı arttırabilir. Önbellek ölçümlerini gibi daha fazla bilgi için `used_memory` ve `used_memory_rss`, bkz: [kullanılabilir Ölçümler ve raporlama aralıkları](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> **Maxmemory ayrılmış** ve **maxfragmentationmemory ayrılmış** ayarları bulunan ve yalnızca standart ve Premium önbelleğe alır.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Anahtar alanı bildirimleri (Gelişmiş ayarları)
Anahtar alanı bildirimleri yapılandırılır redis **Gelişmiş ayarlar** dikey penceresi. Anahtar alanı bildirimleri, belirli olaylar gerçekleştiğinde bildirim almak istemcilerinin izin verir.

![Redis önbelleği Gelişmiş ayarlar](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Anahtar alanı bildirimleri ve **bildirim-anahtar alanı-olayları** ayarı kullanılabilir yalnızca standart ve Premium önbellekler için.
> 
> 

Daha fazla bilgi için [Redis anahtar alanı bildirimleri](http://redis.io/topics/notifications). Örnek kod için bkz: [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) dosyası [Merhaba Dünya](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) örnek.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis Cache Danışmanı
**Redis Cache Danışmanı** önbelleğiniz öneriler dikey pencerede görüntülenir. Normal işlemler sırasında önerisi yok görüntülenir. 

![Öneriler](./media/cache-configure/redis-cache-no-recommendations.png)

Önbelleğinizin yüksek bellek kullanımı, ağ bant genişliği veya sunucu iş yükü gibi işlemleri sırasında tüm koşullar meydana gelirse, bir uyarı görüntülenir **Redis Cache** dikey penceresi.

![Öneriler](./media/cache-configure/redis-cache-recommendations-alert.png)

Daha fazla bilgi bulunabilir **önerileri** dikey penceresi.

![Öneriler](./media/cache-configure/redis-cache-recommendations.png)

Bu ölçümleri izleyebileceğiniz [izleme grafikleri](cache-how-to-monitor.md#monitoring-charts) ve [kullanım grafikleri](cache-how-to-monitor.md#usage-charts) bölümlerini **Redis Cache** dikey penceresi.

Her fiyatlandırma katmanının istemci bağlantıları, bellek ve bant genişliği için farklı sınırlara sahiptir. Önbelleğinizi uzun bir süre içinde Bu ölçümler için maksimum kapasite ulaşıyorsa, bir öneri oluşturulur. Ölçümler ve Gözden Geçiren sınırları hakkında daha fazla bilgi için **önerileri** aracı, aşağıdaki tabloya bakın:

| Redis Cache ölçüm | Daha fazla bilgi |
| --- | --- |
| Ağ bant genişliği kullanımı |[Önbellek performansını - bant](cache-faq.md#cache-performance) |
| Bağlı istemciler |[Varsayılan Redis sunucu yapılandırması - maxclients](#maxclients) |
| Sunucu yükü |[Kullanım grafikleri - Redis sunucu yükü](cache-how-to-monitor.md#usage-charts) |
| Bellek kullanımı |[Önbellek performansını - boyutu](cache-faq.md#cache-performance) |

Önbelleğinizi yükseltmek için tıklayın **şimdi yükseltin** fiyatlandırma katmanını değiştirmek için ve [ölçek](#scale) önbelleğinizi. Bir fiyatlandırma katmanı seçme hakkında daha fazla bilgi için bkz: [hangi Redis önbelleği teklifini ve boyutunu kullanmalıyım?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Ölçek
Tıklayın **ölçek** görüntülemek veya önbellek hesabınız için fiyatlandırma katmanını değiştirmek için. Ölçeklendirme hakkında daha fazla bilgi için bkz. [ölçek Azure Redis Cache nasıl](cache-how-to-scale.md).

![Redis önbelleği fiyatlandırma katmanı](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis küme boyutu
Tıklayın **(Önizleme) Redis küme boyutu** çalışan bir küme boyutunu değiştirmek için kümeleme özelliği etkinleştirilmiş olan premium önbellek.

> [!NOTE]
> Azure Redis Cache Premium katmanı genel kullanıma sunulmuş olsa da Redis küme boyutu özelliği şu anda Önizleme aşamasında olduğunu unutmayın.
> 
> 

![Redis küme boyutu](./media/cache-configure/redis-cache-redis-cluster-size.png)

Küme boyutunu değiştirmek için kaydırıcıyı kullanın veya 1 ile 10 arasında bir sayı yazın **parça sayısı** metin kutusu ve tıklatın **Tamam** kaydetmek için.

> [!IMPORTANT]
> Redis kümeleme yalnızca Premium önbellekler için kullanılabilir. Daha fazla bilgi için bkz. [Premium Azure Redis Cache için kümeleri yapılandırma](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Redis veri kalıcılığı
Tıklayın **Redis veri kalıcılığı** etkinleştirmek, devre dışı bırakın veya premium önbelleğinizi veri kalıcılığını yapılandırma. Azure Redis Cache, Redis kalıcılığı kullanarak sunar [RDB Kalıcılık](cache-how-to-premium-persistence.md#configure-rdb-persistence) veya [AOF Kalıcılık](cache-how-to-premium-persistence.md#configure-aof-persistence).

Daha fazla bilgi için [Premium Azure Redis Cache için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Redis veri kalıcılığı, yalnızca Premium önbellekler için kullanılabilir. 
> 
> 

### <a name="schedule-updates"></a>Güncelleştirmeleri zamanlama
**Güncelleştirmeleri zamanla** dikey penceresinde, bir bakım penceresi için önbelleğinizi Redis sunucu güncelleştirmeleri belirlemek olanak sağlar. 

> [!IMPORTANT]
> Bakım penceresi server güncelleştirmeleri yalnızca Redis için geçerlidir ve değil herhangi bir Azure güncelleştirmeleri veya işletim sistemine sanal makinelerinin ana bilgisayar önbelleği güncelleştirir.
> 
> 

![Güncelleştirmeleri zamanlama](./media/cache-configure/redis-schedule-updates.png)

Bir bakım penceresi belirtmek için istenen gün işaretleyin ve her gün için bakım penceresi başlangıç saati belirleyin ve tıklayın **Tamam**. Bakım penceresi saati UTC biçiminde olduğunu unutmayın. 

> [!IMPORTANT]
> **Güncelleştirmeleri zamanla** işlevselliği, yalnızca Premium katman önbelleklerinden için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz. [Azure Redis Cache yönetim - güncelleştirmeleri zamanla](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Coğrafi çoğaltma

**Coğrafi çoğaltma** dikey bağlama iki Premium katmanı, Azure Redis Cache örnekleri için bir mekanizma sağlar. Bir önbelleği birincil bağlı önbellek ve diğer bağlı ikincil önbellek olarak atanır. Bağlı ikincil önbellek salt okunur hale gelir ve birincil önbelleğe yazılan veri ikincil bağlı önbellek için çoğaltılır. Bu işlev, Azure bölgeleri arasında bir önbellek çoğaltmak için kullanılabilir.

> [!IMPORTANT]
> **Coğrafi çoğaltma** yalnızca Premium katman önbelleklerinden için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz. [Azure Redis Cache için coğrafi çoğaltmayı yapılandırma](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Sanal Ağ
**Sanal ağ** bölümü önbelleğiniz için sanal ağ ayarlarını yapılandırmanıza olanak sağlar. Premium önbellek ile VNET oluşturma hakkında bilgi için destek ve ayarlarını güncelleştirmek, bkz [Premium Azure Redis Cache için sanal ağ desteğini yapılandırma](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Sanal ağ ayarlarını bulunan ve yalnızca yapılandırılmış olan premium önbellekler için önbellek oluşturma sırasında sanal ağ desteği. 
> 
> 

### <a name="firewall"></a>Güvenlik duvarı

Güvenlik duvarı kuralları yapılandırma, tüm Azure Redis Cache katmanları için kullanılabilir.

Tıklayın **Güvenlik Duvarı** görüntülemek ve önbellek için güvenlik duvarı kurallarını yapılandırmak için.

![Güvenlik duvarı](./media/cache-configure/redis-firewall-rules.png)

Güvenlik duvarı kuralları ile bir başlangıç ve bitiş IP adresi aralığı belirtebilirsiniz. Güvenlik duvarı kuralları yapılandırıldığında, yalnızca belirtilen IP adresi aralıklarını'ten istemci bağlantılarını önbelleğine bağlanabilirsiniz. Bir güvenlik duvarı kuralı kaydedildi olduğunda kısa bir gecikme önce etkin bir kuraldır. Bu gecikme, genellikle bir dakikadan az olur.

> [!IMPORTANT]
> Güvenlik duvarı kuralları yapılandırılmış olsa bile Azure Redis Cache izleme sistemlerinden gelen bağlantıları her zaman verilir.
> 
> 

### <a name="properties"></a>Özellikler
Tıklayın **özellikleri** önbellek uç noktası ve bağlantı noktalarını önbelleğinizi hakkındaki bilgileri görüntülemek için.

![Redis önbelleği özellikleri](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Kilitler
**Kilitler** bölümü bir aboneliğe, kaynak grubuna ya da kaynak yanlışlıkla kritik kaynakları silmesini veya kuruluşunuzdaki diğer kullanıcıların önlemek için kilitlemenizi sağlar. Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Otomasyon betiği

Tıklayın **Otomasyon betiği** oluşturup dağıtılmış kaynaklarınızın gelecekteki dağıtımlar için bir şablonu dışarı aktarma. Şablonlar ile çalışma hakkında daha fazla bilgi için bkz. [kaynakları Azure Resource Manager şablonları ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Yönetim ayarları
Ayarlarında **Yönetim** bölümü aşağıdaki yönetim görevlerini gerçekleştirmek için önbelleğinizi izin verir. 

![Yönetim](./media/cache-configure/redis-cache-administration.png)

* [Veri içeri aktarma](#importexport)
* [Verileri dışarı aktarma](#importexport)
* [Yeniden başlatma](#reboot)


### <a name="importexport"></a>İçeri/Dışarı Aktarma
İçeri/dışarı aktarma, içeri aktarma ve içeri aktarma ve bir Redis önbelleği veritabanı'nı (RDB) anlık görüntü için bir Azure depolama hesabındaki bir sayfa blobu premium önbellekten dışarı aktarma önbellekteki verilerin dışarı olanak tanıyan bir Azure Redis Cache veri yönetimi işlemi, ' dir. İçeri/dışarı aktarma, farklı Azure Redis Cache örnekleri arasında geçirmek veya önbellek kullanılmadan önce veri ile doldurmak sağlar.

İçeri aktarma, herhangi bir bulut veya ortam, Linux, Windows ya da herhangi bir bulut sağlayıcısı Amazon Web Hizmetleri ve diğerleri gibi çalışan bir Redis dahil olmak üzere çalışan herhangi bir Redis sunucudan Redis uyumlu RDB dosyaları getirmek için kullanılabilir. Verileri içeri aktarma ile önceden doldurulmuş veri önbellek oluşturmak için kolay bir yoludur. İçeri aktarma işlemi sırasında Azure Redis Cache RDB dosyaları Azure Storage'dan belleğine yükler ve ardından anahtarları önbelleğe ekler.

Dışarı aktarma, Azure Redis için uyumlu RDB dosyaları Redis önbelleğinde depolanan verileri vermenize olanak sağlar. Verileri bir Azure Redis Cache örneğinden diğerine veya başka bir Redis sunucusuna taşımak için bu özelliği kullanabilirsiniz. Dışarı aktarma işlemi sırasında Azure Redis Cache sunucusu örneğini barındıran sanal makine geçici bir dosya oluşturulur ve dosyanın belirtilen depolama hesabına yüklenir. Ya da bir durum başarı veya hata ile dışarı aktarma işlemi tamamlandığında, geçici dosya silinir.

> [!IMPORTANT]
> İçeri/dışarı aktarma, yalnızca Premium katman önbelleklerinden için kullanılabilir. Daha fazla bilgi ve yönergeler için bkz. [içeri ve dışarı aktarma, Azure Redis Cache'te](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Yeniden başlatma
**Yeniden** dikey önbelleğinizin düğümlerini yeniden başlatmanız olanak tanır. Bu yeniden başlatma özelliği, bir önbellek düğümü bir hata varsa, dayanıklılık için uygulamanızı test olanak tanır.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot.png)

Kümeleme özelliği etkinleştirilmiş bir premium önbellek varsa, hangi parçalar önbelleğin yeniden başlatmayı seçebilirsiniz.

![Yeniden başlatma](./media/cache-configure/redis-cache-reboot-cluster.png)

Önbelleğinizin bir veya daha fazla düğümlerini yeniden başlatmanız, istediğiniz düğümleri seçin ve tıklayın **yeniden**. Kümeleme özelliği etkinleştirilmiş bir premium önbellek varsa, yeniden başlatın ve ardından başlatılacak parçalar seçin **yeniden**. Birkaç dakika sonra seçili düğümü yeniden başlatma ve bu birkaç dakika sonra yeniden çevrimiçi.

> [!IMPORTANT]
> Yeniden başlatma için tüm fiyatlandırma katmanında kullanıma sunulmuştur. Daha fazla bilgi ve yönergeler için bkz. [Azure Redis Cache yönetim - yeniden başlatma](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>İzleme

**İzleme** bölümü, tanılama ve Redis Cache için izlemeyi yapılandırmanıza olanak sağlar. Azure Redis Cache izleme ve Tanılama hakkında daha fazla bilgi için bkz. [Azure Redis önbelleğini izleme](cache-how-to-monitor.md).

![Tanılama](./media/cache-configure/redis-cache-diagnostics.png)

* [Redis ölçümleri](#redis-metrics)
* [Uyarı kuralları](#alert-rules)
* [Tanılama](#diagnostics)

### <a name="redis-metrics"></a>Redis ölçümleri
Tıklayın **Redis ölçümleri** için [ölçümleri görüntülemek](cache-how-to-monitor.md#view-cache-metrics) önbelleğiniz.

### <a name="alert-rules"></a>Uyarı kuralları

Tıklayın **uyarı kuralları** Redis Cache ölçümlerine bağlı uyarılar yapılandırmak için. Daha fazla bilgi için [uyarılar](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Tanılama

Varsayılan olarak, Azure İzleyici'de önbellek ölçümleridir [30 gün saklanan](../azure-monitor/platform/data-collection.md#metrics) ve ardından silinir. Kalıcı önbellek ölçümlerinizi 30 günden daha uzun bir süre için tıklayın **tanılama** için [depolama hesabı yapılandırma](cache-how-to-monitor.md#export-cache-metrics) önbellek tanılama verilerini depolamak için kullanılır.

>[!NOTE]
>Depolama için önbellek ölçümlerinizi arşivleme yanı sıra, ayrıca [bunları bir olay hub'ına akış veya bunları Log Analytics'e gönderme](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).
>
>

## <a name="support--troubleshooting-settings"></a>Destek ve sorun giderme ayarları
Ayarlarında **destek + sorun giderme** bölüm önbelleğinizi ile ilgili sorunları çözmek için seçenekler ile size sağlar.

![Destek ve sorun giderme](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Kaynak durumu](#resource-health)
* [Yeni destek isteği](#new-support-request)

### <a name="resource-health"></a>Kaynak durumu
**Kaynak durumu** , kaynağınızı izler ve beklendiği gibi çalışıp çalışmadığını bildirir. Azure kaynak sistem durumu hizmeti hakkında daha fazla bilgi için bkz: [Azure kaynak durumu genel bakış](../resource-health/resource-health-overview.md).

> [!NOTE]
> Kaynak durumu, bir sanal ağda barındırılan Azure Redis Cache örneğinin durumunu raporlamak şu anda alamıyor. Daha fazla bilgi için [tüm önbellek özellikleri, bir sanal ağ içindeki bir önbellek barındırırken çalışmıyor?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Yeni destek isteği
Tıklayın **yeni destek isteği** önbellek hesabınız için bir destek isteği açın.





## <a name="default-redis-server-configuration"></a>Varsayılan Redis sunucu yapılandırması
Yeni Azure Redis Cache örnekleri aşağıdaki varsayılan Redis yapılandırma değerleri ile yapılandırılır:

> [!NOTE]
> Bu bölümde ayarları kullanılarak değiştirilemez. `StackExchange.Redis.IServer.ConfigSet` yöntemi. Bu yöntem, bu bölümdeki komutları biriyle çağrılırsa, aşağıdaki örneğe benzer bir özel durum:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Yapılandırılabilir gibi herhangi bir değeri **en fazla bellek ilke**, Azure portal veya Azure CLI veya PowerShell gibi komut satırı yönetim araçlarını aracılığıyla yapılandırılabilir.
> 
> 

| Ayar | Varsayılan değer | Açıklama |
| --- | --- | --- |
| `databases` |16 |Veritabanı varsayılan sayısı 16'dır ancak fiyatlandırma katmanını temel alan farklı bir numara yapılandırabilirsiniz. <sup>1</sup> varsayılan veritabanı DB 0, farklı bir bağlantı başına kullanma temel seçebileceğiniz `connection.GetDatabase(dbid)` burada `dbid` arasında bir sayı `0` ve `databases - 1`. |
| `maxclients` |Fiyatlandırma katmanına bağlı<sup>2</sup> |Bu değer bağlı istemciler aynı anda izin verilen en büyük sayısıdır. Sınıra ulaşıldığında Redis 'maksimum istemci sayısı üst sınırına' bir hata döndürüyor tüm yeni bağlantıları kapatır. |
| `maxmemory-policy` |`volatile-lru` |Redis ne zaman kaldırmak nasıl seçtiği için Maxmemory İlkesi ayarı olan `maxmemory` (önbellek oluştururken seçtiğiniz sunan önbellek boyutu) ulaşıldı. Azure Redis Cache ile birlikte varsayılan ayardır `volatile-lru`, LRU algoritması kullanılarak ayarlanan bir süre sonu anahtarları kaldırır. Bu ayar, Azure portalında yapılandırılabilir. Daha fazla bilgi için [bellek ilkeleri](#memory-policies). |
| `maxmemory-samples` |3 |Bellek kaydetmek için kesin bir algoritma yerine yaklaşık algoritmaları LRU ve TTL algoritmaları en az olan. Varsayılan olarak üç anahtar denetimleri ve çekme daha kısa bir süre önce kullanıldı bir Redis. |
| `lua-time-limit` |5.000 |En fazla yürütme zamanı Lua komut dosyası milisaniye cinsinden. En fazla yürütme zamanı ulaşılırsa, Redis bir kod yürütülmesine izin verilen en fazla süre geçtikten sonra hala bulunduğu ve bir hata ile sorguları yanıtlamak başlatır günlüğe kaydeder. |
| `lua-event-limit` |500 |Komut dosyası olay sırası en büyük boyutu. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |İstemci çıkış arabelleği sınırları veri yeterince hızlı sunucudan (Pub/Sub istemci iletileri yayımcı üretmek elimizden geldiğince hızlı kullanamıyor yaygın bir nedeni olduğundan) herhangi bir nedenden dolayı okuyorsanız olmayan istemciler bağlantısının kesilmesi zorlamak için kullanılabilir. Daha fazla bilgi için bkz. [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>sınırı `databases` her Azure Redis fiyatlandırma katmanı önbelleği için farklıdır ve önbellek oluşturma sırasında ayarlanabilir. Hayır ise `databases` ayarı belirtilen önbellek oluşturma işlemi sırasında varsayılan değer 16.

* Temel ve standart önbellekler
  * C0 (250 MB) önbellek - 16 veritabanları kadar
  * C1 (1 GB) önbellek - 16 veritabanları kadar
  * C2 (2,5 GB) önbellek - 16 veritabanları kadar
  * C3 (6 GB) önbellek - 16 veritabanları kadar
  * C4 (13 GB) önbellek - 32 veritabanları kadar
  * C5 (26 GB) önbellek - 48 veritabanları kadar
  * C6 (53 GB) önbellek - 64 veritabanları kadar
* Premium önbellekler
  * P1 (6 GB - 60 GB) - 16 veritabanları kadar
  * Ö2 (13 GB 130 GB) - 32 veritabanları kadar
  * P3 (26 GB - 260 GB) - 48 veritabanları kadar
  * P4 (53 GB 530 GB) - 64 veritabanları kadar
  * Redis kümesiyle etkin - tüm premium önbellekler Redis kümesi yalnızca 0 veritabanı kullanımını destekler. böylece `databases` etkin Redis kümesiyle tüm premium önbelleği etkili bir şekilde 1'dir sınırlandırmak ve [seçin](http://redis.io/commands/select) komut izin verilmiyor. Daha fazla bilgi için [kümeleme kullanmak üzere istemci uygulamamın herhangi bir değişiklik yapmanız gerekiyor mu?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Veritabanları hakkında daha fazla bilgi için bkz. [Redis veritabanı nedir?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> `databases` Yapılandırılmış önbellek oluşturma sırasında yalnızca ve yalnızca PowerShell, CLI veya diğer yönetim istemcilerini kullanarak ayarı olabilir. Yapılandırma örneği için `databases` PowerShell kullanarak önbellek oluşturma sırasında bkz [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` her Azure Redis fiyatlandırma katmanı önbelleği için farklıdır.

* Temel ve standart önbellekler
  * C0 (250 MB) önbellek - 256 bağlantıları kadar
  * C1 (1 GB) önbellek - 1.000 bağlantı kadar
  * C2 (2,5 GB) önbellek - 2.000 bağlantıları kadar
  * C3 (6 GB) önbellek - 5.000 bağlantı kadar
  * C4 (13 GB) önbellek - 10.000 bağlantı kadar
  * C5 (26 GB) önbellek - 15.000 bağlantıları kadar
  * C6 (53 GB) önbellek - 20.000 bağlantıları kadar
* Premium önbellekler
  * P1 (6 GB - 60 GB) - 7.500 bağlantıları kadar
  * 15.000 bağlantıları kadar ö2 (13 GB 130 GB)-
  * 30.000 bağlantı kadar (26 GB - 260 GB) - P3
  * 40.000 bağlantıları kadar P4 (53 GB 530 GB)-

> [!NOTE]
> Her önbellek boyutu sağlar, ancak *kadar* belirli sayıda bağlantıları, her bağlantı için Redis ek yükü ile ilişkili. TLS/SSL şifreleme sonucunda CPU ve bellek kullanımı gibi ek yükü örneği olacaktır. Belirtilen önbellek boyutu için en yüksek bağlantı sınırı hafifçe yüklü bir önbellek varsayar. Yük bağlantı ek yükünden *artı* istemci işlemleri yükü sistem kapasiteyi aşıyor, geçerli önbellek boyutu için bağlantı sınırını aştınız değil olsa bile önbelleğin kapasite sorunları oluşabilir.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis komutları Azure Redis Cache'te desteklenmiyor
> [!IMPORTANT]
> Yapılandırma ve yönetim Azure Redis Cache örnekleri yönetilir çünkü Microsoft tarafından aşağıdaki komutları devre dışı bırakıldı. Bunları deneyin, benzer bir hata iletisi alırsınız `"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * YAPILANDIRMA
> * HATA AYIKLAMA
> * GEÇİRME
> * KAYDET
> * KAPATMA
> * SLAVEOF
> * Küme - yazma komutları devre dışı bırakıldı, ancak salt okunur Küme komutları izin verilir.
> 
> 

Redis komutları hakkında daha fazla bilgi için bkz. [ http://redis.io/commands ](http://redis.io/commands).

## <a name="redis-console"></a>Redis Konsolu
Komutları kullanarak, Azure Redis Cache örnekleri için güvenli bir şekilde verebilir **Redis Konsolu**, kullanılabildiği tüm önbellek katmanları için Azure Portalı'nda.

> [!IMPORTANT]
> - Redis konsolu ile çalışmıyor [VNET](cache-how-to-premium-vnet.md). Önbelleğinizi bir sanal ağın parçası olduğunda, yalnızca sanal ağ istemcilerinde önbelleğe erişebilir. Redis Konsolu sanal ağ dışında olan yerel tarayıcınızda çalıştığından önbelleğinize bağlantı kurulamıyor.
> - Tüm Redis komutları, Azure Redis Cache'te desteklenir. Azure Redis Cache için devre dışı bırakılmış Redis komutları listesi için bkz. önceki [Redis komutları Azure Redis Cache'te desteklenmeyen](#redis-commands-not-supported-in-azure-redis-cache) bölümü. Redis komutları hakkında daha fazla bilgi için bkz. [ http://redis.io/commands ](http://redis.io/commands).
> 
> 

Redis konsoluna erişmek için tıklayın **konsol** gelen **Redis Cache** dikey penceresi.

![Redis Konsolu](./media/cache-configure/redis-console-menu.png)

Cache örneğinizin komutları göndermek için istenen komut konsolunda yazın.

![Redis Konsolu](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a>Redis konsolu ile bir kümelenmiş premium önbellek kullanma

Önbellek kümelenmiş bir premium Redis konsolunu kullanarak, önbelleğin tek bir parçanın komutları da verebilir. Belirli bir parça için bir komut göndermek için ilk parça seçici üzerinde tıklayarak istenen parçaya bağlanın.

![Redis Konsolu](./media/cache-configure/redis-console-premium-cluster.png)

Bağlanılan parça değerinden farklı bir parçada depolanan bir anahtar erişmeyi denerseniz, şu iletiye benzer bir hata iletisi alırsınız:

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

Önceki örnekte, seçilen parça parça 1 olan ancak `myKey` tarafından belirtildiği şekilde 0, bir parçada bulunan `(shard 0)` hata iletisi bölümü. Erişmek için bu örnekte, `myKey`, parça parça seçiciyi kullanarak 0'ı seçin ve ardından istenen komutu yürütün.


## <a name="move-your-cache-to-a-new-subscription"></a>Önbelleğinizi yeni bir aboneliğe taşıma
Tıklayarak yeni bir abonelik için önbelleğinizi taşıyabilirsiniz **taşıma**.

![Redis önbelleği taşıma](./media/cache-configure/redis-cache-move.png)

Kaynakları bir kaynak grubundan diğerine ve bir abonelikten diğerine taşıma hakkında daha fazla bilgi için bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Sonraki adımlar
* Redis komutları ile çalışma hakkında daha fazla bilgi için bkz. [Redis komutları nasıl çalıştırırım?](cache-faq.md#how-can-i-run-redis-commands)

