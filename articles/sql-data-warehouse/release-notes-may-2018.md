---
title: Azure SQL veri ambarı sürüm notları May 2018 | Microsoft Docs
description: Azure SQL Data Warehouse için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 843621a8f6e08b2b51d4b7abd05d0ae6c3393fe1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726042"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>Azure SQL Data Warehouse (Mayıs 2018) yenilikler nelerdir?
Azure SQL Data Warehouse sürekli olarak iyileştirmeler alır. Bu makalede, yeni özellikler ve May 2018 sunulan değişiklikler açıklanmaktadır. 

## <a name="gen-2-instances"></a>Gen 2 örnekleri
![alt](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) Azure SQL veri ambarı işlem en iyi duruma getirilmiş Gen2 katmanı bulut veri depolama için yeni performans standartlarını ayarlar. Müşteriler, artık en fazla beş kez daha iyi sorgu performansını almak için daha fazla eşzamanlılık ve beş kez daha yüksek bilgi işlem gücü geçerli nesil karşılaştırıldığında dört kez. Şimdi, en yüksek tüm bulut veri ambarı hizmetinin tek bir kümeden 128 eş zamanlı sorguları hizmet verebilir.

Bkz: [Turbocharge bulut analytics Azure SQL Data Warehouse ile](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) Rohan Kumar, Kurumsal tersine Başkanı, Azure veri blog duyurudan.

## <a name="features"></a>Özellikler

### <a name="auto-statistics"></a>Otomatik istatistikleri
İstatistikleri moderl maliyet tabanlı iyileştirici gibi SQL veri ambarı altyapısında sorgu planı oluşturma en iyi duruma getirmek için kritik öneme sahiptir. Tüm sorguları önceden bilindiğinde istatistikleri nesneleri oluşturulması için gerekenler belirleniyor ulaşılabilir bir görevdir. Sistem geçici ve veri ambarı iş yükleri için tipik olan rastgele sorguları karşılaştığı, Bununla birlikte, sistem yöneticileri başında büyük olasılıkla yetersiz sorgu yürütme planları için oluşturulacak istatistikleri gerekenler tahmin güçlük ve uzun sorgusu yanıt sürelerini. Bu sorunu çözmek için bir istatistik nesneler üzerinde tüm tablo sütunlarını önceden oluşturmak için yoludur. Ancak, istatistikleri nesneleri sırasında yükleme işlemi, uzun yükleme süreleri neden tablo korunmasını gerektiği işlem cezası ile birlikte gelir.

SQL veri ambarı artık daha fazla esneklik, verimliliği ve kullanım kolaylığı sistem yöneticileri ve geliştiriciler için sistem kalite yürütme planları ve en iyi sunmaya devam sağlarken sağlama istatistikleri nesneleri otomatik olarak oluşturulmasını destekler yanıt süreleri.

Etkinleştirmek veya SQL Data warehouse'da otomatik istatistik oluşturmayı devre dışı bırakmak için şu deyimi yürütün:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Bir en iyi yöntem ve kılavuz ayarı öneririz `AUTO_CREATE_STATISTICS` için seçenek `ON`.

> [!NOTE]
> Otomatik istatistik oluşturma işlemi *varsayılan olarak etkin* tüm yeni veri ambarlarında için.
>  

Bkz: [ALTER DATABASE SET seçenekleri](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) ek ayrıntılar için makalenin.

### <a name="rejected-row-support"></a>Satır destek reddetti
Müşteriler sık kullandığınız [PolyBase (tablolar dış veri yüklemek için)](design-elt-data-loading.md) SQL Data Warehouse'a yüksek performans nedeniyle, veri yükleme yapısını paralel. PolyBase olan varsayılan yükleme modeli aracılığıyla verileri yüklenirken [Azure Data Factory](http://azure.com/adf) de. 

SQL veri ambarı reddedilen satır konumu aracılığıyla tanımlama yeteneği ekler `REJECTED_ROW_LOCATION` parametresiyle [dış tablo oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) deyimi. Yürütülmesi sonra bir [oluşturma tablo AS seçin (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) yüklenemedi herhangi bir satır dış tablosundan daha fazla araştırma kaynağı yakın dosyasında depolanır. 

Bkz: [yük güvenle SQL veri ambarı PolyBase reddetti satır konumu ile](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) reddedildi satır davranışı hakkında daha fazla ayrıntı için blogu.

Aşağıdaki örnekte reddedilen satırları belirtmek için yeni sözdizimi gösterilmektedir.

```sql
CREATE EXTERNAL TABLE [dbo].[Reject_Example]
(
    ...
)
WITH
(
    ...
    ,REJECTED_ROW_LOCATION=‘/Reject_Directory'
)
```

### <a name="alter-view"></a>GÖRÜNÜMÜ DEĞİŞTİRME
[ALTER VIEW](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) izinlerini yeniden uygulamak ve görünüm silme/CREATE gerek kalmadan önceden oluşturulmuş bir görünümü değiştirmek bir kullanıcı izin verir. 

Aşağıdaki örnekte önceden oluşturulmuş bir görünümü değiştirir.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

### <a name="concatws"></a>CONCAT_WS
[CONCAT_WS()](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) işlevi bir uçtan uca şekilde iki veya daha fazla değer birleştirme kaynaklanan bir dize döndürür. İlk bağımsız değişkeninde belirtilen sınırlayıcıyı ile birleştirilmiş değerleri ayırır. `CONCAT_WS` İşlevi virgül ile ayrılmış değer (CSV) çıktı oluşturmak için kullanışlıdır.

Aşağıdaki örnek, bir dizi int değerleri virgül ile birleştirerek gösterir.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
Deyim aşağıdaki sonucu döndürür:
```
result
---------
1,2,3
```
Aşağıdaki örnek, bir dizi karma veri türü değerleri virgül ile birleştirerek gösterir.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
Deyim aşağıdaki sonucu döndürür:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```
### <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
[Sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) sistem saklı yordam geçerli ortamı tarafından desteklenen veri türleri hakkında bilgi döndürür. Veri türü araştırma için ODBC bağlantıları üzerinden bağlanma araçları tarafından yaygın olarak kullanılır.

Aşağıdaki örnek, SQL Data Warehouse tarafından desteklenen tüm veri türleri ayrıntılarını alır.

```sql
EXEC sp_datatype_info
```

## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="select-into-with-order-by"></a>SELECT INTO ORDER BY ile
SQL veri ambarı artık engeller `SELECT INTO` içeren sorgular bir `ORDER BY` yan tümcesi. Daha önce bu işlem ilk bellek ve veri tablosu şekli eşleşecek şekilde yeniden sıralama hedef tabloya ekleme verileri sıralama başarılı.

#### <a name="previous-behavior"></a>Önceki davranışı
Aşağıdaki deyim, ek yükü ile işleme başarılı.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

#### <a name="current-behavior"></a>Şu anki davranışı
Aşağıdaki deyim belirten bir hata özel durum oluşturacak `ORDER BY` yan tümcesi desteklenmez bir `SELECT INTO` deyimi.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
Döndürülen hata bildirimi:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

### <a name="set-parseonly-on-query-status"></a>Sorgu durumunu PARSEONLY ayarlayın
Kullanarak `SET PARSEONLY ON` sözdizimi bir kullanıcının her bir T-SQL deyimi sözdizimi inceleyin ve derleme veya deyimi yürütme herhangi bir hata iletisi döndürmek SQL veri ambarı altyapısını sahip olmasını sağlar. Önceden, [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) sistem görünümü, bu deyimleri durumunun kalır `Running` durumu. `sys.dm_pdw_exec_requests` Görünümü şimdi durumu olarak döndürün `Complete`.