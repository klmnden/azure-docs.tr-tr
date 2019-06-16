---
title: Query Store, PostgreSQL - tek bir sunucu için Azure veritabanı
description: Bu makalede, PostgreSQL - tek bir sunucu için Azure veritabanı için Query Store özelliği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: b622de3e21d26676bb11d81a6facf8fea18cabc1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067193"
---
# <a name="monitor-performance-with-the-query-store"></a>Query Store ile performans izleme

**Şunlara uygulanır:** -Tek bir sunucu 9.6 ve 10 olan PostgreSQL için Azure veritabanı

PostgreSQL için Azure veritabanı için Query Store özelliği zaman içinde sorgu performansını izlemek için bir yol sağlar. Query Store performans sorunlarını giderme hızlı bir şekilde yardımcı olarak basitleştirir, uzun süre çalışan ve en yoğun kaynak sorgularını bulun. Query Store geçmişini sorgular ve çalışma zamanı İstatistikleri otomatik olarak yakalar ve bunları gözden geçirmeniz için saklar. Veritabanı kullanım modellerini görebilmeniz için veri zaman pencereleri tarafından ayırır. Adlı bir veritabanında depolanan tüm kullanıcılar, veritabanları ve sorgular için veri **azure_sys** örneği PostgreSQL için Azure veritabanı'nda.

> [!IMPORTANT]
> Değişiklik yapmayın **azure_sys** veritabanı veya kendi şemaları. Bunun yapılması Query Store ve ilgili performans özellikleri doğru çalışmasını engelleyebilir.

## <a name="enabling-query-store"></a>Query Store etkinleştirme
Query Store bir katılımı özelliği olduğundan, bir sunucuda varsayılan olarak etkin değildir. Deponun etkin ya da belirli bir sunucudaki tüm veritabanları için genel olarak devre dışı ve veritabanı başına açıp açılamaz.

### <a name="enable-query-store-using-the-azure-portal"></a>Query Store Azure portalını kullanarak etkinleştirin
1. Azure portalında oturum açın ve PostgreSQL için Azure veritabanı sunucunuza seçin.
2. Seçin **sunucu parametreleri** içinde **ayarları** menüsünün bölümünde.
3. Arama `pg_qs.query_capture_mode` parametresi.
4. Değerine `TOP` ve **Kaydet**.

Bekleme istatistikleri, Query Store etkinleştirmek için: 
1. Arama `pgms_wait_sampling.query_capture_mode` parametresi.
1. Değerine `ALL` ve **Kaydet**.


Alternatif olarak, Azure CLI kullanarak bu parametreleri ayarlayabilirsiniz.
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
az postgres server configuration set --name pgms_wait_sampling.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value ALL
```

İlk toplu işin azure_sys veritabanında kalıcı hale getirmek için veri 20 dakikaya kadar izin verin.

## <a name="information-in-query-store"></a>Query Store bilgileri
Query Store iki deposu sahiptir:
- Sorgu yürütme istatistikleri bilgilerini kalıcı hale getirmeniz için bir çalışma zamanı istatistikleri deposu.
- Bekleme istatistikleri deposu bekleme istatistikleri bilgileri kalıcı hale getirme için.

Query Store kullanmaya yönelik yaygın senaryolar şunlardır:
- Sayısını belirleyen bir sorgu belirli bir zaman penceresinde yürütüldü
- Bir sorgu ortalama yürütme süresi büyük deltaları görmek için zaman pencereleri arasında karşılaştırma
- Uzun çalışan sorguları geçmişte tanımlayan X saat
- Kaynaklarını bekleyen ilk N sorguları tanımlama
- Belirli bir sorgu için bekleme yapısı anlama

Çalışma zamanı yürütme istatistikleri çalışma zamanı istatistikleri depolama alanı kullanımını en aza indirmek için bir sabit, yapılandırılabilir zaman penceresi üzerinde toplanır. Bilgiler, bu depoları, sorgu deposu görünümleri sorgulayarak görülebilir.

Aşağıdaki sorgu, Query Store içinde sorguları hakkında bilgi döndürür:
```sql
SELECT * FROM query_store.qs_view; 
``` 

Veya bu sorgu için bekleme istatistikleri:
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

## <a name="finding-wait-queries"></a>Bulma bekleme sorguları
Bekleme olay türleri farklı bekleme olaylarını benzerliğe göre demetlerin içine birleştirin. Query Store, bekleme olay türü, belirli bir bekleme olay adı ve söz konusu sorgu sağlar. İstatistikleri, hangi sorgu performans özellikleri için katkıda bulunan bir daha iyi anlayabilmek anlamına gelir. Bu bekleme bilgileri sorgu çalışma zamanı ile ilişkilendirmek işaretleyebilmesine.

Nasıl daha fazla bekleme istatistikleri Query Store kullanarak iş yükünüzü Öngörüler elde edebilir, bazı örnekleri aşağıda verilmiştir:

| **Gözlem** | **Eylem** |
|---|---|
|Yüksek kilit bekler | Etkilenen sorgular için sorgu metinleri denetleyin ve hedef varlıklar tanımlayın. Query Store içinde sık yürütülür aynı varlığı değiştirme diğer sorgularını bulun ve/veya yüksek süresine sahip. Bu sorguları tanımlama sonra eşzamanlılığı artırmak için uygulama mantığını değiştirmeyi düşünün veya daha az kısıtlayıcı bir yalıtım düzeyi kullanın.|
| Yüksek arabellek GÇ bekler | Query Store sorgular çok sayıda fiziksel okur ile bulun. Yüksek g/ç beklediği sorgularla eşleşirlerse yapmak için temel alınan varlık üzerinde dizin giriş taramalar yerine arayan göz önünde bulundurun. Bu sorguları g/ç ek yükü en aza. Denetleme **performans önerileri** sunucunuzun dizin önerileri sorguları iyileştirmek bu sunucu için olup olmadığını görmek için Portalı'nda.|
| Yüksek bellek bekler | Query Store sorgularda tüketen başlıca bellek bulun. Büyük olasılıkla, bu sorgular daha fazla ilerleme durumunu etkilenen sorgular geciktirme. Denetleme **performans önerileri** sunucunuzun bu sorguların iyileştirilmesine dizin önerileri olup olmadığını görmek için Portalı'nda.|

## <a name="configuration-options"></a>Yapılandırma seçenekleri
Query Store etkin olduğunda pencere başına 500 ayrı sorguları en fazla 15 dakikalık toplama windows verileri kaydeder. 

Query Store parametreleri yapılandırmak için aşağıdaki seçenekler kullanılabilir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı**|
|---|---|---|---|
| pg_qs.query_capture_mode | Hangi deyimleri izlenen ayarlar. | Yok | Hiçbiri, üst, tüm |
| pg_qs.max_query_text_length | Kaydedilebilmesi için en yüksek sorgu uzunluğu ayarlar. Uzun sorguları kesilecek. | 6000 | 100 - 10 BİN |
| pg_qs.retention_period_in_days | Saklama süresi ayarlar. | 7 | 1 - 30 |
| pg_qs.track_utility | Yardımcı programı komutları izlenip izlenmeyeceğini ayarlar | açık | açık, kapalı |

Özellikle istatistikleri beklemek için aşağıdaki seçenekler geçerlidir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | Deyimleri için izlenen kümelerini istatistikleri bekleyin. | Yok | None, tüm|
| Pgms_wait_sampling.history_period | Sıklık, hangi bekleme sırasında olayları örneklenen milisaniye cinsinden ayarlayın. | 100 | 1-600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** yerini **pgms_wait_sampling.query_capture_mode**. Pg_qs.query_capture_mode yok ise, pgms_wait_sampling.query_capture_mode ayarının bir etkisi yoktur.


Kullanım [Azure portalında](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md) almak veya bir parametre için farklı bir değer ayarlamak için.

## <a name="views-and-functions"></a>Görünümleri ve işlevleri
Görüntüleyin ve Query Store aşağıdaki görünümleri ve işlevleri kullanarak yönetin. Bu görünümlerden herhangi biri PostgreSQL ortak rolün Query Store verileri görmek için kullanabilirsiniz. Bu görünümler yalnızca kullanılabilir **azure_sys** veritabanı.

Sorgular, değişmez değerler ve sabitleri kaldırdıktan sonra kendi yapı bakarak normalleştirilir. İki sorguları dışında değişmez değerler aynıysa, aynı karmaya sahip.

### <a name="querystoreqsview"></a>query_store.qs_view
Bu görünüm Query Store tüm verileri döndürür. Her ayrı bir veritabanı kimliği, kullanıcı kimliği ve sorgu kimliği için bir satır var. 

|**Ad**   |**Tür** | **Başvuruları**  | **Açıklama**|
|---|---|---|---|
|runtime_stats_entry_id |bigint | | Runtime_stats_entries tablosundan kimliği|
|user_id    |oid    |pg_authid.oid  |Deyim yürüten kullanıcının nesne kimliği|
|db_id  |oid    |pg_database.oid    |Deyimi içinde yürütüldüğü veritabanının OID|
|query_id   |bigint  || Deyimin ayrıştırma ağacından hesaplanan iç karma kodu|
|query_sql_text |VARCHAR(10000)  || Temsilci bildirimi metni. Aynı yapıya sahip farklı sorgular birlikte kümelenmiş; Bu metin sorguların kümedeki ilk alınmıştır.|
|plan_id    |bigint |   |Bu sorguya kullanılamıyor henüz karşılık gelen bir plan kimliği|
|start_time |timestamp  ||  Sorguları zaman demetlere göre toplanır - bir demet zaman aralığını varsayılan değer 15 dakikadır. Bu giriş süresi Demetin karşılık gelen başlangıç zamanı budur.|
|end_time   |timestamp  ||  Bu giriş süresi Demetin karşılık gelen bitiş saati.|
|çağrıları  |bigint  || Sorgu çalıştırılmış sayısı|
|TOTAL_TIME |çift duyarlık   ||  Milisaniye cinsinden toplam sorgu yürütme süresi|
|min_time   |çift duyarlık   ||  Milisaniye cinsinden en düşük sorgu yürütme süresi|
|max_time   |çift duyarlık   ||  Milisaniye cinsinden en yüksek sorgu yürütme süresi|
|mean_time  |çift duyarlık   ||  Sorgu yürütme süresi, milisaniye cinsinden ortalama|
|stddev_time|   çift duyarlık    ||  Sorgu yürütme süresi, milisaniye cinsinden standart sapma |
|Satırları   |bigint ||  Toplam alınan veya deyiminden etkilenen satır sayısı|
|shared_blks_hit|   bigint  ||  Toplam deyimi tarafından paylaşılan blok önbellek isabet sayısı|
|shared_blks_read|  bigint  ||  Bildirimi tarafından paylaşılan bloğu sayısı.|
|shared_blks_dirtied|   bigint   || Deyim tarafından kirlenmiş paylaşılan bloğu sayısı. |
|shared_blks_written|   bigint  ||  Deyim tarafından yazılan paylaşılan bloğu sayısı.|
|local_blks_hit|    bigint ||   Toplam deyimi tarafından yerel blok önbellek isabet sayısı|
|local_blks_read|   bigint   || Deyim tarafından okunur yerel bloğu sayısı.|
|local_blks_dirtied|    bigint  ||  Deyim tarafından kirlenmiş yerel bloğu sayısı.|
|local_blks_written|    bigint  ||  Yerel blok deyimi tarafından yazılan toplam sayısı|
|temp_blks_read |bigint  || Bildirimi tarafından geçici bloğu sayısı.|
|temp_blks_written| bigint   || Deyim tarafından yazılan geçici bloğu sayısı.|
|blk_read_time  |çift duyarlık    || Toplam süre deyimi için harcanan milisaniye cinsinden okuma blokları (track_io_timing etkinleştirilirse, aksi halde sıfır)|
|blk_write_time |çift duyarlık    || Toplam deyim harcanan yazma blokları, milisaniye cinsinden süre (track_io_timing etkinleştirilirse, aksi halde sıfır)|
    
### <a name="querystorequerytextsview"></a>query_store.query_texts_view
Bu görünüm, Query Store sorgu metin verileri döndürür. Olan her ayrı query_text için bir satır vardır.

|**Ad**|  **Tür**|   **Açıklama**|
|---|---|---|
|query_text_id  |bigint     |Query_texts tablo kimliği|
|query_sql_text |VARCHAR(10000)     |Temsilci bildirimi metni. Aynı yapıya sahip farklı sorgular birlikte kümelenmiş; Bu metin sorguların kümedeki ilk alınmıştır.|

### <a name="querystorepgmswaitsamplingview"></a>query_store.pgms_wait_sampling_view
Bu görünüm, Query Store olayları verileri döndürür bekleyin. Her farklı veritabanı kimliği, kullanıcı kimliği, sorgu kimliği ve olay için bir satır var.

|**Ad**|  **Tür**|   **Başvuruları**| **Açıklama**|
|---|---|---|---|
|user_id    |oid    |pg_authid.oid  |Deyim yürüten kullanıcının nesne kimliği|
|db_id  |oid    |pg_database.oid    |Deyimi içinde yürütüldüğü veritabanının OID|
|query_id   |bigint     ||Deyimin ayrıştırma ağacından hesaplanan iç karma kodu|
|event_type |metin       ||Arka uç olarak bekleyen olay türü|
|olay  |metin       ||Arka uç şu anda bekliyorsa bekleme olay adı|
|çağrıları  |Integer        ||Yakalanan aynı olay sayısı|


### <a name="functions"></a>İşlevler
Query_store.qs_reset() returns void

`qs_reset` şu ana kadar Query Store tarafından toplanan tüm istatistikleri atar. Bu işlev, yalnızca sunucu yöneticisi rolü tarafından gerçekleştirilebilir.

Query_store.staging_data_reset() returns void

`staging_data_reset` bellekte Query Store (diğer bir deyişle, veriler değil temizlendi bellekte henüz veritabanına) tarafından toplanan tüm istatistikleri atar. Bu işlev, yalnızca sunucu yöneticisi rolü tarafından gerçekleştirilebilir.

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar
- Bir PostgreSQL sunucusu parametresi default_transaction_read_only sahip, Query Store veri yakalayamaz.
- Uzun Unicode sorgular karşılaşırsa, Query Store İşlevler'in kesilmesi (> = 6000 bayt).


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [senaryoları burada Query Store özellikle yararlı olabilir](concepts-query-store-scenarios.md).
- Daha fazla bilgi edinin [Query Store kullanmak için en iyi yöntemler](concepts-query-store-best-practices.md).
