---
title: "Azure SQL veritabanı çok kiracılı uygulama örneği - Wingtip SaaS | Microsoft Docs"
description: "Azure SQL Database, Wingtip SaaS örnek kullanan örnek bir çok kiracılı uygulama kullanarak bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: ddd51c23c7e7d01e38b02c79c27d1951eea61e70
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Bir SQL veritabanı çok kiracılı SaaS uygulama örneği giriş

*Wingtip SaaS* SQL veritabanı benzersiz avantajları gösteren örnek bir çok kiracılı uygulama, bir uygulamadır. Uygulama, birden fazla kiracıya hizmet vermek için SaaS uygulama düzeni olan kiracı başına veritabanını kullanır. Uygulama, birçok SaaS tasarım ve yönetim desenleri dahil olmak üzere, SaaS senaryoları etkinleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır. Hızlıca başlamak ve çalıştırmak için beş dakikadan daha kısa bir süre içinde Wingtip SaaS uygulamayı dağıtır!

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. Komut dosyalarını çalıştırmak için [indirme öğrenme modülleri klasörü](#download-and-unblock-the-wingtip-saas-scripts) yerel bilgisayarınıza.

## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulama Kiracı başına veritabanı modeli kullanır ve verimliliğini en üst düzeye çıkarmak için SQL esnek havuzu kullanır. Sağlama ve verilerine eşleme kiracılar için bir katalog veritabanı kullanılır. Wingtip SaaS uygulamasına çekirdek üç örnek kiracılar havuzuyla yanı sıra, Katalog veritabanı kullanır. Öğreticiler eklentileri ilk dağıtıma neden Wingtip SaaS çoğunu Tamamlanıyor, analitik veritabanları sunarak veritabanları arası şema yönetimi, vb.


![Wingtip SaaS mimarisi](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Şu öğreticileri giderek ve uygulama ile birlikte çalışma sırasında veri katmanı ilgili olarak SaaS düzenlerini esas odaklanmak önemlidir. Başka bir deyişle, veri katmanına odaklanın ve uygulamanın kendisini gereğinden fazla analiz etmeyin. Bu SaaS uygulamasının anlamak desenleri, belirli iş gereksinimlerinizi için gerekli tüm değişiklikleri ınızın uygulamalarınızda bu desenleri uygulama için anahtar.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL veritabanı Wingtip SaaS öğreticileri

Uygulamayı dağıttıktan sonra ilk dağıtım sırasında yapı aşağıdaki öğreticileri keşfedin. SQL veritabanı, SQL veri ambarı ve diğer Azure hizmetleriyle yerleşik özelliklerden yararlanmak ortak SaaS desenler bu öğreticileri keşfedin. Öğreticiler anlama ve uygulamalarınızda aynı SaaS Yönetimi desenleri uygulama büyük ölçüde kolaylaştırma ayrıntılı açıklamalar, PowerShell komut dosyaları içerir.


| Öğretici | Açıklama |
|:--|:--|
| [Kılavuzu ve Azure SQL veritabanı çok kiracılı SaaS uygulama örneği için ipuçları](saas-dbpertenant-wingtip-app-guidance-tips.md) | **BURADAN BAŞLAYIN!** İndirin ve uygulama bölümlerinin hazırlamak için PowerShell betikleri çalıştırın. |
|[Dağıtma ve Wingtip SaaS uygulamasına keşfedin](saas-dbpertenant-get-started-deploy.md)|  Dağıtma ve Azure aboneliğinize Wingtip SaaS uygulamasına keşfedin. |
|[Sağlama ve Katalog kiracılar](saas-dbpertenant-provision-and-catalog.md)| Uygulama Kataloğu veritabanı kullanarak kiracılara nasıl bağlandığını ve Katalog kiracılar verilerini nasıl eşlendiğini öğrenin. |
|[İzleme ve performansı yönetme](saas-dbpertenant-performance-monitoring.md)| SQL veritabanı'nın İzleme özelliklerini kullanmayı ve performans eşikler aşıldığında uyarıları ayarlamak nasıl öğrenin. |
|[Günlük analizi (OMS) ile izleme](saas-dbpertenant-log-analytics.md) | Kullanma hakkında bilgi edinin [günlük analizi](../log-analytics/log-analytics-overview.md) kaynakları, büyük miktarlarda birden çok havuzlardaki izlemek için. |
|[Tek bir kiracı geri yükleme](saas-dbpertenant-restore-single-tenant.md)| Bir kiracı veritabanı zaman içinde önceki bir noktaya geri öğrenin. Varolan Kiracı veritabanı çevrimiçi bırakarak paralel bir veritabanına geri yükleme için adımlar da dahil edilir. |
|[Kiracı şema yönetme](saas-tenancy-schema-management.md)| Şemayı Güncelleştir ve tüm Wingtip SaaS kiracılar arasında başvuru verileri güncelleştirmek hakkında bilgi edinin. |
|[Geçici analizler çalıştırır](saas-tenancy-adhoc-analytics.md) | Bir geçici analytics veritabanı oluşturun ve tüm kiracılar arasında gerçek zamanlı dağıtılmış sorgular çalıştırın.  |
|[Kiracı analizler çalıştırır](saas-tenancy-tenant-analytics.md) | Kiracı veri ambarında çevrimdışı analitik sorguları çalıştırmak için bir analytics veritabanı veya veri ayıklayın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Kılavuzu ve Azure SQL veritabanı çok kiracılı SaaS uygulama örneği için ipuçları](saas-dbpertenant-wingtip-app-guidance-tips.md)

- [Wingtip SaaS uygulamasına dağıtmak](saas-dbpertenant-get-started-deploy.md)
