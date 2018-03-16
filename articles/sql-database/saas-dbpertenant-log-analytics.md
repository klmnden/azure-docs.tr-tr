---
title: "Log Analytics’i bir SQL Veritabanı çok kiracılı uygulaması ile kullanma | Microsoft Docs"
description: "Kurulum ve bir çok kiracılı Azure SQL veritabanı SaaS uygulaması ile günlük analizi (OMS) kullanın"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: b141ca521ae9c4d9bf6a4be620bc8e5432c52f83
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Ayarlama ve bir çok kiracılı Azure SQL veritabanı SaaS uygulaması ile günlük analizi (OMS) kullanma

Bu öğreticide ayarlama ve kullanma *günlük analizi ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* esnek havuzlar ve veritabanları izleme. Bu öğretici derlemeler [performans izleme ve yönetim Öğreticisi](saas-dbpertenant-performance-monitoring.md). Nasıl kullanılacağını gösterir *günlük analizi* izleme ve Azure Portal'da sağlanan uyarı büyütmek için. Günlük analizi esnek havuzlarını izleme binlerce ve yüz binlerce veritabanlarını destekler. Günlük analizi farklı uygulamalar ve Azure Hizmetleri birden çok Azure aboneliği izleme tümleştirebilirsiniz tek bir izleme çözümü sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Log Analytics’i yükleme ve yapılandırma (OMS)
> * Havuzları ve veritabanlarını izlemek için Log Analytics’i kullanma

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Başına Wingtip biletleri SaaS veritabanı Kiracı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

SaaS senaryoları ve düzenleri ile ilgili açıklamalar ve bunların izleme çözümü gereksinimlerini nasıl etkilediği hakkında bilgi edinmek için [Performans İzleme ve Yönetim öğreticisini](saas-dbpertenant-performance-monitoring.md) inceleyin.

## <a name="monitoring-and-managing-database-and-elastic-pool-performance-with-log-analytics-or-operations-management-suite-oms"></a>İzleme ve günlük analizi ya da Operations Management Suite (OMS) ile veritabanı ve esnek havuzu performansı yönetme

İzleme ve uyarı SQL veritabanı için veritabanlarını ve Azure portalında havuzlarında kullanılabilir. Bu yerleşik izleme ve uyarma uygun olmakla birlikte, kaynak özgü olan, onu iyi büyük yüklemeler izleme veya kaynaklar ve abonelikler arasında birleştirilmiş bir görünümünü sağlamak için uygundur.

Yüksek hacimli senaryolar için günlük analizi izleme ve uyarı için kullanılabilir. Günlük analizi analytics tanılama günlüklerini etkinleştiren ayrı bir Azure hizmet ve bir çalışma alanında potansiyel olarak birçok Hizmetleri'nden toplanan telemetri ' dir. Günlük analizi yerleşik bir sorgu işletimsel veri analizi izin vererek dil ve veri görselleştirme araçlar sağlar. Çeşitli ön tanımlı esnek havuz veritabanı izleme ve uyarı görünümleri ve sorguları SQL analiz çözümü sağlar. OMS, özel bir görünüm tasarımcısı da sağlar.

Log Analytics çalışma alanları ve analiz çözümleri, hem Azure portalında hem de OMS’de açılabilir. Azure portalı, daha yeni bir erişim noktası olmasına rağmen bazı alanlarda OMS portalının gerisinde kalabilir.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Kiracılarınız bir iş yükünü taklit ederek Performans Tanılama verileri oluşturma 

1. İçinde **PowerShell ISE**açın *... \\WingtipTicketsSaaS MultiTenantDb ana\\modülleri öğrenme\\performans izleme ve Yönetim\\** Demo PerformanceMonitoringAndManagement.ps1***. Bu öğretici sırasında birkaç yük oluşturma senaryosu çalıştırmak isteyebileceğinizden bu betiği açık tutun.
1. Henüz yapmadıysanız, daha ilginç izleme bağlamı sağlamak için kiracılar toplu sağlayın. Bu işlem birkaç dakika sürer:
   1. Ayarlama **$DemoScenario = 1**, _kiracılar toplu sağlama_
   1. Komut dosyasını çalıştırın ve ek 17 kiracılar dağıtmak için basın **F5**.  

1. Şimdi tüm kiracılar benzetilmiş bir yük çalıştırmak için yük oluşturucunun başlatın.  
    1. Ayarlama **$DemoScenario = 2**, _Generate normal yoğunluğu yük (yaklaşık 30 DTU)_.
    1. Komut dosyasını çalıştırmak için basın **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri PowerShell betikleri engellemesini kaldırmak.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics’i ve Azure SQL Analytics çözümünü yükleme ve yapılandırma

Log Analytics, yapılandırılması gereken ayrı bir hizmettir. Günlük analizi günlük verilerini, telemetri ve günlük analizi çalışma alanındaki ölçümleri toplar. Günlük analizi çalışma alanı, azure'daki diğer kaynakları gibi bir kaynak değildir ve oluşturulmalıdır. Çalışma alanı uygulamaları ile aynı kaynak grubunda oluşturulması değil gerekirken, en uygun kadar sık sağlar yapmak izlediği. Wingtip biletleri uygulama için tek bir kaynak grubu kullanarak çalışma alanında uygulama ile silinir sağlar.

1. İçinde **PowerShell ISE**açın *... \\WingtipTicketsSaaS MultiTenantDb ana\\modülleri öğrenme\\performans izleme ve Yönetim\\oturum Analytics\\** Demo LogAnalytics.ps1***.
1. Komut dosyasını çalıştırmak için basın **F5**.

Bu noktada, Azure portalında (veya OMS portalı) mümkün açık günlük analizi olmalıdır. Telemetri günlük analizi çalışma alanındaki toplanacak ve görünür hale gelmesi birkaç dakika sürer. Uzun daha ilginç deneyimidir tanılama verilerini toplama sistem bırakın. Şimdi gidip bir içecek alabilirsiniz. Yük oluşturucunun çalıştığından emin olmanız yeterli!

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için Log Analytics ve SQL Analytics çözümünü kullanma


Bu alıştırmada, veritabanları ve havuzları için toplanan telemetri bakmak için günlük analizi ve OMS Portalı'nı açın.

1. Gözat [Azure portal](https://portal.azure.com) ve günlük analizi tıklayarak açın **tüm hizmetleri**, günlük analizi için arama yapın:

   ![log analytics’i aç](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Adlı çalışma alanını seçin _wtploganalytics -&lt;kullanıcı&gt;_.

1. Azure portalında Log Analytics çözümünü açmak için **Genel Bakış**’ı seçin.

   ![Genel Bakış-bağlantı](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Birkaç çözüm etkinleştirilmeden önce dakika sürebilir. Sabırlı olun!

1. Açmak için Azure SQL Analytics kutucuğuna tıklayın.

    ![genel bakış](media/saas-dbpertenant-log-analytics/overview.png)

    ![analizler](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Çözüm görünümleri kendi iç kaydırma çubuğunun altındaki (gerekirse sayfayı yenileyin) ile betiklerdeki kaydırın.

1. Özet sayfasında döşeme veya ayrıntıya Gezgini'ni açmak için tek bir veritabanının tıklayarak keşfedin.

1. Bu öğretici çekme - zaman aralığını değiştirmek için ayarı filtresini değiştirin _son 1 saat_

    ![Zaman Filtresi](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Sorgu kullanım ve bu veritabanı için ölçümleri keşfetmek için tek bir veritabanı seçin.

    ![Veritabanı analizi](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Kullanımını görmek için ölçümleri analitik sayfası sağa kaydırın.
 
     ![Veritabanı ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Analytics sayfanın sol kaydırma ve kaynak bilgileri listesinde sunucu kutucuğuna tıklayın. Bu sunucu üzerinde havuzları ve veritabanları gösteren bir sayfa açar. 

     ![kaynak bilgileri](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

 
     ![Sunucu havuzları ve veritabanlarıyla](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Sunucuda server veritabanlarını ve havuzları tıklatın havuzunda açılır sayfası gösterir.  Açılır havuzu sayfasında havuzu ölçümlerini görmek için sağa kaydırın.  

     ![Havuz ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)



1. Günlük analizi çalışma geri üzerinde seçin **OMS portalı** çalışma alanını vardır.

    ![oms](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

OMS Portalı'nda günlük ve ölçüm verilerini daha fazla çalışma alanında gözden geçirebilirsiniz.  

İzleme ve günlük analizi ve OMS uyarı temel sorgulamaları Azure Portalı'ndaki her bir kaynağın tanımlanan uyarı aksine çalışma alanında, veriler üzerinde. Sorgulamaları uyarıları alma tarafından tanımlama tek başına veritabanı yerine, tüm veritabanları üzerinden arar tek bir uyarı tanımlayabilirsiniz. Sorgular, yalnızca çalışma alanında bulunan verilerle sınırlıdır.

Sorgulamak ve uyarıları ayarlamak için OMS kullanma hakkında daha fazla bilgi için bkz: [uyarı kurallarında günlük analizi çalışma](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

SQL Veritabanı için Log Analytics, çalışma alanındaki veri hacmine göre ücretlendirilir. Bu öğreticide, 500 MB günde sınırlı olduğu bir ücretsiz çalışma alanı oluşturuldu. Bu sınıra ulaşıldığında, veriler artık çalışma alanı'na eklenir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Log Analytics’i yükleme ve yapılandırma (OMS)
> * Havuzları ve veritabanlarını izlemek için Log Analytics’i kullanma

[Kiracı analizi öğreticisi](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme sırasında ilk başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtımı ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
