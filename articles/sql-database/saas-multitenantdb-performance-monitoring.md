---
title: Bir çok kiracılı SaaS uygulamasında parçalı bir çok kiracılı Azure SQL veritabanı performansını izleme | Microsoft Docs
description: İzleme ve bir çok kiracılı SaaS uygulamasında parçalı çok kiracılı Azure SQL veritabanı performansını yönetme
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 43bac88a7ab6320c5fdcc9dc0fb6b5209bdbcaa3
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-and-manage-performance-of-sharded-multi-tenant-azure-sql-database-in-a-multi-tenant-saas-app"></a>İzleme ve bir çok kiracılı SaaS uygulamasında parçalı çok kiracılı Azure SQL veritabanı performansını yönetme

Bu öğretici kapsamında, SaaS uygulamalarında kullanılan birkaç önemli performans yönetim senaryolarını ele alınan dizelerle. Etkinlik parçalı çok Kiracı veritabanları arasında benzetimini yapmak için bir yük oluşturucuyu kullanarak yerleşik izleme ve SQL veritabanı özelliklerini uyarı gösterilmiştir.

Wingtip biletleri SaaS çok Kiracı veritabanı uygulama salonundan (Kiracı) veri Kiracı kimliği tarafından birden çok veritabanı arasında dağıtılacağı yeri parçalı çok kiracılı veri modelini kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım deseni yararlanmak için veritabanları bir çözüm maliyetini en iyi duruma getirmek için yukarı ve aşağı genişletilebilir. Bu desen türüyle yüklerini birden çok veritabanı arasında makul dengeli emin olmak için veritabanı kaynak kullanımını izlemek önemlidir. Ayrıca tek tek veritabanlarının yeterli kaynaklara sahip olmayan devreyi emin olun ve gerek kendi [DTU](sql-database-what-is-a-dtu.md) sınırlar. Bu öğreticiyi izlemek ve veritabanları ve iş yükü Çeşitlemeler yanıtta düzeltici işlemleri nasıl yönetmek için yol araştırır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Sağlanan yük Oluşturucu çalıştırarak parçalı çok Kiracı veritabanı kullanımı benzetimi
> * Yük arttıkça yanıt olarak veritabanını izleyin
> * Daha fazla veritabanı yükünü yanıta veritabanında ölçeklendirin
> * Bir kiracı tek Kiracı veritabanına sağlama

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama keşfedin.](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetim desenleri giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. 

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* El ile performansını izlemek zorunda kalmamak için en etkili **veritabanları dışında normal aralıkları parazit durumlarda tetikleyen uyarıları ayarlama**.
* Bir veritabanı performans düzeyi kısa vadeli dalgalanmalara yanıt verecek şekilde **DTU düzeyi ölçeklendirilmiş yukarı veya aşağı**. Bu dalgalanmayı normal veya öngörülebilir bir temelinde oluşursa **otomatik olarak gerçekleşecek şekilde veritabanı ölçekleme zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Yanıt için daha uzun vadeli dalgalanmaları ya da kiracılar değişiklikleri **tek tek kiracıların diğer veritabanına taşınabilir**.
* Kısa vadeli artışlar yanıt verecek şekilde *tek tek* Kiracı yük **tek tek kiracılar bir veritabanından alınır ve bir bireysel performans düzeyi atanan**. Yükü daha azdır sonra Kiracı sonra çok Kiracı veritabanı için döndürülebilir. Bu önceden biliniyorsa, kiracılar pre-emptively veritabanı her zaman, gereken kaynak bulunduğundan emin olun ve diğer kiracılar çok Kiracı veritabanı üzerindeki etkiyi önlemek için taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL veritabanı için izleme ve uyarma veritabanlarında kullanılabilir. Bu yerleşik izleme ve uyarma olduğundan kaynak özgü kaynakları küçük sayılar için uygundur ancak birçok kaynaklarla çalışırken kullanışlı değildir.

Burada çalıştığınız birçok kaynaklarla, yüksek hacimli senaryolar için [günlük analizi](https://azure.microsoft.com/services/log-analytics/) kullanılabilir. Verilmiş tanılama günlüklerini ve günlük analizi çalışma alanındaki toplanan telemetri üzerinden analytics sağlayan ayrı bir Azure hizmet budur. Günlük analizi telemetri birçok Hizmetleri'nden toplayabilir ve sorgu ve uyarıları ayarlamak için kullanılır.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip biletleri SaaS çok Kiracı veritabanı uygulama kaynak koduna ve komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Performans izleme ve yönetim işleyişi ölçekte iyi anlamak için bu öğreticiyi parçalı bir çok kiracılı veritabanında birden çok kiracıya sahip olmanızı gerektirir.

Önceki bir öğreticide, kiracılar toplu zaten sağladıysanız geçin [tüm Kiracı veritabanları benzetim kullanımı](#simulate-usage-on-all-tenant-databases) bölümü.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\performans izleme ve Yönetim\\*Demo PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. **$DemoScenario** = **1**, _Kiracı grubu sağlama_ değerini ayarlayın
1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyası 17 kiracılar çok kiracılı veritabanına birkaç dakika içinde dağıtır. 

*Yeni TenantBatch* komut dosyası anahtarları parçalı çok Kiracı veritabanı içinde benzersiz Kiracı ile yeni kiracılar oluşturur ve Kiracı adı ve salonundan türü ile başlatır. Bu uygulamayı yeni bir kiracı hazırlar yolu ile tutarlıdır. 

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo PerformanceMonitoringAndManagement.ps1* çok kiracılı veritabanına karşı çalışan bir iş yükünü benzetim sağlanan komut dosyasıdır. Yükü, kullanılabilir yük senaryolardan biri, kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunluğu yük oluşturmak (yaklaşık 30 DTU) |
| 3 | Kiracı başına uzun WINS'e yüküyle oluştur|
| 4 | Yük Kiracı başına daha yüksek DTU WINS'e ile oluştur (yaklaşık 70 DTU)|
| 5 | Yüksek yoğunluk oluştur (yaklaşık 90 DTU) tek bir kiracı artı normal yoğunluğu yük diğer tüm kiracılar |

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (Dtu'lar), süresini ve aralıklarını öngörülemeyen Kiracı etkinlik benzetimi tüm veritabanları arasında farklılık gösterir.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\performans izleme ve Yönetim\\*Demo PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Ayarlama **$DemoScenario** = **2**, _Generate normal yoğunluğu yükleme_
1. Tuşuna **F5** tüm kiracılar için yük uygulanacak.

Wingtip biletleri SaaS çok kiracılı bir SaaS uygulaması veritabanıdır ve bir SaaS uygulaması gerçek yükü genellikle durumlarıyla ve tahmin edilemez. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Birkaç dakika kadar 3-5 dakika aşağıdaki bölümlerde yük izlemek denemeden önce yük oluşturucuyu çalıştırmak çıkmaya, yük düzeni için gereklidir.

> [!IMPORTANT]
> Yük Oluşturucu, yeni bir PowerShell penceresi işlerinde birtakım olarak çalışıyor. Oturumu kapatırsanız, yükleme Oluşturucu durdurur. Yük Oluşturucu kaldığı bir *iş çağırma* ürettiği burada oluşturucunun başlatıldıktan sonra sağlanan yeni kiracılar yük durumu. Kullanım *Ctrl-C* yeni işleri çağırma durdurun ve komut dosyası çıkmak için. Yük Oluşturucu, ancak yalnızca var olan kiracılar çalışmaya devam eder.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Uygulanmakta yüklerinin sonuçları kaynak kullanımını izlemek için çok Kiracı veritabanı portalına açmak **tenants1**, kiracılar içeren:

1. Açık [Azure portal](https://portal.azure.com) ve Gözat sunucuya *tenants1-mt -&lt;kullanıcı&gt;*.
1. Aşağı kaydırın ve veritabanlarını bulun ve tıklatın **tenants1**. Bu parçalı çok Kiracı veritabanı şu ana kadar oluşturulan tüm kiracılar içerir.

![Veritabanı şeması](./media/saas-multitenantdb-performance-monitoring/multitenantdb.png)

Gözlemlemek **DTU** grafik.

## <a name="set-performance-alerts-on-the-database"></a>Veritabanı performans uyarıları ayarlayın

Üzerinde tetikler veritabanında bir uyarı ayarlamak \>şekilde % 75 kullanımı:

1. Açık *tenants1* veritabanı (üzerinde *tenants1-mt -&lt;kullanıcı&gt;*  sunucu) içinde [Azure portal](https://portal.azure.com).
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-multitenantdb-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = DTU yüzdesi**
   * **Koşul büyük =**
   * **Eşik = 75**.
   * **Son 30 dakika boyunca süresi =**
1. Bir e-posta adresi ekleyin *ek yönetici email(s)* kutusuna ve tıklatın **Tamam**.

   ![uyarı ayarlama](media/saas-multitenantdb-performance-monitoring/set-alert.png)

## <a name="scale-up-a-busy-database"></a>Meşgul bir veritabanını ölçeklendirme

Veritabanını maxes ve % 100 DTU kullanımı ulaşana point veritabanına yük düzeyini artırır varsa, olası sorgusu yanıt sürelerini yavaşlamasının veritabanı performansı etkilenir.

**Kısa vadeli**, ek kaynaklar sağlamak için veritabanını yedekleyin ölçeklendirme ya da kiracılar (tek başına bir veritabanına çok Kiracı veritabanı dışında taşımak) çok Kiracı veritabanı kaldırma göz önünde bulundurun.

**Uzun vadeli**, sorguları en iyi duruma getirme göz önünde bulundurun veya dizin veritabanı performansını iyileştirmek için kullanım. Uygulamanın duyarlılık performansına bağlı olarak, en iyi uygulama % 100 DTU kullanımı erişmeden önce bir veritabanı ölçeği verir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yük artırarak meşgul veritabanı benzetimini yapabilirsiniz. Daha sık ve için artık, veri bloğu kiracılar tek tek kiracılar gereksinimlerini değiştirmeden çok Kiracı veritabanı artırma neden oluyor. Veritabanını yedeklemek ölçeklendirme portal veya PowerShell kolayca yapılır. Bu alıştırmada portal kullanılmaktadır.

1. Ayarlama *$DemoScenario* = **3**, _Generate yük uzun ve daha sık WINS'e veritabanı başına ile_ toplama yük yoğunluğunu artırmanız için Her Kiracı tarafından gerekli yoğun yük değiştirmeden veritabanı.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.
1. Git **tenants1** Azure portalında veritabanı.

Artan veritabanı üst grafik DTU kullanımını izleyin. Kazandırın yeni daha fazla yük için birkaç dakika sürer ancak max kullanımı isabet Başlat veritabanı hızlı bir şekilde görmeniz gerekir ve yeni modele yük steadies gibi hızlı bir şekilde veritabanı overloads.

1. Veritabanını yedeklemek için ' i tıklatın **fiyatlandırma Katmanı (Ölçek Dtu'lar)** ayarlar dikey penceresinde.
1. Ayarlama **DTU** ayarını **100**. 
1. Tıklatın **Uygula** veritabanı ölçeklendirme isteği göndermek için.

Geri dönerek **tenants1** > **genel bakış** izleme grafikleri görüntülemek için. (Birkaç kiracılar ve rastgele yük ile her zaman biraz zaman çalıştırana kadar conclusively görmek kolay değildir rağmen) veritabanı ile daha fazla kaynak sağlama etkisini izleyin. Grafikler ayı aklınızda % 100 Baktığınız sırada alt grafikte % 100 hala 50 Dtu'lar olsa da üst grafik şimdi 100 Dtu'lar temsil eder.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Uygulama kodu her zaman bırakılan bağlantılarını yeniden yazılması ve böylece veritabanına yeniden bağlanır.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama 

Parçalı çok kiracılı bir model mi, yoksa diğer kiracılar yanında bir çok kiracılı veritabanında yeni bir kiracı sağlamak için kendi veritabanındaki Kiracı sağlayacak seçmenize olanak sağlar. Bir kiracı kendi veritabanında sağlama tarafından diğerleri bağımsız olarak, Kiracı performansını yönetmek için diğer bağımsız olarak, Kiracı geri sağlayarak ayrı veritabanındaki devralınmış yalıtım faydalanır vs. Örneğin, çok Kiracı veritabanı ücretsiz deneme veya normal müşteriler ve premium müşteriler bulunan tek veritabanlarını koymak isteyebilirsiniz.  Yalıtılmış tek Kiracı veritabanları oluşturduysanız, bunlar hala topluca kaynak maliyetleri en iyi duruma getirmek için bir esnek havuz yönetilebilir.

Yeni bir kiracı kendi veritabanında zaten sağlanan varsa, sonraki birkaç adımda atlayın.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\ProvisionTenants\\*Demo ProvisionTenants.ps1*. 
1. Değiştirme **$TenantName "Salix Salsa" =** ve **$VenueType "dans" =**
1. Ayarlama **$Scenario** = **2**, _yeni bir tek Kiracı veritabanında bir kiracı sağlama_
1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyası ayrı bir veritabanında bu Kiracı sağlamak, veritabanı ve Kiracı kataloğa kaydetmek ve kiracının Etkinlikler sayfasını tarayıcıda açın. Olay hub'ı sayfayı yenileyin ve "Salix Salsa" salonundan eklenen görürsünüz.

## <a name="manage-performance-of-a-single-database"></a>Tek bir veritabanının performansını yönetme

Çok Kiracı veritabanı içinde tek bir kiracı sürekli yüksek yük karşılaşırsa, veritabanı kaynakları baskındır ve diğer kiracılar aynı veritabanında etkileyebilir getirmeye eğilimlidir. Etkinlik için bir süre devam etmek olasılığı varsa, Kiracı geçici veritabanı dışında ve kendi tek Kiracı veritabanına taşınabilir. Bu gerekir ve tam olarak diğer kiracılardan yalıtır ek kaynaklar Kiracı sağlar.

Bu alıştırmada Salix bilet satış popüler bir olay için olduğunuzda yüksek yük yaşayan Salsa etkisini taklit eder.

1. Aç... \\ *Demo PerformanceMonitoringAndManagement.ps1* komut dosyası.
1. Ayarlama **$DemoScenario = 5**, _normal yükü artı tek bir kiracı üzerinde büyük bir yük oluşturur (yaklaşık 90 DTU)._
1. Ayarlama **$SingleTenantName Salix Salsa =**
1. **F5**’i kullanarak betiği yürütün.

Portalı'na gidin ve gidin **salixsalsa** > **genel bakış** izleme grafikleri görüntülemek için. 

## <a name="other-performance-management-patterns"></a>Diğer performans yönetim desenleri

**Kiracı Self Servis ölçeklendirme**

Ölçeklendirme göreve kolayca olduğundan yönetim API'si, adlı kolayca ölçeklendirmenizi Kiracı kullanıma yönelik uygulamanız Kiracı veritabanı oluşturmak ve SaaS hizmetiniz bir özellik olarak sunar. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Bir veritabanı yukarı ve aşağı kullanım desenlerini eşleştirmek için bir zamanlamaya göre ölçeklendirme**

Tahmin edilebilir kullanım desenlerini toplama Kiracı kullanım aşağıdaki durumlarda, bir veritabanı bir zamanlamaya göre yukarı ve aşağı doğru ölçeklendirme için Azure Otomasyonu kullanabilirsiniz. Örneğin, bir veritabanı 6 pm sonra de ölçeklendirmeyi azaltın ve yukarı 6 am bildiğinizde hafta önce yeniden yok açılan kaynak gereksinimleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sağlanan yük Oluşturucu çalıştırarak parçalı çok Kiracı veritabanı kullanımı benzetimi
> * Yük arttıkça yanıt olarak veritabanını izleyin
> * Daha fazla veritabanı yükünü yanıta veritabanında ölçeklendirin
> * Bir kiracı tek Kiracı veritabanına sağlama

## <a name="additional-resources"></a>Ek kaynaklar

<!--* [Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Azure otomasyonu](../automation/automation-intro.md)