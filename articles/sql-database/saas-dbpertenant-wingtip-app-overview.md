---
title: "Azure SQL veritabanı çok kiracılı uygulama örneği - Wingtip SaaS | Microsoft Docs"
description: "Azure SQL Database, Wingtip SaaS örnek kullanan örnek bir çok kiracılı uygulama kullanarak bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/12/2017
ms.author: sstein
ms.openlocfilehash: 451e1fc87fc5f626e78760d8cd5c4115ea02bb0d
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="introduction-to-a-multi-tenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Kiracı deseni başına veritabanı ile SQL veritabanı kullanan bir çok kiracılı SaaS uygulamasına giriş

*Wingtip SaaS* bir örnek çok kiracılı uygulama bir uygulamadır. Uygulama, birden çok kiracıya hizmet için SaaS uygulama düzeni, Kiracı başına veritabanı kullanır. Uygulama, birçok SaaS tasarım ve yönetim modellerini kullanan SaaS senaryoları etkinleştirmek Azure SQL veritabanı özelliklerini gösterir. Hızlıca başlamak ve çalıştırmak için beş dakikadan daha kısa bir süre içinde Wingtip SaaS uygulamayı dağıtır!

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Başlamadan önce kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri yönetim komut dosyaları engellemesini kaldırmak.

## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulama Kiracı başına veritabanı modeli kullanır ve verimliliğini en üst düzeye çıkarmak için SQL esnek havuzu kullanır. Sağlama ve verilerine eşleme kiracılar için bir katalog veritabanı kullanılır. Wingtip SaaS uygulamasına çekirdek üç örnek kiracılar havuzuyla yanı sıra, Katalog veritabanı kullanır. Öğreticiler eklentileri ilk dağıtıma neden Wingtip SaaS çoğunu Tamamlanıyor, analitik veritabanları sunarak veritabanları arası şema yönetimi, vb.


![Wingtip SaaS mimarisi](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Şu öğreticileri giderek ve uygulama ile birlikte çalışma sırasında veri katmanı ilgili olarak SaaS düzenlerini esas odaklanmak önemlidir. Başka bir deyişle, veri katmanına odaklanın ve uygulamanın kendisini gereğinden fazla analiz etmeyin. Bu SaaS uygulamasının anlamak desenleri, belirli iş gereksinimlerinizi için gerekli tüm değişiklikleri ınızın uygulamalarınızda bu desenleri uygulama için anahtar.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL veritabanı Wingtip SaaS öğreticileri

Uygulamayı dağıttıktan sonra ilk dağıtım sırasında yapı aşağıdaki öğreticileri keşfedin. SQL veritabanı, SQL veri ambarı ve diğer Azure hizmetleriyle yerleşik özelliklerden yararlanmak ortak SaaS desenler bu öğreticileri keşfedin. Öğreticiler anlama ve uygulamalarınızda aynı SaaS Yönetimi desenleri uygulama büyük ölçüde kolaylaştırma ayrıntılı açıklamalar, PowerShell komut dosyaları içerir.


| Öğretici | Açıklama |
|:--|:--|
| [Kılavuzu ve Azure SQL veritabanı çok kiracılı SaaS uygulama örneği için ipuçları](saas-tenancy-wingtip-app-guidance-tips.md) | **BURADAN BAŞLAYIN!** İndirin ve uygulama bölümlerinin hazırlamak için PowerShell betikleri çalıştırın. |
|[Dağıtma ve Wingtip SaaS uygulamasına keşfedin](saas-dbpertenant-get-started-deploy.md)|  Dağıtma ve Azure aboneliğinize Wingtip SaaS uygulamasına keşfedin. |
|[Sağlama ve Katalog kiracılar](saas-dbpertenant-provision-and-catalog.md)| Uygulama Kataloğu veritabanı kullanarak kiracılara nasıl bağlandığını ve Katalog kiracılar verilerini nasıl eşlendiğini öğrenin. |
|[İzleme ve performansı yönetme](saas-dbpertenant-performance-monitoring.md)| SQL veritabanı'nın İzleme özelliklerini kullanmayı ve performans eşikler aşıldığında uyarıları ayarlamak nasıl öğrenin. |
|[Günlük analizi (OMS) ile izleme](saas-dbpertenant-log-analytics.md) | Kullanma hakkında bilgi edinin [günlük analizi](../log-analytics/log-analytics-overview.md) kaynakları, büyük miktarlarda birden çok havuzlardaki izlemek için. |
|[Tek bir kiracı geri yükleme](saas-dbpertenant-restore-single-tenant.md)| Bir kiracı veritabanı zaman içinde önceki bir noktaya geri öğrenin. Varolan Kiracı veritabanı çevrimiçi bırakarak paralel bir veritabanına geri yükleme için adımlar da dahil edilir. |
|[Kiracı veritabanı şeması yönetme](saas-tenancy-schema-management.md)| Şemayı Güncelleştir ve tüm Kiracı veritabanları arasında başvuru verileri güncelleştirmek hakkında bilgi edinin. |
|[Çapraz Kiracı dağıtılmış sorgular çalıştırın](saas-tenancy-cross-tenant-reporting.md) | Bir geçici analytics veritabanı oluşturun ve tüm kiracılar arasında gerçek zamanlı dağıtılmış sorgular çalıştırın.  |
|[Ayıklanan Kiracı verilerini analiz çalıştırma](saas-tenancy-tenant-analytics.md) | Kiracı veri ambarında bir analytics veritabanı veya veri çevrimdışı analitik sorguları için ayıklayın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Genel rehberlik ve dağıtma ve Wingtip biletleri SaaS uygulama örneği kullanırken ipuçları](saas-tenancy-wingtip-app-guidance-tips.md)

- [Wingtip SaaS uygulamasına dağıtmak](saas-dbpertenant-get-started-deploy.md)
