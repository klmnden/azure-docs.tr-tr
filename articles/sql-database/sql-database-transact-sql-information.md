---
title: T-SQL farklılıkları geçiş Azure SQL veritabanı çözümleme | Microsoft Docs
description: Azure SQL Veritabanında tam olarak desteklenmeyen Transact-SQL deyimleri
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 4d3f27d48819a4bd997cbb62177f5aae4afc85eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60615392"
---
# <a name="resolving-transact-sql-differences-during-migration-to-sql-database"></a>SQL veritabanına geçiş sırasında Transact-SQL farklılıklarını çözümleme

Zaman [veritabanınızı geçirme](sql-database-single-database-migrate.md) Azure SQL Server için SQL Server'dan veritabanınızı SQL Server geçirilebilmesi bazı yeniden tasarımlar gerektiğini fark edebilirsiniz. Bu makale, hem bu yeniden tasarımlar gerçekleştirme ve yeniden tasarımlar neden gerekli olduğu temel nedeni anlamak yardımcı olan yönergeler sağlar. Uyumsuzluklarını algılamak için kullanmak [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Genel Bakış

Uygulamaları kullanan Transact-SQL özelliklerinin çoğu hem Microsoft SQL Server hem de Azure SQL veritabanı olarak tam olarak desteklenir. Örneğin, SQL Server ve SQL veritabanı veri türleri, işleçler, dize, aritmetik, mantıksal ve imleç işlevleri gibi temel SQL bileşenleri aynı şekilde çalışır. Vardır, ancak bazı T-SQL farklılıkları (veri tanımlama dili) DDL ve DML (veri işleme dili) öğeleri T-SQL deyimlerini ve kısmen desteklenen sorgular (Bu makalenin sonraki bölümlerinde ele).

Ayrıca, bazı özellikleri ve Azure SQL veritabanı özellikleri ana veritabanında ve işletim sistemi bağımlılıklardan yalıtmak için tasarlandığından, tüm desteklenmeyen söz dizimi vardır. Bu nedenle, çoğu sunucu düzeyi etkinlik, SQL veritabanı için uygun değildir. Sunucu düzeyi seçenekleri, işletim sistemi bileşenlerini yapılandırın veya dosya sistemi yapılandırmasını belirten T-SQL deyimlerini ve seçenekler kullanılamaz. Bu gibi özellikler gerekli olduğunda, uygun bir alternatif genellikle başka bir şekilde kullanılabilir SQL veritabanı veya başka bir Azure özelliği ya da hizmeti.

Örneğin, yüksek kullanılabilirlik Azure SQL veritabanı'na benzer teknolojisi kullanılarak derlendi [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Kullanılabilirlik gruplarıyla ilgili T-SQL deyimleri SQL veritabanı tarafından desteklenmez ve Always On kullanılabilirlik grupları için ilgili dinamik yönetimi görünümleri de desteklenmez.

Desteklenen ve SQL veritabanı tarafından desteklenmeyen özellikler listesi için bkz. [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md). Liste bu sayfadaki yönergeleri ve özellikler bu makalede tamamlar ve Transact-SQL deyimleriyle üzerinde odaklanır.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Kısmi farklılıkla Transact-SQL söz dizimi deyimleri

Çekirdek (veri tanımlama dili) DDL deyimleri kullanılabilir, ancak bazı DDL deyimleri için disk yerleştirme ilgili ve desteklenmeyen uzantılı özellikleri.

- OLUŞTUR ve ALTER DATABASE deyimleri üzerinde üç düzine seçeneğiniz vardır. İfadeler, dosya yerleşimi, FILESTREAM ve yalnızca SQL Server için geçerli hizmet Aracısı seçenekleri içerir. Geçiş, ancak veritabanı oluşturan T-SQL kodu geçiriyorsanız karşılaştırmanız gerekir önce veritabanları oluşturursanız bu Önemsiz olabilir [veritabanı oluşturma (Azure SQL veritabanı)](https://msdn.microsoft.com/library/dn268335.aspx) SQL Server söz dizimi ile [oluştur Veritabanı (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) kullandığınız tüm seçenekleri desteklenen emin olmak için. Azure SQL veritabanı için veritabanı oluşturma, hizmet hedefi ve yalnızca SQL veritabanı'na geçerli esnek ölçeklendirme seçenekleri de vardır.
- OLUŞTUR ve ALTER TABLE deyimleri FILESTREAM desteklenmediğinden, SQL veritabanı'nda kullanılamayan FileTable seçeneğiniz vardır.
- OLUŞTUR ve Değiştir oturum açma deyimleri desteklenir ancak SQL veritabanı seçenekleri sunmaz. Veritabanınızı daha taşınabilir yapmak için mümkün olduğunda oturumları yerine bağımsız veritabanı kullanıcılarını kullanarak SQL veritabanı teşvik eder. Daha fazla bilgi için [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) ve [denetleme ve veritabanına erişim izni verme](sql-database-manage-logins.md).

## <a name="transact-sql-syntax-not-supported-in-azure-sql-database"></a>Azure SQL veritabanında desteklenmeyen transact-SQL söz dizimi

Açıklanan desteklenmeyen özelliklerle ilgili Transact-SQL deyimleriyle yanı sıra [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md), aşağıdaki deyim ve deyim grupları da desteklenmez. Bu nedenle, bu T-SQL özellikleri ve ifadeleri ortadan kaldırmak için T-SQL veritabanınızı geçirilmesi aşağıdaki özelliklerden herhangi birini kullanırken, yeniden mühendislik.

- Harmanlanmış sistem nesneleri
- Bağlantıyla ilişkili: Uç nokta deyimleri. SQL Veritabanı Windows kimlik doğrulamasını desteklemez ancak ona benzer olan Azure Active Directory kimlik doğrulamasını destekler. Bazı kimlik doğrulaması türleri için SSMS'nin en son sürümü gerekir. Daha fazla bilgi için bkz. [Azure Active Directory Kimlik Doğrulamasını Kullanarak SQL Veritabanına veya SQL Veri Ambarına Bağlanma](sql-database-aad-authentication.md).
- Üç veya dört bölüm adı kullanan veritabanları arası sorgular. (Salt okunur veritabanları arası sorgular [elastik veritabanı sorgusu](sql-database-elastic-query-overview.md) kullanılarak desteklenir.)
- Veritabanları arası sahiplik zinciri, `TRUSTWORTHY` ayarı
- `EXECUTE AS LOGIN` Bunun yerine "EXECUTE AS USER" kullanın.
- Genişletilebilir anahtar yönetimi haricinde şifreleme desteklenir
- Olay: Olaylar, olay bildirimleri, sorgu bildirimleri
- Dosya yerleşimi: Veritabanı dosya yerleşimi, boyut ve Microsoft Azure tarafından otomatik olarak yönetilen veritabanı dosyalarıyla ilgili söz dizimi.
- Yüksek Kullanılabilirlik: Microsoft Azure hesabınız aracılığıyla yönetilen yüksek kullanılabilirlikle ilgili söz dizimi. Buna yedekleme, geri yükleme, Her Zaman Açık, veritabanı yansıtması, günlük aktarma ve kurtarma modları için söz dizimleri dahildir.
- Günlük Okuyucusu: SQL veritabanı'nda kullanılabilir olmayan günlük okuyucusu üzerine kullanır sözdizimi: Çoğaltma, değişiklik verilerini yakalama gönderin. SQL Veritabanı gönderme temelli çoğaltma gönderisinin abonesi olabilir.
- İşlevler: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Donanım: Sunucu donanımı ayarlar ilgili söz dizimleri: bellek, çalışan iş parçacığı, CPU benzeşimi gibi izleme bayrakları. Hizmet katmanları ve boyutları bunun yerine işlem.
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`ve dört kısımlı adlar
- .NET Framework: SQL Server ile CLR tümleştirmesi
- Anlamsal arama
- Sunucu kimlik bilgileri: Kullanım [veritabanı kapsamlı kimlik bilgilerini](https://msdn.microsoft.com/library/mt270260.aspx) yerine.
- Sunucu düzeyi öğeler: Sunucu rollerini `sys.login_token`. `GRANT`, `REVOKE` ve `DENY` sunucu düzeyi izinler kullanılamaz ancak bazıları veritabanı düzeyi izinlerle değiştirilmiştir. Sunucu düzeyi kullanışlı DMV'lerden bazıları, eşdeğer veritabanı düzeyi DMV'lerine sahiptir.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` seçenekleri ve `RECONFIGURE`. Bazı seçenekler [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) ile kullanılabilir.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server Aracısı: SQL Server Agent veya MSDB veritabanına bağımlı söz dizimleri: uyarılar, işleçler, merkezi yönetim sunucuları. Bunun yerine Azure PowerShell gibi betik uygulamaları kullanın.
- SQL Server denetimi: Bunun yerine SQL Veritabanı denetimini kullanın.
- SQL Server izleme
- İzleme bayrakları: Bazı izleme bayrağı öğeleri uyumluluk modlarına taşınmıştır.
- Transact-SQL hata ayıklama
- Tetikleyicileri: Sunucu kapsamlı veya oturum açma Tetikleyicileri
- `USE` deyimi: Farklı bir veritabanı için veritabanı bağlamı değiştirmek için yeni veritabanıyla yeni bir bağlantı yapmanız gerekir.

## <a name="full-transact-sql-reference"></a>Tam Transact-SQL başvurusu

Transact-SQL dil bilgisi, kullanım ve örnekleri hakkında daha fazla bilgi için bkz: [Transact-SQL Başvurusu (veritabanı altyapısı)](https://msdn.microsoft.com/library/bb510741.aspx) SQL Server Books Online.

### <a name="about-the-applies-to-tags"></a>"Uygulandığı öğe" etiketleri hakkında

Transact-SQL Başvurusu için mevcut SQL Server sürümleri 2008 ilgili makaleler içerir. Makale başlığının altındaki simge çubuğunda dört SQL Server Platformu listeleme ve uygulanabilirliği. Örneğin kullanılabilirlik grupları SQL Server 2012'de tanıtılmıştır.  [CREATE AVAILABILITY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) makale belirten deyim için geçerli olduğunu  **SQL Server (2012'den itibaren)**. Deyim SQL Server 2008, SQL Server 2008 R2, Azure SQL Veritabanı, Azure SQL Veri Ambarı veya Paralel Veri Ambarı için geçerli değildir.

Bazı durumlarda, bir makalenin Genel konu bir üründe kullanılıyor ancak ürünler arasında küçük farklar vardır. Farklar başlığının içinde uygun şekilde makaleyi sırasında belirtilir. Bazı durumlarda, bir makalenin Genel konu bir üründe kullanılıyor ancak ürünler arasında küçük farklar vardır. Farklar başlığının içinde uygun şekilde makaleyi sırasında belirtilir. Örneğin CREATE TRIGGER makale SQL veritabanı'nda kullanılabilir. Ancak **tüm sunucu** seçeneği sunucu düzeyi Tetikleyiciler için SQL veritabanı'nda sunucu düzeyinde tetikleyici kullanılamayacağını belirtir. Bunun yerine veritabanı düzeyinde Tetikleyicileri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Desteklenen ve SQL veritabanı tarafından desteklenmeyen özellikler listesi için bkz. [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md). Liste bu sayfadaki yönergeleri ve özellikler bu makalede tamamlar ve Transact-SQL deyimleriyle üzerinde odaklanır.
