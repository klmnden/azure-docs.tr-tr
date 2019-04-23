---
title: Azure abonelik limitleri ve kotaları
description: Yaygın Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar listesini sağlar. Bu makalede, en yüksek değerleri birlikte sınırları artırmak nasıl hakkındaki bilgileri içerir.
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 04/19/2019
ms.author: byvinyal
ms.openlocfilehash: b09de67cddcec26a1083bb64d13b9bbc47c3d5e5
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59998490"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar
Bu belge Ayrıca bazen kota olarak da adlandırılır en yaygın Microsoft Azure sınırları bazıları listelenmiştir. Bu belge, şu anda tüm Azure Hizmetleri ele alınmamıştır. Zamanla, listenin genişletilir ve diğer hizmetler kapsayacak şekilde güncelleştirildi.

Azure fiyatlandırması hakkında daha fazla bilgi için bkz: [Azure fiyatlandırmaya genel bakış](https://azure.microsoft.com/pricing/). Burada, kullanarak maliyetlerinizi tahmin edebilirsiniz [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/). Ayrıca belirli bir hizmet için fiyatlandırma ayrıntıları sayfasına gibi gidebilirsiniz [Windows Vm'leri](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). Maliyetlerinizi yönetmenize yardımcı olmak ipuçları için bkz: [Azure'da faturalandırma ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](billing/billing-getting-started.md).

> [!NOTE]
> Yukarıda belirtilen varsayılan sınırı, kotası ve sınırı artırmak istiyorsanız [ücretsiz bir çevrimiçi müşteri destek isteği açın](azure-resource-manager/resource-manager-quota-errors.md). Aşağıdaki tablolarda gösterilen üst sınırı değerini yukarıda sınırları yükseltilemez. Üst sınır sütun yok ise, kaynak sınırları ayarlanabilir sahip değil.
>
> [Ücretsiz deneme abonelikleri](https://azure.microsoft.com/offers/ms-azr-0044p) sınırı veya kota artırma işlemleri için uygun değildir. Varsa bir [ücretsiz deneme aboneliği](https://azure.microsoft.com/offers/ms-azr-0044p), Yükseltme yapabileceğiniz bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik. Daha fazla bilgi için [Azure ücretsiz deneme aboneliğinizi Kullandıkça Öde aboneliğine yükseltin](billing/billing-upgrade-azure-subscription.md) ve [ücretsiz deneme aboneliği SSS](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-azure-resource-manager"></a>Sınırları ve Azure Resource Manager
Artık, tek bir Azure kaynak grubu içinde birden çok Azure kaynaklarını birleştirmek mümkündür. Kaynak gruplarını kullandığınızda, kez genel sınırları bölge düzeyinde Azure Resource Manager ile yönetilen olur. Azure kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md).

Azure Resource Manager'ı kullandığınızda sınırları aşağıdaki listede yeni bir tablo farkları sınırları yansıtır. Örneğin, bir **abonelik limitleri** tablo ve **abonelik limitleri - Azure Resource Manager** tablo. Her iki senaryo için bir sınır uygulanır, yalnızca ilk tabloda gösterilir. Aksi belirtilmediği sürece, tüm bölgelerde genel limitlerdir.

> [!NOTE]
> Azure kaynak gruplarındaki kaynaklar için kotalar bölge başına abonelik başına değil, abonelik tarafından erişilebilir olduğundan hizmet yönetimi kotalar. VCPU kotaları örnek olarak kullanalım. Vcpu desteğiyle bir kota artırım talebinde bulunmak hangi bölgelerde kullanmak istediğiniz kaç Vcpu karar vermeniz gerekir. Tutarları ve istediğiniz bölgeleri için daha sonra Azure kaynak grubu vCPU kotaları belirli bir istekte bulunmak. Uygulamanız var. çalıştırmayı Batı Avrupa'daki 30 Vcpu kullanmanız gerekiyorsa, özellikle Batı Avrupa'daki 30 Vcpu isteyin. Yalnızca Batı Avrupa 30 vCPU kotası, vCPU kotası herhangi diğer bölgesinde--artırılmış değil.
> <!-- -->
> Sonuç olarak, Azure kaynak grubu kotanızı herhangi bir bölgede iş yükünüz için olmalıdır karar verin. Ardından, dağıtmak istediğiniz her bölgede miktarındaki bu isteyin. Özel bölgeler için geçerli kotanızı belirlemek nasıl daha fazla yardım için bkz [dağıtım sorunlarını giderme](resource-manager-common-deployment-errors.md).
>
>

## <a name="service-specific-limits"></a>Hizmete özgü sınırları
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Otomasyon](#automation-limits)
* [Redis için Azure Önbelleği](#azure-cache-for-redis-limits)
* [Azure Cloud Services](#azure-cloud-services-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [MySQL için Azure Veritabanı](#azure-database-for-mysql)
* [PostgreSQL için Azure Veritabanı](#azure-database-for-postgresql)
* [Azure DNS](#azure-dns-limits)
* [Azure güvenlik duvarı](#azure-firewall-limits)
* [Azure Kubernetes Service](#azure-kubernetes-service-limits)
* [Azure Haritalar](#azure-maps-limits)
* [Azure İzleyici](#monitor-limits)
* [Azure İlkesi](#azure-policy-limits)
* [Azure Search](#azure-search-limits)
* [Azure SignalR hizmeti](#azure-signalr-service-limits)
* [Backup](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Hizmetleri](#biztalk-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [İçerik teslim ağı](#content-delivery-network-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Veritabanı geçiş hizmeti](#database-migration-service-limits)
* [Event Grid](#event-grid-limits)
* [Event Hubs](#event-hubs-limits)
* [Ön kapısı hizmeti](#azure-front-door-service-limits)
* [Identity Manager](#identity-manager-limits)
* [IoT Hub’ı](#iot-hub-limits)
* [IoT Hub Cihazı Sağlama Hizmeti](#iot-hub-device-provisioning-service-limits)
* [Anahtar Kasası](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobil hizmetler](#mobile-services-limits)
* [Multi-Factor Authentication](#multi-factor-authentication-limits)
* [Ağ](#networking-limits)
* [Ağ İzleyicisi](#network-watcher-limits)
* [Notification Hubs](#notification-hubs-limits)
* [Kaynak grubu](#resource-group-limits)
* [Rol tabanlı erişim denetimi](#role-based-access-control-limits)
* [Scheduler](#scheduler-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Veritabanı](#sql-database-limits)
* [SQL Veri Ambarı](#sql-data-warehouse-limits)
* [Depolama](#storage-limits)
* [StorSimple sistemi](#storsimple-system-limits)
* [Akış Analizi](#stream-analytics-limits)
* [Abonelik](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Sanal Makineler](#virtual-machines-limits)
* [Sanal makine ölçek kümeleri](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Abonelik sınırları
#### <a name="subscription-limits---azure-service-management-classic-deployment-model"></a>Abonelik limitleri - Azure Hizmet Yönetimi (Klasik dağıtım modeli)
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonelik limitleri - Azure Resource Manager
Azure Resource Manager ve Azure kaynak grupları kullandığınızda aşağıdaki sınırlar geçerlidir. Azure Resource Manager ile değiştirilmemiş sınırları listelenmiyor. Bu sınırları için önceki tabloya bakın.

Resource Manager API'si hakkında bilgi için okuma ve yazma sınırları, bkz: [istekleri azaltma Resource Manager](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Kaynak grubu sınırları
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Sanal makine limitleri
#### <a name="virtual-machines-limits"></a>Sanal makine limitleri
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Sanal makine limitleri - Azure Resource Manager
Azure Resource Manager ve Azure kaynak grupları kullandığınızda aşağıdaki sınırlar geçerlidir. Azure Resource Manager ile değiştirilmemiş sınırları listelenmiyor. Bu sınırları için önceki tabloya bakın.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Sanal makine ölçek kümeleri sınırları
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Kapsayıcı örnekleri sınırları
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Kapsayıcı kayıt defteri sınırları
Aşağıdaki tabloda temel, standart ve Premium sınırlamaları ve özellikleri ayrıntıları [hizmet katmanları](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="azure-kubernetes-service-limits"></a>Azure Kubernetes hizmeti limitleri
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Ağ limitleri
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Ağ limitleri
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Uygulama ağ geçidi sınırlar

Aşağıdaki tabloda, aksi belirtilmedikçe v1, v2, standart ve WAF SKU'ları için geçerlidir.
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Ağ İzleyicisi sınırları
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager sınırları
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="azure-dns-limits"></a>Azure DNS sınırlamaları
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Azure güvenlik duvarı sınırları
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

#### <a name="azure-front-door-service-limits"></a>Azure ön kapısı hizmet sınırları
[!INCLUDE [azure-front-door-service-limits](../includes/front-door-limits.md)]

### <a name="storage-limits"></a>Depolama sınırları
<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

Depolama hesabı sınırları hakkında daha fazla bilgi için bkz. [Azure depolama ölçeklenebilirlik ve performans hedefleri](storage/common/storage-scalability-targets.md).

#### <a name="storage-resource-provider-limits"></a>Depolama kaynak sağlayıcısı sınırları 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob Depolama sınırları
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure dosyaları sınırları
Azure dosyaları sınırları hakkında daha fazla bilgi için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage/files/storage-files-scale-targets.md).

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

Daha fazla bilgi için [sanal makine boyutları](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

#### <a name="managed-virtual-machine-disks"></a>Yönetilen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Yönetilmeyen sanal makine diskleri

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="azure-cloud-services-limits"></a>Azure Cloud Services sınırları
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>App Service limitleri
Aşağıdaki App Service limitleri, Web Apps, Mobile Apps ve API Apps sınırları içerir.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler sınırları
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch sınırları
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk Hizmetleri sınırları
Aşağıdaki tabloda, Azure BizTalk Hizmetleri için sınırlar gösterilmektedir.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB sınırları
Azure Cosmos DB, aktarım hızını ve depolamayı ne olursa olsun uygulamanızın gerektirdiği işlemek için ölçeklendirilebilir bir küresel ölçek veritabanıdır. Azure Cosmos DB sağlar ölçek hakkında sorularınız varsa, e-posta Gönder askcosmosdb@microsoft.com.

### <a name="azure-database-for-mysql"></a>MySQL için Azure Veritabanı
Sınırları MySQL için Azure veritabanı için bkz: [MySQL için Azure veritabanı'nda sınırlamaları](mysql/concepts-limits.md).

### <a name="azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı
Sınırları PostgreSQL için Azure veritabanı için bkz: [PostgreSQL için Azure veritabanı'nda sınırlamaları](postgresql/concepts-limits.md).

### <a name="azure-search-limits"></a>Azure arama sınırları
Fiyatlandırma katmanı, kapasitesi ve sınırları, arama hizmetinizin belirleyin. Katmanlar şunlardır:

* **Ücretsiz** Azure diğer abonelerle paylaşılan çok kiracılı bir hizmet, değerlendirme ve küçük geliştirme projeleri için tasarlanmıştır.
* **Temel** daha küçük bir ölçekte, yüksek oranda kullanılabilir sorgu iş yükleri için en fazla üç yinelemelerle üretim iş yükleri için adanmış işlem kaynakları sağlar.
* **Standart**, S1, S2, S3, içerir ve S3 yüksek yoğunluklu olan daha büyük üretim iş yükleri için. İş yükü profilinizi en iyi şekilde eşleşen bir kaynak yapılandırması seçebilmeniz standart katmanında birden çok düzeyi yok.

**Abonelik başına limitler**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Arama hizmeti başına limitler**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Belge boyutuna, sorgu başına saniye, anahtarları, istekleri ve yanıtları gibi daha ayrıntılı bir düzeyde sınırları hakkında daha fazla bilgi için bkz: [Azure Search'te hizmet sınırları](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services sınırları
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="content-delivery-network-limits"></a>Content Delivery Network sınırları
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobil hizmetler sınırları
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>İzleyici sınırları
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hubs-limits"></a>Bildirim hub'ları sınırları
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

### <a name="event-grid-limits"></a>Event Grid sınırları
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

### <a name="azure-signalr-service-limits"></a>Azure SignalR hizmeti limitleri
[!INCLUDE [signalr-service-limits](../includes/signalr-service-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery limitleri
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights sınırları
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management sınırları
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-cache-for-redis-limits"></a>Azure önbelleği için Redis sınırları
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Key Vault sınırları
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication-limits"></a>Çok faktörlü kimlik doğrulaması sınırları
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Otomasyon sınırları
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="identity-manager-limits"></a>Identity Manager sınırları
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>Rol tabanlı erişim denetimi sınırları
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>SQL veritabanı sınırları
SQL veritabanı limitleri için bkz. [tek veritabanları için SQL veritabanı kaynak limitleri](sql-database/sql-database-vcore-resource-limits-single-databases.md), [elastik havuzlara ve havuza alınmış veritabanları için SQL veritabanı kaynak limitleri](sql-database/sql-database-vcore-resource-limits-elastic-pools.md), ve [SQL veritabanı kaynak limitleri Yönetilen örnek için](sql-database/sql-database-managed-instance-resource-limits.md).

### <a name="sql-data-warehouse-limits"></a>SQL veri ambarı sınırları
SQL veri ambarı limitleri için bkz. [SQL veri ambarı kaynak sınırları](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Ayrıca bkz.
- [Azure sınırları ve arttıkça anlama](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
- [Azure için sanal makine ve bulut hizmeti boyutları](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure Cloud Services boyutları](cloud-services/cloud-services-sizes-specs.md)
