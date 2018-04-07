---
title: Azure SQL veri eşitleme (Önizleme) | Microsoft Docs
description: Azure SQL veri eşitleme (Önizleme) bu genel bakış sunar
services: sql-database
author: douglaslms
manager: craigg
ms.service: sql-database
ms.custom: data-sync
ms.topic: article
ms.date: 04/01/2018
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: e66adb8b0485e30fded487e18af6b2030f9c7f5b
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync-preview"></a>SQL veri eşitleme (Önizleme) ile birden çok Bulut ve şirket içi veritabanları arasında eşitleme verileri

SQL veri eşitleme veri birden çok SQL veritabanları ve SQL Server örnekleri arasında çift yönlü Seç eşitlemenize olanak sağlayan Azure SQL veritabanı üzerine kurulu bir hizmettir.

Veri Eşitleme eşitleme grubu kavramı temel alır. Eşitleme grubunu eşitlemek istediğiniz veritabanlarını grubudur.

Eşitleme grubu aşağıdaki özelliklere sahiptir:

-   **Eşitleme şema** hangi verilerin eşitleneceğini açıklar.

-   **Eşitleme yönü** çift yönlü olabilir veya yalnızca bir yöne akabilir. Diğer bir deyişle, eşitleme yönü olabilir *üye hub'a* veya *Hub'ına üye*, veya her ikisini de.

-   **Eşitleme aralığı** ne sıklıkla eşitleme gerçekleşir.

-   **Çakışma çözüm İlkesi** olabilir bir grubu düzeyi ilkesi *Hub WINS* veya *üye WINS*.

Veri Eşitleme bir hub ve bağlı bileşen topolojisi verileri eşitlemek için kullanır. Veritabanlarından birini grubunda Hub veritabanı olarak tanımlarsınız. Veritabanlarını geri kalanı üye veritabanları şunlardır. Eşitleme yalnızca Hub ve tek tek üyeleri arasında oluşur.
-   **Hub veritabanı** bir Azure SQL veritabanı olmalıdır.
-   **Üye veritabanları** SQL veritabanları, şirket içi SQL Server veritabanları veya Azure virtual machines'de SQL Server örnekleri olabilir.
-   **Eşitleme veritabanı** veri eşitleme için meta veri ve günlük içerir. Eşitleme veritabanı Hub veritabanı ile aynı bölgede bir Azure SQL veritabanı bulunan olması gerekir. Eşitleme müşteri oluşturuldu ve ait müşteri veritabanıdır.

> [!NOTE]
> Üzerinde bir şirket içi veritabanı kullanıyorsanız, zorunda [yerel aracı yapılandırma](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Veritabanları arasında eşitleme verileri](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Veri Eşitleme kullanma zamanı

Veri eşitleme veri birkaç Azure SQL veritabanlarını veya SQL Server veritabanları arasında güncel tutulması gereken durumlarda yararlıdır. Veri eşitlemesi için ana kullanım örnekleri şunlardır:

-   **Karma veri eşitleme:** veri eşitleme ile şirket içi veritabanları ve karma uygulamaların etkinleştirmek için Azure SQL veritabanları arasında eşitlenen verileri tutabilirsiniz. Bu özellik, buluta taşıma dikkate ve kendi uygulama bazıları Azure'da yerleştirmek istediğiniz müşterileri ilgisini.

-   **Dağıtılmış uygulamalar:** çoğu durumda, farklı iş yükleri farklı veritabanlarındaki ayırmak faydalı olacaktır. Örneğin, bir üretim veritabanınız var, ancak raporlama ya da analytics iş yükü bu veriler üzerinde çalışması gerekir, bu ek iş yükü için ikinci bir veritabanı yararlıdır. Bu yaklaşım, üretim iş yükü üzerindeki performans etkisini en aza indirir. Bu iki veritabanı eşitlenmiş tutmak için veri eşitleme kullanabilirsiniz.

-   **Genel olarak dağıtılmış uygulamalar:** birçok işletme birkaç bölgeler ve hatta bazı ülkelerde span. Ağ gecikmesini en aza indirmek için yakın bir bölgede verileriniz en iyisidir. Veri Eşitleme ile eşitlenen tüm dünyada bölgelerdeki veritabanlarını kolayca tutabilirsiniz.

Veri Eşitleme aşağıdaki senaryolar için uygun değil:

-   Olağanüstü Durum Kurtarma

-   Ölçek okuma

-   ETL (OLTP to OLAP)

-   Şirket içi SQL Server'dan Azure SQL veritabanı geçiş

## <a name="how-does-data-sync-work"></a>Veri Eşitleme nasıl çalışır? 

-   **Veri değişiklikleri izleme:** veri eşitleme Ekle kullanarak değişiklikleri izler, güncelleştirme ve Tetikleyicileri silin. Değişiklikler, kullanıcı veritabanında tarafındaki tabloya kaydedilir.

-   **Veri eşitleme:** veri eşitleme bir Hub ve bağlı bileşen modeli tasarlanmıştır. Hub tek tek her üye ile eşitlenir. Hub değişikliklerden üyesine karşıdan yüklenir ve Hub'ına üye değişikliklerden sonra yüklenir.

-   **Çakışmaları giderme:** veri eşitleme çakışması çözümleme için iki seçenek sunar *Hub WINS* veya *üye WINS*.
    -   Seçerseniz *Hub WINS*, hub değişiklikleri her zaman üyesinde değişikliklerin üzerine.
    -   Seçerseniz *üye WINS*, üye üzerine yaz değişiklikleri hub'ında yapılan değişiklikler. Birden fazla üye ise, hangi üye eşitlenir son değeri bağlıdır.

## <a name="sync-req-lim"></a> Gereksinimler ve sınırlamalar

### <a name="general-considerations"></a>Genel konular

#### <a name="eventual-consistency"></a>Nihai tutarlılık
Veri Eşitleme tetikleyici tabanlı olduğundan, işlem tutarlılığı garanti edilmez. Microsoft, tüm değişiklikler sonunda yapılır ve veri eşitleme veri kaybına neden olmaz garanti eder.

#### <a name="performance-impact"></a>Performans etkisi
Veri Eşitleme kullanır Ekle, Güncelleştir ve değişiklikleri izlemek için Tetikleyiciler silin. Kullanıcı veritabanında değişiklik izleme yan tablolar oluşturur. Bu değişiklik izleme etkinlikleri veritabanının yükünüzü etkiler. Hizmet katmanı değerlendirmek ve gerekirse yükseltin.

### <a name="general-requirements"></a>Genel gereksinimler

-   Her tablonun birincil anahtarı olmalıdır. Herhangi bir satırın birincil anahtarı değerini değiştirmeyin. Bir birincil anahtar değeri değiştirmeniz gerekiyorsa, satır silin ve yeni birincil anahtar değeri ile oluşturun. 

-   Anlık görüntü yalıtımı etkinleştirilmesi gerekir. Daha fazla bilgi için bkz: [anlık görüntü yalıtımı SQL Server'daki](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-limitations"></a>Genel sınırlamalar

-   Bir tablonun birincil anahtarı olmayan bir kimlik sütunu olamaz.

-   Bir birincil anahtar tarih saat veri türüne sahip olamaz.

-   Nesne (veritabanları, tablolar ve sütunlar) adlarını yazdırılabilir karakterleri nokta (.), köşeli ayraç ([) içeren veya sağa kare köşeli ayraç (]) olamaz.

-   Azure Active Directory kimlik doğrulaması desteklenmiyor.

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

-   FileStream

-   SQL/CLR UDT

-   XMLSchemaCollection (desteklenen XML)

-   İmleç, zaman damgası, HierarchyId

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyutları sınırlamalar

| **Boyutlar**                                                      | **Limit**              | **Geçici çözüm**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Eşitleme grubu sayısı için herhangi bir veritabanı ait olabilir.       | 5                      |                             |
| Bir tek eşitleme grubundaki uç noktaları sayısı              | 30                     | Birden çok eşitleme grupları oluşturma |
| Bir tek eşitleme grubundaki şirket içi uç noktaları sayısı üst sınırı. | 5                      | Birden çok eşitleme grupları oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | ad başına 50 karakter |                             |
| Bir eşitleme grubundaki tablolar                                          | 500                    | Birden çok eşitleme grupları oluşturma |
| Bir eşitleme grubundaki bir tablodaki sütunlar                              | 1000                   |                             |
| Bir tabloda veri satır boyutu                                        | 24 Mb                  |                             |
| Minimum eşitleme aralığı                                           | 5 Dakika              |                             |
|||

## <a name="faq-about-sql-data-sync"></a>SQL veri eşitleme hakkında SSS

### <a name="how-much-does-the-sql-data-sync-preview-service-cost"></a>Nasıl SQL veri eşitleme (Önizleme) hizmeti maliyeti nedir?

Önizleme sırasında SQL veri eşitleme (Önizleme) hizmeti için ücret ödemeden yoktur.  Ancak, yine veri aktarımı ücretlerine veri taşıma için SQL veritabanı örneğinde ve bu moddan tahakkuk eder. Daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

### <a name="what-regions-support-data-sync"></a>Hangi bölgeleri veri eşitleme destekliyor?

SQL veri eşitleme (Önizleme) tüm genel bulut bölgelerde kullanılabilir.

### <a name="is-a-sql-database-account-required"></a>Gerekli bir SQL veritabanı hesabı mı? 

Evet. Hub veritabanını barındırmak için bir SQL veritabanı hesabınızın olması gerekir.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Yalnızca SQL Server içi veritabanları arasında eşitlemek için veri eşitleme kullanabilir miyim? 
Doğrudan yönetilemez. SQL Server içi veritabanları arasında dolaylı olarak, ancak Azure Hub veritabanı oluşturma ve ardından şirket içi veritabanlarını eşitleme grubuna ekleyerek eşitleyebilirsiniz.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>I veri eşitleme çekirdek veri my üretim veritabanından boş bir veritabanı ve kullanabileceğiniz bunları eşitlenmiş tut? 
Evet. Şema özgün komut dosyası tarafından yeni veritabanında el ile oluşturun. Şema oluşturduktan sonra tabloları veri kopyalamak ve eşitlenmiş kalmasını sağlamak için bir eşitleme grubuna ekleyin.

### <a name="should-i-use-sql-data-sync-to-back-up-and-restore-my-databases"></a>Yedekleme ve geri yükleme my veritabanları için SQL veri eşitleme kullanmalıyım?

Verilerinizi bir yedekleme oluşturmak için SQL veri eşitleme (Önizleme) kullanmak için önerilmez. Yedekleme ve SQL veri eşitleme (Önizleme) eşitlemeleri sürümlü olduğundan zaman içinde belirli bir noktaya geri alamazsınız. Ayrıca, SQL veri eşitleme (Önizleme) saklı yordamlar gibi diğer SQL nesneleri yedeklemez ve geri yükleme işlemi denk hızlı bir şekilde yapın.

Bir yedekleme teknik önerilen için bkz: [bir Azure SQL veritabanını kopyalama](sql-database-copy.md).

### <a name="is-collation-supported-in-sql-data-sync"></a>Harmanlamayı SQL veri eşitleme destekleniyor mu?

Evet. SQL veri eşitleme harmanlama aşağıdaki senaryolarda destekler:

-   Seçilen eşitleme şema tablolar değil zaten hub veya üye veritabanlarınızı sonra eşitleme grubunu dağıttığınızda ise, hizmet otomatik olarak ilgili tabloları ve sütunları boş hedef veritabanları seçili harmanlama ayarları oluşturur.

-   Zaten eşitlenmiş için tabloları hub ve üye veritabanlarında yoksa, SQL veri eşitleme birincil anahtar sütunlarını eşitleme grubunu başarıyla dağıtmak için hub ve üye veritabanları arasında aynı harmanlamaya sahip olmasını gerektirir. Birincil anahtar sütunlar dışındaki sütunları harmanlama sınırlaması vardır.

### <a name="is-federation-supported-in-sql-data-sync"></a>Federasyon SQL veri eşitleme destekleniyor mu?

Federasyon kök veritabanı SQL veri eşitleme (Önizleme) hizmeti herhangi bir sınırlama olmadan kullanılabilir. Geçerli SQL veri eşitleme (Önizleme) sürümüne Federe veritabanı uç noktası ekleyemezsiniz.

## <a name="next-steps"></a>Sonraki adımlar

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)
-   [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [Günlük analizi ile İzleyici Azure SQL veri eşitleme](sql-database-sync-monitor-oms.md)
-   [Azure SQL Data Sync ile ilgili sorun giderme](sql-database-troubleshoot-data-sync.md)

-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL Data Sync REST API belgelerini indirin](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
