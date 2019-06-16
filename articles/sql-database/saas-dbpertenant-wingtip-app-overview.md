---
title: Azure SQL veritabanı çok kiracılı bir uygulama örneği - Wingtip SaaS | Microsoft Docs
description: Azure SQL veritabanı örnek Wingtip SaaS kullanan örnek çok kiracılı bir uygulama kullanarak bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: 1c16ea44418d99ee1f80a7d0ef7a3e5b3f118f46
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485245"
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Kiracı başına veritabanı desen ile SQL veritabanı kullanan çok kiracılı bir SaaS uygulama giriş

Wingtip SaaS uygulaması bir örnek çok kiracılı uygulamadır. Uygulama, birden çok kiracıya hizmet vermek için Kiracı başına veritabanı SaaS uygulama düzeni kullanır. Uygulama, Azure SQL veritabanı çeşitli SaaS tasarım ve yönetim düzenlerini kullanarak SaaS senaryoları etkinleştirmek özelliklerini gösterir. Hızla çalışmaya başlamak için beş dakikadan kısa bir süre içinde Wingtip SaaS uygulamasını dağıtır.

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Başlamadan önce bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet yönetim komut dosyaları engelini kaldırmak için.

## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulaması, Kiracı başına veritabanı modeli kullanır. SQL elastik havuzları, verimliliği en üst düzeye çıkarmak için kullanır. Sağlama ve verilerine eşleme kiracılar için bir katalog veritabanı kullanılır. Wingtip SaaS uygulaması temel bir havuzla üç örnek Kiracı yanı sıra, Katalog veritabanı kullanır. Katalog ve Kiracı sunucu DNS diğer adları ile sağladınız. Bu diğer adlar, bir başvuru active Wingtip uygulama tarafından kullanılan kaynakları korumak için kullanılır. Bu diğer adlar olağanüstü durum kurtarma öğreticilerdeki kurtarma kaynakları işaret edecek şekilde güncelleştirildi. İlk dağıtım için eklentiler Wingtip SaaS öğreticileri sonuçları birçoğu tamamlanıyor. Analitik veritabanları ve veritabanları arası şema yönetimi gibi eklentiler kullanıma sunulmuştur.


![Wingtip SaaS mimarisi](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Bunlar veri katmanıyla ilişkili olarak SaaS düzenlerine odaklanmak, öğreticileri ve uygulama ile birlikte çalışır. Diğer bir deyişle, veri katmanına odaklanın ve uygulamanın overanalyze yok. Bu SaaS uygulamasının anlamak desenleri, bu düzenleri uygulamalarınıza uygulamak için anahtar. Ayrıca, belirli iş gereksinimlerinize yönelik gerekli değişiklikleri göz önünde bulundurun.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL veritabanı Wingtip SaaS öğreticileri

Uygulamayı dağıttıktan sonra ilk dağıtımı derleme aşağıdaki öğreticileri keşfedin. Bu öğretici, SQL veritabanı, Azure SQL veri ambarı ve diğer Azure hizmetleriyle yerleşik özelliklerinden yararlanmak yaygın SaaS düzenlerinin keşfedin. Öğreticiler, PowerShell betikleri ile ayrıntılı açıklamaları içerir. Açıklamalar, anlama ve SaaS yönetim düzenlerini uygulamalarınızda uygulaması basitleştirin.


| Öğretici | Açıklama |
|:--|:--|
| [Yönergeler ve SQL veritabanı çok kiracılı SaaS uygulaması örneği için ipuçları](saas-tenancy-wingtip-app-guidance-tips.md) | İndirin ve uygulamanın parçaları hazırlamak için PowerShell betikleri çalıştırın. |
|[Wingtip SaaS uygulamasını dağıtma ve keşfetme](saas-dbpertenant-get-started-deploy.md)|  Dağıtma ve Azure aboneliğiniz Wingtip SaaS uygulaması keşfedin. |
|[Kiracılar sağlama ve kataloğa kaydetme](saas-dbpertenant-provision-and-catalog.md)| Katalog veritabanını kullanarak kiracılara nasıl uygulama bağlanır ve Katalog kiracıların verilerini nasıl eşlendiğini öğrenin. |
|[Performansını izleyin ve yönetin](saas-dbpertenant-performance-monitoring.md)| Performans eşikleri aştığında uyarılar ayarlayın ve İzleme özelliklerini SQL veritabanı'nın nasıl kullanılacağını öğrenin. |
|[Azure İzleyici günlüklerine ile izleme](saas-dbpertenant-log-analytics.md) | Nasıl kullanacağınızı öğrenin [Azure İzleyici günlükleri](../log-analytics/log-analytics-overview.md) birden fazla havuzdaki kaynak büyük miktarlarda izlemek için. |
|[Tek bir kiracıyı geri yükleme](saas-dbpertenant-restore-single-tenant.md)| Bir kiracı veritabanı önceki bir noktaya geri yükleme konusunda bilgi edinin. Ayrıca var olan bir kiracı veritabanı çevrimiçi bırakır paralel veritabanına geri yüklemeyi öğreneceksiniz. |
|[Kiracı veritabanı şemasını yönetme](saas-tenancy-schema-management.md)| Şema ve başvuru verilerini tüm Kiracı veritabanlarında güncelleştirme hakkında bilgi edinin. |
|[Kiracılar arası dağıtılmış sorgular çalıştırma](saas-tenancy-cross-tenant-reporting.md) | Bir geçici analiz veritabanı oluşturun ve tüm kiracılar genelinde dağıtılmış gerçek zamanlı sorgular çalıştırın.  |
|[Ayıklanan Kiracı veriler üzerinde analiz çalıştırın](saas-tenancy-tenant-analytics.md) | Çevrimdışı analiz sorguları bir analiz veritabanı veya veri ambarı'na Kiracı verileri ayıklayın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Genel rehberlik ve Wingtip bilet SaaS uygulaması örneği dağıtma ve ipuçları](saas-tenancy-wingtip-app-guidance-tips.md)
- [Wingtip SaaS uygulamasını dağıtma](saas-dbpertenant-get-started-deploy.md)
