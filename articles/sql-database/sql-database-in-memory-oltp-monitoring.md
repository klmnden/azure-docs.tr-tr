---
title: "İzleme XTP bellek içi depolama | Microsoft Docs"
description: "Tahmin ve İzleyici XTP bellek içi depolama, kapasite kullanın; Kapasite hatayı 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jodebrui
ms.openlocfilehash: 1e7088e80cc86e3c7cf8ae8ea180d797de613e71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>İzleyici bellek içi OLTP depolama
Kullanırken [bellek içi OLTP](sql-database-in-memory.md), bellek için iyileştirilmiş tablolar ve Tablo değişkenlerinin verileri bellek içi OLTP depolamada yer alıyor. Her Premium Hizmet katmanını belgelenen en fazla bir bellek içi OLTP depolama boyutuna sahip [tek veritabanı kaynak sınırları](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) ve [esnek havuzu kaynak sınırlarını](sql-database-resource-limits.md#elastic-pool-change-storage-size). Bu sınır aşılırsa sonra ekleme ve güncelleştirme işlemleri hata 41823 bağımsız veritabanları ve esnek havuzlar için 41840 hata ile başarısız başlayabilir. Bu noktada, ya da belleği geri almasını verileri silmek veya veritabanınızın performans katmanı yükseltin.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Veri içinde bellek içi OLTP depolama ucun uygun olup olmadığını belirleme
Farklı Premium hizmet katmanları, depolama caps belirler. Bkz: [tek veritabanı kaynak sınırları](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) ve [esnek havuzu kaynak sınırlarını](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Bellek için iyileştirilmiş tablo works onu aynı şekilde SQL Server için Azure SQL veritabanı'nda mu bellek gereksinimlerini tahmin etme. Bu makale üzerinde gözden geçirmek için birkaç dakika sürebilir [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tablosu ve tablo değişkeni satırları yanı sıra, dizinler, en fazla kullanıcı veri boyutu doğru sayısı. Buna ek olarak, ALTER TABLE tablonun tamamını ve dizinlerini yeni bir sürümünü oluşturmak için yeterli alan gerekir.

## <a name="monitoring-and-alerting"></a>İzleme ve uyarı
Bellek içi depolama kullanımı depolama ucun yüzdesi olarak, performans katmanı için izleyebileceğiniz [Azure portal](https://portal.azure.com/): 

1. Veritabanı dikey penceresinde kaynak kullanımı kutusunu bulun ve Düzenle'yi tıklatın.
2. Ölçümü seçin `In-Memory OLTP Storage percentage`.
3. Bir uyarı eklemek için ölçüm dikey penceresi açmak için kaynak kullanımı kutusuna tıklayın, sonra Ekle uyarıya tıklayın.

Veya bellek içi depolama kullanımını göstermek için aşağıdaki sorguyu kullanın:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Belleğin içinde dışarı OLTP depolama durumlarda - 41823 ve 41840 hatalarını düzeltin
Bellek içi OLTP depolama ucun INSERT deyiminde veritabanı sonuçlarınızda basarsa, güncelleştirme, ALTER ve hata iletisinin 41823 (tek başına veritabanları) veya hata 41840 (esnek havuzlar için) başarısız olan işlemleri oluşturun. Her iki hata oluşmasına neden iptal etmek etkin işlem.

Hata iletileri 41823 ve 41840 bellek için iyileştirilmiş tablolar ve veritabanı veya havuzu Tablo değişkenlerinin bellek içi OLTP depolama boyut üst sınırına belirtin.

Ya da bu hatayı gidermek için:

* Potansiyel olarak Geleneksel, disk tabanlı tablolara veri boşaltma bellek için iyileştirilmiş tablolardaki verileri silmek; veya,
* Bellek için iyileştirilmiş tablolarda tutmak için gereken verileri için yeterli bellek içi depolama sahip bir hizmet katmanına yükseltin.

> [!NOTE] 
> Nadir durumlarda 41823 ve 41840 hataları yeterli kullanılabilir bellek içi OLTP depolama yoktur ve işlemi yeniden denemeden başarılı anlamına geçici olabilir. Bu nedenle hem izlenecek genel kullanılabilir bellek içi OLTP depolama ve ilk hata 41823 veya 41840 karşılaşıldığında yeniden denemek için öneririz. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz: [çakışma algılamasını ve yeniden deneme mantığı bellek içi OLTP ile](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Sonraki adımlar
Kılavuzu izleme için bkz: [Azure SQL Dinamik Yönetim görünümlerini kullanarak veritabanı izleme](sql-database-monitoring-with-dmvs.md).
