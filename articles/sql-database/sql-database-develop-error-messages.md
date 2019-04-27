---
title: SQL hata kodları - veritabanı bağlantı hatası | Microsoft Docs
description: 'Genel veritabanı bağlantı hataları ve veritabanı kopyalama sorunlarını genel hatalar gibi SQL Database istemci uygulamaları için SQL hata kodları hakkında bilgi edinin. '
keywords: SQL hata kodu, erişim sql, veritabanı bağlantı hatası, sql hata kodları
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/06/2019
ms.openlocfilehash: 2682f98628f3c1cf22a2c3767f52bedbc148fa62
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60723519"
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>SQL veritabanı istemci uygulamaları için SQL hata kodları: Veritabanı bağlantı hataları ve diğer sorunlar

Bu makalede, SQL veritabanı, veritabanı bağlantı hataları, geçici hatalar (geçici hatalar olarak da bilinir), kaynak İdaresi hataları, veritabanı kopyalama sorunlarını, elastik havuz ve başka hatalar da dahil olmak üzere istemci uygulamaları için SQL hata kodları listelenmektedir. Çoğu kategorileri, Azure SQL veritabanı'na özgü ve Microsoft SQL Server için geçerli değildir. Ayrıca bkz: [sistem hata mesajlarına](https://technet.microsoft.com/library/cc645603(v=sql.105).aspx).

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Veritabanı bağlantı hataları, geçici hataları ve diğer geçici hataları

Aşağıdaki tabloda, bağlantı kaybı hatalarını ve uygulamanızı SQL veritabanına erişmeye çalışırken karşılaşabileceğiniz diğer geçici hatalar için SQL hata kodları kapsar. Azure SQL veritabanı'na bağlanma başlatılan eğitim almak için bkz: [Azure SQL veritabanı'na bağlanma](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>En yaygın veritabanı bağlantı hatalarını ve geçici hata hataları

Azure altyapısının SQL Veritabanı hizmetinde ağır iş yükleri ortaya çıktığında sunucuları dinamik olarak yeniden yapılandırabilme özelliği vardır.  Bu dinamik davranış istemci programınızın SQL Veritabanı bağlantısını kaybetmesine neden olabilir. Bu tür bir hata koşulu olarak adlandırılan bir *geçici hata*.

İstemci programınız kendisini düzeltmek için geçici hata zamanı sonra bağlantı yeniden böylece yeniden deneme mantığına sahiptir önemle tavsiye edilir.  5 saniye önce ilk, yeniden deneme gecikmesi öneririz. Bulut hizmeti aşırı yüklenilmesini 5 saniye riskleri kısa bir gecikmeden sonra yeniden deneniyor. Gecikme büyüme katlanarak, sonraki her yeniden deneme için en fazla 60 saniye.

Geçici hata hataları genellikle, istemci programlarından aşağıdaki hata iletilerinden biri olarak bildirim:

* Veritabanı &lt;db_name&gt; sunucusundaki &lt;Azure_instance&gt; şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin &lt;session_ıd&gt;
* Veritabanı &lt;db_name&gt; sunucusundaki &lt;Azure_instance&gt; şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin &lt;session_ıd&gt;. (Microsoft SQL Server, hata: 40613)
* Varolan bir bağlantı uzak konak tarafından zorla kapatıldı.
* System.Data.Entity.Core.EntityCommandExecutionException: Komut tanımı yürütülürken bir hata oluştu. Ayrıntılar için iç özel duruma bakın. ---> System.Data.SqlClient.SqlException: Sonuçları sunucudan alınırken bir aktarım düzeyinde hata oluştu. (sağlayıcısı: Oturum sağlayıcısı, hata: 19 - fiziksel bağlantı değilse kullanılabilir)
* Yeniden yapılandırma sürecinde veritabanıdır ve birincil veritabanının etkin bir işlem sırasında ortasında yeni sayfalar uygulama meşgul ikincil bir veritabanı bağlantı denemesi başarısız oldu. 

Yeniden deneme mantığı kod örnekleri için bkz:

* [SQL veritabanı ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) 
* [SQL veritabanı'nda bağlantı hatalarını ve geçici hataları düzeltmek için Eylemler](sql-database-connectivity-issues.md)

Bir tartışma *engelleme süresi* ADO.NET kullanan istemciler için kullanılabilir [SQL Server Connection Pooling (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Geçici hata hata kodları

Aşağıdaki hatalar geçicidir ve uygulama mantığını yeniden denenmesi gerekiyor: 

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 4060 |16 |Veritabanı açılamıyor. "%.&#x2a;ls" oturum açma tarafından istenen. Oturum açma başarısız. Daha fazla bilgi için [hataları için 4000 4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999)|
| 40197 |17 |Hizmet isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin. Hata kodu %d.<br/><br/>Hizmetin kapalı yazılım veya donanım yükseltmeleri, donanım arızaları veya başka bir yük devretme sorunları nedeniyle olduğunda bu hatayı alırsınız. Hata 40197 ileti içinde gömülü hata kodu (%d) hatası oluştu, yük devretme veya türü hakkında ek bilgi sağlar. Bazı kodları 40197 hata iletisi içinde gömülü hata 40020, 40143 40166 ve 40540 örnekleridir.<br/><br/>SQL veritabanı sunucusuna otomatik olarak yeniden bağlanmayı veritabanınızı sağlıklı bir kopyasına bağlanır. Uygulamanızı hata 40197, günlük sorun giderme iletisi içinde katıştırılmış bir hata kodu (%d) catch ve kaynakların kullanılabilir olduğundan ve, bağlantı yeniden kurulana kadar SQL veritabanı'na yeniden bağlanmayı deneyin. Daha fazla bilgi için [geçici hatalar](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 40501 |20 |Hizmet şu anda meşgul. İsteği 10 saniye sonra yeniden deneyin. Olay Kimliği: %ls. Kodu: %d. Daha fazla bilgi için bkz. <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).|
| 40613 |17 |Veritabanı '%.&#x2a;ls' sunucusundaki '%.&#x2a;ls' şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin '%.&#x2a;ls'.<br/><br/> Veritabanına kurulan bir varolan adanmış yönetici bağlantısı (DAC) zaten varsa, bu hata oluşabilir. Daha fazla bilgi için [geçici hatalar](sql-database-connectivity-issues.md#transient-errors-transient-faults).|
| 49918 |16 |İsteği işlenemiyor. İsteği işlemek için yeterli kaynak yok.<br/><br/>Hizmet şu anda meşgul. Lütfen istek daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). |
| 49919 |16 |İşlem oluşturamaz veya güncelleştirme isteği. Çok fazla sayıda oluşturma veya güncelleştirme işlemleri sürüyor abonelik için "% ld".<br/><br/>Hizmet meşgul oluşturmak veya güncelleştirmek için abonelik veya sunucu istekleri birden çok işleme. İstek için kaynak iyileştirme şu anda engelleniyor. Sorgu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) için bekleyen işlemler. Bekleyen oluşturma veya güncelleştirme istekleri tamamlanana veya, bekleyen isteklerden birini silip isteğinizi daha sonra yeniden deneyin kadar bekleyin. Daha fazla bilgi için bkz. <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). |
| 49920 |16 |İsteği işlenemiyor. Abonelik için çok sayıda işlem devam eden "% ld".<br/><br/>Bu abonelik için birden çok isteklerin işlenmesiyle meşgul hizmetidir. İstek için kaynak iyileştirme şu anda engelleniyor. Sorgu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) işlem durumu. Bekleyen istekler kadar bekleyin, tamamlandı veya bekleyen isteklerinizden birini silin ve isteğinizi daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). |
| 4221 |16 |Okuma-ikincil oturum açma, 'Hadr_database_waıt_for_transıtıon_to_versıonıng' üzerindeki uzun bekleme nedeniyle başarısız oldu. Çoğaltma dönüştürüldü olduğunda yürütülen işlemler için satır sürümleri eksik olduğundan çoğaltma oturum açma için kullanılamıyor. Sorun, birincil çoğaltmadaki etkin işlemler geri alma veya çözülebilir. Bu durum oluşumlarını birincil üzerinde uzun yazma işlemlerinden önleyerek indirgenebilir. |

## <a name="database-copy-errors"></a>Veritabanı kopyalama hataları

Azure SQL veritabanında bir veritabanı kopyalanırken şu hatalarla karşılaşılabilir. Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md).

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 40635 |16 |İstemci IP adresi '%.&#x2a;ls' geçici olarak devre dışıdır. |
| 40637 |16 |Oluşturma veritabanı kopyalama şu anda devre dışı. |
| 40561 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veya hedef veritabanı yok. |
| 40562 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı bırakılmış. |
| 40563 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı bırakılmış. |
| 40564 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40565 |16 |Veritabanı kopyalama başarısız oldu. 1'den fazla eş zamanlı veritabanı kopyasını aynı kaynaktan izin verilir. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |
| 40566 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40567 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40568 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı kullanılamaz duruma geldi. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40569 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı kullanılamıyor. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40570 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |
| 40571 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız. Lütfen hedef veritabanını bırakın ve daha sonra tekrar deneyin. |

## <a name="resource-governance-errors"></a>Kaynak İdaresi hataları

Azure SQL veritabanı ile çalışırken kaynakların aşırı kullanımı şu hatalar nedeniyle. Örneğin:

* Bir işlem çok uzun süre açık kaldığı.
* Bir işlem çok fazla sayıda kilit tutuyor.
* Uygulamanın çok fazla bellek tüketiyor.
* Uygulamanın çok fazla tüketen `TempDb` alanı.

İlgili konu başlıkları:

* Daha fazla bilgi için bkz.
  * [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)
  * [Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)
  * [Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)
  * [tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)
  * [Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)
  * [Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). 

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 10928 |20 |Kaynak Kimliği: %d. Veritabanı %s sınırı %d şeklindedir ve üst sınırına ulaşıldı. Daha fazla bilgi için [tek ve havuza alınmış veritabanları için SQL veritabanı kaynak limitleri](sql-database-resource-limits-database-server.md).<br/><br/>Kaynak Kimliği sınırına kaynak gösterir. Çalışan iş parçacıkları, kaynak kimliği için = 1. Oturumlarının kaynak kimliği = 2.<br/><br/>Bu hata ve nasıl çözümleyeceğiniz hakkında daha fazla bilgi için bkz: <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). |
| 10929 |20 |Kaynak Kimliği: %d. %S en az garantisi %d, üst sınır: %d, ve veritabanı için geçerli kullanım %d. Ancak, sunucu şu anda bu veritabanı için %d büyük istekler desteklemek için çok meşgul. Kaynak Kimliği sınırına kaynak gösterir. Çalışan iş parçacıkları, kaynak kimliği için = 1. Oturumlarının kaynak kimliği = 2. Daha fazla bilgi için bkz. <br/>&bull; &nbsp;[Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[Tek veritabanları için DTU tabanlı sınırları](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md). <br/>Aksi halde, lütfen daha sonra tekrar deneyin. |
| 40544 |20 |Veritabanı boyut kotasına ulaştı. Verileri bölün veya silin, dizinleri bırakın veya olası çözümler için belgelere bakın. Veritabanı ölçekleme için bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).|
| 40549 |16 |Uzun süre çalışan bir işlemin olduğundan oturum sonlandırıldı. İşleminiz kısaltmayı deneyin. Toplu işleme hakkında daha fazla bilgi için bkz: [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma nasıl](sql-database-use-batching-to-improve-performance.md).|
| 40550 |16 |Oturum, çok fazla sayıda kilit aldığından sonlandırıldı. Try okuma veya tek bir işlemde daha az sayıda satır değiştirme. Toplu işleme hakkında daha fazla bilgi için bkz: [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma nasıl](sql-database-use-batching-to-improve-performance.md).|
| 40551 |16 |Oturum nedeniyle aşırı sonlandırıldı `TEMPDB` kullanım. Geçici tablo alanı kullanımını azaltacak şekilde sorgunuzu değiştirmeyi deneyin.<br/><br/>Geçici nesneler kullanıyorsanız alandan tasarruf etmek `TEMPDB` oturumunda artık gerekli olmayan bırakarak geçici nesneler tarafından veritabanı. SQL veritabanı tempdb kullanımı hakkında daha fazla bilgi için bkz. [SQL veritabanı'nda veritabanı Tempdb](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).|
| 40552 |16 |Aşırı işlem günlüğü alanı kullanımı nedeniyle oturum sonlandırıldı. Tek bir işlemde daha az sayıda satır değiştirmeyi deneyin. Toplu işleme hakkında daha fazla bilgi için bkz: [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma nasıl](sql-database-use-batching-to-improve-performance.md).<br/><br/>Gerçekleştirdiğiniz toplu ekler kullanarak `bcp.exe` yardımcı programı veya `System.Data.SqlClient.SqlBulkCopy` sınıfı, kullanmayı deneyin `-b batchsize` veya `BatchSize` sunucunun her bir işlem içinde satır sayısını sınırlamak için seçenekleri kopyalanır. Dizin ile yeniden varsa `ALTER INDEX` ifadesi kullanmayı deneyin `REBUILD WITH ONLINE = ON` seçeneği. Sanal çekirdek satın alma modeli için işlem günlüğü boyutları hakkında daha fazla bilgi için bkz: <br/>&bull; &nbsp;[tek veritabanları için sanal çekirdek tabanlı sınırları](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).|
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Sorgunuzu daha az sayıda satır işleyecek şekilde değiştirmeyi deneyin.<br/><br/>Sayısını azaltmayı `ORDER BY` ve `GROUP BY` Transact-SQL kodunuzu işlemlerinde sorgunuzu bellek gereksinimlerini azaltır. Veritabanı ölçekleme için bkz: [tek veritabanı kaynaklarının ölçeğini](sql-database-single-database-scale.md) ve [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).|

## <a name="elastic-pool-errors"></a>Elastik Havuz Hataları

Oluşturma ve elastik havuzlar kullanarak şu hatalarla ilgili:

| Hata kodu | Severity | Açıklama | Düzeltici Eylem |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |Esnek havuz depolama sınırına ulaştı. Esnek havuz depolama alanı kullanımı (%d) MB aşamaz. Esnek havuz depolama sınırını ulaşıldığında bir veritabanına veri yazmak çalışıyor. Kaynak sınırları hakkında daha fazla bilgi için bkz: <br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md). <br/> |Dtu'larının artırmayı deneyin ve/veya kendi depolama sınırını artırmak için elastik havuza mümkünse ekleyerek depolama elastik havuz içindeki tek tek veritabanları tarafından kullanılan depolama veya veritabanlarını elastik havuzdan kaldırabilirsiniz. Elastik havuzu ölçekleme için bkz: [ölçeğini elastik havuz kaynakları](sql-database-elastic-pool-scale.md).|
| 10929 | 16 |%S en az garantisi %d, üst sınır: %d, ve veritabanı için geçerli kullanım %d. Ancak, sunucu şu anda bu veritabanı için %d büyük istekler desteklemek için çok meşgul. Kaynak sınırları hakkında daha fazla bilgi için bkz: <br/>&bull; &nbsp;[Elastik havuzlar için DTU tabanlı limitleri](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[Elastik havuzlar için sanal çekirdek tabanlı limitleri](sql-database-vcore-resource-limits-elastic-pools.md). <br/> Aksi halde, lütfen daha sonra tekrar deneyin. DTU /; veritabanı başına en düşük vCore değeri DTU / veritabanı başına en yüksek vCore değeri. Esnek havuzdaki tüm veritabanları arasında eş zamanlı çalışan (istek) toplam sayısı, havuz sınırı aşan çalışıldı. |Alt sınırını artırmak için Dtu veya sanal çekirdek esnek havuzun mümkünse artırmayı deneyin veya veritabanlarını elastik havuzdan kaldırabilirsiniz. |
| 40844 | 16 |Veritabanı '%ls' sunucusundaki '%ls' bir elastik havuzdaki '%ls' sürümü veritabanıdır ve sürekli kopyalama ilişkiye sahip olamaz.  |Yok |
| 40857 | 16 |Esnek havuz için sunucu bulunamadı: '%ls', elastik havuz adı: '%ls'. Belirtilen bir elastik havuz, belirtilen sunucuda yok. | Geçerli bir elastik havuz adı sağlayın. |
| 40858 | 16 |'%Ls' esnek havuzu zaten şu sunucuda: '%ls'. Belirtilen bir elastik havuz, belirtilen SQL veritabanı sunucusu zaten mevcut. | Yeni elastik havuz adı sağlayın. |
| 40859 | 16 |Elastik havuz, '%ls' hizmet katmanı desteklemez. Belirtilen hizmet katmanı, elastik havuz sağlama için desteklenmiyor. |Doğru sürümü sağlayın veya hizmet katmanı varsayılan hizmet katmanı kullanmak için boş bırakın. |
| 40860 | 16 |Elastik havuz, '%ls' ve hizmet hedefi '%ls' birleşimi geçerli değil. Elastik havuz ve hizmet katmanı belirtilebilir birlikte yalnızca 'ElasticPool' kaynak türü belirtilirse. |Elastik havuz ve Hizmet katmanını doğru birleşimini belirtin. |
| 40861 | 16 |Veritabanı sürümü ' %. *ls olan esnek havuz katmanından farklı olamaz ' %.* ls'. Esnek havuz katmanından farklı veritabanı sürümüdür. |Esnek havuz katmanından farklı bir veritabanı sürümü belirtmeyin.  Veritabanı sürümü belirtilmesi gerekmez unutmayın. |
| 40862 | 16 |Esnek havuz adının belirtilmesi gerekir esnek havuz hizmeti hedefi belirtildiyse smbiosguid'sinin. Esnek havuz hizmeti hedefi, bir elastik havuzu benzersiz olarak tanımlamıyor. |Esnek havuz hizmeti hedefi kullanarak elastik havuz adı belirtin. |
| 40864 | 16 |Elastik havuz için Dtu'lar'ın en az olmalıdır (%d) Dtu hizmet katmanı için ' %. * ls'. Alt sınır aşağıda elastik havuz için Dtu'lar yapılmaya çalışılıyor. |Esnek havuz için en az alt sınır Dtu ayarı yeniden deneyin. |
| 40865 | 16 |Elastik havuz için Dtu'lar (%d) Dtu hizmet katmanı için aşamaz ' %. * ls'. Üst sınırı üstünde elastik havuz için Dtu'lar yapılmaya çalışılıyor. |Yeniden deneme sınırına aşmayan elastik havuz için Dtu'lar ayarlanamadı. |
| 40867 | 16 |Veritabanı başına maksimum DTU olmalıdır en az (%d) hizmet katmanı için ' %. * ls'. Desteklenen sınırı aşağıdaki veritabanı başına en fazla DTU yapılmaya çalışılıyor. | İstenen ayarını destekleyen esnek havuz Hizmet katmanını kullanmayı düşünün. |
| 40868 | 16 |Hizmet katmanı için veritabanı başına maksimum DTU (%d) büyük olamaz ' %. * ls'. Desteklenen sınırı aşan veritabanı başına en fazla DTU yapılmaya çalışılıyor. | İstenen ayarını destekleyen esnek havuz Hizmet katmanını kullanmayı düşünün. |
| 40870 | 16 |Hizmet katmanı için veritabanı başına minimum DTU (%d) büyük olamaz ' %. * ls'. Desteklenen sınırı aşan veritabanı başına minimum DTU yapılmaya çalışılıyor. | İstenen ayarını destekleyen esnek havuz Hizmet katmanını kullanmayı düşünün. |
| 40873 | 16 |Dtu esnek havuzun (%d) veritabanları (%d) ve (%d) veritabanı başına minimum DTU sayısını aşamaz. Esnek havuz Dtu aşan esnek havuzdaki veritabanları için minimum DTU belirtin çalışılıyor. | Elastik havuz için Dtu'lar artırmayı veya veritabanı başına minimum DTU azaltın veya esnek havuzdaki veritabanlarının sayısını azaltın. |
| 40877 | 16 |Herhangi bir veritabanına içermediği sürece, bir elastik havuz silinemez. Esnek havuz, bir veya daha fazla veritabanı içerir ve bu nedenle silinemiyor. |Veritabanları, silmek için esnek havuzdan kaldırın. |
| 40881 | 16 |Esnek havuzu ' %. * ls, veritabanı sayısı sınırına ulaşıldı.  Esnek havuz için veritabanı sayısı sınırına (%d) dtu'ları ile bir elastik havuz için (%d) aşamaz. Oluşturma veya elastik havuz, veritabanı sayısı sınırına ulaşıldığında veritabanı için elastik havuz ekleme çalışılıyor. | Veritabanı sınırını artırmak için Dtu esnek havuzun mümkünse artırmayı deneyin veya veritabanlarını elastik havuzdan kaldırabilirsiniz. |
| 40889 | 16 |Dtu'lar ve esnek havuz depolama sınırı ' %. * ls kullanılamaz azaldıkça, yeterli depolama alanına veritabanları için sağlamayacağından. Depolama kullanımını aşağıda esnek havuz depolama sınırını azaltın çalışılıyor. | Tek veritabanlarını elastik havuzdaki depolama kullanımını azaltmayı deneyin veya veritabanlarını kendi Dtu'lar ve depolama sınırı azaltmak için havuzdan kaldırabilirsiniz. |
| 40891 | 16 |(%D) veritabanı başına minimum DTU (%d) veritabanı başına maksimum DTU'yu aşamaz. Veritabanı başına en fazla DTU daha yüksek veritabanı başına minimum DTU yapılmaya çalışılıyor. |Veritabanı başına minimum DTU, veritabanı başına en fazla DTU aşmadığından emin olun. |
| TBD | 16 |Bir elastik havuzdaki tek bir veritabanı depolama alanı boyutu tarafından izin verilen en büyük boyutu aşamaz ' %. * ls hizmet katmanı elastik havuz. Veritabanı için en büyük boyutu esnek havuz hizmet katmanı tarafından izin verilen en büyük boyutu aşıyor. |Esnek havuz hizmet katmanı tarafından izin verilen en büyük boyutu sınırları dahilinde veritabanının maksimum boyutunu ayarlayın. |

İlgili konu başlıkları:

* [Elastik havuz (C#) oluşturma](sql-database-elastic-pool-manage-csharp.md)
* [Bir esnek havuzunu yönetme (C#)](sql-database-elastic-pool-manage-csharp.md)
* [Elastik havuz (PowerShell) oluşturma](sql-database-elastic-pool-manage-powershell.md)
* [İzleme ve yönetme (PowerShell) elastik havuz](sql-database-elastic-pool-manage-powershell.md)

## <a name="general-errors"></a>Genel hata

Aşağıdaki hatalar, önceki tüm kategoriye ayrılır değil.

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| [15006](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-15000-to-15999) |16 |(AdministratorLogin), geçersiz karakterler içerdiğinden geçerli bir ad değil.|
| [18452](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Oturum açma başarısız. Oturum açma güvenilmeyen bir etki alanından ve Windows authentication.%.&#x2a;ls ile kullanılamaz ls (Windows oturumu açma desteklenmez SQL Server'ın bu sürümünde.) |
| [18456](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Oturum açma başarısız oldu, kullanıcı için ' %. &#x2a;ls'.%. &#x2a;ls %. &#x2a;ls (kullanıcısı için oturum açılamadı "%.&#x2a; ls".) |
| [18470](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-18000-to-18999) |14 |Kullanıcı için oturum açma başarısız '%.&#x2a;ls'. Neden: Disabled.% hesaptır. &#x2a;ls |
| 40014 |16 |Birden fazla veritabanı aynı işlemde kullanılamaz. |
| 40054 |16 |Bu SQL Server sürümünde kümelenmiş dizini olmayan tablolar desteklenmez. Kümelenmiş bir dizin oluşturun ve yeniden deneyin. |
| 40133 |15 |Bu işlem, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40506 |16 |Belirtilen SID bu SQL Server sürümü için geçerli değil. |
| 40507 |16 |'%.&#x2a;ls' SQL Server'ın bu sürümünde parametrelerle çağrılacak olamaz. |
| 40508 |16 |USE deyiminin veritabanları arasında geçiş yapmak için desteklenmiyor. Farklı bir veritabanına bağlanmak için yeni bir bağlantı kullanın. |
| 40510 |16 |Deyimi '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor |
| 40511 |16 |Yerleşik işlevi '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40512 |16 |Kullanım dışı bırakılan özellik '%ls', SQL Server'ın bu sürümünde desteklenmiyor. |
| 40513 |16 |Sunucu değişkeni '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40514 |16 |'%ls', SQL Server'ın bu sürümünde desteklenmiyor. |
| 40515 |16 |Veritabanı ve/veya sunucu adına başvuru içinde '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40516 |16 |Genel geçici nesneler bu SQL Server sürümünde desteklenmiyor. |
| 40517 |16 |Anahtar sözcüğü veya deyim seçeneği '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40518 |16 |DBCC komutu '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40520 |16 |Güvenliği sağlanabilir sınıfı '% S_MSG' Bu SQL Server sürümünde desteklenmiyor. |
| 40521 |16 |Güvenliği sağlanabilir sınıfı '% S_MSG' Bu SQL Server sürümünde sunucu kapsamında desteklenmiyor. |
| 40522 |16 |Veritabanı asıl '%.&#x2a;ls' türü SQL Server'ın bu sürümünde desteklenmiyor. |
| 40523 |16 |'%.&#x2a;ls' örtük kullanıcı oluşturma, SQL Server'ın bu sürümünde desteklenmiyor. Açıkça kullanmadan önce kullanıcı oluşturun. |
| 40524 |16 |Veri türü '%.&#x2a;ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40525 |16 |'İle %.ls' Bu SQL Server sürümünde desteklenmiyor. |
| 40526 |16 |'%.&#x2a;ls satır kümesi sağlayıcısı bu SQL Server sürümünde desteklenmiyor. |
| 40527 |16 |Bağlı sunucular bu SQL Server sürümünde desteklenmez. |
| 40528 |16 |Kullanıcıların sertifikalar, asimetrik anahtarlar veya SQL Server'ın bu sürümü Windows oturum açma eşlenemez. |
| 40529 |16 |Yerleşik işlevi '%.&#x2a;ls' kimliğe bürünme bağlam, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40532 |11 |Sunucu açamıyor "%.&#x2a;ls" oturum açma tarafından istenen. Oturum açma başarısız. |
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Sorgunuzu daha az sayıda satır işleyecek şekilde değiştirmeyi deneyin.<br/><br/> Sayısını azaltmayı `ORDER BY` ve `GROUP BY` Transact-SQL kodunuzu işlemlerinde yardımcı olur, sorgunun bellek gereksinimlerini azaltın. |
| 40604 |16 |Sunucunun kotasını aşacağından CREATE/ALTER DATABASE olabilir. |
| 40606 |16 |Veritabanlarını ekleme, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40607 |16 |Windows oturum açma bilgileri, SQL Server'ın bu sürümünde desteklenmez. |
| 40611 |16 |Sunucuları tanımlanan en fazla 128 güvenlik duvarı kuralları olabilir. |
| 40614 |16 |Güvenlik duvarı kuralının başlangıç IP adresi, bitiş IP adresini aşamaz. |
| 40615 |16 |Sunucu açamıyor '{0}' oturum açma tarafından istenen. İstemci IP adresi ile{1}' sunucuya erişmesine izin verilmiyor.<br /><br />Erişimi etkinleştirmek için SQL veritabanı portalını veya sp çalıştırın\_ayarlamak\_Güvenlik Duvarı\_kural bu IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturmak için ana veritabanı üzerinde. Bu, bu değişikliğin etkili olması beş dakikaya kadar sürebilir. |
| 40617 |16 |(Kural adı) ile başlayan güvenlik duvarı kuralı adı çok uzun. En fazla uzunluk 128'dir. |
| 40618 |16 |Güvenlik duvarı kuralı adı boş olamaz. |
| 40620 |16 |Kullanıcı için oturum açma başarısız "%.&#x2a;ls". Parola değiştirme başarısız. Oturum açma sırasında parola değiştirme bu SQL Server sürümünde desteklenmiyor. |
| 40627 |20 |İşlem sunucusunda '{0}'ve veritabanı'{1}' devam ediyor. Lütfen tekrar denemeden önce birkaç dakika bekleyin. |
| 40630 |16 |Parola doğrulama başarısız oldu. Parola, çok kısa olduğundan ilke gereksinimlerini karşılamıyor. |
| 40631 |16 |Belirttiğiniz parola çok uzun. Parola en fazla 128 karakter içermelidir. |
| 40632 |16 |Parola doğrulama başarısız oldu. Yeterince karmaşık olmadığı için parola ilke gereksinimlerini karşılamıyor. |
| 40636 |16 |Ayrılmış veritabanı adı kullanamazsınız '%.&#x2a;ls' Bu işlemde. |
| 40638 |16 |Geçersiz abonelik kimliği (abonelik kimliği). Abonelik yok. |
| 40639 |16 |İstek şemaya uymuyor: (şema hatası). |
| 40640 |20 |Sunucu beklenmeyen bir özel durumla karşılaştı. |
| 40641 |16 |Belirtilen konum geçersiz. |
| 40642 |17 |Sunucu şu anda çok meşgul. Lütfen daha sonra tekrar deneyin. |
| 40643 |16 |Belirtilen x-ms-version üstbilgi değeri geçersiz. |
| 40644 |14 |Belirtilen aboneliğe erişim yetkisi verilemedi. |
| 40645 |16 |ServerName (servername), boş veya null olamaz. Bunu yalnızca küçük harfler meydana gelebilir 'a'-'z', 0-9 arasındaki rakamları ve kısa çizgi. Kısa çizgi veya adın sonunda. |
| 40646 |16 |Abonelik kimliği boş olamaz. |
| 40647 |16 |Abonelik (subscrıptıon-ID) sunucu (servername) sahip değil. |
| 40648 |17 |Çok fazla istek gerçekleştirildi. Lütfen daha sonra yeniden deneyin. |
| 40649 |16 |Geçersiz content-type belirtildi. Yalnızca uygulama/xml desteklenir. |
| 40650 |16 |Abonelik (subscrıptıon-ID) yok veya işlem için hazır değil. |
| 40651 |16 |' % S'aboneliğine (abonelik kimliği) devre dışı bırakıldığından sunucu oluşturulamadı. |
| 40652 |16 |Taşınamıyor veya sunucu oluşturulamıyor. Abonelik (subscrıptıon-ID) sunucu kotasını aşacak. |
| 40671 |17 |Ağ geçidi ve yönetim hizmeti arasında bağlantı hatası oluştu. Lütfen daha sonra yeniden deneyin. |
| 40852 |16 |Veritabanı açılamıyor. ' %. \*ls sunucusundaki ' %. \*ls, oturum açma tarafından istenen. Veritabanına erişimi yalnızca güvenli bağlantı dizesi kullanarak izin verilir. Bu veritabanına erişmek için bağlantı dizeleri içerecek şekilde değiştirin. 'güvenli' sunucu FQDN'SİNDE - 'sunucu adı'.database.windows .net 'sunucu adı'.database için değiştirilmesi gerekir. `secure`. windows.net. |
| 40914 | 16 | Sunucu açamıyor '*[sunucu-adı]*' oturum açma tarafından istenen. İstemcinin sunucuya erişmesine izin verilmiyor.<br /><br />Sorunu gidermek için eklemeyi göz önünde bulundurun bir [sanal ağ kuralı](sql-database-vnet-service-endpoint-rule-overview.md). |
| 45168 |16 |SQL Azure sistem yük altındayken ve eş zamanlı DB CRUD işlemleri tek bir SQL veritabanı sunucusu için üst sınır yerleştirerek (örneğin, veritabanı oluşturma). Hata iletisinde belirtilen sunucu, en fazla eş zamanlı bağlantı sayısını aştı. Daha sonra tekrar deneyin. |
| 45169 |16 |Azure SQL sistem yük altındayken ve eş zamanlı sunucu CRUD işlemleri için tek bir abonelik sayısı üst sınırını yerleştirme (örneğin, sunucu oluşturma). Hata iletisinde belirtilen aboneliği en fazla eş zamanlı bağlantı sayısını aştı ve istek reddedildi. Daha sonra tekrar deneyin. |

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Azure SQL veritabanı özellikleri](sql-database-features.md).
* Hakkında bilgi edinin [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md).
* Hakkında bilgi edinin [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md).

