---
title: Çok kiracılı SaaS uygulamasında parçalı bir çok kiracılı Azure SQL veritabanının performansını izleme | Microsoft Docs
description: Çok kiracılı SaaS uygulamasında parçalı çok kiracılı Azure SQL veritabanının performansını izleyin ve yönetin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: be7dbe35800bbe911bc56d1883462534a16499a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485632"
---
# <a name="monitor-and-manage-performance-of-sharded-multi-tenant-azure-sql-database-in-a-multi-tenant-saas-app"></a>Çok kiracılı SaaS uygulamasında parçalı çok kiracılı Azure SQL veritabanının performansını izleyin ve yönetin

Bu öğreticide, SaaS uygulamalarında kullanılan birkaç temel performans yönetimi senaryosu incelenmektedir. Parçalı çok kiracılı veritabanı arasında etkinliği benzetimi için bir yük oluşturucuyu kullanarak, yerleşik izleme ve uyarı özelliklerini SQL veritabanı'nın gösterilmiştir.

Wingtip bilet SaaS çok kiracılı veritabanı uygulama, mekan (Kiracı) veri potansiyel olarak birden fazla veritabanında Kiracı Kimliğine göre dağıtıldığı bir parçalı çok kiracılı veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım düzeninden yararlanmak için veritabanları çözüm maliyetini en iyi duruma getirmek için yukarı ve aşağı ölçeklendirilebilir. Bu düzen türü ile yükü birden çok veritabanı arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı kaynak kullanımını izlemek önemlidir. Ayrıca ayrı ayrı veritabanları için yeterli kaynağa sahip ve ulaşmadığından emin olmak gerekir, [DTU](sql-database-purchase-models.md#dtu-based-purchasing-model) sınırlar. Bu öğretici, veritabanları ve iş yükündeki değişikliklere yanıt düzeltici eylemlerde nasıl izleyebilir ve yönetebilirsiniz yolları açıklar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * Sağlanan bir yük oluşturucuyu çalıştırarak parçalı bir çok kiracılı veritabanında kullanım benzetimi
> * Yükteki artışa yanıt veren veritabanı izleyin
> * Artan veritabanı yüküne yanıt olarak veritabanını ölçeklendirme
> * Tek kiracılı veritabanına bir kiracı sağlama

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS çok kiracılı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip bilet SaaS çok kiracılı veritabanı uygulama](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetimi düzenlerine giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. 

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* Performansı el ile izlemek zorunda kalmamak için en etkili yöntemdir **veritabanları normal aralıkların dışına çıktığında, tetikleyicinin uyarıları**.
* Kısa vadeli dalgalanmalara bir veritabanı için işlem boyutu yanıt **DTU düzeyde ölçeklenebilen yukarı veya aşağı**. Bu dalgalanma düzenli veya öngörülebilir aralıklarla üzerinde oluşursa **otomatik olarak gerçekleşecek şekilde veritabanını ölçeklendirme zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Yanıt vermek için uzun vadeli dalgalanmalara ya da değişiklikler, kiracılar **bireysel kiracılar, başka bir veritabanına taşınabilir**.
* Kısa süreli bir artış yanıt *bireysel* Kiracı yük **bireysel kiracılar bir veritabanından alınan ve atanmış bir tek işlem boyutu**. Kiracı, sonra Yük düşürülür sonra çok kiracılı veritabanına döndürülebilir. Bunu önceden biliniyorsa, kiracılar bilindiğinde veritabanının her zaman gerekli kaynaklara sahip sağlamak ve diğer kiracılar çok kiracılı veritabanında üzerindeki etkiyi önlemek için taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL veritabanı için izleme ve uyarı veritabanlarında kullanılabilir. Bu yerleşik izleme ve uyarı özellikleri kaynağa özeldir, az sayıda kaynak için kullanılması kolaydır ancak çok sayıda kaynakla çalışırken kullanışlı olmaz.

Burada çalıştığınızı çok sayıda kaynakla, yüksek hacimli senaryolar için [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) kullanılabilir. Yayınlanan tanılama günlükleri ve Log Analytics çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmeti budur. Azure İzleyici günlüklerine birçok hizmetten telemetri toplayabilir ve sorgu ve uyarıları ayarlamak için kullanılabilir.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip bilet SaaS çok kiracılı veritabanı uygulama kaynak kodu ve betikleri Al

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Bu öğretici nasıl performans izleme ve yönetim ölçeklenerek iyi anlamak için birden çok kiracının parçalı bir çok kiracılı veritabanına sahip olmanızı gerektirir.

Önceki bir öğreticide zaten bir kiracı grubu sağladıysanız, atlamak [tüm Kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme](#simulate-usage-on-all-tenant-databases) bölümü.

1. İçinde **PowerShell ISE**açın... \\Öğrenme modülleri\\performans izleme ve Yönetim\\*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. **$DemoScenario** = **1**, _Kiracı grubu sağlama_ değerini ayarlayın
1. Betiği çalıştırmak için **F5**'e basın.

Betik, çok kiracılı veritabanına birkaç dakika içinde 17 Kiracı dağıtır. 

*New-TenantBatch* betiği, yeni kiracılar ile benzersiz Kiracı anahtarları parçalı çok kiracılı veritabanı içinde oluşturur ve bunları Kiracı adı ve mekan türü ile başlatır. Bu uygulamayı yeni bir kiracı sağlama yöntemiyle tutarlıdır. 

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo-PerformanceMonitoringAndManagement.ps1* , çok kiracılı veritabanında çalışan bir iş yükünün benzetimini gerçekleştiren sağlanan komut dosyasıdır. Yükleme, kullanılabilir yük senaryoları biri kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunlukta yük (yaklaşık 30 DTU) oluşturma |
| 3 | Kiracı başına daha uzun artışları ile yük oluşturma|
| 4 | Yük (yaklaşık olarak 70 DTU) Kiracı başına daha yüksek dtu ile oluşturma|
| 5 | Yüksek yoğunluk (yaklaşık 90 DTU) tek bir kiracının artı tüm diğer kiracıların bir normal yoğunlukta yük oluştur |

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (Dtu), süresi ve aralıkları tüm veritabanlarında öngörülemez Kiracı etkinliğini benzetimi farklılık gösterir.

1. İçinde **PowerShell ISE**açın... \\Öğrenme modülleri\\performans izleme ve Yönetim\\*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Ayarlama **$DemoScenario** = **2**, _normal yoğunlukta yük_
1. Tuşuna **F5** tüm kiracılar için yük uygulamak için.

Wingtip bilet SaaS çok kiracılı veritabanı bir SaaS uygulamasıdır ve SaaS uygulamasındaki gerçek Yük genellikle düzensiz ve tahmin edilemezdir. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Birkaç dakika çıkması yük düzeni için bu nedenle aşağıdaki bölümlerde yükü izlemeye çalışmadan önce 3-5 dakika için yük oluşturucuyu çalıştırmak gereklidir.

> [!IMPORTANT]
> Yük Oluşturucu, bir dizi yeni bir PowerShell penceresi iş olarak çalışıyor. Oturumu kapatırsanız yük Oluşturucu durur. Yük Oluşturucu kaldığı bir *iş çağırma* ürettiği nereye yük oluşturucunun başlatıldıktan sonra sağlanan tüm yeni kiracılara durumu. Kullanım *Ctrl-C* yeni işleri yürütmesini durdurmak ve betik çıkın. Yük Oluşturucu, ancak yalnızca var olan kiracılar üzerinde çalışmaya devam eder.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Uygulanmakta olan yükün neden olduğu kaynak kullanımını izlemek için çok kiracılı veritabanı portalda açın **tenants1**, kiracılar içeren:

1. Açık [Azure portalında](https://portal.azure.com) ve sunucuya Gözat *tenants1-mt -&lt;kullanıcı&gt;* .
1. Aşağı kaydırın ve veritabanları bulup tıklayın **tenants1**. Şu ana kadar oluşturulan tüm kiracılar Bu parçalı çok kiracılı veritabanı içerir.

![Veritabanı şeması](./media/saas-multitenantdb-performance-monitoring/multitenantdb.png)

Gözlemleyin **DTU** grafiği.

## <a name="set-performance-alerts-on-the-database"></a>Veritabanı Performans uyarılarını ayarlama

Uyarı üzerinde tetikleyen veritabanında ayarlama \>aşağıdaki gibi % 75 Kullanım:

1. Açık *tenants1* veritabanı (üzerinde *tenants1-mt -&lt;kullanıcı&gt;*  server) içinde [Azure portalında](https://portal.azure.com).
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-multitenantdb-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = DTU yüzdesi**
   * **Koşul = büyüktür**
   * **Eşik = 75**.
   * **Son 30 dakika boyunca süresi =**
1. Bir e-posta adresine ekleme *ek yönetici email(s)* kutusuna ve tıklatın **Tamam**.

   ![uyarı ayarlama](media/saas-multitenantdb-performance-monitoring/set-alert.png)

## <a name="scale-up-a-busy-database"></a>Meşgul bir veritabanının ölçeğini

Yük düzeyi, bir veritabanında veritabanının ölçeğini yükselir ve % 100 DTU kullanımına ulaşırsa noktasına artarsa, büyük olasılıkla sorgusu yanıt sürelerini yavaşlatmasını veritabanı performansı etkilenir.

**Kısa vadeli**, ek kaynak sağlamak için veritabanını ölçeklendirme veya kiracılar (tek başına bir veritabanı için çok kiracılı veritabanı dışına taşımak) çok kiracılı veritabanından kaldırılıyor göz önünde bulundurun.

**Uzun vadeli**, sorguları iyileştirmeyi düşünün veya veritabanı performansını artırmak için kullanım dizin. Uygulamanın duyarlılık performansına bağlı olarak % 100 DTU kullanımı ulaşmadan önce bir veritabanının ölçeğini için en iyi verir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yükü artırarak meşgul veritabanı benzetimini yapabilirsiniz. Artık, daha sık ve için veri bloğu kiracılar, çok kiracılı veritabanı üzerindeki yükü bireysel kiracılar gereksinimlerini değiştirmeden artırma neden oluyor. Veritabanını ölçeklendirme portalda veya PowerShell üzerinden kolayca gerçekleştirilebilir. Bu alıştırmada portal kullanılmaktadır.

1. Ayarlama *$DemoScenario* = **3**, _veritabanı başına daha uzun ve daha sık artışları ile yük oluşturma_ üzerinde toplam yükün yoğunluğunu artırmak için Her Kiracı tarafından gerekli en büyük yükü değiştirmeden veritabanı.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.
1. Git **tenants1** Azure portalında veritabanı.

DTU kullanımı üst grafikten artan veritabanı izleyin. İstiyorsanız, yeni yüksek yükleme birkaç dakika sürer ancak hızlı bir şekilde en yüksek kullanımı isabet başlangıç veritabanı görmeniz gerekir ve hızlı bir şekilde yük yeni düzene uyum gereğinden fazla doldurur gibi veritabanı aşırı.

1. Veritabanının ölçeğini için tıklatın **fiyatlandırma Katmanı (Ölçek Dtu)** ayarlar dikey penceresindeki.
1. Ayarlama **DTU** ayarını **100**. 
1. Tıklayın **Uygula** veritabanı ölçeklendirme isteği göndermek için.

Geri Git **tenants1** > **genel bakış** izleme grafiklerini görüntülemek için. (Birkaç kiracılar ve rastgele bir yük ile her zaman biraz zaman çalıştırana kadar yaratacağı görmek kolay değildir rağmen) veritabanı ile daha fazla kaynak sağlama etkisini izleyin. Aklınızda grafikleri ayı % 100 Baktığınız sırada alt grafikteki % 100 devam 50 Dtu olduğu üst grafikten artık 100 Dtu'yu temsil eder.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Uygulama kodu her zaman kesilen bağlantıları yeniden yazılması gerekir ve bu nedenle veritabanına yeniden bağlanılır.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama 

Parçalı çok kiracılı model diğer kiracıların yanı sıra çok kiracılı veritabanında yeni bir kiracı sağlama veya kendi veritabanındaki Kiracı sağlama seçmenize olanak sağlar. Bir kiracı kendi veritabanında sağlayarak, böylece diğerleri bağımsız olarak bu kiracının performansını yönetmek için diğerlerinin bağımsız olarak bu kiracıyı geri yükleme ayrı bir veritabanı'ndaki yalıtım faydalanır vs. Örneğin, çok kiracılı veritabanı müşteriler ücretsiz deneme sürümü veya normal ve premium müşterileri bulunan tek veritabanlarını koymak isteyebilirsiniz.  Yalıtılmış tek kiracılı veritabanı oluşturduysanız, bunlar yine de topluca kaynak maliyetleri iyileştirmek için bir elastik havuzda yönetilebilir.

Yeni bir kiracı kendi veritabanında zaten sağlanmış, sonraki birkaç adımı atlayın.

1. İçinde **PowerShell ISE**açın... \\Öğrenme modülleri\\ProvisionTenants\\*tanıtım ProvisionTenants.ps1*. 
1. Değiştirme **$TenantName "Salix Salsa" =** ve **$VenueType "dans" =**
1. Ayarlama **$Scenario** = **2**, _tek kiracılı veritabanında yeni bir kiracı sağlama_
1. Betiği çalıştırmak için **F5**'e basın.

Betik bu kiracıya ayrı bir veritabanı sağlama, veritabanı ve Kiracı kataloğa kaydetmek ve ardından kiracının olayları sayfası tarayıcıda açın. Olay hub'ı sayfayı yenileyin ve "Salix Salsa" bir mekan eklenmiş olan görürsünüz.

## <a name="manage-performance-of-an-individual-database"></a>Tek veritabanı performansını yönetme

Tek bir kiracı içinde bir çok kiracılı veritabanı sürekli yüksek bir yük karşılaşırsa, veritabanı kaynaklarının baskındır ve diğer kiracılar aynı veritabanında etkisi eğilimli olabilir. Etkinlik için bir süre devam etme olasılığı ise, Kiracı geçici veritabanı dışına ve kendi tek kiracılı veritabanına taşınabilir. Bu, Kiracı ek kaynaklara ihtiyaç duyar ve tam olarak, diğer kiracılardan ayırır sahip sağlar.

Bu alıştırmada Salix biletler satışa popüler bir etkinlik için gittiğinizde, yüksek bir yükle karşılaşan Salsa etkisini benzetimini yapar.

1. Aç... \\ *Demo-PerformanceMonitoringAndManagement.ps1* betiği.
1. Ayarlama **$DemoScenario = 5**, _normal yük ve yoğun yük (yaklaşık 90 DTU) tek bir kiracı üzerinde oluşturur._
1. Ayarlama **$SingleTenantName Salix Salsa =**
1. **F5**’i kullanarak betiği yürütün.

Portala gidin ve gidin **salixsalsa** > **genel bakış** izleme grafiklerini görüntülemek için. 

## <a name="other-performance-management-patterns"></a>Diğer performans yönetimi düzenleri

**Kiracı Self-Servis ölçeklendirme**

Ölçeklendirme görev bir kolayca olduğu için yönetim API'si, aracılığıyla adlı kolayca kiracıya yönelik uygulamanıza Kiracı veritabanlarını ölçeklendirme özelliğini yapı ve SaaS hizmetinizin bir özelliği olarak sunar. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Bir veritabanını kullanım düzenlerine uygun bir zamanlamaya göre yukarı ve aşağı ölçeklendirme**

Toplu Kiracı kullanımının öngörülebilir kullanım düzenlerini takip ettiği durumlarda, bir veritabanı bir zamanlamaya göre ölçeğini artırma ve azaltma için Azure Automation'ı kullanabilirsiniz. Örneğin, bir veritabanı 6 pm sonra de ölçeklendirmeyi azaltın ve yukarı 6 bildiğinizde hafta önce yeniden yok açılan kaynak gereksinimleri.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sağlanan bir yük oluşturucuyu çalıştırarak parçalı bir çok kiracılı veritabanında kullanım benzetimi
> * Yükteki artışa yanıt veren veritabanı izleyin
> * Artan veritabanı yüküne yanıt olarak veritabanını ölçeklendirme
> * Tek kiracılı veritabanına bir kiracı sağlama

## <a name="additional-resources"></a>Ek kaynaklar

<!--* [Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Azure otomasyonu](../automation/automation-intro.md)