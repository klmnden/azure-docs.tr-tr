---
title: Azure abonelik limitleri ve kotaları
description: Yaygın Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar listesini sağlar. Bu, en yüksek değerleri birlikte sınırları artırmak nasıl hakkındaki bilgileri içerir.
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 08/16/2018
ms.author: byvinyal
ms.openlocfilehash: 6b6e713c0da11a3d2c8cfbf388b84940a4542e95
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42056564"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar
Bu belge Ayrıca bazen kota olarak da adlandırılır en yaygın Microsoft Azure sınırları bazıları listelenmiştir. Bu belge, şu anda tüm Azure Hizmetleri ele alınmamıştır. Zamanla, listenin genişletilir ve daha fazla platform kapsayacak şekilde güncelleştirildi.

Lütfen [Azure fiyatlandırma genel bakış](https://azure.microsoft.com/pricing/) Azure fiyatlandırması hakkında daha fazla bilgi edinmek için. Kullanarak maliyetlerinizi tahmin, [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) veya bir hizmeti için fiyatlandırma ayrıntıları sayfasına ziyaret edebilirsiniz (örneğin, [Windows Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Maliyetlerinizi yönetmenize yardımcı olmak ipuçları için bkz: [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing/billing-getting-started.md).

> [!NOTE]
> Kota yukarıdaki ve sınırı artırmak istiyorsanız **varsayılan sınırı**, [ücretsiz bir çevrimiçi müşteri destek isteği açın](azure-resource-manager/resource-manager-quota-errors.md). Yukarıda sınırları yükseltilemez **sınırı** aşağıdaki tablolarda gösterilen değer. Yoksa hiçbir **sınırı** sütun, ardından kaynak sınırları ayarlanabilir yok.
>
> [Ücretsiz deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınırı veya kota artırma işlemleri için uygun değildir. Varsa bir [ücretsiz deneme aboneliği](https://azure.microsoft.com/offers/ms-azr-0044p), Yükseltme yapabileceğiniz bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik. Daha fazla bilgi için [yükseltme Azure ücretsiz denemesini Kullandıkça Öde aboneliğine](billing/billing-upgrade-azure-subscription.md) ve [ücretsiz deneme aboneliği SSS](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-the-azure-resource-manager"></a>Limitler ve Azure Resource Manager
Artık, tek bir Azure kaynak grubu içinde birden çok Azure kaynaklarına birleştirmek mümkündür. Kaynak grupları kullanılırken kez genel sınırları Azure Resource Manager ile bölge düzeyinde yönetilir hale. Azure kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md).

Aşağıdaki sınırlar yeni bir tablo, Azure Resource Manager kullanırken farkları sınırları yansıtacak şekilde eklendi. Örneğin, bir **abonelik limitleri** tablo ve **abonelik limitleri - Azure Resource Manager** tablo. Her iki senaryo için bir sınır uygulanır, yalnızca ilk tabloda gösterilir. Aksi belirtilmediği sürece, tüm bölgelerde genel limitlerdir.

> [!NOTE]
> Azure kaynak gruplarındaki kaynaklar için kotalar bölge başına aboneliğiniz tarafından erişilebilir olan ve olmayan, abonelik Hizmet Yönetimi kotalar gibi vurgulamak önemlidir. VCPU kotaları örnek olarak kullanalım. Vcpu desteğiyle bir kota artırım talebinde bulunmak gerekiyorsa, hangi bölgelerde kullanın ve ardından belirli Azure kaynak grubu vCPU kotaları tutarları ve istediğiniz bölgeleri için istekte istediğiniz kaç Vcpu karar vermeniz gerekir. Bu nedenle, uygulamanız var. çalıştırmayı Batı Avrupa'daki 30 Vcpu kullanmanız gerekiyorsa, Batı Avrupa'daki 30 Vcpu özellikle istemeniz gerekir. Ancak başka bir bölgede artırmak bir vCPU kotası yoktur; yalnızca Batı Avrupa 30 vCPU kotası olacak.
> <!-- -->
> Sonuç olarak, Azure kaynak grubu kotanızı herhangi bir bölgede iş yükünüz için gerekenler karar vermeyle ilgili dikkate alınması gereken kullanışlı ve bu tutar, dağıtım dikkate her bölgede isteyin. Bkz: [dağıtım sorunlarını giderme](resource-manager-common-deployment-errors.md) geçerli kotanızı özel bölgeler için keşfetmek daha fazla yardım için.
>
>

## <a name="service-specific-limits"></a>Hizmete özgü sınırları
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Otomasyon](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [MySQL için Azure Veritabanı](#azure-database-for-mysql)
* [PostgreSQL için Azure Veritabanı](#azure-database-for-postgresql)
* [Azure Event Grid](#azure-event-grid-limits)
* [Azure Haritalar](#azure-maps-limits)
* [Azure İlkesi](#azure-policy-limits)
* [Azure Redis Önbelleği](#azure-redis-cache-limits)
* [Backup](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Hizmetleri](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Kubernetes hizmeti](#kubernetes-service-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Veritabanı geçiş hizmeti](#database-migration-service-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [Azure güvenlik duvarı](#azure-firewall-limits)
* [IoT Hub’ı](#iot-hub-limits)
* [IoT Hub Cihazı Sağlama Hizmeti](#iot-hub-device-provisioning-service-limits)
* [Anahtar Kasası](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Yönetilen kimlik](#managed-identity-limits)
* [Media Services](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Mobil hizmetler](#mobile-services-limits)
* [İzleme](#monitor-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Ağ](#networking-limits)
* [Ağ İzleyicisi](#network-watcher-limits)
* [Bildirim hub'ı hizmeti](#notification-hub-service-limits)
* [Kaynak Grubu](#resource-group-limits)
* [Rol tabanlı erişim denetimi](#role-based-access-control-limits)
* [Scheduler](#scheduler-limits)
* [Search](#search-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [SQL Veri Ambarı](#sql-data-warehouse-limits)
* [Depolama](#storage-limits)
* [StorSimple sistemi](#storsimple-system-limits)
* [Akış Analizi](#stream-analytics-limits)
* [Abonelik](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Sanal Makineler](#virtual-machines-limits)
* [Sanal Makine Ölçek Kümeleri](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Abonelik sınırları
#### <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonelik limitleri - Azure Resource Manager
Azure kaynak grupları ve Azure Resource Manager kullanırken aşağıdaki sınırlar geçerlidir. Aşağıda Azure Resource Manager ile değişmedi sınırları listelenmemiştir. Lütfen bu sınırları için önceki tabloya bakın.

Resource Manager istek sınırları işleme hakkında daha fazla bilgi için bkz: [istekleri azaltma Resource Manager](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Kaynak grubu sınırları
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Sanal makine limitleri
#### <a name="virtual-machine-limits"></a>Sanal makine limitleri
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Sanal makine limitleri - Azure Resource Manager
Azure kaynak grupları ve Azure Resource Manager kullanırken aşağıdaki sınırlar geçerlidir. Aşağıda Azure Resource Manager ile değişmedi sınırları listelenmemiştir. Lütfen bu sınırları için önceki tabloya bakın.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Sanal makine ölçek kümeleri sınırları
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Kapsayıcı örnekleri sınırları
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Kapsayıcı kayıt defteri sınırları
Aşağıdaki tabloda temel, standart ve Premium sınırlamaları ve özellikleri ayrıntıları [hizmet katmanları](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="kubernetes-service-limits"></a>Kubernetes hizmet sınırları
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Ağ limitleri
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Ağ limitleri
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Uygulama ağ geçidi sınırlar
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Ağ İzleyicisi sınırları
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager sınırları
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS sınırları
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Azure güvenlik duvarı sınırları
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

### <a name="storage-limits"></a>Depolama sınırları
Depolama hesabı sınırları hakkında ek bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage/common/storage-scalability-targets.md).

<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob Depolama sınırları
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure dosyaları sınırları
Azure dosyaları sınırları hakkında ek ayrıntılar için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage/files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Azure dosya eşitleme sınırları
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Azure kuyruk depolama sınırları
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Azure tablo depolama sınırları
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Sanal makine disk limitleri
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Bkz: [sanal makine boyutları](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın.

#### <a name="managed-virtual-machine-disks"></a>Yönetilen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Yönetilmeyen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="cloud-services-limits"></a>Bulut Hizmetleri sınırları
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>App Service limitleri
Aşağıdaki App Service limitleri, Web Apps, Mobile Apps, API Apps ve Logic Apps sınırları içerir.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler sınırları
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch sınırları
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk Hizmetleri sınırları
Aşağıdaki tabloda, Azure Biztalk Hizmetleri için sınırlar gösterilmektedir.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB sınırları
Azure Cosmos DB, aktarım hızını ve depolamayı ne olursa olsun uygulamanızın gerektirdiği işlemek için ölçeklendirilebilir bir küresel ölçek veritabanıdır. Azure Cosmos DB sağlar ölçek hakkında sorularınız varsa lütfen e-posta gönderin askcosmosdb@microsoft.com.

### <a name="azure-database-for-mysql"></a>MySQL için Azure Veritabanı
Sınırları MySQL için Azure veritabanı için bkz: [MySQL için Azure veritabanı'nda sınırlamaları](mysql/concepts-limits.md).

### <a name="azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı
Sınırları PostgreSQL için Azure veritabanı için bkz: [PostgreSQL için Azure veritabanı'nda sınırlamaları](postgresql/concepts-limits.md).

### <a name="mobile-engagement-limits"></a>Mobile Engagement sınırları
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Arama sınırları
Fiyatlandırma katmanı, kapasitesi ve sınırları, arama hizmetinizin belirleyin. Katmanlar şunlardır:

* *Ücretsiz* Azure diğer abonelerle paylaşılan çok kiracılı bir hizmet, değerlendirme ve küçük geliştirme projeleri yöneliktir.
* *Temel* daha küçük bir ölçekte, yüksek oranda kullanılabilir sorgu iş yükleri için en fazla üç yinelemelerle üretim iş yükleri için adanmış işlem kaynakları sağlar.
* *Standart (S1, S2, S3, S3 yüksek yoğunluklu)* olan daha büyük üretim iş yükleri için. İş yükü profilinizi en iyi şekilde eşleşen bir kaynak yapılandırması seçebilmeniz standart katmanında birden çok düzeyi yok.

**Abonelik başına limitler**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Arama hizmeti başına limitler**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Belge boyutuna, sorgu başına saniye, anahtarları, istekleri ve yanıtları gibi daha ayrıntılı bir düzeyde sınırları hakkında daha fazla bilgi için bkz: [Azure Search'te hizmet sınırları](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services sınırları
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN sınırları
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobil hizmetler sınırları
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>İzleyici sınırları
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Bildirim hub'ı hizmet sınırları
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Olay hub'ları sınırları
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus sınırları
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IOT hub'ı sınırları
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>IOT Hub cihazı sağlama hizmeti sınırları
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Veri Fabrikası sınırları
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics sınırları
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store sınırları
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Veritabanı geçiş hizmeti limitleri
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics sınırları
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory sınırları
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Azure Event Grid sınırları
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-maps-limits"></a>Azure haritalar sınırları
[!INCLUDE [maps-limits](../includes/maps-limits.md)]

### <a name="azure-policy-limits"></a>Azure İlkesi sınırları
[!INCLUDE [policy-limits](../includes/azure-policy-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple sistemi sınırları
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Log Analytics sınırları
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Yedekleme sınırları
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery limitleri
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights sınırları
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management sınırları
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis Cache sınırları
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Key Vault sınırları
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Otomasyon sınırları
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="managed-identity-limits"></a>Yönetilen kimlik sınırları
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>Rol tabanlı erişim denetimi sınırları
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>SQL veritabanı sınırları
SQL veritabanı limitleri için bkz. [tek veritabanları için SQL veritabanı kaynak limitleri](sql-database/sql-database-vcore-resource-limits-single-databases.md) ve [elastik havuzlara ve havuza alınmış veritabanları için SQL veritabanı kaynak limitleri](sql-database/sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="sql-data-warehouse-limits"></a>SQL veri ambarı sınırları
SQL veri ambarı limitleri için bkz. [SQL veri ambarı kaynak sınırları](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Ayrıca bkz.
[Azure sınırları ve arttıkça anlama](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Sanal makine ve bulut hizmeti boyutları için Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Cloud Services boyutları](cloud-services/cloud-services-sizes-specs.md)
