---
title: T-SQL farkları geçiş Azure SQL veritabanı çözme | Microsoft Docs
description: Azure SQL Veritabanında tam olarak desteklenmeyen Transact-SQL deyimleri
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: migrate
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 5a3196f1cdbebd131d6880bab6fc1468f4c1b849
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>SQL veritabanı geçiş sırasında Transact-SQL farkları çözme   
Zaman [veritabanınızı geçirme](sql-database-cloud-migrate.md) Azure SQL Server için SQL Server'dan veritabanınızı SQL Server geçirilebilecek önce bazı yeniden mühendislik gerektirdiğini fark edebilirsiniz. Bu makalede hem bu yeniden mühendislik gerçekleştirme ve yeniden mühendislik neden gerekli olduğu temel nedeni anlamak yardımcı olan yönergeler sağlar. Uyumsuzlukları algılamak için kullanmak [veri geçiş Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Genel Bakış
Uygulamaları kullanan çoğu Transact-SQL özelliklerini hem Microsoft SQL Server hem de Azure SQL veritabanı tam olarak desteklenir. Örneğin, veri türleri, işleçler, dize, aritmetik, mantıksal ve imleci işlevleri gibi çekirdek SQL bileşenleri aynı SQL Server ve SQL veritabanı çalışır. Ancak bazı T-SQL farklılıklar vardır DDL (veri tanımlama dili) ve T-SQL deyimlerini ve yalnızca kısmen desteklenen sorgu DML (veri işleme dili) öğeleri (, biz bu makalenin sonraki bölümlerinde ele alınmıştır).

Ayrıca, bazı özellikleri ve Azure SQL veritabanı ana veritabanı ve işletim sistemi bağımlılıkları özelliklerinden yalıtmak için tasarlandığından, hiç desteklenmeyen sözdizimi vardır. Bu nedenle, sunucu düzeyi etkinliklerin çoğu SQL veritabanı için uygun. Sunucu düzeyinde seçeneklerini, işletim sistemi bileşenleri yapılandırmak veya dosya sistemi yapılandırması belirtin T-SQL deyimlerini ve seçenekler kullanılamaz. Aşağıdakiler gibi özelliklerinden gerekli olduğunda, uygun bir alternatif genellikle diğer herhangi bir yolla kullanılabilir SQL veritabanı veya başka bir Azure özellik veya hizmet. 

Örneğin, yüksek kullanılabilirlik (aktif coğrafi çoğaltma daha hızlı kurtarma için bir olağanüstü durumda yapılandırmak istediğiniz ancak) her zaman açık yapılandırma gerekli değildir Azure oluşturulmuştur. Bu nedenle, kullanılabilirlik gruplarıyla ilgili T-SQL deyimleri SQL veritabanı tarafından desteklenmiyor ve için her zaman açık ilgili dinamik yönetim görünümlerini de desteklenmez.

Desteklenen ve SQL veritabanı tarafından desteklenmeyen özelliklerin listesi için bkz: [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md). Bu sayfada listesinde bu yönergeleri ve özellikleri makale tamamlayan ve Transact-SQL deyimlerini temel odaklanır.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Kısmi farklarla birlikte Transact-SQL söz dizimi deyimleri
Çekirdek (veri tanımlama dili) DDL deyimleri kullanılabilir, ancak bazı DDL deyimleri disk yerleştirme ilgili ve desteklenmeyen uzantılı özellikleri. 

- OLUŞTURMA ve ALTER DATABASE deyimleri üzerinde üç düzine seçeneğiniz vardır. İfadeler, dosya yerleştirme, FILESTREAM ve yalnızca SQL Server için geçerli hizmet Aracısı seçenekleri içerir. Geçiş, ancak veritabanları oluşturuyorsa T-SQL kodunu geçiriyorsanız karşılaştırmanız gerekir önce veritabanları oluşturursanız, bu önemli değil [CREATE DATABASE (Azure SQL veritabanı)](https://msdn.microsoft.com/library/dn268335.aspx) SQL Server sözdizimiyle [CREATE DATABASE (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) kullandığınız tüm seçenekleri desteklenir emin olmak için. Azure SQL veritabanı için veritabanı oluşturma, hizmet hedefi ve yalnızca SQL veritabanı için geçerli esnek ölçek seçenekleri de vardır.
- OLUŞTURMA ve ALTER TABLE deyimleri FILESTREAM desteklenmediği için SQL veritabanında kullanılamaz FileTable seçeneğiniz vardır.
- CREATE ve ALTER oturum açma ifadeleri desteklenir, ancak SQL veritabanı seçenekleri sağlamaz. Veritabanınızı daha taşınabilir yapmak için mümkün olduğunda oturumları yerine kapsanan veritabanı kullanıcıları kullanarak SQL veritabanı önerir. Daha fazla bilgi için bkz: [CREATE/ALTER oturum açma](https://msdn.microsoft.com/library/ms189828.aspx) ve [denetleme ve veritabanı erişimi verme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Azure SQL veritabanı'nda desteklenmeyen transact-SQL söz dizimi   
Transact-SQL deyimleri açıklanan desteklenmeyen özellikler ilgili ek olarak [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md), aşağıdaki deyimleri ve gruplara deyimleri desteklenmiyor. Bu nedenle, bu T-SQL özelliklerini ve deyimleri ortadan kaldırmak için T-SQL geçirilmesi için veritabanınızın aşağıdaki özelliklerden herhangi birini kullanıyorsanız, yeniden mühendislik.

- Harmanlanmış sistem nesneleri
- İlgili bağlantı: uç nokta deyimleri. SQL Veritabanı Windows kimlik doğrulamasını desteklemez ancak ona benzer olan Azure Active Directory kimlik doğrulamasını destekler. Bazı kimlik doğrulaması türleri için SSMS'nin en son sürümü gerekir. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Veritabanına veya SQL Veri Ambarına Bağlanma](sql-database-aad-authentication.md).
- Üç veya dört bölüm adı kullanan veritabanları arası sorgular. (Salt okunur veritabanları arası sorgular [elastik veritabanı sorgusu](sql-database-elastic-query-overview.md) kullanılarak desteklenir.)
- Veritabanları arası sahiplik zinciri, `TRUSTWORTHY` ayarı
- `EXECUTE AS LOGIN` Bunun yerine "EXECUTE AS USER" kullanın.
- Genişletilebilir anahtar yönetimi haricinde şifreleme desteklenir
- Olay: Olaylar, olay bildirimleri, sorgu bildirimleri
- Dosya yerleştirme: sözdizimi ilgili veritabanı dosyası yerleşimi, boyutu ve Microsoft Azure tarafından otomatik olarak yönetilir veritabanı dosyaları.
- Yüksek Kullanılabilirlik: sözdizimi ilgili Microsoft Azure hesabınız üzerinden yönetilen yüksek kullanılabilirlik için. Buna yedekleme, geri yükleme, Her Zaman Açık, veritabanı yansıtması, günlük aktarma ve kurtarma modları için söz dizimleri dahildir.
- Günlük Okuyucusu: SQL veritabanı üzerinde kullanılabilir değil günlük okuyucu üzerine dayanır sözdizimi: itme çoğaltma, değişiklik verilerini yakalama. SQL Veritabanı gönderme temelli çoğaltma gönderisinin abonesi olabilir.
- İşlevler: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Donanım: Sözdizimi donanımla ilgili sunucu ayarlarıyla ilgili: bellek, çalışan iş parçacığı, CPU benzeşimi gibi izleme bayrakları. Bunun yerine hizmet düzeylerini kullanın.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`ve dört bölümden oluşan adları
- .NET framework: SQL Server CLR tümleştirme
- Anlamsal arama
- Sunucu kimlik bilgileri: kullanım [veritabanı kapsamlı kimlik bilgileri](https://msdn.microsoft.com/library/mt270260.aspx) yerine.
- Sunucu düzeyinde öğeleri: sunucu rolleri `sys.login_token`. `GRANT`, `REVOKE` ve `DENY` sunucu düzeyi izinler kullanılamaz ancak bazıları veritabanı düzeyi izinlerle değiştirilmiştir. Sunucu düzeyi kullanışlı DMV'lerden bazıları, eşdeğer veritabanı düzeyi DMV'lerine sahiptir.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` seçenekleri ve `RECONFIGURE`. Bazı seçenekler [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) ile kullanılabilir.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server Agent: SQL Server Agent veya MSDB veritabanı güvenen söz dizimi: uyarıları, işleçler, merkezi yönetim sunucuları. Bunun yerine Azure PowerShell gibi betik uygulamaları kullanın.
- SQL Server audit: SQL veritabanını kullan yerine denetleme.
- SQL Server izleme
- İzleme bayrakları: bazı izleme bayrağı öğeleri için Uyumluluk modları taşınmıştır.
- Transact-SQL hata ayıklama
- Tetikleyiciler: Sunucu kapsamlı veya oturum açma tetikleyicileri
- `USE` deyimi: Veritabanı bağlamını farklı bir veritabanıyla değiştirmek için yeni veritabanıyla yeni bir bağlantı kurmanız gerekir.

## <a name="full-transact-sql-reference"></a>Tam Transact-SQL başvurusu
Transact-SQL dil bilgisi, kullanımı ve örnekleri için SQL Server Çevrimiçi Kitapları sayfasında [Transact-SQL Başvurusu (Veritabanı Altyapısı)](https://msdn.microsoft.com/library/bb510741.aspx) bölümüne bakın. 

### <a name="about-the-applies-to-tags"></a>"Uygulandığı öğe" etiketleri hakkında
Transact-SQL başvurusu geçerli için SQL Server sürümleri 2008 ilgili makaleler vardır. Var. makale başlığı bir simge çubuğu, dört SQL sunucu platformları listeleme ve uygulanabilirliğini belirten. Örneğin kullanılabilirlik grupları SQL Server 2012'de tanıtılmıştır. [AVAILABILTY grubu oluşturma](https://msdn.microsoft.com/library/ff878399.aspx) makale gösterir deyim için geçerli olduğunu **SQL Server'ın (2012'den başlayarak)**. Deyim SQL Server 2008, SQL Server 2008 R2, Azure SQL Veritabanı, Azure SQL Veri Ambarı veya Paralel Veri Ambarı için geçerli değildir.

Bazı durumlarda, bir makalenin Genel konu bir üründe kullanılabilir, ancak ürünleri arasında küçük farklılıklar vardır. Farkları Orta noktalar uygun şekilde makaledeki sırasında belirtilir. Bazı durumlarda, bir makalenin Genel konu bir üründe kullanılabilir, ancak ürünleri arasında küçük farklılıklar vardır. Farkları Orta noktalar uygun şekilde makaledeki sırasında belirtilir. Örneğin CREATE TRIGGER makaleyi SQL veritabanında kullanılabilir. Ancak **tüm sunucu** seçeneği için sunucu düzeyi tetikleyiciler, sunucu düzeyi Tetikleyiciler SQL veritabanında kullanılamaz olduğunu gösterir. Bunun yerine veritabanı düzeyi tetikleyicilerin kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen ve SQL veritabanı tarafından desteklenmeyen özelliklerin listesi için bkz: [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md). Bu sayfada listesinde bu yönergeleri ve özellikleri makale tamamlayan ve Transact-SQL deyimlerini temel odaklanır.

