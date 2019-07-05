---
title: Azure SQL veritabanı sunucu kaynak sınırları | Microsoft Docs
description: Bu makalede tek veritabanları ve elastik havuzlar için kaynak sınırları Azure SQL veritabanı sunucusuna genel bakış sağlar. Ayrıca, bu kaynak sınırları isabet veya sınırlar aşıldığında ne ile ilgili bilgi sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
manager: craigg
ms.date: 04/18/2019
ms.openlocfilehash: 4e4c0a6cd25587b33c06526b57e6acdbebb69c8b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445639"
---
# <a name="sql-database-resource-limits-for-azure-sql-database-server"></a>Azure SQL veritabanı sunucusu için SQL veritabanı kaynak limitleri

Bu makalede tek veritabanları ve elastik havuzlar yöneten SQL veritabanı sunucusu için SQL veritabanı kaynak limitleri genel bir bakış sağlar. Ayrıca, bu kaynak sınırları isabet veya sınırlar aşıldığında ne ile ilgili bilgi sağlar.

> [!NOTE]
> Yönetilen örnek limitleri için bkz. [yönetilen örnekleri için SQL veritabanı kaynak limitleri](sql-database-managed-instance-resource-limits.md).

## <a name="maximum-resource-limits"></a>En fazla kaynak sınırları

| Resource | Sınır |
| :--- | :--- |
| Sunucu başına veritabanı | 5000 |
| Her bölgede abonelik başına sunucular varsayılan sayısı | 20 |
| Sunucuları her bölgede abonelik başına en fazla sayısı | 200 |  
| DTU / sunucu başına eDTU kotası | 54,000 |  
| Sunucu/örnek başına sanal çekirdek kotası | 540 |
| Sunucu başına en fazla havuz | Dtu veya sanal çekirdek sayısı sınırlıdır. Örneğin, her bir havuzu 1000 Dtu'ya ise, bir sunucu 54 havuzları destekleyebilir.|
|||

> [!NOTE]
> Daha fazla DTU /eDTU kotası, sanal çekirdek kota veya varsayılan tutarından daha fazla sunucu elde etmek için yeni bir destek isteği sorun türünü "Kota" aboneliği için Azure portalında gönderilebilir. DTU / sunucu başına eDTU kota ve veritabanı sınırı sunucu başına elastik havuzlar sayısını kısıtlar.
> [!IMPORTANT]
> Veritabanı sayısı SQL veritabanı sunucu başına sınıra yaklaştığında, aşağıdaki oluşabilir:
> - Sorguları ana veritabanına karşı çalışır artan gecikme süresi'ı seçin.  Bu, kaynak kullanımı istatistikleri sys.resource_stats gibi görünümlerini içerir.
> - Yönetim işlemlerini gecikme artırma ve sunucusundaki veritabanlarını içeren portal bakış açılarını işleme.

### <a name="storage-size"></a>Depolama boyutu
- Tek veritabanları için rources Lütfen ya da başvurmak [DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md) veya [sanal çekirdek tabanlı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md) fiyatlandırma katmanı başına depolama boyutu sınırları.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Veritabanı kaynak limitleri ulaşıldığında ne olur

### <a name="compute-dtus-and-edtus--vcores"></a>İşlem (Dtu ve Edtu / sanal çekirdek)

(Dtu ve Edtu veya sanal çekirdek tarafından ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgunun gecikme süresi artar ve hatta zaman aşımına olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynaklar için kaynak olarak yürütme boş duruma sağlanır.
Yüksek işlem kullanımı ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik havuz, veritabanı ile daha fazla işlem kaynakları sağlamak için işlem boyutunu artırma. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- Her sorgu, kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Veritabanı boş alanı kullanılan en büyük boyut sınırını ulaştığında, veritabanı ekler ve veri boyutunu artırın güncelleştirmeler başarısız ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Veritabanı veya elastik maksimum boyutunu artırma havuz veya daha fazla depolama alanı ekleyin. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- Veritabanını bir elastik havuzda ise, böylece kendi depolama alanını diğer veritabanlarıyla paylaşılmıyor ardından alternatif olarak veritabanı havuz dışına taşınabilir.
- Kullanılmayan alanı geri kazanmak için bir veritabanı daraltır. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetme](sql-database-file-space-management.md)

### <a name="sessions-and-workers-requests"></a>Oturumlarının ve çalışan (istek)

Oturumlar ve çalışan sayısı, hizmet katmanı tarafından belirlenir ve boyutu (Dtu ve Edtu) işlem. Yeni istekler oturumu veya çalışan sınırlara ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir, ancak eş zamanlı çalışan sayısı genellikle tahmin edin ve denetlemek daha zordur. Bu ne zaman veritabanı kaynak limitleri ulaştı ve çalışanları nedeniyle uzun çalışan sorguları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur.

Oturum veya çalışan yüksek kullanım ile karşılaşıldığında, risk azaltma seçenekleri şunlardır:

- Artan hizmet katmanı veya boyutu veritabanınız veya elastik havuzun işlem. Bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).
- İşlem kaynakları için Çekişme nedeniyle artan çalışan kullanımı nedenini ise, her sorgu, kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="transaction-log-rate-governance"></a>İşlem günlüğü oranı idare 
İşlem günlüğü oranı idare ekleme bir işlem toplu gibi iş yükleri için yüksek alma oranlarını sınırlamak için kullanılan Azure SQL veritabanında SELECT INTO ve dizin oluşturur ' dir. Bu limitler izlenir ve günlük kaydı oluşturma hızı için saniyeden düzeyinde zorunlu, kaç tane IOs bakılmaksızın sınırlama aktarım hızı, veri dosyalarını karşı verilebilir.  İşlem günlüğü nesil hızları şu anda doğrusal olarak donanım bağımlı olduğu bir noktaya kadar ölçeklendirme, en büyük günlük ile oranı 96 MB/sn ile satın alma modeli vcore değeri olan izin. 

> [!NOTE]
> İşlem günlüğü dosyaları gerçek fiziksel IOs kapsamındadır veya sınırlı değildir. 

Günlük oranlarını elde edilebilmeleri ve genel sistem, kullanıcı yükü simge durumuna küçültülmüş etkisi işlevselliğini tutabilirsiniz ancak senaryoları, çeşitli Sürdürülen şekilde ayarlanır. Günlük oran idare, işlem günlüğü yedeklemeleri yayımlanan kurtarılabilirliği SLA'lar içinde kalmasını sağlar.  Bu idare aşırı bir biriktirme listesi üzerinde ikincil çoğaltmaları ayrıca önler.

Günlük kayıtlarının oluşturulma gibi her işlem değerlendirilir ve olup olmadığı, en çok istenen günlük fiyatı (MB/sn / saniye) sürdürmek için geciktirileceğini için değerlendirilen. Günlük kayıtlarını yerine depolama alanına Temizlenen olduğunda gecikmeleri günlük oran idare günlüğü oranı oluşturma işlemi sırasında kendisini uygulanan eklenmez.

Gerçek günlük oluşturma zamanında uygulanan ücretler sistemin sabitlenmesini için geçici olarak izin verilen günlük oranlar azaltma geri bildirim mekanizmaları tarafından etkilenebilir. İçine çoğaltma mekanizması günlük alanı koşulu ve kullanılabilirlik grubu dışında çalışan önleme, günlük dosyası alanı yönetimi, genel sistem sınırları geçici olarak düşürebilir. 

Aşağıdaki bekleme türleri günlük oran İdarecisi, trafik şekillendirme yapılandırmalarla kullanıma (içinde kullanıma sunulan [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) DMV):

| Tür bekleyin | Notlar |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Veritabanı sınırlama |
| POOL_LOG_RATE_GOVERNOR | Havuz sınırlama |
| INSTANCE_LOG_RATE_GOVERNOR | Örnek düzeyi sınırlama |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Geri bildirim denetimi, kullanılabilirlik grubu fiziksel Çoğaltmada Premium/iş kritik tutmaktan değil |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Günlük alanı koşulu dışı önlemek için oranları sınırlama, geri bildirim denetimi |
|||

İstenen ölçeklenebilirlik hampering günlük oran sınırı ile karşılaşıldığında, aşağıdaki seçenekleri göz önünde bulundurun:
- Maksimum 96 MB/sn günlük oran alabilmek için daha büyük bir katmana ölçeği artırabilirsiniz. 
- Geçici veri ise, yani bir ETL işlemi verileri hazırlama, (Bu en düşük düzeyde kaydedilir) tempdb yüklenebilir. 
- Analitik senaryoları için bir kapsamdaki kümelenmiş columnstore tablosuna yükleyin. Bu, sıkıştırma nedeniyle gerekli günlük oran azaltır. Bu teknik, CPU kullanımı artırmak ve yalnızca kümelenmiş columnstore dizin yararlı veri kümeleri için geçerlidir. 

## <a name="next-steps"></a>Sonraki adımlar

- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz: [Dtu'lar ve Edtu'lar](sql-database-purchase-models.md#dtu-based-purchasing-model).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı'nda TempDB](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
