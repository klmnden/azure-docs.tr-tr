---
title: "SQL hata kodları - veritabanı bağlantı hatası | Microsoft Docs"
description: "Ortak veritabanı bağlantı hataları, veritabanı kopyalama sorunlarını ve genel hataları gibi SQL Database istemci uygulamaları için SQL hata kodları hakkında bilgi edinin. "
keywords: "SQL hata kodu, erişim sql, veritabanı bağlantı hatası, sql hata kodları"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2a23e4ca-ea93-4990-855a-1f9f05548202
ms.service: sql-database
ms.custom: develop apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: sstein
ms.openlocfilehash: 34e7142b5ca13ad8de5a4dbd380377abdf055c04
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>SQL Database istemci uygulamaları için SQL hata kodları: veritabanı bağlantı hataları ve diğer sorunlar

Bu makalede SQL veritabanı, veritabanı bağlantı hataları, geçici hataları (geçici hataları olarak da bilinir), kaynak İdaresi hataları, veritabanı kopyalama sorunlarını, esnek havuz ve başka hatalar da dahil olmak üzere istemci uygulamaları için SQL hata kodları listelenmektedir. Çoğu kategorileri, Azure SQL veritabanına belirli ve Microsoft SQL Server için geçerli değildir.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Veritabanı bağlantı hataları, geçici hataları ve diğer geçici hataları
Aşağıdaki tabloda bağlantı kaybı hataları ve uygulamanızı SQL veritabanına erişim girişiminde bulunduğunda, karşılaşabileceğiniz diğer geçici hataları için SQL hata kodları yer almaktadır. Azure SQL veritabanına bağlanmak nasıl Başlarken eğitimleri için bkz: [Azure SQL veritabanına bağlanma](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>En yaygın veritabanı bağlantı hataları ve geçici hata hataları
Azure altyapı sunucularını yoğun iş yükleri SQL Database hizmetinde ortaya zaman dinamik olarak yeniden özelliğine sahiptir.  Bu dinamik davranış istemci programınız SQL veritabanına olan bağlantısı kaybetmesine neden olabilir. Bu tür bir hata durumu olarak adlandırılan bir *geçici hata*.

Böylece kendisini düzeltmek için geçici hata zaman vermiş sonra bağlantı yeniden, istemci programınızı yeniden deneme mantığına sahiptir önerilir.  İlk, yeniden denemeden önce 5 saniye gecikme öneririz. Bulut hizmeti aşırı 5 saniye riskleri kısa bir gecikme sonrasında yeniden deneniyor. Gecikme büyümesine katlanarak, her sonraki yeniden deneme için en fazla 60 saniye.

Geçici hata hatalar genellikle, istemci programlarından aşağıdaki hata iletilerinden biri olarak bildirimi:

* Veritabanı &lt;db_name&gt; sunucuda &lt;Azure_instance&gt; şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin &lt;session_id&gt;
* Veritabanı &lt;db_name&gt; sunucuda &lt;Azure_instance&gt; şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin &lt;session_id&gt;. (Microsoft SQL Server, hata: 40613)
* Varolan bir bağlantıyı zorla uzak ana bilgisayar tarafından kapatıldı.
* System.Data.Entity.Core.EntityCommandExecutionException: Komut tanımı yürütülürken bir hata oluştu. Ayrıntılar için iç özel duruma bakın. System.Data.SqlClient.SqlException--->: sonuçları sunucudan alırken bir aktarım düzeyi hatası oluştu. (sağlayıcısı: oturum sağlayıcısı, hata: 19 - fiziksel bağlantı değil kullanılabilir)
* Veritabanı reconfguration sürecinde olduğu ve birincil veritabanında bir etkin işlem sırasında ortasında yeni sayfalar uygulama meşgul ikincil veritabanına bir bağlantı girişimi başarısız oldu. 

Yeniden deneme mantığı kod örnekleri için bkz:

* [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md) 
* [SQL veritabanı'nda bağlantı hatalarını ve geçici hataları düzeltmek için Eylemler](sql-database-connectivity-issues.md)

Bir tartışma *engelleme süresi* ADO.NET kullanan istemciler için kullanılabilir [SQL Server bağlantı havuzu (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Geçici hata hata kodları
Aşağıdaki hatalar geçicidir ve uygulama mantığını yeniden denenmesi gerekiyor: 

| Hata kodu | Önem Derecesi | Açıklama |
| ---:| ---:|:--- |
| 4060 |16 |Veritabanı açılamıyor. "%. & #x2a; ls" oturum açma tarafından istenen. Oturum açma başarısız. |
| 40197 |17 |Hizmet isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin. Hata kodu %d.<br/><br/>Hizmet aşağı yazılım veya donanım yükseltmeleri, donanım hataları ya da başka bir yük devretme sorunlar nedeniyle olduğunda bu hatayı alırsınız. Katıştırılmış 40197 hata iletisi içinde hata kodu (%d) tür hata veya oluştu yük devretme hakkında ek bilgi sağlar. Bazı kodlar hata 40197 ileti içinde katıştırılmış hata 40020, 40143, 40166 ve 40540 gösterilebilir.<br/><br/>SQL veritabanı sunucusuna otomatik olarak yeniden bağlanmayı veritabanınızın sağlıklı bir kopyasını bağlanır. Uygulamanız hata 40197, günlük sorun giderme için iletisindeki katıştırılmış hata kodu (%d) yakalamak ve SQL veritabanına kaynaklar kullanılabilir ve, bağlantı yeniden kurulana kadar yeniden bağlanmayı deneyin. |
| 40501 |20 |Hizmet şu anda meşgul. İsteği 10 saniye sonra yeniden deneyin. Olay Kimliği: %ls. Kodu: %d.<br/><br/>Daha fazla bilgi için bkz.<br/>• [Azure SQL veritabanı kaynak sınırları](sql-database-service-tiers.md). |
| 40613 |17 |Veritabanı '%. & #x2a; ls' sunucusundaki '%. & #x2a; ls' şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse müşteri desteğine başvurun ve oturum izleme Kimliğini verin '%. & #x2a; ls'. |
| 49918 |16 |İsteği işleyemiyor. İsteği işlemek için yeterli kaynak yok.<br/><br/>Hizmet şu anda meşgul. Lütfen isteği daha sonra yeniden deneyin. |
| 49919 |16 |İşlem oluşturulamıyor veya istek güncelleştirilemiyor. Çok sayıda oluşturma veya güncelleştirme devam eden işlemleri aboneliği için "% ld".<br/><br/>Hizmet meşgul birden çok işleme oluştur veya güncelleştir abonelik veya sunucu için istekleri. İstek şu anda kaynak iyileştirme için engellenir. Sorgu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) için bekleyen işlemler. Kasa Oluştur bekleyin veya güncelleştirme isteklerinin tamamlandığı veya bekleyen istekler birini silin ve isteğinizi daha sonra yeniden deneyin. |
| 49920 |16 |İsteği işleyemiyor. Devam eden çok fazla işlemleri aboneliği için "% ld".<br/><br/>Hizmet, bu abonelik için birden çok istek işleme meşgul. İstek şu anda kaynak iyileştirme için engellenir. Sorgu [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) işlem durumu. Bekleyen istekler kadar bekleyin, tam veya bekleyen istekler birini silin ve isteğinizi daha sonra yeniden deneyin. |
| 4221 |16 |Read-ikincil oturum açma 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING' üzerine uzun süre beklenmesi nedeniyle başarısız oldu. Satır sürümleri çoğaltma dönüştürüldü yükleyen yürütülen işlemler için eksik olduğundan çoğaltma oturum açma için kullanılamaz. Sorun, geri alma ya da birincil çoğaltmadaki etkin işlem yürüten tarafından çözülebilir. Bu koşul oluşumlarını uzun yazma işlemleri birincil kaçınarak en aza indirgenebilir. |

## <a name="database-copy-errors"></a>Veritabanı kopyalama hataları
Azure SQL veritabanında bir veritabanı kopyalanırken şu hatalarla karşılaşıldı. Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md).

| Hata kodu | Önem Derecesi | Açıklama |
| ---:| ---:|:--- |
| 40635 |16 |İstemci IP adresi '%. & #x2a; ls' geçici olarak devre dışıdır. |
| 40637 |16 |Oluşturma veritabanı kopyalama şu anda devre dışı. |
| 40561 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veya hedef veritabanı yok. |
| 40562 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı bırakılmış. |
| 40563 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı bırakılmış. |
| 40564 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40565 |16 |Veritabanı kopyalama başarısız oldu. 1'den fazla eşzamanlı veritabanı kopyası aynı kaynaktan izin verilir. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40566 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40567 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız oldu. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40568 |16 |Veritabanı kopyalama başarısız oldu. Kaynak veritabanı kullanılamıyor. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40569 |16 |Veritabanı kopyalama başarısız oldu. Hedef veritabanı kullanılamıyor. Lütfen hedef veritabanını bırakın ve yeniden deneyin. |
| 40570 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |
| 40571 |16 |Bir iç hata nedeniyle veritabanı kopyalama başarısız oldu. Lütfen hedef veritabanını bırakın ve daha sonra yeniden deneyin. |

## <a name="resource-governance-errors"></a>Kaynak İdaresi hataları
Aşağıdaki hatalar, Azure SQL Database ile çalışırken aşırı kaynaklarının kullanımını nedeniyle oluşur. Örneğin:

* Bir işlem için çok uzun süredir.
* Bir işlem çok fazla sayıda kilit tutuyor.
* Bir uygulama çok fazla bellek tükettikten.
* Bir uygulama çok tüketen `TempDb` alanı.

İlgili Konular:

* Daha ayrıntılı bilgi sağlanmıştır burada: [Azure SQL veritabanı kaynak sınırları](sql-database-service-tiers.md).

| Hata kodu | Önem Derecesi | Açıklama |
| ---:| ---:|:--- |
| 10928 |20 |Kaynak Kimliği: %d. Veritabanı için %s sınırı %d ve üst sınırına ulaşıldı. Daha fazla bilgi için bkz: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Kaynak Kimliği sınırına kaynak gösterir. Çalışan iş parçacıkları, kaynak kimliği için = 1. Oturumları, kaynak kimliği = 2.<br/><br/>Bu hata ve nasıl çözümleyeceğiniz hakkında daha fazla bilgi için bkz:<br/>• [Azure SQL veritabanı kaynak sınırları](sql-database-service-tiers.md). |
| 10929 |20 |Kaynak Kimliği: %d. %S en az garantisi %d, üst sınır: %d, ve veritabanı için geçerli kullanım %d. Ancak, sunucu şu anda bu veritabanı için %d büyük istekler desteklemek için çok meşgul. Daha fazla bilgi için bkz: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Aksi halde, lütfen daha sonra yeniden deneyin.<br/><br/>Kaynak Kimliği sınırına kaynak gösterir. Çalışan iş parçacıkları, kaynak kimliği için = 1. Oturumları, kaynak kimliği = 2.<br/><br/>Bu hata ve nasıl çözümleyeceğiniz hakkında daha fazla bilgi için bkz:<br/>• [Azure SQL veritabanı kaynak sınırları](sql-database-service-tiers.md). |
| 40544 |20 |Veritabanı boyut kotasına ulaştı. Verileri bölün veya silin, dizinleri bırakın veya olası çözümler için belgelere bakın. |
| 40549 |16 |Uzun süre çalışan işlem olduğundan oturum sonlandırıldı. İşleminiz kısaltmayı deneyin. |
| 40550 |16 |Oturum, çok fazla sayıda kilit aldığından sonlandırıldı. Try okuma veya tek bir işlemde daha az sayıda satır değiştirme. |
| 40551 |16 |Oturum aşırı nedeniyle sonlandırıldı `TEMPDB` kullanımı. Geçici tablo alanı kullanımını azaltacak şekilde sorgunuzu değiştirmeyi deneyin.<br/><br/>Geçici nesneler kullanıyorsanız, alandan tasarruf etmek `TEMPDB` artık bir oturum tarafından gerekli sonra geçici nesneler bırakarak tarafından veritabanı. |
| 40552 |16 |Aşırı işlem günlüğü alanı kullanımı nedeniyle oturum sonlandırıldı. Tek bir işlemde daha az sayıda satır değiştirmeyi deneyin.<br/><br/>Gerçekleştirdiğiniz toplu ekler kullanarak `bcp.exe` yardımcı programı veya `System.Data.SqlClient.SqlBulkCopy` sınıfı, kullanmayı deneyin `-b batchsize` veya `BatchSize` satır sayısını sınırlamak için seçenekleri her bir işlem sunucusuna kopyalanır. Dizin ile yeniden oluşturma, `ALTER INDEX` deyimi, kullanmayı deneyin `REBUILD WITH ONLINE = ON` seçeneği. |
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Sorgunuzu daha az sayıda satır işleyecek şekilde değiştirmeyi deneyin.<br/><br/>Sayısının azaltılması `ORDER BY` ve `GROUP BY` Transact-SQL kodunuzu işlemlerinde sorgunuzun bellek gereksinimlerini azaltır. |

## <a name="elastic-pool-errors"></a>Esnek Havuz Hataları
Aşağıdaki hatalar oluşturma ve esnek havuzlarını kullanarak ilgili:

| HataNumarası | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX_RESOURCE |Esnek havuz, depolama sınırına ulaştı. Esnek havuz depolama alanı kullanımı (%d) MB aşamaz. |MB cinsinden esnek havuz alanı sınırı. |Esnek havuz depolama sınırını erişildiğinde bir veritabanına veri yazmak çalışıyor. |Mümkünse esnek havuz depolama sınırını artırın, esnek havuz içindeki tek tek veritabanları tarafından kullanılan depolama alanını azaltmak için Dtu artırmayı deneyin veya veritabanlarını esnek havuzdan kaldırın. |
| 10929 |EX_USER |%S en az garantisi %d, üst sınır: %d, ve veritabanı için geçerli kullanım %d. Ancak, sunucu şu anda bu veritabanı için %d büyük istekler desteklemek için çok meşgul. Bkz: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) Yardım için. Aksi halde, lütfen daha sonra yeniden deneyin. |Veritabanı başına minimum DTU; Veritabanı başına maksimum DTU |Esnek havuzdaki tüm veritabanları arasında eşzamanlı çalışan (istek) toplam sayısı, havuz sınırı aşan çalışıldı. |Alt sınırını artırmak için mümkünse esnek havuz Dtu artırmayı deneyin veya veritabanlarını esnek havuzdan kaldırın. |
| 40844 |EX_USER |'%Ls' sunucusundaki '%ls' veritabanını bir esnek havuzdaki '%ls' sürümü veritabanıdır ve sürekli kopyalama ilişkisine sahip olamaz. |Veritabanı adı, veritabanı sürümü, sunucu adı |Esnek havuzdaki premium olmayan db için StartDatabaseCopy komutu verildi. |Çok yakında |
| 40857 |EX_USER |Esnek havuz için sunucusu bulunamadı: '%ls', esnek havuz adı: '%ls'. |Sunucu adı; Esnek havuz adı |Belirtilen esnek Havuz belirtilen sunucu yok. |Geçerli esnek havuz adını belirtin. |
| 40858 |EX_USER |'%Ls' esnek havuzu zaten şu sunucuda: '%ls' |Esnek havuz adı, sunucu adı |Belirtilen esnek Havuz belirtilen mantıksal sunucusunda zaten var. |Yeni bir esnek havuz adını belirtin. |
| 40859 |EX_USER |Esnek havuz hizmet Katmanı '%ls' desteklemez. |Esnek havuz hizmet katmanı |Belirtilen hizmet katmanı, esnek havuz sağlamak için desteklenmiyor. |Doğru sürümü belirtin ya da hizmet katmanı varsayılan hizmet katmanı kullanmak için boş bırakın. |
| 40860 |EX_USER |Esnek havuz '%ls' ve hizmet hedefi '%ls' birleşimi geçersiz. |Esnek havuz adı; Hizmet düzeyi hedef adı |Esnek havuzu ve hizmet hedefi belirtilebilir birlikte yalnızca hizmet hedefi 'ElasticPool' belirtilmişse. |Esnek havuz ve hizmet hedefi doğru bileşimini belirtin. |
| 40861 |EX_USER |Veritabanı sürümü ' %. *ls olan esnek havuz katmanından farklı olamaz ' %.* ls'. |veritabanı sürümü, esnek havuz hizmet katmanı |Esnek havuz katmanından farklı veritabanı sürümüdür. |Esnek havuz katmanından farklı olan bir veritabanı sürümü belirtmeyin.  Veritabanı sürümü belirtilmesi gerekmez unutmayın. |
| 40862 |EX_USER |Esnek havuz adının belirtilmesi gerekir esnek havuz hizmeti hedefi belirtildiyse smbiosguid'sinin. |None |Esnek havuz hizmeti hedefi bir esnek havuz benzersiz olarak tanımlamıyor. |Esnek havuz hizmeti hedefi kullanarak esnek havuz adı belirtin. |
| 40864 |EX_USER |Esnek havuz için Dtu'lar en az olmalıdır (%d) Dtu'lar hizmet katmanı için ' %. * ls'. |Esnek havuz için Dtu'lar; Esnek havuz hizmet katmanı. |Alt sınır aşağıda esnek havuz için Dtu'lar yapılmaya çalışılıyor. |Esnek havuz için en az alt sınırı Dtu'lar ayarı yeniden deneyin. |
| 40865 |EX_USER |Esnek havuz için Dtu'lar (%d) Dtu'lar hizmet katmanı için aşamaz ' %. * ls'. |Esnek havuz için Dtu'lar; Esnek havuz hizmet katmanı. |Maksimum sınırı üstünde esnek havuz için Dtu'lar yapılmaya çalışılıyor. |Esnek havuz için Dtu'lar üst sınırı'den büyük ayarı yeniden deneyin. |
| 40867 |EX_USER |Veritabanı başına maksimum DTU olmalıdır en az (%d) hizmet katmanı için ' %. * ls'. |Veritabanı başına maksimum DTU; Esnek havuz hizmet katmanı |Desteklenen sınırı aşağıda veritabanı başına DTU max yapılmaya çalışılıyor. | İstenen ayarını destekler esnek havuz hizmet katmanı kullanarak onsider. |
| 40868 |EX_USER |Veritabanı başına maksimum DTU (%d) hizmet katmanı için aşamaz ' %. * ls'. |Veritabanı başına maksimum DTU; Esnek havuz hizmet katmanı. |Desteklenen sınırı aşan veritabanı başına DTU max yapılmaya çalışılıyor. | İstenen ayarını destekler esnek havuz hizmet katmanı kullanmayı düşünün. |
| 40870 |EX_USER |Veritabanı başına minimum DTU (%d) hizmet katmanı için aşamaz ' %. * ls'. |Veritabanı başına minimum DTU; Esnek havuz hizmet katmanı. |Desteklenen sınırı aşan veritabanı başına minimum DTU yapılmaya çalışılıyor. | İstenen ayarını destekler esnek havuz hizmet katmanı kullanmayı düşünün. |
| 40873 |EX_USER |Veritabanı (%d) ve (%d) veritabanı başına DTU minimum sayısı, (%d) esnek havuz Dtu aşamaz. |Esnek havuzdaki veritabanları sayı; Veritabanı başına minimum DTU; Esnek havuz Dtu. |Esnek havuz Dtu aşıyor esnek havuzdaki veritabanları için minimum DTU belirtin çalışılıyor. | Esnek havuz için Dtu'lar artırmayı veya veritabanı başına minimum DTU azaltın veya veritabanları esnek havuzda sayısını azaltın. |
| 40877 |EX_USER |Tüm veritabanları içermediği sürece bir esnek havuz silinemez. |None |Esnek havuz, bir veya daha fazla veritabanı içerir ve bu nedenle silinemez. |Veritabanları esnek havuzdan silmek için kaldırın. |
| 40881 |EX_USER |Esnek havuz ' %. * ls, veritabanı sayısı sınırına ulaşıldı.  Esnek havuz için veritabanı sayısı sınırına (%d) Dtu'ya sahip bir esnek havuz için (%d) aşamaz. |Esnek havuz adı; Esnek havuz veritabanı sayısı sınırını; Kaynak havuzu için Edtu. |Oluşturma veya esnek havuz veritabanı sayısı sınırına ulaşıldığında esnek havuza veritabanı ekleme girişimi. | Kendi veritabanı sınırını artırmak için mümkünse esnek havuz Dtu artırmayı deneyin veya veritabanlarını esnek havuzdan kaldırın. |
| 40889 |EX_USER |Dtu'lar ve esnek havuz depolama sınırı ' %. * ls olamaz azaltılabilir, yeterli depolama alanına veritabanları için sağlamayacağından. |Esnek havuz adı. |Depolama kullanım aşağıda esnek havuz depolama sınırını azaltın çalışılıyor. | Tek veritabanlarını esnek havuzdaki depolama kullanımı azaltmayı deneyin veya kendi Dtu'lar ve depolama sınırı azaltmak için veritabanlarını havuzdan kaldırın. |
| 40891 |EX_USER |(%D) veritabanı başına minimum DTU (%d) veritabanı başına DTU max aşamaz. |Veritabanı başına minimum DTU; Veritabanı başına maksimum DTU. |Veritabanı başına DTU maksimum değerinden yüksek veritabanı başına minimum DTU yapılmaya çalışılıyor. |Veritabanı başına minimum DTU veritabanı başına DTU max aşmadığından emin olun. |
| TBD |EX_USER |Tek bir veritabanının esnek havuzdaki depolama boyutu tarafından izin verilen en büyük boyut aşamaz ' %. * ls hizmet katmanı esnek havuz. |Esnek havuz hizmet katmanı |Veritabanı için en büyük boyut esnek havuz hizmet katmanı tarafından izin verilen en fazla boyutu aşıyor. |Veritabanı esnek havuz hizmet katmanı tarafından izin verilen en büyük boyut sınırları içinde en büyük boyutunu ayarlayın. |

İlgili Konular:

* [Bir esnek havuz (C#) oluşturma](sql-database-elastic-pool-manage-csharp.md) 
* [Bir esnek havuz (C#) yönetme](sql-database-elastic-pool-manage-csharp.md). 
* [Bir esnek havuz (PowerShell) oluşturma](sql-database-elastic-pool-manage-powershell.md) 
* [İzleme ve yönetme bir esnek havuz (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Genel hataları
Aşağıdaki hatalar herhangi önceki kategorilere ayrılır değil.

| Hata kodu | Önem Derecesi | Açıklama |
| ---:| ---:|:--- |
| 15006 |16 |(Admınıstratorlogın), geçersiz karakterler içerdiğinden geçerli bir ad değil. |
| 18452 |14 |Oturum açma başarısız oldu. Oturum açma güvenilmeyen bir etki alanından ve Windows authentication.%. & #x2a; ile kullanılamaz ls (Windows oturumu açma desteklenmez SQL Server'ın bu sürümünde.) |
| 18456 |14 |Kullanıcı için oturum açma başarısız ' %. #x2a;ls'.%. & #x2a ls %. & #x2a; ls (kullanıcı için oturum açma başarısız "%. & #x2a; ls". Parola değiştirme başarısız oldu. Oturum açma sırasında parola değiştirme bu SQL Server sürümünde desteklenmiyor.) |
| 18470 |14 |Kullanıcı için oturum açma başarısız '%. & #x2a; ls'. Neden: Disabled.%. & #x2a; hesaptır ls |
| 40014 |16 |Birden çok veritabanı aynı işlemde kullanılamaz. |
| 40054 |16 |Kümelenmiş dizini olmayan tablolar, SQL Server'ın bu sürümünde desteklenmez. Kümelenmiş bir dizin oluşturun ve yeniden deneyin. |
| 40133 |15 |Bu işlem, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40506 |16 |Belirtilen SID bu SQL Server sürümü için geçersiz. |
| 40507 |16 |' %. & #x2a; ls'SQL Server'ın bu sürümünde parametrelerle çağrılacak olamaz. |
| 40508 |16 |USE deyiminin veritabanları arasında geçiş yapmak için desteklenmiyor. Farklı bir veritabanına bağlanmak için yeni bir bağlantı kullanın. |
| 40510 |16 |Deyimi '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor |
| 40511 |16 |Yerleşik işlevi '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40512 |16 |Kullanım dışı özelliği '%ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40513 |16 |Sunucu değişkeni '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40514 |16 |'%ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40515 |16 |Veritabanı ve/veya sunucu adına başvuru içinde '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40516 |16 |Genel geçici nesneler bu SQL Server sürümünde desteklenmiyor. |
| 40517 |16 |Anahtar sözcüğü veya deyim seçeneği '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40518 |16 |DBCC komutu '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40520 |16 |Güvenliği sağlanabilir sınıfı '% S_MSG' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40521 |16 |Güvenliği sağlanabilir sınıfı '% S_MSG' SQL Server'ın bu sürümünde sunucu kapsamında desteklenmiyor. |
| 40522 |16 |Veritabanı asıl '%. & #x2a; ls' türü SQL Server'ın bu sürümünde desteklenmiyor. |
| 40523 |16 |'%. & #X2a; ls' örtük kullanıcı oluşturma, SQL Server'ın bu sürümünde desteklenmiyor. Açıkça kullanmadan önce kullanıcı oluşturun. |
| 40524 |16 |Veri türü '%. & #x2a; ls' SQL Server'ın bu sürümünde desteklenmiyor. |
| 40525 |16 |'%.Ls' ile SQL Server'ın bu sürümünde desteklenmiyor. |
| 40526 |16 |' %. & #x2a; ls satır kümesi sağlayıcısı bu SQL Server sürümünde desteklenmiyor. |
| 40527 |16 |Bağlantılı sunucular, SQL Server'ın bu sürümünde desteklenmez. |
| 40528 |16 |Kullanıcıların sertifikalar, asimetrik anahtarlar veya SQL Server'ın bu sürümünde Windows oturumları eşlenemiyor. |
| 40529 |16 |Yerleşik işlevi '%. & #x2a; ls' kimliğe bürünme bağlam, SQL Server'ın bu sürümünde desteklenmiyor. |
| 40532 |11 |Sunucu açamıyor "%. & #x2a; ls" oturum açma tarafından istenen. Oturum açma başarısız. |
| 40553 |16 |Aşırı bellek kullanımı nedeniyle oturum sonlandırıldı. Sorgunuzu daha az sayıda satır işleyecek şekilde değiştirmeyi deneyin.<br/><br/> Sayısının azaltılması `ORDER BY` ve `GROUP BY` Transact-SQL kodunuzu işlemlerinde yardımcı sorgunuzun bellek gereksinimlerini azaltın. |
| 40604 |16 |Sunucunun kotasını aşacağından CREATE/ALTER DATABASE verebilir. |
| 40606 |16 |Veritabanları ekleme SQL Server'ın bu sürümünde desteklenmiyor. |
| 40607 |16 |Windows oturum açma bilgileri, SQL Server'ın bu sürümünde desteklenmez. |
| 40611 |16 |Sunucuları tanımlanan en fazla 128 güvenlik duvarı kuralları olabilir. |
| 40614 |16 |Güvenlik duvarı kuralının başlangıç IP adresi bitiş IP adresini aşamaz. |
| 40615 |16 |Sunucu '{0}' oturum açma tarafından istenen açamıyor. IP adresi '{1}' olan istemcinin sunucuya erişmek için izin verilmiyor.<br /><br />Erişimi etkinleştirmek için SQL veritabanı Portalı'nı kullanın veya sp çalıştırın\_ayarlamak\_Güvenlik Duvarı\_bu IP adresi veya adres aralığı için bir güvenlik duvarı kuralı oluşturmak için ana veritabanı üzerinde kuralı. Bu değişikliğin etkili olması beş dakika kadar sürebilir. |
| 40617 |16 |(Kural adı) ile başlayan güvenlik duvarı kuralı adı çok uzun. En fazla uzunluk 128'dir. |
| 40618 |16 |Güvenlik duvarı kuralı adı boş olamaz. |
| 40620 |16 |Kullanıcı için oturum açma başarısız "%. & #x2a; ls". Parola değiştirme başarısız oldu. Oturum açma sırasında parola değiştirme bu SQL Server sürümünde desteklenmiyor. |
| 40627 |20 |Sunucu '{0}' ve veritabanı '{1}' işlemi devam ediyor. Yeniden denemeden önce birkaç dakika bekleyin. |
| 40630 |16 |Parola doğrulama başarısız oldu. Parola çok kısa olduğundan ilke gereksinimlerini karşılamıyor. |
| 40631 |16 |Belirttiğiniz parola çok uzun. Parola en fazla 128 karakter olmalıdır. |
| 40632 |16 |Parola doğrulama başarısız oldu. Parola yeterince karmaşık olmadığı için ilke gereksinimlerini karşılamıyor. |
| 40636 |16 |Ayrılmış veritabanı adı kullanamazsınız '%. & #x2a; ls' Bu işlemde. |
| 40638 |16 |Geçersiz abonelik kimliği (subscrıptıon-ID). Abonelik yok. |
| 40639 |16 |İstek şemaya uymuyor: (şema hatası). |
| 40640 |20 |Sunucu beklenmeyen bir özel durumla karşılaştı. |
| 40641 |16 |Belirtilen konum geçersiz. |
| 40642 |17 |Sunucu şu anda çok meşgul. Lütfen daha sonra tekrar deneyin. |
| 40643 |16 |Belirtilen x-ms-version üstbilgi değeri geçersiz. |
| 40644 |14 |Belirtilen aboneliğe erişim yetkisi verilemedi. |
| 40645 |16 |ServerName (SunucuAdı), boş veya null olamaz. Bunu yalnızca küçük harfler yapılabilir 'a'-'z', 0-9 sayıları ve kısa çizgi. Tire değil sağlama veya adı izi. |
| 40646 |16 |Abonelik kimliği boş olamaz. |
| 40647 |16 |Abonelik (subscrıptıon-ID) (SunucuAdı) sunucusu yok. |
| 40648 |17 |Çok fazla istek gerçekleştirildi. Lütfen daha sonra yeniden deneyin. |
| 40649 |16 |Geçersiz içerik türü belirtildi. Yalnızca application/xml desteklenir. |
| 40650 |16 |Abonelik (subscrıptıon-ID) yok veya işlem için hazır değil. |
| 40651 |16 |Abonelik (subscrıptıon-ID) devre dışı bırakıldığından sunucu oluşturulamadı. |
| 40652 |16 |Taşınamıyor veya sunucu oluşturulamıyor. Abonelik (subscrıptıon-ID) sunucu kotası aşılacak. |
| 40671 |17 |Ağ geçidi ve yönetim hizmeti arasındaki iletişim hatası. Lütfen daha sonra yeniden deneyin. |
| 40852 |16 |Veritabanı açılamıyor. ' %. \*ls sunucusundaki ' %. \*ls oturum açma tarafından istenen. Güvenliği etkinleştirilmiş bağlantı dizesi kullanılarak veritabanına erişimi yalnızca izin verilir. Bu veritabanına erişmek için bağlantı dizelerinizi içerecek şekilde değiştirin. sunucunun FQDN - 'sunucu adı'.database.windows ' güvenli'.net 'sunucu adı'.database değiştirilmemelidir. `secure`. windows.net. |
| 40914 | 16 | Sunucu açamıyor '*[sunucu-adı]*' oturum açma tarafından istenen. İstemci, sunucuya erişimine izin verilmiyor.<br /><br />Durumu düzeltmek için eklemeyi düşünün bir [sanal ağ kuralı](sql-database-vnet-service-endpoint-rule-overview.md). |
| 45168 |16 |SQL Azure sistem yük altında ve bir üst sınır, tek bir sunucu için eş zamanlı DB CRUD işlemleri yerleştirerek (örn., veritabanı oluşturma). Hata iletisinde belirtilen sunucusu en fazla eşzamanlı bağlantı sayısını aştı. Daha sonra yeniden deneyin. |
| 45169 |16 |SQL azure sistem yük altında ve bir eş zamanlı sunucu CRUD işlemleri için tek bir abonelik sayısı üst sınırını yerleştirme (örneğin, sunucu oluşturma). Hata iletisinde belirtilen abonelik en fazla eşzamanlı bağlantı sayısını aştı ve istek reddedildi. Daha sonra yeniden deneyin. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [Azure SQL veritabanı özellikleri](sql-database-features.md).
* Hakkında bilgi edinin [hizmet katmanları](sql-database-service-tiers.md).

