---
title: Azure Danışmanı performans önerilerini | Microsoft Docs
description: Azure dağıtımlarınızı performansını iyileştirmek için Danışman'ı kullanın.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 3caf838fec3a5c0ab847ded85b269df7a66859e0
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266754"
---
# <a name="advisor-performance-recommendations"></a>Danışmanı performans önerileri

Azure Danışmanı performans önerilerini, hız ve yanıt hızını iş açısından kritik uygulamalarınızın geliştirilmesine yardımcı olur. Performans önerileri Danışmandan alma **performans** Danışman Panosu sekmesi.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>DNS sağlıklı uç noktalar için daha hızlı yük devretme için Traffic Manager profilinizin üzerinde etkin kalma süresi azaltın

[Yaşam süresi (TTL) ayarları süresi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) Traffic Manager profilinizin uç noktaları belirli bir uç nokta sorgularına yanıt vermemeye başlarsa hızlı bir şekilde geçiş yapma belirtmenizi sağlar. TTL değerleri azaltma, istemcileri işlevsel Uç noktalara daha hızlı yönlendirilir anlamına gelir.

Azure Danışmanı, yapılandırılmış için uzun bir TTL Traffic Manager profillerini tanımlar ve önerir 20 saniye ya da bağlı olarak 60 saniyede TTL yapılandırma profili için yapılandırıldığından [hızlı yük devretme](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>SQL DB Danışmanı ile veritabanı performansını artırın

Danışman, öneriler tüm Azure kaynaklarınız için tutarlı ve birleştirilmiş bir görünümünü sağlar. SQL Azure veritabanınızın performansını artırmak için önerileri getirmek için SQL veritabanı Danışmanı ile tümleştirilir. SQL veritabanı Danışmanı, kullanım geçmişinizi analiz ederek SQL Azure veritabanlarınızın performansını değerlendirir. Ardından, veritabanının tipik iş yükünü çalıştırmak için en uygun adaylardır öneriler sunar.

> [!NOTE]
> Öneriler almak için bir veritabanı kullanımının bir hafta hakkında sahip olmalıdır ve bu hafta içinde var. bazı tutarlı etkinlik olması gerekir. SQL veritabanı Danışmanı daha kolay etkinlik rastgele ani artışlar için tutarlı sorgu desenleri için en iyi duruma getirebilirsiniz.

SQL veritabanı Danışmanı hakkında daha fazla bilgi için bkz: [SQL veritabanı Danışmanı](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-app-service-performance-and-reliability"></a>App Service performans ve güvenilirliğini artırın

Azure Danışmanı, uygulama hizmetleri deneyiminizi geliştirmek ve ilgili platform özelliklerini keşfetmek için en iyi yöntem önerilerini tümleştirir. Uygulama Hizmetleri önerileri örnekleri şunlardır:
* Algılama örnekleri burada bellek veya CPU kaynaklarını azaltma seçenekleri ile uygulama çalışma zamanı tarafından bitti.
* Algılama örnekleri burada collocating kaynak web uygulamaları ve veritabanları gibi performans ve düşük maliyetli artırabilir.

Uygulama Hizmetleri öneriler hakkında daha fazla bilgi için bkz. [Azure App Service için en iyi](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Disk g/ç azalmasını engellemek için yönetilen diskleri kullanma

Danışman, ölçeklenebilirlik hedefine ulaşmak bir depolama hesabına ait sanal makineleri tanımlar. Bu, g/ç azalmasını getirir. Danışman, bu sanal makinelerin performans düşüşünü önlemek için yönetilen diskleri kullanmanız önerir.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>Premium Depolama'yı kullanarak performans ve sanal makine disklerini güvenilirliğini artırın

Danışman, depolama hesabınızda yüksek hacimli işlemler sahip standart disklere sahip sanal makineleri tanımlar ve premium disklere yükseltme önerir. 

Azure Premium depolama, g/Ç açısından yoğun iş yüklerini çalıştıran sanal makineleri için yüksek performanslı, düşük gecikme süreli disk desteği sunar. Premium depolama hesapları kullanan sanal makine disklerini verileri katı hal sürücülerine (SSD) depolar. Uygulamanız için en iyi performans için premium depolama yüksek IOPS gerektiren tüm sanal makine disklerinin geçişi öneririz.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablosu üzerinde eğriltme verilerini kaldırma

Veri dengesizliği gereksiz veri hareketi veya kaynak darboğazları yükünüz çalıştırırken neden olabilir. Advisor dağıtım veri dengesizliği % 15'den büyük ve verilerinizi dağıtan ve tablo dağıtım anahtar seçimlerinizi yeniden ziyaret öneririz algılar. Ve hakkında daha fazla tanımlama eğriltme kaldırma bilgi edinmek için [eğriltme sorun giderme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablosu güncel olmayan tablo istatistikleri güncelle

Advisor tanımlayan güncel olmayan tablolar [tablo istatistikleri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) ve tablo istatistikleri oluşturmak veya güncelleştirmek önerir. Sorgu iyileştiricisi, en hızlı performans için yüksek kaliteli bir sorgu planı oluşturmak sorgu iyileştiricisi sağlayan sorgu sonucu satır sayısı ve kardinalite tahmin etmek için güncel statikler kullanır. SQL veri ambarı.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tabloları önbellek kullanımını iyileştirmek için ölçeği büyütün

Azure Danışmanı, SQL veri ambarınızın olması durumunda yüksek önbellek yüzdesini kullanılan ve düşük isabet yüzdesi algılar. Bu, SQL veri ambarınızın performansını etkileyebilir yüksek önbellek çıkarma gösterir. İş yükünüz için yeterli önbellek kapasite ayırma sağlamak için SQL veri ambarınızın ölçeğini Advisor önerir.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>Sorgu performansını artırmak için çoğaltılmış tablolar için SQL veri ambarı tabloları Dönüştür

Danışman, çoğaltılmış tablolar olmayan ancak dönüştürmenizi avantaj elde edecektir tabloları tanımlar ve bu tablolar dönüştürme önerir. Öneriler çoğaltılmış tablo boyutu, sütunları, tabloda dağıtım türü ve SQL veri ambarı tablosunun bölüm sayısını temel alır. Ek buluşsal yöntemler, bağlamı için Önerideki sağlanabilir. Bu öneri belirleme hakkında daha fazla bilgi için bkz: [SQL veri ambarı önerileri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Depolama hesabınızı tüm en yeni Azure özellikleri almak için Azure Resource Manager'a geçiş

Depolama hesabı, dağıtım modeli için Azure Resource Manager (şablon dağıtımları, ek güvenlik seçenekleri ve Azure Depolama'nın en son özelliklerin kullanımı bir GPv2 hesabına yükseltme olanağı yararlanmak için ARM) geçirin. Danışman, Klasik dağıtım modelini kullanarak herhangi bir tek başına bir depolama hesabı tanımlayacak ve ARM dağıtım modeline geçirilmesi önerir.

> [!NOTE]
> Klasik uyarılar Azure İzleyici'de duyurulan Haziran 2019 ' devre dışı bırakmak için yeni platform ile uyarı işlevselliği korumak için ARM için Klasik depolama hesabı yükseltmeniz kesinlikle önerilir. Daha fazla bilgi için [Klasik uyarılar devre dışı bırakılması](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Nasıl Danışmanı performans önerileri

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2.  Advisor panosunda **performans** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:

* [Advisor giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerisi](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)
