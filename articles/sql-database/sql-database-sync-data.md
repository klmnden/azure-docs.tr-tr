---
title: Azure SQL Data Sync'i | Microsoft Docs
description: Azure SQL Data Sync bu genel bakış sunar
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: a887c79a51c7a239e7057171e51e67a53af2f84b
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58483566"
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>SQL Data Sync ile birden fazla Bulut ve şirket içi veritabanı arasında veri eşitleme

SQL Data Sync, birden çok SQL veritabanları ve SQL Server örnekleri arasında çift seçin verileri eşitleyin olanak sağlayan Azure SQL veritabanı üzerinde oluşturulmuş bir hizmettir.

> [!IMPORTANT]
> Azure SQL Data Sync mu **değil** şu anda Azure SQL veritabanı yönetilen örneği destekler.

## <a name="when-to-use-data-sync"></a>Veri Eşitleme kullanıldığı durumlar

Veri eşitleme veri burada birkaç Azure SQL veritabanı veya SQL Server veritabanlarını güncel tutulması gereken durumlarda yararlıdır. Veri eşitleme için ana kullanım örnekleri şunlardır:

- **Karma veri eşitleme:** Veri Eşitleme ile şirket içi veritabanlarınıza ve karma uygulamalar'ı etkinleştirmek için Azure SQL veritabanları arasında verileri tutabilirsiniz. Bu özellik, buluta geçiş düşünüyorsanız ve bazı uygulamalarını Azure'da koymak istediğiniz müşterileri itiraz.
- **Dağıtılmış uygulamalar:** Çoğu durumda, farklı iş yüklerini farklı veritabanlarında ayırmak yararlıdır. Örneğin, bir büyük üretim veritabanına sahip, ancak Ayrıca bu veriler üzerinde raporlama veya Analiz iş yükü çalıştırmak gerekir, bu ek iş yükü için ikinci bir veritabanı yararlıdır. Bu yaklaşım, üretim iş yükünüz üzerindeki performans etkisini en aza indirir. Veri eşitleme, bu iki veritabanı eşitlenmiş tutmak için kullanabilirsiniz.
- **Global olarak dağıtılmış uygulamalar:** Birçok işletme, çeşitli bölgeleri ve hatta bazı ülkelerde yayılır. Ağ gecikmesini en aza indirmek için size yakın bir bölgede verilerinizin sağlamak en iyisidir. Data Sync ile eşitlenmiş dünyanın dört bir yanındaki bölgelerdeki veritabanlarına kolayca tutabilirsiniz.

Veri eşitleme, aşağıdaki senaryolar için tercih edilen bir çözüm değil:

| Senaryo | Bazı önerilen çözümler |
|----------|----------------------------|
| Olağanüstü Durum Kurtarma | [Azure coğrafi olarak yedekli yedeklemeleri](sql-database-automated-backups.md) |
| Okuma ölçeği | [Yük Dengeleme (Önizleme) salt okunur sorgu iş yükleri için salt okunur çoğaltmalar kullanın](sql-database-read-scale-out.md) |
| ETL (OLTP OLAP için) | [Azure veri fabrikası](https://azure.microsoft.com/services/data-factory/) veya [SQL Server Integration Services](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) |
| Şirket içi SQL Server'dan Azure SQL veritabanına geçiş | [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/services/database-migration/) |
|||

## <a name="overview-of-sql-data-sync"></a>SQL Data Sync'i genel bakış

Veri eşitleme, bir eşitleme grubu kavramını temel alır. Bir eşitleme grubu, eşitlemek istediğiniz veritabanlarını grubudur.

Veri eşitleme, verileri eşitlemek için bir hub ve bağlı bileşen topolojisi kullanır. Veritabanlarından birini eşitleme grubunda Hub veritabanı tanımlarsınız. Veritabanlarını geri kalanını olan üye veritabanları. Yalnızca Hub ve tek tek üyeleri arasında eşitleme gerçekleşir.

- **Hub veritabanı** Azure SQL veritabanı olmalıdır.
- **Üye veritabanları** SQL veritabanları, şirket içi SQL Server veritabanları veya Azure sanal makineler'de SQL Server örnekleri olabilir.
- **Eşitleme veritabanı** Data Sync için meta verileri ve günlük içerir. Eşitleme veritabanı Hub veritabanı olarak Azure SQL veritabanı aynı bölgede olması gerekir. Eşitleme müşteri oluşturuldu ve müşterinin sahip olduğu bir veritabanıdır.

> [!NOTE]
> İçin yüklü bir şirket içi veritabanı bir üye veritabanı kullanıyorsanız, [yükleyip yerel bir eşitleme Aracısı yapılandırın](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Veritabanları arasında verileri eşitleme](media/sql-database-sync-data/sync-data-overview.png)

Bir eşitleme grubu, aşağıdaki özelliklere sahiptir:

- **Eşitleme şemasını** hangi verilerin eşitlenmesi açıklar.
- **Eşitleme yönü** yönlü olabilir veya tek bir yöne akabilir. Diğer bir deyişle, eşitleme yönü olabilir *üye hub'a*, veya *üye Hub'ına*, veya her ikisini de.
- **Eşitleme aralığı** ne sıklıkta açıklar eşitleme gerçekleşir.
- **Çakışma çözüm İlkesi** olabilir bir grubu düzeyi ilkesi *Hub WINS* veya *üye WINS*.

## <a name="how-does-data-sync-work"></a>Veri Eşitleme nasıl çalışır

- **Veri değişikliklerini izleme:** Veri eşitleme, INSERT, update ve delete Tetikleyicileri kullanarak değişiklikleri izler. Bir kullanıcı veritabanındaki bir yan tablodaki değişiklikler kaydedilir. TOPLU ekleme Tetikleyicileri varsayılan olarak başlatılmıyor olduğunu unutmayın. Fıre_trıggers belirtilmezse hiçbir INSERT tetikleyicisi yürütün. Böylece Data Sync'i Bu eklemeleri izleyebilirsiniz fıre_trıggers seçeneğini ekleyin. 
- **Veri eşitleme:** Veri Eşitleme bir Hub ve bağlı bileşen modeli tasarlanmıştır. Hub'ı ayrı ayrı her bir üyeyi eşitler. Üye değişiklikleri Hub'ından indirilir ve hub'a üye değişikliklerden sonra yüklenir.
- **Çakışmaları giderme:** Veri eşitleme çakışma çözümü için iki seçenek sağlar *Hub WINS* veya *üye WINS*.
  - Seçerseniz *Hub WINS*, değişikliklerin hub'ında her zaman üyesi değişiklikleri üzerine.
  - Seçerseniz *üye WINS*, üye değişikliklerin üzerine yaz hub'ında yapılan değişiklikler. Birden fazla üyesi ise, hangi üye eşitler son değeri bağlıdır.

## <a name="compare-data-sync-with-transactional-replication"></a>İşlem çoğaltma ile veri eşitleme karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | -Etkin-etkin desteği<br/>İki yönlü şirket içi ve Azure SQL veritabanı arasında | -Daha düşük gecikme süresi<br/>-İşlem tutarlılığı<br/>-Mevcut topolojisi geçişten sonra yeniden kullanma |
| Dezavantajları | -5 dakika veya daha fazla gecikme süresi<br/>-İşlem tutarlılığı<br/>-Daha yüksek performans etkisi | -Azure SQL veritabanı tek veritabanı veya havuza veritabanı yayımlanamıyor<br/>-Yüksek bakım maliyeti |
| | | |

## <a name="get-started-with-sql-data-sync"></a>SQL Data Sync'yi kullanmaya başlayın

### <a name="set-up-data-sync-in-the-azure-portal"></a>Azure portalında Data Sync'i Ayarla

- [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
- Veri Eşitleme Aracısı - [veri Aracısı Azure SQL Data Sync için eşitleme](sql-database-data-sync-agent.md)

### <a name="set-up-data-sync-with-powershell"></a>PowerShell ile Data Sync'i ayarlama

- [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
- [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

### <a name="review-the-best-practices-for-data-sync"></a>Data Sync için en iyi uygulamaları gözden geçirin

- [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)

### <a name="did-something-go-wrong"></a>Bir şey yanlış gitti

- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

## <a name="consistency-and-performance"></a>Tutarlılık ve performans

#### <a name="eventual-consistency"></a>Nihai tutarlılık

Veri Eşitleme tetikleyici tabanlı olduğundan, işlem tutarlılığı garanti edilmez. Microsoft, tüm değişiklikleri sonunda yapılan ve veri eşitleme veri kaybına neden olmayan garanti eder.

#### <a name="performance-impact"></a>Performans etkisi

Veri Eşitleme kullanır ekleyin, güncelleştirin ve değişiklikleri izlemek için Tetikleyiciler silin. Değişiklik izleme için kullanıcı veritabanında tablolar oluşturur. Bu değişiklik izleme etkinliklerin veritabanı iş yükünüz etkisi. Hizmet katmanınızı değerlendirmek ve gerekirse yükseltme.

Sağlama ve eşitleme grubu oluşturma, güncelleştirme ve silme işlemi sırasında sağlamayı kaldırma, veritabanı performansını da etkileyebilir. 

## <a name="sync-req-lim"></a> Gereksinimler ve sınırlamalar

### <a name="general-requirements"></a>Genel gereksinimler

- Her tabloda bir birincil anahtarı olmalıdır. Herhangi bir satırın birincil anahtarı değerini değiştirmeyin. Bir birincil anahtar değerini değiştirmeniz gerekiyorsa, satır silin ve yeni birincil anahtar değeriyle yeniden oluşturun. 
- Anlık görüntü yalıtımı etkinleştirilmiş olması gerekir. Daha fazla bilgi için bkz. [SQL Server'da anlık görüntü yalıtımı](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Genel sınırlamalar

- Bir tablonun birincil anahtarı olmayan bir kimlik sütunu olamaz.
- Bir birincil anahtar, aşağıdaki veri türleri bulunamaz: sql_variant, binary, varbinary, resim, xml. 
- Yalnızca desteklenen duyarlık olduğu için birincil anahtar olarak, aşağıdaki veri türleri kullanırken dikkatli olun: zaman, datetime, datetime2, datetimeoffset.
- Nesne (veritabanları, tablolar ve sütunlar) adlarını yazdırılabilir karakterleri nokta (.), köşeli ayraç ([) içeren veya sağa kare köşeli ayraç (]) olamaz.
- Azure Active Directory kimlik doğrulaması desteklenmiyor.
- Aynı ada ancak farklı şeması (örneğin, dbo.customers ve sales.customers) sahip tablolar desteklenmez.

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

- FILESTREAM
- SQL/CLR UDT
- XMLSchemaCollection (desteklenen XML)
- İmleç, RowVersion, zaman damgası, Hierarchyid

#### <a name="unsupported-column-types"></a>Desteklenmeyen sütun türü

Veri eşitleme, salt okunur veya sistem tarafından oluşturulan sütunları eşitleme yapılamıyor. Örneğin:

- Hesaplanan sütunlar.
- Zamana bağlı tablolar için sistem tarafından oluşturulan sütunları.

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyut sınırlamaları

| **Boyutları**                                                      | **Sınırı**              | **Geçici çözüm**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Eşitleme grubu sayısı için herhangi bir veritabanına ait olabilir.       | 5                      |                             |
| Bir tek eşitleme grubundaki uç noktaları sayısı              | 30                     |                             |
| Şirket içi Uç noktalara tek bir eşitleme grubunda en fazla sayısı. | 5                      | Birden çok eşitleme grubu oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | ad başına 50 karakter |                             |
| Bir eşitleme grubunda tabloları                                          | 500                    | Birden çok eşitleme grubu oluşturma |
| Bir eşitleme grubunda bir tablodaki sütunları                              | 1000                   |                             |
| Bir tablodaki verileri satır boyutu                                        | 24 mb                  |                             |
| En düşük eşitleme aralığı                                           | 5 Dakika              |                             |
|||
> [!NOTE]
> Olabilir en fazla 30 uç noktaları bir tek eşitleme grubunda yoksa yalnızca bir eşitleme grubu. Birden fazla eşitleme grubu varsa, uç noktalar genelinde tüm eşitleme gruplarını toplam sayısı 30 aşamaz. Bir veritabanının birden çok eşitleme gruplarına ait birden fazla uç noktası, bir geçersiz sayılır.

## <a name="faq-about-sql-data-sync"></a>SQL Data Sync hakkında SSS

### <a name="how-much-does-the-sql-data-sync-service-cost"></a>SQL Data Sync hizmet nin ücreti ne kadardır

SQL Data Sync hizmet için ücret alınmaz.  Ancak, yine de veri aktarım ücretleri veri taşıma için SQL veritabanı örneğiniz içine ve dışına tahakkuk eder. Daha fazla bilgi için bkz. [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Hangi bölgeler Data Sync desteği

SQL Data Sync tüm bölgelerde kullanılabilir.

### <a name="is-a-sql-database-account-required"></a>Gerekli bir SQL veritabanı hesabı

Evet. Hub veritabanı barındırmak için bir SQL veritabanı hesabınızın olması gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Yalnızca SQL Server şirket içi veritabanı arasında eşitleme Data Sync kullanabilir miyim

Doğrudan yönetilemez. SQL Server şirket içi veritabanı arasında dolaylı olarak, ancak Azure Hub veritabanı oluşturarak ve sonra şirket içi veritabanları eşitleme grubuna ekleyerek eşitleyebilirsiniz.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-subscriptions"></a>Farklı aboneliklere ait bir SQL veritabanı arasında eşitlenecek veri eşitleme kullanabilir miyim

Evet. Kaynak grupları tarafından farklı aboneliklere ait ait bir SQL veritabanı arasında eşitleyebilirsiniz.

- Aboneliklerin aynı kiracıda ve tüm abonelikleri iznine sahip, eşitleme grubunun Azure portalında yapılandırabilirsiniz.
- Aksi takdirde, farklı aboneliklere ait eşitleme üyesi ekleme için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-databases-that-belong-to-different-clouds-like-azure-public-cloud-and-azure-china"></a>Farklı bulutlara (örneğin, Azure genel bulutunda hem Azure Çin) ait bir SQL veritabanı arasında eşitlenecek veri eşitleme kullanabilir miyim

Evet. Farklı bulutlara ait bir SQL veritabanı arasında eşitleme, farklı aboneliklere ait eşitleme üyesi ekleme için PowerShell kullanmanız gerekir.

### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-sync-them"></a>Veri eşitleme için çekirdek veri üretim Veritabanım için boş bir veritabanı kullanmak ve miyim bunları eşitleyebilirsiniz

Evet. Şema el ile özgün komut dosyası tarafından yeni veritabanı oluşturun. Şema oluşturduktan sonra tabloları, verileri kopyalayın ve eşitlenmiş kalmasını sağlamak için bir eşitleme grubuna ekleyin.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Yedekleme ve geri yükleme my veritabanları için SQL Data Sync kullanmalıyım

SQL Data Sync verilerinizi bir yedekleme oluşturmak üzere kullanmak için önerilmez. Yedekleme ve SQL Data Sync eşitlemeler tutulan olmadığı için belirli bir noktaya geri yükleme yapamazsınız. Ayrıca, SQL Data Sync saklı yordamlar gibi diğer SQL nesneleri yedeklemez ve geri yükleme işlemi denk hızlı bir şekilde yapın.

Bir yedekleme tekniktir önerilen için bkz: [bir Azure SQL veritabanını kopyalamayı](sql-database-copy.md).

### <a name="can-data-sync-sync-encrypted-tables-and-columns"></a>Veri eşitleme, şifrelenmiş tablolar ve sütunlar eşitleme yapabilirim

- Bir veritabanı, Always Encrypted kullanıyorsa, yalnızca tablolar ve olan sütunlar eşitleyebilirsiniz *değil* şifrelenmiş. Veri Eşitleme verilerin şifresini çözemez için şifrelenmiş sütunları eşitleyemiyor.
- Bir sütun sütun düzeyinde şifrelemeyi (CLE) kullanıyorsa, satır boyutu 24 MB maksimum boyuttan daha az olduğu sürece sütun eşitleyebilirsiniz. Veri eşitleme, normal bir ikili veri olarak (CLE) anahtar ile şifrelenmiş sütun değerlendirir. Diğer eşitleme üyeler verilerin şifresini çözmek için aynı sertifika olması gerekir.

### <a name="is-collation-supported-in-sql-data-sync"></a>SQL Data Sync harmanlaması destekleniyor

Evet. SQL Data Sync, aşağıdaki senaryolarda harmanlamasını destekler:

- Seçilen eşitleme şeması tablolar zaten hub veya üye veritabanlarınızın ardından eşitleme grubuna dağıttığınızda olmayan, hizmet otomatik olarak ilgili tabloları ve sütunları boş hedef veritabanlarında seçili harmanlama ayarları ile oluşturur.
- Zaten eşitlenmiş tabloları hub ve üye veritabanlarında varsa, SQL Data Sync birincil anahtar sütunlarını eşitleme grubu başarıyla dağıtmak için hub ve üye veritabanları arasında aynı harmanlamaya sahip olmasını gerektirir. Sütunlarda birincil anahtar sütunları dışındaki harmanlama kısıtlama yoktur.

### <a name="is-federation-supported-in-sql-data-sync"></a>Federasyon SQL Data Sync destekleniyor mu

Federasyon kök veritabanı, SQL veri eşitleme hizmetinde hiçbir sınırlama olmadan kullanılabilir. SQL Data Sync için geçerli sürümü, veritabanı Federasyon uç noktası ekleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="update-the-schema-of-a-synced-database"></a>Eşitlenen bir veritabanı şemasını güncelleştirme

Bir eşitleme grubunda bir veritabanının şemasını güncelleştirmek zorunda mıyım? Şema değişiklikleri otomatik olarak çoğaltılmaz. Bazı çözümler için aşağıdaki makalelere bakın:

- [Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme](sql-database-update-sync-schema.md)
- [Mevcut bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)

### <a name="monitor-and-troubleshoot"></a>İzleme ve sorun giderme

SQL Data Sync beklendiği gibi çalışıyor mu? Etkinlik izleme ve sorunlarını gidermek için aşağıdaki makalelere bakın:

- [Azure İzleyici günlüklerine ile Azure SQL Data Sync izleme](sql-database-sync-monitor-oms.md)
- [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

### <a name="learn-more-about-azure-sql-database"></a>SQL Veritabanı hakkında daha fazla bilgi edinin.

SQL veritabanı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
