---
title: Temmuz 2018 tarihinden itibaren Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 08/06/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 123198b21122a23d81794db0a5ca2051b15ee2e7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61476128"
---
# <a name="whats-new-in-azure-sql-data-warehouse-july-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Temmuz 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Temmuz 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="lightning-fast-query-performance"></a>Işık hızlı sorgu performansı
[Azure SQL veri ambarı](https://aka.ms/sqldw) karıştırma işlemlerinin artıran anında veri erişim sunulmasıyla birlikte, yeni performans kıyaslamaları ayarlar. Hızlı veri erişimi, doğrudan SQL Server için SQL Server yerel veri işlemleri kullanarak veri taşıma işlemleri için ek yükü azaltır. SQL veri ambarı artık, doğrudan veri taşıma için SQL Server altyapısı ile tümleştirme anlamına gelir **%67 Amazon Redshift daha hızlı** standart iyi tanınan sektörden türetilmiş bir iş yükünü kullanarak [TPC Y (TPC-H) benchmark™](http://www.tpc.org/tpch/).

![Azure SQL veri ambarı, daha hızlı ve ucuz Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/eb3b908a-464d-4847-b384-9f296083a737.png)
<sub>kaynağı: [Gigaom araştırma analisti raporu: Veri ambarı'nda bulut Kıyaslama](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

Çalışma zamanı performansını ötesinde [Gigaom araştırma](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/) rapor, belirli iş yüklerinin ABD Doları maliyet ölçmek için fiyat-performans oranı de ölçülür. SQL veri ambarı olan **en az yüzde 23 ucuz** Redshift 30 TB iş yükleri için daha. Yalnızca hizmet kullanımda olduğunda işlem elastik olarak ölçeklendirmenize yanı sıra duraklatma ve sürdürme iş yükleri için SQL veri ambarı'nın özelliği sayesinde, daha fazla maliyetlerini düşüren, müşterilerin ödeme yaparsınız.
![Azure SQL veri ambarı, daha hızlı ve ucuz Amazon Redshift](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/cb76447e-621e-414b-861e-732ffee5345a.png)
<sub>kaynağı: [Gigaom araştırma analisti raporu: Veri ambarı'nda bulut Kıyaslama](https://gigaom.com/report/data-warehouse-in-the-cloud-benchmark/)</sub>

### <a name="query-concurrency"></a>Sorgu eşzamanlılık
SQL veri ambarı Ayrıca, veri, kuruluşlar arasında erişilebilir olmasını sağlar. Microsoft, böylece daha fazla kullanıcı aynı veritabanını sorgulama yapabilirsiniz ve diğer istekleri tarafından engellenmiş değil 128 eş zamanlı sorguları desteklemek için hizmet geliştirdi. Buna karşılık, Amazon Redshift en fazla eş zamanlı sorguları 50'ye kuruluş içindeki veri erişimi sınırlandırma kısıtlar.

SQL veri ambarı, benzersiz mimarisinin ayrılmış depolama ve işlem ile bağlı bu sorgu performansı ve sorgu eşzamanlılık kazanımları herhangi bir artış fiyat ve yapı olmadan sunar.

## <a name="finer-granularity-for-cross-region-and-server-restores"></a>Çapraz bölge ve sunucu geri yüklemeler için daha iyi tanecikli
Bölgeler ve her 24 saatte gerçekleşen coğrafi olarak yedekli yedeklemeleri seçmek yerine herhangi bir geri yükleme noktası kullanarak sunucuları arasında şimdi geri yükleyebilirsiniz. Bölge ve sunucu geri yükleme, her iki kullanıcı tanımlı ya da otomatik geri yükleme noktaları daha iyi tanecikli ek veri koruma için etkinleştirilmesi için desteklenir. Daha fazla geri yükleme noktaları ile kullanılabilir veri Ambarınızı bölgeler arasında geri yüklerken mantıksal olarak tutarlı olur emin olabilirsiniz.

![Command - Azure SQL veri ambarını geri yükleme](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6ac23972-9ec0-4502-ab10-7b6bc1a3d947.png)
![geri yükleme seçeneklerinizi - Azure SQL veri ambarı](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/6c63bd0e-9c52-414d-b4be-d3bd3774ee08.png)

Daha fazla bilgi için [hızlandırılmış ve esnek geri yükleme noktaları](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blog gönderisi.

## <a name="20-minute-restorations"></a>20 dakika geri yüklemeler
SQL veri ambarı artık sunan herhangi bir veri ambarı'nın bir geri yükleme olur **20 dakikadan** veritabanı boyutundan bağımsız olarak aynı bölge içinde. Geri yükleme işlemi, aynı veya farklı bir mantıksal sunucu aynı bölge içinde olup olmadığını geri zaman geçerlidir. Ayrıca, anlık görüntü işlemi, geri yükleme noktası oluşturmak için gereken süreyi azaltmak için iyileştirilmiştir. Alt içinde geliştirilmesi performans düzeyleri (alt DWU ayarları) olan bir *2 x azaltma* anlık görüntü oluşturma zamanında.

Daha fazla bilgi için [hızlandırılmış ve esnek geri yükleme noktaları](https://azure.microsoft.com/blog/accelerated-and-flexible-restore-points-with-sql-data-warehouse/) blog gönderisi.

## <a name="custom-restoration-configurations"></a>Özel bir geri yükleme yapılandırmaları
Şimdi, Azure portalında geri yüklerken performans düzeyinizi (DWU) değiştirebilirsiniz. Ayrıca, bir yükseltilmiş 2. nesil veri ambarı'na geri yükleyebilirsiniz. Gen 2 örneğine geri yükleyerek etkisi önce Gen2, artık değerlendirebilirsiniz [Gen1 veri ambarınıza yükseltme](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation).

![Özel bir geri yükleme yapılandırması - Azure SQL veri ambarı](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/f4c410c7-8515-409c-a983-0976792b8628.png)

## <a name="spdescribeundeclaredparameters"></a>SP_DESCRIBE_UNDECLARED_PARAMETERS
[Sp_describe_undeclared_parameters](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql) saklı yordam genellikle araçları tarafından parametreleri Transact-SQL toplu işindeki hakkındaki meta verileri almak için kullanılır. Yordam Bu parametre için çıkarılan tür bilgisi batch'le her parametre için bir satır döndürür. 

```sql
EXEC sp_describe_undeclared_parameters
    @tsql = 
    N'SELECT
        object_id, name, type_desc
      FROM
        sys.indexes
      WHERE
        object_id = @id;'
```

Sonuç kümesi hakkında meta veriler içeren `@id` parametre (kısmi sonuç gösterilir)
```
parameter_ordinal | name | suggested_system_type_id | suggested_system_type_name
--------------------------------------------------------------------------------
1                 | @id  | 56                       | int
```
## <a name="sprefreshsqlmodule"></a>SP_REFRESHSQLMODULE
[Sp_refreshsqlmodule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql) saklı yordam, temel alınan meta veriler nedeniyle değişiklikler temel alınan nesnelerin güncel duruma bir veritabanı nesnesi için meta verileri güncelleştirir. Bu görünümün temel tablolardan değiştirilmiş ve görünümü yeniden taşınmadığından oluşabilir. Bu adım, bırakarak ve bağımlı nesneler yeniden kaydeder.

Aşağıdaki örnek, temel alınan tabloda değişiklik nedeniyle eski hale gelir bir görünümü gösterir. İlk sütun değişikliği (Mollie 1) için veriler doğrudur ancak sütun adı geçersiz ve ikinci sütun mevcut değil. fark edeceksiniz. 
```sql
CREATE TABLE base_table (Id INT);
GO

INSERT INTO base_table (Id) VALUES (1);
GO

CREATE VIEW base_view AS SELECT * FROM base_table;
GO

SELECT * FROM base_view;
GO

-- Id
-- ----
-- 1

DROP TABLE base_table;
GO

CREATE TABLE base_table (fname VARCHAR(10), lname VARCHAR(10));
GO

INSERT INTO base_table (fname, lname) VALUES ('Mollie', 'Gallegos');
GO

SELECT * FROM base_view;
GO

-- Id
-- ----------
-- Mollie

EXEC sp_refreshsqlmodule @Name = 'base_view';
GO

SELECT * FROM base_view;
GO

-- fname     | lname
-- ---------- ----------
-- Mollie    | Gallegos
```

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
