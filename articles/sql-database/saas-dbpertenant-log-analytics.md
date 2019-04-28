---
title: Azure İzleyici günlüklerine bir SQL veritabanı çok kiracılı uygulaması ile kullanma | Microsoft Docs
description: Ayarlama ve Azure İzleyici günlüklerine çok kiracılı bir Azure SQL veritabanı SaaS uygulama ile kullanma
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 6380488faa9a4554df5df5ea67e11dbeb8853fff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61390734"
---
# <a name="set-up-and-use-azure-monitor-logs-with-a-multitenant-sql-database-saas-app"></a>Ayarlama ve Azure İzleyici günlüklerine çok kiracılı bir SQL veritabanı SaaS uygulama ile kullanma

Bu öğreticide, ayarlama ve kullanma [Azure İzleyici günlükleri](/azure/log-analytics/log-analytics-overview) elastik havuzları ve veritabanlarını izlemek için. Bu öğreticide yapılar [performans izleme ve yönetim öğreticisini](saas-dbpertenant-performance-monitoring.md). Azure İzleyici günlüklerine izleme büyütmek için nasıl kullanılacağını gösterir ve uyarı Azure portalında sağlanan. Azure İzleyici, yüz binlerce veritabanını ve elastik havuzların binlik izleme destekler günlüğe kaydeder. Azure İzleyici günlüklerine birden çok Azure aboneliği genelinde farklı uygulamalar ve Azure hizmetlerinin izlenmesini tümleştiren tek bir izleme çözümü sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleyin ve Azure İzleyici günlüklerine yapılandırın.
> * Azure İzleyicisi'ni, havuzları ve veritabanlarını izlemek için günlüğe kaydeder.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS Kiracı başına veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Wingtip bilet SaaS Kiracı başına veritabanı uygulamayı keşfetme](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Bkz: [performans izleme ve yönetim öğreticisini](saas-dbpertenant-performance-monitoring.md) bir irdelemesi ve SaaS senaryoları ve düzenleri ve bunların izleme çözümü gereksinimlerini nasıl etkiler.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-azure-monitor-logs"></a>Azure İzleyici günlükleri ile veritabanı ve elastik havuz performansını izleme ve yönetme

İzleme ve uyarı Azure SQL veritabanı için veritabanlarını ve Azure portalında havuzlarında kullanılabilir. Bu yerleşik izleme ve uyarı çok kullanışlı, ancak ayrıca kaynağa özgü. Bu, çok da iyi büyük kurulumları izlemek ya da kaynak ve Aboneliklerde birleşik bir görünümünü sağlamak için uygun olan anlamına gelir.

Yüksek hacimli senaryolar için Azure İzleyici günlüklerine, izleme ve uyarı amacıyla kullanabilirsiniz. Azure İzleyici, tanılama günlükleri ve potansiyel olarak birçok hizmetten bir çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmetidir. Azure İzleyici günlüklerine yerleşik bir sorgu işlem verilerinin analiz izin dili ve veri görselleştirme araçları sağlar. SQL analizleri çözümü, birkaç önceden tanımlanmış bir elastik havuz ve veritabanı izleme ve uyarı görünümleri ve sorguları sağlar. Azure İzleyici günlüklerine, özel bir Görünüm Tasarımcısı da sağlar.

OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır. Log Analytics çalışma alanları ve analiz çözümleri, Azure portalında açın. Azure portalında yeni erişim noktasıdır, ancak bazı alanlar Operations Management Suite portalında arkasında nedir olabilir.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Kiracılarınız bir iş yüküne benzetimini yaparak performansı tanılama veri oluşturma 

1. PowerShell ISE'de açın *... \\WingtipTicketsSaaS MultiTenantDb ana\\öğrenme modülleri\\performans izleme ve Yönetim\\Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç yük oluşturma senaryosu çalıştırmak isteyebilirsiniz, çünkü bu betiği açık tutun.
1. Bunu zaten yapmadıysanız bir izleme bağlamı daha ilginç hale getirmek için Kiracı grubu sağlama. Bu işlem birkaç dakika sürer.

   a. Ayarlama **$DemoScenario = 1**, _bir kiracı grubu sağlama_.

   b. Betiği çalıştırın ve bir ek 17 Kiracı dağıtmak için F5 tuşuna basın.

1. Artık tüm kiracının benzetilmiş bir yük çalıştırmak için yük oluşturucuyu başlatma.

    a. Ayarlama **$DemoScenario = 2**, _normal yoğunlukta yük (yaklaşık 30 DTU)_.

    b. Betiği çalıştırmak için F5 tuşuna basın.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS Kiracı başına veritabanı uygulama betiklerini alma

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Wingtip biletleri PowerShell betikleri engellemesini indirip adımları için bkz [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-workspace-and-the-azure-sql-analytics-solution"></a>Log Analytics çalışma alanı ve Azure SQL Analytics çözümünü yükleme ve yapılandırma

Azure İzleyici yapılandırılması gereken ayrı bir hizmettir. Azure İzleyici, bir Log Analytics çalışma alanında toplanan günlük verilerini, telemetri ve ölçümleri günlüğe kaydeder. Bir Log Analytics çalışma alanı yalnızca azure'daki diğer kaynakları gibi yeniden oluşturulması gerekir. Çalışma alanı, izlediği uygulamalarla aynı kaynak grubunda oluşturulması gerekmez. Çoğunlukla yapılması ancak anlamlı. Wingtip bilet uygulaması için tek bir kaynak grubu ile uygulama çalışma alanı silindi emin olmak için kullanın.

1. PowerShell ISE'de açın *... \\WingtipTicketsSaaS MultiTenantDb ana\\öğrenme modülleri\\performans izleme ve Yönetim\\Log Analytics\\Demo-LogAnalytics.ps1*.
1. Betiği çalıştırmak için F5 tuşuna basın.

Azure İzleyicisi'ni açmak için artık Azure portalında günlüğe kaydeder. Log Analytics çalışma alanında telemetri toplamak için ve görünür yapmak için birkaç dakika sürer. Artık daha ilgi çekici bir deneyim olan tanılama verilerini toplama sistem bırakın. 

## <a name="use-log-analytics-workspace-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için log Analytics çalışma alanı ve SQL Analytics çözümünü kullanın


Bu alıştırmada veritabanları ve havuzları için toplanan telemetriyi bakmak için Azure portalında Log Analytics çalışma alanını açın.

1. [Azure portala](https://portal.azure.com) gidin. Seçin **tüm hizmetleri** Log Analytics çalışma alanını açın. Ardından, Log Analytics için arama yapın.

   ![Log Analytics çalışma alanını Aç](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Adlı çalışma alanı seçin _wtploganalytics -&lt;kullanıcı&gt;_.

1. Seçin **genel bakış** Azure portalında log analytics çözümünü açmak için.

   ![Genel Bakış](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Bu, birkaç çözüm etkinleştirilmeden önce dakika sürebilir. 

1. Seçin **Azure SQL Analytics** kutucuğunu açın.

    ![Genle bakış kutucuğu](media/saas-dbpertenant-log-analytics/overview.png)

1. Çözümdeki görünümler ile kendi iç kaydırma çubuğu altındaki betiklerdeki kaydırın. Gerekirse, sayfayı yenileyin.

1. Özet sayfasında keşfetmek için detaya gitme Gezgini'ni açmak için tek tek veritabanları ve kutucuklar'ı seçin.

    ![Log analytics Panosu](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Zaman aralığını değiştirmek için filtre ayarlarını değiştirin. Bu öğreticide, seçin **son 1 saat**.

    ![Zaman Filtresi](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Sorgu kullanım ve ölçüm veritabanı için keşfetmek için tek bir veritabanı seçin.

    ![Analiz veritabanı](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Kullanım ölçümleri görmek için analiz sayfasını sağa kaydırın.
 
     ![Veritabanı ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Analiz sayfasını sola kaydırın ve sunucu kutucuğu seçin **kaynak bilgisi** listesi.  

    ![Kaynak bilgileri listesi](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Havuzları ve veritabanlarını sunucuda gösteren bir sayfa açılır.

    ![Havuzlar ve veritabanları ile sunucu](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Bir havuz seçin. Açılan havuzu sayfasında havuz ölçümleri görmek için sağa kaydırın. 

    ![Havuz ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


1. Geri Log Analytics çalışma alanında **OMS portalında** var. çalışma alanını açın.

    ![Log Analytics çalışma alanı](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

Log Analytics çalışma alanında, daha fazla günlük ve ölçüm verileri araştırabilirsiniz. 

İzleme ve uyarı Azure İzleyici'de Azure portalında her kaynağı tanımlanan uyarı aksine çalışma alanındaki veriler üzerinde günlükleri sorguları temel alır. Sorguları uyarıları alma tarafından tanımlama tek başına veritabanı yerine tüm veritabanları üzerinde görünen tek bir uyarı tanımlayabilirsiniz. Sorgular yalnızca çalışma alanında bulunan verilerle sınırlıdır.

Azure İzleyici günlüklerine sorgulamak ve uyarıları ayarlamak için nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure İzleyicisi'nde uyarı kuralları ile iş günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Azure İzleyici, çalışma alanındaki veri hacmine dayalı ve SQL veritabanı ücretleri için günlüğe kaydeder. Bu öğreticide, günde 500 MB ile sınırlı olan ücretsiz bir çalışma alanı oluşturulur. Bu sınıra ulaşıldıktan sonra veriler artık çalışma alanına eklenir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yükleyin ve Azure İzleyici günlüklerine yapılandırın.
> * Azure İzleyicisi'ni, havuzları ve veritabanlarını izlemek için günlüğe kaydeder.

Deneyin [Kiracı analizi Öğreticisi](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [İlk Wingtip bilet SaaS Kiracı başına veritabanı uygulama dağıtımı geliştirecek ek öğreticilerden](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure İzleyici günlükleri](../azure-monitor/insights/azure-sql.md)
