---
title: Günlük analizi bir SQL veritabanı çok müşterili uygulama ile kullanma | Microsoft Docs
description: Ayarlama ve günlük analizi çok müşterili bir Azure SQL veritabanı SaaS uygulaması ile kullanma
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.reviewer: billgib
ms.openlocfilehash: 02c380c78fa773b56a3c8b666e890836a3d8e54b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="set-up-and-use-log-analytics-with-a-multitenant-sql-database-saas-app"></a>Ayarlama ve günlük analizi çok müşterili bir SQL veritabanı SaaS uygulaması ile kullanma

Bu öğreticide ayarlama ve Azure kullanma [günlük analizi](/azure/log-analytics/log-analytics-overview) esnek havuzlar ve veritabanları izlemek için. Bu öğretici derlemeler [performans izleme ve yönetim Öğreticisi](saas-dbpertenant-performance-monitoring.md). Azure Portalı'nda sağlanan uyarı ve günlük analizi izleme büyütmek için nasıl kullanılacağını gösterir. Günlük analizi esnek havuzlarını izleme binlerce ve yüz binlerce veritabanlarını destekler. Günlük analizi farklı uygulamalar ve Azure Hizmetleri birden çok Azure aboneliği izleme tümleştirebilirsiniz tek bir izleme çözümü sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yükleyin ve günlük analizi yapılandırın.
> * Günlük analizi havuzları ve veritabanları izlemek için kullanın.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS Kiracı başına veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS Kiracı başına veritabanı uygulama keşfetme](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

Bkz: [performans izleme ve yönetim öğretici](saas-dbpertenant-performance-monitoring.md) SaaS senaryolar ve desenleri ve bir izleme çözümü gereksinimleri nasıl etkilediklerini Tartışması için.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-log-analytics"></a>İzleme ve günlük analizi ile veritabanı ve esnek havuzu performansı yönetme

İzleme ve uyarma Azure SQL veritabanı için veritabanlarını ve Azure portalında havuzlarında kullanılabilir. Bu yerleşik izleme ve uyarı uygun olmakla birlikte, ayrıca kaynak özgü. İyi büyük yüklemeleri izlemek ya da kaynakları ve abonelikler arasında birleştirilmiş bir görünümünü sağlamak için uygundur anlamına gelir.

Yüksek hacimli senaryolar için izleme ve uyarı için günlük analizi kullanabilirsiniz. Günlük analizi tanılama günlüklerini ve bir çalışma alanında potansiyel olarak birçok Hizmetleri'nden toplanan telemetri üzerinden analizi sağlar ayrı bir Azure hizmetidir. Günlük analizi yerleşik bir sorgu işletimsel veri analizi izin dil ve veri görselleştirme araçlar sağlar. Birçok önceden tanımlanmış esnek havuz veritabanı izleme ve uyarma görünümleri ve sorguları SQL analiz çözümü sağlar. Günlük analizi Özel Görünüm Tasarımcısı da sağlar.

Günlük analizi çalışma alanları ve analiz çözümleri, Azure portalında ve Operations Management Suite açın. Azure portalı, yeni erişim noktası olmakla birlikte bazı alanlar Operations Management Suite portalında arkasında olabilir.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Kiracılarınız bir iş yükünü taklit ederek Performans Tanılama verileri oluşturma 

1. PowerShell ISE açmak *... \\WingtipTicketsSaaS MultiTenantDb ana\\modülleri öğrenme\\performans izleme ve Yönetim\\Demo PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç iş yükünün oluşturma senaryoları çalıştırmak isteyebilirsiniz çünkü bu komut dosyası açık tutun.
2. Bunu zaten yapmadıysanız izleme bağlamı daha ilginç hale kiracılar toplu sağlayın. Bu işlem birkaç dakika sürer.

   a. Ayarlama **$DemoScenario = 1**, _kiracılar toplu sağlama_.

   b. Komut dosyasını çalıştırın ve ek 17 kiracılar dağıtmak için F5 tuşuna basın.

3. Şimdi tüm kiracılar benzetilmiş bir yük çalıştırmak için yük oluşturucunun başlatın.

    a. Ayarlama **$DemoScenario = 2**, _Generate normal yoğunluğu yük (yaklaşık 30 DTU)_.

    b. Komut dosyasını çalıştırmak için F5 tuşuna basın.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip biletleri SaaS Kiracı başına veritabanı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok müşterili veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Karşıdan yüklemek ve Wingtip biletleri PowerShell betikleri engellemesini kaldırmak adımlar için bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md).

## <a name="install-and-configure-log-analytics-and-the-azure-sql-analytics-solution"></a>Günlük analizi ve Azure SQL analiz çözümü yükleme ve yapılandırma

Günlük analizi yapılandırılmalıdır ayrı bir hizmettir. Günlük analizi günlük verilerini, telemetri ve günlük analizi çalışma alanındaki ölçümleri toplar. Yalnızca diğer kaynaklar gibi Azure günlük analizi çalışma alanı oluşturulması gerekir. Çalışma alanı izlediği uygulamaları ile aynı kaynak grubunda oluşturulması gerekmez. Bu nedenle sık yapılması çoğu ancak mantıklıdır. Wingtip biletleri uygulama için tek bir kaynak grubu çalışma alanında uygulama ile silinir emin olmak için kullanın.

1. PowerShell ISE açmak *... \\WingtipTicketsSaaS MultiTenantDb ana\\modülleri öğrenme\\performans izleme ve Yönetim\\oturum Analytics\\Demo LogAnalytics.ps1*.
2. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Şimdi Azure portal ya da Operations Management Suite portalına günlük analizi açabilirsiniz. Günlük analizi çalışma alanındaki telemetri toplamak ve görünür yapmak için birkaç dakika sürer. Uzun daha ilginç deneyimidir tanılama verilerini toplama sistem bırakın. 

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için Log Analytics ve SQL Analytics çözümünü kullanma


Bu alıştırmada, veritabanları ve havuzları için toplanan telemetri bakmak için günlük analizi ve Operations Management Suite portalına açın.

1. Gözat [Azure portal](https://portal.azure.com). Seçin **tüm hizmetleri** günlük analizi açın. Ardından günlük analizi arayın.

   ![Açık günlük analizi](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

2. Adlı çalışma alanını seçin _wtploganalytics -&lt;kullanıcı&gt;_.

3. Azure portalında Log Analytics çözümünü açmak için **Genel Bakış**’ı seçin.

   ![Genel Bakış](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Birkaç çözüm etkinleştirilmeden önce dakika sürebilir. 

4. Seçin **Azure SQL analizi** açmak için kutucuğa.

    ![Genle bakış kutucuğu](media/saas-dbpertenant-log-analytics/overview.png)

5. Çözüm görünümleri betiklerdeki kendi iç kaydırma çubuğunun altındaki ile kaydırın. Gerekirse, sayfayı yenileyin.

6. Özet sayfasında keşfetmek için kutucukları veya ayrıntıya Gezgini'ni açmak için ayrı veritabanlarını seçin.

    ![Günlük analizi Panosu](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

7. Zaman aralığını değiştirmek için Filtre ayarını değiştirin. Bu öğretici için seçin **son 1 saat**.

    ![Zaman Filtresi](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

8. Bu veritabanı için ölçümleri ve sorgu kullanım keşfetmek için tek bir veritabanı seçin.

    ![Veritabanı analizi](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

9. Kullanım ölçümlerini görmek için sağa analytics sayfasına gidin.
 
     ![Veritabanı ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

10. Analytics sayfanın sol kaydırma ve sunucu parçasında seçin **kaynak bilgileri** listesi.  

    ![Kaynak bilgileri listesi](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Veritabanları ve havuzları sunucuda gösteren bir sayfa açılır.

    ![Sunucu havuzları ve veritabanlarıyla](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

11. Bir havuz seçin. Açılır havuzu sayfasında havuzu ölçümlerini görmek için sağa kaydırın. 

    ![Havuz ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


12. Geri günlük analizi çalışma alanında seçin **OMS portalı** çalışma alanını vardır.

    ![Operations Management Suite portalına döşeme](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

Operations Management Suite Portalı'nda günlük ve ölçüm verilerini daha fazla çalışma alanında gözden geçirebilirsiniz. 

İzleme ve günlük analizi uyarı sorgulamaları Azure Portalı'ndaki her bir kaynağın tanımlanan uyarı aksine çalışma alanında, veriler üzerinde temel alır. Sorgulamaları uyarıları alma tarafından tanımlama tek başına veritabanı yerine, tüm veritabanları üzerinden arar tek bir uyarı tanımlayabilirsiniz. Sorgular yalnızca çalışma alanında veri tarafından sınırlıdır.

Günlük analizi sorgulamak ve uyarıları ayarlamak için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [uyarı kurallarında günlük analizi ile çalışırsınız](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating).

Günlük analizi çalışma alanındaki veri hacmi göre SQL veritabanı giderler. Bu öğreticide, 500 MB günde sınırlı olduğu bir ücretsiz çalışma alanı oluşturuldu. Bu sınıra ulaşıldıktan sonra verileri artık çalışma alanı'na eklenir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yükleyin ve günlük analizi yapılandırın.
> * Günlük analizi havuzları ve veritabanları izlemek için kullanın.

Deneyin [Kiracı analytics Öğreticisi](saas-dbpertenant-log-analytics.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [İlk Wingtip biletleri SaaS Kiracı başına veritabanı uygulama dağıtımı yapı ek öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
