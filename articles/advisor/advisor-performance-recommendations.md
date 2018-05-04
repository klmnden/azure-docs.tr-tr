---
title: Azure Danışmanı performans önerileri | Microsoft Docs
description: Azure dağıtımlarınızı performansını iyileştirmek için Danışmanı'nı kullanın.
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 00abb5aafc6f3aec2e2dd7326a307bee74d97cc1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="advisor-performance-recommendations"></a>Advisor performans önerileri

Azure Danışmanı performans önerileri hızı ve kritik iş uygulamalarının yanıtlama hızını geliştirilmesine yardımcı olun. Performans öneriler danışmanına alın **performans** Danışmanı Pano sekmesi.

## <a name="improve-database-performance-with-sql-db-advisor"></a>SQL DB Danışmanı ile veritabanı performansı

Advisor önerileri tüm Azure kaynakları için tutarlı, birleştirilmiş bir görünümünü sağlar. SQL Azure veritabanının performansını geliştirmek için öneriler getirmek için SQL Database Advisor ile tümleşir. SQL veritabanı Danışmanı, SQL Azure veritabanının performansını kullanım geçmişiniz çözümleyerek değerlendirir. Ardından, veritabanının tipik iş yükünü çalıştırmak için en uygun öneriler sunar. 

> [!NOTE]
> Önerileri almak için bir veritabanı hakkında kullanım haftada olması gerekir ve bu hafta içinde var. bazı tutarlı etkinlik olması gerekir. SQL veritabanı Danışmanı daha kolay rastgele WINS'e etkinlik için tutarlı bir sorgu modelleri için en iyi duruma getirebilirsiniz.

SQL Database Advisor hakkında daha fazla bilgi için bkz: [SQL veritabanı Danışmanı'nı](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis önbelleği performansı ve güvenilirliği iyileştirmek

Advisor burada performansını olumsuz yönde yüksek bellek kullanımı, sunucu iş yükü, ağ bant genişliği veya çok sayıda istemci bağlantıları tarafından etkilenebilir Redis önbelleği örnekleri tanımlar. Advisor en iyi yöntemler, olası sorunları önlemenize yardımcı olacak öneriler de sağlar. Redis önbelleği öneriler hakkında daha fazla bilgi için bkz: [Redis önbelleği Danışmanı](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Uygulama hizmeti performans ve güvenilirlik geliştirmek

Azure Danışmanı uygulama hizmetleri deneyiminizi geliştirmek ve ilgili platform özelliklerini keşfetmek için en iyi uygulama önerilerini tümleştirir. Uygulama Hizmetleri önerileri örnekleri şunlardır:
* Algılama, bellek veya CPU kaynaklarını azaltma seçenekleri ile uygulama çalışma zamanları tarafından tükendiği örnekleri.
* Burada collocating kaynakları web uygulamaları ve veritabanları gibi örneklerinin algılama, performans ve düşük maliyetli artırabilir. 

Uygulama Hizmetleri öneriler hakkında daha fazla bilgi için bkz: [Azure App Service için en iyi uygulamaları](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Performans önerileri Danışmanı erişme

1. Oturum [Azure portal](https://portal.azure.com)ve ardından açın [Danışmanı](https://aka.ms/azureadvisordashboard).

2.  Advisor Panoda tıklatın **performans** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:

* [Advisor giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
* [Advisor yüksek kullanılabilirlik önerileri](advisor-high-availability-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)

