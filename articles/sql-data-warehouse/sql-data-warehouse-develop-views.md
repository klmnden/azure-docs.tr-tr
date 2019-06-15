---
title: T-SQL görünümlerini kullanarak Azure SQL veri ambarı'nda | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL veri ambarı'nda T-SQL görünümleri kullanma hakkında ipuçları.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: d4321f8aef6e754d8a1c5b16ac82b4fa62c40949
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873603"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda görünümleri
Çözümleri geliştirme için Azure SQL veri ambarı'nda T-SQL görünümleri kullanma hakkında ipuçları. 

## <a name="why-use-views"></a>Görünümleri neden kullanmalısınız?
Görünümler, bir birkaç farklı yolla çözümünüzün kalitesini artırmak için kullanılabilir.  Bu makalede göz önünde bulundurulması gereken sınırlamalar yanı sıra, görünümleri ile çözümünüzü zenginleştirmek birkaç örnekleri vurgulanır.

> [!NOTE]
> Söz dizimi görünümü oluşturmak için bu makalede ele alınmamıştır. Daha fazla bilgi için [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql) belgeleri.
> 
> 

## <a name="architectural-abstraction"></a>Mimari Özet
Yaygın bir uygulama modeli tabloları CREATE TABLE AS SELECT (veri yükleme yaparken düzeni yeniden adlandırma, bir nesne tarafından izlenen CTAS) kullanarak yeniden oluşturmaktır.

Aşağıdaki örnek bir tarih boyutu için yeni bir tarih kayıtları ekler. Nasıl DimDate_New, yeni bir tablo ilk oluşturulur ve özgün tablonun sürümünü değiştirmek için yeniden adlandırılmış unutmayın.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Ancak, bu yaklaşım görünen ve "tablo yok" hata iletileri yanı sıra bir kullanıcının görünümü kaybolmasını tabloları neden olabilir. Görünümler, temel nesneler olarak yeniden adlandırıldı artırabileceksiniz kullanıcıları içeren bir tutarlı sunu katmanı sağlamak için kullanılabilir. Görünüm verilerine erişim sağlayarak, kullanıcıların temel tablolara görünürlük gerekmez. Bu katman veri modeline veri tasarımcıları ambarı sağlayarak geliştirebilirsiniz tutarlı bir kullanıcı deneyimi sağlar. İşaretleyebilmesine temel tabloları geliştirilebilen tasarımcıları CTAS veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için kullanabileceğiniz anlamına gelir.   

## <a name="performance-optimization"></a>Performansı iyileştirme
Görünümler, performans için iyileştirilmiş tablolar arasındaki birleştirmelere zorlamak için de yararlanılabilir. Örneğin, bir görünüm yedekli dağıtım anahtarı katılma ölçütü veri taşıma en aza indirmek için bir parçası olarak dahil edebilirsiniz. Bir görünümünün başka bir avantajı, belirli bir sorgu veya birleşme ipucu zorlamak için olabilir. Bu şekilde görünümlerini kullanarak birleştirmeler gereken kullanıcılar için hatırlaması, birleşimler için doğru yapı önlemenin en iyi bir şekilde her zaman gerçekleştirilir garanti eder.

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı'nda görünümleri yalnızca meta veri olarak depolanır. Sonuç olarak, aşağıdaki seçenekleri kullanılamaz:

* Şema bağlama seçeneği yoktur
* Temel tabloyu görünüm üzerinden güncelleştirilemiyor
* Geçici tablolar görünümlerini oluşturulamıyor
* İçin genişletme desteği yoktur / NOEXPAND İpucu
* SQL veri ambarı'nda dizini oluşturulmuş görünüm yok

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


