---
title: XTP bellek içi depolama izleme | Microsoft Docs
description: Tahmini ve izleme XTP bellek içi depolama alanı, kapasite kullanın. Kapasite hatası 41823 çözümleyin
services: sql-database
ms.service: sql-database
ms.subservice: monitor
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: genemi
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 7542e9fa04eb838baca37dbe13f7cdacdfaf041b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61035768"
---
# <a name="monitor-in-memory-oltp-storage"></a>İzleyici bellek içi OLTP depolama alanı

Kullanırken [bellek içi OLTP](sql-database-in-memory.md), bellek için iyileştirilmiş tablolarda ve Tablo değişkenleri veriler bellek içi OLTP depolama alanında bulunur. Her Premium ve iş açısından kritik hizmet katmanında bellek içi OLTP depolama alanı boyut sınırı bulunur. Bkz: [DTU tabanlı kaynak sınırları - tek veritabanı](sql-database-dtu-resource-limits-single-databases.md), [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md),[sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Bu sınır aşılırsa sonra ekleme ve güncelleştirme işlemleri, hata 41823 tek veritabanları ve elastik havuzlar için 41840 hata ile başarısız başlayabilir. Bu noktada, veri belleği geri kazanmak veya hizmet katmanını yükseltme veya veritabanınızın boyutunu hesaplamak için ya da silmeniz gerekir.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Veri içinde bellek içi OLTP depolama kapasitesi en uygun olup olmadığını belirleme

Farklı hizmet katmanları, depolama caps belirleyin. Bkz: [DTU tabanlı kaynak sınırları - tek veritabanı](sql-database-dtu-resource-limits-single-databases.md), [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md),[sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

Bellek için iyileştirilmiş tablo çalıştığı için aynı şekilde SQL Server için Azure SQL veritabanı'nda mu bellek gereksinimlerini tahmin etme. Bu makale üzerinde gözden geçirmek için birkaç dakikanızı ayırarak [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tablosu ve tablo değişkeni satırları yanı sıra, dizinler, doğru maks. kullanıcı veri boyutu sayısı. Ayrıca, ALTER TABLE tablonun tamamını ve dizinlerini yeni bir sürümünü oluşturmak için yeterli alan gerekir.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
İşlem boyutunuz için bellek içi depolama kullanımı depolama sınırı yüzdesi olarak izleyebilirsiniz [Azure portalında](https://portal.azure.com/): 

1. Veritabanı dikey penceresinde, kaynak kullanımı kutusunu bulun ve Düzenle'yi tıklatın.
2. Ölçümü seçin `In-Memory OLTP Storage percentage`.
3. Uyarı eklemek için ölçüm dikey penceresini açmak için kaynak kullanımını kutusunu tıklatın, sonra Ekle uyarıya tıklayın.

Veya, bellek içi depolama kullanımını göstermek için aşağıdaki sorguyu kullanın:

```sql
    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats
```

## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Belleğin içinde kullanıma OLTP depolama durumlarda - 41823 ve 41840 hataları düzeltin

Bellek içi OLTP depolama kapasitesi Ekle veritabanı sonucu ulaşmaktan, güncelleştirme, ALTER ve oluşturma işlemleri (tek veritabanları için) hata iletisi 41823 veya 41840 (için elastik havuzları) hata ile başarısız oluyor. Her iki hata iptal etmek etkin işlem neden.

Bellek için iyileştirilmiş tablolar ve tablo değişkenlerinde veritabanı veya havuz en yüksek bellek içi OLTP depolama alanı boyutu üst sınırına ulaştınız 41823 ve 41840 hata iletilerini gösterir.

Ya da bu hatayı çözmek için:

* Büyük olasılıkla Geleneksel, disk tabanlı tablolara veri boşaltma bellek için iyileştirilmiş tablolardaki verileri Sil; veya,
* Bellek için iyileştirilmiş tablolarda tutmak için ihtiyacınız olan verileri için yeterli bellek içi depolama ile bir hizmet katmanına yükseltin.

> [!NOTE] 
> Nadiren de olsa, hataları 41823 ve 41840 geçici yeterli kullanılabilir bellek içi OLTP depolama alanı yoktur ve işlemi yeniden denemeden başarılı olabilir. Bu nedenle her iki izleme genel olarak kullanılabilir bellek içi OLTP depolama alanı ve ilk 41823 veya 41840 hata ile karşılaşıldığında yeniden denemek için önerilir. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz: [çakışma algılaması ve bellek içi OLTP ile yeniden deneme mantığı](https://docs.microsoft.com/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Sonraki adımlar
İzleme kılavuzu için bkz: [izleme Azure dinamik yönetim görünümlerini kullanarak SQL veritabanı](sql-database-monitoring-with-dmvs.md).
