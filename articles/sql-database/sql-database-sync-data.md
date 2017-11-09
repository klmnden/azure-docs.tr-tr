---
title: "Azure SQL veri eşitleme (Önizleme) | Microsoft Docs"
description: "Azure SQL veri eşitleme (Önizleme) bu genel bakış sunar"
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: 5c4509bc1d05bc422f6bc5599d4635020ded63e9
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-azure-sql-data-sync-preview"></a>Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme)

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
> Üzerinde bir şirket içi veritabanı kullanıyorsanız, zorunda [bir yerel Aracısı yapılandırın.](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-sql-data-sync)

![Veritabanları arasında eşitleme verileri](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Veri Eşitleme kullanma zamanı

Veri eşitleme veri birkaç Azure SQL veritabanlarını veya SQL Server veritabanları arasında güncel tutulması gereken durumlarda yararlıdır. Veri eşitlemesi için ana kullanım örnekleri şunlardır:

-   **Karma veri eşitleme:** veri eşitleme ile şirket içi veritabanları ve karma uygulamaların etkinleştirmek için Azure SQL veritabanları arasında eşitlenen verileri tutabilirsiniz. Bu özellik, buluta taşıma dikkate ve kendi uygulama bazıları Azure'da yerleştirmek istediğiniz müşterileri ilgisini.

-   **Dağıtılmış uygulamalar:** çoğu durumda, farklı iş yükleri farklı veritabanlarındaki ayırmak faydalı olacaktır. Örneğin, bir üretim veritabanınız var, ancak raporlama ya da analytics iş yükü bu veriler üzerinde çalışması gerekir, bu ek iş yükü için ikinci bir veritabanı yararlıdır. Bu yaklaşım, üretim iş yükü üzerindeki performans etkisini en aza indirir. Bu iki veritabanı eşitlenmiş tutmak için veri eşitleme kullanabilirsiniz.

-   **Genel olarak dağıtılmış uygulamalar:** birçok işletme birkaç bölgeler ve hatta bazı ülkelerde span. Ağ gecikmesini en aza indirmek için yakın bir bölgede verileriniz en iyisidir. Veri Eşitleme ile eşitlenen tüm dünyada bölgelerdeki veritabanlarını kolayca tutabilirsiniz.

Veri Eşitleme aşağıdaki senaryolar için uygun değil:

-   Olağanüstü Durum Kurtarma

-   Ölçek okuma

-   ETL (OLTP OLAP için)

-   Şirket içi SQL Server'dan Azure SQL veritabanı geçiş

## <a name="how-does-data-sync-work"></a>Veri Eşitleme nasıl çalışır? 

-   **Veri değişiklikleri izleme:** veri eşitleme Ekle kullanarak değişiklikleri izler, güncelleştirme ve Tetikleyicileri silin. Değişiklikler, kullanıcı veritabanında tarafındaki tabloya kaydedilir.

-   **Veri eşitleme:** veri eşitleme bir Hub ve bağlı bileşen modeli tasarlanmıştır. Hub tek tek her üye ile eşitlenir. Hub değişikliklerden üyesine karşıdan yüklenir ve Hub'ına üye değişikliklerden sonra yüklenir.

-   **Çakışmaları giderme:** veri eşitleme çakışması çözümleme için iki seçenek sunar *Hub WINS* veya *üye WINS*.
    -   Seçerseniz *Hub WINS*, hub değişiklikleri her zaman üyesinde değişikliklerin üzerine.
    -   Seçerseniz *üye WINS*, üye üzerine yaz değişiklikleri hub'ında yapılan değişiklikler. Birden fazla üye ise, hangi üye eşitlenir son değeri bağlıdır.

## <a name="common-questions"></a>Sık sorulan sorular

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Veri Eşitleme verilerimi ne sıklıkta eşitleyebilirsiniz? 
En az beş dakikada sıklığıdır.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Yalnızca SQL Server içi veritabanları arasında eşitlemek için veri eşitleme kullanabilir miyim? 
Doğrudan yönetilemez. SQL Server içi veritabanları arasında dolaylı olarak, ancak Azure Hub veritabanı oluşturma ve ardından şirket içi veritabanlarını eşitleme grubuna ekleyerek eşitleyebilirsiniz.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>I veri eşitleme çekirdek veri my üretim veritabanından boş bir veritabanı ve kullanabileceğiniz bunları eşitlenmiş tut? 
Evet. Şema özgün komut dosyası tarafından yeni veritabanında el ile oluşturun. Şema oluşturduktan sonra tabloları veri kopyalamak ve eşitlenmiş kalmasını sağlamak için bir eşitleme grubuna ekleyin.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Oluşturulamadı tabloları neden görüyor musunuz?  
Veri Eşitleme yan tablolar değişiklik izleme, veritabanınızdaki oluşturur. Bunları silmeyin veya veri eşitleme çalışmayı durdurur.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Başka bir deyişle hata iletisi aldım "sütununa NULL değer eklenemiyor \<sütun\>. Sütun null değerlere izin vermiyor." Bu ne anlama geliyor ve nasıl hata düzeltebilirsiniz? 
Bu hata iletisi, iki aşağıdaki sorunlardan biri gösterir:
1.  Bir birincil anahtar olmayan bir tablo olabilir. Bu sorunu gidermek için birincil anahtarı eşitleniyor tüm tablolar için ekleyin.
2.  CREATE INDEX deyiminde WHERE yan tümcesi olabilir. Eşitleme, bu durum işlemez. Bu sorunu gidermek için WHERE yan tümcesini kaldırın veya tüm veritabanları için el ile değişiklik. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Veri Eşitleme döngüsel başvurulara nasıl işler? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve sonuç olarak değiştirerek korur?
Veri Eşitleme döngüsel başvurulara işleyemez. Bunları önlemek emin olun. 

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Nasıl dışarı aktarma ve veri eşitleme ile veritabanı alma?
Bir veritabanı olarak dışarı aktardıktan sonra bir `.bacpac` dosya ve yeni bir veritabanı oluşturmak için dosya alma, veri eşitleme yeni veritabanı kullanmak için aşağıdaki iki birini yapmanız gerekir:
1.  Veri eşitleme nesneleri yan tablolar üzerinde Temizleme **yeni veritabanı** kullanarak [bu komut dosyası](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Bu komut tüm gerekli veri eşitleme nesneleri veritabanından siler.
2.  Yeni bir veritabanı ile eşitleme grubunu yeniden oluşturun. Eski eşitleme grubu artık ihtiyacınız varsa dosyayı silin.

## <a name="sync-req-lim"></a>Gereksinimler ve sınırlamalar

### <a name="general-requirements"></a>Genel gereksinimler

-   Her tablonun birincil anahtarı olmalıdır. Herhangi bir satırın birincil anahtarı değerini değiştirmeyin. Bunu yapmak varsa, satır silin ve yeni birincil anahtar değeri ile oluşturun. 

-   Bir tablonun birincil anahtarı olmayan bir kimlik sütunu olamaz.

-   Nesne (veritabanları, tablolar ve sütunlar) adlarını yazdırılabilir karakterleri nokta (.), köşeli ayraç ([) içeren veya sağa kare köşeli ayraç (]) olamaz.

-   Anlık görüntü yalıtımı etkinleştirilmesi gerekir. Daha fazla bilgi için bkz: [anlık görüntü yalıtımı SQL Server'daki](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-considerations"></a>Genel konular

#### <a name="eventual-consistency"></a>Nihai tutarlılık
Veri Eşitleme tetikleyici tabanlı olduğundan, işlem tutarlılığı garanti edilmez. Microsoft, tüm değişiklikler sonunda yapılır ve veri eşitleme veri kaybına neden olmaz garanti eder.

#### <a name="performance-impact"></a>Performans etkisi
Veri Eşitleme kullanır Ekle, Güncelleştir ve değişiklikleri izlemek için Tetikleyiciler silin. Kullanıcı veritabanında değişiklik izleme yan tablolar oluşturur. Bu değişiklik izleme etkinlikleri veritabanının yükünüzü etkiler. Hizmet katmanı değerlendirmek ve gerekirse yükseltin.

### <a name="general-limitations"></a>Genel sınırlamalar

#### <a name="unsupported-data-types"></a>Desteklenmeyen veri türleri

-   FILESTREAM

-   SQL/CLR UDT

-   XMLSchemaCollection (desteklenen XML)

-   İmleç, zaman damgası, HierarchyId

#### <a name="limitations-on-service-and-database-dimensions"></a>Hizmet ve veritabanı boyutları sınırlamalar

| **Boyutlar**                                                      | **Sınırı**              | **Geçici çözüm**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Eşitleme grubu sayısı için herhangi bir veritabanı ait olabilir.       | 5                      |                             |
| Bir tek eşitleme grubundaki uç noktaları sayısı              | 30                     | Birden çok eşitleme grupları oluşturma |
| Bir tek eşitleme grubundaki şirket içi uç noktaları sayısı üst sınırı. | 5                      | Birden çok eşitleme grupları oluşturma |
| Veritabanı, tablo, şema ve sütun adları                       | ad başına 50 karakter |                             |
| Bir eşitleme grubundaki tablolar                                          | 500                    | Birden çok eşitleme grupları oluşturma |
| Bir eşitleme grubundaki bir tablodaki sütunlar                              | 1000                   |                             |
| Bir tabloda veri satır boyutu                                        | 24 mb                  |                             |
| Minimum eşitleme aralığı                                           | 5 dakika              |                             |
|||

## <a name="next-steps"></a>Sonraki adımlar

SQL veri eşitleme hakkında daha fazla bilgi için bkz:

-   [Azure SQL veri eşitlemeye başlama](sql-database-get-started-sql-data-sync.md)
-   [Azure SQL veri eşitleme için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [Azure SQL veri eşitleme ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)

-   SQL veri eşitleme yapılandırmayı gösterir PowerShell örnekleri tamamlayın:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL veri eşitleme REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
