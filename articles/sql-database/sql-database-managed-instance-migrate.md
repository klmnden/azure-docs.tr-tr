---
title: Veritabanını Azure SQL veritabanı - yönetilen örnek için SQL Server örneğinden geçirme | Microsoft Docs
description: Bir veritabanını Azure SQL veritabanı - yönetilen örnek için SQL Server örneğinden geçirmeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
manager: craigg
ms.date: 02/11/2019
ms.openlocfilehash: 9fe6ab797eaa325ad802702e95f5a0e5b8e4fef4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070421"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server örneği geçiş Azure SQL veritabanı yönetilen örneği

Bu makalede, bir SQL Server 2005 veya üzeri sürümü örneğine geçirmek için yöntemler hakkında bilgi [Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance.md). Bir tek veritabanı veya elastik Havuzu'nu geçirme hakkında daha fazla bilgi için bkz: [tek veya havuza alınmış bir veritabanı geçiş](sql-database-cloud-migrate.md). Diğer platformlardan geçirme hakkında geçiş için bilgi [Azure veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).

Yüksek düzeyde, veritabanı geçiş işlemi aşağıdaki gibi görünür:

![Geçiş işlemi](./media/sql-database-managed-instance-migration/migration-process.png)

- [Yönetilen örnek uyumluluğunu değerlendirmek](#assess-managed-instance-compatibility)
- [Uygulama bağlantı seçeneğini belirleme](sql-database-managed-instance-connect-app.md)
- [En iyi şekilde boyutlandırılmış yönetilen örneğine dağıtma](#deploy-to-an-optimally-sized-managed-instance)
- [Geçiş yöntemi seçin ve geçirme](#select-migration-method-and-migrate)
- [Uygulama izleme](#monitor-applications)

> [!NOTE]
> Tek bir veritabanını tek veritabanı veya elastik havuzun geçirmek için bkz [bir SQL Server veritabanını Azure SQL veritabanı'na geçirme](sql-database-single-database-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Yönetilen örnek uyumluluğunu değerlendirmek

İlk olarak, yönetilen örneği, uygulamanızın veritabanı gereksinimleriyle uyumlu belirlemek. Yönetilen örnek dağıtım seçeneği, kolay lift and shift ile geçiş için çoğu şirket içi SQL Server kullanan mevcut uygulamaları ya da sanal makineler sağlamak için tasarlanmıştır. Ancak, bazen özellikleri gerektirebilir veya henüz desteklenmeyen bazı özellikler ve geçici bir çözüm uygulama maliyeti çok yüksek.

Kullanım [Data Migration Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) olası algılamak için veritabanı işlevselliğini etkileyen Azure SQL veritabanı uyumluluk sorunları. DMA'yı desteklemez henüz destek yönetilen örnek geçişi hedef olarak ancak değerlendirmesi, Azure SQL veritabanında çalıştırın ve dikkatli bir şekilde bildirilen özellik eşliği ve uyumluluk sorunlarına karşı ürün belgelerinin listesini incelemek için tavsiye edilir. Bkz: [Azure SQL veritabanı özellikleri](sql-database-features.md) denetlemek için değil yönetilen örneğinde blockers ile bir Azure SQL veritabanına geçiş önleme engelleme sorunlarını çoğunu kaldırıldığı için yönetilen bir engelleyici soruna bazı bildirilen vardır örneği. Örneği, platformlar arası sorguları, aynı örnek veritabanları arası işlemleri, diğer SQL kaynakları, CLR, genel geçici tabloların bağlı sunucusuna gibi özellikler için örnek düzeyi görünümleri, hizmet aracısı ve benzeri yönetilen durumlarda kullanılabilir.

Varsa bazı bildirilen yönetilen örnek dağıtım seçeneğiyle kaldırılmaz engelleme sorunları, alternatif bir seçenek gibi düşünün gerekebilir [Azure sanal makineler'de SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/). Bazı örnekler şunlardır:

- İşletim sistemi veya dosya sistemi, yükleme üçüncü taraf veya özel aracıları aynı sanal makinede SQL Server örneği için doğrudan erişim gerekiyorsa.
- Yine, FILESTREAM gibi desteklenmeyen özelliklerle ilgili katı bağımlılığı varsa / FileTable, PolyBase ve çapraz örnek işlemleri.
- Belirli bir SQL Server sürümünde kalmak kesinlikle gerekli (2012 örneği için).
- Bu yönetilen örneği, bilgi işlem gereksinimlerinizi çok daha düşük olduğunda sunar (bir sanal çekirdek, örneği için) ve veritabanı birleştirme kabul edilebilir bir seçenek değil.

Tüm geçiş engelleyiciler ve yönetilen örneğe geçiş devam tanımlanan çözdüyseniz, bazı değişiklikler İş yükünüzün performansını etkileyebilir dikkat edin:
- Düzenli aralıklarla basit/toplu günlüğe modeli kullanılan veya isteğe bağlı yedeklemeler durdurulur zorunlu tam kurtarma modeli ve normal otomatik yedekleme zamanlaması bakım/ETL işlemleri veya iş yükü performansını etkileyebilir.
- Farklı sunucu veya veritabanı düzeyinde veya gibi yapılandırmaları izleme bayrakları Uyumluluk Düzeyleri
- Saydam veritabanı şifrelemesi (TDE) veya otomatik yük devretme grupları gibi kullandığınız yeni özellikler, CPU ve g/ç kullanımını etkileyebilir.

Bu özellikler tarafından neden yükünü devre dışı bile senaryolarda kritik örneği garantisi % 99,99 kullanılabilirlik yönetilen. Daha fazla bilgi için [farklı performans ve SQL Server yönetilen örneği neden olabilecek kök nedenleri](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/).

### <a name="create-performance-baseline"></a>Performans taban çizgisi oluşturma

SQL sunucusu üzerinde çalışan, özgün iş yükü ile yönetilen örneği, bir iş yükü performansını karşılaştırmak gerekiyorsa, karşılaştırma için kullanılacak olan performans taban çizgisi oluşturmak gerekir. Bazı ölçmek için SQL Server örneğinde gereken Parametreler şunlardır: 
- [CPU kullanımı, SQL Server örneğinde izleme](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) ortalama kaydetmek ve en yüksek CPU kullanımı.
- [SQL Server örneğinde bellek kullanımını izleyecek](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage) ve arabellek havuzu gibi başka bileşenleri kullandığı bellek miktarını belirlemek önbellek, sütun depolama havuzu planlama [bellek içi OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017)vb. Ayrıca, sayfa yaşam beklentisinin bellek performans sayacının ortalama ve en üst seviyeye değerleri bulmanız gerekir.
- Kaynak SQL Server örneği kullanarak disk GÇ kullanım izleme [sys.dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) görünümü veya [performans sayaçları](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage).
- İş yükü ve sorgu performansını veya SQL Server örneğinizi SQL Server 2016 + sürümünden geçiş yapıyorsanız dinamik yönetim görünümleri ya da Query Store inceleyerek izleyin. Ortalama süresi ve en önemli sorgular yönetilen örneği'nde çalışan sorguları karşılaştırmak için iş yükünüz, CPU kullanımı belirleyin.

> [!Note]
> SQL Server'da, iş yüküyle yüksek CPU kullanımı, sabit bir bellek baskısı, tempdb veya parametrization sorunları gibi herhangi bir sorunla karşılaşırsanız, taban çizgisi ve geçiş yapmadan önce kaynak SQL Server örneğinde çözümleyin çalışmanız gerekir. Geçiş bildiğiniz herhangi yeni bir sistem migh sorunları beklenmeyen sonuçlara neden ve herhangi bir performans karşılaştırma geçersiz.

Bu etkinliğin sonucu, ortalama ve en yüksek değerler için CPU, bellek ve GÇ kullanımını kaynak sisteminizde yanı sıra ortalama ve maksimum süre ve CPU kullanımını baskın ve en önemli sorguları iş yükünüzü belgelemesi gerekir. Bu değerler daha sonra iş yüküne kaynak SQL Server'ın temel performansından memnun yönetilen örneği İş yükünüzün performansını karşılaştırmak için kullanmanız gerekir.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>En iyi şekilde boyutlandırılmış yönetilen örneğine dağıtma

Yönetilen örnek buluta taşımak için planlama şirket içi iş yükleri için uygun hale getirilir. Tanıttığı bir [yeni satın alma modeli](sql-database-service-tiers-vcore.md) seçerken doğru düzeyde kaynakları iş yükleriniz için büyük esneklik sağlar. Şirket içi dünyasında, fiziksel çekirdek ve g/ç bant genişliği'ni kullanarak bu iş yükleri boyutlandırma için büyük olasılıkla alışkın olduğunuz. Yönetilen örnek için satın alma modeli temel sanal çekirdek ya da "Şununla," ek depolama alanı ve kullanılabilir GÇ ayrı olarak temel alır. VCore modeli bir basittir ve kullandığınız bulut işlem gereksinimlerinizi anlamak için şirket içi bugün. Bu yeni modeli, hedef ortamınızda bulut için doğru boyutu sağlar. Doğru hizmet katmanı ve özelliklerini seçmek için yardımcı olabilecek bazı genel yönergeleri aşağıda açıklanmıştır:
- [CPU kullanımı, SQL Server örneğinde izleme](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) ve ne kadar işlem şu anda kullandığınız (Dinamik Yönetim görünümleri, SQL Server Management Studio veya diğer izleme araçları kullanarak) güç denetimi. CPU özelliklerine uygun şekilde ölçeklendirilmesine gerekebileceğini aklınızda sahip SQL Server'da kullandığınız çekirdek eşleşen yönetilen örneğe sağlayabileceğiniz [yönetilen örneğin yüklendiği VM özelliklerine](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#hardware-generation-characteristics).
- Kullanılabilir bellek miktarı, SQL Server örneğinde işaretleyin ve [eşleşen belleğe sahip hizmet katmanı](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#hardware-generation-characteristics). Sayfa yaşam beklentisinin belirlemek için SQL Server örneğinde ölçmek yararlı olabilecek [ek bellek ihtiyacınız](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444).
- Genel amaçlı ve iş açısından kritik hizmet katmanı arasında tercih yapma dosya alt sisteminin g/ç gecikme süresini ölçme.

İşlem seçebilirsiniz ve depolama kaynakları dağıtım süresi ve daha sonra Tanıtımı kullanarak uygulamanızı için kapalı kalma süresi olmadan değiştirin [Azure portalında](sql-database-scale-resources.md):

![Yönetilen örnek boyutlandırma](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Sanal ağ altyapısı ve yönetilen örnek oluşturma hakkında bilgi edinmek için [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> Hedef sanal ağ ve alt ağ her zaman içinde belge tutmak önemlidir [yönetilen örnek sanal ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Hiçbir uyumsuzluk yeni kopyalarını oluşturmak veya önceden oluşturulmuş bu kullanarak engelleyebilirsiniz. Daha fazla bilgi edinin [yeni oluşturma](sql-database-managed-instance-create-vnet-subnet.md) ve [mevcut yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) ağlar.

## <a name="select-migration-method-and-migrate"></a>Geçiş yöntemi seçin ve geçirme

Şirket içi veya Iaas yığın veritabanı geçişi gerektiren yönetilen örnek dağıtım seçeneği hedefleri kullanıcı senaryoları uygulamaları veritabanı. Lift- and -shift arka uç, düzenli olarak örnek düzeyi kullanın ve / veya platformlar arası işlevleri uygulamaların gerektiğinde bunlar en iyi seçimdir. Senaryonuz buysa, karşılık gelen bir Azure ortamında, uygulamalarının mimarisini yeniden tasarlamak zorunda kalmadan tüm örneğine taşıyabilirsiniz.

SQL örnekleri taşımak için dikkatli bir şekilde planlamanız gerekir:

- (Çalışan aynı örneğinin üzerinde birlikte bulunan olanlar) olması gereken tüm veritabanlarının geçiş
- Oturum açma bilgileri, kimlik bilgileri, SQL Aracısı işleri ve işleçler ve sunucu düzeyi Tetikleyiciler dahil olmak üzere, uygulamanızın bağımlı örnek düzeyi nesneleri geçirme.

Yönetilen örnek, yerleşik olarak gibi platforma normal DBA etkinliklerin bazıları temsilci olanak tanıyan bir yönetilen hizmettir. Bu nedenle, bazı örnek düzeyi verileri, düzenli yedeklemeler veya Always On yapılandırması için bakım işleri gibi olarak geçirilmesi gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Yönetilen örnek (şu anda bunlar yalnızca desteklenen geçiş yöntemleridir) aşağıdaki veritabanı geçiş seçeneklerini destekler:

- Azure veritabanı geçiş hizmeti - sıfıra yakın kapalı kalma süresiyle geçiş
- Yerel `RESTORE DATABASE FROM URL` - yerel SQL Server yedeklemeleri kullanır ve bazı kapalı kalma süresi gerektirir.

### <a name="azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti

[Azure veritabanı geçiş hizmeti (DMS)](../dms/dms-overview.md) birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure Data platformlarına sağlamak için tasarlanmış, tam olarak yönetilen bir hizmettir. Bu hizmeti mevcut üçüncü taraf ve SQL Server veritabanlarını Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri genel Önizleme sırasında Azure SQL veritabanı ve SQL Server veritabanlarını bir Azure sanal Makinesi'nde veritabanları ekleyin. DMS önerilen geçiş, kurumsal iş yükleri için yöntemidir.

Şirket içi SQL Server üzerinde SQL Server Integration Services (SSIS) kullandığınız DMS SSIS paketlerini depolar geçirme SSIS kataloğunu (SSISDB) henüz desteklemiyor, ancak Azure-SSIS Integration Runtime (IR) Azure Data Factory (ADF) sağlayabilirsiniz, olur Yeni bir SSISDB içinde yönetilen örnek oluşturma ve paketlerinizi yeniden dağıtım yapabileceklerini daha sonra bkz: [ADF içinde Azure-SSIS IR oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

DMS için bu senaryo ve yapılandırma adımları hakkında daha fazla bilgi edinmek için [şirket içi veritabanınızı DMS kullanarak yönetilen örneğe geçirme](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>URL yerel YEDEKTEN geri yükleyin

SQL Server şirket içinden alınan yerel yedeklemeler (.bak dosyaları) geri yükleme veya [sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)üzerinden [Azure depolama](https://azure.microsoft.com/services/storage/), bir yönetilen örnek dağıtım önemli özellikleri hızlı ve kolay çevrimdışı veritabanı geçişi sağlayan seçeneği.

Aşağıdaki diyagram, işlemin üst düzey bir genel bakış sağlar:

![geçiş akışı](./media/sql-database-managed-instance-migration/migration-flow.png)

Aşağıdaki tabloda, kaynak SQL Server sürümüne bağlı olarak kullanabileceğiniz yöntemleri çalıştırdığınız hakkında daha fazla bilgi sağlar:

|Adım|SQL altyapısı ve sürüm|Yedekleme / geri yükleme yöntemi|
|---|---|---|
|Azure Storage'a yedekleme yerleştirin|Önceki SQL 2012 SP1 CU2|.Bak dosyası doğrudan Azure depolamaya yükleme|
||2012 SP1 CU2 - 2016|Kullanım dışı doğrudan Yedekleme kullanılarak [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) söz dizimi|
||2016 ve üzeri|Doğrudan Yedekleme kullanılarak [ile SAS kimlik bilgisi](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Azure depolama biriminden yönetilen örneğine geri yükleyin.|[Geri yükleme kaynak URL ile SAS kimlik bilgisi](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Tarafından korunan bir veritabanını geçirirken [saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md) yerel bir geri yükleme seçeneğini kullanarak bir yönetilen örneğine karşılık gelen sertifika şirket içi veya Iaas SQL Server veritabanı önce geçirilmesi gerekiyor geri yükleme. Ayrıntılı adımlar için bkz. [yönetilen örneğe geçirme TDE cert](sql-database-managed-instance-migrate-tde-certificate.md)
> - Sistem veritabanlarının geri yükleme desteklenmiyor. Örnek düzeyi nesneler (ana veya msdb veritabanlarında depolanan) geçirmek için bunları komut dosyası ve hedef örneğinde T-SQL betiklerini çalıştırma öneririz.

Bir SAS kimlik bilgisi kullanarak bir yönetilen örnek veritabanı yedeklemesini geri yükleme gösteren Hızlı Başlangıç için bkz: [yedekten bir yönetilen örneğine geri](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]


## <a name="monitor-applications"></a>Uygulamaları izleme

Yönetilen örnek geçişi tamamladıktan sonra İş yükünüzün performansını ve uygulama davranışını izlemelisiniz. Bu işlem aşağıdaki etkinlikleri içerir:
- [Yönetilen örneğinde çalışan iş yükü performansını karşılaştırmak](#compare-performance-with-the-baseline) ile [oluşturduğunuz kaynak SQL Server performans taban çizgisi](#create-performance-baseline).
- Sürekli olarak [İş yükünüzün performansını izleme](#monitor-performance) olası sorunlar ve geliştirme tanımlamak için.

### <a name="compare-performance-with-the-baseline"></a>Performans taban çizgisi ile Karşılaştır

Başarılı geçiş sonrasında hemen almak için gereken ilk iş yükü performansının taban çizgisi iş yükü performansı ile Karşılaştırılacak etkinliğidir. Amacı, bu etkinlik, yönetilen Örneğinize iş yükü performansını gereksinimlerinizi karşıladığını doğrulamaktır. 

Yönetilen örneği için veritabanı geçişi, veritabanı ayarlarını ve özgün uyumluluk düzeyi çoğu durumda tutar. Mümkün olduğunda, kaynak SQL Server kıyasla bazı performans performansındaki düşüşleri riskini azaltmak için özgün ayarları korunur. Bir kullanıcı veritabanı uyumluluk düzeyini 100 veya daha yüksek geçişten önce, aynı geçişten sonra kalır. Bir kullanıcı veritabanı uyumluluk düzeyi 90 yükseltilen veritabanında, geçiş işleminden önce olduysa uyumluluk düzeyi en düşük desteklenen uyumluluk düzeyini yönetilen örneğinde olan 100'e ayarlanır. Sistem veritabanlarının uyumluluk düzeyi, 140 değeri. Yönetilen örneğe geçiş gerçekten SQL Server Veritabanı Altyapısı'nın en son sürüme geçirme olduğundan yeniden şaşırtıcı bazı performans sorunlarından kaçınmak için İş yükünüzün performansını test etmek ihtiyacınız farkında olmalıdır.

Bir önkoşul olarak aşağıdaki etkinlikleri tamamladığınızdan emin olun:
- Yönetilen örneği ayarlarınızı ayarlarla kaynak SQL Server örneğinden çeşitli örneği, veritabanı, temdb ayarları ve yapılandırmaları incelenerek hizalayın. İlk performans karşılaştırma çalıştırmadan önce Uyumluluk Düzeyleri veya şifreleme gibi ayarları değiştirmediğinizi emin olun veya etkinleştirdiğiniz yeni özelliklerinden bazıları etkileyebilecek bazı sorgular riskiyle kabul edin. Geçiş risklerini azaltmak için yalnızca performansı izleme sonra veritabanı uyumluluk düzeyini değiştirin.
- Uygulama [genel amaçlı depolama en iyi uygulama kılavuzları](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525) gibi daha iyi performans almak için dosyaların boyutunu önceden ayrılıyor.
- Hakkında bilgi edinin [önemli yönetilen örneği SQL Server arasındaki performans farkı neden olabilecek ortam farklar]( https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) ve performansını etkileyebilir risklerini tanımlayın.
- Query Store etkin ve yönetilen Örneğinize otomatik ayarlama tutmak olduğundan emin olun. Bu özellikleri iş yükü performansını ölçmek ve olası performans sorunlarını otomatik olarak düzeltmeye olanak sağlar. Query Store en uygun bir araç olarak iş yükü performansı önce ve sonra veritabanı uyumluluk düzeyi değiştirme hakkında bilgi almak için açıklandığı şekilde kullanmayı öğrenin [tutmak performans kararlılık yükseltme sırasındadahayeniSQLServersürümü](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).
Karşılaştırılabilir ortamı hazırlandıktan sonra şirket içi ortamınıza mümkün olduğunca yükünüzü çalıştıran başlatabilir ve performansı ölçme. Ölçüm işlemi, ölçülen aynı parametreleri içermelidir [kaynak SQL Server, iş yükü ölçülerin performansınız oluştururken](#create-performance-baseline).
Sonuç olarak, performans taban çizgisi parametrelerle karşılaştırın ve önemli farkları tanımlamak.

> [!NOTE]
> Çoğu durumda, yönetilen örneği ve SQL Server üzerinde tam olarak eşleşen performansı elde etmek mümkün olmaz. Yönetilen örnek bir SQL Server veritabanı altyapısıdır ancak bazı fark, altyapı ve yönetilen örneği, yüksek kullanılabilirlik yapılandırmasında neden olabilir. Diğer bir kısmının daha yavaş olabilir ancak bazı sorguları daha hızlı İmparatoru bekleyebilirsiniz. Yönetilen örnek iş yükü performansını performans üzerinde SQL Server (ortalama) eşleştiğini doğrulayın ve tanımlamak için karşılaştırma amacının olan özgün performansınızı eşleşmeyen performansa sahip kritik sorgular vardır.

Performans karşılaştırma sonucunu olabilir:
- Yönetilen örnek iş yükü performansı hizalanır veya, daha iyi SQL Server iş yükü performansını. Bu durumda, geçiş işleminin başarılı olduğunu başarıyla doğruladı.
- Performans parametrelerini ve iş yükü iş güzel, performansın düşmesine neden olan bazı özel durumlar sorguları çoğunluğu. Bu durumda, farklar ve önemini tanımlamak gerekir. Bazı önemli performans sorgularla varsa, araştırmanız gereken temel SQL planları değiştirilir veya sorguları bazı kaynak sınırları karşılaştınız demektir. Risk azaltma, bazı ipuçları gereken kritik sorguları (örneğin değiştirilmiş bir uyumluluk düzeyi, eski kardinalite tahmin) ya da doğrudan uygulamak veya planı kılavuzlarını kullanarak yeniden oluşturmanız veya istatistikleri ve planları etkileyebilecek dizinler oluşturmak için bu durumda olabilir. 
- Sorguların çoğu, yönetilen örneği, kaynak SQL Server karşılaştırıldığında yavaştır. Bu durumda deneyin gibi fark kök nedenlerini belirlemek [bazı kaynak sınırınıza ulaşmanız]( sql-database-managed-instance-resource-limits.md#instance-level-resource-limits) GÇ sınırlarından, bellek sınırı, örnek günlük hız sınırı vb. ister. Fark neden olabilecek hiçbir kaynak sınırları varsa, veritabanı uyumluluk düzeyini değiştirmeyi deneyin veya test yeniden başlatın ve eski kardinalite tahmini gibi veritabanı ayarlarını değiştir. Performans gerileyen sorguları belirlemek için yönetilen örneği veya Query Store görünümler tarafından sağlanan önerileri gözden geçirin.

> [!IMPORTANT]
> Yönetilen örnek, varsayılan olarak etkin yerleşik otomatik plan düzeltme özelliği vardır. Yapıştırma seçeneğiyle düzgün çalışan sorguları gelecekte düşürmeyecektir, bu özelliği sağlar. Bu özellik etkin olduğundan ve temel performans ve planları hakkında bilgi edinmek yönetilen örneği'ni etkinleştirmek için yeni ayarları değiştirmeden önce iş yükünü yeterince uzun eski ayarlarla yürüttünüz emin olun.

Parametrelerden biri değişikliği yapın veya hizmet katmanları için en uygun yapılandırma gereksinimlerinize en uygun iş yükü performansı elde edene kadar yakınsanmasını yükseltin.

### <a name="monitor-performance"></a>Performansı izleme

Tam olarak yönetilen bir platformda ve iş yükü performanslarını SQL Server iş yükü performansı değişkenler doğruladıktan sonra SQL veritabanı hizmetinin bir parçası olarak otomatik olarak sağlanan avantajlar yararlanın. 

Geçiş sırasında yönetilen örneğinde bazı değişiklikler yapmayın olsa bile, en son veritabanı altyapısı iyileştirmelerden yararlanmak için örneğinizin çalıştırırken, bazı yeni özellikleri etkinleştirmek yüksek olasılığı vardır. Bazı değişiklikler yalnızca bir kez etkinleştirilir [veritabanı uyumluluk düzeyi değiştirildi](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database).


Örneğin, yedeklemeleri yönetilen örneğinde oluşturmanız gerekmez - hizmet yedeklemeleri sizin için otomatik olarak gerçekleştirir. Artık zamanlama, alma ve yedekleri yönetme hakkında endişe gerekir. Yönetilen örnek kullanarak bu elde tutma dönemi içinde zaman içinde herhangi bir noktasına geri yükleme olanağı sağlayan [işaret zaman Kurtarma (PITR) içinde](sql-database-recovery-using-backups.md#point-in-time-restore). Ayrıca, yüksek oranda kullanılabilir ayarlama endişelenmeniz gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Güvenliği güçlendirmek için kullanmayı [Azure Active Directory kimlik doğrulaması](sql-database-security-overview.md), [denetim](sql-database-managed-instance-auditing.md), [tehdit algılama](sql-database-advanced-data-security.md), [satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), ve [dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ).

Gelişmiş Yönetim ve güvenlik özelliklerine ek olarak, yönetilen örneği, size yardımcı olabilecek Gelişmiş araçlar kümesi sağlar. [izleme ve ayarlama iş yükünüz](sql-database-monitor-tune-overview.md). [Azure SQL analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) çok sayıda yönetilen örnekler izlemenizi ve çok sayıda örneklerini ve veritabanlarını izleme merkezileştirmenizi sağlar. [Otomatik ayarlama](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) yönetilen örneği, sürekli olarak, SQL planı yürütme istatistikleri performansını izleme ve otomatik olarak tanımlanan performans sorunları giderin.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekleri hakkında daha fazla bilgi için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Bir yedekten içeren bir öğretici için bkz. [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- DMS kullanarak öğretici gösteren geçiş için bkz: [şirket içi veritabanınızı DMS kullanarak yönetilen örneğe geçirme](../dms/tutorial-sql-server-to-managed-instance.md).  
