---
title: "SQL Server veritabanını Azure SQL Veritabanına geçirme | Microsoft Docs"
description: "SQL Server veritabanını buluttaki Azure SQL Veritabanına nasıl geçireceğinizi öğrenin. Veritabanı geçişinden önce uyumluluğu test etmek için veritabanı taşıma araçlarını kullanın."
keywords: "veritabanı geçişi,sql server veritabanı geçişi,veritabanı taşıma araçları,veritabanı taşıma,sql veritabanı geçişi"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 02/08/2017
ms.author: carlrab
ms.openlocfilehash: 90c78007368c2679e1c5afdb9369869adde77f0d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server veritabanını buluttaki SQL Veritabanına taşıma
Bu makalede SQL Server 2005 veya sonraki bir veritabanını Azure SQL Veritabanına geçirmeye yönelik iki birincil yöntem hakkında bilgi verilmektedir. İlk yöntem basittir, ancak geçiş sırasında önemli olabilecek bazı kapalı kalma sürelerine neden olabilir. İkinci yöntem daha karmaşık olmasına karşın, geçiş sırasında kapalı kalma süresini önemli ölçüde ortadan kaldırır.

Kaynak veritabanı Azure SQL veritabanı kullanarak ile uyumlu olduğundan emin olmak gereken her iki durumda da [veri geçiş Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595). SQL Database V12 yaklaşan [özellik eşliği](sql-database-features.md) sunucu düzeyinde ve veritabanları arası işlemleriyle ilgili sorunlar dışında SQL Server ile. [Kısmen desteklenen veya desteklenmeyen işlevleri](sql-database-transact-sql-information.md) kullanan veritabanları ve uygulamalar için SQL Server veritabanının geçirilebilmesi için [bu uyumsuzlukların giderilmesi amacıyla yeniden mühendislik](sql-database-cloud-migrate.md#resolving-database-migration-compatibility-issues) işlemlerinin yapılması gerekir.

> [!NOTE]
> Microsoft Access, Sybase, MySQL Oracle ve DB2 olmak üzere SQL Server harici veritabanlarını Azure SQL Veritabanına geçirmek için bkz. [SQL Server Geçiş Yardımcısı](https://blogs.msdn.microsoft.com/datamigration/2016/12/22/released-sql-server-migration-assistant-ssma-v7-2/).
> 

## <a name="method-1-migration-with-downtime-during-the-migration"></a>Yöntem 1: Geçiş sırasında kapalı kalma süresi ile geçiş

 Veritabanının kapalı kalması kabul edilebilir bir durumsa veya ileride geçiş yapmak üzere bir üretim veritabanının test geçişini gerçekleştiriyorsanız bu yöntemi kullanın. Bir öğretici için bkz: [bir SQL Server veritabanını geçirme](sql-database-migrate-your-sql-server-database.md).

Aşağıdaki liste, bu yöntem kullanılarak gerçekleştirilen bir SQL Server veritabanı geçişinin genel iş akışını içermektedir.

  ![VSSSDT geçiş şeması](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. [Veri Geçişi Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) aracının son sürümünü kullanarak veritabanının uyumluluğunu değerlendirin.
2. Transact-SQL betikleri halinde tüm gerekli düzeltmeleri hazırlayın.
3. Geçirilmekte olan kaynak veritabanının işlemsel açıdan tutarlı bir kopyasını oluşturun ve kaynak veritabanında başka bir değişiklik yapılmadığından emin olun (bu tür değişiklikleri geçiş tamamlandıktan sonra el ile de uygulayabilirsiniz). Bir veritabanını yavaşça çevrimdışına geçirmek için kullanabileceğiniz istemci bağlantısını devre dışı bırakmaktan [veritabanı anlık görüntüsü](https://msdn.microsoft.com/library/ms175876.aspx) oluşturmaya kadar birçok yöntem vardır.
4. Düzeltmeleri veritabanı kopyasına uygulamak için Transact-SQL betiklerini dağıtın.
5. Veritabanı kopyasını yerel sürücüdeki bir BACPAC dosyasına [aktarın](sql-database-export.md).
6. Çok sayıda BACPAC içeri aktarma aracından herhangi birini kullanarak (en iyi performans için SQLPackage.exe önerilir), .BACPAC dosyasını yeni bir Azure SQL veritabanı olarak [içeri aktarın](sql-database-import.md).

### <a name="optimizing-data-transfer-performance-during-migration"></a>Geçiş sırasında veri aktarımı performansını en iyi duruma getirme 

Aşağıdaki liste, içeri aktarma işlemi sırasında en iyi performans için öneriler içerir.

* Aktarım performansını en üst düzeye çıkarmak için bütçenizin izin verdiği en yüksek hizmet düzeyini ve performans katmanını seçin. Geçiş tamamlandıktan sonra paradan tasarruf etmek için ölçeği azaltabilirsiniz. 
* .BACPAC dosyanız ile hedef veri merkezi arasındaki mesafeyi azaltın.
* Geçiş sırasında otomatik istatistikleri devre dışı bırakma
* Tabloları ve dizinleri bölümleme
* Dizini oluşturulmuş görünümleri bırakma ve tamamlandıktan sonra yeniden oluşturma
* Nadiren sorgulanan geçmiş verileri başka bir veritabanına kaldırın ve bu geçmiş verileri ayrı bir Azure SQL veritabanına geçirin. Daha sonra bu geçmiş verileri [esnek sorgular](sql-database-elastic-query-overview.md) kullanarak sorgulayabilirsiniz.

### <a name="optimize-performance-after-the-migration-completes"></a>Geçiş tamamlandıktan sonra performansı en iyi duruma getirme

Geçiş tamamlandıktan sonra tam tarama ile [istatistikleri güncelleştirin](https://msdn.microsoft.com/library/ms187348.aspx).

## <a name="method-2-use-transactional-replication"></a>Yöntem 2: İşlem Çoğaltma Kullanma

Geçiş gerçekleşirken SQL Server veritabanınızı üretimden kaldırmak kabul edilebilir bir durum değilse, geçiş çözümü olarak SQL Server işlem çoğaltmayı kullanabilirsiniz. Bu yöntemi kullanmak için, kaynak veritabanının [işlem çoğaltma gereksinimlerini](https://msdn.microsoft.com/library/mt589530.aspx) karşılaması ve Azure SQL Veritabanı ile uyumlu olması gerekir. AlwaysOn SQL çoğaltma hakkında daha fazla bilgi için bkz: [Always On kullanılabilirlik grupları (SQL Server) için çoğaltma yapılandırma](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server).

Bu çözümü kullanmak için, Azure SQL Veritabanınızı, geçirmek istediğiniz SQL Server örneğine abone olacak şekilde yapılandırabilirsiniz. Yeni işlemler gerçekleşmeye devam ederken, işlem çoğaltma dağıtıcısı, veritabanındaki eşitlenecek verileri eşitler (yayımcı). 

İşlem çoğaltma ile verilerinizde veya şemanıza yapılan tüm değişiklikler Azure SQL Veritabanınızda gösterilir. Eşitleme tamamlandıktan sonra geçişe hazır olduğunuzda, uygulamalarınızın bağlantı dizesini Azure SQL Veritabanınıza işaret edecek şekilde değiştirin. İşlem çoğaltma özelliği kaynak veritabanınızda kalan tüm değişiklikleri boşalttığında ve tüm uygulamalarınız Azure DB’yi işaret ettiğinde, işlem çoğaltma özelliğini kaldırabilirsiniz. Azure SQL Veritabanınız artık üretim sisteminizdir.

 ![SeedCloudTR diyagramı](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> İşlem çoğaltmayı, kaynak veritabanınızın bir alt kümesini geçirmek için de kullanabilirsiniz. Azure SQL Veritabanına çoğalttığınız yayın, çoğaltmakta olduğunuz veritabanındaki bir tablo alt kümesiyle sınırlanabilir. Çoğaltılmakta olan her tablo için verileri bir satır alt kümesi ve/veya sütun alt kümesi ile sınırlayabilirsiniz.
>

### <a name="migration-to-sql-database-using-transaction-replication-workflow"></a>İşlem Çoğaltma iş akışı kullanılarak SQL Veritabanına geçiş

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı özelliklere sahip olması için SQL Server Management Studio’nun en son sürümünü kullanın. SQL Server Management Studio’nun eski sürümleri, SQL Veritabanını abone olarak ayarlayamaz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).
> 

1. Dağıtımı Ayarlama
   -  [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   -  [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)
2. Yayın Oluşturma
   -  [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   -  [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Abonelik Oluşturma
   -  [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   -  [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

### <a name="some-tips-and-differences-for-migrating-to-sql-database"></a>SQL veritabanına geçiş için bazı ipuçları ve farklılıklar

1. Yerel dağıtıcı kullanma 
   - Bu durum sunucuda performans düşüşüne neden olur. 
   - Performans etkisi kabul edilemez boyuttaysa başka bir sunucu kullanabilirsiniz, ancak bunun yapılması yönetimi daha karmaşık hale getirir.
2. Bir anlık görüntü klasörü seçerken, seçtiğiniz klasörün çoğaltmak istediğiniz her tabloya ait BCP’yi saklayacak kadar büyük olduğundan emin olun. 
3. Anlık görüntü oluşturma işlemi tamamlanana kadar ilişkili tabloları kilitler, bu nedenle anlık görüntünüzü uygun şekilde zamanlayın. 
4. Azure SQL Veritabanında yalnızca iletme abonelikleri desteklenir. Aboneleri yalnızca kaynak veritabanından ekleyebilirsiniz.

## <a name="resolving-database-migration-compatibility-issues"></a>Veritabanı geçişi uyumluluk sorunlarını çözme
Kaynak veritabanındaki SQL Server sürümüne ve geçirdiğiniz veritabanının karmaşıklığına bağlı olarak karşılaşabileceğiniz çok çeşitli uyumluluk sorunları vardır. Eski SQL Server sürümlerinde daha fazla uyumluluk sorunları algılanabilir. Aşağıdaki kaynakları kullanabilir ve ek olarak istediğiniz arama motorunu kullanarak hedefli bir İnternet araması yapabilirsiniz:

* [Azure SQL Veritabanında desteklenmeyen SQL Server veritabanı özellikleri](sql-database-transact-sql-information.md)
* [SQL Server 2016'da Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [SQL Server 2014'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [SQL Server 2012'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [SQL Server 2008 R2'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [SQL Server 2005'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

İnternet aramasına ve bu kaynaklara ek olarak [MSDN SQL Server topluluk forumlarını](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) veya [StackOverflow](http://stackoverflow.com/) sitesini de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Geçiş sırasında tempdb kullanımını izlemek](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/) için Azure SQL EMEA Mühendisleri blogundaki betiği kullanın.
* [Geçiş devam ederken veritabanınızın işlem günlüğü alanını izlemek](https://blogs.msdn.microsoft.com/azuresqlemea/2016/10/31/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database/0) için Azure SQL EMEA Mühendisleri blogundaki betiği kullanın.
* BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
* Geçişten sonra UTC saati ile çalışma hakkında daha fazla bilgi için bkz. [Yerel saat diliminiz için varsayılan saat dilimini değiştirme](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/).
* Geçişten sonra veritabanının varsayılan dilini değiştirme hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanının varsayılan dilini değiştirme](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/).


