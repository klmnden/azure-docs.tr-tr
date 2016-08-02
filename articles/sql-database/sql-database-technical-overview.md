<properties
    pageTitle="SQL Database nedir? SQL Database'e Giriş | Microsoft Azure"
    description="SQL Database ile tanışın: Microsoft'un bulut üzerindeki ilişkisel veritabanı yönetim sistemine (RDBMS) ilişkin teknik ayrıntılar ve özellikler."
    keywords="introduction to sql,intro to sql,what is sql database"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="05/23/2016"
   ms.author="shkurhek"/>

# SQL Database nedir? SQL Database'e Giriş

SQL Database, iş açısından önemli işlevleriyle piyasaya öncülük eden Microsoft SQL Server altyapısını temel alan, bulut üzerindeki bir ilişkisel veritabanı hizmetidir. SQL Database, yönetime neredeyse hiç gerek olmaksızın tahmin edilebilir performans, kesintisiz ölçeklenebilirlik, iş sürekliliği ve veri koruma olanağı sunar. Böylece sanal makineleri ve altyapıyı yönetmek yerine hızlı bir şekilde uygulama geliştirmeye ve pazara girişinizi hızlandırmaya odaklanabilirsiniz. SQL Database, [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) altyapısını kullandığı için var olan SQL Server araçlarını, kitaplıklarını ve API'lerini destekler. Böylece buluta taşıma ve genişletme işlemlerini gerçekleştirmeniz daha kolay hale gelir.

Bu makale performans, ölçeklenebilirlik ve yönetilebilirlik ile ilgili temel SQL Database kavramlarına ve özelliklerine bir giriş niteliğindedir ve ayrıntıları inceleyebilmeniz için ilgili bağlantılarla desteklenmiştir. Hemen başlamaya hazırsanız dakikalar içinde [İlk SQL veritabanınızı oluşturabilir](sql-database-get-started.md) veya [Esnek veritabanı havuzu oluşturabilirsiniz](sql-database-elastic-pool-create-portal.md). Daha ayrıntılı bilgi edinmek istiyorsanız 30 dakikalık bu videoyu izleyin.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## Kesinti olmadan performansı ayarlama ve ölçeklendirme

SQL veritabanları Temel, Standart ve Premium *hizmet katmanlarında* kullanılabilir. Her bir hizmet katmanı, hafiften ağıra tüm iş yüklerini desteklemek üzere [farklı performans ve işlev düzeyleri](sql-database-service-tiers.md) sunar. Uygulamanız veya müşterileriniz için kesinti olmadan ayda birkaç liraya küçük bir veritabanı üzerinde ilk uygulamanızı oluşturabilir, ardından uygulamanız dünya çapında bilinir hale geldiğinde istediğiniz zaman el ile veya programlama yoluyla [hizmet katmanını değiştirebilirsiniz](sql-database-scale-up.md).

Veritabanı oluşturabilmek ve tek veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir.

SQL Database'deki [esnek havuzlar](sql-database-elastic-pool.md) bu sorunu çözer. Esnek havuzların işleyiş mantığı gayet basittir. Bir havuzun performansını önceden belirleyin ve tek veritabanı performansı yerine havuzun toplu performansı için ödeme yapın. Veritabanı performansını yükseltmenize veya düşürmenize gerek yok. Havuz içindeki *esnek veritabanı* adı verilen veritabanları, talebi karşılamak üzere ölçeği otomatik olarak artırır veya azaltır. Esnek veritabanları havuzu kullanır ancak havuz sınırlarını aşmaz, böylece veritabanı kullanımınız tahmin edilebilir olmasa bile maliyetleriniz için durum tam tersidir. Ayrıca kontrolü sizin elinizde olan bir bütçe dahilinde [havuza veritabanı ekleme ve havuzdan veritabanı kaldırma](sql-database-elastic-pool-manage-portal.md) işlemlerini gerçekleştirebilir ve uygulamanızı birkaç veritabanından tutun binlerce veritabanına ölçeklendirebilirsiniz. Esnek havuzları kullanan SaaS uygulamalarının tasarım desenleri hakkında daha fazla bilgi edinmek için bkz. [Azure SQL Database kullanan Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Tek veya esnek... Hangisini seçerseniz seçin değişiklik yapmakta özgürsünüz. Yenilikçi tasarımlar oluşturmak için, tek veritabanlarını esnek veritabanı havuzlarıyla bir araya getirebilir; tek veritabanlarının ve havuzların hizmet katmanlarını değiştirebilirsiniz. Ayrıca Azure'ın benzersiz gücü ve erişim özellikleri sayesinde benzersiz ve modern uygulama tasarımı ihtiyaçlarınızı karşılamak, maliyet ve kaynak verimliliği sağlamak ve yeni iş fırsatlarını yakalamak amacıyla Azure hizmetlerini SQL Database ile birleştirebilir ve eşleştirebilirsiniz.

Peki veritabanlarının ve veritabanı havuzlarının performanslarını nasıl karşılaştırabilirsiniz? Performansı yükseltmeye veya düşürmeye karar vereceğiniz doğru zamanı nasıl belirlersiniz? Bu sorunun cevabı, tek veritabanları için Veritabanı İşlem Birimleri (DTU'lar); esnek veritabanları ve veritabanı havuzları için ise esnek Veritabanı İşlem Birimleridir (eDTU'lar). Ayrıntılı bilgi için bkz. [SQL Database seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama](sql-database-service-tiers.md).

## Uygulamanızın ve işinizin hiç kesintiye uğramamasını sağlayın

Azure'ın Microsoft yönetimindeki veri merkezlerinin küresel bir ağı tarafından desteklenen ve endüstri lideri niteliğinde %99,99'luk bir kullanılabilirlik oranı sunan hizmet düzeyi sözleşmesi [(SLA)](http://azure.microsoft.com/support/legal/sla/), uygulamanızın 7/24 kesintiye uğramamasına yardımcı olur. SQL veritabanları sayesinde tasarlamanız, satın almanız, oluşturmanız ve yönetmeniz gerekmeksizin yerleşik veri koruması ve hataya dayanıklılık olanaklarından yararlanırsınız. Yine de iş gereksinimlerinize bağlı olarak, uygulamanızın ve işinizin olağanüstü bir durumda, bir hata ile karşılaşıldığında veya başka bir durumda hızlıca kurtarılabileceğinden emin olmak için ek koruma katmanları talep edebilirsiniz. SQL Database'deki her hizmet katmanı, uygulamanızı ve işinizi oluşturup çalışır halde kalmasını sağlamanız amacıyla kullanabileceğiniz farklı özellik seçenekleri sunar. Bir veritabanını daha önceki bir durumuna (35 güne kadar) geri döndürmek üzere belirli bir noktaya geri yükleme işlemini gerçekleştirebilirsiniz. Ayrıca, veritabanlarınızı barındıran veri merkezinde bir kesinti oluşursa farklı bir bölgedeki veritabanı çoğaltmalarına yük devretme işlemi gerçekleştirebilirsiniz. Çoğaltmaları, yükseltme ya da farklı bölgelere yeniden konumlandırma işlemleri için de kullanabilirsiniz.

![SQL Database Coğrafi Çoğaltma](./media/sql-database-technical-overview/azure_sqldb_map.png)


Farklı hizmet katmanları için kullanılabilen çeşitli iş sürekliliği özellikleri hakkında ayrıntılı bilgi için bkz. [İş Sürekliliği](sql-database-business-continuity.md).

## Verilerinizin güvenliğini sağlama
SQL Server; erişimi sınırlayan, verileri koruyan ve etkinlikleri izlemenize yardımcı olan özelliklerle SQL Database tarafından desteklenen sağlam bir veri güvenliği anlayışına sahiptir. SQL Database'in sunduğu güvenlik seçeneklerinin kısa bir özeti için bkz. [SQL veritabanınızın güvenliğini sağlama](sql-database-security.md). Güvenlik özelliklerinin daha kapsamlı bir görünümü için bkz. [SQL Server Database Altyapısı ve SQL Database için Güvenlik Merkezi](https://msdn.microsoft.com/library/bb510589). Azure'ın platform güvenliği hakkında bilgi edinmek için [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/security/)'ni ziyaret edin.

## Sonraki adımlar
SQL Database'e giriş niteliğindeki makaleyi okuduğunuza ve "SQL Database Nedir?" sorusunu yanıtladığınıza göre şunları yapmaya hazırsınız:

- Tek veritabanı ve esnek veritabanı maliyet karşılaştırmaları ve hesaplayıcıları için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).
- [Esnek havuzlar](sql-database-elastic-pool.md) hakkında bilgi edinin.
- [İlk veritabanınızı oluşturarak](sql-database-get-started.md) başlayın.
- [SSMS ile bağlanma ve sorgu yürütme](sql-database-connect-query-ssms.md)
- C#, Java, Node.js, PHP, Python veya Ruby kullanarak ilk uygulamanızı oluşturma: [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)
- [Azure SQL Database hizmeti hakkındaki tüm konu başlıklarının](sql-database-index-all-articles.md) ve açıklamaların bulunduğu dizini inceleyin.



<!---HONumber=Jun16_HO2-->


