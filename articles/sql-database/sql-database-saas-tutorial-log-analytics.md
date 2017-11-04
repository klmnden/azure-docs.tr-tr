---
title: "Log Analytics’i bir SQL Veritabanı çok kiracılı uygulaması ile kullanma | Microsoft Docs"
description: "Kurulum ve bir çok kiracılı Azure SQL veritabanı SaaS uygulaması ile günlük analizi (OMS) kullanın"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: billgib; sstein
ms.openlocfilehash: 43d46e6a31ee05add33da59348a1d180c4078f97
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="setup-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Kurulum ve bir çok kiracılı Azure SQL veritabanı SaaS uygulaması ile günlük analizi (OMS) kullanın

Bu öğreticide ayarlama ve kullanma *günlük analizi ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* esnek havuzlar ve veritabanları izleme. Bu öğretici derlemeler [performans izleme ve yönetim Öğreticisi](sql-database-saas-tutorial-performance-monitoring.md). Nasıl kullanılacağını gösterir *günlük analizi* izleme ve Azure Portal'da sağlanan uyarı büyütmek için. Günlük analizi, izleme ve havuzları yüzlerce ve yüz binlerce veritabanları desteklediğinden ölçekte uyarı için uygundur. Ayrıca, birden fazla Azure aboneliği genelinde farklı uygulamaların ve Azure hizmetlerinin izlenmesini tümleştiren tek bir izleme çözümü sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Log Analytics’i yükleme ve yapılandırma (OMS)
> * Havuzları ve veritabanlarını izlemek için Log Analytics’i kullanma

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](sql-database-saas-tutorial.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

SaaS senaryoları ve düzenleri ile ilgili açıklamalar ve bunların izleme çözümü gereksinimlerini nasıl etkilediği hakkında bilgi edinmek için [Performans İzleme ve Yönetim öğreticisini](sql-database-saas-tutorial-performance-monitoring.md) inceleyin.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Log Analytics (OMS) ile performansı izleme ve yönetme

SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarı özellikleri, kaynağa özeldir ve az sayıda kaynak için kullanışlıdır. Ancak büyük kurulumları izlemek veya farklı kaynak ve aboneliklerde birleşik bir görünüm sağlamak için pek uygun değildir.

Yüksek hacimli senaryolar için Log Analytics kullanılabilir. Bu, yayınlanan tanılama günlükleri ve log analytics çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmetidir. Bu hizmet, birçok hizmetten telemetri toplayabilir ve uyarıları sorgulamak ve ayarlamak için kullanılabilir. Log Analytics, işlem verilerinin analiz edilmesini ve görselleştirilmesini sağlayan yerleşik bir sorgu dili ve veri görselleştirme araçları sunar. SQL analiz çözümü çeşitli ön tanımlı esnek havuz ve izleme ve görünümleri ve sorguları uyarı veritabanı sağlar ve kendi geçici sorgular eklemek ve bunları gerektiği gibi kaydetmenize olanak tanır. OMS, özel bir görünüm tasarımcısı da sağlar.

Log Analytics çalışma alanları ve analiz çözümleri, hem Azure portalında hem de OMS’de açılabilir. Azure portalı, daha yeni bir erişim noktası olmasına rağmen bazı alanlarda OMS portalının gerisinde kalabilir.

### <a name="create-data-by-starting-the-load-generator"></a>Yük Oluşturucu başlatarak verileri oluşturma 

1. **PowerShell ISE**’de **Demo-PerformanceMonitoringAndManagement.ps1** öğesini açın. Bu öğretici sırasında birkaç yük oluşturma senaryosu çalıştırmak isteyebileceğinizden bu betiği açık tutun.
1. Daha az beş kiracılar varsa, sağlama daha ilginç sağlamak için kiracılar toplu bağlam izleme:
   1. **$DemoScenario = 1,** **Kiracı grubu sağlama** değerini ayarlayın
   1. Komut dosyasını çalıştırmak için basın **F5**.

1. Ayarlama **$DemoScenario** = 2, **Generate normal yoğunluğu yük (yaklaşık 40 DTU)**.
1. Komut dosyasını çalıştırmak için basın **F5**.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics’i ve Azure SQL Analytics çözümünü yükleme ve yapılandırma

Log Analytics, yapılandırılması gereken ayrı bir hizmettir. Log Analytics, log analytics çalışma alanında günlük veriler, telemetri ve ölçümleri toplar. Çalışma alanı, Azure’daki diğer kaynaklara benzer bir kaynaktır ve oluşturulması gerekir. Çalışma alanının izlediği uygulamayla aynı kaynak grubunda oluşturulması gerekli olmasa da bu, çoğu zaman en mantıklı seçimdir. Wingtip SaaS uygulaması için bu kaynak grubunu silerek uygulamayla birlikte kolayca silinmesi için çalışma sağlar.

1. **PowerShell ISE**’de ...\\Öğrenme Modülleri\\Performans İzleme ve Yönetim\\Log Analytics\\*Demo-LogAnalytics.ps1* öğesini açın.
1. Komut dosyasını çalıştırmak için basın **F5**.

Bu noktada, Azure portalında (veya OMS portalı) mümkün açık günlük analizi olmalıdır. Telemetri günlük analizi çalışma alanındaki toplanacak ve görünür hale gelmesi birkaç dakika sürer. Uzun daha ilginç deneyimidir veri toplamayı sistem bırakın. Şimdi gidip bir içecek alabilirsiniz. Yük oluşturucunun çalıştığından emin olmanız yeterli!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için Log Analytics ve SQL Analytics çözümünü kullanma


Bu alıştırmada, veritabanları ve havuzları için toplanan telemetri bakmak için günlük analizi ve OMS Portalı'nı açın.

1. [Azure portalına](https://portal.azure.com) göz atın ve Diğer hizmetler’e tıklayarak Log Analytics’i açın, ardından Log Analytics’i aratın:

   ![log analytics’i aç](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. *wtploganalytics-&lt;USER&gt;* adlı çalışma alanını seçin.

1. Azure portalında Log Analytics çözümünü açmak için **Genel Bakış**’ı seçin.
   ![genel bakış-bağlantı](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **ÖNEMLİ**: Çözümün etkinleştirilmesi birkaç dakikayı bulabilir. Sabırlı olun!

1. Açmak için Azure SQL Analytics kutucuğuna tıklayın.

    ![genel bakış](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analizler](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Görünüm, çözüm dikey penceresinde en alttaki kendi kaydırma çubuğuyla yana doğru kaydırılır (gerekirse dikey pencereyi yenileyin).

1. Çeşitli görünümlere tıklayarak bunları keşfedin veya her bir kaynağa tıklayarak ayrıntılı gezgini açın. Bu gezginde, sol üst köşedeki zaman kaydırıcısını kullanabilir veya dikey çubuğa tıklayarak daha dar bir zaman dilimine odaklanabilirsiniz. Bu görünümle, belirli kaynaklara odaklanmak için veritabanlarını veya havuzları tek tek seçebilirsiniz:

    ![grafik](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Çözüm dikey penceresinde, en sağa doğru kaydırdığınızda açmak ve keşfetmek üzere tıklayabileceğiniz birkaç kayıtlı sorgu görebilirsiniz. Bunları değiştirmeyi deneyebilir ve oluşturduğunuz ilgi çekici sorguları daha sonra tekrar açıp diğer kaynaklarla kullanmak üzere kaydedebilirsiniz.

1. Log Analytics çalışma alanı dikey penceresine dönün ve çözümü burada açmak için OMS Portalı’nı seçin.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. OMS portalında uyarıları yapılandırabilirsiniz. Veritabanı DTU görünümünün uyarı kısmına tıklayın.

1. Açılan Günlük Araması görünümünde, sunulan ölçümlerin çubuk grafiğini göreceksiniz.

    ![günlük araması](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Araç çubuğunda Uyarı’ya tıklarsanız uyarı yapılandırmasını görebilir ve değiştirebilirsiniz.

    ![uyarı kuralı ekle](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

Log Analytics ve OMS’deki izleme ve uyarı özelliği, kaynak dikey pencerelerindeki kaynağa özel uyarı özelliğinin aksine çalışma alanındaki veriler üzerinde yapılan sorguları temel alır. Bu nedenle, veritabanı başına bir uyarı tanımlamak yerine tüm veritabanları için geçerli bir uyarı tanımlayabilirsiniz. İsterseniz birden fazla kaynak türü üzerinde birleşik bir sorgu kullanan bir uyarı da yazabilirsiniz. Sorgular, yalnızca çalışma alanında bulunan verilerle sınırlıdır.

SQL Veritabanı için Log Analytics, çalışma alanındaki veri hacmine göre ücretlendirilir. Bu öğreticide, günlük 500 MB ile sınırlı olan ücretsiz bir çalışma alanı oluşturdunuz. Sınıra ulaşıldığında, artık çalışma alanına veri eklenmez.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]
> * Log Analytics’i yükleme ve yapılandırma (OMS)
> * Havuzları ve veritabanlarını izlemek için Log Analytics’i kullanma

[Kiracı analizi öğreticisi](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Derleme sırasında ilk Wingtip SaaS uygulama dağıtımı ek öğreticileri](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
