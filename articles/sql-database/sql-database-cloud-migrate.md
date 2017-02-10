---
title: "SQL Server veritabanını Azure SQL Veritabanına geçirme | Microsoft Docs"
description: "Şirket içi SQL Server veritabanını buluttaki Azure SQL Veritabanına nasıl geçireceğinizi öğrenin. Veritabanı geçişinden önce uyumluluğu test etmek için veritabanı taşıma araçlarını kullanın."
keywords: "veritabanı geçişi,sql server veritabanı geçişi,veritabanı taşıma araçları,veritabanı taşıma,sql veritabanı geçişi"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: 61fb027dfdd5830d87fe4fcfff57f685db71475e


---
# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL Server veritabanını buluttaki SQL Veritabanına taşıma
Bu makalede şirket içi SQL Server 2005 veya üzeri veritabanını Azure SQL Veritabanına nasıl geçireceğinizi öğreneceksiniz. Bu veritabanı geçiş işlemi sırasında SQL Server veritabanına ait şemanızı ve verilerinizi mevcut ortamınızdan SQL Veritabanına taşıyacaksınız. İşlemin başarılı olması için var olan veritabanının bir uyumluluk testinden geçirilmesi gerekir. SQL Veritabanı V12 ile sunucu düzeyi ve veritabanları arası işlemlerle ilgili sorunlara ek olarak [özellik eşliği](sql-database-features.md) açısından da yaklaşıyoruz. [Kısmen desteklenen veya desteklenmeyen işlevleri](sql-database-transact-sql-information.md) kullanan veritabanları ve uygulamalar için SQL Server veritabanının geçirilebilmesi için bu uyumsuzlukların giderilmesi amacıyla yeniden mühendislik işlemlerinin yapılması gerekir.

Geçiş için aşağıdaki adımları uygulamanız gerekir:

* **Uyumluluk Testi**: Veritabanının SQL Veritabanı ile uyumlu olup olmadığını doğrulayın. 
* **Varsa Uyumluluk Sorunlarını giderme**: Doğrulama başarısız olursa doğrulama hatalarını gidermeniz gerekir.  
* **Geçişi gerçekleştirme**: Veritabanınızı uyumlu hale getirdikten sonra geçişi gerçekleştirmek için bir veya daha fazla yöntemden faydalanabilirsiniz. 

SQL Server bu görevlerin her birini tamamlamak için birden fazla yöntem sunmaktadır. Bu makale her bir görevle kullanılabilen yöntemler için genel bir bakış sağlar. Aşağıdaki şemada adımlar ve yöntemler gösterilmektedir.

  ![VSSSDT geçiş şeması](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> Microsoft Access, Sybase, MySQL Oracle ve DB2 olmak üzere SQL Server harici veritabanlarını Azure SQL Veritabanına geçirmek için bkz. [SQL Server Geçiş Yardımcısı](http://blogs.msdn.com/b/ssma/).
> 
> 

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Veritabanı geçiş araçları SQL Server veritabanı ile SQL Veritabanı arasındaki uyumluluğu test eder
Veritabanı geçiş işlemine başlamadan önce SQL Veritabanı uyumluluk sorunlarıyla ilgili test gerçekleştirmek için aşağıdaki yöntemlerden birini kullanın:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [Visual Studio için SQL Server Veri Araçları ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT, SQL Veritabanı V12 uyumsuzluklarını algılamak için en güncel uyumluluk kurallarını kullanın. Algılanan uyumsuzluklarla ilgili sorunları doğrudan bu aracın içinden giderebilirsiniz. Bu yöntem, SQL Veritabanı V12 uyumluluk sorunlarını test etmek ve gidermek için önerilen yöntemdir. 
* [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage, uyumluluk sorunlarını test eden ve algılanan uyumluluk sorunlarını içeren bir rapor oluşturan komut satırı yardımcı programdır. Bu aracı kullanıyorsanız en güncel uyumluluk sorunlarını algılamak için en güncel sürümü kullandığınızdan emin olun. Algılanan uyumluluk sorunlarını gidermek için başka bir araç kullanmanız gerekir. Bunun için SSDT önerilir.  
* [SQL Server Management Studio'daki Veri Katmanını Dışarı Aktar uygulama sihirbazı](sql-database-cloud-migrate-determine-compatibility-ssms.md): Bu sihirbaz, hataları algılar ve ekranda bildirir. Hata algılanmadıysa işleme devam ederek SQL Veritabanı geçişini tamamlayabilirsiniz. Algılanan uyumluluk sorunlarını gidermek için başka bir araç kullanmanız gerekir. Bunun için SSDT önerilir.
* [SQL Azure Geçiş Sihirbazı ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW, Azure SQL Veritabanı V12 uyumsuzluklarını algılamak için Azure SQL Veritabanı V11 uyumluluk kurallarını kullanan bir codeplex aracıdır. Algılanan uyumsuzluklarla ilgili sorunların bazılarını doğrudan bu aracın içinden giderebilirsiniz. Bu araç, düzeltilmesi gerekmeyen uyumsuzluklar da algılayabilir. Bu araç, kullanıma sunulan ilk Azure SQL Veritabanı geçiş yardım aracıdır ve SQL Server topluluğu tarafından etkin olarak desteklenmektedir. Bu araç ayrıca geçişi kendi içinden de tamamlayabilir. 

## <a name="fix-database-migration-compatibility-issues"></a>Veritabanı geçişi uyumluluk sorunlarını giderme
SQL Server veritabanı geçiş işlemine başlamadan önce algılanan uyumluluk sorunlarını gidermeniz gerekir. Kaynak veritabanındaki SQL Server sürümüne ve geçirdiğiniz veritabanının karmaşıklığına bağlı olarak karşılaşabileceğiniz çok çeşitli uyumluluk sorunları vardır. Eski SQL Server sürümlerinde daha fazla uyumluluk sorunları algılanabilir. Aşağıdaki kaynakları kullanabilir ve ek olarak istediğiniz arama motorunu kullanarak hedefli bir İnternet araması yapabilirsiniz:

* [Azure SQL Veritabanında desteklenmeyen SQL Server veritabanı özellikleri](sql-database-transact-sql-information.md)
* [SQL Server 2016'da Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [SQL Server 2014'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [SQL Server 2012'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [SQL Server 2008 R2'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [SQL Server 2005'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

İnternet aramasına ve bu kaynaklara ek olarak [MSDN SQL Server topluluk forumlarını](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) veya [StackOverflow](http://stackoverflow.com/) sitesini de kullanabilirsiniz.

Algılanan sorunları gidermek için aşağıdaki veritabanı geçiş araçlarından birini kullanın:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [Visual Studio için SQL Server Veri Araçları ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) uygulamasını kullanın: SSDT uygulamasını kullanmak için veritabanı şemanızı Visual Studio için SQL Server Veri Araçları ("SSDT") uygulamasına aktarmanız ve projeyi SQL Veritabanı V12 dağıtımı için derlemeniz gerekir. Ardından SSDT uygulamasında algılanan tüm uyumluluk sorunlarını giderin. İşlemi tamamladığınızda değişiklikleri kaynak veritabanıyla (veya kaynak veritabanının bir kopyasıyla) eşitleyin. SSDT şu anda SQL Veritabanı V12 uyumluluk sorunlarını test etmek ve gidermek için önerilen yöntemdir. [SSDT uygulamasını kullanma adımlarına](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) göz atın.
* [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) uygulamasını kullanın: SSMS uygulamasını kullanmak için Transact-SQL komutlarını çalıştırarak başka bir araç tarafından algılanan hataları giderebilirsiniz. Bu yöntem, veritabanı şemasını doğrudan kaynak veritabanında değiştirmek isteyen ileri düzey kullanıcılara yöneliktir. 
* [SQL Azure Geçiş Sihirbazı ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md) uygulamasını kullanın: SAMW uygulamasını kullanmak için kaynak veritabanından bir Transact-SQL betiği oluşturmanız gerekir. Sihirbaz mümkün olduğunda betiği dönüştürerek şemayı SQL Veritabanı V12 ile uyumlu hale getirir. İşlem tamamlandığında SAMW, SQL Veritabanı V12 ile bağlantı kurarak betiği çalıştırabilir. Bu araç ayrıca uyumluluk sorunlarını belirlemek için izleme dosyalarını da analiz eder. Betik yalnızca şemayla oluşturulabilir veya BCP biçiminde veri içerebilir.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Uyumlu bir SQL Server veritabanını SQL Veritabanına geçirme
Microsoft, uyumlu bir SQL Server veritabanını geçirmek için farklı senaryolarda kullanılabilecek birden fazla geçiş yöntemi sunar. Kabul edilebilir kesinti süresi, SQL Server veritabanınızın boyutu ve karmaşıklığı ile Microsoft Azure bulutuna bağlantınıza göre kendinize uygun yöntemi belirleyebilirsiniz.  

> [!div class="op_single_selector"]
> * [SSMS Geçiş Sihirbazı](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [BACPAC Dosyasına Dışarı Aktarma](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [BACPAC Dosyasından İçeri Aktarma](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [İşlem Çoğaltması](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

Geçiş yönteminizi belirlemek için sormanız gereken ilk soru, geçiş sırasında veritabanınızın hizmet dışı kalmasının kabul edilip edilemeyeceğidir. Bir veritabanının etkin işlemler gerçekleşirken geçirilmesi, veritabanı tutarsızlıklarına ve veritabanında olası bozulmalara neden olabilir. Bir veritabanını yavaşça çevrimdışına geçirmek için kullanabileceğiniz istemci bağlantısını devre dışı bırakmaktan [veritabanı anlık görüntüsü](https://msdn.microsoft.com/library/ms175876.aspx) oluşturmaya kadar birçok yöntem vardır.

Geçişi en az kapalı kalma süresi ile gerçekleştirmek istiyorsanız, veritabanınızın işlem çoğaltması gereksinimlerini karşılaması şartıyla [SQL Server işlem çoğaltmasını](sql-database-cloud-migrate-compatible-using-transactional-replication.md) kullanabilirsiniz. Veritabanının kapalı kalması kabul edilebilir bir durumsa veya ileride geçiş yapmak üzere bir üretim veritabanının test geçişini gerçekleştiriyorsanız, aşağıdaki üç yöntemden birini kullanmayı düşünebilirsiniz:

* [SSMS Geçiş Sihirbazı](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): Küçük ve orta büyüklükteki veritabanları için uyumlu bir SQL Server 2005 veya üzeri veritabanını geçirmek, SQL Server Management Studio'da [Veritabanını Microsoft Azure Veritabanına Geçirme Sihirbazı](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)'nı çalıştırmak kadar kolaydır.
* [BACPAC Dosyasına Dışarı Aktarma](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) ve ardından [BACPAC Dosyasından İçeri Aktarma](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): Bağlantı sorunları yaşıyorsanız (bağlantı kesilmesi, düşük bant genişliği veya zaman aşımı sorunları) ve veritabanınız orta ila büyük ölçekteyse bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosyası kullanabilirsiniz. Bu yöntemle SQL Server şemasını ve verileri bir BACPAC dosyasına dışarı aktarırsınız. Ardından bu BACPAC dosyasını SQL Veritabanına SQL Server Management Studio'daki Veri Katmanını Dışarı Aktar Uygulama Sihirbazını veya [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komut satırı yardımcı programını kullanarak içeri aktarırsınız.
* BACPAC ve BCP dosyalarını birlikte kullanma: Karmaşık veritabanlarında daha yüksek performansa ulaşmak için daha üstün paralelleştirmeye ulaşmak üzere bir [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) dosyasını [BCP](https://msdn.microsoft.com/library/ms162802.aspx) dosyasıyla birlikte kullanın. Bu yöntemle şemayı ve verileri ayrı ayrı geçirebilirsiniz.
  
  * [Yalnızca şemayı bir BACPAC dosyasına dışarı aktarın](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
  * [Yalnızca şemayı BACPAC dosyasından](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) SQL Veritabanına aktarın.
  * [BCP](https://msdn.microsoft.com/library/ms162802.aspx) kullanarak verileri düz dosyalara ayıklayın ve ardından bu dosyaları Azure SQL Veritabanına [paralel şekilde yükleyin](https://technet.microsoft.com/library/dd425070.aspx).
    
     ![SQL Server veritabanı geçişi - SQL veritabanını buluta geçirin.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Sonraki adımlar
* [SSDT'nin en yeni sürümü](https://msdn.microsoft.com/library/mt204009.aspx)
* [SQL Server Management Studio'nun en yeni sürümü](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Ek kaynaklar
* [SQL Veritabanı özellikleri](sql-database-features.md)
  [Transact-SQL kısmen desteklenen veya desteklenmeyen işlevler](sql-database-transact-sql-information.md)
* [SQL Server Geçiş Yardımcısını kullanarak SQL Server harici veritabanlarını geçirme](http://blogs.msdn.com/b/ssma/)




<!--HONumber=Jan17_HO1-->


