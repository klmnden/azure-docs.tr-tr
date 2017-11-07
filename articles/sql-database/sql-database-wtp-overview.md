---
title: "Azure SQL veritabanı çok kiracılı uygulama örneği - Wingtip SaaS | Microsoft Docs"
description: "Azure SQL Database, Wingtip SaaS örnek kullanan örnek bir çok kiracılı uygulama kullanarak bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: 46c9a3eadc2c23959b4d08649c6c0215d44b493e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Bir SQL veritabanı çok kiracılı SaaS uygulama örneği giriş

*Wingtip SaaS* SQL veritabanı benzersiz avantajları gösteren örnek bir çok kiracılı uygulama, bir uygulamadır. Uygulama, birden fazla kiracıya hizmet vermek için SaaS uygulama düzeni olan kiracı başına veritabanını kullanır. Uygulama, birçok SaaS tasarım ve yönetim desenleri dahil olmak üzere, SaaS senaryoları etkinleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır. Hızlıca başlamak ve çalıştırmak için beş dakikadan daha kısa bir süre içinde Wingtip SaaS uygulamayı dağıtır!

Uygulama kaynak kodu ve yönetim komut dosyaları kullanılabilir [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. Komut dosyalarını çalıştırmak için [indirme öğrenme modülleri klasörü](#download-and-unblock-the-wingtip-saas-scripts) yerel bilgisayarınıza.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL veritabanı Wingtip SaaS öğreticileri

Uygulamayı dağıttıktan sonra ilk dağıtım sırasında yapı aşağıdaki öğreticileri keşfedin. SQL veritabanı, SQL veri ambarı ve diğer Azure hizmetleriyle yerleşik özelliklerden yararlanmak ortak SaaS desenler bu öğreticileri keşfedin. Öğreticiler anlama ve uygulamalarınızda aynı SaaS Yönetimi desenleri uygulama büyük ölçüde kolaylaştırma ayrıntılı açıklamalar, PowerShell komut dosyaları içerir.


| Öğretici | Açıklama |
|:--|:--|
|[Dağıtma ve Wingtip SaaS uygulamasına keşfedin](sql-database-saas-tutorial.md)| **BURADAN BAŞLAYIN!** Dağıtma ve Azure aboneliğinize Wingtip SaaS uygulamasına keşfedin. |
|[Sağlama ve Katalog kiracılar](sql-database-saas-tutorial-provision-and-catalog.md)| Uygulama Kataloğu veritabanı kullanarak kiracılara nasıl bağlandığını ve Katalog kiracılar verilerini nasıl eşlendiğini öğrenin. |
|[İzleme ve performansı yönetme](sql-database-saas-tutorial-performance-monitoring.md)| SQL veritabanı'nın İzleme özelliklerini kullanmayı ve performans eşikler aşıldığında uyarıları ayarlamak nasıl öğrenin. |
|[Günlük analizi (OMS) ile izleme](sql-database-saas-tutorial-log-analytics.md) | Kullanma hakkında bilgi edinin [günlük analizi](../log-analytics/log-analytics-overview.md) kaynakları, büyük miktarlarda birden çok havuzlardaki izlemek için. |
|[Tek bir kiracı geri yükleme](sql-database-saas-tutorial-restore-single-tenant.md)| Bir kiracı veritabanı zaman içinde önceki bir noktaya geri öğrenin. Varolan Kiracı veritabanı çevrimiçi bırakarak paralel bir veritabanına geri yükleme için adımlar da dahil edilir. |
|[Kiracı şema yönetme](sql-database-saas-tutorial-schema-management.md)| Şemayı Güncelleştir ve tüm Wingtip SaaS kiracılar arasında başvuru verileri güncelleştirmek hakkında bilgi edinin. |
|[Geçici analizler çalıştırır](sql-database-saas-tutorial-adhoc-analytics.md) | Bir geçici analytics veritabanı oluşturun ve tüm kiracılar arasında gerçek zamanlı dağıtılmış sorgular çalıştırın.  |
|[Kiracı analizler çalıştırır](sql-database-saas-tutorial-tenant-analytics.md) | Kiracı veri ambarında çevrimdışı analitik sorguları çalıştırmak için bir analytics veritabanı veya veri ayıklayın. |



## <a name="application-architecture"></a>Uygulama mimarisi

Wingtip SaaS uygulama Kiracı başına veritabanı modeli kullanır ve verimliliğini en üst düzeye çıkarmak için SQL esnek havuzu kullanır. Sağlama ve verilerine eşleme kiracılar için bir katalog veritabanı kullanılır. Wingtip SaaS uygulamasına çekirdek üç örnek kiracılar havuzuyla yanı sıra, Katalog veritabanı kullanır. Öğreticiler eklentileri ilk dağıtıma neden Wingtip SaaS çoğunu Tamamlanıyor, analitik veritabanları sunarak veritabanları arası şema yönetimi, vb..


![Wingtip SaaS mimarisi](media/sql-database-wtp-overview/app-architecture.png)


Şu öğreticileri giderek ve uygulama ile birlikte çalışma sırasında veri katmanı ilgili olarak SaaS düzenlerini esas odaklanmak önemlidir. Başka bir deyişle, veri katmanına odaklanın ve uygulamanın kendisini gereğinden fazla analiz etmeyin. Bu SaaS uygulamasının anlamak desenleri, belirli iş gereksinimlerinizi için gerekli tüm değişiklikleri ınızın uygulamalarınızda bu desenleri uygulama için anahtar.

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Karşıdan yükleme ve Wingtip SaaS betikleri Engellemeyi Kaldır

ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken ***ayıklanıyor önce .zip dosyası engellemesini kaldırmak için aşağıdaki adımları izleyin***. Bu komut dosyalarını çalıştırma izni sağlar.

1. Gözat [Wingtip SaaS github deposuna](https://github.com/Microsoft/WingtipSaaS).
1. Tıklatın **Kopyala veya indir**.
1. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
1. Sağ **WingtipSaaS-master.zip** dosyasını bulun ve seçin **özellikleri**.
1. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**.
1. **Tamam** düğmesine tıklayın.
1. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\WingtipSaaS ana\\öğrenme modülleri* klasör.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Wingtip SaaS PowerShell komut dosyaları ile çalışma

En iyi örnek almak için sağlanan komut dosyalarına daha yakından inceleyin gerekir. Farklı SaaS desenleri nasıl uygulandığını ayrıntılarını inceleyerek komut dosyalarıyla adım ve kesme noktaları kullanın. Sağlanan komut dosyalarını ve modülleri için en iyi anlama aracılığıyla kolayca adım için kullanmanızı öneririz [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Dağıtımınız için yapılandırma dosyasını güncelleştir

Düzen **UserConfig.psm1** dosya dağıtımı sırasında ayarladığınız kaynak grubu ve kullanıcı değerine sahip:

1. Açık *PowerShell ISE* ve yükle... \\Modülleri öğrenme\\*UserConfig.psm1* 
1. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
1. Değişiklikleri kaydedin!

Bu değerleri ayarı burada basitçe, her komut dosyası bu dağıtım özgü değerleri güncelleştirmek zorunda kalmaktan tutar.

### <a name="execute-scripts-by-pressing-f5"></a>F5’e basarak Betikleri çalıştırma

Birkaç betiklerini kullanın *$PSScriptRoot* klasörleri gidin ve *$PSScriptRoot* tuşuna basarak komut yürütüldüğünde yalnızca değerlendirilir **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) neden hataları, bu nedenle basın **F5** betikleri çalışırken.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Uygulamayı incelemek üzere betiklerde ilerleme

Komut dosyalarını anlamak için en iyi ne yaptıklarını görmek için aralarında adımla yoludur. Dahil edilen denetleyin **Demo -** kolay bir üst düzey iş akışı izleyin sunmak komut dosyaları. **Demo -** komut dosyaları göster her görevi, kesme noktaları olacak şekilde ayarlamanız ve incelemek için gerekli adımları derin farklı SaaS desenler için uygulama ayrıntılarını görmek için tek tek çağrıları içine.

Keşfetmek ve PowerShell komut dosyalarıyla Adımlama ipuçları:

* Açık **Demo -** PowerShell ISE komut.
* Execute veya devam **F5** (kullanarak **F8** çünkü önerilmez *$PSScriptRoot* seçimleri komut dosyası çalıştırılırken değerlendirilmez).
* Bir çizgiye tıklayarak veya çizgiyi seçerek ve **F9**’a basarak kesme noktaları yerleştirin.
* **F10**’u kullanarak bir işlev veya betiği atlayın.
* **F11**’i kullanarak bir işlev veya betiğe gidin.
* **Shift + F11**’i kullanarak geçerli işlev veya betikten çıkın.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Veritabanı şemasını keşfetme ve SSMS kullanarak SQL sorguları yürütme

Kullanım [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmayı ve uygulama sunucuları ve veritabanları göz atın.

Dağıtım - bağlanmak için iki SQL veritabanı sunucularının başlangıçta sahip *tenants1 -&lt;kullanıcı&gt;*  sunucu ve *katalog -&lt;kullanıcı&gt;*  Sunucu. Başarılı demo bağlantı sağlamak için her iki sunucuyu sahip bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) aracılığıyla tüm IP'ler izin verme.


1. *SSMS*’yi açın ve *tenants1-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.
1. **Bağlan** > **Veritabanı Altyapısı...**:

   ![katalog sunucusu seçeneğine tıklayın](media/sql-database-wtp-overview/connect.png)

1. Tanıtım kimlik bilgileri: Kullanıcı adı = *developer*, Parola = *P@ssword1*

   ![bağlantı](media\sql-database-wtp-overview\tenants1-connect.png)

1. 2-3. adımları tekrarlayın ve *catalog-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.

Başarıyla bağlandıktan sonra her iki sunucuyu da görmeniz gerekir. Veritabanlarının listesini sağlanan kiracılar bağlı olarak farklı olabilir:

![nesne gezgini](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar

[Wingtip SaaS uygulamasına dağıtmak](sql-database-saas-tutorial.md)
