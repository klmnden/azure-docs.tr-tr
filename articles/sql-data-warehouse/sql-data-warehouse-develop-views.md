---
title: "Azure SQL Data Warehouse'da T-SQL görünümlerini kullanma | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse'da Transact-SQL görünümlerini kullanma ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="views-in-sql-data-warehouse"></a>SQL veri ambarı görünümlerde
Görünümleri SQL veri ambarı'nda özellikle yararlıdır. Çeşitli çözümünüzü kalitesini artırmak için farklı şekillerde kullanılabilir.  Bu makalede ele alınması gereken sınırlamalar yanı sıra görünümleri çözümünüzle zenginleştirmek birkaç örnekleri vurgular.

> [!NOTE]
> Sözdizimi `CREATE VIEW` bu makalede ele alınmamıştır. Lütfen [CREATE VIEW] [ CREATE VIEW] MSDN makalesinde bu başvuru bilgileri için.
> 
> 

## <a name="architectural-abstraction"></a>Mimari Özet
Yaygın bir uygulama düzeni oluşturma tablo AS seçin (desen veri yükleme yaparken yeniden adlandırma nesne tarafından izlenen CTAS) kullanarak tabloları yeniden oluşturmaktır.

Aşağıdaki örnek, bir tarih boyutu yeni tarihi kayıtları ekler. Not DimDate_New, yeni bir tabble ilk nasıl oluşturulur ve özgün sürümü tablosunun değiştirmek için yeniden adlandırıldı.

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

Ancak, bu yaklaşım görünmesini ve "tablo yok" hata iletileri yanı sıra bir kullanıcının görünüm kayboluyor tabloları neden olabilir. Görünümler, arka plandaki nesneleri yeniden adlandırıldıktan adımında kullanıcılara tutarlı sunu katmanı ile sağlamak için kullanılabilir. Kullanıcılar bir görünüm verilerine erişim sağlayarak, kullanıcıların temel tabloların görünürlüğünü olması gerekmez anlamına gelir. Bu, veri tasarımcıları ambarı sağlama veri modeli gelişmesi ve veri yükleme işlemi sırasında CTAS kullanarak performansı en üst düzeye çıkarmak tutarlı bir kullanıcı deneyimi sağlar.    

## <a name="performance-optimization"></a>Performansı iyileştirme
Görünümler, tablolar arasındaki en iyi duruma getirilmiş performans birleştirmelere zorlamak için de kullanılabilir. Örneğin, bir görünüm olarak yedekli dağıtım anahtarı veri taşıma en aza indirmek için katılma ölçütünün bir parçası dahil edebilirsiniz.  Özel bir sorgu veya birleşme ipucu zorlamak için bir görünüm başka bir yararı olabilir. Bu şekilde görünümleri birleştirmeler gereken kullanıcılar için kendi birleştirmelerde doğru yapı hatırlaması önleme bir en iyi şekilde her zaman gerçekleştirildiğinden emin güvence altına alır.

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı görünümlerinde yalnızca meta verilerdir.  Sonuç olarak aşağıdaki seçenekler kullanılabilir değil:

* Şema bağlama seçeneği yoktur
* Temel tabloyu görünüm üzerinden güncelleştirilemiyor
* Geçici tablolar üzerindeki görünüm oluşturulamıyor
* GENİŞLETME için destek yok / NOEXPAND ipuçları
* SQL veri ambarı'nda hiç dizini oluşturulmuş görünüm yok

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Veri Ambarı’nda geliştirmeye genel bakış][SQL Data Warehouse development overview].
İçin `CREATE VIEW` sözdizimi başvurmak için lütfen [CREATE VIEW][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
