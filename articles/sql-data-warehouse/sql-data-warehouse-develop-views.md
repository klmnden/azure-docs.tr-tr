---
title: Azure SQL Data Warehouse'da T-SQL görünümlerini kullanma | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse'da T-SQL görünümlerini kullanma ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 28280a067e7008c20361e0a0041c81ba84e7f74c
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526004"
---
# <a name="views-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı görünümlerde
Çözümleri geliştirme için Azure SQL Data Warehouse'da T-SQL görünümlerini kullanma ipuçları. 

## <a name="why-use-views"></a>Görünümleri neden kullanılır?
Görünümleri çeşitli çözümünüzü kalitesini artırmak için farklı şekillerde kullanılabilir.  Bu makalede ele alınması gereken sınırlamalar yanı sıra görünümleri çözümünüzle zenginleştirmek birkaç örnekleri vurgular.

> [!NOTE]
> Sözdizimi CREATE VIEW için bu makalede ele alınmamıştır. Daha fazla bilgi için bkz: [CREATE VIEW](/sql/t-sql/statements/create-view-transact-sql) belgeleri.
> 
> 

## <a name="architectural-abstraction"></a>Mimari Özet
Ortak bir uygulama düzeni oluşturma tablo AS seçin (desen veri yükleme yaparken yeniden adlandırma nesne tarafından izlenen CTAS) kullanarak tabloları yeniden oluşturmaktır.

Aşağıdaki örnek, bir tarih boyutu yeni tarihi kayıtları ekler. Not DimDate_New, yeni bir tablo ilk nasıl oluşturulur ve özgün sürümü tablosunun değiştirmek için yeniden adlandırıldı.

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

Ancak, bu yaklaşım görünmesini ve "tablo yok" hata iletileri yanı sıra bir kullanıcının görünüm kayboluyor tabloları neden olabilir. Görünümler, arka plandaki nesneleri yeniden adlandırıldıktan adımında kullanıcılara tutarlı sunu katmanı ile sağlamak için kullanılabilir. Veri görünümleri aracılığıyla erişim sağlayarak, kullanıcıların temel tablolara görünürlük gerekmez. Bu katman veri tasarımcıları ambarı sağlama veri modeli gelişmesi tutarlı bir kullanıcı deneyimi sağlar. Çalışabilme temel tabloların gelişmesi tasarımcıları CTAS veri yükleme işlemi sırasında performansı en üst düzeye çıkarmak için kullanabileceğiniz anlamına gelir.   

## <a name="performance-optimization"></a>Performansı iyileştirme
Görünümler, tablolar arasındaki en iyi duruma getirilmiş performans birleştirmelere zorlamak için de kullanılabilir. Örneğin, bir görünüm olarak yedekli dağıtım anahtarı veri taşıma en aza indirmek için katılma ölçütünün bir parçası dahil edebilirsiniz. Özel bir sorgu veya birleşme ipucu zorlamak için bir görünüm başka bir yararı olabilir. Bu şekilde görünümleri birleştirmeler gereken kullanıcılar için kendi birleştirmelerde doğru yapı hatırlaması önleme bir en iyi şekilde her zaman gerçekleştirildiğinden emin güvence altına alır.

## <a name="limitations"></a>Sınırlamalar
SQL veri ambarı görünümlerinde yalnızca meta veri depolanır. Sonuç olarak, aşağıdaki seçenekler kullanılabilir değil:

* Şema bağlama seçeneği yoktur
* Temel tabloyu görünüm üzerinden güncelleştirilemiyor
* Geçici tablolar üzerindeki görünüm oluşturulamıyor
* GENİŞLETME için destek yok / NOEXPAND ipuçları
* SQL veri ambarı'nda hiç dizini oluşturulmuş görünüm yok

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


