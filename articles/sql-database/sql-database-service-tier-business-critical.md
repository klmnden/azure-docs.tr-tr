---
title: Kritik iş katmanı - Azure SQL veritabanı hizmeti | Microsoft Docs
description: Azure SQL veritabanı iş açısından kritik katmanı hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: e9f40e749642f2025c5298df74f9d8ff87aec14b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60709333"
---
# <a name="business-critical-tier---azure-sql-database"></a>Kritik iş katmanı - Azure SQL veritabanı

> [!NOTE]
> Kritik iş katmanı Premium DTU satın alma modeli denir. Sanal çekirdek tabanlı satın alma modeli DTU tabanlı satın alma modeli ile bir karşılaştırması için bkz: [Azure SQL veritabanı'nın modelleri ve kaynakları satın alma](sql-database-purchase-models.md).

Azure SQL veritabanı altyapı hataları durumda bile % 99,99 kullanılabilirlik sağlamak üzere bulut ortamı için ayarlanmış bir SQL Server veritabanı altyapısı mimarisini temel alır. Azure SQL veritabanı'nda kullanılan üç Mimari modeli vardır:
- Genel amaçlı/standart 
- Kritik iş/Premium
- Hiper Ölçek

Premium/iş açısından kritik hizmet katman modeli, veritabanı altyapısı işlemleri kümede temel alır. Bu mimari bir model, her zaman bir kullanılabilir veritabanı altyapısı düğüm Çekirdeği ve en az düzeyde performans etkisi bile bakım etkinlikleri sırasında iş yüküne sahip bir olgu kullanır.

Azure, yükseltir ve son kullanıcılar için en düşük kapalı kalma süresi ile temel işletim sistemi, sürücüler ve SQL Server Veritabanı Altyapısı'nın şeffaf bir şekilde yamaları. 

Premium kullanılabilirlik, Premium ve iş açısından kritik hizmet katmanlarında Azure SQL veritabanı'nın etkinleştirilmiş ve devam eden bakım işlemleri nedeniyle bir performans etkisi kabul edilemez kullanımlı iş yükleri için tasarlanmıştır.

Premium modelde, Azure SQL veritabanı, işlem ve depolama tek düğümde tümleştirir. Bu mimari modelinde yüksek kullanılabilirlik çoğaltma dört düğümlü küme, SQL Server'a benzer teknolojisini kullanarak dağıtılmış hesaplama (SQL Server veritabanı altyapısı işlem) ve depolama (yerel olarak bağlı SSD) tarafından gerçekleştirilir [her zaman açık Kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Veritabanı altyapısı düğüm kümesi](media/sql-database-managed-instance/business-critical-service-tier.png)

Hem SQL veritabanı altyapısı işlem ve arka plandaki mdf/ldf dosyaları aynı düğümde yerel olarak bağlı SSD depolamasında iş yükünüz için düşük gecikme süresi sağlayan yerleştirilir. Yüksek kullanılabilirlik için SQL Server benzer teknolojisi kullanılarak uygulanır [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Her müşterinin iş yükü ve verilerin kopyalarını içeren üç bir ikincil işlemleri için erişilebilir olan bir birincil veritabanı ile veritabanı düğümden oluşan bir kümede veritabanıdır. Birincil düğümden ikincil düğümlerine değişiklikleri herhangi bir nedenle birincil düğüm kilitlenmesi durumunda, veriler ikincil çoğaltmalarda kullanılabilir olmasını sağlamak için sürekli iter. Yük devretme SQL Server veritabanı altyapısı tarafından gerçekleştirilir: bir ikincil çoğaltma birincil düğüm haline gelir ve yeterli düğümleri sağlamak için yeni bir ikincil çoğaltma oluşturulur. İş yükü, yeni birincil düğüme otomatik olarak yönlendirilir.

Ayrıca, iş açısından kritik küme yerleşiktir [okuma ölçeği genişletme](sql-database-read-scale-out.md) sağlayan ücretsiz, yetenek ücret performans engellememelidir salt okunur sorguları (örneğin, raporları) çalıştırmak için kullanılan yerleşik salt okunur düğüm Birincil iş yükünü.

## <a name="when-to-choose-this-service-tier"></a>Bu hizmet katmanını seçmek ne zaman?

İş kritik hizmet katmanı, düşük gecikme süresi gerektiren uygulamalar için tasarlanmıştır yanıtları temel alınan SSD depolaması (1-2 ms içinde ortalama), altyapının başarısız olursa Hızlı Kurtarma ya da raporlar, analiz, yükünü azaltmak için gereksinim ve salt okunur ücretsiz olarak okunabilir ikincil çoğaltma birincil veritabanının ücretsiz sorgular.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [genel amaçlı](sql-database-service-tier-general-purpose.md) ve [hiper ölçekli](sql-database-service-tier-hyperscale.md) katmanları.
- Hakkında bilgi edinin [Service Fabric](../service-fabric/service-fabric-overview.md).
- Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için daha fazla seçenek için bkz: [iş sürekliliği](sql-database-business-continuity.md).
