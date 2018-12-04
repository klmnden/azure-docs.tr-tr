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
ms.openlocfilehash: ba79d2d5ed4350960af1b92ee863595e3540a1d2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843936"
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

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis Cache performans ve güvenilirliğini artırın

Danışman, Redis Cache örnekleri burada performansı olumsuz yönde yüksek bellek kullanımı, sunucu iş yükü, ağ bant genişliği veya çok sayıda istemci bağlantıları tarafından etkilenebilir tanımlar. Advisor, ayrıca olası sorunları önlemek için öneriler en iyi yöntemler sağlar. Redis Cache öneriler hakkında daha fazla bilgi için bkz. [Redis Cache Danışmanı](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>App Service performans ve güvenilirliğini artırın

Azure Danışmanı, uygulama hizmetleri deneyiminizi geliştirmek ve ilgili platform özelliklerini keşfetmek için en iyi yöntem önerilerini tümleştirir. Uygulama Hizmetleri önerileri örnekleri şunlardır:
* Algılama örnekleri burada bellek veya CPU kaynaklarını azaltma seçenekleri ile uygulama çalışma zamanı tarafından bitti.
* Algılama örnekleri burada collocating kaynak web uygulamaları ve veritabanları gibi performans ve düşük maliyetli artırabilir. 

Uygulama Hizmetleri öneriler hakkında daha fazla bilgi için bkz. [Azure App Service için en iyi](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablosu üzerinde eğriltme verilerini kaldırma

Veri dengesizliği gereksiz veri hareketi veya kaynak darboğazları yükünüz çalıştırırken neden olabilir. Advisor dağıtım veri dengesizliği % 15'den büyük ve verilerinizi dağıtan ve tablo dağıtım anahtar seçimlerinizi yeniden ziyaret öneririz algılar. Ve hakkında daha fazla tanımlama eğriltme kaldırma bilgi edinmek için [eğriltme sorun giderme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Sorgu performansını artırmak için SQL veri ambarı tablosu güncel olmayan tablo istatistikleri güncelle

Advisor tanımlayan güncel olmayan tablolar [tablo istatistikleri](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) ve tablo istatistikleri oluşturmak veya güncelleştirmek önerir. Sorgu iyileştiricisi, en hızlı performans için yüksek kaliteli bir sorgu planı oluşturmak sorgu iyileştiricisi sağlayan sorgu sonucu satır sayısı ve kardinalite tahmin etmek için güncel statikler kullanır. SQL veri ambarı.

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Depolama hesabınızı tüm en yeni Azure özellikleri almak için Azure Resource Manager'a geçiş

Depolama hesabı, dağıtım modeli için Azure Resource Manager (şablon dağıtımları, ek güvenlik seçenekleri ve Azure Depolama'nın en son özelliklerin kullanımı bir GPv2 hesabına yükseltme olanağı yararlanmak için ARM) geçirin. Danışman, Klasik dağıtım modelini kullanarak herhangi bir tek başına bir depolama hesabı tanımlayacak ve ARM dağıtım modeline geçirilmesi önerir. 

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

