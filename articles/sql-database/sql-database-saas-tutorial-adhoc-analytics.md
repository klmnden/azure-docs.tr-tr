---
title: "Birden çok Azure SQL veritabanında geçici analiz sorguları çalıştırma | Microsoft Docs"
description: "Çok kiracılı bir uygulamadaki birden çok veritabanında geçici analiz sorguları çalıştırma"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: dd41e7f1f131f6c18e03d2434982c3d681342b8b
ms.contentlocale: tr-tr
ms.lasthandoff: 05/12/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wtp-tenants"></a>Tüm WTP kiracılarında geçici analiz sorguları çalıştırma

Bu öğreticide bir geçici analiz veritabanı oluşturup, tüm kiracılarda birkaç sorgu çalıştıracaksınız. Bu sorgular, WTP uygulamasının günlük işlem verilerine gömülü öngörüleri ayıklayabilir.

WTP uygulaması geçici analiz sorguları çalıştırmak için (birden fazla kiracıda), bir analiz veritabanı ile birlikte [Esnek Sorgu](sql-database-elastic-query-overview.md) kullanır.


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Geçici bir analiz veritabanı dağıtma
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma



Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)


## <a name="ad-hoc-analytics-pattern"></a>Geçici analiz düzeni

SaaS uygulamalarının sunduğu harika fırsatlardan biri de bulutta merkezi olarak depoladığınız büyük miktarda kiracı verisini kullanmaktır. Uygulamalarınızın çalışmasını ve kullanımını, kiracılarınızı, kullanıcılarını ve kullanıcıların tercihleri ile davranışlarını anlamak için bu verileri kullanın. Bulduğunuz öngörüler uygulama ve hizmetlerinize yönelik gelecekteki özellik geliştirmeleri, kullanılabilirlik iyileştirmeleri ve diğer yatırımlar için rehberlik sağlayabilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Buna yönelik bir yaklaşım da ortak şemaya sahip dağıtılmış bir veritabanı kümesinde geçici sorgulamayı sağlayan Esnek Sorgu kullanılmasıdır. Esnek Sorgu, dış tabloların tanımlandığı ve dağıtılmış (kiracı) veritabanlarındaki tabloları yansıtan tek bir *baş* veritabanı kullanır. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Esnek Sorgu, kiracı veritabanlarının konumunu sağlamak üzere katalog veritabanındaki parça eşlemesini kullanır. Normal T-SQL ile kurulum ve sorgulama basittir ve Power BI ile Excel gibi araçlardan geçici sorgulama yapmayı destekler.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>Geçici analiz sorguları için kullanılan veritabanını dağıtma

Bu alıştırmada, tüm kiracı veritabanlarına yayılan geçici sorgular düzenlemek için kullanılan şemayı içeren Geçici Analiz veritabanı dağıtılır.

1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\İşlem Analizi\\Geçici Analiz\\*Demo-AdhocAnalytics.ps1* dosyasını açıp aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 2, **Geçici analiz veritabanı dağıtma**.

1. **F5** tuşuna basarak betiği çalıştırın ve geçici analiz için yeni bir SQL veritabanı oluşturup WTP kataloğuna ekleyin. TicketPurchases, Tickets ve Venues tabloları, sorgulanabilen dış tablolar olarak eklenir.
   Burada *RPC sunucusu kullanılamıyor* şeklinde uyarılarla karşılaşmanız normaldir.


Şu anda, dağıtılmış sorguları çalıştırmak ve tüm kiracılarda öngörüler elde etmek için bir *adhocanalytics* veritabanına sahip oldunuz!

## <a name="run-ad-hoc-analytics-queries"></a>Geçici analiz sorguları çalıştırma

Bu alıştırmada, WTP uygulamasından kiracı öngörülerini ortaya çıkarmak için geçici analiz sorguları çalıştırılır.

1. SSMS’te ...\\Öğrenme Modülleri\\İşlem Analizi\\Geçici Analiz\\*Demo-AdhocAnalyticsQueries.sql* dosyasını açın.
1. **adhocanalytics** veritabanına bağlı olduğunuzdan emin olun
1. Çalıştırmak istediğiniz sorguyu seçip **F5** tuşuna basın:

    ![sorgu](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Geçici bir analiz veritabanı dağıtma
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma

[Log Analytics (OMS) öğreticisi](sql-database-saas-tutorial-log-analytics.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Esnek Sorgu](sql-database-elastic-query-overview.md)

