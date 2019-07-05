---
title: MySQL için Azure veritabanı'nda Query Store
description: Bu makalede MySQL için Azure veritabanı için Query Store özelliği
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: a24bba0786201f4ea1d1be431107f7bfe26a2a8f
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461714"
---
# <a name="monitor-azure-database-for-mysql-performance-with-query-store"></a>Query Store performansla MySQL için Azure veritabanı izleme

**İçin geçerlidir:**  MySQL 5.7 için Azure veritabanı

> [!NOTE]
> Query Store Önizleme aşamasındadır.

MySQL için Azure veritabanı için Query Store özelliği zaman içinde sorgu performansını izlemek için bir yol sağlar. Query Store performans sorunlarını giderme hızlı bir şekilde yardımcı olarak basitleştirir, uzun süre çalışan ve en yoğun kaynak sorgularını bulun. Query Store geçmişini sorgular ve çalışma zamanı İstatistikleri otomatik olarak yakalar ve bunları gözden geçirmeniz için saklar. Veritabanı kullanım modellerini görebilmeniz için veri zaman pencereleri tarafından ayırır. Tüm kullanıcılar, veritabanları ve sorgular için veriler depolanır **mysql** MySQL örneği için Azure veritabanı'nda veritabanı şeması.

## <a name="common-scenarios-for-using-query-store"></a>Query Store kullanma ile ilgili yaygın senaryolar

Sorgu deposu aşağıdaki gibi senaryolarda, bir süre içinde kullanılabilir:

- Gerileyen sorgu algılama
- Sayısını belirleyen bir sorgu belirli bir zaman penceresinde yürütüldü
- Bir sorgu ortalama yürütme süresi büyük deltaları görmek için zaman pencereleri arasında karşılaştırma

## <a name="enabling-query-store"></a>Query Store etkinleştirme

Query Store bir katılımı özelliği olduğundan, bir sunucuda varsayılan olarak etkin değildir. Query store etkin ya da belirli bir sunucudaki tüm veritabanları için genel olarak devre dışı ve veritabanı başına açıp açılamaz.

### <a name="enable-query-store-using-the-azure-portal"></a>Query Store Azure portalını kullanarak etkinleştirin

1. Azure portalında oturum açın ve MySQL için Azure veritabanı sunucunuza seçin.
1. Seçin **sunucu parametreleri** içinde **ayarları** menüsünün bölümünde.
1. Query_store_capture_mode parametresi için arama yapın.
1. Tüm ayarlayın ve **Kaydet**.

Bekleme istatistikleri, Query Store etkinleştirmek için:

1. Query_store_wait_sampling_capture_mode parametresi için arama yapın.
1. Tüm ayarlayın ve **Kaydet**.

İlk toplu işin mysql veritabanında kalıcı hale getirmek için veri 20 dakikaya kadar izin verin.

## <a name="information-in-query-store"></a>Query Store bilgileri

Query Store iki deposu sahiptir:

- Bir çalışma zamanı istatistikleri sorgu yürütme istatistikleri bilgilerini kalıcı hale getirilmesine yönelik depolayın.
- Bekleme istatistikleri deposu bekleme istatistikleri bilgileri kalıcı hale getirme için.

Çalışma zamanı yürütme istatistikleri çalışma zamanı istatistikleri depolama alanı kullanımını en aza indirmek için bir sabit, yapılandırılabilir zaman penceresi üzerinde toplanır. Bilgiler, bu depoları, sorgu deposu görünümleri sorgulayarak görülebilir.

Aşağıdaki sorgu, Query Store içinde sorguları hakkında bilgi döndürür:

```sql
SELECT * FROM mysql.query_store;
```

Veya bu sorgu için bekleme istatistikleri:

```sql
SELECT * FROM mysql.query_store_wait_stats;
```

## <a name="finding-wait-queries"></a>Bulma bekleme sorguları

Bekleme olay türleri farklı bekleme olaylarını benzerliğe göre demetlerin içine birleştirin. Query Store, bekleme olay türü, belirli bir bekleme olay adı ve söz konusu sorgu sağlar. İstatistikleri, hangi sorgu performans özellikleri için katkıda bulunan bir daha iyi anlayabilmek anlamına gelir. Bu bekleme bilgileri sorgu çalışma zamanı ile ilişkilendirmek işaretleyebilmesine.

Nasıl daha fazla bekleme istatistikleri Query Store kullanarak iş yükünüzü Öngörüler elde edebilir, bazı örnekleri aşağıda verilmiştir:

| **Gözlem** | **Eylem** |
|---|---|
|Yüksek kilit bekler | Etkilenen sorgular için sorgu metinleri denetleyin ve hedef varlıklar tanımlayın. Query Store içinde sık yürütülür aynı varlığı değiştirme diğer sorgularını bulun ve/veya yüksek süresine sahip. Bu sorguları tanımlama sonra eşzamanlılığı artırmak için uygulama mantığını değiştirmeyi düşünün veya daha az kısıtlayıcı bir yalıtım düzeyi kullanın. |
|Yüksek arabellek GÇ bekler | Query Store sorgular çok sayıda fiziksel okur ile bulun. Yüksek g/ç beklediği sorgularla eşleşirlerse taramalar yerine giriş yapmak için temel alınan varlık, bir dizini arar göz önünde bulundurun. Bu sorguları g/ç ek yükü en aza. Denetleme **performans önerileri** sunucunuzun dizin önerileri sorguları iyileştirmek bu sunucu için olup olmadığını görmek için Portalı'nda. |
|Yüksek bellek bekler | Query Store sorgularda tüketen başlıca bellek bulun. Büyük olasılıkla, bu sorgular daha fazla ilerleme durumunu etkilenen sorgular geciktirme. Denetleme **performans önerileri** sunucunuzun bu sorguların iyileştirilmesine dizin önerileri olup olmadığını görmek için Portalı'nda.|

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Query Store etkin olduğunda pencere başına 500 ayrı sorguları en fazla 15 dakikalık toplama windows verileri kaydeder.

Query Store parametreleri yapılandırmak için aşağıdaki seçenekler kullanılabilir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı** |
|---|---|---|---|
| query_store_capture_mode | AÇIK/kapalı değerine göre sorgu deposu özelliği etkinleştirin. Not: Performance_schema kapalı ise, üzerinde query_store_capture_mode kapatma performance_schema ve performans şema araçları bu özellik için gerekli bir alt kümesi üzerinde kapatır. | TÜM | NONE, TÜM |
| query_store_capture_interval | Sorgu deposu aralığı dakika cinsinden yakalayın. İçinde sorgu ölçümleri toplanır aralığı belirtmeye izin verir | 15 | 5 - 60 |
| query_store_capture_utility_queries | Kapatma açık veya kapalı sistemde yürütülen tüm yardımcı programı sorgular yakalamak için. | NO | EVET, HAYIR |
| query_store_retention_period_in_days | Zaman penceresi ilişkin sorgu Deposu'nda veri saklanacağı gün sayısı. | 7 | 1 - 30 |

Özellikle istatistikleri beklemek için aşağıdaki seçenekler geçerlidir.

| **Parametre** | **Açıklama** | **Varsayılan** | **Aralığı** |
|---|---|---|---|
| query_store_wait_sampling_capture_mode | / OFF bekleme istatistikleri ON kapatma izin verir. | YOK | NONE, TÜM |
| query_store_wait_sampling_frequency | Saniye cinsinden bekleme örnekleme Alters sıklığı. 5 ile 300 saniye. | 30 | 5-300 |

> [!NOTE]
> Şu anda **query_store_capture_mode** hem anlamına gelir, bu yapılandırma yerini **query_store_capture_mode** ve **query_store_wait_sampling_capture_mode** Tüm çalışmak bekleme istatistikleri için etkinleştirilmesi gerekir. Varsa **query_store_capture_mode** bekleme istatistikleri etkin performance_schema yanı sıra, sorgu deposu tarafından yakalanan query_text yararlanan bu yana bekleme istatistikleri de kapalıdır sonra devre dışı bırakılır.

Kullanım [Azure portalında](howto-server-parameters.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md) almak veya bir parametre için farklı bir değer ayarlamak için.

## <a name="views-and-functions"></a>Görünümleri ve işlevleri

Görüntüleyin ve Query Store aşağıdaki görünümleri ve işlevleri kullanarak yönetin. Herhangi biri [seçme ayrıcalığı ortak rolün](howto-create-users.md#how-to-create-additional-admin-users-in-azure-database-for-mysql) bu görünümler, Query Store verileri görmek için kullanabilirsiniz. Bu görünümler yalnızca kullanılabilir **mysql** veritabanı.

Sorgular, değişmez değerler ve sabitleri kaldırdıktan sonra kendi yapı bakarak normalleştirilir. İki sorguları dışında değişmez değerler aynıysa, aynı karmaya sahip.

### <a name="mysqlquerystore"></a>mysql.query_store

Bu görünüm Query Store tüm verileri döndürür. Her ayrı bir veritabanı kimliği, kullanıcı kimliği ve sorgu kimliği için bir satır var.

| **Name** | **Veri türü** | **IS_NULLABLE** | **Açıklama** |
|---|---|---|---|
| `schema_name`| varchar(64) | NO | Şema adı |
| `query_id`| bigint(20) | NO| Aynı sorgu farklı Şeması'nda, yeni bir kimliği çalıştırırsa, belirli bir sorgu için oluşturulan benzersiz bir kimlik oluşturulur |
| `timestamp_id` | timestamp| NO| Sorgunun yürütüldüğü zaman damgası. Bu query_store_interval yapılandırmasına dayanır|
| `query_digest_text`| LONGTEXT| NO| Tüm harfleri kaldırdıktan sonra normalleştirilmiş sorgu metni|
| `query_sample_text` | LONGTEXT| NO| İlk değişmez değerleri ile gerçek sorgu görünümü|
| `query_digest_truncated` | bit| EVET| Sorgu metni olup kesildi. Sorgu 1 KB'den daha uzun olması durumunda değeri Evet olacaktır|
| `execution_count` | bigint(20)| NO| Sorgu çalıştırılmış bu zaman damgası kimliği / yapılandırılmış aralığı süresince sayısı|
| `warning_count` | bigint(20)| NO| Bu sorgu iç sırasında oluşturulan uyarı sayısı|
| `error_count` | bigint(20)| NO| Bu sorgu zaman aralığı boyunca oluşturulan hata sayısı|
| `sum_timer_wait` | double| EVET| Bu sorgu aralığı boyunca toplam yürütme süresi|
| `avg_timer_wait` | double| EVET| Bu sorgu aralığı sırasında ortalama yürütme süresi|
| `min_timer_wait` | double| EVET| Bu sorgu için minimum yürütme süresi|
| `max_timer_wait` | double| EVET| En fazla yürütme zamanı|
| `sum_lock_time` | bigint(20)| NO| Bu sorgu yürütme sırasında bu zaman penceresi için tüm kilitler için harcanan süre toplam miktarı|
| `sum_rows_affected` | bigint(20)| NO| Etkilenen satır sayısı|
| `sum_rows_sent` | bigint(20)| NO| İstemciye gönderilen satır sayısı|
| `sum_rows_examined` | bigint(20)| NO| Denetlenen satır sayısı|
| `sum_select_full_join` | bigint(20)| NO| Katılma tam sayısı|
| `sum_select_scan` | bigint(20)| NO| Select tarama sayısı |
| `sum_sort_rows` | bigint(20)| NO| Sıralanmış satır sayısı|
| `sum_no_index_used` | bigint(20)| NO| Dizin sorgu zaman kullanmayan sayısı|
| `sum_no_good_index_used` | bigint(20)| NO| Sorgu yürütme altyapısı iyi dizin ne zaman kullanmayan sayısı|
| `sum_created_tmp_tables` | bigint(20)| NO| Geçici tablolar oluşturulan toplam sayısı|
| `sum_created_tmp_disk_tables` | bigint(20)| NO| Toplam sayısını (g/ç oluşturur) diskte oluşturulan geçici tablolar|
| `first_seen` | timestamp| NO| İlk örneğini (UTC) sorgu toplama penceresi sırasında|
| `last_seen` | timestamp| NO| Son oluşum (UTC) sorgunun bu toplama penceresi sırasında|

### <a name="mysqlquerystorewaitstats"></a>mysql.query_store_wait_stats

Bu görünüm, Query Store olayları verileri döndürür bekleyin. Her farklı veritabanı kimliği, kullanıcı kimliği, sorgu kimliği ve olay için bir satır var.

| **Name**| **Veri türü** | **IS_NULLABLE** | **Açıklama** |
|---|---|---|---|
| `interval_start` | timestamp | NO| Başlangıç aralığı (15 dakikalık artırma)|
| `interval_end` | timestamp | NO| Bitiş aralığı (15 dakikalık artırma)|
| `query_id` | bigint(20) | NO| Normalleştirilmiş sorgu (sorgu deposundan) üzerinde oluşturulan benzersiz kimliği|
| `query_digest_id` | VARCHAR(32) | NO| (Sorgu deposundan) değişmez değerler kaldırdıktan sonra normalleştirilmiş sorgu metni |
| `query_digest_text` | LONGTEXT | NO| Gerçek sorgu (sorgu deposundan) değişmez değerleri ile ilk görünümü |
| `event_type` | VARCHAR(32) | NO| Bekleme olay kategorisi |
| `event_name` | VARCHAR(128) | NO| Bekleme olayın adı |
| `count_star` | bigint(20) | NO| Sorgu aralığı sırasında örneklenen bekleme olay sayısı |
| `sum_timer_wait_ms` | double | NO| Toplam bekleme süresi (milisaniye cinsinden) bu sorgu aralığı sırasında |

### <a name="functions"></a>İşlevler

| **Name**| **Açıklama** |
|---|---|
| `mysql.az_purge_querystore_data(TIMESTAMP)` | Belirli bir zaman damgasını önce tüm sorgu deposu verileri temizler |
| `mysql.az_procedure_purge_querystore_event(TIMESTAMP)` | Belirli bir zaman damgasını önce olay verilerini temizler tüm bekleyin |
| `mysql.az_procedure_purge_recommendation(TIMESTAMP)` | Öneriler, sona erme önce belirtilen zaman damgası olan temizler |

## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

- MySQL sunucusu parametresi varsa `default_transaction_read_only` Query Store, veri yakalama gerçekleştiremez.
- Uzun Unicode sorgular karşılaşırsa, Query Store İşlevler'in kesilmesi (\>= 6000 bayt).
- Bekleme istatistikleri için bekletme süresi 24 saattir.
- Bekleme istatistikleri örnek Za yakalama olayların bir kesir kullanır. Sıklık parametresini kullanarak değiştirilebilir `query_store_wait_sampling_frequency`.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [sorgu performansı öngörüleri](concepts-query-performance-insight.md)