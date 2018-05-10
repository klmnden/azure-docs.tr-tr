---
title: Azure abonelik sınırlarını ve kotaları | Microsoft Docs
description: Ortak Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları listesini sağlar. Bu, en yüksek değerleri birlikte sınırları artırmak hakkında bilgi içerir.
services: ''
documentationcenter: ''
author: rothja
manager: jeffreyg
editor: ''
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: byvinyal
ms.openlocfilehash: 28b6f6c269dfb1550b4ed12af3366b483d402f86
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure aboneliği ve hizmet sınırları, kotalar ve kısıtlamalar
Bu belge kotaları bazen adlı en yaygın Microsoft Azure sınırları, bazıları listelenmiştir. Bu belge şu anda tüm Azure hizmetlerini kapsamaz. Zamanla, liste genişletilir ve daha fazla platform kapsayacak şekilde güncelleştirildi.

Lütfen şu adresi ziyaret [Azure fiyatlandırma genel bakış](https://azure.microsoft.com/pricing/) Azure fiyatlandırma hakkında daha fazla bilgi edinmek için. Burada, kullanarak maliyetlerinizi tahmin edebilirsiniz [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/) veya bir hizmet için fiyatlandırma ayrıntıları sayfasını ziyaret (örneğin, [Windows Vm'lerini](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Maliyetlerinizi yönetmenize yardımcı olmak ipuçları için bkz: [Azure faturalama ve maliyet yönetimi ile beklenmeyen maliyetleri önlemek](billing/billing-getting-started.md).

> [!NOTE]
> Sınırı veya kotası yukarıdaki yükseltmek istiyorsanız **varsayılan sınır**, [ücretsiz bir çevrimiçi müşteri destek isteği açma](azure-supportability/resource-manager-core-quotas-request.md). Yukarıdaki sınırları yükseltilemez **sınırı** aşağıdaki tabloda gösterilen değeri. Varsa hiçbir **sınırına** sütun sonra kaynak ayarlanabilir sınırlar yok.
>
> [Deneme abonelikleri serbest](https://azure.microsoft.com/offers/ms-azr-0044p) sınırı veya kotası artışları için uygun değilsiniz. Varsa bir [ücretsiz deneme aboneliği](https://azure.microsoft.com/offers/ms-azr-0044p), Yükseltme yapabileceğiniz bir [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/) abonelik. Daha fazla bilgi için bkz: [yükseltme Azure ücretsiz deneme sürümü için Kullandıkça Öde](billing/billing-upgrade-azure-subscription.md) ve [ücretsiz deneme aboneliği SSS](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-the-azure-resource-manager"></a>Sınırları ve Azure Resource Manager
Artık, tek bir Azure kaynak grubu içinde birden çok Azure kaynaklarında birleştirmek mümkündür. Kaynak grupları kullanırken, kez genel sınırları bölgesel düzeyinde Azure Resource Manager ile yönetilen haline gelir. Azure kaynak grupları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](azure-resource-manager/resource-group-overview.md).

Aşağıdaki sınırlar, Azure Kaynak Yöneticisi'ni kullanırken sınırları farklılıkları yansıtacak şekilde yeni bir tablo eklendi. Örneğin, bir **abonelik sınırları** tablo ve **abonelik sınırları - Azure Resource Manager** tablo. Bir sınır iki senaryoyu uyguladığında, yalnızca ilk tabloda gösterilir. Aksi belirtilmedikçe, tüm bölgeler arasında genel kısıtlamalardır.

> [!NOTE]
> Hizmet Yönetimi kotalar gibi Azure kaynak grupları, kaynaklar için kotalar başına bölge aboneliğinizi tarafından erişilebilir olan ve abonelik başına, olmayan vurgulamak önemlidir. Şimdi vCPU kotalarını örnek olarak kullanın. Vcpu'lar desteğiyle kota artışı isteği göndermek üzere ihtiyacınız varsa, hangi bölgelerde kullanın ve ardından belirli Azure kaynak grubu vCPU kotaları tutarlar ve istediğiniz bölgeler için istekte istediğiniz kaç Vcpu'lar karar vermeniz gerekir. Bu nedenle, uygulamanız var. çalıştırmak için 30 Vcpu'lar Batı Avrupa'da kullanmanız gerekiyorsa, Batı Avrupa'da 30 Vcpu'lar özellikle istemeniz gerekir. Ancak, diğer herhangi bir bölgede artırmak vCPU kota sahip olmaz – yalnızca Batı Avrupa 30 vCPU kotası olacak.
> <!-- -->
> Sonuç olarak, iş yükü herhangi bir bölgede olması için Azure kaynak grubu kotaları gerekenler karar dikkate kullanışlı ve bu miktar içine dağıtım dikkate her bölgede isteyin. Bkz: [dağıtım sorunlarını giderme](resource-manager-common-deployment-errors.md) özel bölgeler için geçerli kotalar keşfetme daha fazla yardım için.
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
* [Azure Event Grid](#azure-event-grid-limits)
* [Azure eşlemeleri](#azure-maps-limits)
* [Azure Redis Önbelleği](#azure-redis-cache-limits)
* [Backup](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk Hizmetleri](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Kubernetes hizmeti](#container-service-aks-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Veritabanı geçiş hizmeti](#database-migration-service-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [IoT Hub’ı](#iot-hub-limits)
* [IoT Hub Cihazı Sağlama Hizmeti](#iot-hub-device-provisioning-service-limits)
* [Anahtar Kasası](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Mobil hizmetler](#mobile-services-limits)
* [İzleme](#monitor-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Ağ](#networking-limits)
* [Ağ İzleyicisi](#network-watcher-limits)
* [Bildirim hub'ı hizmeti](#notification-hub-service-limits)
* [Kaynak Grubu](#resource-group-limits)
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

#### <a name="subscription-limits---azure-resource-manager"></a>Abonelik sınırları - Azure Resource Manager
Azure Resource Manager ve Azure kaynak gruplarını kullanırken aşağıdaki sınırları uygulayın. Azure Resource Manager ile değişmemiştir sınırları aşağıda listelenen değil. Lütfen bu sınırları için önceki tabloya bakın.

Resource Manager istekleri sınırları işleme hakkında daha fazla bilgi için bkz: [azaltma Resource Manager istekleri](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Kaynak grubu sınırları
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Sanal makineler sınırları
#### <a name="virtual-machine-limits"></a>Sanal makine sınırları
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Sanal makineler sınırları - Azure Resource Manager
Azure Resource Manager ve Azure kaynak gruplarını kullanırken aşağıdaki sınırları uygulayın. Azure Resource Manager ile değişmemiştir sınırları aşağıda listelenen değil. Lütfen bu sınırları için önceki tabloya bakın.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Sanal makine ölçek kümeleri sınırları
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Kapsayıcı örnekleri sınırları
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Kapsayıcı kayıt defteri sınırları
Aşağıdaki tablo özellikler ve temel, standart ve Premium sınırları ayrıntıları [hizmet katmanları](./container-registry/container-registry-skus.md).

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

#### <a name="traffic-manager-limits"></a>Trafik Yöneticisi sınırları
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS sınırları
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Depolama sınırları
Depolama hesabı sınırları hakkında daha fazla ayrıntı için bkz: [Azure Storage ölçeklenebilirlik ve performans hedefleri](storage/common/storage-scalability-targets.md).

<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob Depolama sınırları
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure dosyaları sınırları
Azure dosyaları sınırları hakkında daha fazla ayrıntı için bkz: [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](storage/files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Azure dosya eşitleme sınırları
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Azure kuyruk depolama sınırları
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Azure tablo depolama sınırları
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Sanal makine disk sınırları
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Bkz: [sanal makine boyutlarını](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ek ayrıntılar için.

#### <a name="managed-virtual-machine-disks"></a>Yönetilen sanal makine disklerini

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Yönetilmeyen sanal makine disklerini

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="cloud-services-limits"></a>Bulut Hizmetleri sınırları
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Uygulama hizmeti sınırları
Aşağıdaki uygulama hizmeti sınırları, Web Apps, Mobile Apps, API Apps ve Logic Apps için sınırları içerir.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Scheduler sınırları
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Toplu iş sınırları
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk Services sınırları
Aşağıdaki tabloda, Azure Biztalk Services için sınırlarını gösterir.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB sınırları
Azure Cosmos DB, üretilen iş ve depolama ne olursa olsun uygulamanızın gerektirdiği işlemek için ölçeklendirilebilir bir küresel ölçekteki veritabanıdır. Azure Cosmos DB sağlar ölçek hakkında sorularınız varsa lütfen e-posta Gönder askcosmosdb@microsoft.com.

### <a name="mobile-engagement-limits"></a>Mobile Engagement sınırları
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Arama sınırları
Fiyatlandırma katmanlarına kapasite ve arama hizmetinizi sınırları belirler. Katmanları mevcuttur:

* *Ücretsiz* diğer Azure aboneleriyle paylaşılan çok kiracılı hizmeti projeleri değerlendirme ve küçük geliştirme için amaçlar.
* *Temel* yüksek oranda kullanılabilir sorgu iş yükleri için en çok üç yinelemelerle bir daha küçük ölçekli üretim iş yükleri için özel bilgi işlem kaynakları sağlar.
* *Standart (S1, S2, S3, S3 yüksek yoğunluk)* büyük üretim iş yükleri için değil. İş yükü profilinizi en iyi şekilde eşleşen bir kaynak yapılandırması seçebilmeniz standart katmanı içinde birden çok düzeyi yok.

**Abonelik başına sınırları**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Arama hizmeti başına sınırları**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Belge boyutu, sorguları ikinci, anahtarları, istekleri ve yanıtları, her gibi daha ayrıntılı bir düzeyde sınırları hakkında daha fazla bilgi için bkz: [hizmet sınırları Azure Search'te](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services sınırları
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN sınırları
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobile Services sınırları
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>İzleyici sınırları
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Bildirim hub'ı hizmet sınırları
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Olay hub'ları sınırları
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Hizmet veri yolu sınırları
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IOT hub'ı sınırları
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>IOT Hub cihaz sağlama hizmet sınırları
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Veri Fabrikası sınırları
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics sınırları
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store sınırları
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Veritabanı geçiş hizmet sınırları
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>Akış analizi sınırlar
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory sınırları
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Azure olay kılavuz sınırları
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-maps-limits"></a>Azure eşlemeleri sınırları
[!INCLUDE [maps-limits](../includes/maps-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple sistemi sınırları
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Günlük analizi sınırlar
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Yedekleme sınırları
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery limitleri
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Uygulama Öngörüler sınırları
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management sınırları
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis önbelleği sınırları
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Anahtar kasası sınırları
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Otomasyon sınırları
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL veritabanı sınırları
SQL veritabanı sınırları için bkz: [SQL veritabanı kaynak sınırları](sql-database/sql-database-resource-limits.md).

### <a name="sql-data-warehouse-limits"></a>SQL veri ambarı sınırları
SQL veri ambarı sınırları için bkz: [SQL veri ambarı kaynak sınırları](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Ayrıca bkz.
[Azure sınırları ve artar anlama](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Sanal makine ve bulut hizmeti boyutları Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Cloud Services boyutları](cloud-services/cloud-services-sizes-specs.md)
