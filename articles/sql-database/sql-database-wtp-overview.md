---
title: "Bir kutuda SaaS (Azure SQL Veritabanı’nı kullanan örnek SaaS uygulaması) | Microsoft Belgeleri"
description: "SQL Veritabanı’nı kullanan SaaS uygulamaları oluşturma"
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
ms.openlocfilehash: bf5745a788cd9ab6bf2ea8d5d97b8c04f083fc5d
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-the-wingtip-tickets-platform-wtp-sample-saas-application"></a>Wingtip Bilet Platformu (WTP) örnek SaaS uygulamasına giriş

Wingtip Bilet Platformu (WTP) SaaS uygulaması, SQL Veritabanı’nın benzersiz avantajlarını gösteren bir örnek çok kiracılı uygulamadır. Uygulama, birden fazla kiracıya hizmet vermek için SaaS uygulama düzeni olan kiracı başına veritabanını kullanır. WTP uygulaması, Azure SQL Veritabanı’nın SaaS senaryolarını (SaaS tasarımı ve yönetimi düzenleri) mümkün kılan özelliklerini göstermek için tasarlanmıştır. Hızla çalışmaya başlamanız için [WTP uygulaması beş dakikadan daha kısa sürede dağıtılır](sql-database-saas-tutorial.md)!

WTP uygulamasını dağıttıktan sonra, ilk dağıtımın gösterildiği [öğretici koleksiyonunu](#sql-database-saas-tutorials) keşfedin. Her öğretici, SaaS uygulamalarında uygulanan genel görevlere odaklanır. Görevler, SQL Veritabanı’nın yerleşik özelliklerinden yararlanan SaaS düzenleri kullanılarak uygulanır. Açıklanan düzenlere; yeni kiracılar sağlama, kiracı veritabanlarını geri yükleme, tüm kiracılar genelinde dağıtılmış sorgular çalıştırma ve tüm kiracı veritabanlarına şema değişikliklerini kullanıma sunma dahildir. Her öğretici, SaaS yönetim düzenlerini anlamanızı ve uygulamalarınızda aynılarını kullanmanızı büyük ölçüde kolaylaştıran ayrıntılı açıklamalar içeren yeniden kullanılabilir betikler içerir.

WTP uygulaması bir örnek uygulama kadar eksiksiz ve ilgi uyandırıcı olsa da veri katmanıyla ilişkili olduklarından temel SaaS düzenlerine odaklanmak önem taşır. Başka bir deyişle, veri katmanına odaklanın ve uygulamanın kendisini gereğinden fazla analiz etmeyin. Bu temel SaaS düzenlerinin nasıl uygulandığını anlamanız, bu düzenleri uygulamalarınıza kendi iş gereksinimlerinize yönelik gerekli değişiklikleri göz önünde bulundurarak uygulamanız için kritik bir öneme sahiptir.



## <a name="application-architecture"></a>Uygulama mimarisi

WTP uygulaması, verimliliği en üst düzeye çıkarmak için kiracı başına veritabanı düzenini ve SQL elastik havuzlarını kullanır.
Yönetim ve bağlantı sağlamak için kiracı kataloğu kullanımı.
Tümleşik uygulama, havuz, veritabanı izleme ve uyarma (OMS).
Kiracılar arası şema ve başvuru verileri yönetimi (elastik veritabanı işleri).
Kiracılar arası sorgu, işlem analizi (esnek sorgu).
Genişletilmiş erişim için coğrafi olarak dağıtılmış veriler kullanma.
Kendi kendine oluşan hatalardan kurtarmak için iş sürekliliği Tek kiracılı kurtarma (PITR) Ölçekli DR (coğrafi geri yükleme, coğrafi çoğaltma, otomatik DR) Kiracı self servis yönetimi (Yönetim API'leriyle) PITR.

Temel Wingtip uygulaması, üç örnek kiracı bulunan bir havuz ve katalog veritabanı kullanır.

![WTP mimarisi](media/sql-database-wtp-overview/wtp-architecture.png)


## <a name="sql-database-wtp-saas-tutorials"></a>SQL Veritabanı WTP SaaS öğreticileri

Aşağıdaki öğreticiler, [Wingtip Bilet Platformu SaaS uygulama örneğinin](sql-database-saas-tutorial.md) ilk dağıtımına yöneliktir:

| Alan | Açıklama | Betik konumu |
|:--|:--|:--|
|[Kiracıları sağlama ve kataloğa kaydetme öğreticisi](sql-database-saas-tutorial-provision-and-catalog.md)| Yeni kiracılar sağlama ve bunları kataloğa kaydetme | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Provision%20and%20Catalog) |
|[Performansı izleme ve yönetme öğreticisi](sql-database-saas-tutorial-performance-monitoring.md)| Veritabanı ve havuz performansını izleme ve yönetme | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management) |
|[Tek bir kiracıyı geri yükleme öğreticisi](sql-database-saas-tutorial-restore-single-tenant.md)| Kiracı veritabanlarını geri yükleme | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Business%20Continuity%20and%20Disaster%20Recovery/RestoreTenant) |
|[Kiracı şemasını yönetme öğreticisi](sql-database-saas-tutorial-schema-management.md)| Tüm kiracılar genelinde sorgu yürütme  | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Schema%20Management) |
|[Geçici analiz çalıştırma öğreticisi](sql-database-saas-tutorial-adhoc-analytics.md) | Geçici analiz veritabanı oluşturma ve tüm kiracılar genelinde sorgu çalıştırma  | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Adhoc%20Analytics) |
|[Log Analytics (OMS) ile Yönetme öğreticisi](sql-database-saas-tutorial-log-analytics.md) | Log Analytics’i yapılandırma ve keşfetme | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management/LogAnalytics) |
|[Kiracı analizi çalıştırma öğreticisi](sql-database-saas-tutorial-tenant-analytics.md) | Kiracı analiz sorguları oluşturma ve çalıştırma | [Github'da betikler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Tenant%20Analytics) |

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="working-with-the-wtp-powershell-scripts"></a>WTP PowerShell Betikleriyle Çalışma

Sağlanan betikleri ayrıntılı olarak incelemek ve farklı SaaS düzenlerinin nasıl uygulandığını incelemek, WTP uygulamasıyla çalışmanın avantajları arasında yer alır.

Sağlanan betikleri ve modülleri görüntülemek ve daha iyi anlamak amacıyla bunlarda ilerlemeyi kolaylaştırmak için [Windows PowerShell ISE’yi](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise) kullanın. Önünde *Demo-* ifadesi bulunan betiklerin çoğu çalıştırılmadan önce değiştirebileceğiniz değişkenler içerdiğinden, PowerShell ISE’nin kullandığınızda bu betiklerle daha kolay çalışabilirsiniz.

Her WTP uygulama dağıtımında, dağıtım sırasında tanımladığınız kaynak grubunu ve kullanıcı adı değerlerini ayarlamak için iki parametre içeren **UserConfig.psm1** dosyası bulunur. Dağıtım tamamlandıktan sonra, _ResourceGroupName_ ve _Name_ parametrelerini ayarlayarak **UserConfig.psm1** modülünü düzenleyin. Bu değerler, başarılı şekilde çalışmak üzere diğer betikler tarafından kullanılır, bu nedenle dağıtım tamamlandığında bunların ayarlanması önerilir!



### <a name="execute-scripts-by-pressing-f5"></a>F5’e basarak Betikleri çalıştırma

Birçok betik, klasörlerde gezinmeye imkan tanımak için *$PSScriptRoot*’u kullanır ve bu değişken, yalnızca betik **F5** tuşuna basılarak çalıştırıldığında değerlendirilir.  Bir seçimin vurgulanması ve çalıştırılması (**F8**), hatalara yol açabilir. Bu nedenle, WTP betiklerini çalıştırırken **F5**’e basın.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Uygulamayı incelemek üzere betiklerde ilerleme

Betikleri incelemenin en büyük avantajı, ne gibi işlemler gerçekleştirdiklerini görmek üzere betiklerde ilerlemektir. Her bir görevi gerçekleştirmek üzere gereken adımları gösteren okunması kolay üst düzey bir iş akışı sağlayan birinci düzey _Demo-_ betiklerini gözden geçirin. Farklı SaaS düzenlerinin uygulama ayrıntılarını görmek için çağrıları tek tek ayrıntılı olarak inceleyin.

PowerShell betikleriyle çalışmaya [ve bu betiklerde hata ayıklamaya](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise) yönelik ipuçları:

* PowerShell ISE’de demo- betiklerini açıp yapılandırın.
* Çalıştırın veya **F5** ile devam edin. Betik seçimleri çalıştırılırken *$PSScriptRoot* değerlendirilmediğinden **F8**’in kullanılması önerilmez.
* Bir çizgiye tıklayarak veya çizgiyi seçerek ve **F9**’a basarak kesme noktaları yerleştirin.
* **F10**’u kullanarak bir işlev veya betiği atlayın.
* **F11**’i kullanarak bir işlev veya betiğe gidin.
* **Shift + F11**’i kullanarak geçerli işlev veya betikten çıkın.




## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Veritabanı şemasını keşfetme ve SSMS kullanarak SQL sorguları yürütme

WTP sunucularını ve veritabanlarını bağlamak ve bu sunuculara göz atmak için [SQL Server Management Studio’yu (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanın.

WTP örnek uygulamasının başlangıçta bağlanacak iki SQL Veritabanı sunucusu vardır: *tenants1* ve *katalog* sunucusu:


1. *SSMS*’yi açın ve *tenants1-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.
2. **Bağlan** > **Veritabanı Altyapısı...**:

   ![katalog sunucusu seçeneğine tıklayın](media/sql-database-wtp-overview/connect.png)

1. Tanıtım kimlik bilgileri: Kullanıcı adı = *developer*, Parola = *P@ssword1*

   ![bağlantı](media\sql-database-wtp-overview\tenants1-connect.png)

1. 2-3. adımları tekrarlayın ve *catalog-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.

Başarıyla bağlandıktan sonra her iki sunucuyu da görmeniz gerekir. Sağladığınız kiracı sayısına bağlı olarak daha fazla veya daha az veritabanı görebilirsiniz:

![nesne gezgini](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar

[Wingtip Bilet SaaS uygulaması örneği dağıtma](sql-database-saas-tutorial.md)
