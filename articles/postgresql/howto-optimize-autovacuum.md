---
title: PostgreSQL sunucusu için Azure veritabanı üzerinde autovacuum en iyi duruma getirme
description: Bu makalede, PostgreSQL sunucusu için Azure veritabanı üzerinde autovacuum nasıl iyileştirebileceğiniz de açıklanır.
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.topic: conceptual
origin.date: 10/22/2018
ms.date: 04/29/2019
ms.openlocfilehash: e1b4bf1f9fa956da7a7b0ca1521439002d1ce76b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60420279"
---
# <a name="optimize-autovacuum-on-an-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için Azure veritabanı üzerinde autovacuum en iyi duruma getirme 
Bu makalede, PostgreSQL sunucusu için Azure veritabanı üzerinde autovacuum etkili bir şekilde iyileştirmek açıklar.

## <a name="overview-of-autovacuum"></a>Autovacuum genel bakış
PostgreSQL, büyük veritabanı eşzamanlılık izin vermek için çok sürümlü eşzamanlılık denetimi (MVCC) kullanır. Her güncelleştirme bir INSERT ve delete sonuçlarını ve her silme soft-silinmek üzere işaretlenmiş olan satırları sonuçlanır. Geçici işaretleme sonrası temizlenecek ölü tanımlama grubu tanımlar. Bu görevleri gerçekleştirmek için PostgreSQL Süpür işi çalıştırır.

Süpür bir işi elle veya otomatik olarak tetiklenebilir. Daha fazla ölü diziler veritabanı yoğun güncelleştirme yaşadığında mevcut veya silme işlemleri. Veritabanı boşta olduğunda daha az ölü tanımlama grubu yok. Veritabanı yükünü Süpür işlerin getiren yoğun olduğunda daha sık vakum gerek *el ile* kullanışsız.

Autovacuum yapılandırılabilir ve ayarlama gelen avantajları. Ürünün tüm tür cihazlar üzerinde çalıştığından emin olmak PostgreSQL ile birlikte gelen varsayılan değerleri deneyin. Bu cihazlar, Raspberry yönergelerinin içerir. İdeal yapılandırma değerlerini bağlıdır:
- Toplam kaynaklar SKU ve depolama boyutu gibi kullanılabilir.
- Kaynak kullanımı.
- Tek nesne özellikleri.

## <a name="autovacuum-benefits"></a>Autovacuum avantajları
Zaman zaman vakum yoksa, accumulate ölü diziler neden olabilir:
- Daha büyük veritabanları ve tablolar gibi veri Şişirme.
- Daha büyük yetersiz dizinler.
- Daha yüksek g/ç.

## <a name="monitor-bloat-with-autovacuum-queries"></a>İzleyici Şişirme autovacuum sorgularla
Aşağıdaki örnek sorguda, XYZ adlı bir tablodaki ölü ve canlı başlıkların sayısını belirlemek için tasarlanmıştır:
 
    'SELECT relname, n_dead_tup, n_live_tup, (n_dead_tup/ n_live_tup) AS DeadTuplesRatio, last_vacuum, last_autovacuum FROM pg_catalog.pg_stat_all_tables WHERE relname = 'XYZ' order by n_dead_tup DESC;'

## <a name="autovacuum-configurations"></a>Autovacuum yapılandırmaları
İki anahtar soruların yanıtlarını autovacuum denetleyen yapılandırma parametrelerini dayanır:
- Ne zaman başlamalıdır?
- Ne kadar başladıktan sonra temizlemelidir?

Bazı yönergeler yanı sıra önceki sorular güncelleştirebileceğiniz parametreleri dayalı bazı autovacuum yapılandırması aşağıda verilmiştir.

Parametre|Açıklama|Varsayılan değer
---|---|---
autovacuum_vacuum_threshold|Tek bir tabloda Süpür bir işlemi tetiklemek için gereken, güncellenen veya silinen tanımlama grubu sayısı alt sınırını belirtir. 50 diziler varsayılandır. Bu parametreyi yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlayın. Tek tek tablolar için ayarı geçersiz kılmak için Tablo depolama parametreleri değiştirin.|50
autovacuum_vacuum_scale_factor|Bir bölümü için autovacuum_vacuum_threshold Süpür bir işlemi tetiklemek verirken eklemek için tablo boyutu belirtir. 0,2, tablo boyutu yüzde 20'si olan varsayılandır. Bu parametreyi yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlayın. Tek tek tablolar için ayarı geçersiz kılmak için Tablo depolama parametreleri değiştirin.|yüzde 5
autovacuum_vacuum_cost_limit|Otomatik Süpür işlemlerde kullanılan maliyet sınır değeri belirtir. -1, varsayılan değer belirtilmemişse, normal vacuum_cost_limit değeri kullanılır. Birden fazla çalışan ise, değer orantılı olarak çalışan autovacuum çalışanlar arasında dağıtılır. Her çalışan sınırları toplamını bu değişkenin değeri aşmadığından. Bu parametreyi yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlayın. Tek tek tablolar için ayarı geçersiz kılmak için Tablo depolama parametreleri değiştirin.|-1
autovacuum_vacuum_cost_delay|Otomatik Süpür işlemlerde kullanılan maliyet gecikme değeri belirtir. -1 belirtilirse, bu normal vacuum_cost_delay değeri kullanılır. Varsayılan değer 20 milisaniyedir. Bu parametreyi yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlayın. Tek tek tablolar için ayarı geçersiz kılmak için Tablo depolama parametreleri değiştirin.|20 ms
autovacuum_nap_time|Autovacuum arasındaki en düşük gecikmeyi herhangi belirli bir veritabanı üzerinde çalışan belirtir. Her hepsini arka plan programı veritabanı inceler ve bu veritabanındaki tablolar için gerektiği şekilde ELEKTRİKLİ ve ANALİZ komutlar verir. Gecikme süresini saniye cinsinden ölçülür ve bir dakika (1 dak) varsayılandır. Bu parametreyi yalnızca postgresql.conf dosyası veya sunucu komut satırında ayarlayın.|15 s
autovacuum_max_workers|Herhangi bir anda çalıştırabileceğiniz autovacuum Başlatıcısı dışındaki autovacuum işlemlerin en fazla sayısını belirtir. Varsayılan üçüncü bölümüdür. Bu parametreyi yalnızca sunucu başlangıcında ayarlayın.|3

Tek tek tablolar için ayarları geçersiz kılmak için Tablo depolama parametreleri değiştirin. 

## <a name="autovacuum-cost"></a>Autovacuum maliyeti
"Süpür işlemi çalıştırmanın maliyetlerini" şunlardır:

- Elektrikli üzerinde çalıştığı veri sayfaları kilitlenir.
- İşlem ve bellek Süpür bir iş çalışırken kullanılır.

Sonuç olarak, Süpür işleri ya da çok sık veya çok seyrek çalıştırmayın. İş yüküne uyum Süpür iş gerekir. Tüm autovacuum parametre değişiklikleri dezavantajları her biri nedeniyle test edin.

## <a name="autovacuum-start-trigger"></a>Autovacuum start tetikleyicisi
Ölü tanımlama grubu sayısı autovacuum_vacuum_threshold + autovacuum_vacuum_scale_factor aştığında Autovacuum tetiklenir * reltuples. Burada, reltuples bir sabittir.

Autovacuum ağından temizlenmeyi veritabanı yük tutmalısınız. Aksi takdirde depolama alanınız ve sorgularda genel yavaşlama karşılaşırsınız. Zaman içinde amorti edilmiş, oluşturulan ve başlıkları ölü oranı, ölü diziler Süpür işlemi temizler oranı eşit olmalıdır.

Veritabanları birçok güncelleştirme ve silme ile ölü daha fazla tanımlama grubu olması ve daha fazla alana ihtiyacınız olur. Genellikle, veritabanları ile birçok güncelleştirir ve avantajı autovacuum_vacuum_scale_factor autovacuum_vacuum_threshold ve düşük değerleri siler. Düşük değerleri, verilerinizi uzun süren birikmesi ölü tanımlama grubu engel olabilir. Vacuuming gereksinimini daha az Acil olduğundan, yüksek değerler daha küçük veritabanları ile her iki parametre için kullanabilirsiniz. Sık vacuuming işlem ve bellek karşılığında sunulur.

Kullanılacak varsayılan ölçeklendirme katsayısını yüzde 20 oranında başlıkları ölü düşük yüzdesi tablolarla iyi çalışır. İyi tablolarda bulunan başlıkları ölü yüksek miktarda çalışmaz. Örneğin, 20 GB tablosunda, bu ölçek faktörü 4 GB'lık başlıkları ölü çevirir. 1 TB tablosunda, etkin olmayan başlık 200 GB'dır.

PostgreSQL ile tablo düzeyinde veya örnek düzeyinde bu parametreleri ayarlayabilirsiniz. Günümüzde, bu parametreler yalnızca Azure veritabanı'nda tablo düzeyinde PostgreSQL için ayarlayabilirsiniz.

## <a name="estimate-the-cost-of-autovacuum"></a>Autovacuum maliyetini tahmin etmek
Çalışan autovacuum "" maliyetlidir ve Süpür işlemleri çalışma zamanını denetlemek için parametre yok. Aşağıdaki parametreleri, elektrikli çalıştırmanın maliyeti tahmin etmenize yardımcı:
- vacuum_cost_page_hit = 1
- vacuum_cost_page_miss 10 =
- vacuum_cost_page_dirty = 20

Süpür işlem fiziksel sayfalar okur ve ölü diziler için kontrol eder. Shared_buffers her sayfa 1 (vacuum_cost_page_hit) maliyetini olduğu kabul edilir. Diğer tüm sayfalar ölü hiçbir tanımlama grubu varsa bir maliyet 20 (başlıkları ölü varsa vacuum_cost_page_dirty) ya da 10 (vacuum_cost_page_miss) sahibi olarak kabul edilir. İşlemi autovacuum_vacuum_cost_limit aştığında Süpür işlemi durdurur. 

Sınıra ulaşıldıktan sonra işlemi yeniden başlatılmadan önce autovacuum_vacuum_cost_delay parametresi tarafından belirtilen süre için uyku moduna geçer. Sınırına değil, autovacuum autovacuum_nap_time parametresi tarafından belirtilen değerden sonra başlar.

Özet olarak, ne kadar veri temizleme zaman birimi izin verilen autovacuum_vacuum_cost_delay ve autovacuum_vacuum_cost_limit parametreleri denetleyin. Varsayılan değerleri için birçok fiyatlandırma katmanları çok düşük olduğuna dikkat edin. En iyi değerleri bu parametrelerin bağımlı katmanı fiyatlandırması ve uygun şekilde yapılandırılmış olması gerekir.

Autovacuum_max_workers parametre sayısı aynı anda çalışabilecek autovacuum işlemleri belirler.

PostgreSQL ile tablo düzeyinde veya örnek düzeyinde bu parametreleri ayarlayabilirsiniz. Günümüzde, bu parametreler yalnızca Azure veritabanı'nda tablo düzeyinde PostgreSQL için ayarlayabilirsiniz.

## <a name="optimize-autovacuum-per-table"></a>Tablo başına autovacuum en iyi duruma getirme
Önceki tüm yapılandırma parametreleri Tablo başına yapılandırabilirsiniz. Bir örneği aşağıda verilmiştir:
```sql
ALTER TABLE t SET (autovacuum_vacuum_threshold = 1000);
ALTER TABLE t SET (autovacuum_vacuum_scale_factor = 0.1);
ALTER TABLE t SET (autovacuum_vacuum_cost_limit = 1000);
ALTER TABLE t SET (autovacuum_vacuum_cost_delay = 10);
```

Autovacuum bir tablo başına zaman uyumlu işlemidir. Daha büyük bir tablo olan, daha yüksek ölü diziler yüzdesi autovacuum "maliyetine". Güncelleştirme ve silme birden çok tablolara oranı yüksek olan tabloları bölebilirsiniz. Tablolar bölme ve "bir tablo üzerinde autovacuum tamamlanması maliyetini" autovacuum paralel hale getirmek için yardımcı olur. Ayrıca, çalışanları liberally zamanlandığını emin olmak için paralel autovacuum çalışanların sayısını artırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Autovacuum ayarlama ve kullanma hakkında daha fazla bilgi için aşağıdaki PostgreSQL belgelere bakın:

 - [Bölüm 18, sunucu yapılandırması](https://www.postgresql.org/docs/9.5/static/runtime-config-autovacuum.html)
 - [Bölüm 24, yordam veritabanı bakım görevlerini](https://www.postgresql.org/docs/9.6/static/routine-vacuuming.html)
