---
title: "SQL Database nedir? SQL Veritabanı&quot;na Giriş | Microsoft Belgeleri"
description: 'Get an introduction to SQL Database: technical details and capabilities of Microsoft''s relational database management system (RDBMS) in the cloud.'
keywords: "sql&quot;e giriş,sql veritabanı nedir"
services: sql-database
documentationcenter: 
author: shontnew
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/20/2016
ms.author: shkurhek;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 262583a2a11c41cf7acb55599041692137165e8f

---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>SQL Database nedir? SQL Database'e Giriş
SQL Veritabanı, piyasa lideri Microsoft SQL Server altyapısını temel alan ve görev açısından kritik iş yüklerini üstlenebilen, Microsoft bulutu tabanlı bir ilişkisel veritabanı hizmetidir. SQL Veritabanı, yönetime neredeyse hiç gerek olmaksızın birden fazla hizmet seviyesinde tahmin edilebilir performans, kesintisiz dinamik ölçeklenebilirlik, yerleşik iş sürekliliği ve veri koruma özellikleri sunar. Bu özellikler sayesinde değerli zamanınızı ve kaynaklarınızı sanal makine ve altyapı yönetimi yerine hızlı uygulama geliştirmeye ve piyasaya sunma sürenizi kısaltmaya ayırabilirsiniz. SQL Veritabanı, [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) altyapısını kullandığından var olan SQL Server araçlarını, kitaplıklarını ve API’lerini destekler. Sonuç olarak, yeni beceriler edinmenize gerek kalmadan yeni çözümler geliştirebilir, var olan SQL Server çözümlerinizi taşıyabilir ve Microsoft bulutunu kullanarak kapsamlarını genişletebilirsiniz.

Bu makale performans, ölçeklenebilirlik ve yönetilebilirlik ile ilgili temel SQL Database kavramlarına ve özelliklerine bir giriş niteliğindedir ve ayrıntıları inceleyebilmeniz için ilgili bağlantılarla desteklenmiştir. Uygulamalı öğreticilere hemen başlamaya hazırsanız [İlk SQL veritabanınızı oluşturabilir](sql-database-get-started.md) veya [Elastik havuz oluşturabilirsiniz](sql-database-elastic-pool-create-portal.md). Kısa bir tanıtım için bu videoyu izleyin.

> [!VİDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-create-DBs-in-seconds/player]
> 
> 

## <a name="adjust-performance-and-scale-without-downtime"></a>Kesinti olmadan performansı ayarlama ve ölçeklendirme
SQL Veritabanı hizmeti, üç hizmet katmanı sunmaktadır: Temel, Standart ve Premium. Her bir hizmet katmanı, hafiften ağıra tüm iş yüklerini desteklemek üzere [farklı performans ve işlev düzeyleri](sql-database-service-tiers.md) sunar. Ayda birkaç liraya küçük bir veritabanı üzerinde ilk uygulamanızı oluşturabilir, ardından istediğiniz zaman el ile veya programlama yoluyla [hizmet katmanını değiştirebilirsiniz](sql-database-scale-up.md). Bunu uygulamanız veya müşterileriniz kesinti yaşamadan gerçekleştirebilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

## <a name="elastic-pools-to-maximize-resource-utilization"></a>Kaynak kullanımını en verimli hale getirmek için elastik havuzlar
Tek veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. [Elastik havuzlar](sql-database-elastic-pool.md) bu sorunu çözmek için tasarlanmıştır. Esnek havuzların işleyiş mantığı gayet basittir. Performans kaynaklarını tek bir veritabanı yerine bir havuza ayırarak tek bir veritabanının performansı için değil havuzun ortak performans kaynakları için ödeme yaparsınız. Elastik havuzlar sayesinde kaynak talebindeki dalgalanmalara ayak uydurmak için veritabanı performansında sürekli ayarlama yapmanız gerekmez. Havuza alınmış veritabanları, gerektiğinde elastik havuzun performans kaynaklarını tüketir. Havuza alınan veritabanları havuzu kullanır ancak havuz sınırlarını aşmaz, böylece veritabanı kullanımınız tahmin edilebilir olmasa bile maliyetleriniz için durum tam tersidir. Ayrıca kontrolü sizin elinizde olan bir bütçe dahilinde [havuza veritabanı ekleme ve havuzdan veritabanı kaldırma](sql-database-elastic-pool-manage-portal.md) işlemlerini gerçekleştirebilir ve uygulamanızı birkaç veritabanından tutun binlerce veritabanına ölçeklendirebilirsiniz. Son olarak havuzdaki veritabanlarına sunulan kaynakların alt ve üst sınırlarını denetleyerek havuzdaki veritabanlarının havuzdaki tüm kaynakları kullanmasını önleyebilir ve havuzdaki tüm veritabanlarının kaynaklardan pay almasını garanti edebilirsiniz. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="blend-single-databases-with-pooled-databases"></a>Tek veritabanlarını havuza alınmış veritabanlarıyla karıştırma
İster tek veritabanlarını isterseniz elastik havuzları seçin, tercihlerinizi dilediğiniz zaman değiştirebilirsiniz. Tek veritabanlarını elastik havuzlarla bir araya getirebilir, tek veritabanlarının ve elastik havuzların hizmet katmanlarını hızla ve kolaylıkla kendi durumunuza uyarlamak üzere değiştirebilirsiniz. Ayrıca Azure'ın benzersiz gücü ve erişim özellikleri sayesinde benzersiz uygulama tasarımı ihtiyaçlarınızı karşılamak, maliyet ve kaynak verimliliği sağlamak ve yeni iş fırsatlarını yakalamak amacıyla diğer Azure hizmetlerini SQL Veritabanı ile birleştirebilir ve eşleştirebilirsiniz.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Peki tek veritabanlarıyla ve elastik havuzların performanslarını nasıl karşılaştırabilirsiniz? Performansı yükseltmeye veya düşürmeye karar vereceğiniz doğru zamanı nasıl belirlersiniz? [Yerleşik performans izleme](sql-database-performance.md) ve [uyarı](sql-database-insights-alerts-portal.md) araçlarına ek olarak [tek veritabanları için Veritabanı İşlem Birimlerini (DTU), elastik havuzlar için de elastik DTU’ları (eDTU’lar)](sql-database-what-is-a-dtu.md) kullanırsınız. Bu araçları kullanarak geçerli veya projeye özgü performans ihtiyaçlarınıza göre ölçek büyütme veya küçültme işlemlerinin etkisini hızlı bir şekilde değerlendirebilirsiniz. Ayrıntılı bilgi için bkz. [SQL Database seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama](sql-database-service-tiers.md).

## <a name="keep-your-app-and-business-running"></a>Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın
Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi [(SLA)](http://azure.microsoft.com/support/legal/sla/), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. SQL veritabanları sayesinde satın almanız, tasarlamanız, oluşturmanız ve yönetmeniz gerekmeksizin; yerleşik güvenlik, hataya dayanıklılık ve [veri koruması](sql-database-automated-backups.md) olanaklarından yararlanırsınız. SQL Database'deki her hizmet katmanı, uygulamanızı ve işinizi oluşturup çalışır halde kalmasını sağlamanız amacıyla kullanabileceğiniz kapsamlı bir iş sürekliliği özellikleri ve seçenekleri kümesi sunar. Bir veritabanını daha önceki bir durumuna (35 güne kadar) geri döndürmek üzere[ belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md) işlemini gerçekleştirebilirsiniz. Yedekleri güvenli bir kasada 10 yıla kadar saklamak için [uzun süreli yedek saklama](sql-database-long-term-retention.md) özelliğini yapılandırabilirsiniz. Ayrıca, veritabanlarınızı barındıran veri merkezinde bir kesinti oluşursa, son yedeklemelerin [coğrafi olarak yedekli kopyalarından]((sql-database-recovery-using-backups.md) veritabanlarını geri yükleyebilirsiniz. Gerekirse veri merkezinde kesinti oluşması halinde bir veya daha fazla bölgede hızlı yük devretme gerçekleştirmek için [coğrafi olarak yedekli okunabilir kopya](sql-database-geo-replication-overview.md) yapılandırabilirsiniz. Ayrıca bu kopyaları kullanarak farklı coğrafi bölgelerde daha hızlı okuma performansı elde edebilir veya [kesinti yaşamadan uygulama yükseltmelerini gerçekleştirebilirsiniz](sql-database-manage-application-rolling-upgrade.md). 

![SQL Database Coğrafi Çoğaltma](./media/sql-database-technical-overview/azure_sqldb_map.png)

Farklı hizmet katmanları için kullanılabilen çeşitli iş sürekliliği özellikleri hakkında ayrıntılı bilgi için bkz. [İş Sürekliliği](sql-database-business-continuity.md).

## <a name="secure-your-data"></a>Verilerinizin güvenliğini sağlama
SQL Server; erişimi sınırlayan, verileri koruyan ve etkinlikleri izlemenize yardımcı olan özelliklerle SQL Database tarafından desteklenen veri güvenliği anlayışına sahiptir. SQL Database'in sunduğu güvenlik seçeneklerinin kısa bir özeti için bkz. [SQL veritabanınızın güvenliğini sağlama](sql-database-security-overview.md). Güvenlik özelliklerinin daha kapsamlı bir görünümü için bkz. [SQL Server Database Altyapısı ve SQL Database için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589). Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/security/)'ni ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
SQL Database'e giriş niteliğindeki makaleyi okuduğunuza ve "SQL Database Nedir?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:

* Tek veritabanı ve elastik havuz maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).
* [Esnek havuzlar](sql-database-elastic-pool.md) hakkında bilgi edinin.
* [İlk veritabanınızı oluşturarak](sql-database-get-started.md) başlayın.
* C#, Java, Node.js, PHP, Python veya Ruby kullanarak ilk uygulamanızı oluşturma: [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)



<!--HONumber=Dec16_HO4-->


