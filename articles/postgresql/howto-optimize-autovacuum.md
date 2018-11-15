---
title: PostgreSQL için Azure veritabanı'nda autovacuum en iyi duruma getirme
description: Bu makalede, PostgreSQL için Azure veritabanı'nda autovacuum nasıl iyileştirebileceğiniz de açıklanır.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 3f35779337082b7280398bd13ef870c74f3ec082
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685999"
---
# <a name="optimizing-autovacuum-on-azure-database-for-postgresql-server"></a>PostgreSQL için Azure veritabanı autovacuum en iyi duruma getirme 
Bu makalede, etkili bir şekilde PostgreSQL için Azure veritabanı autovacuum en iyi duruma getirme açıklanır.

## <a name="overview-of-autovacuum"></a>Autovacuum genel bakış
PostgreSQL, büyük veritabanı eşzamanlılık izin vermek için MVCC kullanır. Her güncelleştirme bir INSERT ve delete sonuçlarını ve her silme soft-silinmek üzere işaretlenmiş olan satırları sonuçlanır. Geçici işaretleme sonuçları daha sonra temizlenecek ölü demetleri tanımlama. PostgreSQL, tüm bu Süpür iş çalıştırarak elde eder.

Süpür bir işi elle veya otomatik olarak tetiklenebilir. Daha fazla ölü diziler veritabanı yoğun güncelleştirme yaşadığında mevcut veya işlemler ve daha az boştayken Sil.  Elektrikli daha sık çalıştırmak için gereken veritabanı ağır yük altında olduğunda büyüktür; çalışan Süpür iş yapma **el ile** kullanışsız.

Autovacuum yapılandırılabilir ve ayarlama gelen avantajları. Ürün her türlü Raspberry yönergelerinin dahil olmak üzere cihazları üzerinde çalışır ve ideal yapılandırma değerlerini bir dizi faktöre bağlı emin olmak PostgreSQL ile birlikte gelen varsayılan değerleri deneyin:
- Kaynaklar - toplam SKU ve depolama boyutu.
- Kaynak kullanımı.
- Tek nesne özellikleri.

## <a name="autovacuum-benefits"></a>Autovacuum avantajları
Elektrikli zaman zaman çalıştırmazsanız accumulate ölü diziler neden olabilir:
- Veri Şişirme - daha büyük veritabanları ve tablolar.
- Daha büyük yetersiz dizinler.
- Daha yüksek g/ç.

## <a name="monitoring-bloat-with-autovacuum-queries"></a>Autovacuum sorguları sahip Şişirme izleme
Aşağıdaki örnek sorguda "XYZ" adlı bir tablodaki ölü ve canlı başlıkların sayısını belirlemek için tasarlanmıştır: ' SELECT relname, n_dead_tup n_live_tup, (n_dead_tup / n_live_tup) AS DeadTuplesRatio, last_vacuum last_autovacuum FROM pg_catalog.pg_stat_all_ WHERE relname tabloları DESC; n_dead_tup tarafından = 'XYZ' order'

## <a name="autovacuum-configurations"></a>Autovacuum yapılandırmaları
Geçici bir çözüm iki önemli sorular autovacuum denetleyen yapılandırma parametrelerini çalışmalarınızı:
- Ne zaman başlamalıdır?
- Ne kadar başladıktan sonra temizlemelidir?

Aşağıda bazı güncelleştirebilirsiniz autovacuum yapılandırma parametreleri rehberlik yanı sıra, yukarıdaki soruları dayanır:
Parametre|Açıklama|Varsayılan değer
---|---|---
autovacuum_vacuum_threshold|Herhangi bir tablo BOŞLUKTA tetiklemek için gereken, güncellenen veya silinen tanımlama grubu sayısı alt sınırını belirtir. 50 diziler varsayılandır. Bu parametre yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlanabilir. Ayar tek tek tablolar için Tablo depolama parametrelerinin değiştirilmesi tarafından geçersiz kılınabilir.|50
autovacuum_vacuum_scale_factor|Bir kesir autovacuum_vacuum_threshold için bir ELEKTRİKLİ tetikleme karar verirken eklemek için tablo boyutu belirtir. 0,2 (tablo boyutunun yüzde 20) varsayılandır. Bu parametre yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlanabilir. Ayar tek tek tablolar için Tablo depolama parametrelerinin değiştirilmesi tarafından geçersiz kılınabilir.|yüzde 5
autovacuum_vacuum_cost_limit|Otomatik ELEKTRİKLİ işlemlerinde kullanılacak olan maliyet sınır değeri belirtir. -1 (varsayılan değer olan) belirtilmişse, normal vacuum_cost_limit değeri kullanılır. Birden fazla çalışan ise değer orantılı olarak autovacuum çalışanlar arasında dağıtılır. Her çalışan sınırları toplamını bu değişkenin değeri aşmadığından. Bu parametre yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlanabilir. Ayar tek tek tablolar için Tablo depolama parametrelerinin değiştirilmesi tarafından geçersiz kılınabilir.|-1
autovacuum_vacuum_cost_delay|Otomatik ELEKTRİKLİ işlemlerinde kullanılacak maliyet gecikme değeri belirtir. -1 belirtilirse, bu normal vacuum_cost_delay değeri kullanılır. Varsayılan değer 20 milisaniyedir. Bu parametre yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlanabilir. Ayar tek tek tablolar için Tablo depolama parametrelerinin değiştirilmesi tarafından geçersiz kılınabilir.|20 ms
autovacuum_nap_time|Autovacuum arasındaki en düşük gecikmeyi herhangi belirli bir veritabanı üzerinde çalışan belirtir. Her hepsini arka plan programı veritabanı inceler ve bu veritabanındaki tablolar için gerektiği şekilde ELEKTRİKLİ ve ANALİZ komutlar verir. Gecikme süresini saniye cinsinden ölçülür ve bir dakika (1 dak) varsayılandır. Bu parametre yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlanabilir.|15 s
autovacuum_max_workers|Herhangi bir anda çalışan autovacuum işlemleri (autovacuum Başlatıcısı dışında) en fazla sayısını belirtir. Varsayılan üçüncü bölümüdür. Bu parametre yalnızca sunucu başlangıcında ayarlayabilirsiniz.|3
Yukarıdaki ayarları tek tek tablolar için Tablo depolama parametrelerinin değiştirilmesi tarafından geçersiz kılınabilir.  

## <a name="autovacuum-cost"></a>Autovacuum maliyeti
"Süpür işlemi çalıştırmanın maliyetlerini" aşağıda verilmiştir:
- Bir kilit üzerinde çalıştığı sayfaları elektrikli veri yerleştirilir.
- İşlem ve bellek elektrikli çalıştırırken kullanılır.

Bu, elektrikli ya da çok sık çalıştırmamalısınız veya çok seyrek iş yüküne Uyarlamalı yapması gerekir anlamına gelir. Tüm autovacuum parametre değişiklikleri dezavantajları her biri nedeniyle test etmenizi öneririz.

## <a name="autovacuum-start-trigger"></a>Autovacuum start tetikleyicisi
Ölü tanımlama grubu sayısı autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor aştığında Autovacuum tetiklenir * reltuples, burada reltuples olan sabit bir değer.

Veritabanı yük tutmak autovacuum ağından temizlenmeyi gerekir, aksi takdirde depolama alanınız ve sorgularda genel yavaşlama deneyimi. Zaman içinde amorti edilmiş, hangi elektrikli oranı temizler eski başlıkları ölü diziler oluşturulur oranı eşit.

Veritabanları ile birçok güncelleştirme/silme, daha fazla ölü diziler sahip ve daha fazla alana ihtiyacınız olur. Genellikle, veritabanları ile birçok güncelleştirme/silme autovacuum_vacuum_scale_factor düşük değerleri ve başlıkları ölü Süren birikmesi önlemek için autovacuum_vacuum_threshold düşük değerleri özelliğinden yararlanır. Elektrikli gereksinimini Acil değildir çünkü her iki parametre ile daha küçük veritabanları için yüksek değerler kullanabilirsiniz. Vacuuming sık anımsatıcı, işlem ve bellek karşılığında sunulur.

Kullanılacak varsayılan ölçeklendirme katsayısını yüzde 20 oranında de düşük yüzde ölü tanımlama gruplarını içeren tablolar, ancak yüksek yüzde ölü tanımlama grubu tablolarla çalışır. Örneğin, 20 GB tablosunda bu 4 GB'lık başlıkları ölü dönüşür ve 1 TB tablosunda, 200 GB başlıkları ölü olduğundan.

PostgreSQL ile tablo düzeyinde veya örnek düzeyinde bu parametreleri ayarlayabilirsiniz. Günümüzde, bu parametreler yalnızca Azure veritabanı'nda tablo düzeyinde PostgreSQL için ayarlanabilir.

## <a name="estimating-the-cost-of-autovacuum"></a>Autovacuum maliyetini tahmin etme
Çalışan autovacuum olan "maliyetli" Equal Süpür işlemleri çalışma zamanını denetlemek için parametre yok. Aşağıdaki parametreleri, elektrikli çalıştırmanın maliyeti tahmin etmenize yardımcı:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss 10 =
- vacuum_cost_page_dirty = 20

Süpür işlem fiziksel sayfalar okur ve ölü diziler için kontrol eder. Shared_buffers her sayfa 1 (vacuum_cost_page_hit) maliyetini sahip olduğu kabul edildiği, diğer tüm sayfalar ölü hiçbir tanımlama grubu varsa bir maliyet 20 (başlıkları ölü varsa vacuum_cost_page_dirty) ya da 10 (vacuum_cost_page_miss) sahibi olarak kabul edilir. İşlemi autovacuum_vacuum_cost_limit aştığında Süpür işlemi durdurur.  

Sınıra ulaşıldıktan sonra işlemi yeniden Başlatılmakta olan önce autovacuum_vacuum_cost_delay parametresi tarafından belirtilen süre için uyku moduna geçer. Sınırına değil, autovacuum autovacuum_nap_time parametresi tarafından belirtilen değerden sonra başlar.

Özet olarak, ne kadar veri temizleme birim saat izin verilen autovacuum_vacuum_cost_delay ve autovacuum_vacuum_cost_limit parametreleri denetleyin. Varsayılan değerleri için birçok fiyatlandırma katmanları çok düşük olduğunu unutmayın. En iyi değerleri bu parametrelerin bağımlı katmanı fiyatlandırması ve uygun şekilde yapılandırılmış olması gerekir.

Autovacuum_max_workers parametre sayısı aynı anda çalışan autovacuum işlemleri belirler.

PostgreSQL ile tablo düzeyinde veya örnek düzeyinde bu parametreleri ayarlayabilirsiniz. Günümüzde, bu parametreler yalnızca Azure veritabanı'nda tablo düzeyinde PostgreSQL için ayarlanabilir.

## <a name="optimizing-autovacuum-per-table"></a>Tablo başına autovacuum en iyi duruma getirme
Yukarıdaki tüm yapılandırma parametreleri Tablo başına örneğin yapılandırılabilir:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum olduğu bir tablo zaman uyumlu işlem başına. Ölü diziler tablo büyük yüzdesi vardır, daha yüksek autovacuum "maliyetine".  Güncelleştirme/silme birden çok tablolara oranı yüksek olan tabloları bölme ve "bir tablo üzerinde autovacuum tamamlanması maliyetini" autovacuum paralel hale getirmek için yardımcı olur. Ayrıca, çalışanları liberally zamanlanmış emin olmak için paralel autovacuum çalışanların sayısını artırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Autovacuum ayarlama ve kullanma hakkında daha fazla bilgi edinmek için aşağıdaki PostgreSQL documenatation gözden geçirin:
 - PostgreSQL belgeleri - [Bölüm 18, sunucu yapılandırması](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - PostgreSQL belgeleri – [Bölüm 24, rutin veritabanı bakım görevlerini](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
