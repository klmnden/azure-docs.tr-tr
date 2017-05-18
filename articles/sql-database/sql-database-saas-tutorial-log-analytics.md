---
title: "Log analytics’i ayarlama ve çalıştırma (Azure SQL Veritabanı’nı kullanan örnek SaaS uygulaması) | Microsoft Belgeleri"
description: "WTP örnek SaaS uygulamasıyla Log Analytics’i ayarlama ve kullanma"
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
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 7cb9b7dd90123a91cabe66fd8efa8ae4c9e2fa01
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="setup-and-use-log-analytics-oms-with-the-wtp-sample-saas-app"></a>WTP örnek SaaS uygulamasıyla Log Analytics’i (OMS) ayarlama ve kullanma

Bu öğreticide, elastik havuzları ve veritabanlarını izlemek için WTP uygulamasıyla *Log Analytics’i ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* ayarlayacak ve kullanacaksınız. Bu öğretici, [Performans İzleme ve Yönetim öğreticisini](sql-database-saas-tutorial-performance-monitoring.md) temel alır ve Azure portalında sağlanan izleme ve uyarı özelliklerini güçlendirmek için *Log Analytics*’in nasıl kullanılacağını gösterir. Log Analytics, yüzlerce havuz ve yüz binlerce veritabanını desteklediğinden özellikle büyük ölçekli izleme ve uyarı özellikleri için uygundur. Ayrıca, birden fazla Azure aboneliği genelinde farklı uygulamaların ve Azure hizmetlerinin izlenmesini tümleştiren tek bir izleme çözümü sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Log Analytics’i yükleme ve yapılandırma (OMS)
> * Havuzları ve veritabanlarını izlemek için Log Analytics’i kullanma

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

SaaS senaryoları ve düzenleri ile ilgili açıklamalar ve bunların izleme çözümü gereksinimlerini nasıl etkilediği hakkında bilgi edinmek için [Performans İzleme ve Yönetim öğreticisini](sql-database-saas-tutorial-performance-monitoring.md) inceleyin.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Log Analytics (OMS) ile performansı izleme ve yönetme

SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarı özellikleri, kaynağa özeldir ve az sayıda kaynak için kullanışlıdır. Ancak büyük kurulumları izlemek veya farklı kaynak ve aboneliklerde birleşik bir görünüm sağlamak için pek uygun değildir.

Yüksek hacimli senaryolar için Log Analytics kullanılabilir. Bu, yayınlanan tanılama günlükleri ve log analytics çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmetidir. Bu hizmet, birçok hizmetten telemetri toplayabilir ve uyarıları sorgulamak ve ayarlamak için kullanılabilir. Log Analytics, işlem verilerinin analiz edilmesini ve görselleştirilmesini sağlayan yerleşik bir sorgu dili ve veri görselleştirme araçları sunar. SQL Analizleri çözümü, önceden tanımlanmış çok sayıda elastik havuz ve veritabanı izleme ve uyarı görünümleri ve sorguları sağlar. Ayrıca kendi geçici sorgularınızı ekleyip gerektiğinde bunları kaydetmenize imkan tanır. OMS, özel bir görünüm tasarımcısı da sağlar.

Log Analytics çalışma alanları ve analiz çözümleri, hem Azure portalında hem de OMS’de açılabilir. Azure portalı, daha yeni bir erişim noktası olmasına rağmen bazı alanlarda OMS portalının gerisinde kalabilir.

### <a name="start-the-load-generator-to-create-data-to-analyze"></a>Analiz edilecek verileri oluşturmak için yük oluşturucuyu başlatma

1. **PowerShell ISE**’de **Demo-PerformanceMonitoringAndManagement.ps1** öğesini açın. Bu öğretici sırasında birkaç yük oluşturma senaryosu çalıştırmak isteyebileceğinizden bu betiği açık tutun.
1. Beşten az kiracınız varsa daha ilgi çekici bir izleme bağlamı sağlamak için bir kiracı grubu sağlayın:
   1. **$DemoScenario = 1,** **Kiracı grubu sağlama** değerini ayarlayın
   1. Betiği çalıştırmak için **F5**'e basın.

1. **$DemoScenario** = 2, **Normal yoğunlukta yük (yaklaşık 40 DTU) oluşturma**’yı ayarlayın.
1. Betiği çalıştırmak için **F5**'e basın.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Log Analytics’i ve Azure SQL Analytics çözümünü yükleme ve yapılandırma

Log Analytics, yapılandırılması gereken ayrı bir hizmettir. Log Analytics, log analytics çalışma alanında günlük veriler, telemetri ve ölçümleri toplar. Çalışma alanı, Azure’daki diğer kaynaklara benzer bir kaynaktır ve oluşturulması gerekir. Çalışma alanının izlediği uygulamayla aynı kaynak grubunda oluşturulması gerekli olmasa da bu, çoğu zaman en mantıklı seçimdir. WTP uygulaması söz konusu olduğunda bu, çalışma alanının yalnızca kaynak grubu silinerek uygulamayla kolayca silinmesini sağlar.

1. **PowerShell ISE**’de ...\\Öğrenme Modülleri\\Performans İzleme ve Yönetim\\Log Analytics\\*Demo-LogAnalytics.ps1* öğesini açın.
1. Betiği çalıştırmak için **F5**'e basın.

Bu noktada, Azure portalında (veya OMS portalında) Log Analytics’i açabilmeniz gerekir. Telemetrinin Log Analytics çalışma alanında toplanması ve görünür olması birkaç dakikayı bulabilir. Sistemin veri toplamasına ne kadar uzun süre izin verirseniz deneyim o kadar ilgi çekici olur. Şimdi gidip bir içecek alabilirsiniz. Yük oluşturucunun çalıştığından emin olmanız yeterli!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için Log Analytics ve SQL Analytics çözümünü kullanma


Bu alıştırmada, WTP veritabanları ve havuzları için toplanan telemetriyi incelemek amacıyla Log Analytics’i ve OMS portalını açın.

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

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)

