---
title: Bir çok kiracılı SaaS uygulamasında birçok Azure SQL veritabanı performansını izleme | Microsoft Docs
description: İzleme ve Azure SQL veritabanı ve bir çok kiracılı SaaS uygulama havuzlarında performansını yönetme
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: d8e260b8dabb4c6823d59374a7b8661e024f1b3d
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752280"
---
# <a name="monitor-and-manage-performance-of-azure-sql-databases-and-pools-in-a-multi-tenant-saas-app"></a>İzleme ve Azure SQL veritabanı ve bir çok kiracılı SaaS uygulama havuzlarında performansını yönetme

Bu öğretici kapsamında, SaaS uygulamalarında kullanılan birkaç önemli performans yönetim senaryolarını ele alınan dizelerle. Tüm Kiracı veritabanları arasında etkinliği benzetimi için bir yük oluşturucuyu kullanarak yerleşik izleme ve SQL veritabanı ve esnek havuzlar özelliklerini uyarı gösterilmiştir.

Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama her salonundan (Kiracı) kendi veritabanına sahip olduğu bir kiracı tek veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım düzeninden yararlanmak için kiracı veritabanları elastik veritabanı havuzlarına dağıtılır. Elastik havuzlar, kaynakları çok sayıda veritabanı arasında paylaştırarak çözüm maliyetini en iyi duruma getirir. Bu düzen türü ile yüklerin havuzlar arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı ve havuz kaynak kullanımının izlenmesi önemlidir. Ayrıca, her bir veritabanının yeterli kaynağa sahip olduğundan ve havuzların [eDTU](sql-database-service-tiers.md#what-are-database-transaction-units-dtus) sınırına ulaşmadığından emin olmanız gerekir. Bu öğreticide, veritabanı ve havuzları izleyip yönetme yollarını ve iş yükündeki değişikliklere yanıt olarak nasıl düzeltici işlem yapılacağını incelenmektedir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]

> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yükünü dengelemek üzere ikinci bir Elastik havuz sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Başına Wingtip biletleri SaaS veritabanı Kiracı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve başına Wingtip biletleri SaaS veritabanı Kiracı uygulama keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS performans yönetim desenleri giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. Elastik veritabanı havuzları birden çok kiracıyı barındırdığında, öngörülemez iş yükü içeren bir veritabanı grubu için kaynakları sağlama ve yönetmenin uygun maliyetli bir yolunu sunar. Bazı iş yükü düzenlerinde iki S3 veritabanı bile tek bir havuzda yönetilebilir.

![uygulama diyagramı](./media/saas-dbpertenant-performance-monitoring/app-diagram.png)

Havuzları ve havuzları, veritabanları, kabul edilebilir performans aralıklara kalırlar emin olmak için izlenmelidir. Havuz edtu'larını genel iş yükü için uygun olduğundan emin olduktan tüm veritabanlarının toplam iş yükü gereksinimlerini karşılamak için havuzu yapılandırmasını ayarlayın. Veritabanı başına en düşük ve veritabanı başına en yüksek eDTU değerlerini, uygulamanıza özel gereksinimler için uygun değerlere ayarlayın.

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* El ile performansını izlemek zorunda kalmamak için en etkili **veritabanları veya havuzları dışında normal aralıkları parazit durumlarda tetikleyen uyarıları ayarlama**.
* Bir havuzun toplam performans düzeyindeki kısa vadeli dalgalanmalara yanıt vermek için **havuz eDTU düzeyinin ölçeği artırılabilir veya azaltılabilir**. Bu dalgalanma düzenli veya öngörülebilir aralıklarla gerçekleşiyorsa, **havuz ölçeklendirmesi otomatik olarak gerçekleşecek şekilde zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Daha uzun vadeli dalgalanmalara ya da veritabanı sayısındaki değişikliklere yanıt vermek için, **tek veritabanları diğer havuzlara taşınabilir**.
* Kısa vadeli artışlar yanıt verecek şekilde *tek tek* veritabanı yük **tek veritabanlarını dışında bir havuzu alınır ve bir bireysel performans düzeyi atanan**. Yükü azaldıktan sonra veritabanını havuza döndürebilirsiniz. Bu önceden bilindiğinde veritabanları pre-emptively veritabanı her zaman, gereken kaynak bulunduğundan emin olun ve havuzdaki diğer veritabanlarına üzerindeki etkiyi önlemek için taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarma kaynak özgü kaynakları küçük sayılar için uygundur ancak birçok kaynaklarla çalışırken çok uygun değil.

Burada çalıştığınız birçok kaynaklarla, yüksek hacimli senaryolar için [günlük analizi](saas-dbpertenant-log-analytics.md) kullanılabilir. Verilmiş tanılama günlüklerini ve günlük analizi çalışma alanındaki toplanan telemetri üzerinden analytics sağlayan ayrı bir Azure hizmet budur. Günlük analizi telemetri birçok Hizmetleri'nden toplayabilir ve sorgu ve uyarıları ayarlamak için kullanılır.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Havuzlar yalnızca iki adet S3 veritabanı ile uygun maliyetli olabilse de, havuzdaki veritabanı sayısı arttıkça ortalama etki daha uygun maliyetli olur. Performans izleme ve yönetiminin ölçeklenerek nasıl çalıştığını anlamak için bu öğreticide en az 20 veritabanınızın dağıtılmış olması gerekir.

Önceki bir öğreticide kiracılar toplu zaten sağlandı, geçin [tüm Kiracı veritabanları benzetim kullanımı](#simulate-usage-on-all-tenant-databases) bölümü.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\performans izleme ve Yönetim\\*Demo PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. **$DemoScenario** = **1**, **Kiracı grubu sağlama** değerini ayarlayın
1. Betiği çalıştırmak için **F5**'e basın.

Bu betik, beş dakikadan daha kısa bir süre içinde 17 kiracı dağıtır.

*Yeni TenantBatch* komut dosyası kullanan bir iç içe ya da bağlantılı dizi [Resource Manager](../azure-resource-manager/index.md) veritabanını kopyalar, varsayılan kiracılar, toplu oluşturmak şablonlar **basetenantdb**yeni Kiracı veritabanları oluşturmak için katalog sunucusunda ardından bunlar kataloğunda kaydeder ve son olarak bunları Kiracı adı ve salonundan türü ile başlatır. Bu uygulamayı yeni bir kiracı hazırlar yolu ile tutarlıdır. Yapılan değişiklikler *basetenantdb* bundan sonra sağlanan yeni kiracılar için uygulanır. Bkz: [şema yönetimi öğretici](saas-tenancy-schema-management.md) şema değişiklikleri yapma görmek için *varolan* Kiracı veritabanları (de dahil olmak üzere *basetenantdb* veritabanı).

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

*Demo PerformanceMonitoringAndManagement.ps1* tüm Kiracı veritabanları karşı çalışan bir iş yükünü benzetim sağlanan komut dosyasıdır. Yükü, kullanılabilir yük senaryolardan biri, kullanılarak oluşturulur:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunlukta yük (yaklaşık 40 DTU) oluşturma |
| 3 | Veritabanı başına daha uzun ve daha sık artışla yük oluşturma|
| 4 | Veritabanı başına daha yüksek DTU veri bloğu (yaklaşık 80 DTU) ile yük oluşturma|
| 5 | Tek bir kiracı üzerinde normal yük ve yoğun yük oluşturma (yaklaşık 95 DTU)|
| 6 | Birden çok havuzda dengesiz yük oluşturma|

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (eDTU cinsinden), süresi ve aralıkları tüm veritabanlarında farklıdır ve öngörülemez kiracı etkinliğini benzetimi gerçekleştirir.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\performans izleme ve Yönetim\\*Demo PerformanceMonitoringAndManagement.ps1*. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. Ayarlama **$DemoScenario** = **2**, *Generate normal yoğunluğu yük*.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

Başına Wingtip biletleri SaaS veritabanı Kiracı bir SaaS uygulaması ve bir SaaS uygulaması gerçek yükü genellikle durumlarıyla ve tahmin edilemez. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Birkaç dakika kadar 3-5 dakika aşağıdaki bölümlerde yük izlemek denemeden önce yük oluşturucuyu çalıştırmak çıkmaya, yük düzeni için gereklidir.

> [!IMPORTANT]
> Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş çalıştırıyor. *Demo-PerformanceMonitoringAndManagement.ps1* sekmesini açık tutun! Sekmeyi kapatırsanız veya makineyi askıya alırsanız, yük oluşturucu durur. Yük Oluşturucu kaldığı bir *iş çağırma* ürettiği burada oluşturucunun başlatıldıktan sonra sağlanan yeni kiracılar yük durumu. Kullanım *Ctrl-C* yeni işleri çağırma durdurun ve komut dosyası çıkmak için. Yük Oluşturucu, ancak yalnızca var olan kiracılar çalışmaya devam eder.

## <a name="monitor-resource-usage-using-the-azure-portal"></a>Azure portalını kullanarak kaynak kullanımını izleme

Uygulanmakta olan yük sonuçları kaynak kullanımını izlemek için Kiracı veritabanları içeren havuzu portalına açın:

1. Açık [Azure portal](https://portal.azure.com) ve *tenants1-dpt -&lt;kullanıcı&gt;*  sunucu.
1. Aşağı kaydırıp elastik havuzları bulun ve **Pool1**’e tıklayın. Bu havuz o ana kadar oluşturulmuş tüm kiracı veritabanlarını içerir.

Gözlemlemek **esnek Havuz izleme** ve **esnek veritabanı izleme** grafik.

Havuza ait kaynak kullanımı havuzdaki tüm veritabanları için birleşik veritabanı kullanımı ' dir. Veritabanı grafik beş yeni veritabanları gösterir:

![Veritabanı şeması](./media/saas-dbpertenant-performance-monitoring/pool1.png)

Havuz üst beş ötesinde ek veritabanları olduğundan havuzu kullanımı üst beş veritabanları grafikte yansıtılmaz etkinliğini gösterir. Ek ayrıntılar için tıklatın **veritabanı kaynak kullanımını**:

![veritabanı kaynak kullanımı](./media/saas-dbpertenant-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Havuzda performans uyarılarını ayarlama

Bir uyarı üzerinde tetikler havuzu Ayarla \>şekilde % 75 kullanımı:

1. Açık *Pool1* (üzerinde *tenants1-dpt -\<kullanıcı\>*  sunucu) içinde [Azure portal](https://portal.azure.com).
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/saas-dbpertenant-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin,
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = eDTU yüzdesi**
   * **Koşul büyük =**
   * **Eşik 75 =**
   * **Son 30 dakika boyunca süresi =**
1. Bir e-posta adresi ekleyin *ek yönetici email(s)* kutusuna ve tıklatın **Tamam**.

   ![uyarı ayarlama](media/saas-dbpertenant-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Meşgul bir havuzun ölçeğini artırma

Bir havuzdaki toplam yük düzeyi havuzu kapasitesini aşacak bir noktaya yükselir ve %100 eDTU kullanımına ulaşırsa, bağımsız veritabanı performansı etkilenir ve havuzdaki tüm veritabanları için sorgu yanıt süreleri yavaşlayabilir.

**Kısa vadeli**, ek kaynaklar sağlamak için kuyruğunu ölçekleme veya (bunları havuzlarını veya havuza dışında bir tek başına hizmet katmanına taşıma) havuzdan veritabanı kaldırma göz önünde bulundurun.

**Uzun vadeli**, sorguları en iyi duruma getirme göz önünde bulundurun veya dizin veritabanı performansını iyileştirmek için kullanım. Uygulamanın performans sorunlarına karşı duyarlılığına bağlı olarak, bir havuzun ölçeğini havuz %100 eDTU kullanımına ulaşmadan artırmak idealdir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yükü artırarak meşgul bir havuzun benzetimini gerçekleştirebilirsiniz. Daha sık ve için artık, veri bloğu için veritabanlarını tek veritabanlarını gereksinimlerini değiştirmeden artan Havuzu'ndaki toplam yük neden oluyor. Havuz ölçeğini, portaldan veya PowerShell’den kolayca artırabilirsiniz. Bu alıştırmada portal kullanılmaktadır.

1. Her bir veritabanı için gereken en büyük yükü değiştirmeden toplam yükün yoğunluğunu artırmak üzere *$DemoScenario* = **3**, _Veritabanı başına daha uzun ve daha sık artışla yük oluşturma_ değerini ayarlayın.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

1. Git **Pool1** Azure portalında.

Üst grafik artan havuz eDTU kullanımı izleyin. Kazandırın yeni daha fazla yük için birkaç dakika sürer ancak max kullanımı isabet Başlat havuzu hızlı bir şekilde görmeniz gerekir ve yeni modele yük steadies gibi hızlı bir şekilde havuzu overloads.

1. Havuzu oluşturan ölçeklendirmek için tıklatın **havuzu yapılandırma** en üstündeki **Pool1** sayfası.
1. Ayarlama **havuz eDTU** ayarını **100**. Havuz eDTU değerinin değiştirilmesi, veritabanı başına ayarları değiştirmez (veritabanı başına hala en fazla 50 eDTU’dur). Veritabanı başına ayarlarını sağ tarafında görebilir **havuzu yapılandırma** sayfası.
1. Tıklatın **kaydetmek** havuzu ölçeklendirme isteği göndermek için.

Geri dönerek **Pool1** > **genel bakış** izleme grafikleri görüntülemek için. (Birkaç veritabanları ve rastgele bir yükleme ile bu her zaman biraz zaman çalıştırana kadar conclusively görmek kolay olmasa da) ile daha fazla kaynak havuzu sağlama etkisini izleyin. Grafiklere bakarken, üst grafikteki %100 değerinin 100 eDTU’yu, alt grafikteki %100 değerinin ise veritabanı başına en yüksek değer hala 50 eDTU olduğundan 50 eDTU’yu temsil ettiğini aklınızda bulundurun.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Her veritabanının yeni havuz eDTU değeriyle etkinleştirilmeye hazır olduğu son anda tüm etkin bağlantılar kesilir. Kesilen bağlantıları yeniden denemek için her zaman uygulama kodunun yazılması gerekir, böylece ölçeği artırılmış havuzda veritabanına yeniden bağlanılır.

## <a name="load-balance-between-pools"></a>Havuzları arasında Yük Dengelemesi

Havuz ölçeğini artırmanın alternatif bir yolu, ikinci bir havuz oluşturup veritabanlarını bu havuza taşımak ve iki havuz arasındaki yükü dengelemektir. Bunu yapmak için yeni havuzun birinci havuzla aynı sunucuda oluşturulması gerekir.

1. İçinde [Azure portal](https://portal.azure.com), açık **tenants1-dpt -&lt;kullanıcı&gt;**  sunucu.
1. Tıklatın **+ yeni havuz** geçerli sunucuda bir havuzu oluşturmak için.
1. Üzerinde **esnek veritabanı havuzu** şablonu:

    1. Ayarlama **adı** için *Pool2*.
    1. Fiyatlandırma katmanını **Standart Havuz** olarak bırakın.
    1. **Havuzu yapılandır**'a tıklayın,
    1. Ayarlama **havuz eDTU** için *50 eDTU*.
    1. Tıklatın **eklemek veritabanları** eklenebilir sunucusundaki veritabanlarının listesini görmek için *Pool2*.
    1. Bu yeni havuzuna taşıyın ve ardından 10 istediğiniz veritabanlarını seçin **seçin**. Yük Oluşturucu çalışır durumda, hizmet performansı profilinizi varsayılan 50 eDTU boyutundan daha büyük bir havuzu gerektirir ve bir 100 eDTU ayarı ile başlayan önerir zaten bilir.

    ![öneri](media/saas-dbpertenant-performance-monitoring/configure-pool.png)

    1. Bu öğretici için varsayılan 50 Edtu bırakın ve tıklatın **seçin** yeniden.
    1. Seçin **Tamam** yeni havuzu oluşturun ve içine seçilen veritabanlarını taşımak için.

Havuz oluşturma ve veritabanlarını taşıma birkaç dakika sürer. Veritabanları, kadar çok son şu anda çevrimiçi ve tam olarak erişilebilir kalırlar taşınırken, bu noktada açık olan bağlantıları kapanır. Bazı yeniden deneme mantığı sahip olduğu sürece, istemciler yeni havuz veritabanına bağlanır.

Gözat **Pool2** (üzerinde *tenants1-dpt -\<kullanıcı\>*  sunucu) havuzunu açar ve kendi performansını izlemek için. Göremiyorsanız, tamamlamak için yeni havuzu sağlama için bekleyin.

Artık kaynak kullanımının gördüğünüz *Pool1* bıraktı ve *Pool2* şimdi benzer şekilde yüklenir.

## <a name="manage-performance-of-a-single-database"></a>Tek bir veritabanının performansını yönetme

Bir havuzdaki tek bir veritabanı sürekli yüksek bir yük altındaysa, havuz yapılandırmasına bağlı olarak, havuzdaki kaynaklara yön vermeye ve diğer veritabanlarını etkilemeye eğilimli olabilir. Etkinlik için bir süre devam etmek olasılığı varsa, veritabanını geçici olarak havuzu dışında taşınabilir. Bu gerekir ve diğer veritabanlarından yalıtır ek kaynaklar veritabanı sağlar.

Bu alıştırmada, popüler bir konser için biletler satışa çıktığında yüksek bir yükle karşılaşan Contoso Konser Salonu etkisinin benzetimi gerçekleştirilmektedir.

1. İçinde **PowerShell ISE**açın... \\ *Demo PerformanceMonitoringAndManagement.ps1* komut dosyası.
1. **$DemoScenario = 5, Tek bir kiracı üzerinde normal yük ve yoğun yük oluşturma (yaklaşık 95 DTU)** ayarını yapın.
1. **$SingleTenantDatabaseName = contosoconcerthall** değerini ayarlayın
1. **F5**’i kullanarak betiği yürütün.


1. İçinde [Azure portal](https://portal.azure.com), veritabanlarının listesini Gözat *tenants1-dpt -\<kullanıcı\>*  sunucu. 
1. Tıklayın **contosoconcerthall** veritabanı.
1. Havuzunda'ı tıklatın, **contosoconcerthall** bulunmaktadır. Havuzda bulun **esnek veritabanı havuzu** bölümü.

1. İnceleme **esnek Havuz izleme** grafik ve artan havuz eDTU kullanımı için bakın. Bir veya iki dakika sonra, daha yüksek olan yük etkisini göstermeye başlar ve havuzun %100 kullanıma ulaştığını görürsünüz.
2. İnceleme **esnek veritabanı izleme** görüntülemek, son bir saat içindeki yeni veritabanları gösterir. *Contosoconcerthall* veritabanı yakında görünmelidir beş yeni veritabanları biri olarak.
3. **Esnek veritabanı izleme'yi tıklatın** **grafik** ve açar **veritabanı kaynak kullanımını** burada izleyebilirsiniz herhangi bir veritabanı sayfası. Bu görüntü için ayırmanıza olanak tanır *contosoconcerthall* veritabanı.
4. Veritabanları listesinden tıklatın **contosoconcerthall**.
5. Tıklatın **fiyatlandırma Katmanı (Ölçek Dtu'lar)** açmak için **performansını yapılandırın** veritabanı için bir tek başına performans düzeyi ayarlayabileceğiniz sayfa.
6. Standart katmanındaki ölçeklendirme seçeneklerini açmak için **Standart** sekmesine tıklayın.
7. Slayt **DTU kaydırıcı** seçmek için sağdaki **100** Dtu'lar. Bu hizmet hedefi için karşılık gelen Not **S3**.
8. Tıklatın **Uygula** dışında havuzu veritabanını taşımak ve bunu yapmak için bir *standart S3* veritabanı.
9. Ölçeklendirme işlemi tamamlandıktan sonra contosoconcerthall veritabanı üzerindeki etkisini ve esnek havuzu ve veritabanı dikey pencereleri üzerinde Pool1 izleyin.

Yüksek yük contosoconcerthall veritabanında subsides sonra hemen kendi maliyetini azaltmak havuzuna döndürmelidir. Belirsiz ise, ayarladığınız uyarı veritabanı üzerinde ne zaman olacağını tetiklemek DTU kullanımı veritabanı başına düştüğünde havuzunda maks. Veritabanını havuza taşıma işlemi 5. alıştırmada açıklanmıştır.

## <a name="other-performance-management-patterns"></a>Diğer performans yönetim desenleri

**Önleyici ölçeklendirme** incelediniz burada yalıtılmış bir veritabanı ölçeklendirme alıştırmada yukarıdaki aranacak hangi veritabanı biliyorduk. Contoso birlikte Hall yönetim yaklaşan bilet satış Wingtips haberdar değilse, veritabanı dışında havuzu pre-emptively taşınmış. Aksi takdirde, neler olduğunu belirlemek için havuz veya veritabanı üzerinde bir uyarı verilmesini isteyebilirdi. Bu durumu, havuzda performans düşüklüğünden şikayetçi olan diğer kiracılardan öğrenmek istemezsiniz. Kiracı ek kaynaklara ne süreyle ihtiyaç duyduğunu öngörebiliyorsa, tanımlanmış bir zamanlamaya göre veritabanını havuz dışına ve sonra tekrar içine taşımak için bir Azure Otomasyonu runbook'u ayarlayabilirsiniz.

**Kiracı self-servis ölçeklendirme** Ölçeklendirme, yönetim API’si aracılığıyla kolayca çağrılan bir görev olduğundan, kiracıya yönelik uygulamanızda kiracı veritabanlarını ölçeklendirme özelliğini kolayca oluşturabilir ve SaaS hizmetinizin bir özelliği olarak sunabilirsiniz. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

**Bir havuz kullanım desenlerini eşleştirmek için bir zamanlamaya göre yukarı ve aşağı doğru ölçeklendirme**

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

* Ek [derleme başına Wingtip biletleri SaaS veritabanı Kiracı uygulama dağıtımı sırasında öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [SQL Elastik havuzları](sql-database-elastic-pool.md)
* [Azure otomasyonu](../automation/automation-intro.md)
* [Log Analytics](saas-dbpertenant-log-analytics.md) - Log Analytics ayarlama ve kullanma öğreticisi
