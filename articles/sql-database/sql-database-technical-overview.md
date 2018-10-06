---
title: Azure SQL Veritabanı hizmeti nedir? | Microsoft Docs
description: "SQL Database ile tanışın: Microsoft'un bulut üzerindeki ilişkisel veritabanı yönetim sistemine (RDBMS) ilişkin teknik ayrıntılar ve özellikler."
keywords: sql'e giriş,sql veritabanı nedir
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: 747ef66b3d60e42e0d28669fb58364fd7c393438
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831239"
---
# <a name="the-azure-sql-database-service"></a>Azure SQL veritabanı hizmeti

SQL Veritabanı, Microsoft Azure'da yer alan ve ilişkisel veri, JSON, uzamsal ve XML gibi yapıları destekleyen çok amaçlı ilişkisel veritabanı yönetilen hizmetidir. SQL Veritabanı, iki farklı satın alma modeli [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) arasında dinamik olarak ölçeklenebilir performans sunar. SQL Veritabanı ayrıca çok büyük ölçekli analitik analiz ve raporlama için [columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) ve raporlama ve çok büyük ölçekli işlemler için [bellek içi OLTP](sql-database-in-memory.md) gibi seçenekler sunar. Microsoft, SQL kod tabanıyla ilgili tüm düzeltme ve güncelleştirme işlerini sorunsuz olarak yaparak altyapı yönetimini tamamen soyutlar.

Azure SQL Veritabanı, bir Azure SQL veritabanı için aşağıdaki dağıtım seçeneklerini sunar:

- Mantıksal sunucu aracılığıyla yönetilen ve kendi kaynak kümesine sahip tek bir veritabanı
- Mantıksal sunucuyla yönetilen paylaşılan kaynak kümesine sahip [elastik havuz](sql-database-elastic-pool.md) içinde havuza eklenmiş bir veritabanı
- Bir koleksiyon olarak bilinen veritabanlarının bir parçası olarak bir [yönetilen örnek](sql-database-managed-instance.md) sistem ve kullanıcı veritabanlarını ve bir kaynak kümesini paylaşımı içerir

Aşağıdaki şekilde bu dağıtım seçenekleri gösterilmektedir:

![dağıtım seçenekleri](./media/sql-database-technical-overview/deployment-options.png) 

SQL Veritabanı, [Microsoft SQL Server veritabanı altyapısı](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation) ile aynı kod tabanını kullanır. Microsoft'un bulut öncelikli stratejisi kapsamında en yeni SQL Server özellikleri önce SQL Veritabanı'na, ardından da SQL Server'ın kendine uygulanır. Bu yaklaşım sayesinde düzeltme veya yükseltme zahmetine girmeden milyonlarca veritabanında test edilmiş en yeni SQL Server özelliklerine sahip olabilirsiniz. Yeni özellikler açıklandıkça haberdar olmak için bkz:

- **[SQL Veritabanı için Azure Yol Haritası](https://azure.microsoft.com/roadmap/?category=databases)**: Bu ürün yol haritasında, yenilikleri ve gelecekte sizi nelerin beklediğini bulabilirsiniz.
- **[Azure SQL Veritabanı blogu](https://azure.microsoft.com/blog/topics/database)**: SQL Server ürün ekibi üyelerinin SQL Veritabanı haberleri ve özellikleri hakkında yazdıkları blog.

> [!IMPORTANT]
> SQL Veritabanı ile SQL Server arasındaki özellik farklarını anlamak için bkz. [SQL özellikleri](sql-database-features.md).

SQL veritabanı ile birden çok kaynak türleri, hizmet katmanları ve bilgi işlem boyutlarına hiç kapalı kalma süresi, yerleşik zeka iyileştirmesi, global ölçeklenebilirlik ve kullanılabilirlik ve Gelişmiş Güvenlik ile dinamik ölçeklenebilirlik sunan, tahmin edilebilir performans sunar. seçenekleri — neredeyse sıfır yönetim gereksinimiyle. Bu özellikler sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. SQL Veritabanı şu anda dünyanın farklı yerlerindeki toplam 38 veri merkezini kullanmaktadır ve düzenli aralıklarla eklenen yeni veri merkezleri, veritabanınızı bulunduğunuz konuma yakın bir veri merkezinde çalıştırmanızı sağlar.

## <a name="scalable-performance-and-pools"></a>Ölçeklenebilir performans ve havuzlar

SQL veritabanı ile her veritabanı birbirine ve taşınabilir, her biri kendi hizmet katmanı içinde yalıtılmış [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) ve bir kesin boyutu işlem. SQL veritabanı farklı ihtiyaçlara uygun farklı bilgi işlem boyutlarına sağlar ve için veritabanlarının havuza eklenebilmesini kaynaklarının kullanımını en üst düzeye çıkarmak ve paradan tasarruf sağlar.

- İle [SQL veritabanı yönetilen örneği](sql-database-managed-instance.md), her örnek, garanti edilen kaynaklarla diğer örneklerden yalıtılır. Daha fazla bilgi için bkz. [SQL Veritabanı Yönetilen Örneği](sql-database-managed-instance.md).
- İle [hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md) (Önizleme) satın alma modeli vcore değeri, 100 TB için hızlı yedekleme ile ölçeklendirin ve geri yükleme.

### <a name="adjust-performance-and-scale-without-downtime"></a>Kesinti olmadan performansı ayarlama ve ölçeklendirme

SQL Veritabanı bir [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) veya [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) sunar.

- DTU tabanlı satın alma modeli, basit ve ağır veritabanı iş yüklerini desteklemek için üç hizmet katmanında işlem, bellek ve G/Ç kaynakları karışımını sunar: Temel, Standart ve Premium. İşlem boyutları her katman içinde ek depolama kaynakları ekleyebileceğiniz, bu kaynakları farklı bir karışımını sağlar.
- Sanal çekirdek tabanlı satın alma modeli, sanal çekirdek sayısını, bellek miktarını ve depolama alanı miktarını ve hızını seçmenize olanak sağlar.

Düşük bir maliyetle genel amaçlı hizmet katmanındaki aylık küçük, tek bir veritabanı üzerinde ilk uygulamanızı oluşturun ve ardından Hizmet katmanını elle veya programlama yoluyla herhangi bir zamanda iş kritik hizmet katmanına çözümünüzün gereksinimlerini karşılayacak şekilde değiştirebilirsiniz. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

> [!IMPORTANT]
> [Hiper ölçekli hizmet katmanı](sql-database-service-tier-hyperscale.md) şu anda genel Önizleme aşamasındadır. Tüm üretim iş yüklerini hiper ölçekli veritabanlarında henüz çalıştıran önerilmemektedir. Diğer hizmet katmanları için bir hiper ölçekli veritabanı güncelleştirilemiyor. Test amacıyla geçerli veritabanınızın bir kopyasını alın ve kopyasını hiper ölçekli hizmet katmanına güncelleştirmek önerilir...

Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. Bir hizmet ölçütlere dayalı olarak otomatik şekilde ölçeklendirildiğinde otomatik ölçeklendirme oluşurken, dinamik ölçeklenebilirlik ise kesinti süresi olmadan el ile ölçeklendirmeye olanak sağlar. Tek Azure SQL Veritabanı, el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi desteklemez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın. Ancak, tek bir Azure SQL Veritabanı için ölçeklenebilirliğin otomatikleştirilmesine yardımcı olabilecek betikler vardır. Örnek için bkz. [PowerShell kullanarak tek bir SQL Veritabanını izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md).

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Kaynak kullanımını en verimli hale getirmek için elastik havuzlar

Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırarak tek bir veritabanının performansı için değil havuzun ortak performans kaynakları için ödeme yaparsınız.

   ![elastik havuzlar](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Elastik havuzlar sayesinde kaynak talebindeki dalgalanmalara ayak uydurmak için veritabanı performansında sürekli ayarlama yapmanız gerekmez. Havuza alınmış veritabanları, gerektiğinde elastik havuzun performans kaynaklarını tüketir. Havuza alınan veritabanları havuzu kullanır ancak havuz sınırlarını aşmaz, böylece veritabanı kullanımınız tahmin edilebilir olmasa bile maliyetleriniz için durum tam tersidir. Ayrıca kontrolü sizin elinizde olan bir bütçe dahilinde [havuza veritabanı ekleme ve havuzdan veritabanı kaldırma](sql-database-elastic-pool-manage-portal.md) işlemlerini gerçekleştirebilir ve uygulamanızı birkaç veritabanından tutun binlerce veritabanına ölçeklendirebilirsiniz. Havuzdaki veritabanlarına sunulan kaynakların alt ve üst sınırlarını denetleyerek havuzdaki veritabanlarının havuzdaki tüm kaynakları kullanmasını önleyebilir ve havuzdaki tüm veritabanlarının kaynaklardan pay almasını garanti edebilirsiniz. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [SQL Veritabanı kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Betikler, elastik havuzların izlenmesine ve ölçeklendirilmesine yardımcı olabilir. Örnek için bkz. [PowerShell kullanarak Azure SQL Veritabanında bir SQL elastik havuzunu izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-pool-powershell.md)

> [!IMPORTANT]
> SQL Veritabanı Yönetilen Örneği, elastik havuzları desteklemez.

### <a name="blend-single-databases-with-pooled-databases"></a>Tek veritabanlarını havuza alınmış veritabanlarıyla karıştırma

İster tek veritabanlarını isterseniz elastik havuzları seçin, tercihlerinizi dilediğiniz zaman değiştirebilirsiniz. Tek veritabanlarını elastik havuzlarla bir araya getirebilir, tek veritabanlarının ve elastik havuzların hizmet katmanlarını hızla ve kolaylıkla kendi durumunuza uyarlamak üzere değiştirebilirsiniz. Azure'un benzersiz gücü ve erişim özellikleri sayesinde benzersiz uygulama tasarımı ihtiyaçlarınızı karşılamak, maliyet ve kaynak verimliliği sağlamak ve yeni iş fırsatlarını yakalamak amacıyla diğer Azure hizmetlerini SQL Veritabanı ile birleştirebilir ve eşleştirebilirsiniz.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Kapsamlı izleme ve uyarı özellikleri

Peki tek veritabanlarıyla ve elastik havuzların performanslarını nasıl karşılaştırabilirsiniz? Performansı yükseltmeye veya düşürmeye karar vereceğiniz doğru zamanı nasıl belirlersiniz? Performans değerlendirmeleriyle birlikte [yerleşik performans izleme](sql-database-performance.md) ve [uyarı](sql-database-insights-alerts-portal.md) araçlarını kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre ölçek büyütme veya küçültme işlemlerinin etkisini hızlı bir şekilde değerlendirebilirsiniz. Ayrıntılar için bkz. [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

SQL Veritabanı ayrıca izlemeyi kolaylaştırmak için [ölçümler ve tanılama günlükleri oluşturabilir](sql-database-metrics-diag-logging.md). SQL Veritabanını kaynak kullanımını, çalışanları, oturumları ve bu Azure kaynaklarından birine yapılan bağlantıları kaydedecek şekilde yapılandırabilirsiniz:

- **Azure Depolama**: Küçük maliyetlerle çok sayıda telemetri arşivleme için
- **Azure Olay Hub'ı**: SQL Veritabanı telemetrisini özel izleme çözümünüz veya yoğun işlem hatlarıyla tümleştirmek için
- **Azure Log Analytics**: Raporlama, uyarı ve azaltma özelliklerine sahip yerleşik izleme çözümü için. Azure Log Analytics, bir özelliği olan [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md)

    ![architecture](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Kullanılabilirlik özellikleri

Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi [(SLA)](http://azure.microsoft.com/support/legal/sla/), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. Azure platformu tüm Azure SQL Veritabanlarını tam kapsamlı olarak yönetir ve veri kaybı yaşanmayacağı ve yüksek oranda veri kullanılabilirliği garantisi verir. Azure düzeltme eki uygulama, yedekleme, çoğaltma, hata algılama, arka plandaki potansiyel donanım, yazılım veya ağ arızaları, hata giderme, yük devretme, veritabanı yükseltmesi ve diğer bakım görevlerini otomatik olarak gerçekleştirir. Standart kullanılabilirlik düzeyine ulaşmak için işlem ve depolama katmanları ayrılır. Premium kullanılabilirlik düzeyine ulaşmak için ise işlem ve depolama katmanları performans için tek bir düğüm üzerinde tümleştirilirken arka planda AlwaysOn Kullanılabilirlik Grupları uygulanır. Azure SQL veritabanı yüksek kullanılabilirlik özellikleri tam bir irdelemesi için bkz: [SQL veritabanı kullanılabilirlik](sql-database-high-availability.md). SQL Veritabanı ayrıca aşağıdakiler dahil olmak üzere yerleşik [iş sürekliliği ve global ölçeklenebilirlik](sql-database-business-continuity.md) özelliklerine sahiptir:

- **[Otomatik yedekleme](sql-database-automated-backups.md)**: SQL Veritabanı otomatik olarak tam, değişiklik ve işlem günlüğü kapsamlı yedeklemeler gerçekleştirir.
- **[Belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md)**: SQL Veritabanı otomatik yedek elde tutma süresi içindeki bir noktaya geri yüklemeyi destekler.
- **[Etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md)**: SQL Veritabanı aynı yerde veya global ölçekte dağıtılmış Azure veri merkezlerinde en fazla dört okunabilir ikincil veritabanı yapılandırmanızı sağlar.  Örneğin, yüksek hacimli eşzamanlı salt okunur işlemlere sahip bir katalog veritabanı kullanan bir SaaS uygulamanız varsa, etkin coğrafi çoğaltmayı kullanarak global okuma ölçeğini etkinleştirebilir ve birincil veritabanı üzerindeki okuma iş yükü kaynaklı performans sorunlarını ortadan kaldırabilirsiniz. 
- **[Yük devretme grupları](sql-database-geo-replication-overview.md)**: SQL Veritabanı saydam coğrafi çoğaltma ve çok sayıda veritabanı ve elastik havuz için yük devretme dahil olmak üzere global ölçekte yüksek kullanılabilirlik ve yük dengeleme gerçekleştirmenizi sağlar. Yük devretme grupları ve etkin coğrafi çoğaltma asgari düzeyde yönetimle global olarak dağıtılmış SaaS uygulamalarının oluşturulmasını sağlar ve tüm karmaşık izleme, yönlendirme ve yük devretme düzenlemesini SQL Veritabanı'na bırakır.
- **[Sıfır yedekli veritabanları](sql-database-high-availability.md)**: SQL Veritabanını kullanarak birden fazla kullanılabilirlik alanında Premium veya İş Açısından Kritik veritabanları ya da elastik havuzlar sağlayabilirsiniz. Bu veritabanları ve elastik havuzlar yüksek kullanılabilirlik için birden fazla yedekli çoğaltma içerdiğinden, bu çoğaltmaların birden fazla kullanılabilirlik alanına yerleştirilmesi, veri merkezi ölçek hatalarından veri kaybı olmadan otomatik olarak kurtarma becerisi de dahil olmak üzere daha yüksek esneklik sağlar.  

## <a name="built-in-intelligence"></a>Yerleşik zeka

SQL Veritabanı ile veritabanı çalıştırma ve yönetim maliyetlerini önemli ölçüde azaltmanıza yardımcı olan ve uygulamanızı hem performans hem de güvenlik açısından üst düzeye taşıyan yerleşik zekaya sahip olursunuz. Her saniye milyonlarca müşterinin iş yükünü çalıştıran SQL Veritabanı çok büyük miktarlarda telemetri verilerini toplayıp işlemenin yanı sıra arka planda müşteri gizliliğini de korumaktadır. Telemetri verilerini sürekli değerlendiren çok farklı algoritmalar sayesinde hizmet uygulamanızı öğrenip kendini ona göre uyarlayabilir. Hizmet bu analize dayalı olarak iş yükünüze özel performans geliştirme önerileri sunar. 

### <a name="automatic-performance-monitoring-and-tuning"></a>Otomatik performans izleme ve ayarlama

SQL Veritabanı izlemeniz gereken sorgularla ilgili ayrıntılı öngörüler sunar. SQL Veritabanı'nın veritabanı desenleriniz hakkında öğrendikleri sayesinde veritabanı şemanızı iş yükünüze uyarlayabilirsiniz. SQL Veritabanı, [performans ayarlama önerilerinde](sql-database-advisor.md) bulunur. Siz de bu ayarları gözden geçirebilir ve uygulayabilirsiniz. 

Ancak özellikle birden fazla veritabanıyla ilgilenirken bir veritabanını sürekli izlemek zor ve yorucu bir görevdir. [Akıllı Öngörüler](sql-database-intelligent-insights.md), SQL Veritabanı performansını otomatik olarak ölçekte izleyerek bu işi sizin için yapar ve performans düşüşü sorunlarını size bildirir, sorunun kök kaynağını belirler ve mümkün olduğunda performans iyileştirme önerileri sağlar.

Çok sayıda veritabanını yönetmek SQL Veritabanı ve Azure portalı tarafından sunulan araçlarla bile verimli şekilde yapılması imkanız bir görev haline gelebilir. Veritabanınızı el ile izlemek ve ayarlamak yerine izleme ve ayarlama işlerinin bazılarını SQL Veritabanı'na bırakarak [otomatik ayarlamayı](sql-database-automatic-tuning.md) kullanabilirsiniz. SQL Veritabanı önerileri otomatik olarak uygular, test eder ve yapılan tüm ayarları doğrulayarak performansın arttığından emin olur. SQL Veritabanı bu şekilde iş yükünüze kontrollü ve güvenli bir şekilde ayak uydurur. Otomatik ayarlama, veritabanınızın performansının dikkatli bir şekilde izlenmesi ve her ayar işleminden önce ve sonraki durumunun karşılaştırılarak performans artışı görülmediği durumlarda ayarların geri alındığı bir durumdur.

Bugün SQL Veritabanı üzerinde [çok kiracılı SaaS multi-tenant uygulamaları](sql-database-design-patterns-multi-tenancy-saas-applications.md) çalıştıran iş ortaklarımızın çoğu uygulamalarının kararlı ve tahmin edilebilir performansa sahip olduğundan emin olmak için otomatik performans ayarlarına güveniyor. İş ortaklarımız bu özelliğin gecenin ortasında performans sorunu yaşama riskini önemli ölçüde azalttığını söylüyor. Ayrıca müşterilerinin belirli bölümü SQL Server kullandığından SQL Server müşterilerine yardımcı olmak için SQL Veritabanı tarafından sunulan dizinleme önerilerinden faydalanıyorlar.

[SQL Veritabanında](sql-database-automatic-tuning.md) iki otomatik ayarlama yöntemi mevcuttur:

- **Otomatik dizin yönetimi**: Veritabanınıza eklenmesi ve veritabanınızdan kaldırılması gereken dizinleri tanımlar.
- **Otomatik plan düzeltme**: Sorunlu planları tanımlar ve SQL planı performans sorunlarını düzeltir (çok yakında, SQL Server 2017 ile kullanılabilir).

### <a name="adaptive-query-processing"></a>Uyarlamalı sorgu işleme

SQL Veritabanı'na çok durumlu tablo değerli işlevler için araya eklemeli yürütme, toplu iş modu bellek ataması geri bildirimi ve toplu iş modu uyarlamalı birleşimler dahil olmak üzere [uyarlamalı sorgu işleme](/sql/relational-databases/performance/adaptive-query-processing) özellik ailesini de ekliyoruz. Bu uyarlamalı sorgu işleme özelliklerinin her biri benzer "öğren ve uyarla" tekniklerini uygulayarak geçmişe dönük zorlu sorgu iyileştirme sorunlarıyla ilgili performans sorunlarında yardımcı olmaktadır.

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk

SQL Veritabanı, uygulamanızın çeşitli güvenlik ve uyumluluk gereksinimlerine uygun olmasına yardımcı olmak için bir dizi [yerleşik güvenlik ve uyum özelliğine](sql-database-security-overview.md) sahiptir. 

### <a name="advance-threat-protection"></a>Gelişmiş Tehdit Koruması

SQL Gelişmiş Tehdit Koruması, gelişmiş SQL güvenlik özellikleri için birleştirilmiş bir paket sunar. Hassas verileri keşfedip sınıflandırma, veritabanınızın güvenlik açıklarını yönetme ve veritabanınıza ilişkin bir tehdit belirtisi olabilecek anormal etkinlikleri algılamaya yönelik işlevsellik sağlar. Bu özellikler tek bir konumdan etkinleştirilebilir ve yönetilebilir.

- [Veri Bulma ve Sınıflandırma](sql-database-data-discovery-and-classification.md) (önizleme sürümündedir), veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve korumak için yerleşik Azure SQL Veritabanı özellikleri sunar. Veri sınıflandırma durumunuz için görünürlük sağlamanın yanı sıra veritabanı içindeki ve dışındaki hassas verilere erişimin izlenmesi için kullanılabilir.
- [Güvenlik Açığı Değerlendirmesi](sql-vulnerability-assessment.md) olası veritabanı güvenlik açıklarını keşfetmenizi ve izlemenizi sağlamanın yanı sıra bunları gidermeye yardımcı olan yapılandırması kolay bir hizmettir. Güvenlik durumunuz hakkında görünürlük sağlamasının yanı sıra güvenlik sorunlarınızı çözmek ve veritabanı güçlendirmelerinizi geliştirmek için eyleme dönüştürülebilir adımlar sunar.
- [Tehdit Algılama](sql-database-threat-detection.md), veritabanlarına erişme veya bunları kullanma konusunda olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder. Veritabanınızı şüpheli etkinliklere karşı sürekli izler ve olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişim modelleri hakkında anında güvenlik uyarıları sunar. Tehdit Algılama uyarıları, şüpheli etkinliğin ayrıntılarının yanı sıra tehdidi araştırmak ve ortadan kaldırmak için önerilen eylemleri içerir.

### <a name="auditing-for-compliance-and-security"></a>Uyumluluk ve güvenlik denetimi

[SQL Veritabanı Denetimi](sql-database-auditing.md) veritabanı olaylarını izler ve olayları Azure depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında öngörü sahip olmanıza yardımcı olabilir.

### <a name="data-encryption"></a>Veri şifrelemesi

SQL Veritabanı, hareket halindeki verileriniz için [Aktarım Katmanı Güvenliği](https://support.microsoft.com/kb/3135244), bekleyen veriler için [Saydam Veri Şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ve kullanılan veriler için [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) kullanarak verilerinizi şifreler ve güvenliğini sağlar.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

SQL Veritabanı, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md) ile veritabanı kullanıcısı ve diğer Microsoft hizmetleri kimliklerini bir merkezden yönetmenizi sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication.md) (MFA) ve çoklu oturum açma işlemini destekler.

### <a name="compliance-certification"></a>Uyumluluk sertifikası

SQL Veritabanı düzenli olarak denetimden geçmektedir ve birden fazla uyumluluk standardı sertifikasına sahiptir. Daha fazla bilgi için günceli [SQL Veritabanı uyumluluk sertifikası](https://azure.microsoft.com/support/trust-center/services/) listesine ulaşabileceğiniz [Microsoft Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/) sayfasına bakın.

## <a name="easy-to-use-tools"></a>Kullanımı kolay araçlar

SQL Veritabanı uygulama oluşturma ve uygulamaların bakımını yapma işlemlerinin daha kolay ve daha verimli şekilde yapılmasını sağlar. SQL Veritabanı size, en iyi yaptığınız işe; mükemmel uygulamalar oluşturmaya odaklanma seçeneği sunar. Sahip olduğunuz araçları ve becerileri kullanarak SQL Veritabanı ile yönetebilir ve geliştirebilirsiniz.

- **[Azure portalı](https://portal.azure.com/)**: Tüm Azure hizmetlerini yönetebileceğiniz web tabanlı uygulama 
- **[SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)**: SQL Server'dan SQL Veritabanı'na tüm SQL altyapılarını yönetebileceğiniz ücretsiz ve indirilebilir istemci uygulaması
- **[Visual Studio'daki SQL Server Veri Araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)**: SQL Server ilişkisel veritabanları, Azure SQL veritabanları, Integration Services paketleri, Analysis Services veri modelleri ve Reporting Services raporları geliştirebileceğiniz ücretsiz ve indirilebilir istemci uygulaması.
- **[Visual Studio Code](https://code.visualstudio.com/docs)**: Microsoft SQL Server, Azure SQL Veritabanı ve SQL Veri Ambarı'nı sorgulamak için [mssql uzantısı](https://aka.ms/mssql-marketplace) dahil olmak üzere uzantıları destekleyen Windows, macOS ve Linux üzerinde çalışabilen ücretsiz, indirilebilir ve açık kaynak kod düzenleyicisi.

SQL Veritabanı MacOS, Linux ve Windows üzerinde Python, Java, Node.js, PHP, Ruby ve .NET ile uygulama oluşturmayı destekler. SQL Veritabanı, SQL Server ile aynı [bağlantı kitaplıklarını destekler](sql-database-libraries.md).

## <a name="engage-with-the-sql-server-engineering-team"></a>SQL Server mühendislik ekibi ile iletişime geçme

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): Veritabanı yönetimi hakkında sorular için
- [Stack Overflow](http://stackoverflow.com/questions/tagged/sql-server): Geliştirme hakkında sorular için
- [MSDN Forumları](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): Teknik sorular için
- [Geri bildirim](http://aka.ms/sqlfeedback): Hata bildirimleri ve özellik istekleri için
- [Reddit](https://www.reddit.com/r/SQLServer/): SQL Server’ı tartışmak için

## <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanı ve elastik havuz maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).

- Başlamanıza yardımcı olacak şu hızlı başlangıçlara bakın:

  - [Azure portalda SQL veritabanı oluşturma](sql-database-get-started-portal.md)  
  - [Azure CLI ile SQL veritabanı oluşturma](sql-database-get-started-cli.md)
  - [PowerShell ile SQL veritabanı oluşturma](sql-database-get-started-powershell.md)

- Azure CLI ve PowerShell örnekleri için bkz.:
  - [SQL Veritabanı için Azure CLI örnekleri](sql-database-cli-samples.md)
  - [SQL Veritabanı için Azure PowerShell örnekleri](sql-database-powershell-samples.md)
