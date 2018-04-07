---
title: Azure SQL veritabanı çok müşterili uygulama örneği - Wingtip SaaS | Microsoft Docs
description: Azure SQL Database, Wingtip SaaS örnek kullanan örnek bir çok kiracılı uygulama kullanarak bilgi edinin
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 224639dcc7da950801c7a5959ec14fc5ac7313e0
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="introduction-to-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Kiracı başına veritabanı desen ile SQL veritabanı kullanan çok kiracılı bir SaaS uygulama giriş

Bir örnek çok müşterili uygulama Wingtip SaaS uygulamasıdır. Uygulama, birden çok kiracıya hizmet için Kiracı başına veritabanı SaaS uygulama deseni kullanır. Uygulama, birçok SaaS tasarım ve yönetim desenler kullanılarak SaaS senaryoları etkinleştirmek Azure SQL veritabanı özelliklerini gösterir. Hızlıca başlamak ve çalıştırmak için beş dakikadan daha kısa bir süre içinde Wingtip SaaS uygulamayı dağıtır.

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Başlamadan önce bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri yönetim komut dosyaları engellemesini kaldırmak.

## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulaması Kiracı başına veritabanı modeli kullanır. SQL esnek havuzu verimliliğini en üst düzeye çıkarmak için kullanır. Sağlama ve verilerine eşleme kiracılar için bir katalog veritabanı kullanılır. Wingtip SaaS uygulamasına çekirdek üç örnek kiracılar havuzuyla yanı sıra, Katalog veritabanı kullanır. İlk dağıtım eklentileri Wingtip SaaS öğreticileri sonuçlarında çoğunu tamamlanıyor. Analitik veritabanları ve veritabanları arası şema yönetimi gibi eklentiler sunulur.


![Wingtip SaaS mimarisi](media/saas-dbpertenant-wingtip-app-overview/app-architecture.png)


Şu öğreticileri gidin ve uygulama ile birlikte çalışmak, veri katmanı ilgili olarak SaaS düzenlerini esas odaklanın. Diğer bir deyişle, veri katmanına odaklanmanıza ve uygulama overanalyze yok. Bu SaaS uygulamasının anlamak desenleri, uygulamalarınızı bu desenleri uygulama için anahtar. Ayrıca, belirli iş gereksinimlerinizi için gerekli tüm değişiklikleri göz önünde bulundurun.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL veritabanı Wingtip SaaS öğreticileri

Uygulamayı dağıttıktan sonra ilk dağıtımı yapı aşağıdaki öğreticileri keşfedin. SQL Database, Azure SQL veri ambarı ve diğer Azure hizmetleriyle yerleşik özelliklerden yararlanmak ortak SaaS desenler bu öğreticileri keşfedin. Öğreticiler ayrıntılı açıklamalar PowerShell komut dosyaları içerir. Açıklamaları anlama ve aynı SaaS Yönetimi desenler uygulamalarınızda uyarlamasını basitleştirin.


| Öğretici | Açıklama |
|:--|:--|
| [Kılavuzu ve SQL veritabanı çok kiracılı SaaS uygulama örneği için ipuçları](saas-tenancy-wingtip-app-guidance-tips.md) | İndirin ve uygulama bölümlerinin hazırlamak için PowerShell betikleri çalıştırın. |
|[Dağıtma ve Wingtip SaaS uygulamasına keşfedin](saas-dbpertenant-get-started-deploy.md)|  Dağıtma ve Azure aboneliğinizle Wingtip SaaS uygulamasına keşfedin. |
|[Sağlama ve Katalog kiracılar](saas-dbpertenant-provision-and-catalog.md)| Uygulama Kataloğu veritabanını kullanarak kiracılara nasıl bağlandığını ve Katalog kiracılar verilerini nasıl eşlendiğini öğrenin. |
|[İzleme ve performansı yönetme](saas-dbpertenant-performance-monitoring.md)| SQL veritabanı'nın İzleme özelliklerini kullanın ve performans eşikler aşıldığında uyarıları ayarlama hakkında bilgi edinin. |
|[Azure günlük analizi ile izleme](saas-dbpertenant-log-analytics.md) | Nasıl kullanacağınızı öğrenin [günlük analizi](../log-analytics/log-analytics-overview.md) kaynakları büyük miktarlarda birden çok havuzlardaki izlemek için. |
|[Tek bir kiracı geri yükleme](saas-dbpertenant-restore-single-tenant.md)| Bir kiracı veritabanı zaman içinde önceki bir noktaya geri öğrenin. Ayrıca varolan Kiracı veritabanı çevrimiçi ayrıldığında bir paralel veritabanına geri öğrenin. |
|[Kiracı veritabanı şeması yönetme](saas-tenancy-schema-management.md)| Şema ve tüm Kiracı veritabanları arasında başvuru verileri güncelleştirme öğrenin. |
|[Çapraz Kiracı dağıtılmış sorgular çalıştırın](saas-tenancy-cross-tenant-reporting.md) | Bir geçici analytics veritabanı oluşturun ve tüm kiracılar arasında gerçek zamanlı dağıtılmış sorgular çalıştırın.  |
|[Ayıklanan Kiracı verilerini analiz çalıştırma](saas-tenancy-tenant-analytics.md) | Kiracı veri ambarında bir analytics veritabanı veya veri çevrimdışı analitik sorguları için ayıklayın. |


## <a name="next-steps"></a>Sonraki adımlar

- [Genel rehberlik ve dağıtmak ve Wingtip biletleri SaaS uygulama örneği kullandığınızda ipuçları](saas-tenancy-wingtip-app-guidance-tips.md)
- [Wingtip SaaS uygulamasına dağıtmak](saas-dbpertenant-get-started-deploy.md)
