---
title: "Bir SQL Veritabanı SaaS uygulamasının performansını izleme | Microsoft Docs"
description: "Azure SQL Veritabanı örnek Wingtip Biletleri (WTP) uygulamasının performansını izleme ve yönetme"
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
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: af9511978718af10c97bee6af3a2835c9d2c1ff4
ms.contentlocale: tr-tr
ms.lasthandoff: 05/12/2017


---
# <a name="monitor-performance-of-the-wtp-sample-saas-application"></a>WTP örnek SaaS uygulamasının performansını izleme

Bu öğreticide, SQL Veritabanı ve elastik havuzların yerleşik izleme ve uyarı özellikleri gösterilmekte, ardından SaaS uygulamalarında kullanılan birkaç temel performans yönetimi senaryosu incelenmektedir.

Wingtip Bilet uygulaması, her mekanın (kiracının) kendi veritabanına sahip olduğu tek kiracılı veri modeli kullanır. Birçok SaaS uygulaması gibi, beklenen kiracı iş yükü düzeni öngörülemez ve düzensizdir. Diğer bir deyişle, bilet satışı herhangi bir zamanda gerçekleşebilir. Bu tipik veritabanı kullanım düzeninden yararlanmak için kiracı veritabanları elastik veritabanı havuzlarına dağıtılır. Elastik havuzlar, kaynakları çok sayıda veritabanı arasında paylaştırarak çözüm maliyetini en iyi duruma getirir. Bu düzen türü ile yüklerin havuzlar arasında makul bir şekilde dengelendiğinden emin olmak için veritabanı ve havuz kaynak kullanımının izlenmesi önemlidir. Ayrıca, her bir veritabanının yeterli kaynağa sahip olduğundan ve havuzların [eDTU](sql-database-what-is-a-dtu.md) sınırına ulaşmadığından emin olmanız gerekir. Bu öğreticide, veritabanı ve havuzları izleyip yönetme yollarını ve iş yükündeki değişikliklere yanıt olarak nasıl düzeltici işlem yapılacağını incelenmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yükünü dengelemek üzere ikinci bir Elastik havuz sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>SaaS Performans Yönetimi Düzenlerine Giriş

Veritabanı performans yönetimi, performans verilerini derleyip çözümlemeyi ve ardından uygulamanız için kabul edilebilir bir yanıt süresi sağlamak için parametreleri ayarlayarak bu verilere yanıt vermeyi içerir. Elastik veritabanı havuzları birden çok kiracıyı barındırdığında, öngörülemez iş yükü içeren bir veritabanı grubu için kaynakları sağlama ve yönetmenin uygun maliyetli bir yolunu sunar. Bazı iş yükü düzenlerinde iki S3 veritabanı bile tek bir havuzda yönetilebilir. Bir havuz yalnızca kaynakların maliyetini paylaşmaz, aynı zamanda her bir veritabanının sürekli olarak izleyip takip etme gereksinimini ortadan kaldırır.

![medya](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Havuzlar ve havuzlardaki veritabanları, kabul edilebilir performans aralıkları dahilinde olduklarından emin olmak için yine de izlenmelidir. Toplu iş yükü gereksinimlerini karşılamak ve havuz eDTU’larının genel iş yükü için uygun olduğundan emin olmak için havuz yapılandırmasını ayarlayın. Veritabanı başına en düşük ve veritabanı başına en yüksek eDTU değerlerini, uygulamanıza özel gereksinimler için uygun değerlere ayarlayın.

### <a name="performance-management-strategies"></a>Uygulama performansı stratejileri

* Performansı el ile izlemek zorunda kalmamak için, **veritabanı veya havuzlar normal aralıkların dışına çıktığında tetiklenen uyarılar ayarlamak** en etkili yöntemdir.
* Bir havuzun toplam performans düzeyindeki kısa vadeli dalgalanmalara yanıt vermek için **havuz eDTU düzeyinin ölçeği artırılabilir veya azaltılabilir**. Bu dalgalanma düzenli veya öngörülebilir aralıklarla gerçekleşiyorsa, **havuz ölçeklendirmesi otomatik olarak gerçekleşecek şekilde zamanlanabilir**. Örneğin, iş yükünüzün hafif olduğunu bildiğiniz gece veya hafta sonları gibi zamanlarda ölçeği azaltabilirsiniz.
* Daha uzun vadeli dalgalanmalara ya da veritabanı sayısındaki değişikliklere yanıt vermek için, **tek veritabanları diğer havuzlara taşınabilir**.
* *Bağımsız* veritabanı yükündeki kısa vadeli artışlara yanıt vermek üzere **bağımsız veritabanları havuz dışına alınıp bunlara belirli bir süre için ayrı bir performans düzeyi atanabilir**. Yükü azaldıktan sonra veritabanını havuza döndürebilirsiniz. Bu durum önceden bilindiğinde, veritabanının her zaman gerekli kaynaklara sahip olduğundan emin olmak ve havuzdaki diğer veritabanlarını etkilemesini önlemek amacıyla veritabanları önceden taşınabilir. Popüler bir etkinlik için bilet satışı yoğunluğu yaşanan bir mekanda olduğu gibi bu gereksinim öngörülebildiği takdirde bu yönetim davranışı uygulamayla tümleştirilebilir.

[Azure portalı](https://portal.azure.com), çoğu kaynak üzerinde yerleşik izleme ve uyarı özelliği sağlar. SQL Veritabanı için veritabanı ve havuzlarda izleme ve uyarı özellikleri kullanılabilir. Bu yerleşik izleme ve uyarı özellikleri kaynağa özeldir. Bu nedenle az sayıda kaynak için kullanılması kolaydır, ancak çok sayıda kaynakla çalışırken kullanışlı olmaz.

Yüksek hacimli senaryolar için Log Analytics (aynı zamanda OMS olarak bilinir) kullanılabilir. Bu, yayınlanan tanılama günlükleri ve log analytics çalışma alanında toplanan telemetri üzerinden analizler sağlayan ayrı bir Azure hizmetidir. Bu hizmet, birçok hizmetten telemetri toplayabilir ve uyarıları sorgulamak ve ayarlamak için kullanılabilir.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="provision-additional-tenants"></a>Ek kiracılar sağlama

Havuzlar yalnızca iki adet S3 veritabanı ile uygun maliyetli olabilse de, havuzdaki veritabanı sayısı arttıkça ortalama etki daha uygun maliyetli olur. Performans izleme ve yönetiminin ölçeklenerek nasıl çalıştığını anlamak için bu öğreticide en az 20 veritabanınızın dağıtılmış olması gerekir.

Önceki bir öğreticide zaten bir kiracı grubu sağladıysanız, [Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme](#simulate-usage-on-all-tenant-databases) bölümüne atlayabilirsiniz.

1. **PowerShell ISE**’de …\\Öğrenme Modülleri\\Sağlama ve Kataloğa Kaydetme Oluşturma\\*Demo-PerformanceMonitoringAndManagement.ps1* öğesini açın. Bu öğretici sırasında birkaç senaryo çalıştıracağından bu betiği açık tutun.
1. **$DemoScenario** = **1**, **Kiracı grubu sağlama** değerini ayarlayın
1. Betiği çalıştırmak için **F5**'e basın.

Bu betik, beş dakikadan daha kısa bir süre içinde 17 kiracı dağıtır.

*New-TenantBatch* betiği, yeni kiracı veritabanları oluşturmak üzere varsayılan olarak **baseTenantDb** veritabanını katalog sunucusuna kopyalayan, ardından kataloğa kaydeden ve son olarak bunları kiracı adı ve yer türü ile başlatan bir kiracı grubu oluşturan iç içe veya bağlı bir [Resource Manager](../azure-resource-manager/index.md) şablon kümesi kullanır. Bu, WTP uygulamasının yeni bir kiracı sağlama yöntemiyle tutarlıdır. *baseTenantDB* üzerinde yapılan tüm değişiklikler ondan sonra sağlanan tüm yeni kiracılara uygulanır. *Var olan* kiracı veritabanlarında (*altın* veritabanı dahil) nasıl değişiklik yapılacağını görmek için bkz. [Şema Yönetimi öğreticisi](sql-database-saas-tutorial-schema-management.md).

## <a name="simulate-different-usage-patterns-by-generating-different-load-types"></a>Farklı yük türleri oluşturarak farklı kullanım düzenlerinin benzetimini gerçekleştirme

*Demo-PerformanceMonitoringAndManagement.ps1* betiği, kullanılabilir *yük türlerinden* birini kullanarak yük oluşturucuyu başlatır:

| Tanıtım | Senaryo |
|:--|:--|
| 2 | Normal yoğunlukta yük (yaklaşık 40 DTU) oluşturma |
| 3 | Veritabanı başına daha uzun ve daha sık artışla yük oluşturma|
| 4 | Veritabanı başına daha yüksek DTU veri bloğu (yaklaşık 80 DTU) ile yük oluşturma|
| 5 | Tek bir kiracı üzerinde normal yük ve yoğun yük oluşturma (yaklaşık 95 DTU)|
| 6 | Birden çok havuzda dengesiz yük oluşturma|

## <a name="simulate-usage-on-all-tenant-databases"></a>Tüm kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme

Yük oluşturucu her kiracı veritabanına *yapay* bir yalnızca CPU yükü uygular. Oluşturucu her kiracı veritabanı için yükü oluşturan saklı yordamı düzenli olarak çağıran bir iş başlatır. Yük düzeyleri (eDTU cinsinden), süresi ve aralıkları tüm veritabanlarında farklıdır ve öngörülemez kiracı etkinliğini benzetimi gerçekleştirir.

1. **$DemoScenario** = **2**, *Normal yoğunlukta yük oluştur* değerini ayarlayın.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.

Yükün seyrek niteliği nedeniyle, etkinliği kararlı bir duruma gelmesi ve uygun bir düzen uygulaması için 10-20 dakika çalıştırın.

> [!IMPORTANT]
> Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş çalıştırıyor. *Demo-PerformanceMonitoringAndManagement.ps1* sekmesini açık tutun! Sekmeyi kapatırsanız veya makineyi askıya alırsanız, yük oluşturucu durur.

## <a name="monitor-resource-usage-using-the-portal"></a>Portalı kullanarak kaynak kullanımını izleme

Uygulanmakta olan yükün neden olduğu kaynak kullanımını izlemek için, kiracı veritabanlarını içeren havuzu portalda açın.

1. [Azure portalını](https://portal.azure.com) açın ve tenants1-&lt;USER&gt; sunucusuna göz atın.
1. Yeni veritabanı grubunu içeren kiracı veritabanı listesini görmeniz gerekir.
1. Aşağı kaydırıp elastik havuzları bulun ve **Pool1**’e tıklayın. Bu havuz o ana kadar oluşturulmuş tüm kiracı veritabanlarını içerir.
1. Açılan havuz dikey penceresini genişletin ve havuz kullanım grafiği ile en yoğun veritabanı kullanım grafiğini inceleyin.

Havuz kullanımı, temelde havuzdaki tüm veritabanlarına ait veritabanı kullanımının toplamıdır. Veritabanı kullanım grafiğinde en yoğun 5 veritabanı gösterilir:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Havuzda ilk 5’ten başka veritabanları da olduğundan havuz kullanımı, ilk beş veritabanı grafiğinde yansıtılmayan etkinlikleri gösterir. Bazı ek ayrıntıları görmek için bkz. **Veritabanı Kaynak Kullanımı**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Havuzda performans uyarılarını ayarlama

Havuzda \>5 dakika boyunca devam eden %75 kullanım halinde tetiklenen bir uyarıyı aşağıda anlatılan şekilde ayarlayın:

1. [Azure portalında](https://portal.azure.com) *Pool1*’i (*tenants1-\<user\>* sunucusunda) açın.
1. **Uyarı Kuralları**ve ardından **+ Uyarı ekle**’ye tıklayın:

   ![uyarı ekle](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. **Yüksek DTU** gibi bir ad belirtin, 
1. Aşağıdaki değerleri ayarlayın:
   * **Ölçüm = eDTU yüzdesi**
   * **Koşul = büyüktür**.
   * **Eşik = 75**.
   * **Süre = Son 30 dakika boyunca**.

   ![uyarı ayarlama](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)

Bildirimlerin Azure hesabı e-postanıza ve isteğe bağlı olarak başka e-postalara gönderilmesini isteyebilirsiniz (kullanılmakta olan aboneliğin sahibi değilseniz, başka e-postalara göndermemeniz önerilir).

> [!NOTE]
> Uyarı yalnızca son 30 dakika boyunca eşik aşılırsa tetiklendiğinden, yük oluşturucunun uyarıyı test edebilmesi için 30 dakikadan uzun süredir çalışıyor olması gerekir.


## <a name="scale-up-a-busy-pool"></a>Meşgul bir havuzun ölçeğini artırma

Bir havuzdaki toplam yük düzeyi havuzu kapasitesini aşacak bir noktaya yükselir ve %100 eDTU kullanımına ulaşırsa, bağımsız veritabanı performansı etkilenir ve havuzdaki tüm veritabanları için sorgu yanıt süreleri yavaşlayabilir.

Kısa vadede, ek kaynak sağlamak için havuz ölçeğini artırmayı veya havuzdan veritabanı kaldırmayı (diğer havuzlara veya havuzdan tek başına hizmet katmanına taşıyarak) düşünün.

Uzun vadede, veritabanı performansını artırmak üzere sorguları veya dizin kullanımını iyileştirmeyi düşünün. Uygulamanın performans sorunlarına karşı duyarlılığına bağlı olarak, bir havuzun ölçeğini havuz %100 eDTU kullanımına ulaşmadan artırmak idealdir. Sizi önceden uyarması için bir uyarı ayarlayın.

Oluşturucu tarafından üretilen yükü artırarak meşgul bir havuzun benzetimini gerçekleştirebilirsiniz. Veritabanlarının daha sık ve daha uzun süreyle kapasitesinin aşılması, bağımsız veritabanlarının gereksinimlerini değiştirmeden havuz üzerindeki toplam yükü artırır. Havuz ölçeğini, portaldan veya PowerShell’den kolayca artırabilirsiniz. Bu alıştırmada portal kullanılmaktadır.

1. Her bir veritabanı için gereken en büyük yükü değiştirmeden toplam yükün yoğunluğunu artırmak üzere *$DemoScenario* = **3**, _Veritabanı başına daha uzun ve daha sık artışla yük oluşturma_ değerini ayarlayın.
1. Bir yükü tüm kiracı veritabanlarınıza uygulamak için **F5** tuşuna basın.


1. **tenants1/Pool1** için **Havuz dikey penceresini açın**.


1. Üst grafikten artan havuz DTU kullanımını izleyin. Daha yüksek olan yeni yükün etkisini göstermesi birkaç dakika sürer, ancak havuzun %100 kullanıma ulaştığını hemen görebilirsiniz ve yük yeni düzene uyum sağlarken havuzu hızlı bir şekilde gereğinden fazla doldurur.


1. Havuzun ölçeğini artırmak için **Havuzu yapılandır** öğesine tıklayın
1. **Havuz eDTU** kaydırıcısını 100’e ayarlayın (maliyetleri sınırlamak için daha yükseğe çıkmamanız önerilir). Havuzdaki tüm veritabanları için **Havuz GB** ile gösterilen kullanılabilir toplam depolamanın, eDTU ayarına bağlı olduğuna ve aynı zamanda arttığına dikkat edin. Havuz eDTU değerinin değiştirilmesi, veritabanı başına ayarları değiştirmez (veritabanı başına hala en fazla 50 eDTU’dur). Veritabanı başına ayarları **Havuzu yapılandır** dikey penceresinin sağ tarafında görebilirsiniz.
1. İsteği göndermek için **Kaydet**’e tıklayın. Değişiklik, Standart bir havuz için genellikle 3-5 dakika sürer.

İzleme grafiklerini görüntülemek için **Pool1** > **Genel Bakış** bölümüne geri dönün. Havuza daha fazla kaynak sağlamanın yaratacağı etkiyi izleyin (az sayıda veritabanı ve rastgele bir yük olsa da her zaman kesin olarak görmek kolay değildir). Grafiklere bakarken, üst grafikteki %100 değerinin 100 eDTU’yu, alt grafikteki %100 değerinin ise veritabanı başına en yüksek değer hala 50 eDTU olduğundan 50 eDTU’yu temsil ettiğini aklınızda bulundurun.

İşlem boyunca veritabanları çevrimiçi ve tam olarak kullanılabilir durumdadır. Her veritabanının yeni havuz eDTU değeriyle etkinleştirilmeye hazır olduğu son anda tüm etkin bağlantılar kesilir. Kesilen bağlantıları yeniden denemek için her zaman uygulama kodunun yazılması gerekir, böylece ölçeği artırılmış havuzda veritabanına yeniden bağlanılır.

## <a name="create-a-second-pool-and-load-balance-databases-to-handle-increased-aggregate-load"></a>İkinci bir havuz oluşturma ve artan toplam yükü işlemek için veritabanlarının yük dengelemesini yapma

Havuz ölçeğini artırmanın alternatif bir yolu, ikinci bir havuz oluşturup veritabanlarını bu havuza taşımak ve iki havuz arasındaki yükü dengelemektir. Bunu yapmak için yeni havuzun birinci havuzla aynı sunucuda oluşturulması gerekir.

1. **customers1-&lt;USER&gt; sunucusunun sunucu dikey penceresini** açın. Bir veritabanı veya havuz dikey penceresindeyseniz, temel denetimler açılır menüsünden sunucu adını kısayol olarak seçebilirsiniz.
1. Geçerli sunucu üzerinde havuz oluşturmak için **+ Yeni havuz**’a tıklayın
1. Yeni Elastik veritabanı havuzu şablonunda:

    1. **Ad = Pool2** ayarını yapın.
    1. Fiyatlandırma katmanını **Standart Havuz** olarak bırakın.
    1. **Havuzu yapılandır**'a tıklayın,
    1. Açılan Havuzu Yapılandır dikey penceresinde **Havuz eDTU = 50 DTU** ayarını yapın.
    1. Bu sunucuda olup geçerli havuzda bulunmayan veritabanlarının listesini görmek için **Veritabanı ekle** komutuna tıklayın.
    1. Listede veritabanlarının yarısını (10/20) yeni havuza taşımak üzere **işaretleyin** ve ardından **Seç**’e tıklayın.
    1. Yapılandırma değişikliklerini kabul etmek için **Seç**’e yeniden tıklayın. Belirlenen seçeneklerle bir aylık kullanıma yönelik tahmini maliyeti not edin.
    1. Yeni yapılandırmaya sahip yeni havuzu oluşturmak ve veritabanlarını taşımak için **Tamam**’ı seçin.

Havuzun oluşturulması ve veritabanlarının taşınması birkaç dakika sürer. Taşınan veritabanlarının her biri son ana kadar çevrimiçi ve tam olarak erişilebilir durumdadır; son ana gelindiğinde ise tüm açık bağlantılar kapatılır. İstemci bağlantıyı yeniden denediğinde, yeni havuzdaki veritabanına bağlanır.

Havuzu oluşturulduktan sonra customers1 sunucusu dikey penceresinde görünür. Havuz adına tıklayarak havuz dikey penceresini açın ve performansını izleyin.

Pool1 üzerindeki kaynak kullanımının azaldığını ve Pool2’nin benzer şekilde yüklendiğini görmeniz gerekir.

## <a name="manage-an-increased-load-on-a-single-database"></a>Tek bir veritabanındaki artan yükü yönetme

Bir havuzdaki tek bir veritabanı sürekli yüksek bir yük altındaysa, havuz yapılandırmasına bağlı olarak, havuzdaki kaynaklara yön vermeye ve diğer veritabanlarını etkilemeye eğilimli olabilir. Etkinliğin bir süre devam etme olasılığı varsa, veritabanı geçici olarak havuz dışına taşınabilir. Bu durum hem veritabanına havuzdaki diğer veritabanlarından daha fazla kaynak verilmesini sağlar hem de veritabanını diğer veritabanlarından ayırır. Bu alıştırmada, popüler bir konser için biletler satışa çıktığında yüksek bir yükle karşılaşan Contoso Konser Salonu etkisinin benzetimi gerçekleştirilmektedir.

1. …\\**Demo-PerformanceManagementAndMonitoring**.ps1 betiğinde
1. **$DemoScenario = 5, Tek bir kiracı üzerinde normal yük ve yoğun yük oluşturma (yaklaşık 95 DTU)** ayarını yapın.
1. **$SingleTenantDatabaseName = contosoconcerthall** değerini ayarlayın
1. **F5**’i kullanarak betiği yürütün.
1. **Customers1/Pool1** için **Havuz dikey penceresini açın**.
1. Dikey pencerenin üst kısmındaki **Elastik havuz izleme** ekranına bakın ve artan havuz DTU kullanımını arayın. Bir veya iki dakika sonra, daha yüksek olan yük etkisini göstermeye başlar ve havuzun %100 kullanıma ulaştığını görürsünüz.
1. Ayrıca, son bir saat içindeki en yoğun veritabanlarını gösteren **Elastik veritabanı izleme** ekranını izleyin. Contosoconcerthall veritabanı kısa süre içinde en yoğun 5 veritabanı arasında görünmelidir.
1. **Elastik veritabanı izleme** **grafiğine** tıkladığınızda, veritabanlarının herhangi birini seçerek izleyebileceğiniz **Veritabanı Kaynak Kullanımı** dikey penceresi açılır. Bu dikey pencerede contosoconcerthall veritabanının ekranını ayırabilirsiniz.
1. Veritabanları listesinden **contosoconcerthall**’a tıkladığınızda veritabanı dikey penceresi açılır.
1. Bağlam menüsünden **Fiyatlandırma Katmanı (ölçek DTU’ları)** öğesine tıklayarak veritabanının ayrılmış performans düzeyini ayarlayabileceğiniz **Performansı yapılandır** dikey penceresini açın.
1. Standart katmanındaki ölçeklendirme seçeneklerini açmak için **Standart** sekmesine tıklayın.
1. **DTU kaydırıcısını** sağa kaydırarak 100 DTU’yu seçin. Bu değer, DTU ile Depolama boyutu ölçümleri arasında parantez içinde gösterilen **S3** hizmet hedefine karşılık gelir.
1. Veritabanını havuz dışına taşımak ve Standart S3 veritabanı haline getirmek için **Uygula**’ya tıklayın.
1. Dağıtım tamamlandıktan sonra, elastik havuz ve veritabanı dikey pencerelerinde contosoconcerthall veritabanına ve kaldırıldığı havuza etkisini izleyin.

Contosoconcerthall veritabanındaki normalden yüksek yük hafifledikten sonra, maliyetini azaltmak için veritabanını hemen havuza döndürmeniz gerekir. Bunun ne zaman olacağı belli değilse, veritabanında DTU kullanımı havuzdaki veritabanı başına en yüksek değerin altına indiğinde tetiklenecek bir uyarı ayarlayabilirsiniz. Veritabanını havuza taşıma işlemi 5. alıştırmada açıklanmıştır.

## <a name="other-performance-management-patterns"></a>Diğer Performans Yönetimi Düzenleri

**Önleyici ölçeklendirme** Ayırdığınız veritabanını nasıl ölçeklendireceğinizi öğrendiğiniz 6. alıştırmada hangi veritabanına bakmanız gerektiğini biliyorsunuz. Contoso Konser Salonu yönetimi WTP’yi yaklaşan bilet satışından haberdar etseydi, veritabanı önleyici bir eylem olarak havuzdan olarak taşınabilirdi. Aksi takdirde, neler olduğunu belirlemek için havuz veya veritabanı üzerinde bir uyarı verilmesini isteyebilirdi. Bu durumu, havuzda performans düşüklüğünden şikayetçi olan diğer kiracılardan öğrenmek istemezsiniz. Kiracı ek kaynaklara ne süreyle ihtiyaç duyduğunu öngörebiliyorsa, tanımlanmış bir zamanlamaya göre veritabanını havuz dışına ve sonra tekrar içine taşımak için bir Azure Otomasyonu runbook'u ayarlayabilirsiniz.

**Kiracı self-servis ölçeklendirme** Ölçeklendirme, yönetim API’si aracılığıyla kolayca çağrılan bir görev olduğundan, kiracıya yönelik uygulamanızda kiracı veritabanlarını ölçeklendirme özelliğini kolayca oluşturabilir ve SaaS hizmetinizin bir özelliği olarak sunabilirsiniz. Örneğin, kiracıların ölçek artırma ve azaltma işlemini kendi kendine yönetmesine, belki de doğrudan faturalarına bağlamasına izin verebilirsiniz!

### <a name="scaling-a-pool-up-and-down-on-a-schedule-to-match-usage-patterns"></a>Kullanım düzenlerine uygun bir zamanlama ile havuz ölçeğini artırma ve azaltma

Toplu kiracı kullanımının öngörülebilir kullanım düzenlerini takip ettiği durumlarda, belirli bir zamanlamaya göre bir havuzun ölçeğini artırıp azaltmak için Azure Otomasyonu’nu kullanabilirsiniz. Örneğin, kaynak gereksinimlerinde düşüş olduğunu bildiğiniz hafta içi günlerinde havuz ölçeğini akşam 6’dan sonra azaltıp sabah 6’dan önce yeniden artırabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sağlanan bir yük oluşturucuyu çalıştırarak kiracı veritabanlarındaki kullanımın benzetimini gerçekleştirme
> * Yükteki artışa yanıt veren kiracı veritabanlarını izleme
> * Artan veritabanı yüküne yanıt olarak Elastik havuzun ölçeğini artırma
> * Veritabanı etkinliğinin yük dengelemesini yapmak üzere ikinci bir Elastik havuz sağlama

[Tek bir kiracıyı geri yükleme öğreticisi](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [SQL Elastik havuzları](sql-database-elastic-pool.md)
* [Azure otomasyonu](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) - Log Analytics ayarlama ve kullanma öğreticisi
