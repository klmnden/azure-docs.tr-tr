---
title: 'Saas uygulama: Birçok Azure SQL veritabanı performansını izleme | Microsoft Docs'
description: Azure SQL veritabanlarının ve havuzların bir çok kiracılı SaaS uygulaması performansını izleme ve yönetme
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
ms.openlocfilehash: 075d0e2471457e1a585f7fdea9b523b1d13499c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388651"
---
# <a name="monitor-and-manage-performance-of-azure-sql-databases-and-pools-in-a-multi-tenant-saas-app"></a>Azure SQL veritabanlarının ve havuzların bir çok kiracılı SaaS uygulaması performansını izleme ve yönetme

Bu öğreticide, SaaS uygulamalarında kullanılan birkaç temel performans yönetimi senaryosu incelenmektedir. Tüm Kiracı veritabanlarında etkinliği benzetimi için bir yük oluşturucuyu kullanarak, yerleşik izleme ve uyarı özelliklerini SQL veritabanı ve elastik havuzların gösterilmiştir.

Wingtip bilet SaaS her Kiracı veritabanı uygulama, her mekanın (kiracının) kendi veritabanına sahip olduğu bir tek kiracılı veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım düzeninden yararlanmak için Kiracı veritabanları elastik havuzlarına dağıtılır. Elastik havuzlar, kaynakları çok sayıda veritabanı arasında paylaştırarak çözüm maliyetini en iyi duruma getirir. Bu düzen türü ile yüklerin havuzlar arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı ve havuz kaynak kullanımının izlenmesi önemlidir. Ayrıca, her bir veritabanının yeterli kaynağa sahip olduğundan ve havuzların [eDTU](sql-database-purchase-models.md#dtu-based-purchasing-model) sınırına ulaşmadığından emin olmanız gerekir. Bu öğreticide, veritabanı ve havuzları izleyip yönetme yollarını ve iş yükündeki değişikliklere yanıt olarak nasıl düzeltici işlem yapılacağını incelenmektedir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> 
> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yükünü dengelemek üzere ikinci bir Elastik havuz sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Wingtip bilet SaaS her Kiracı veritabanı uygulamayı keşfetme](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetimi düzenlerine giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. Birden çok kiracıyı barındırdığında, elastik havuzlar sağlamak ve öngörülemeyen iş yükleri ile veritabanı grubu için kaynakları yönetmek için uygun maliyetli bir yoldur. Bazı iş yükü düzenlerinde iki S3 veritabanı bile tek bir havuzda yönetilebilir.

![Uygulama diyagramı](./media/saas-dbpertenant-performance-monitoring/app-diagram.png)

Havuzlar ve havuzlardaki veritabanları, kabul edilebilir performans aralıkları dahilinde kalırlar emin olmak için izlenmelidir. Tüm veritabanları, havuz edtu'ları genel iş yükü için uygun olduğundan emin olmak, toplu iş yükü gereksinimlerini karşılamak için havuz yapılandırmasını ayarlayın. Veritabanı başına en düşük ve veritabanı başına en yüksek eDTU değerlerini, uygulamanıza özel gereksinimler için uygun değerlere ayarlayın.

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* Performansı el ile izlemek zorunda kalmamak için en etkili yöntemdir **veritabanı veya havuzlar normal aralıkların dışına çıktığında, tetikleyicinin uyarıları**.
* Bir havuzun işlem boyutu için kısa vadeli dalgalanmaları ve toplam yanıt vermek için **havuz eDTU düzeyde ölçeklenebilen yukarı veya aşağı**. Bu dalgalanma düzenli veya öngörülebilir aralıklarla gerçekleşiyorsa, **havuz ölçeklendirmesi otomatik olarak gerçekleşecek şekilde zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Daha uzun vadeli dalgalanmalara ya da veritabanı sayısındaki değişikliklere yanıt vermek için, **tek veritabanları diğer havuzlara taşınabilir**.
* Kısa süreli bir artış yanıt *bireysel* veritabanı yük **bağımsız veritabanları havuz dışına alınıp ve tek tek işlem boyutu atanan**. Yükü azaldıktan sonra veritabanını havuza döndürebilirsiniz. Bunu önceden biliniyorsa, veritabanları bilindiğinde veritabanının her zaman gerekli kaynaklara sahip olmak ve havuzdaki diğer veritabanlarını etkilemesini önlemek için taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarı özellikleri kaynağa özeldir, az sayıda kaynak için kullanılması kolaydır ancak çok sayıda kaynakla çalışırken kullanışlı olmaz.

Burada çalıştığınızı çok sayıda kaynakla, yüksek hacimli senaryolar için [Azure İzleyici günlükleri](saas-dbpertenant-log-analytics.md) kullanılabilir. Yayınlanan tanılama günlükleri ve Log Analytics çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmeti budur. Azure İzleyici günlüklerine birçok hizmetten telemetri toplayabilir ve sorgu ve uyarıları ayarlamak için kullanılabilir.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS her Kiracı veritabanı uygulama betiklerini alma

Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Havuzlar yalnızca iki adet S3 veritabanı ile uygun maliyetli olabilse de, havuzdaki veritabanı sayısı arttıkça ortalama etki daha uygun maliyetli olur. Performans izleme ve yönetiminin ölçeklenerek nasıl çalıştığını anlamak için bu öğreticide en az 20 veritabanınızın dağıtılmış olması gerekir.

Önceki bir öğreticide zaten bir kiracı grubu sağladıysanız atlamak [tüm Kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme](#simulate-usage-on-all-tenant-databases) bölümü.

1. İçinde **PowerShell ISE**açın... \\Öğrenme modülleri\\performans izleme ve Yönetim\\*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. **$DemoScenario** = **1**, **Kiracı grubu sağlama** değerini ayarlayın
1. Betiği çalıştırmak için **F5**'e basın.

Bu betik, beş dakikadan daha kısa bir süre içinde 17 kiracı dağıtır.

*New-TenantBatch* iç içe veya bağlı bir dizi betiği kullanır [Resource Manager](../azure-resource-manager/index.yml) veritabanını kopyalar, varsayılan olarak bir kiracı grubu, oluşturduğunuz şablonlar **basetenantdb**katalog sunucusuna yeni Kiracı veritabanları oluşturmak için ardından kataloğa kaydeden ve son olarak bunları Kiracı adı ve mekan türü ile başlatır. Bu uygulamayı yeni bir kiracı sağlama yöntemiyle tutarlıdır. Yapılan tüm değişiklikler *basetenantdb* sonra sağlanan tüm yeni kiracılara uygulanır. Bkz: [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md) değişiklik yapılacağını görmek için *mevcut* Kiracı veritabanlarında (dahil olmak üzere *basetenantdb* veritabanı).

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo-PerformanceMonitoringAndManagement.ps1* , tüm Kiracı veritabanlarına yönelik çalışan bir iş yükünün benzetimini gerçekleştiren sağlanan komut dosyasıdır. Yükleme, kullanılabilir yük senaryoları biri kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunlukta yük (yaklaşık 40 DTU) oluşturma |
| 3 | Veritabanı başına daha uzun ve daha sık artışla yük oluşturma|
| 4 | Yük (yaklaşık 80 DTU) veritabanı başına daha yüksek dtu ile oluşturma|
| 5 | Normal yük ve yoğun yük (yaklaşık 95 DTU) tek bir kiracı üzerinde oluşturma|
| 6 | Birden çok havuzda dengesiz yük oluşturma|

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (eDTU cinsinden), süresi ve aralıkları tüm veritabanlarında farklıdır ve öngörülemez kiracı etkinliğini benzetimi gerçekleştirir.

1. İçinde **PowerShell ISE**açın... \\Öğrenme modülleri\\performans izleme ve Yönetim\\*Demo-PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Ayarlama **$DemoScenario** = **2**, *normal yoğunlukta yük*.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

Wingtip bilet SaaS her Kiracı veritabanı bir SaaS uygulamasıdır ve SaaS uygulamasındaki gerçek Yük genellikle düzensiz ve tahmin edilemezdir. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Birkaç dakika çıkması yük düzeni için bu nedenle aşağıdaki bölümlerde yükü izlemeye çalışmadan önce 3-5 dakika için yük oluşturucuyu çalıştırmak gereklidir.

> [!IMPORTANT]
> Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş çalıştırıyor. *Demo-PerformanceMonitoringAndManagement.ps1* sekmesini açık tutun! Sekmeyi kapatırsanız veya makineyi askıya alırsanız, yük oluşturucu durur. Yük Oluşturucu kaldığı bir *iş çağırma* ürettiği nereye yük oluşturucunun başlatıldıktan sonra sağlanan tüm yeni kiracılara durumu. Kullanım *Ctrl-C* yeni işleri yürütmesini durdurmak ve betik çıkın. Yük Oluşturucu, ancak yalnızca var olan kiracılar üzerinde çalışmaya devam eder.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Uygulanmakta olan yükün neden olduğu kaynak kullanımını izlemek için Kiracı veritabanlarını içeren havuzu portalda açın:

1. Açık [Azure portalında](https://portal.azure.com) ve *tenants1-dpt -&lt;kullanıcı&gt;*  sunucusu.
1. Aşağı kaydırıp elastik havuzları bulun ve **Pool1**’e tıklayın. Bu havuz o ana kadar oluşturulmuş tüm kiracı veritabanlarını içerir.

Gözlemleyin **esnek Havuz izleme** ve **elastik veritabanı izleme** grafikler.

Havuzun kaynak kullanımını toplama veritabanı kullanımının havuzdaki tüm veritabanları ' dir. Veritabanı grafik beş en yoğun veritabanlarını gösterir:

![Veritabanı şeması](./media/saas-dbpertenant-performance-monitoring/pool1.png)

Havuza en iyi beş ötesinde ek veritabanları olduğundan havuz kullanımı, ilk beş veritabanı grafiğinde yansıtılmayan etkinlikleri gösterir. Ek ayrıntılar için tıklayın **veritabanı kaynak kullanımı**:

![veritabanı kaynak kullanımı](./media/saas-dbpertenant-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Havuzda performans uyarılarını ayarlama

Uyarı üzerinde tetikleyen Havuzu'ndaki ayarlama \>aşağıdaki gibi % 75 Kullanım:

1. Açık *Pool1* (üzerinde *tenants1-dpt -\<kullanıcı\>*  server) içinde [Azure portalında](https://portal.azure.com).
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-dbpertenant-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = eDTU yüzdesi**
   * **Koşul = büyüktür**
   * **Eşik = 75**
   * **Son 30 dakika boyunca süresi =**
1. Bir e-posta adresine ekleme *ek yönetici email(s)* kutusuna ve tıklatın **Tamam**.

   ![uyarı ayarlama](media/saas-dbpertenant-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Meşgul bir havuzun ölçeğini artırma

Bir havuzdaki toplam yük düzeyi havuzu kapasitesini aşacak bir noktaya yükselir ve %100 eDTU kullanımına ulaşırsa, bağımsız veritabanı performansı etkilenir ve havuzdaki tüm veritabanları için sorgu yanıt süreleri yavaşlayabilir.

**Kısa vadeli**, ek kaynak sağlamak için havuz ölçeğini artırmayı veya (bunları diğer havuzlara veya havuzdan tek başına hizmet katmanına taşıyarak) havuzdan veritabanı kaldırma göz önünde bulundurun.

**Uzun vadeli**, sorguları iyileştirmeyi düşünün veya veritabanı performansını artırmak için kullanım dizin. Uygulamanın performans sorunlarına karşı duyarlılığına bağlı olarak, bir havuzun ölçeğini havuz %100 eDTU kullanımına ulaşmadan artırmak idealdir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yükü artırarak meşgul bir havuzun benzetimini gerçekleştirebilirsiniz. Veritabanlarının daha sık ve için daha uzun veri bloğu bağımsız veritabanlarının gereksinimlerini değiştirmeden havuz üzerindeki toplam yükü artırma neden oluyor. Havuz ölçeğini, portaldan veya PowerShell’den kolayca artırabilirsiniz. Bu alıştırmada portal kullanılmaktadır.

1. Her bir veritabanı için gereken en büyük yükü değiştirmeden toplam yükün yoğunluğunu artırmak üzere *$DemoScenario* = **3**, _Veritabanı başına daha uzun ve daha sık artışla yük oluşturma_ değerini ayarlayın.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

1. Git **Pool1** Azure portalında.

Üst grafikten artan havuz eDTU kullanımı izleyin. İstiyorsanız, yeni yüksek yükleme birkaç dakika sürer ancak hızlı bir şekilde en yüksek kullanımı isabet havuzun görmeniz gerekir ve yük yeni düzene uyum gereğinden fazla doldurur gibi havuzu hızlı bir şekilde aşırı.

1. Havuzun ölçeğini artırmak için tıklatın **havuzu Yapılandır** en üstündeki **Pool1** sayfası.
1. Ayarlama **havuz eDTU** ayarını **100**. Havuz eDTU değerinin değiştirilmesi, veritabanı başına ayarları değiştirmez (veritabanı başına hala en fazla 50 eDTU’dur). Veritabanı başına ayarları sağ tarafında görebilirsiniz **havuzu Yapılandır** sayfası.
1. Tıklayın **Kaydet** havuzu ölçeklendirme isteği göndermek için.

Geri Git **Pool1** > **genel bakış** izleme grafiklerini görüntülemek için. (Az sayıda veritabanı ve rastgele bir yük ile her zaman biraz zaman çalıştırana kadar yaratacağı görmek kolay değildir ancak) ile daha fazla kaynak havuzu sağlama etkisini izleyin. Grafiklere bakarken, üst grafikteki %100 değerinin 100 eDTU’yu, alt grafikteki %100 değerinin ise veritabanı başına en yüksek değer hala 50 eDTU olduğundan 50 eDTU’yu temsil ettiğini aklınızda bulundurun.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Her veritabanının yeni havuz eDTU değeriyle etkinleştirilmeye hazır olduğu son anda tüm etkin bağlantılar kesilir. Kesilen bağlantıları yeniden denemek için her zaman uygulama kodunun yazılması gerekir, böylece ölçeği artırılmış havuzda veritabanına yeniden bağlanılır.

## <a name="load-balance-between-pools"></a>Havuzlar arasında Yük Dengeleme

Havuz ölçeğini artırmanın alternatif bir yolu, ikinci bir havuz oluşturup veritabanlarını bu havuza taşımak ve iki havuz arasındaki yükü dengelemektir. Bunu yapmak için yeni havuzun birinci havuzla aynı sunucuda oluşturulması gerekir.

1. İçinde [Azure portalında](https://portal.azure.com)açın **tenants1-dpt -&lt;kullanıcı&gt;**  sunucusu.
1. Tıklayın **+ yeni havuz** geçerli sunucu üzerinde havuz oluşturmak için.
1. Üzerinde **elastik havuz** şablonu:

   1. Ayarlama **adı** için *Pool2*.
   1. Fiyatlandırma katmanını **Standart Havuz** olarak bırakın.
   1. **Havuzu yapılandır**'a tıklayın,
   1. Ayarlama **havuz eDTU** için *50 eDTU*.
   1. Tıklayın **veritabanı Ekle** eklenebilir sunucuda veritabanlarının listesini görmek için *Pool2*.
   1. Bu yeni havuza taşımak ve ardından 10 tüm veritabanlarını seçin **seçin**. Yük Oluşturucu çalıştırıyorsunuz ise hizmet, performans profili varsayılan 50 eDTU boyutundan daha büyük bir havuzu gerektirir ve bir 100 eDTU ayarı ile başlayan önerir zaten bilir.

      ![öneri](media/saas-dbpertenant-performance-monitoring/configure-pool.png)

   1. Bu öğretici için varsayılan değeri bırakın 50 Edtu ve tıklayın **seçin** yeniden.
   1. Seçin **Tamam** yeni havuz oluşturun ve içine seçili veritabanlarını taşımak için.

Havuzun oluşturulması ve veritabanlarının taşınması birkaç dakika sürer. Veritabanları, çok son şu kadar çevrimiçi ve tam olarak erişilebilir kalmasını taşırken bu noktada tüm açık bağlantılar kapatılır. Bazı yeniden deneme mantığı olduğu sürece, istemciler yeni havuzdaki veritabanına bağlanır.

Gözat **Pool2** (üzerinde *tenants1-dpt -\<kullanıcı\>*  sunucusu) havuzu açın ve performansını izlemek için. Görmüyorsanız, sağlama yeni havuzunu tamamlanması için bekleyin.

Üzerindeki kaynak kullanımının artık bkz *Pool1* bıraktı ve *Pool2* artık benzer şekilde yüklenir.

## <a name="manage-performance-of-an-individual-database"></a>Tek veritabanı performansını yönetme

Bir havuzdaki tek veritabanı, havuz yapılandırmasına bağlı olarak uzun süreli yüksek yük altındaysa Havuzu'ndaki kaynakları baskındır ve diğer veritabanlarını eğilimli olabilir. Etkinlik için bir süre devam etme olasılığı ise, veritabanı geçici olarak havuz dışına taşınabilir. Bu ek kaynaklara ihtiyaç duyar ve diğer veritabanlarından ayırır sahip veritabanına sağlar.

Bu alıştırmada, popüler bir konser için biletler satışa çıktığında yüksek bir yükle karşılaşan Contoso Konser Salonu etkisinin benzetimi gerçekleştirilmektedir.

1. İçinde **PowerShell ISE**açın... \\ *Demo-PerformanceMonitoringAndManagement.ps1* betiği.
1. Ayarlama **$DemoScenario = 5, yanı sıra yüksek yük (yaklaşık 95 DTU) tek bir kiracı üzerinde normal yük oluşturur.**
1. **$SingleTenantDatabaseName = contosoconcerthall** değerini ayarlayın
1. **F5**’i kullanarak betiği yürütün.


1. İçinde [Azure portalında](https://portal.azure.com), veritabanlarının listesini Gözat *tenants1-dpt -\<kullanıcı\>*  sunucusu. 
1. Tıklayarak **contosoconcerthall** veritabanı.
1. Havuzda'a tıklayın, **contosoconcerthall** bulunmaktadır. Havuzda bulun **elastik havuz** bölümü.

1. İnceleme **esnek Havuz izleme** grafik ve artan havuz eDTU kullanımı bakın. Bir veya iki dakika sonra, daha yüksek olan yük etkisini göstermeye başlar ve havuzun %100 kullanıma ulaştığını görürsünüz.
2. İnceleme **elastik veritabanı izleme** görüntülemek, son bir saat içinde en yoğun veritabanlarını gösterir. *Contosoconcerthall* veritabanı kısa süre içinde görünmelidir beş en yoğun veritabanlarını biri olarak.
3. **Elastik veritabanı izleme'yi tıklatın** **grafik** ve onu açar **veritabanı kaynak kullanımı** burada izlemek için kullanabileceğiniz tüm veritabanlarını sayfası. Bu görüntü için ayırmanıza olanak tanır *contosoconcerthall* veritabanı.
4. Veritabanları listesinden tıklayın **contosoconcerthall**.
5. Tıklayın **fiyatlandırma Katmanı (Ölçek Dtu)** açmak için **performansı yapılandırma** veritabanı için bir tek başına işlem boyutu ayarlayabileceğiniz sayfası.
6. Standart katmanındaki ölçeklendirme seçeneklerini açmak için **Standart** sekmesine tıklayın.
7. Slayt **DTU kaydırıcısını** seçmek için sağdaki **100** dtu'ları. Bu hizmet hedefine karşılık gelen Not **S3**.
8. Tıklayın **Uygula** veritabanını havuz dışına taşımak ve bunu yapmak için bir *standart S3* veritabanı.
9. Ölçeklendirme işlemi tamamlandıktan sonra dikey pencerede contosoconcerthall veritabanının üzerindeki etkisini ve elastik havuz ve veritabanı dikey pencereleri üzerinde Pool1 izleyin.

Dikey pencerede contosoconcerthall veritabanının yüksek yük hafifledikten sonra en kısa sürede, maliyetini azaltmak için havuza döndürmelidir. Belirsiz ise, size bir uyarı ayarlayabilirsiniz veritabanında, ne zaman olacağını tetikleme DTU kullanımı, veritabanı başına düştüğünde havuzunda en fazla. Veritabanını havuza taşıma işlemi 5. alıştırmada açıklanmıştır.

## <a name="other-performance-management-patterns"></a>Diğer performans yönetimi düzenleri

**Önleyici ölçeklendirme** incelediniz burada yalıtılmış bir veritabanını nasıl ölçeklendireceğinizi alıştırmada yukarıdaki aramak için hangi veritabanı biliyorduk. Contoso Konser Salonu Yönetimi Wingtips'e, yaklaşan bilet satışından haberdar etseydi, veritabanı havuz dışına bilindiğinde taşınmış. Aksi takdirde, neler olduğunu belirlemek için havuz veya veritabanı üzerinde bir uyarı verilmesini isteyebilirdi. Bu durumu, havuzda performans düşüklüğünden şikayetçi olan diğer kiracılardan öğrenmek istemezsiniz. Kiracı ek kaynaklara ne süreyle ihtiyaç duyduğunu öngörebiliyorsa, tanımlanmış bir zamanlamaya göre veritabanını havuz dışına ve sonra tekrar içine taşımak için bir Azure Otomasyonu runbook'u ayarlayabilirsiniz.

**Kiracı self-servis ölçeklendirme** Ölçeklendirme, yönetim API’si aracılığıyla kolayca çağrılan bir görev olduğundan, kiracıya yönelik uygulamanızda kiracı veritabanlarını ölçeklendirme özelliğini kolayca oluşturabilir ve SaaS hizmetinizin bir özelliği olarak sunabilirsiniz. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Bir havuz kullanım düzenlerine uygun bir zamanlamaya göre yukarı ve aşağı ölçeklendirme**

Toplu kiracı kullanımının öngörülebilir kullanım düzenlerini takip ettiği durumlarda, belirli bir zamanlamaya göre bir havuzun ölçeğini artırıp azaltmak için Azure Otomasyonu’nu kullanabilirsiniz. Örneğin, kaynak gereksinimlerinde düşüş olduğunu bildiğiniz hafta içi günlerinde havuz ölçeğini akşam 6’dan sonra azaltıp sabah 6’dan önce yeniden artırabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yük dengelemesini yapmak üzere ikinci bir Elastik havuz sağlama

[Tek bir kiracıyı geri yükleme öğreticisi](saas-dbpertenant-restore-single-tenant.md)


## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip bilet SaaS her Kiracı veritabanı uygulama dağıtımına dayalı eğitimler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [SQL Elastik havuzları](sql-database-elastic-pool.md)
* [Azure otomasyonu](../automation/automation-intro.md)
* [Azure İzleyici günlüklerine](saas-dbpertenant-log-analytics.md) - ayar Azure İzleyici günlüklerine öğretici ayarlama ve kullanma
